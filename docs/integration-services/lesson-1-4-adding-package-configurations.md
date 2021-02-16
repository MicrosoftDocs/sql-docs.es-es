---
description: 'Lección 1-4: Agregar configuraciones de paquetes'
title: 'Paso 4: Agregar configuraciones de paquetes | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: e04a5321-63d5-4ec5-85b9-cb4eaf6c87f6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ca2d3bb08a1059970b8c72da5541b2ad3d9d1c8d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354335"
---
# <a name="lesson-1-4---adding-package-configurations"></a>Lección 1-4: Agregar configuraciones de paquetes

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En esta tarea, agregará una configuración a cada paquete. Las configuraciones actualizan los valores de las propiedades de los paquetes y los objetos de los paquetes en tiempo de ejecución.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] proporciona diversos tipos de configuración. Puede almacenar configuraciones en variables de entorno, entradas del Registro, variables definidas por el usuario, tablas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y archivos XML. Para proporcionar más flexibilidad, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] admite el uso de configuraciones indirectas. Esto significa que se usa una variable de entorno para especificar la ubicación de la configuración, que a su vez especifica los valores reales. Los paquetes del proyecto Deployment Tutorial utilizan una combinación de archivos de configuración XML y configuraciones indirectas. Un archivo de configuración XML puede incluir configuraciones de varias propiedades y, si hace falta, varios paquetes pueden hacer referencia a él. En este tutorial, utilizará un archivo de configuración independiente para cada paquete.  
  
