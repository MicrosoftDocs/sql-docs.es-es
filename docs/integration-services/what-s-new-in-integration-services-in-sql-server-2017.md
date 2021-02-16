---
description: Novedades de Integration Services en SQL Server 2017
title: Novedades de Integration Services en SQL Server 2017 | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e26d7884-e772-46fa-bfdc-38567fe976a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b955aa61a4e196fc4c419a9b34e186590006df42
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345671"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2017"></a>Novedades de Integration Services en SQL Server 2017

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En este tema, se describen las características que se han agregado o actualizado en [!INCLUDE[ssSQLv14_md](../includes/sssql17-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].

> [!NOTE]
> SQL Server 2017 también incluye las características de SQL Server 2016 y las características agregadas en las actualizaciones de SQL Server 2016. Para obtener información sobre las nuevas características de SSIS en SQL Server 2016, consulte [Novedades de Integration Services en SQL Server 2016](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md).

## <a name="highlights-of-this-release"></a>Aspectos destacados de esta versión

A continuación se presentan las características nuevas más importantes de Integration Services en SQL Server 2017.

-   **Escalabilidad horizontal**. Distribuya la ejecución de paquetes SSIS más fácilmente en varios equipos de trabajo, y administre ejecuciones y trabajos de un único equipo maestro. Para obtener más información, consulte [Integration Services Scale Out](../integration-services/scale-out/integration-services-ssis-scale-out.md) (Escalabilidad horizontal de Integration Services).

-   **Integration Services en Linux**. Ejecute paquetes SSIS en equipos Linux. Para obtener más información, consulte [Extracción, transformación y carga de datos en Linux con SSIS](../linux/sql-server-linux-migrate-ssis.md) .

-   **Mejoras en la conectividad**. Conéctese a fuentes de OData de Microsoft Dynamics AX Online y Microsoft Dynamics CRM Online con los componentes de OData actualizados. 

## <a name="new-in-azure-data-factory"></a>Novedades en Azure Data Factory

Con la versión preliminar pública de Azure Data Factory versión 2 de septiembre de 2017, ahora puede hacer lo siguiente:
-   Implementar paquetes en la base de datos del catálogo de SSIS (SSISDB) en Azure SQL Database.
-   Ejecutar paquetes implementados en Azure en Integration Runtime para la integración de SSIS en Azure, un componente de Azure Data Factory versión 2.

Para obtener más información, consulte [Lift and shift SQL Server Integration Services workloads to the cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md) (Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift).

Estas nuevas funcionalidades requieren SQL Server Data Tools (SSDT) versión 17.2 o posterior, pero no requieren SQL Server 2017 o SQL Server 2016. Al implementar paquetes en Azure, el Asistente para la implementación de paquetes siempre actualiza los paquetes al formato más reciente.

## <a name="new-in-the-azure-feature-pack"></a>Novedades de Azure Feature Pack

Además de las mejoras de conectividad en SQL Server, Integration Services Feature Pack para Azure admite Azure Data Lake Store. Para obtener más información, consulte la entrada de blog [New Azure Feature Pack Release Strengthening ADLS Connectivity](https://blogs.msdn.microsoft.com/ssis/2017/08/29/new-azure-feature-pack-release-strengthening-adls-connectivity/) (Nueva versión de Azure Feature Pack que refuerza la conectividad de ADLS). Consulte también [Azure Feature Pack for Integration Services (SSIS)](azure-feature-pack-for-integration-services-ssis.md) (Azure Feature Pack para Integration Services [SSIS]).

## <a name="new-in-sql-server-data-tools-ssdt"></a>Novedades de SQL Server Data Tools (SSDT)

Ahora puede desarrollar proyectos y paquetes de SSIS para las versiones de SQL Server desde 2012 hasta 2017 en Visual Studio 2017 o Visual Studio 2015. Para obtener más información, vea [Descargar SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md).

## <a name="new-in-ssis-in-sql-server-2017-rc1"></a>Novedades de SSIS en SQL Server 2017 RC1

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>Características nuevas y modificadas en Escalabilidad horizontal para SSIS

-   Patrón de escalabilidad horizontal ahora admite alta disponibilidad. Puede habilitar Always On para SSISDB y configurar el clúster de conmutación por error de Windows Server para el servidor que hospeda el servicio del patrón de escalabilidad horizontal. Al aplicar este cambio al patrón de escalabilidad horizontal, evita un único punto de error y proporciona una alta disponibilidad para la implementación de escalabilidad horizontal completa.
-   Se mejoró el control de conmutación por error de los registros de ejecución de Trabajadores de escalabilidad horizontal. Los registros de ejecución se guardan en un disco local por si el trabajo de escalabilidad horizontal se detiene inesperadamente. Posteriormente, cuando se reinicia el trabajo, vuelve a cargar los registros persistentes y los sigue guardando en SSISDB.
-   Se cambió el nombre del parámetro *runincluster* del procedimiento almacenado **[catálogo].[create_execution]** a *runinscaleout* para mejorar la coherencia y la legibilidad. Este cambio en el nombre del parámetro tiene las siguientes consecuencias:
    -   Si tiene scripts existentes para ejecutar paquetes en Escalabilidad horizontal, debe cambiar el nombre del parámetro de *runincluster* a *runinscaleout* para que los scripts funcionen en RC1.
    -   SQL Server Management Studio (SSMS) 17.1 y versiones anteriores no pueden desencadenar la ejecución de paquetes en Escalabilidad horizontal en RC1. El mensaje de error es este: " *@runincluster* no es un parámetro para el procedimiento **create_execution**". Este problema se corrige en la versión siguiente de SSMS, la versión 17.2. La versión 17.2 y versiones posteriores de SSMS admiten el nuevo nombre de parámetro y la ejecución de paquetes en Escalabilidad horizontal. Hasta que esté disponible la versión 17.2 de SSMS, como solución alternativa, puede usar la versión existente de SSMS para generar el script de ejecución de paquetes, cambiar el nombre del parámetro *runincluster* a *runinscaleout* en el script y, luego, ejecutar el script.
-   El catálogo de SSIS tiene una nueva propiedad global para especificar el modo predeterminado de ejecución de los paquetes de SSIS. Esta nueva propiedad se aplica cuando llama al procedimiento almacenado **[catalog].[create_execution]** con el parámetro *runinscaleout* establecido en null. Este modo también se aplica a los trabajos del Agente SQL de SSIS. Puede establecer la propiedad global nueva en el cuadro de diálogo Propiedades para el nodo SSISDB en SSMS o con el siguiente comando:
    ```sql
    EXEC [catalog].[configure_catalog] @property_name=N'DEFAULT_EXECUTION_MODE', @property_value=1
    ```

## <a name="new-in-ssis-in-sql-server-2017-ctp-21"></a>Novedades de SSIS en SQL Server 2017 CTP 2.1

### <a name="new-and-changed-features-in-scale-out-for-ssis"></a>Características nuevas y modificadas en Escalabilidad horizontal para SSIS

-   Ahora puede usar el parámetro **Use32BitRuntime** cuando desencadena la ejecución en Escalabilidad horizontal.
-   Se mejoró el rendimiento de inicio de sesión en SSISDB para ejecuciones de paquetes en Escalabilidad horizontal. Ahora, los registros de mensaje del evento y de contexto del mensaje se escriben en SSISDB por lotes en lugar de uno a uno. A continuación se muestran algunas notas adicionales sobre esta mejora:        
    - Actualmente, en algunos informes de la versión actual de SQL Server Management Studio (SSMS) no se muestran estos registros para las ejecuciones en Escalabilidad horizontal. Prevemos que se admitirán en la próxima versión de SSMS. Los informes afectados incluyen el informe *Todas las conexiones*, el informe *Contexto de error* y la sección *Información de conexión* en el Panel Servicio de integración.
    - Se agregó la nueva columna **event_message_guid**. Utilice esta columna para combinar las vistas [catalog].[event_message_context] y [catalog].[event_messages] en lugar de usar **event_message_id** al consultar estos registros de ejecuciones en Escalabilidad horizontal.
-   Para obtener la aplicación de administración para Escalabilidad horizontal de SSIS, [descargue SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) 17.1 o una versión posterior.

## <a name="new-in-ssis-in-sql-server-2017-ctp-20"></a>Novedades de SSIS en SQL Server 2017 CTP 2.0

No hay características nuevas de SSIS en SQL Server 2017 CTP 2.0.

## <a name="new-in-ssis-in-sql-server-2017-ctp-14"></a>Novedades de SSIS en SQL Server 2017 CTP 1.4

No hay características nuevas de SSIS en SQL Server 2017 CTP 1.4.

## <a name="new-in-ssis-in-sql-server-2017-ctp-13"></a>Novedades de SSIS en SQL Server 2017 CTP 1.3

No hay características nuevas de SSIS en SQL Server 2017 CTP 1.3.

## <a name="new-in-ssis-in-sql-server-2017-ctp-12"></a>Novedades de SSIS en SQL Server 2017 CTP 1.2

No hay características nuevas de SSIS en SQL Server 2017 CTP 1.2.

## <a name="new-in-ssis-in-sql-server-2017-ctp-11"></a>Novedades de SSIS en SQL Server 2017 CTP 1.1

No hay características nuevas de SSIS en SQL Server 2017 CTP 1.1.

## <a name="new-in-ssis-in-sql-server-2017-ctp-10"></a>Novedades de SSIS en SQL Server 2017 CTP 1.0

### <a name="scale-out-for-ssis"></a>Escalado horizontal para SSIS

La característica de escalado horizontal facilita mucho la ejecución de [!INCLUDE[ssIS_md](../includes/ssis-md.md)] en varios equipos. 
   
Después de instalar el patrón y los trabajadores de escalado horizontal, se puede distribuir el paquete para su ejecución automática en diferentes trabajadores. Si la ejecución finaliza inesperadamente, se vuelve a intentar automáticamente. Además, todas las ejecuciones y los trabajadores pueden administrarse de manera centralizada mediante el patrón.
   
Para obtener más información, consulte [Escalado horizontal de Integration Services](../integration-services/scale-out/integration-services-ssis-scale-out.md).
   
### <a name="support-for-microsoft-dynamics-online-resources"></a>Compatibilidad con recursos de Microsoft Dynamics Online

El origen OData y el administrador de conexiones OData ahora admiten la conexión a fuentes de OData de Microsoft Dynamics AX Online y Microsoft Dynamics CRM Online.