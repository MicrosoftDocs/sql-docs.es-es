---
description: Monitor de ejecución de paquetes y otras operaciones
title: Monitor de ejecución de paquetes y otras operaciones | Microsoft Docs
ms.custom: supportability
ms.date: 06/04/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isoperations.executions.f1
- sql13.ssis.ssms.isoperations.general.f1
ms.assetid: cbbcd79f-ab9b-46ec-84cb-4821c1d16b99
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2716b85dde5aaa7a7e2df15e87004c838b8137ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353023"
---
# <a name="monitor-running-packages-and-other-operations"></a>Monitor de ejecución de paquetes y otras operaciones

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Puede supervisar las ejecuciones de paquetes [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , validaciones de proyectos y otras operaciones mediante una o varias de las herramientas siguientes. Algunas herramientas como las derivaciones de datos solo están disponibles para los proyectos que se implementan en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
-   Registros  
  
     Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
-   Informes  
  
     Para obtener más información, consulte [Reports for the Integration Services Server](#reports).  
  
-   Vistas  
  
     Para obtener más información, vea [Vistas &#40;catálogo de Integration Services&#41;](../../integration-services/system-views/views-integration-services-catalog.md).  
  
-   Contadores de rendimiento  
  
     Para más información, consulte [Performance Counters](../../integration-services/performance/performance-counters.md).  
  
-   Derivaciones de datos  

> [!NOTE]
> En este artículo se describe cómo supervisar la ejecución de los paquetes SSIS en general y la de los paquetes de forma local. Los paquetes SSIS también se pueden ejecutar y supervisar en Azure SQL Database. Para obtener más información, consulte [Lift and shift SQL Server Integration Services workloads to the cloud](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md) (Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift).
>
> Aunque los paquetes SSIS también se pueden ejecutar en Linux, no se proporcionan herramientas de supervisión. Para obtener más información, consulte [Extracción, transformación y carga de datos en Linux con SSIS](../../linux/sql-server-linux-migrate-ssis.md) .

## <a name="operation-types"></a>Tipos de operación  
 En el catálogo de **SSISDB** se supervisan varios tipos diferentes de operaciones, en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Cada operación puede tener varios mensajes asociados. Cada mensaje se puede clasificar en uno de varios tipos. Por ejemplo, un mensaje puede ser de información, de advertencia o de error. Para obtener la lista completa de tipos de mensaje, vea la documentación de la vista [catalog.operation_messages &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md) de Transact-SQL. Para obtener una lista completa de los tipos de operaciones, vea [catalog.operations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).  
  
 Se usan nueve tipos de estado diferentes para indicar el estado de una operación. Para obtener una lista completa de los tipos de estado, vea la vista [catalog.operations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md).  

## <a name="active-operations-dialog-box"></a><a name="active_ops"></a>Cuadro de diálogo Operaciones activas
  Utilice el cuadro de diálogo **Operaciones activas** para ver el estado de las operaciones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que se están ejecutando actualmente en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , como implementación, validación, y ejecución del paquete. Estos datos se almacenan en el catálogo de SSISDB.  
  
 Para obtener más información sobre las vistas de [!INCLUDE[tsql](../../includes/tsql-md.md)] relacionadas, vea [catalog.operations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operations-ssisdb-database.md), [catalog.validations &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-validations-ssisdb-database.md) y [catalog.executions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md).  
  
###  <a name="open-the-active-operations-dialog-box"></a><a name="open_dialog"></a> Abrir el cuadro de diálogo Operaciones activas  
  
1.  Abra [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
2.  Conéctese al motor de base de datos de Microsoft SQL Server  
  
3.  En el Explorador de objetos, expanda el nodo **Integration Services** , haga clic con el botón derecho en **SSISDB** y, después, haga clic en **Operaciones activas**.  
  
### <a name="configure-the-options"></a>Configurar las opciones  
  
 **Tipo**  
 Especifica el tipo de operación. A continuación se muestran los valores posibles del campo **Tipo** y los valores correspondientes en la columna operations_type de la vista **catalog.operations** de Transact-SQL.  
  
|Descripción del campo de tipo|Valor operations_type|  
|-|-|  
|Inicialización de Integration Services|1|  
|Limpieza de operaciones (trabajo del Agente SQL Server)|2|  
|Limpieza de versiones de proyecto (trabajo del Agente SQL Server)|3|  
|Implementar proyecto|101|  
|Restaurar proyecto|106|  
|Crear e iniciar la ejecución del paquete|200|  
|Detener operación (detención de una validación o una ejecución)|202|  
|Validar proyecto|300|  
|Validar paquete|301|  
|Configurar catálogo|1000|  
  
 **Detención**  
 Haga clic para detener una operación que se esté ejecutando actualmente.  

## <a name="viewing-and-stopping-packages-running-on-the-integration-services-server"></a>Ver y detener los paquetes en ejecución en el Servidor de Integration Services
  La base de datos de **SSISDB** almacena el historial de ejecuciones en tablas internas que no son visibles para los usuarios. Sin embargo, expone la información que necesita a través de vistas públicas que puede consultar. También proporciona procedimientos almacenados a los que puede llamar para realizar tareas frecuentes relacionadas con los paquetes.  
  
 Normalmente, administra los objetos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] del servidor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Sin embargo, también puede consultar las vistas de base de datos y llamar a los procedimientos almacenados directamente, o escribir código personalizado que llame a la API administrada. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y la API administrada consultan las vistas y llaman a los procedimientos almacenados para realizar muchas de sus tareas. Por ejemplo, puede ver la lista de paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que se están ejecutando actualmente en el servidor y solicitar que se detengan si es necesario.  
  
### <a name="viewing-the-list-of-running-packages"></a>Ver la lista de paquetes en ejecución  
 Puede ver la lista de paquetes que hay en ejecución actualmente en el servidor en el cuadro de diálogo **Operaciones activas** . Para más información, consulte [Active Operations Dialog Box](#active_ops).  
  
 Para obtener información sobre los demás métodos que puede usar para ver la lista de paquetes en ejecución, vea los siguientes temas.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] access  
 Para ver la lista de los paquetes que se están ejecutando en el servidor, consulte la vista [catalog.executions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md) para los paquetes que tengan un estado de 2.  
  
 Acceso mediante programación a través de la API administrada  
 Vea el espacio de nombres <xref:Microsoft.SqlServer.Management.IntegrationServices> y sus clases.  
  
### <a name="stopping-a-running-package"></a>Detener un paquete en ejecución  
 En el cuadro de diálogo **Operaciones activas** , puede solicitar que se detenga un paquete en ejecución. Para más información, consulte [Active Operations Dialog Box](#active_ops).  
  
 Para obtener información acerca de los otros métodos que puede usar para detener un paquete en ejecución, vea los temas siguientes.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] access  
 Para detener un paquete en ejecución en el servidor, llame al procedimiento almacenado [catalog.stop_operation &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md).  
  
 Acceso mediante programación a través de la API administrada  
 Vea el espacio de nombres <xref:Microsoft.SqlServer.Management.IntegrationServices> y sus clases.  
  
### <a name="viewing-the-history-of-packages-that-have-run"></a>Ver el historial de paquetes ejecutados  
 Para ver el historial de los paquetes que se han ejecutado en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], use el informe **Todas las ejecuciones** . Para obtener más información sobre el informe **Todas las ejecuciones** y otros informes estándar, vea [Informes para el servidor de Integration Services](#reports).  
  
 Para obtener información acerca de los demás métodos que puede usar para ver el historial de paquetes en ejecución, vea los siguientes temas.  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] access  
 Para ver información sobre los paquetes que se han ejecutado, consulte la vista [catalog.executions &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-executions-ssisdb-database.md).  
  
 Acceso mediante programación a través de la API administrada  
 Vea el espacio de nombres <xref:Microsoft.SqlServer.Management.IntegrationServices> y sus clases.  

## <a name="reports-for-the-integration-services-server"></a><a name="reports"></a> Reports for the Integration Services Server
  En la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]existen dos informes estándar en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para ayudarle a supervisar los proyectos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que se han implementado en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Estos informes le ayudan a ver el estado y historial de los paquetes y, si es necesario, identificar la causa de los errores de ejecución de paquetes.  
  
 En la parte superior de cada página de informe, el icono Atrás le lleva a la página que vio anteriormente, el icono Actualizar actualiza la información que se muestra en la página y el icono Imprimir le permite imprimir la página actual.  
  
 Para obtener más información sobre cómo implementar paquetes en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vea [Deploy Integration Services (SSIS) Projects and Packages](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md) (Implementación de proyectos y paquetes de Integration Services [SSIS]).  
  
### <a name="integration-services-dashboard"></a>Panel de Integration Services  
 El informe **Panel de Integration Services** proporciona información general sobre todas las ejecuciones de paquetes de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para cada paquete que se ha ejecutado en el servidor, el panel le permite 'acercar' para buscar detalles específicos sobre los errores que podrían haberse producido durante la ejecución del paquete.  
  
 El informe muestra las siguientes secciones de información.  
  
|Sección|Descripción|  
|-------------|-----------------|  
|**Información de ejecución**|Muestra el número de ejecuciones en distintos estados (error, en ejecución, correcto, otros) en las últimas 24 horas.|  
|**Información del paquete**|Muestra el número total de paquetes que se han ejecutado en las últimas 24 horas.|  
|**Información de conexión**|Muestra las conexiones que se han usado en las ejecuciones con error en las últimas 24 horas.|  
|**Información detallada del paquete**|Muestra los detalles de las ejecuciones completadas que se han realizado en las últimas 24 horas. Por ejemplo, esta sección muestra el número de ejecuciones con errores frente al número total de ejecuciones, la duración de las ejecuciones (en segundos) y la duración promedio de las ejecuciones durante los últimos tres meses.<br /><br /> Puede ver información adicional sobre un paquete si hace clic en **Información general**, **Todos los mensajes** y **Rendimiento de la ejecución**.<br /><br /> El informe **Rendimiento de la ejecución** muestra la duración de la última instancia de ejecución, así como las horas de inicio y de finalización, y el entorno que se aplicó.<br /><br /> El gráfico y la tabla asociada incluidos en el informe **Rendimiento de la ejecución** muestran la duración de las 10 últimas ejecuciones correctas del paquete. La tabla muestra también la duración promedio de ejecución durante un período de tres meses. Pueden haberse aplicado diferentes entornos y valores literales en tiempo de ejecución para estas 10 ejecuciones correctas del paquete.<br /><br /> Por último, el informe **Rendimiento de la ejecución** muestra el Tiempo activo y el TIempo total para los componentes de flujo de datos del paquete. El Tiempo activo se refiere al tiempo total que el componente ha pasado ejecutándose en todas las fases y el Tiempo total se refiere al tiempo total transcurrido para un componente. El informe solo muestra esta información para los componentes del paquete cuando el nivel de registro de la ejecución del último paquete se estableció en Performance (Rendimiento) o Verbose (Detallado).<br /><br /> El informe **Información general** muestra el estado de las tareas del paquete. El informe **Mensajes** muestra los mensajes de eventos y los mensajes de error para el paquete y las tareas, como las horas de inicio y de finalización, y el número de filas escritas.<br /><br /> También puede hacer clic **Ver mensajes** en el informe **Información general** para navegar al informe **Mensajes** . También puede hacer clic **Ver información general** en el informe **Mensajes** para navegar al informe **Información general** .|  
  
 Puede filtrar la tabla que se muestra en cualquier página si hace clic en **Filtro** y selecciona los criterios deseados en el cuadro de diálogo **Configuración de filtro** . Los criterios de filtro disponibles dependen de los datos que se muestran. Puede cambiar el criterio de ordenación del informe haciendo clic en el icono de ordenación del cuadro de diálogo **Configuración de filtro** .  
  
### <a name="all-executions-report"></a>Informe Todas las ejecuciones  
 El informe **Todas las ejecuciones** muestra un resumen de todas las ejecuciones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] realizadas en el servidor. Puede haber varias ejecuciones del paquete de ejemplo. A diferencia del informe **Panel de Integration Services** , puede configurar el informe **Todas las ejecuciones** para que muestre las ejecuciones que se han iniciado durante un intervalo de fechas. Las fechas pueden abarcar varios días, meses o años.  
  
 El informe muestra las siguientes secciones de información.  
  
