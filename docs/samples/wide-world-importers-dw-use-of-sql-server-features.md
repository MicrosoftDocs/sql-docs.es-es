---
title: Características clave de la base de datos WideWorldImporters de DW
description: Obtenga información sobre cómo la base de datos WideWorldImportersDW muestra las características clave de SQL Server que son adecuadas para el almacenamiento y el análisis de datos.
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.date: 07/01/2020
ms.reviewer: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||>=aps-pdw-2016||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: ad1c38ab7c479497c50e84b305fe23d1441501d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056969"
---
# <a name="wideworldimportersdw-use-of-sql-server-features-and-capabilities"></a>WideWorldImportersDW el uso de características y funcionalidades SQL Server
[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]
WideWorldImportersDW está diseñado para presentar muchas de las características clave de SQL Server que son adecuadas para el almacenamiento y el análisis de datos. A continuación se muestra una lista de las características y capacidades de SQL Server y una descripción de cómo se usan en WideWorldImportersDW.

## <a name="polybase"></a>PolyBase

[Se aplica a SQL Server (2016 y versiones posteriores)]

Polybase se usa para combinar la información de ventas de WideWorldImportersDW con un conjunto de datos público acerca de los datos demográficos con el fin de comprender qué ciudades pueden interesarle para ampliar la venta.

Para habilitar el uso de polybase en la base de datos de ejemplo, asegúrese de que está instalado y ejecute el siguiente procedimiento almacenado en la base de datos:

```sql
EXECUTE [Application].[Configuration_ApplyPolyBase]
```

Se creará una tabla externa `dbo.CityPopulationStatistics` que hace referencia a un conjunto de datos público que contiene los datos de población de las ciudades del Estados Unidos, hospedado en Azure BLOB Storage. Se recomienda revisar el código del procedimiento almacenado para comprender el proceso de configuración. Si desea hospedar sus propios datos en Azure BLOB Storage y mantenerlos protegidos del acceso público general, deberá llevar a cabo pasos de configuración adicionales. La siguiente consulta devuelve los datos de ese conjunto de datos externos:

```sql
SELECT
        CityID, StateProvinceCode, CityName,
        YearNumber, LatestRecordedPopulation
    FROM
        dbo.CityPopulationStatistics;
```

Para comprender qué ciudades pueden ser de interés para una expansión adicional, la consulta siguiente examina la tasa de crecimiento de las ciudades y devuelve las mayores 100 de las ciudades más grandes con un crecimiento significativo y donde Wide World Importers no tiene una presencia de ventas. La consulta implica una combinación entre la tabla remota `dbo.CityPopulationStatistics` y la tabla local `Dimension.City` , y un filtro que implica la tabla local `Fact.Sales` .

```sql
WITH PotentialCities
AS
(
    SELECT cps.CityName,
            cps.StateProvinceCode,
            MAX(cps.LatestRecordedPopulation) AS PopulationIn2016,
            (MAX(cps.LatestRecordedPopulation) - MIN(cps.LatestRecordedPopulation)) * 100.0
                / MIN(cps.LatestRecordedPopulation) AS GrowthRate
    FROM dbo.CityPopulationStatistics AS cps
    WHERE cps.LatestRecordedPopulation IS NOT NULL
    AND cps.LatestRecordedPopulation <> 0
    GROUP BY cps.CityName, cps.StateProvinceCode
),
InterestingCities
AS
(
    SELECT DISTINCT pc.CityName,
                    pc.StateProvinceCode,
                    pc.PopulationIn2016,
                    FLOOR(pc.GrowthRate) AS GrowthRate
    FROM PotentialCities AS pc
    INNER JOIN Dimension.City AS c
    ON pc.CityName = c.City
    WHERE GrowthRate > 2.0
    AND NOT EXISTS (SELECT 1 FROM Fact.Sale AS s WHERE s.[City Key] = c.[City Key])
)
SELECT TOP(100) CityName, StateProvinceCode, PopulationIn2016, GrowthRate
FROM InterestingCities
ORDER BY PopulationIn2016 DESC;
```

## <a name="clustered-columnstore-indexes"></a>Índices clúster de almacén de columnas

(Versión completa del ejemplo)

Los índices de almacén de columnas en clúster (CCI) se usan con todas las tablas de hechos, para reducir la superficie de almacenamiento y mejorar el rendimiento de las consultas. Con el uso de CCI, el almacenamiento base para las tablas de hechos usa la compresión de columnas.

Los índices no clúster se usan sobre el índice de almacén de columnas agrupado para facilitar las restricciones de clave principal y clave externa. Estas restricciones se han agregado fuera de una abundancia de PRECAUCIÓN: el proceso ETL origina los datos de la base de datos WideWorldImporters, que tiene restricciones para aplicar la integridad. Al quitar las restricciones de clave principal y externa, y sus índices de apoyo, se reduciría la superficie de almacenamiento de las tablas de hechos.

**Tamaño de los datos**

La base de datos de ejemplo tiene un tamaño de datos limitado para facilitar la descarga e instalación del ejemplo. Sin embargo, para ver las ventajas de rendimiento reales de los índices de almacén de columnas, debería usar un conjunto de datos más grande.

Puede ejecutar la instrucción siguiente para aumentar el tamaño de la `Fact.Sales` tabla mediante la inserción de otras 12 millones filas de datos de ejemplo. Estas filas se insertan en el año 2012, de modo que no hay interferencias con el proceso ETL.

```sql
    EXECUTE [Application].[Configuration_PopulateLargeSaleTable]
```

Esta instrucción tardará unos 5 minutos en ejecutarse. Para insertar más de 12 millones filas, pase el número deseado de filas que se van a insertar como parámetro de este procedimiento almacenado.

Para comparar el rendimiento de las consultas con y sin almacén de columnas, puede quitar o volver a crear el índice de almacén de columnas agrupado.

Para quitar el índice:

```sql
 DROP INDEX [CCX_Fact_Order] ON [Fact].[Order]
```

Para volver a crear:

```sql
CREATE CLUSTERED COLUMNSTORE INDEX [CCX_Fact_Order] ON [Fact].[Order]
```

## <a name="partitioning"></a>Creación de particiones

(Versión completa del ejemplo)

El tamaño de los datos en un almacenamiento de datos puede aumentar de tamaño. Por lo tanto, se recomienda utilizar las particiones para administrar el almacenamiento de las tablas grandes en la base de datos.

Todas las tablas de hechos mayores se particionan por año. La única excepción es `Fact.Stock Holdings` , que no se basa en la fecha y tiene un tamaño de datos limitado en comparación con las demás tablas de hechos.

La función de partición que se usa para todas las tablas con particiones es `PF_Date` y el esquema de partición utilizado es `PS_Date` .

## <a name="in-memory-oltp"></a>OLTP en memoria

(Versión completa del ejemplo)

WideWorldImportersDW usa SCHEMA_ONLY las tablas optimizadas para memoria para las tablas de almacenamiento provisional. Todas `Integration.` * `_Staging` las tablas se SCHEMA_ONLY tablas optimizadas para memoria.

La ventaja de SCHEMA_ONLY tablas es que no se registran y no requieren ningún acceso a disco. Esto mejora el rendimiento del proceso ETL. Como estas tablas no se registran, su contenido se pierde si se produce un error. Sin embargo, el origen de datos sigue estando disponible, por lo que el proceso ETL simplemente se puede reiniciar si se produce un error.
