---
description: Herramientas para solucionar problemas con el desarrollo de paquetes
title: Herramientas para solucionar problemas con el desarrollo de paquetes | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Integration Services packages, troubleshooting
- SSIS packages, troubleshooting
- Integration Services, troubleshooting
- errors [Integration Services], troubleshooting
- packages [Integration Services], troubleshooting
ms.assetid: 41dd248c-dab3-4318-b8ba-789a42d5c00c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8555afca3e9b291dcbeb38269bac5603afaf37bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352582"
---
# <a name="troubleshooting-tools-for-package-development"></a>Herramientas para solucionar problemas con el desarrollo de paquetes

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye características y herramientas que se pueden usar para solucionar problemas de los paquetes mientras estos se desarrollan en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
## <a name="troubleshooting-design-time-validation-issues"></a>Solucionar problemas de validación en tiempo de diseño  
 En la versión actual de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], cuando se abre un paquete, el sistema valida todas las conexiones antes de validar todos los componentes del flujo de datos y establece las conexiones que son lentas o no están disponibles para trabajar sin conexión. Esto ayuda a reducir el retardo al validar el flujo de datos del paquete.  
  
 Después de abrir un paquete, también puede desactivar una conexión si hace clic con el botón derecho en el administrador de conexiones en el área **Administradores de conexiones** y, después, hace clic en **Trabajar sin conexión**. Esto puede acelerar las operaciones en el Diseñador SSIS.  
  
 Las conexiones establecidas para trabajar sin conexión seguirán sin conexión hasta que realice uno de los procedimientos siguientes:  
  
-   Pruebe la conexión; para ello, haga clic con el botón derecho en el administrador de conexiones en el área **Administradores de conexiones** del Diseñador SSIS y, después, haga clic en **Probar conectividad**.  
  
     Por ejemplo, una conexión se ha establecido inicialmente para trabajar sin conexión al abrir el paquete. Puede modificar la cadena de conexión para resolver el problema y hacer clic en **Probar conectividad** para probar la conexión.  
  
-   Vuelva a abrir el paquete y el proyecto que contiene el paquete. La validación se ejecuta de nuevo en todas las conexiones del paquete.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye las siguientes características adicionales para ayudarle a evitar los errores de validación:  
  
-   **Establezca todo el paquete y todas las conexiones para trabajar sin conexión cuando los orígenes de datos no estén disponibles**. Puede habilitar **Trabajar sin conexión** en el menú **SSIS** . A diferencia de la propiedad **DelayValidation** , la opción **Trabajar sin conexión** se encuentra disponible incluso antes de abrir un paquete. También se puede habilitar la opción **Trabajar sin conexión** para acelerar las operaciones en el diseñador, y deshabilitarla solo cuando se quiere validar el paquete.  
  
-   **Configure la propiedad DelayValidation para los elementos del paquete que no son válidos hasta el tiempo de ejecución**. Se puede establecer **DelayValidation** en **True** para los elementos del paquete cuya configuración no sea válida en tiempo de diseño a fin de evitar errores de validación. Por ejemplo, puede haber una tarea Flujo de datos que utilice una tabla de destino inexistente antes de que la tarea de ejecución de SQL cree la tabla en tiempo de ejecución. La propiedad **DelayValidation** se puede habilitar en el nivel de paquete o en el de tareas y contenedores individuales incluidos en el paquete. Por lo general, se debe mantener el valor de esta propiedad en **True** para los mismos elementos del paquete cuando se implementa el paquete, a fin de evitar los mismos errores de validación en tiempo de ejecución.  
  
     Es posible establecer la propiedad **DelayValidation** para una tarea Flujo de datos, pero no para componentes individuales de flujo de datos. Se puede obtener un resultado similar al establecer la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> para componentes individuales de flujo de datos en **false**. No obstante, cuando el valor de esta propiedad es **false**, el componente no detecta los cambios realizados en los metadatos de los orígenes de datos externos.  
  
 Si los objetos de base de datos usados por el paquete están bloqueados durante la validación, es posible que este proceso deje de responder. En estas circunstancias, el Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] también deja de responder. Puede reanudar la validación usando [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] para cerrar la sesión asociada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. También puede evitar este problema si usa la configuración descrita en esta sección.  
  
## <a name="troubleshooting-control-flow"></a>Solucionar problemas de flujo de control  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye las siguientes características y herramientas que puede usar para solucionar problemas del flujo de control en paquetes durante el desarrollo de paquetes:  
  
-   **Establecer puntos de interrupción en tareas, en contenedores y en el paquete**. Puede establecer puntos de interrupción con las herramientas gráficas que proporciona el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Los puntos de interrupción se pueden habilitar en el nivel de paquete o en el de tareas y contenedores individuales incluidos en el paquete. Algunas tareas y contenedores proporcionan condiciones de interrupción adicionales para establecer puntos de interrupción. Por ejemplo, puede habilitar una condición de interrupción en el contenedor de bucles For que suspenda la ejecución al principio de cada iteración del bucle.  
  
-   **Usar las ventanas de depuración**. Al ejecutar un paquete con puntos de interrupción, las ventanas de depuración de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] proporcionan acceso a valores de variables y mensajes de estado.  
  
