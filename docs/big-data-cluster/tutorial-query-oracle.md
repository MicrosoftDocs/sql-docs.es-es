---
title: Consultar datos externos en Oracle
titleSuffix: SQL Server big data clusters
description: En este tutorial, se muestra cómo consultar datos de Oracle desde un clúster de macrodatos de SQL Server 2019. Cree una tabla externa sobre datos en Oracle y, después, ejecute una consulta.
author: MikeRayMSFT
ms.author: dacoelho
ms.reviewer: mikeray
ms.date: 10/01/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 670ab5046c1904fee16f6c0271c49e2a4ecd72fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045649"
---
# <a name="tutorial-query-oracle-from-sql-server-big-data-cluster"></a>Tutorial: Consulta Oracle desde Clústeres de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este tutorial, se muestra cómo consultar datos de Oracle desde un clúster de macrodatos de SQL Server 2019. Para ejecutar este tutorial, necesita acceder a un servidor de Oracle. Se requiere una cuenta de usuario de Oracle con privilegios de lectura para el objeto externo. Se admite la autenticación de usuario de proxy de Oracle. Si no tiene acceso, este tutorial puede proporcionarle una idea general de cómo funciona la virtualización de datos para orígenes de datos externos en un clúster de macrodatos de SQL Server.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una tabla externa de datos de una base de datos de Oracle externa.
> * Combinar estos datos con datos de alto valor en la instancia maestra.

> [!TIP]
> Si lo prefiere, puede descargar y ejecutar un script con los comandos de este tutorial. Para obtener instrucciones, vea los [ejemplos de virtualización de datos](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization) en GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Requisitos previos

