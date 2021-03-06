---
title: Realización de una evaluación de migración SQL Server
titleSuffix: Data Migration Assistant
description: Aprenda a usar Data Migration Assistant para evaluar una SQL Server local antes de migrar a otro SQL Server o a Azure SQL Database
ms.date: 01/15/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 300d88b66c2105235ab04ff616d9fcf81b24e944
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247334"
---
# <a name="perform-a-sql-server-migration-assessment-with-data-migration-assistant"></a>Evaluación de la migración de SQL Server con Data Migration Assistant

Las siguientes instrucciones paso a paso le ayudarán a realizar la primera evaluación de la migración a SQL Server locales, SQL Server que se ejecutan en una máquina virtual de Azure o Azure SQL Database mediante el uso de Data Migration Assistant.

Data Migration Assistant v 5.0 incluye compatibilidad para analizar la conectividad de base de datos y las consultas SQL incrustadas en el código de la aplicación. Para obtener más información, consulte la entrada de blog sobre el [uso de Data Migration Assistant para evaluar el nivel de acceso a datos de una aplicación](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Using-Data-Migration-Assistant-to-assess-an-application-s-data/ba-p/990430).

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="create-an-assessment"></a>Crear una evaluación

1. Seleccione el icono de **nuevo** (+) y, a continuación, seleccione el tipo de proyecto **evaluación** .

2. Establezca el tipo de servidor de origen y de destino.

    Si va a actualizar la instancia de SQL Server local a una instancia de SQL Server local moderna o a SQL Server hospedada en una máquina virtual de Azure, establezca el tipo de servidor de origen y de destino en **SQL Server**. Si va a migrar a Azure SQL Database, establezca el tipo de servidor de destino en **Azure SQL Database**.

3. Haga clic en **Crear**.

   ![Crear una evaluación](../dma/media/dma-assesssqlonprem/new-assessment.png)

## <a name="choose-assessment-options"></a>Elegir opciones de evaluación

1. Seleccione la versión de SQL Server de destino a la que planea migrar.

2. Seleccione el tipo de informe.

   Al evaluar la instancia de SQL Server de origen para migrar a SQL Server local o SQL Server hospedada en destinos de VM de Azure, puede elegir uno de los siguientes tipos de informe de evaluación o ambos:

    - **Problemas de compatibilidad**
    - **Recomendación de nuevas características**

   ![Seleccionar un tipo de informe de evaluación para SQL Server destino](../dma/media/dma-assesssqlonprem/assessment-types.png)

   Al evaluar la instancia de SQL Server de origen para migrar a Azure SQL Database, puede elegir uno de los siguientes tipos de informe de evaluación o ambos:

    - **Check database compatibility (Comprobar compatibilidad de bases de datos)**
    - **Check feature parity (Comprobar paridad de características)**

    ![Seleccionar el tipo de informe de evaluación para SQL Database destino](../dma/media/dma-assesssqlonprem/assessment-types-azure.png)

## <a name="add-databases-and-extended-events-trace-to-assess"></a>Agregar bases de datos y seguimiento de eventos extendidos para evaluar

1. Seleccione **Agregar orígenes** para abrir el menú contextual de la conexión.

2. Escriba el nombre de la instancia de SQL Server, elija el tipo de autenticación, establezca las propiedades de conexión correctas y, a continuación, seleccione **conectar**.

3. Seleccione las bases de datos que desea evaluar y, a continuación, seleccione **Agregar**.

    > [!NOTE]
    > Para quitar varias bases de datos, selecciónelas mientras mantiene presionadas las teclas Mayús o Ctrl y, a continuación, haga clic en **quitar orígenes**. También puede Agregar bases de datos de varias instancias de SQL Server si selecciona **Agregar orígenes**.

4. Si tiene consultas SQL ad hoc o dinámicas o cualquier instrucción DML iniciada a través de la capa de datos de la aplicación, escriba la ruta de acceso a la carpeta en la que colocó todos los archivos de sesión de eventos extendidos que recopiló para capturar la carga de trabajo en el SQL Server de origen.

     En el ejemplo siguiente se muestra cómo crear una sesión de eventos extendidos en el SQL Server de origen para capturar la carga de trabajo de la capa de datos de la aplicación.  Capture la carga de trabajo durante la duración que representa la carga de trabajo máxima.

    ```
    DROP EVENT SESSION [DatalayerSession] ON SERVER
    go
    CREATE EVENT SESSION [DatalayerSession] ON SERVER  
    ADD EVENT sqlserver.sql_batch_completed( 
        ACTION (sqlserver.sql_text,sqlserver.client_app_name,sqlserver.client_hostname,sqlserver.database_id))
    ADD TARGET package0.asynchronous_file_target(SET filename=N'C:\temp\Demos\DataLayerAppassess\DatalayerSession.xel')  
    WITH (MAX_MEMORY=2048 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=3 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
    go
    ---Start the session
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = START;
    ---Wait for few minutes
    
    ---Query events
        
        SELECT 
        object_name,
        CAST(event_data as xml) as event_data,
        file_name, 
        file_offset
    FROM sys.fn_xe_file_target_read_file('C:\temp\Demos\DataLayerAppassess\DatalayerSession*xel', 
                'C:\\temp\\Demos\\DataLayerAppassess\\DatalayerSession*xem', 
                null,
                null)
    ---Stop the session after capturing the peak load.
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = STOP;
        
        go
    ```

