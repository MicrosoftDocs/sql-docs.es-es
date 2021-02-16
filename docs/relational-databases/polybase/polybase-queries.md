---
title: Escenarios de consulta de PolyBase | Microsoft Docs
description: Vea ejemplos de consultas en las que se usa la característica PolyBase de SQL Server, como SELECT, la instrucción JOIN externa con tablas locales, importación y exportación de datos, y nuevas vistas de catálogo.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
keywords:
- PolyBase
helpviewer_keywords:
- PolyBase, import and export
- Hadoop, import with PolyBase
- Hadoop, export with PolyBase
- Azure blob storage, import with PolyBase
- Azure blob storage, export with PolyBase
ms.assetid: 2c5aa2bd-af7d-4f57-9a28-9673c2a4c07e
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: cdcd5d06395d8216fdf3e9c61b8434ab226c9837
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351835"
---
# <a name="polybase-query-scenarios"></a>Escenarios de consulta de PolyBase

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

En este artículo se proporcionan ejemplos de consultas mediante el uso de la característica [PolyBase](../../relational-databases/polybase/polybase-guide.md) de SQL Server (a partir de la versión de 2016). Antes de usar estos ejemplos, debe instalar y configurar PolyBase. Para más información, consulte la [información general de PolyBase](polybase-guide.md).
  
Ejecute instrucciones de Transact-SQL en tablas externas o use herramientas de BI para consultar tablas externas.
  
## <a name="select-from-external-table"></a>SELECT desde tabla externa  

Consulta simple que devuelve datos de una tabla externa definida.  

```sql  
SELECT TOP 10 * FROM [dbo].[SensorData];   
```

Consulta simple que incluye un predicado.

```sql
SELECT * FROM [dbo].[SensorData]
WHERE Speed > 65;
```

## <a name="join-external-tables-with-local-tables"></a>JOIN tablas externas con tablas locales

```sql
SELECT InsuranceCustomers.FirstName,   
   InsuranceCustomers.LastName,   
   SensorData.Speed  
FROM InsuranceCustomers INNER JOIN SensorData    
ON InsuranceCustomers.CustomerKey = SensorData.CustomerKey   
WHERE SensorData.Speed > 65   
ORDER BY SensorData.Speed DESC  
  
```  

## <a name="import-data"></a>Importar datos

Importe datos desde Almacenamiento de Azure o Hadoop en SQL Server para obtener un almacenamiento persistente. Use SELECT INTO para importar datos a los que se hace referencia en una tabla externa para el almacenamiento persistente en SQL Server. Cree una tabla relacional sobre la marcha y luego cree un índice de almacén de columnas sobre la tabla en un segundo paso.

```sql
-- PolyBase scenario - import external data into SQL Server
-- Import data for fast drivers into SQL Server to do more in-depth analysis
-- Leverage columnstore technology
  
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

## <a name="export-data"></a>Exportar datos

Exporte datos de SQL Server a Hadoop o Azure Storage. 

En primer lugar, habilite la funcionalidad de exportación; para ello, establezca el valor `sp_configure` de "Permitir exportación de PolyBase" en 1. Luego, cree una tabla externa que apunte al directorio de destino. La instrucción CREATE EXTERNAL TABLE crea el directorio de destino, si aún no existe. Después, use INSERT INTO para exportar datos de una tabla de SQL Server local al origen de datos externo. 

Los resultados de la instrucción SELECT se exportan a la ubicación especificada en el formato de archivo especificado. Los archivos externos se denominan *QueryID_date_time_ID.format*, donde *ID* es un identificador incremental y *format* es el formato de los datos exportados. Por ejemplo, un nombre de archivo podría ser QID776_20160130_182739_0.orc.

> [!NOTE]
> Al exportar datos a Hadoop o Azure Blob Storage mediante PolyBase, solo se exportan los datos, y no los nombres de columna (metadatos), tal y como se define en el comando CREATE EXTERNAL TABLE.

```sql  
-- PolyBase scenario  - export data from SQL Server to Hadoop
-- Create an external table
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
INSERT INTO dbo.FastCustomers2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```

## <a name="new-catalog-views"></a>Nuevas vistas de catálogo

Las siguientes nuevas vistas de catálogo muestran recursos externos.

```sql
SELECT * FROM sys.external_data_sources;   
SELECT * FROM sys.external_file_formats;  
SELECT * FROM sys.external_tables;  
```

 Determine si una tabla es una tabla externa mediante `is_external`  

```sql  
SELECT name, type, is_external FROM sys.tables WHERE name='myTableName'   
```  

## <a name="next-steps"></a>Pasos siguientes  

Para obtener más información sobre solución de problemas, vea [Solución de problemas de PolyBase](../../relational-databases/polybase/polybase-troubleshooting.md).
