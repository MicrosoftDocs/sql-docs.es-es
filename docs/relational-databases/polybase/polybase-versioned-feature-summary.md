---
description: Características y limitaciones de PolyBase
title: Características y limitaciones de PolyBase | Microsoft Docs
descriptions: This article summarizes PolyBase features available for SQL Server products and services. It lists T-SQL operators supported for pushdown and known limitations.
ms.date: 11/13/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
ms.assetid: 6591994d-6109-4285-9c5b-ecb355f8a111
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2afdc0e62fdd725584c464bda516fc6284d20f01
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489995"
---
# <a name="polybase-features-and-limitations"></a>Características y limitaciones de PolyBase

[!INCLUDE[appliesto-ss2016-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

Este artículo es un resumen de las características de PolyBase disponibles para los servicios y productos de SQL Server.  
  
## <a name="feature-summary-for-product-releases"></a>Resumen de características para las versiones de productos

En esta tabla se indican las características fundamentales de PolyBase y los productos en los que están disponibles.  

|**Característica** |**SQL Server** (a partir de 2016) |**Azure SQL Database** |**Azure Synapse Analytics** |**Almacenamiento de datos paralelos** |
|---------|---------|---------|---------|---------|
|Consultar datos de Hadoop con [!INCLUDE[tsql](../../includes/tsql-md.md)]|Sí|No|No|Sí|
|Importar datos desde Hadoop|Sí|No|No|Sí|
|Exportar datos a Hadoop  |Sí|No|No| Sí|
|Consultar, importar desde y exportar a Azure HDInsight |No|No|No|No
|Aplicar cálculos de consulta a Hadoop|Sí|No|No|Sí|  
|Importar datos desde Azure Blob Storage|Sí|Sí<sup>*</sup>|Sí|Sí|
|Exportar datos a Azure Blob Storage|Sí|No|Sí|Sí|  
|Importar datos de Azure Data Lake Store|No|No|Sí|No|
|Exportar datos a Azure Data Lake Store|No|No|Sí|No|
|Ejecutar consultas de PolyBase desde las herramientas de BI de Microsoft|Sí|No|Sí|Sí|

<sup>*</sup> Presentado en SQL Server 2017, vea [Ejemplos de acceso masivo a datos en Azure Blob Storage](../import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).


## <a name="pushdown-computation-supported-by-t-sql-operators"></a>Cálculo de aplicación compatible con los operadores T-SQL

En SQL Server y APS, no todos los operadores T-SQL se pueden aplicar al clúster de Hadoop. En esta tabla se indican todos los operadores compatibles y un subconjunto de los operadores no admitidos.

|**Nombre de operador** |**Aplicable a Hadoop** |**Aplicable a Blob Storage** |
|---------|---------|---------|
|Proyecciones de columna|Sí|No|
|Predicados|Sí|No|
|Agregados|Parcial|No|
|Combinaciones entre tablas externas|No|No|
|Combinaciones entre tablas externas y tablas locales|No|No|
|Ordenaciones|No|No|

La agregación parcial significa que se debe producir una agregación final una vez que los datos lleguen a SQL Server. Pero una parte de la agregación se produce en Hadoop. Este método es habitual a la hora de calcular agregaciones en sistemas de procesamiento paralelo masivo.  

## <a name="known-limitations"></a>Restricciones conocidas

PolyBase presenta las siguientes limitaciones:

- Para poder usar PolyBase, debe tener permisos a nivel de CONTROL SERVER o sysadmin en la base de datos.

- El tamaño máximo posible de fila, que incluye la longitud total de las columnas de longitud variable, no puede superar los 32 KB en SQL Server ni 1 MB en Azure Synapse Analytics.

- Cuando se exportan los datos a un formato de archivo ORC desde SQL Server o Azure Synapse Analytics, las columnas de texto intensivo pueden limitarse. Se pueden limitar a tan solo 50 columnas debido a los mensajes de error de memoria insuficiente de Java. Para solucionar este problema, exporte solo un subconjunto de las columnas.

- PolyBase no se puede conectar a una instancia de Hortonworks si Knox está habilitado.

- Si usa tablas de Hive con "transactional = true", PolyBase no puede acceder a los datos del directorio de tablas de Hive.

<!--SQL Server 2016-->
::: moniker range="= sql-server-2016 "

- [PolyBase no se instala cuando se agrega un nodo a un clúster de conmutación por error de SQL Server 2016](https://support.microsoft.com/help/3173087/fix-polybase-feature-doesn-t-install-when-you-add-a-node-to-a-sql-server-2016-failover-cluster).

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre PolyBase, consulte [¿Qué es PolyBase?](polybase-guide.md)