|Sección|Descripción|  
|-------------|-----------------|  
|Filter|Muestra el filtro actual que se aplica al informe, como el Intervalo de tiempo de inicio.|  
|Información de ejecución|Muestra la hora de inicio, la hora de finalización y la duración de cada ejecución del paquete. Puede ver una lista de los valores de parámetro usados con una ejecución de paquete, como los valores que se pasaron a un paquete secundario mediante la tarea Ejecutar paquete. Para ver la lista de parámetros, haga clic en Información general.|  
  
 Para obtener más información sobre cómo utilizar la tarea Ejecutar paquete con el fin de que los valores estén disponibles para un paquete secundario, vea [Execute Package Task](../../integration-services/control-flow/execute-package-task.md).  
  
 Para más información sobre los parámetros, vea [Paquete de Integration Services (SSIS) y los parámetros del proyecto](../../integration-services/integration-services-ssis-package-and-project-parameters.md).  
  
### <a name="all-connections"></a>Todas las conexiones  
 El informe **Todas las conexiones** proporciona la siguiente información para las conexiones con errores, para las ejecuciones que se ha realizado en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 El informe muestra las siguientes secciones de información.  
  
|Sección|Descripción|  
|-------------|-----------------|  
|Filter|Muestra el filtro actual que se aplica al informe, como conexiones con una cadena especificada y el intervalo de **Hora del último error** .<br /><br /> Establezca el intervalo de **Hora del último error** para que se muestren solo los errores de conexión que se produjeron durante un intervalo de fechas. El intervalo puede abarcar varios días, meses o años.|  
|Detalles|Muestra la cadena de conexión, el número de ejecuciones en las que se produjo un error de conexión y la fecha en la que hubo un error de conexión por última vez.|  
  