-   **Revisar la información de la pestaña Progreso**. [!INCLUDE[ssIS](../../includes/ssis-md.md)] proporciona información adicional sobre el flujo de control al ejecutar un paquete en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. En la pestaña Progreso se muestran las tareas y los contenedores en orden de ejecución; incluye las horas de inicio y finalización, las advertencias y los mensajes de error de cada tarea y contenedor, incluido el paquete en sí.  
  
 Para obtener más información acerca de estas características, vea [Debugging Control Flow](../../integration-services/troubleshooting/debugging-control-flow.md).  
  
## <a name="troubleshooting-data-flow"></a>Solucionar problemas de flujo de datos  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye las siguientes características y herramientas que puede usar para solucionar problemas del flujo de datos en paquetes durante el desarrollo de paquetes:  
  
-   **Hacer pruebas con solo un subconjunto de los datos**. Si desea solucionar problemas del flujo de datos en un paquete utilizando únicamente una muestra del conjunto de datos, puede incluir una transformación Muestreo de porcentaje o Muestreo de fila para crear una muestra de los datos insertados en tiempo de ejecución. Para obtener más información, consulte [Percentage Sampling Transformation](../../integration-services/data-flow/transformations/percentage-sampling-transformation.md) y [Row Sampling Transformation](../../integration-services/data-flow/transformations/row-sampling-transformation.md).  
  
-   **Usar los visores de datos para supervisar los datos a medida que éstos se mueven por el flujo de datos**. Los visores de datos muestran los valores de los datos a medida que estos se mueven entre orígenes, transformaciones y destinos. Un visor de datos puede mostrar los datos en una cuadrícula. Puede copiar los datos de un visor de datos al Portapapeles y, después, pegarlos en un archivo o en una hoja de cálculo de Excel. Para obtener más información, consulte [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md) .  
  
-   **Configurar salidas de error para los componentes de flujo de datos que las admitan**. Muchos orígenes, transformaciones y destinos del flujo de datos también admiten salidas de error. La configuración de la salida de error de un componente de flujo de datos permite dirigir los datos que contienen errores a otro destino. Por ejemplo, puede capturar los datos que generaron errores o se truncaron en un archivo de texto independiente. También se pueden adjuntar visores de datos a las salidas de error y examinar solo los datos erróneos. En tiempo de diseño, las salidas de error capturan valores de datos con problemas para ayudarle a desarrollar paquetes que controlen de forma eficaz los datos del mundo real. Sin embargo, mientras que otras herramientas y características de solución de problemas solamente son útiles en tiempo de diseño, las salidas de error siguen siendo útiles en el entorno de producción. Para más información, vea [Control de errores en los datos](../../integration-services/data-flow/error-handling-in-data.md).  
  
-   **Capturar el recuento de filas procesadas**. Al ejecutar un paquete en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , el número de filas que ha pasado a través de una ruta se muestra en el diseñador de flujo de datos. Este número se actualiza periódicamente mientras los datos pasan por la ruta. También puede agregar una transformación Recuento de filas al flujo de datos para capturar el recuento de filas final en una variable. Para más información, consulte [Row Count Transformation](../../integration-services/data-flow/transformations/row-count-transformation.md).  
  
-   **Revisar la información de la pestaña Progreso**. [!INCLUDE[ssIS](../../includes/ssis-md.md)] proporciona información adicional sobre los flujos de datos al ejecutar un paquete en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. En la pestaña Progreso se muestran los componentes de flujo de datos en el orden de ejecución, y se incluye información sobre su progreso para cada fase del paquete, mostrado como porcentaje finalizado, y el número de filas escritas en el destino.  
  
 Para obtener más información acerca de estas características, vea [Debugging Data Flow](../../integration-services/troubleshooting/debugging-data-flow.md).  
  
## <a name="troubleshooting-scripts"></a>Solucionar problemas de scripts  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) es el entorno de desarrollo en el cual se escriben los scripts que la tarea Script y el componente Script han usado. VSTA dispone de las siguientes características y herramientas que pueden usarse para solucionar problemas de los scripts durante el desarrollo de paquetes:  
  
-   **Establecimiento de puntos de interrupción en un script en las tareas Script.** VSTA solamente proporciona compatibilidad con la depuración de scripts en la tarea Script. Los puntos de interrupción establecidos en las tareas Script se integran con los puntos de interrupción establecidos en los paquetes y en las tareas y contenedores del paquete, lo que permite la depuración fluida de todos los elementos del paquete.  
  
    > [!NOTE]  
    >  Al depurar un paquete que contiene varias tareas Script, el depurador alcanzará los puntos de interrupción solo en una de las tareas Script y omitirá los puntos de interrupción del resto. Si una tarea Script forma parte de un contenedor de bucles Foreach o For, el depurador omite los puntos de interrupción en la tarea Script después de la primera iteración del bucle.  
  
 Para más información, consulte [Debugging Script](../../integration-services/troubleshooting/debugging-script.md). Para obtener sugerencias sobre cómo depurar el componente de script, vea [Codificar y depurar el componente de script](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
## <a name="troubleshooting-errors-without-a-description"></a>Solucionar problemas de errores sin descripción  
 Si se encuentra con un número de error de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sin una descripción asociada durante el desarrollo de un paquete, puede ver la descripción en [Referencia de errores y mensajes de Integration Services](../../integration-services/integration-services-error-and-message-reference.md). En este momento, la lista no incluye información sobre cómo solucionar problemas.  
  
## <a name="see-also"></a>Consulte también  
 [Herramientas para solucionar problemas con la ejecución de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)   
 [Características de rendimiento del flujo de datos](../../integration-services/data-flow/data-flow-performance-features.md)  
  
  