Los archivos de configuración suelen contener información confidencial como cadenas de conexión. Por tanto, debe utilizar una lista de control de acceso (ACL) para restringir el acceso a la ubicación o a la carpeta donde se almacenan los archivos, y permitir el acceso solamente a los usuarios o cuentas que pueden ejecutar paquetes. Para más información, vea [Acceso a los archivos usados por los paquetes](../integration-services/security/security-overview-integration-services.md#files).  
  
Los paquetes (DataTransfer y LoadXMLData) que ha agregado al proyecto Deployment Tutorial en la tarea anterior necesitan configuraciones para ejecutarse correctamente una vez que se han implementado en el servidor de destino. Para implementar configuraciones, primero creará las configuraciones indirectas para los archivos de configuración XML y, a continuación, creará los archivos de configuración XML.  
  
Creará dos archivos de configuración, DataTransferConfig.dtsConfig y LoadXMLData.dtsConfig. Estos archivos contienen los pares de nombre/valor que actualizan las propiedades en los paquetes que especifican la ubicación de los datos y los archivos de registro utilizados por el paquete. Más tarde, como un paso del proceso de implementación, actualizará los valores de los archivos de configuración para reflejar la nueva ubicación de los archivos en el equipo de destino.  
  
[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] reconoce que los archivos DataTransferConfig.dtsConfig y LoadXMLData.dtsConfig son dependencias de los paquetes DataTransfer y LoadXMLData y automáticamente incluye los archivos de configuración al crear el paquete de implementación en la siguiente lección.  
  
### <a name="to-create-indirect-configuration-for-the-datatransfer-package"></a>Para crear una configuración indirecta para el paquete DataTransfer  

Compruebe el modelo de implementación actual del proyecto y establézcalo en **Modelo de implementación de paquetes** si es necesario. En el menú **Proyecto**, haga clic en **Convertir al modelo de implementación de paquetes**.
  
1.  En el Explorador de soluciones, haga doble clic en DataTransfer.dtsx.  
  
2.  En el Diseñador de [!INCLUDE[ssIS](../includes/ssis-md.md)] , haga clic en cualquier parte del fondo de la superficie de diseño del flujo de control.  
  
3.  En el menú **SSIS** , haga clic en **Configuraciones de paquetes**.  
  
4.  En el cuadro de diálogo **Organizador de configuraciones de paquetes**, seleccione **Habilitar configuraciones de paquetes** si no está seleccionado y haga clic en **Agregar**.  
  
5.  En la página de bienvenida del Asistente para la configuración de paquetes, haga clic en **Siguiente**.  
  
6.  En la página Seleccionar tipo de configuración, seleccione **Archivo de configuración XML** en la lista **Tipo de configuración** , seleccione la opción **La ubicación de configuración se almacena en una variable de entorno** y escriba **DataTransfer** o seleccione la variable de entorno **DataTransfer** en la lista.  
  
    > [!NOTE]  
    > Para hacer que la variable de entorno esté disponible en la lista, puede que tenga que reiniciar el equipo después de agregar la variable. Si no desea reiniciar el equipo, puede escribir el nombre de la variable de entorno.  
  
7.  Haga clic en **Next**.  
  
8.  En la página Finalización del asistente, escriba **Configuración de DataTransfer EV** en el cuadro **Nombre de la configuración** , revise el contenido de la configuración en el panel **Vista previa** y, a continuación, haga clic en **Finalizar**.  
  
9. Cierre el cuadro de diálogo **Organizador de configuraciones de paquetes**.  
  
### <a name="to-create-the-xml-configuration-for-the-datatransfer-package"></a>Para crear una configuración XML para el paquete DataTransfer  
  
1.  En el Explorador de soluciones, haga doble clic en DataTransfer.dtsx.  
  
2.  En el Diseñador de [!INCLUDE[ssIS](../includes/ssis-md.md)] , haga clic en cualquier parte del fondo de la superficie de diseño del flujo de control.  
  
3.  En el menú **SSIS** , haga clic en **Configuraciones de paquetes**.  
  
4.  En el cuadro de diálogo Package Configuration Organizer (Organizador de configuraciones de paquetes), active la casilla **Habilitar configuraciones de paquetes** y haga clic en **Agregar**.  
  
5.  En la página de bienvenida del Asistente para la configuración de paquetes, haga clic en **Siguiente**.  
  
6.  En la página Seleccionar tipo de configuración, seleccione **Archivo de configuración XML** en la lista **Tipo de configuración** y, a continuación, haga clic en **Examinar**.  
  
7.  En el cuadro de diálogo **Seleccionar ubicación del archivo de configuración** , vaya a C:\DeploymentTutorial, escriba **DataTransferConfig** en el cuadro **Nombre de archivo** y, después, haga clic en **Guardar**.  
  
8.  En la página Seleccionar tipo de configuración, haga clic en **Siguiente**.  
  
9. En la página Seleccionar propiedades para la exportación, expanda DataTransfer, Administradores de conexión, Registro del tutorial de implementación y Propiedades y, luego, active la casilla **Cadena de conexión** .  
  
10. Dentro de Administradores de conexión, expanda NewCustomers y, después, active la casilla **Cadena de conexión** .  
  
11. Haga clic en **Next**.  
  
12. En la página Finalización del asistente, escriba **Configuración de DataTransfer** en el cuadro **Nombre de la configuración** , revise el contenido de la configuración y, a continuación, haga clic en **Finalizar**.  
  
13. En el cuadro de diálogo **Organizador de configuraciones de paquetes** , compruebe que Configuración de DataTransfer EV y Configuración de DataTransfer aparecen en primer y segundo lugar respectivamente y, a continuación, haga clic en **Cerrar**.  
  
### <a name="to-create-indirect-configuration-for-the-loadxmldata-package"></a>Para crear una configuración indirecta para el paquete LoadXMLData  
  
1.  En el Explorador de soluciones, haga doble clic en LoadXMLData.dtsx.  
  
2.  En el Diseñador de [!INCLUDE[ssIS](../includes/ssis-md.md)] , haga clic en cualquier parte del fondo de la superficie de diseño del flujo de control.  
  
3.  En el menú **SSIS** , haga clic en **Configuraciones de paquetes**.  
  
4.  En el cuadro de diálogo **Organizador de configuraciones de paquetes**, haga clic en **Agregar**.  
  
5.  En la página de bienvenida del Asistente para la configuración de paquetes, haga clic en **Siguiente**.  
  
6.  En la página Seleccionar tipo de configuración, seleccione **Archivo de configuración XML** en la lista **Tipo de configuración** , seleccione la opción **La ubicación de configuración se almacena en una variable de entorno** y escriba **LoadXMLData** o seleccione la variable de entorno **LoadXMLData** en la lista.  
  
    > [!NOTE]  
    > Para hacer que la variable de entorno esté disponible en la lista, puede que tenga que reiniciar el equipo después de agregar la variable.  
  
7.  Haga clic en **Next**.  
  
8.  En la página Finalización del asistente, escriba **Configuración de LoadXMLData EV** en el cuadro **Nombre de la configuración** , revise el contenido de la configuración y, a continuación, haga clic en **Finalizar**.  
  
### <a name="to-create-the-xml-configuration-for-the-loadxmldata-package"></a>Para crear una configuración XML para el paquete LoadXMLData  
  
1.  En el Explorador de soluciones, haga doble clic en LoadXMLData.dtsx.  
  
2.  En el Diseñador de [!INCLUDE[ssIS](../includes/ssis-md.md)] , haga clic en cualquier parte del fondo de la superficie de diseño del flujo de control.  
  
3.  En el menú **SSIS** , haga clic en **Configuraciones de paquetes**.  
  
4.  En el cuadro de diálogo Package Configuration Organizer (Organizador de configuraciones de paquetes), active la casilla **Habilitar configuraciones de paquetes** y, luego, haga clic en **Agregar**.  
  
5.  En la página de bienvenida del Asistente para la configuración de paquetes, haga clic en **Siguiente**.  
  
6.  En la página Seleccionar tipo de configuración, seleccione **Archivo de configuración XML** en la lista **Tipo de configuración** y, a continuación, haga clic en **Examinar**.  
  
7.  En el cuadro de diálogo **Seleccionar ubicación del archivo de configuración** , vaya a C:\DeploymentTutorial, escriba **LoadXMLDataConfig** en el cuadro **Nombre de archivo** y, después, haga clic en **Guardar**.  
  
8.  En la página Seleccionar tipo de configuración, haga clic en **Siguiente**.  
  
9. En la página Seleccionar propiedades para la exportación, expanda LoadXMLData, Ejecutables, Cargar datos XML y Propiedades y, después, active las casillas **[XMLSource].[XMLData]** y **[XMLSource].[XMLSchemaDefinition]** .  
  
10. Haga clic en **Next**.  
  
11. En la página Finalización del asistente, escriba **Configuración de LoadXMLData** en el cuadro **Nombre de la configuración** , revise el contenido de la configuración y, a continuación, haga clic en **Finalizar**.  
  
12. En el cuadro de diálogo **Organizador de configuraciones de paquetes** , compruebe que Configuración de LoadXMLData EV y Configuración de LoadXMLData aparecen en primer y segundo lugar respectivamente y, a continuación, haga clic en **Cerrar**.  
  
## <a name="next-task-in-lesson"></a>Siguiente tarea de la lección  
[Paso 5: Prueba de los paquetes actualizados](../integration-services/lesson-1-5-testing-the-updated-packages.md)  
  
## <a name="see-also"></a>Consulte también  
[Configuraciones de paquetes](./packages/legacy-package-deployment-ssis.md)  
[Crear configuraciones de paquetes](./packages/legacy-package-deployment-ssis.md)  
[Acceso a los archivos usados por los paquetes](../integration-services/security/security-overview-integration-services.md#files)