### <a name="all-operations-report"></a>Informe Todas las operaciones  
 El informe **Todas las operaciones** muestra un resumen de todas las operaciones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] realizadas en el servidor, incluidas las de implementación, validación y ejecución de paquetes, así como otras operaciones administrativas. Al igual que con el Panel de Integration Services, puede aplicar un filtro a la tabla para reducir la información mostrada.  
  
### <a name="all-validations-report"></a>Informe Todas las validaciones  
 El informe **Todas las validaciones** muestra un resumen de todas las validaciones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] realizadas en el servidor. El resumen muestra información para cada validación, como el estado, la hora de inicio y la hora de finalización. Cada entrada de resumen incluye un vínculo a los mensajes generados durante la validación. Al igual que con el Panel de Integration Services, puede aplicar un filtro a la tabla para reducir la información mostrada.  
  
### <a name="custom-reports"></a>informes personalizados  
 Puede agregar un informe personalizado (archivo .rdl) al nodo del catálogo de **SSISDB** en el nodo **Catálogos de Integration Services** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Antes de agregar el informe, confirme que usa una convención de nomenclatura de tres partes para calificar completamente los objetos a los que hace referencia como una tabla de origen. De lo contrario, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] mostrará un error. La convención de nomenclatura es \<database>.\<owner>.\<object>. Un ejemplo sería SSISDB.internal.executions.  
  
