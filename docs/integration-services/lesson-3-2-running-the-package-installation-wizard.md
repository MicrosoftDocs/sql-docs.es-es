---
description: 'Lección 3-2: Ejecutar el Asistente para instalar paquetes'
title: 'Paso 2: Ejecutar el Asistente para instalar paquetes | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f91fbb89-4626-4c47-b96d-56052dc45861
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 66895007a304ee10878b54f1761b5e72792e1d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354940"
---
# <a name="lesson-3-2---running-the-package-installation-wizard"></a>Lección 3-2: Ejecutar el Asistente para instalar paquetes

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En esta tarea, ejecutará el Asistente para la instalación de paquetes para implementar los paquetes del proyecto Deployment Tutorial en una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Solamente los paquetes se pueden instalar en la tabla sysssispackages de la base de datos msdb de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , los archivos compatibles que incluya el paquete de implementación se implementarán en el sistema de archivos.  
  
El Asistente para la instalación de paquetes le guiará por los pasos para instalar y configurar los paquetes. Instalará los paquetes en una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en el equipo de destino (el equipo en el que copió el paquete de implementación). También creará una carpeta, C:\DeploymentTutorialInstall, en la que el asistente instalará los archivos no empaquetados.  
  
En una lección anterior, modificó los paquetes del tutorial para utilizar configuraciones. Con el Asistente para la instalación de paquetes, editará estas configuraciones para permitir que los paquetes se ejecuten correctamente en el entorno de la instalación.  
  
### <a name="to-install-the-packages"></a>Para instalar los paquetes  
  
1.  En el equipo de destino, busque el paquete de implementación.  
  
    Si ha usado el valor predeterminado (bin\Deployment) como ubicación de la utilidad de implementación, el paquete de implementación está en la carpeta Deployment dentro del proyecto Deployment Tutorial.  
  
2.  En la carpeta Deployment, haga doble clic en el archivo de manifiesto, Deployment Tutorial.SSISDeploymentManifest.  
  
3.  En la página principal del Asistente para instalar paquetes, haga clic en **Siguiente**.  
  
4.  En la página Implementar paquetes SSIS, seleccione la opción **Implementación en SQL Server** , active la casilla **Validar los paquetes después de la instalación** y luego haga clic en **Siguiente**.  
  
5.  En la página Especificar el SQL Server de destino, especifique **(local**) en el cuadro **Nombre del servidor** .  
  
6.  Si la instancia de SQL Server admite la autenticación de Windows, seleccione **Usar autenticación de Windows**; en caso contrario, seleccione **Usar autenticación de SQL Server** y proporcione un nombre de usuario y una contraseña.  
  
7.  Compruebe que la casilla **Basar el cifrado en el almacenamiento del servidor** está desactivada.  
  
8.  Haga clic en **Siguiente**.  
  
9. En la página Seleccionar la carpeta de instalación, haga clic en **Examinar**.  
  
10. En el cuadro de diálogo **Buscar carpeta**, expanda **Mi PC** y, a continuación, haga clic en **Disco local (C:)**.  
  
11. Haga clic en **Crear nueva carpeta** y reemplace el nombre predeterminado de la nueva carpeta, **Nueva carpeta**, por **DeploymentTutorialInstall**.  
  
    > [!IMPORTANT]  
    > En el valor de las variables de entorno que utilizan las configuraciones se hace referencia a este nombre. El nombre de la carpeta y la referencia deben coincidir o el paquete no se ejecutará.  
  
12. Haga clic en **Aceptar**.  
  
13. En la página Seleccionar la carpeta de instalación, compruebe que el cuadro Carpeta contiene **C:\DeploymentTutorialInstall** y luego haga clic en **Siguiente**.  
  
14. En la página Confirmar la instalación, haga clic en **Siguiente**.  
  
    El asistente instala los paquetes. Una vez finalizada la instalación, se abre la página Configurar paquetes.  
  
15. En la página Configurar paquetes, compruebe que el cuadro **Archivo de configuración** contiene datatransferconfig.dtsconfig y loadxmldataconfig.dtsconfig.  
  
16. En la lista **Archivo de configuración** , haga clic en **datatransferconfig.dtsconfig**, expanda Propiedad en la columna **Ruta de acceso** del cuadro **Configuraciones** y actualice la columna **Valor** con los siguientes valores:  
  
    |Propiedad|Value|Valor actualizado|  
    |------------|---------|-----------------|  
    |\Package.Connections[Deployment Tutorial Log].Properties[ConnectionString]|C:\Archivos de programa\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Completed Packages\Deployment Tutorial Log|C:\DeploymentTutorialInstall\Deployment Tutorial Log|  
    |\Package.Connections[NewCustomers].Properties[ConnectionString]|C:\Archivos de programa\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\NewCustomers.txt|C:\DeploymentTutorialInstall\NewCustomers.txt|  
  
17. En la lista **Archivo de configuración** , haga clic en loadxmldataconfig.dtsconfig, expanda Propiedad en la columna **Ruta de acceso** del cuadro **Configuraciones** y actualice la columna **Valor** con los siguientes valores:  
  
    |Propiedad|Value|Valor actualizado|  
    |------------|---------|-----------------|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLData]]|C:\Archivos de programa\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xml|C:\DeploymentTutorialInstall\orders.xml|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLSchemaDefinition]]|C:\Archivos de programa\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xsd|C:\DeploymentTutorialInstall\orders.xsd|  
  
18. En la página Validación de paquete, vea el resultado de la validación de cada paquete instalado y, después, haga clic en **Siguiente**.  
  
    Puesto que los valores de las variables de entorno en el equipo de destino son distintos de los valores de las variables de entorno en el equipo de desarrollo, en la página de validación de paquetes aparecen varias advertencias. Debe esperar cuatro advertencias:  
  
    -   El archivo de configuración: "C:\DeploymentTutorial\DataTransferConfig.dtsConfig" no es válido. Compruebe el nombre del archivo de configuración.  
  
    -   Error al cargar al menos una de las entradas de configuración en el paquete. Compruebe las entradas de configuración y las advertencias anteriores para ver una descripción de los errores de configuración.  
  
    -   El archivo de configuración: "C:\DeploymentTutorial\LoadXMLDataConfig.dtsConfig” no es válido. Compruebe el nombre del archivo de configuración.  
  
    -   Error al cargar al menos una de las entradas de configuración en el paquete. Compruebe las entradas de configuración y las advertencias anteriores para ver una descripción de los errores de configuración.  
  
    Estas advertencias no afectan a la instalación de los paquetes.  
  
    Si no ha seleccionado la opción **Validar los paquetes después de la instalación** en la página Implementar paquetes SSIS, la página Validación de paquete no se abre y el asistente no muestra ninguna información posterior a la instalación sobre la validación.  
  
19. En la página Salir del Asistente para instalar paquetes, lea el resumen de la instalación y luego haga clic en **Finalizar**.  
  
    > [!NOTE]  
    > Se crea un archivo de registro temporal para utilizarlo en la validación de paquetes. Este archivo no se usa cuando se ejecuta el paquete.  
  
## <a name="next-task-in-lesson"></a>Siguiente tarea de la lección  
[Paso 3: Prueba de los paquetes implementados](../integration-services/lesson-3-3-testing-the-deployed-packages.md)  
  
## <a name="see-also"></a>Consulte también  
[Servicio Integration Services &#40;servicio SSIS&#41;](../integration-services/service/integration-services-service-ssis-service.md)  
