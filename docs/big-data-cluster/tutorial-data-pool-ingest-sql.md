---
title: Ingerir datos en un grupo de datos de SQL Server
titleSuffix: SQL Server big data clusters
description: En este tutorial, se muestra cómo ingerir datos en el grupo de datos de un clúster de macrodatos de SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d7e24258e61412584b7364f8dae7d07a1581727b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043553"
---
# <a name="tutorial-ingest-data-into-a-sql-server-data-pool-with-transact-sql"></a>Tutorial: ingerir datos en un grupo de datos de SQL Server con Transact-SQL

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este tutorial, se muestra cómo usar Transact-SQL para cargar datos en el [grupo de datos](concept-data-pool.md) de un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Con [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], se pueden ingerir y distribuir datos de una amplia variedad de orígenes en varias instancias de grupos de datos.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una tabla externa en el grupo de datos.
> * Insertar datos de secuencias de clics web de ejemplo en la tabla del grupo de datos.
> * Combinar datos de la tabla del grupo de datos con tablas locales.

> [!TIP]
> Si lo prefiere, puede descargar y ejecutar un script con los comandos de este tutorial. Para obtener instrucciones, vea los [Ejemplos de grupos de datos](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-pool) en GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Requisitos previos

- [Herramientas de macrodatos](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Extensión de SQL Server 2019**
- [Cargar datos de ejemplo en un clúster de macrodatos](tutorial-load-sample-data.md)

## <a name="create-an-external-table-in-the-data-pool"></a>Crear una tabla externa en el grupo de datos

En los pasos siguientes, crearemos una tabla externa en el grupo de datos llamado **web_clickstream_clicks_data_pool**. Después, esta tabla se puede usar como una ubicación para ingerir datos en el clúster de macrodatos.

1. En Azure Data Studio, conéctese a la instancia maestra de SQL Server del clúster de macrodatos. Para obtener más información, vea [Conectarse a una instancia maestra de SQL Server](connect-to-big-data-cluster.md#master).

1. Haga doble clic en la conexión de la ventana **Servidores** para mostrar el panel del servidor de la instancia maestra de SQL Server. Seleccione **Nueva consulta**.

   ![Consultar una instancia maestra de SQL Server](./media/tutorial-data-pool-ingest-sql/sql-server-master-instance-query.png)

1. Ejecute el siguiente comando de Transact-SQL para cambiar el contexto de la base de datos **Ventas** de la instancia maestra.

   ```sql
   USE Sales
   GO
   ```

1. Cree un origen de datos externo al grupo de datos (si aún no existe).

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlDataPool')
     CREATE EXTERNAL DATA SOURCE SqlDataPool
     WITH (LOCATION = 'sqldatapool://controller-svc/default');
   ```

1. Cree una tabla externa llamada **web_clickstream_clicks_data_pool** en el grupo de datos.

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_tables WHERE name = 'web_clickstream_clicks_data_pool')
      CREATE EXTERNAL TABLE [web_clickstream_clicks_data_pool]
      ("wcs_user_sk" BIGINT , "i_category_id" BIGINT , "clicks" BIGINT)
      WITH
      (
         DATA_SOURCE = SqlDataPool,
         DISTRIBUTION = ROUND_ROBIN
      );
   ```

La creación de la tabla externa del grupo de datos es una operación de bloqueo. El control se devuelve cuando se ha creado la tabla especificada en todos los nodos del grupo de datos de back-end. Si se produce un error durante la operación de creación, se devuelve un mensaje de error al autor de la llamada.

## <a name="load-data"></a>Cargar datos

En los pasos siguientes, se ingieren datos de secuencias de clics web de ejemplo en el grupo de datos mediante la tabla externa creada en los pasos anteriores.

1. Use una instrucción `INSERT INTO` para insertar los resultados de la consulta en el grupo de datos (la tabla externa **web_clickstream_clicks_data_pool**).

   ```sql
   INSERT INTO web_clickstream_clicks_data_pool
   SELECT wcs_user_sk, i_category_id, COUNT_BIG(*) as clicks
     FROM sales.dbo.web_clickstreams_hdfs
   INNER JOIN sales.dbo.item it ON (wcs_item_sk = i_item_sk
                           AND wcs_user_sk IS NOT NULL)
   GROUP BY wcs_user_sk, i_category_id
   HAVING COUNT_BIG(*) > 100;
   ```

1. Inspeccione los datos insertados con dos consultas SELECT.

   ```sql
   SELECT count(*) FROM [dbo].[web_clickstream_clicks_data_pool]
   SELECT TOP 10 * FROM [dbo].[web_clickstream_clicks_data_pool]  
   ```

## <a name="query-the-data"></a>Consultar los datos

Combine los resultados de la consulta almacenados en el grupo de datos con los datos locales de la tabla **Ventas**.

```sql
SELECT TOP (100)
   w.wcs_user_sk,
   SUM( CASE WHEN i.i_category = 'Books' THEN 1 ELSE 0 END) AS book_category_clicks,
   SUM( CASE WHEN w.i_category_id = 1 THEN 1 ELSE 0 END) AS [Home & Kitchen],
   SUM( CASE WHEN w.i_category_id = 2 THEN 1 ELSE 0 END) AS [Music],
   SUM( CASE WHEN w.i_category_id = 3 THEN 1 ELSE 0 END) AS [Books],
   SUM( CASE WHEN w.i_category_id = 4 THEN 1 ELSE 0 END) AS [Clothing & Accessories],
   SUM( CASE WHEN w.i_category_id = 5 THEN 1 ELSE 0 END) AS [Electronics],
   SUM( CASE WHEN w.i_category_id = 6 THEN 1 ELSE 0 END) AS [Tools & Home Improvement],
   SUM( CASE WHEN w.i_category_id = 7 THEN 1 ELSE 0 END) AS [Toys & Games],
   SUM( CASE WHEN w.i_category_id = 8 THEN 1 ELSE 0 END) AS [Movies & TV],
   SUM( CASE WHEN w.i_category_id = 9 THEN 1 ELSE 0 END) AS [Sports & Outdoors]
FROM [dbo].[web_clickstream_clicks_data_pool] as w
INNER JOIN (SELECT DISTINCT i_category_id, i_category FROM item) as i
   ON i.i_category_id = w.i_category_id
GROUP BY w.wcs_user_sk;
```

## <a name="clean-up"></a>Limpieza

Use este comando para quitar los objetos de la base de datos creados en este tutorial.

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstream_clicks_data_pool];
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo ingerir datos en el grupo de datos con trabajos de Spark:
> [!div class="nextstepaction"]
> [Ingerir datos con trabajos de Spark](tutorial-data-pool-ingest-spark.md)
