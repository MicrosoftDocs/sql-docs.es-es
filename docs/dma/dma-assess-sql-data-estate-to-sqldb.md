---
title: Evaluar SQL Server preparación para migrar a Azure SQL Database
titleSuffix: Data Migration Assistant
description: Obtenga información sobre cómo usar Data Migration Assistant para migrar una SQL Server de datos para la migración a Azure SQL Database
ms.date: 12/19/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 1094f10eedc819123cd446c0933ec14df511ac75
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247346"
---
# <a name="assess-the-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql-database-using-the-data-migration-assistant"></a>Evaluar la preparación de una SQL Server la migración de datos a Azure SQL Database mediante el Data Migration Assistant

La migración de cientos de instancias de SQL Server y miles de bases de datos a Azure SQL Database o de Azure SQL Instancia administrada, nuestras ofertas de plataforma como servicio (PaaS), es una tarea considerable. Para simplificar el proceso tanto como sea posible, debe sentirse seguro de la preparación relativa de la migración. La identificación de frutas de bajo nivel, incluidos los servidores y las bases de datos que están totalmente preparados o que requieren un esfuerzo mínimo para prepararse para la migración, facilitan y aceleran sus esfuerzos.

En este artículo se proporcionan instrucciones paso a paso para aprovechar el [Data Migration Assistant](./dma-overview.md) para resumir los resultados de la preparación y exponerlos en el concentrador de [Azure Migrate](https://portal.azure.com/?feature.customPortal=false#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview) .

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

>
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Migration-Assistant/player?WT.mc_id=dataexposed-c9-niner]

## <a name="create-a-project-and-add-a-tool"></a>Creación de un proyecto e incorporación de una herramienta

Configure un nuevo proyecto de Azure Migrate en una suscripción de Azure y, después, agregue una herramienta.

El proyecto de Azure Migrate se usa para almacenar los metadatos de detección, valoración y migración recopilados del entorno que se va a evaluar o migrar. También se usa un proyecto para realizar el seguimiento de los activos detectados y para organizar la evaluación y la migración.

1. Inicie sesión en el Azure Portal, seleccione **todos los servicios** y, a continuación, busque Azure Migrate.
2. En **Servicios**, seleccione **Azure Migrate**.

   ![Azure Migrate: seleccionar servicio](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-services.png)