5. Haga clic en **Siguiente** para iniciar la evaluación.

    ![Adición de orígenes e inicio de la evaluación](../dma/media/dma-assesssqlonprem/select-database1.png)

> [!NOTE]
> Para ejecutar varias evaluaciones simultáneamente y ver su estado, abra la página **All Assessments** (Todas las evaluaciones).

## <a name="view-results"></a>Vista de resultados

La duración de la valoración depende del número de bases de datos agregadas y del tamaño de esquema de cada base de datos. Los resultados se mostrarán para cada base de datos en cuanto estén disponibles.

1. Seleccione la base de datos que ha completado la evaluación y, a continuación, cambie entre los **problemas de compatibilidad** y las **recomendaciones de características** mediante el modificador.

2. Revise los problemas de compatibilidad en todos los niveles de compatibilidad admitidos por la versión de SQL Server de destino que seleccionó en la página **Opciones** .

Puede revisar los problemas de compatibilidad analizando el objeto afectado, sus detalles y, potencialmente, una corrección para cada problema identificado en **cambios importantes**, **cambios de comportamiento** y **características en desuso**.

![Ver los resultados de la evaluación](../dma/media/dma-assesssqlonprem/review-results.png)

Del mismo modo, puede revisar las recomendaciones sobre características en las áreas de **rendimiento**, **almacenamiento** y **seguridad** .

Las recomendaciones de características cubren diferentes tipos de características, como In-Memory OLTP, almacén de columnas, Stretch Database, Always Encrypted, Enmascaramiento dinámico de datos y Cifrado de datos transparente.

![Ver recomendaciones de características](../dma/media/dma-assesssqlonprem/feature-recommendations.png)

Por Azure SQL Database, las evaluaciones proporcionan problemas de bloqueo de la migración y problemas de paridad de características. Revise los resultados de ambas categorías seleccionando las opciones específicas.

- La categoría de **paridad de características SQL Server** proporciona un conjunto completo de recomendaciones, enfoques alternativos disponibles en Azure y procedimientos de mitigación. Le ayuda a planear este esfuerzo en los proyectos de migración.

  ![Ver información de SQL Server paridad de características](../dma/media/dma-assesssqlonprem/sql-feature-parity.png)

- La categoría de **problemas de compatibilidad** proporciona características parcialmente compatibles o no compatibles que bloquean la migración de bases de datos locales de SQL Server a bases de datos SQL de Azure. A continuación, se proporcionan recomendaciones para ayudarle a solucionar esos problemas.

  ![Ver problemas de compatibilidad](../dma/media/dma-assesssqlonprem/compatibility-issues.png)

## <a name="assess-a-data-estate-for-target-readiness"></a>Evaluación de la disponibilidad de los datos para el destino

Si desea ampliar aún más estas evaluaciones a toda la información y encontrar la preparación relativa de SQL Server instancias y bases de datos para la migración a Azure SQL Database, cargue los resultados en el concentrador de Azure Migrate seleccionando **cargar en Azure Migrate**.

Esto le permite ver los resultados consolidados en el proyecto de Azure Migrate Hub.

[Aquí](./dma-assess-sql-data-estate-to-sqldb.md)encontrará una guía paso a paso detallada para las evaluaciones de preparación de destino.

   ![Cargar resultados en Azure Migrate](../dma/media/dma-assesssqlonprem/upload-to-azure-migrate.png)

## <a name="export-results"></a>Exportar resultados

Una vez finalizada la evaluación de todas las bases de datos, seleccione **exportar Informe** para exportar los resultados a un archivo JSON o a un archivo CSV. A continuación, puede analizar los datos con su comodidad.

## <a name="save-and-load-assessments"></a>Guardado y carga de evaluaciones

Además de exportar los resultados de una evaluación, puede guardar los detalles de la evaluación en un archivo y cargar un archivo de evaluación para revisarlo más adelante.  Para obtener más información, consulte el artículo sobre cómo [Guardar y cargar evaluaciones con Data Migration Assistant](../dma/dma-save-load-assessments.md).