- [Herramientas de macrodatos](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Extensión de SQL Server 2019**
- [Cargar datos de ejemplo en un clúster de macrodatos](tutorial-load-sample-data.md)

## <a name="create-an-oracle-table"></a>Crear una tabla de Oracle

En los pasos siguientes, creará una tabla de ejemplo llamada `INVENTORY` en Oracle.

1. Conéctese a una base de datos y una instancia de Oracle que quiera usar para este tutorial.

1. Ejecute la instrucción siguiente para crear la tabla `INVENTORY`:

   ```sql
    CREATE TABLE "INVENTORY"
    (
        "INV_DATE" NUMBER(10,0) NOT NULL,
        "INV_ITEM" NUMBER(10,0) NOT NULL,
        "INV_WAREHOUSE" NUMBER(10,0) NOT NULL,
        "INV_QUANTITY_ON_HAND" NUMBER(10,0)
    );

    CREATE INDEX INV_ITEM ON HR.INVENTORY(INV_ITEM);
    ```

1. Importe el contenido del archivo **inventory.csv** en esa tabla. El archivo se ha creado con los scripts de creación de ejemplo de la sección [Requisitos previos](#prereqs).

## <a name="create-an-external-data-source"></a>Crear un origen de datos externo

El primer paso es crear un origen de datos externo que pueda acceder al servidor de Oracle.

1. En Azure Data Studio, conéctese a la instancia maestra de SQL Server del clúster de macrodatos. Para obtener más información, vea [Conectarse a una instancia maestra de SQL Server](connect-to-big-data-cluster.md#master).

1. Haga doble clic en la conexión de la ventana **Servidores** para mostrar el panel del servidor de la instancia maestra de SQL Server. Seleccione **Nueva consulta**.

   ![Consultar una instancia maestra de SQL Server](./media/tutorial-query-oracle/sql-server-master-instance-query.png)

1. Ejecute el siguiente comando de Transact-SQL para cambiar el contexto de la base de datos **Ventas** de la instancia maestra.

   ```sql
   USE Sales
   GO
   ```

1. Cree una credencial con ámbito de la base de datos para conectarse al servidor de Oracle. Proporcione las credenciales adecuadas para el servidor de Oracle en la instrucción siguiente.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL [OracleCredential]
   WITH IDENTITY = '<oracle_user,nvarchar(100),SYSTEM>', SECRET = '<oracle_user_password,nvarchar(100),manager>';
   ```

1. Cree un origen de datos externo que apunte al servidor de Oracle.

   ```sql
   CREATE EXTERNAL DATA SOURCE [OracleSalesSrvr]
   WITH (LOCATION = 'oracle://<oracle_server,nvarchar(100)>',CREDENTIAL = [OracleCredential]);
   ```

### <a name="optional-oracle-proxy-authentication"></a>Opcional: autenticación de proxy de Oracle

Oracle admite la autenticación de proxy para proporcionar un control de acceso específico. Un usuario de proxy se conecta a la base de datos de Oracle con sus credenciales y suplanta a otro usuario en la base de datos. 

Un usuario de proxy se puede configurar para tener acceso limitado en comparación con el usuario que se va a suplantar. Por ejemplo, se puede permitir a un usuario de proxy conectarse mediante un rol de base de datos específico del usuario que se va a suplantar. La identidad del usuario que se conecta a la base de datos de Oracle a través del usuario de proxy se conserva en la conexión, incluso si varios usuarios se conectan mediante la autenticación de proxy. Esto permite a Oracle aplicar el control de acceso y auditar las acciones realizadas en nombre del usuario real.

Si su escenario requiere el uso de un usuario de proxy de Oracle, __reemplace los pasos 4 y 5 anteriores por lo siguiente__.

4. Cree una credencial con ámbito de la base de datos para conectarse al servidor de Oracle. Proporcione las credenciales de usuario de proxy de Oracle adecuadas para el servidor de Oracle en la instrucción siguiente.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL [OracleProxyCredential]
   WITH IDENTITY = '<oracle_proxy_user,nvarchar(100),SYSTEM>', SECRET = '<oracle_proxy_user_password,nvarchar(100),manager>';
   ```

5. Cree un origen de datos externo que apunte al servidor de Oracle.

   ```sql
   CREATE EXTERNAL DATA SOURCE [OracleSalesSrvr]
   WITH (LOCATION = 'oracle://<oracle_server,nvarchar(100)>',
   CONNECTION_OPTIONS = 'ImpersonateUser=% CURRENT_USER',
   CREDENTIAL = [OracleProxyCredential]);
   ```

## <a name="create-an-external-table"></a>Crear una tabla externa

Después, cree una tabla externa denominada **iventory_ora** en la tabla `INVENTORY` del servidor de Oracle.

```sql
CREATE EXTERNAL TABLE [inventory_ora]
    ([inv_date] DECIMAL(10,0) NOT NULL, [inv_item] DECIMAL(10,0) NOT NULL,
    [inv_warehouse] DECIMAL(10,0) NOT NULL, [inv_quantity_on_hand] DECIMAL(10,0))
WITH (DATA_SOURCE=[OracleSalesSrvr],
        LOCATION='<oracle_service_name,nvarchar(30),xe>.<oracle_schema,nvarchar(128),HR>.<oracle_table,nvarchar(128),INVENTORY>');
```

> [!NOTE]
> Los nombres de tabla y de columna usarán el identificador entre comillas de SQL de ANSI al realizar consultas en Oracle. Como resultado, los nombres distinguen mayúsculas de minúsculas. Es importante especificar el nombre en la definición de la tabla externa que coincida con el uso de mayúsculas exacto de los nombres de tabla y de columna de los metadatos de Oracle.

## <a name="query-the-data"></a>Consultar los datos

Ejecute la consulta siguiente para combinar los datos de la tabla externa `iventory_ora` con las tablas de la base de datos de `Sales` local.

```sql
SELECT TOP(100) w.w_warehouse_name, i.inv_item, SUM(i.inv_quantity_on_hand) as total_quantity
  FROM [inventory_ora] as i
  JOIN item as it
    ON it.i_item_sk = i.inv_item
  JOIN warehouse as w
    ON w.w_warehouse_sk = i.inv_warehouse
 WHERE it.i_category = 'Books' and i.inv_item BETWEEN 1 and 18000 --> get items within specific range
 GROUP BY w.w_warehouse_name, i.inv_item;
```

## <a name="clean-up"></a>Limpieza

Use este comando para quitar los objetos de la base de datos creados en este tutorial.

```sql
DROP EXTERNAL TABLE [inventory_ora];
DROP EXTERNAL DATA SOURCE [OracleSalesSrvr] ;
DROP DATABASE SCOPED CREDENTIAL [OracleCredential];
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo ingerir datos en el grupo de datos:
> [!div class="nextstepaction"]
> [Cargar datos en el grupo de datos](tutorial-data-pool-ingest-sql.md)