3. En la página **información general** , seleccione **evaluar y migrar bases de datos**.

   ![Azure Migrate-inicio de la evaluación](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

4. En **bases de datos**, en **Introducción**, seleccione **Agregar herramientas**.

   ![Azure Migrate-agregar herramientas](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tools.png)

5. En la pestaña **migrar proyecto** , seleccione su suscripción de Azure y el grupo de recursos (si aún no tiene un grupo de recursos, cree uno).
6. En **detalles del proyecto**, especifique el nombre del proyecto y la geografía en la que desea crear el proyecto.

    ![Azure Migrate: agregar un cuadro de diálogo de herramientas](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tool-dialog.png)

    Puede crear un proyecto de Azure Migrate en cualquiera de estas zonas geográficas.

    | **Geografía**  | **Región de ubicación del almacenamiento** |
    | ------------- | ------------- |
    | Asia | Sudeste de Asia o Este de Asia |
    | Europa | Sur de Europa u Oeste de Europa |
    | Reino Unido | Sur de Reino Unido u Oeste de Reino Unido |
    | Estados Unidos | Centro de EE. UU. u Oeste de EE. UU. 2 |

    La ubicación geográfica especificada para el proyecto solo se utiliza para almacenar los metadatos que se recopilan a partir de máquinas virtuales locales. Puede seleccionar cualquier región de destino para la migración real.

7. Seleccione **siguiente** y, a continuación, agregue una herramienta de evaluación.

   > [!NOTE]
   > Al crear un proyecto, debe agregar al menos una herramienta de evaluación o de migración.

8. En la pestaña **Seleccionar herramienta de evaluación** , **Azure Migrate: evaluación de base de datos** aparece como la herramienta de evaluación que se va a agregar. Si actualmente no necesita una herramienta de evaluación, active la casilla **omitir la adición de una herramienta de evaluación ahora** . Seleccione **Siguiente**.

    ![Azure Migrate: ficha seleccionar herramienta de evaluación](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-assessment-tool.png)

9. En la pestaña **Seleccionar herramienta de migración** , **Azure Migrate: la migración de bases de datos** aparece como la herramienta de migración que se va a agregar. Si actualmente no necesita una herramienta de migración, seleccione la **herramienta para omitir la adición de una migración por ahora**. Seleccione **Siguiente**.

    ![Azure Migrate: ficha seleccionar herramienta de migración](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-migration-tool.png)

10. En **revisar y agregar herramientas**, revise la configuración y seleccione **Agregar herramientas**.

    ![Azure Migrate: revisar y agregar pestañas de herramientas](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-tools.png)

    Después de crear el proyecto, puede seleccionar herramientas adicionales para la valoración y la migración de servidores, cargas de trabajo, bases de datos y aplicaciones web.

## <a name="assess-and-upload-assessment-results"></a>Evaluación y carga de los resultados de la evaluación

Después de crear correctamente un proyecto de migración, en **herramientas de evaluación**, en el cuadro **Azure Migrate: evaluación de base de datos** , instrucciones para descargar y usar la presentación de la herramienta de Data Migration Assistant.

   ![Herramienta de evaluación de Azure Migrate agregada](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessment-tool-added.png)

1. Descargue Data Migration Assistant mediante el vínculo proporcionado y, a continuación, instálelo en un equipo con acceso a las instancias de SQL Server de origen.
2. Inicie Data Migration Assistant.

### <a name="create-an-assessment"></a>Crear una evaluación

1. A la izquierda, seleccione el **+** icono y, a continuación, seleccione el **tipo de proyecto** evaluación.
2. Especifique el nombre del proyecto y, a continuación, seleccione los tipos de servidor de origen y de servidor de destino.

    Si está actualizando la instancia de SQL Server local a una versión posterior de SQL Server o SQL Server hospedada en una máquina virtual de Azure, establezca el tipo de servidor de origen y de destino en **SQL Server**. Establezca el tipo de servidor de destino en **Azure SQL instancia administrada** para obtener una evaluación de preparación de destino de Azure SQL Database (PaaS).

3. Seleccione **Crear**.

   ![Interfaz de Data Migration Assistant de Azure Migrate](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-interface.png)

### <a name="choose-assessment-options"></a>Elegir opciones de evaluación

1. Seleccione el tipo de informe.

    Puede elegir uno de los siguientes tipos de informe o ambos:
    * Check database compatibility (Comprobar compatibilidad de bases de datos)
    * Check feature parity (Comprobar paridad de características)

   ![Azure Migrate Data Migration Assistant pantalla de opciones de evaluación](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-options-screen.png)

2. Seleccione **Siguiente**.

### <a name="add-databases-to-assess"></a>Adición de bases de datos para evaluar

1. Seleccione **Agregar orígenes** para abrir el menú desplegable conexión.
2. Escriba el nombre de la instancia de SQL Server, elija el tipo de autenticación, establezca las propiedades de conexión correctas y, a continuación, seleccione **conectar**.
3. Seleccione las bases de datos que desea evaluar y, a continuación, seleccione **Agregar**.

   > [!NOTE]
   > Para quitar varias bases de datos, selecciónelas mientras mantiene presionadas las teclas Mayús o Ctrl y, a continuación, haga clic en quitar orígenes. También puede Agregar bases de datos de varias instancias de SQL Server mediante el botón Agregar orígenes.

4. Seleccione **Siguiente** para iniciar la evaluación.

   ![Azure Migrate-Data Migration Assistant: pantalla de orígenes seleccionados](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-select-sources-screen.png)

5. Una vez finalizada la evaluación, seleccione **cargar en Azure Migrate**.

   ![Captura de pantalla que muestra el Data Migration Assistant con la opción cargar en Azure Migrate llamada.](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-review-results-screen.png)

6. Inicie sesión en Azure Portal.

   ![Captura de pantalla del Data Migration Assistant que muestra la ventana Azure Portal inicio de sesión.](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-signin.png)

7. Seleccione la suscripción y Azure Migrate proyecto en el que desea cargar los resultados de la evaluación y, a continuación, seleccione **cargar**.

   Espere a que se confirme la confirmación de carga de evaluación.

## <a name="view-target-readiness-assessment-results"></a>Ver resultados de evaluación de preparación de destino

1. Inicie sesión en el Azure Portal, busque Azure Migrate y seleccione **Azure Migrate**.

   ![Azure Migrate-Azure Portal-búsqueda de servicios](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-search.png)

2. Seleccione **evaluar y migrar bases de datos** para obtener los resultados de la evaluación.

   ![Azure Migrate: revisar los resultados de la evaluación](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

    Puede ver el Resumen de preparación de SQL Server, asegúrese de que está en el proyecto de migración correcto; de lo contrario, use la opción cambiar para seleccionar otro proyecto de migración.

    Cada vez que actualice los resultados de la evaluación al proyecto de Azure Migrate, Azure Migrate Hub consolidará todos los resultados y proporcionará el informe de resumen.  Puede ejecutar varias evaluaciones de Data Migration Assistant en paralelo y cargar los resultados en el proyecto de migración único para obtener el informe de preparación consolidada.

   ![Azure Migrate: revisar los resultados de la preparación](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-readiness.png)

    **Instancias de base de datos evaluadas**: el número de instancias de SQL Server que se han evaluado hasta ahora.
    **Bases de datos evaluadas**: número total de bases de datos evaluadas en una o varias instancias de SQL Server se han evaluado **las bases de datos preparadas para SQL Database**: el número de bases de datos que están listas para migrar a Azure SQL Database (PaaS).
    **Bases de datos listas para la máquina virtual de Azure SQL**: el número de bases de datos consta de uno o varios bloqueadores de migración para Azure SQL Database (PaaS), pero está listo para migrar a Azure SQL Server máquinas virtuales.

3. Seleccione **instancias de base de datos evaluadas** para obtener acceso a SQL Server vista de nivel de instancia.

   ![Azure Migrate-revisar la preparación de la instancia](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-instances.png)

    Puede encontrar el estado de disponibilidad porcentual de cada instancia de SQL Server que migra a Azure SQL Database (PaaS).

4. Seleccione una instancia específica para obtener acceso a la vista preparación de la base de datos.

   ![Azure Migrate revisar la preparación de la base de datos](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-databases.png)

    Puede ver el número de bloqueadores de migración por cada base de datos, el destino recomendado por cada base de datos en la vista anterior.

5. Revise los resultados de la evaluación de DMA para obtener más detalles sobre los bloqueadores de migración.

   ![Azure Migrate-revisar los bloqueadores de migración](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-migration-blockers.png)

## <a name="see-also"></a>Consulte también

* [Data Migration Assistant (DMA)](../dma/dma-overview.md)
* [Data Migration Assistant: opciones de configuración](../dma/dma-configurationsettings.md)
* [Data Migration Assistant: procedimientos recomendados](../dma/dma-bestpractices.md)