> [!NOTE]  
>  Al agregar informes personalizados al nodo **SSISDB** en el nodo **Bases de datos** , el prefijo de SSISDB no es necesario.  
  
 Para obtener instrucciones sobre cómo agregar y agregar un informe personalizado, vea [Add a Custom Report to Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md).  

## <a name="view-reports-for-the-integration-services-server"></a>Ver informes del servidor de Integration Services
  En la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]existen dos informes estándar en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para ayudarle a supervisar los proyectos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que se han implementado en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  Para más información sobre los informes, vea [Informes para el servidor de Integration Services](#reports).  
  
### <a name="to-view-reports-for-the-integration-services-server"></a>Para ver informes del servidor de Integration Services  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda el nodo **Catálogos de Integration Services** del Explorador de objetos.  
  
2.  Haga clic con el botón derecho en **SSISDB**, haga clic en **Informes** y, después, haga clic en **Informes estándar**.  
  
3.  Haga clic en una o más de las opciones siguientes para ver un informe.  
  
    -   **Panel de Integration Services**  
  
    -   **Todas las ejecuciones**  
  
    -   **Todas las validaciones**  
  
    -   **Todas las operaciones**  
  
    -   **Todas las conexiones**  

## <a name="see-also"></a>Consulte también  
 [Execution of Projects and Packages](../packages/deploy-integration-services-ssis-projects-and-packages.md)  (Ejecución de proyectos y paquetes)  
 [Solucionar problemas de informes para la ejecución de paquetes](../troubleshooting/troubleshooting-reports-for-package-execution.md)  
