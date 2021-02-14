---
title: Catálogo de base de datos OLAP de WideWorldImporters-SQL | Microsoft Docs
description: Comprenda los esquemas, las tablas y los procedimientos almacenados que se usan para el almacenamiento de datos y el procesamiento analítico en la base de datos WideWorldImportersDW.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.custom: ''
ms.date: 08/04/2018
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 4481a0c96a8f4a7f2f82e8babc5d8dce5ea97e92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077818"
---
# <a name="wideworldimportersdw-database-catalog"></a>Catálogo de base de datos WideWorldImportersDW
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
Explicaciones para los esquemas, tablas y procedimientos almacenados en la base de datos WideWorldImportersDW. 

La base de datos WideWorldImportersDW se utiliza para el procesamiento analítico y el almacenamiento de datos. Los datos transaccionales sobre ventas y compras se generan en la base de datos WideWorldImporters y se cargan en la base de datos WideWorldImportersDW mediante un **proceso ETL diario**.

Por tanto, los datos de WideWorldImportersDW espejan los datos en WideWorldImporters, pero las tablas se organizan de forma diferente. Aunque WideWorldImporters tiene un esquema normalizado tradicional, WideWorldImportersDW usa el enfoque de [esquema de estrella](https://wikipedia.org/wiki/Star_schema) para su diseño de tabla. Además de las tablas de hechos y dimensiones, la base de datos incluye una serie de tablas de almacenamiento provisional que se usan en el proceso ETL.

## <a name="schemas"></a>Esquemas

Los diferentes tipos de tablas se organizan en tres esquemas.

|Schema|Descripción|
|-----------------------------|---------------------|
|Dimensión|Tablas de dimensiones.|
|Fact| Tablas de hechos|  
|Integración|Tablas de ensayo y otros objetos necesarios para ETL.|  

## <a name="tables"></a>Tablas

A continuación se enumeran las tablas de dimensiones y hechos. Las tablas del esquema de integración solo se usan para el proceso ETL y no se muestran en la lista.

### <a name="dimension-tables"></a>Tablas de dimensiones

WideWorldImportersDW tiene las siguientes tablas de dimensiones. La descripción incluye la relación con las tablas de origen en la base de datos WideWorldImporters.

|Tabla|Tablas de origen|
|-----------------------------|---------------------|
|City|`Application.Cities`, `Application.StateProvinces`, `Application.Countries`.|
|Customer|`Sales.Customers`, `Sales.BuyingGroups`, `Sales.CustomerCategories`.|
|Date|Nueva tabla con información sobre las fechas, incluido el año financiero (según el primer inicio del año financiero).|
|Empleado|`Application.People`.|
|StockItem|`Warehouse.StockItems`, `Warehouse.Colors`, `Warehouse.PackageType`.|
|Supplier|`Purchasing.Suppliers`, `Purchasing.SupplierCategories`.|
|PaymentMethod (MétodoPago)|`Application.PaymentMethods`.|
|TransactionType|`Application.TransactionTypes`.|

### <a name="fact-tables"></a>Tablas de hechos

WideWorldImportersDW tiene las siguientes tablas de hechos. La descripción incluye la relación con las tablas de origen en la base de datos WideWorldImporters, así como las clases de consultas de análisis e informes en las que se usa normalmente cada tabla de hechos.

|Tabla|Tablas de origen|Análisis de ejemplo|
|-----------------------------|---------------------|---------------------|
|Pedido de|`Sales.Orders` y `Sales.OrderLines`|Personal de ventas, productividad del selector/compresor y en el tiempo para seleccionar pedidos. Además, hay situaciones de baja existencias que conducen a los pedidos en espera.|
|Sale|`Sales.Invoices` y `Sales.InvoiceLines`|Fechas de ventas, fechas de entrega, rentabilidad con el tiempo, rentabilidad por vendedor.|
|Compra|`Purchasing.PurchaseOrderLines`|Tiempos de entrega reales esperados de vs|
|Transacción|`Sales.CustomerTransactions` y `Purchasing.SupplierTransactions`|Medición de fechas de emisión frente a fechas de finalización e importes.|
|Movimiento|`Warehouse.StockTransactions`|Movimientos a lo largo del tiempo.|
|Mantenimiento de existencias|`Warehouse.StockItemHoldings`|Valores y niveles de existencias disponibles.|

## <a name="stored-procedures"></a>Procedimientos almacenados

Los procedimientos almacenados se utilizan principalmente para el proceso ETL y con fines de configuración.

Se recomienda que todas las extensiones del ejemplo usen el `Reports` esquema para Reporting Services informes y el `PowerBI` esquema para el acceso de Power BI.

### <a name="application-schema"></a>Esquema de aplicación

Estos procedimientos se utilizan para configurar el ejemplo. Se usan para aplicar las características de la edición Enterprise a la versión Standard Edition del ejemplo, agregar polybase y reinicializar ETL.

|Procedimiento|Propósito|
|-----------------------------|---------------------|
|Configuration_ApplyPartitionedColumnstoreIndexing|Aplica las particiones y los índices de almacén de columnas para las tablas de hechos.|
|Configuration_ConfigureForEnterpriseEdition|Aplica las particiones, la indexación de almacén de columnas y en memoria.|
|Configuration_EnableInMemory|Reemplaza las tablas de ensayo de integración por SCHEMA_ONLY las tablas optimizadas para memoria para mejorar el rendimiento de ETL.|
|Configuration_ApplyPolyBase|Configura un origen de datos externo, un formato de archivo y una tabla.|
|Configuration_PopulateLargeSaleTable|Aplica los cambios de edición Enterprise y, a continuación, rellena una cantidad mayor de datos para el año natural 2012 como historial adicional.|
|Configuration_ReseedETL|Quita los datos existentes y reinicia las semillas de ETL. Esto permite volver a rellenar la base de datos OLAP para que coincida con las filas actualizadas en la base de datos OLTP.|

### <a name="integration-schema"></a>Esquema de integración

Los procedimientos utilizados en el proceso ETL se encuentran en estas categorías:
- Procedimientos auxiliares del paquete ETL: todos los procedimientos get *.
- Procedimientos utilizados por el paquete ETL para migrar los datos almacenados provisionalmente a las tablas DW-All Migrate * Procedures.
- `PopulateDateDimensionForYear` -Tarda un año y garantiza que todas las fechas del año se rellenan en la `Dimension.Date` tabla.

### <a name="sequences-schema"></a>Esquema de secuencias

Procedimientos para configurar las secuencias en la base de datos.

|Procedimiento|Propósito|
|-----------------------------|---------------------|
|ReseedAllSequences|Llama al procedimiento `ReseedSequenceBeyondTableValue` para todas las secuencias.|
|ReseedSequenceBeyondTableValue|Se usa para cambiar la posición del valor de secuencia siguiente más allá del valor de cualquier tabla que use la misma secuencia. (Como un `DBCC CHECKIDENT` para las columnas de identidad equivalentes para las secuencias, pero en potencialmente varias tablas).|
