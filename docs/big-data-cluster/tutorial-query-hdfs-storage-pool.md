---
title: 'Consulta de datos de HDFS: grupo de almacenamiento'
titleSuffix: SQL Server Big Data Clusters
description: En este tutorial, se muestra cómo consultar datos de HDFS en un clúster de macrodatos de SQL Server 2019. Cree una tabla externa con los datos en el grupo de almacenamiento y, después, ejecute una consulta.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: aa376549bf3d6430ab5967e193069d35650be76b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043555"
---
# <a name="tutorial-query-hdfs-in-a-sql-server-big-data-cluster"></a>Tutorial: consultar HDFS en un clúster de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este tutorial, se muestra cómo consultar datos de HDFS desde un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una tabla externa que apunte a datos de HDFS en un clúster de macrodatos.
> * Combinar estos datos con datos de alto valor en la instancia maestra.

> [!TIP]
> Si lo prefiere, puede descargar y ejecutar un script con los comandos de este tutorial. Para obtener instrucciones, vea los [ejemplos de virtualización de datos](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization) en GitHub.

Este vídeo de 7 minutos le guía a través de la consulta de datos de HDFS en un clúster de macrodatos:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Query-HDFS-data-inside-SQL-Server-big-data-cluster/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a><a id="prereqs"></a> Requisitos previos

- [Herramientas de macrodatos](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Extensión de SQL Server 2019**
- [Cargar datos de ejemplo en un clúster de macrodatos](tutorial-load-sample-data.md)

## <a name="create-an-external-table-to-hdfs"></a>Crea una tabla externa a HDFS

El grupo de almacenamiento contiene datos de secuencias de clics web en un archivo CSV almacenado en HDFS. Siga este procedimiento para definir una tabla externa que pueda acceder a los datos de ese archivo.

1. En Azure Data Studio, conéctese a la instancia maestra de SQL Server del clúster de macrodatos. Para obtener más información, vea [Conectarse a una instancia maestra de SQL Server](connect-to-big-data-cluster.md#master).

1. Haga doble clic en la conexión de la ventana **Servidores** para mostrar el panel del servidor de la instancia maestra de SQL Server. Seleccione **Nueva consulta**.

   ![Consultar una instancia maestra de SQL Server](./media/tutorial-query-hdfs-storage-pool/sql-server-master-instance-query.png)

1. Ejecute el siguiente comando de Transact-SQL para cambiar el contexto de la base de datos **Ventas** de la instancia maestra.

   ```sql
   USE Sales
   GO
   ```

1. Defina el formato del archivo CSV que se leerá desde HDFS. Presione F5 para ejecutar la instrucción.

   ```sql
   CREATE EXTERNAL FILE FORMAT csv_file
   WITH (
       FORMAT_TYPE = DELIMITEDTEXT,
       FORMAT_OPTIONS(
           FIELD_TERMINATOR = ',',
           STRING_DELIMITER = '"',
           FIRST_ROW = 2,
           USE_TYPE_DEFAULT = TRUE)
   );
   ```

1. Cree un origen de datos externo al grupo de almacenamiento (si aún no existe).

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   BEGIN
     CREATE EXTERNAL DATA SOURCE SqlStoragePool
     WITH (LOCATION = 'sqlhdfs://controller-svc/default');
   END
   ```

1. Cree una tabla externa que pueda leer `/clickstream_data` desde el grupo de almacenamiento. **SqlStoragePool** es accesible desde la instancia maestra de un clúster de macrodatos.

   ```sql
   CREATE EXTERNAL TABLE [web_clickstreams_hdfs]
   ("wcs_click_date_sk" BIGINT , "wcs_click_time_sk" BIGINT , "wcs_sales_sk" BIGINT , "wcs_item_sk" BIGINT , "wcs_web_page_sk" BIGINT , "wcs_user_sk" BIGINT)
   WITH
   (
       DATA_SOURCE = SqlStoragePool,
       LOCATION = '/clickstream_data',
       FILE_FORMAT = csv_file
   );
   GO
   ```

## <a name="query-the-data"></a>Consultar los datos

Ejecute la consulta siguiente para unir los datos de HDFS en la tabla externa `web_clickstream_hdfs` con los datos relacionales en la base de datos de `Sales` local.

```sql
SELECT  
    wcs_user_sk,
    SUM( CASE WHEN i_category = 'Books' THEN 1 ELSE 0 END) AS book_category_clicks,
    SUM( CASE WHEN i_category_id = 1 THEN 1 ELSE 0 END) AS [Home & Kitchen],
    SUM( CASE WHEN i_category_id = 2 THEN 1 ELSE 0 END) AS [Music],
    SUM( CASE WHEN i_category_id = 3 THEN 1 ELSE 0 END) AS [Books],
    SUM( CASE WHEN i_category_id = 4 THEN 1 ELSE 0 END) AS [Clothing & Accessories],
    SUM( CASE WHEN i_category_id = 5 THEN 1 ELSE 0 END) AS [Electronics],
    SUM( CASE WHEN i_category_id = 6 THEN 1 ELSE 0 END) AS [Tools & Home Improvement],
    SUM( CASE WHEN i_category_id = 7 THEN 1 ELSE 0 END) AS [Toys & Games],
    SUM( CASE WHEN i_category_id = 8 THEN 1 ELSE 0 END) AS [Movies & TV],
    SUM( CASE WHEN i_category_id = 9 THEN 1 ELSE 0 END) AS [Sports & Outdoors]
  FROM [dbo].[web_clickstreams_hdfs]
  INNER JOIN item it ON (wcs_item_sk = i_item_sk
                        AND wcs_user_sk IS NOT NULL)
GROUP BY  wcs_user_sk;
GO
```

## <a name="clean-up"></a>Limpieza

Use el comando siguiente para quitar la tabla externa usada en este tutorial.

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstreams_hdfs];
GO
```

## <a name="next-steps"></a>Pasos siguientes

Vea el artículo siguiente para obtener información sobre cómo consultar datos de Oracle desde un clúster de macrodatos.
> [!div class="nextstepaction"]
> [Consultar datos externos en Oracle](tutorial-query-oracle.md)
