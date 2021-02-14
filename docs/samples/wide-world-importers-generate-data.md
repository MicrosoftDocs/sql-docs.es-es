---
title: Generar datos en ejemplos de SQL WideWorldImporters
description: Use estas instrucciones SQL para generar e importar datos de ejemplo hasta la fecha actual de las bases de datos de ejemplo WideWorldImporters.
ms.date: 10/23/2020
ms.reviewer: ''
ms.prod: sql
ms.prod_service: sql
ms.technology: samples
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: f54370d9aa54602964817fc17704a324134c0757
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354103"
---
# <a name="wideworldimporters-data-generation"></a>Generación de datos de WideWorldImporters
[!INCLUDE [SQL Server Azure SQL Database](../includes/applies-to-version/sql-asdb.md)]
Las versiones de lanzamiento de las bases de datos WideWorldImporters y WideWorldImportersDW tienen datos del 1 de enero de 2013, hasta el día en que se generaron las bases de datos.

Al utilizar estas bases de datos de ejemplo, puede que desee incluir datos de ejemplo más recientes.

## <a name="data-generation-in-wideworldimporters"></a>Generación de datos en WideWorldImporters

Para generar datos de ejemplo hasta la fecha actual:

1. Si no lo ha hecho, instale una versión limpia de la base de datos WideWorldImporters. Para obtener instrucciones de instalación, consulte [instalación y configuración](wide-world-importers-oltp-install-configure.md).
2. Ejecute la siguiente instrucción en la base de datos:

    ```
        EXECUTE DataLoadSimulation.PopulateDataToCurrentDate
            @AverageNumberOfCustomerOrdersPerDay = 60,
            @SaturdayPercentageOfNormalWorkDay = 50,
            @SundayPercentageOfNormalWorkDay = 0,
            @IsSilentMode = 1,
            @AreDatesPrinted = 1;
    ```

    Esta instrucción agrega datos de ventas y compra de ejemplo a la base de datos hasta la fecha actual. Muestra el progreso de la generación de datos por día. Debido a un factor aleatorio en la generación de datos, existen algunas diferencias en los datos que se generan entre ejecuciones.

    Para aumentar o disminuir la cantidad de datos generados para pedidos por día, cambie el valor del parámetro `@AverageNumberOfCustomerOrdersPerDay` . Use los parámetros `@SaturdayPercentageOfNormalWorkDay` y `@SundayPercentageOfNormalWorkDay` para determinar el volumen de los días del fin de semana.

> [!TIP]
> Forzar la [durabilidad diferida](../relational-databases/logs/control-transaction-durability.md) en la base de datos puede mejorar la velocidad de generación de datos, especialmente cuando el registro de transacciones de la base de datos se encuentra en un subsistema de almacenamiento de latencia alta. Tenga en cuenta las posibles implicaciones de [pérdida de datos](../relational-databases/logs/control-transaction-durability.md#bkmk_DataLoss) al usar la durabilidad diferida y considere la posibilidad de habilitar la durabilidad diferida para la duración de la generación de datos.

## <a name="import-generated-data-in-wideworldimportersdw"></a>Importar datos generados en WideWorldImportersDW

Para importar datos de ejemplo hasta la fecha actual en la base de datos OLAP de WideWorldImportersDW:

1. Ejecute la lógica de generación de datos en la base de datos OLTP de WideWorldImporters siguiendo los pasos de la sección anterior.
2. Si todavía no lo ha hecho, instale una versión limpia de la base de datos WideWorldImportersDW. Para obtener instrucciones de instalación, consulte [instalación y configuración](wide-world-importers-oltp-install-configure.md).
3. Reinicialice la base de datos OLAP ejecutando la siguiente instrucción en la base de datos:

    ```sql
    EXECUTE [Application].Configuration_ReseedETL
    ```

4. Ejecute el paquete *ETL. ISPAC* de SQL Server Integration Services diaria para importar los datos en la base de datos OLAP. Para obtener información sobre cómo ejecutar el trabajo ETL, consulte [WIDEWORLDIMPORTERS ETL Workflow](wide-world-importers-perform-etl.md).

## <a name="generate-data-in-wideworldimportersdw-for-performance-testing"></a>Generar datos en WideWorldImportersDW para pruebas de rendimiento

WideWorldImportersDW puede aumentar arbitrariamente el tamaño de los datos para las pruebas de rendimiento. Por ejemplo, puede aumentar el tamaño de los datos para usarlos con la indexación de almacén de columnas en clúster.

Uno de los desafíos es mantener el tamaño de la descarga lo suficientemente pequeño como para descargar fácilmente, pero lo suficientemente grande como para mostrar SQL Server características de rendimiento. Por ejemplo, las ventajas significativas para los índices de almacén de columnas solo se logran cuando se trabaja con un número mayor de filas. 

Puede utilizar el `Application.Configuration_PopulateLargeSaleTable` procedimiento para aumentar el número de filas de la `Fact.Sale` tabla. Las filas se insertan en el año natural 2012 para evitar colisiones con los datos de World Wide Importers existentes que comienzan el 1 de enero de 2013.

### <a name="procedure-details"></a>Detalles del procedimiento

#### <a name="name"></a>Nombre

`Application.Configuration_PopulateLargeSaleTable`

#### <a name="parameters"></a>Parámetros

`@EstimatedRowsFor2012`**BIGINT** (con un valor predeterminado de 12 millones)

#### <a name="result"></a>Resultado

Aproximadamente el número necesario de filas se insertan en la `Fact.Sale` tabla en el año 2012. El procedimiento limita artificialmente el número de filas a 50.000 por día. Puede cambiar esta limitación, pero la limitación le ayuda a evitar sobreinflaciones accidentales de la tabla.

El procedimiento también aplica la indexación de almacén de columnas en clúster si aún no se ha aplicado.
