---
title: 'Acceso a datos externos: Azure Blob Storage: PolyBase'
description: En el artículo se usa PolyBase en una instancia de SQL Server con Azure Blob Storage. PolyBase es adecuado para consultas ad hoc de tablas externas y de importación y exportación de datos.
ms.date: 12/02/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019, seo-lt-2019
ms.openlocfilehash: 5363cabdce211884ae9e0f998fcde3056700d7a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073659"
---
# <a name="configure-polybase-to-access-external-data-in-azure-blob-storage"></a>Configurar PolyBase para acceder a datos externos en Azure Blob Storage

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

En el artículo se explica cómo usar PolyBase en una instancia de SQL Server para consultar datos externos en Azure Blob Storage.

## <a name="prerequisites"></a>Prerrequisitos

Si no ha instalado PolyBase, consulte [Instalación de PolyBase](polybase-installation.md). En el artículo de instalación se explican los requisitos previos.

### <a name="configure-azure-blob-storage-connectivity"></a>Configurar la conectividad de Azure Blob Storage

En primer lugar, configure SQL Server PolyBase para usar Azure Blob Storage.

1. Ejecute [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) con "hadoop connectivity" establecido en un proveedor de Azure Blob Storage. Para hallar el valor de los proveedores, consulte [Configuración de la conectividad de PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). De forma predeterminada, la conectividad de Hadoop se establece en 7.

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Reinicie SQL Server con **services.msc**. Al reiniciar SQL Server, se reinician estos servicios:  

   - Servicio de movimiento de datos de SQL Server PolyBase  
   - Motor de SQL Server PolyBase  
  
   ![detener e iniciar servicios de PolyBase en services.msc](../../relational-databases/polybase/media/polybase-stop-start.png "detener e iniciar servicios de PolyBase en services.msc")  
  
## <a name="configure-an-external-table"></a>Configurar una tabla externa

Para consultar los datos en el origen de datos de Hadoop, debe definir una tabla externa para usar en las consultas de Transact-SQL. Los pasos siguientes describen cómo configurar la tabla externa.

1. Cree una clave maestra en la base de datos. Se necesita la clave maestra para cifrar el secreto de credencial.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

1. Cree una credencial de ámbito de base de datos para Azure Blob Storage.

   ```sql
   -- IDENTITY: any string (this is not used for authentication to Azure storage).  
   -- SECRET: your Azure storage account key.  
   CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
   WITH IDENTITY = 'user', Secret = '<azure_storage_account_key>';
   ```

1. Cree un origen de datos externo con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION:  Azure account storage account name and blob container name.  
   -- CREDENTIAL: The database scoped credential created above.  
   CREATE EXTERNAL DATA SOURCE AzureStorage with (  
         TYPE = HADOOP,
         LOCATION ='wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',  
         CREDENTIAL = AzureStorageCredential  
   );  
   ```

1. Cree un formato de archivo externo con [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))  
   ```

1. Cree una tabla externa que señale a los datos almacenados en Azure Storage con [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md). En este ejemplo, los datos externos contienen datos de sensor de vehículo.

   ```sql
   -- LOCATION: path to file or directory that contains the data (relative to HDFS root).  
   CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
         [SensorKey] int NOT NULL,
         [CustomerKey] int NOT NULL,
         [GeographyKey] int NULL,
         [Speed] float NOT NULL,
         [YearMeasured] int NOT NULL  
   )  
   WITH (LOCATION='/Demo/',
         DATA_SOURCE = AzureStorage,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

1. Cree estadísticas en una tabla externa.

   ```sql
   CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
   ```

## <a name="polybase-queries"></a>Consultas de PolyBase

PolyBase es adecuado para realizar tres funciones:  
  
- Consultas ad hoc en tablas externas.  
- Importar datos.  
- Exportar datos.  

Las siguientes consultas proporcionan un ejemplo con datos de sensor de vehículo ficticios.

### <a name="ad-hoc-queries"></a>Consultas ad hoc  

La siguiente consulta ad hoc combina datos relacionales con datos de Hadoop. Selecciona los clientes que conducen a más de 35 mph, y combina los datos estructurados del cliente almacenados en SQL Server con datos de sensor de vehículo almacenados en Hadoop.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>Importar datos  

La consulta siguiente importa datos externos en SQL Server. En este ejemplo se importan los datos relativos a los conductores más rápidos en SQL Server para hacer un análisis más profundo. Para mejorar el rendimiento, aprovecha la tecnología Columnstore.  

```sql
SELECT DISTINCT
      Insured_Customers.FirstName, Insured_Customers.LastName,   
      Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
      SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome  
  
CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
```  

### <a name="exporting-data"></a>Exportación de datos  

La consulta siguiente exporta datos de SQL Server a Azure Blob Storage. En primer lugar, habilite la exportación de PolyBase. Luego, cree una tabla externa para el destino antes de exportar los datos a ella.

```sql
-- Enable INSERT into external table  
sp_configure 'allow polybase export', 1;  
reconfigure  
  
-- Create an external table.
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
      [FirstName] char(25) NOT NULL,
      [LastName] char(25) NOT NULL,
      [YearlyIncome] float NULL,
      [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
      LOCATION='/old_data/2009/customerdata',  
      DATA_SOURCE = HadoopHDP2,  
      FILE_FORMAT = TextFileFormat,  
      REJECT_TYPE = VALUE,  
      REJECT_VALUE = 0  
);  

-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

La exportación de PolyBase con este método puede crear varios archivos.

## <a name="view-polybase-objects-in-ssms"></a>Ver objetos PolyBase en SSMS  

En SSMS, las tablas externas se muestran en una carpeta independiente llamada " **Tablas externas**". Los orígenes de datos y los formatos de archivo externos se encuentran en subcarpetas de **Recursos externos**.  
  
![Objetos PolyBase en SSMS](media/polybase-management.png)  

## <a name="next-steps"></a>Pasos siguientes

Descubra más formas de usar y supervisar PolyBase en los siguientes artículos:

[Grupos de escalabilidad horizontal de PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
[Solución de problemas de PolyBase](polybase-troubleshooting.md).  
