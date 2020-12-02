---
description: 'Lección 1-1: Crear carpetas de trabajo y variables de entorno'
title: 'Paso 1: Crear carpetas de trabajo y variables de entorno | Microsoft Docs'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 45091ba2-ea3d-4399-9814-489d812b42cc
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c91be2fffde29a362dd73da41159b4a4658bce3e
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449753"
---
# <a name="lesson-1-1---creating-working-folders-and-environment-variables"></a>Lección 1-1: Crear carpetas de trabajo y variables de entorno

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


En esta tarea, creará la carpeta de trabajo (C:\DeploymentTutorial) y las nuevas variables de entorno del sistema (`DataTransfer` y `LoadXMLData`) que usará en posteriores tareas del tutorial.  
  
La carpeta de trabajo está en la raíz de la unidad C. Si debe usar otra unidad o ubicación, puede hacerlo. No obstante, deberá anotar esta ubicación y usarla siempre que el tutorial haga referencia a la ubicación de la carpeta de trabajo DeploymentTutorial.  
  
En una lección posterior implementará paquetes que están guardados en el sistema de archivos en la tabla sysssispackages de la base de datos msdb de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Lo ideal es que implemente los paquetes de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] en otro equipo. Si no es posible, puede aprender a hacerlo en este tutorial si implementa los paquetes en una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que se encuentre en el equipo local. Las variables de entorno que se utilizan en el equipo local y de destino tienen los mismos nombres de variable, pero diferentes valores almacenados. Por ejemplo, en el equipo local, el valor de la variable de entorno `DataTransfer` hace referencia a la carpeta C:\DeploymentTutorial, mientras que en el equipo de destino la variable de entorno `DataTransfer` hace referencia a la carpeta C:\DeploymentTutorialInstall.  
  
Si planea realizar una implementación en el equipo local, solamente necesita crear un conjunto de variables de entorno; no obstante, deberá actualizar el valor de las variables de entorno en un valor apropiado antes de realizar la implementación local.  
  
Si planea implementar los paquetes en otro equipo, debe crear dos conjuntos de variables de entorno: un conjunto para el equipo local y otro para el equipo de destino. Ahora solamente puede crear las variables para el equipo de origen y, más tarde, crear las variables para el equipo de destino, pero debe tener la carpeta y las variables de entorno disponibles en el equipo de destino antes de poder instalar los paquetes en ese equipo.  
  
### <a name="to-create-the-local-working-folder"></a>Para crear la carpeta de trabajo local  
  
1.  Haga clic con el botón secundario en el menú Inicio y haga clic en Explorar.  
  
2.  Haga clic en **Disco local (C:)**.  
  
3.  En el menú **Archivo**, seleccione **Nuevo** y, a continuación, haga clic en **Carpeta**.  
  
4.  Cambie el nombre de la nueva carpeta a **DeploymentTutorial**.  
  
### <a name="to-create-local-environment-variables"></a>Para crear variables del entorno local  
  
1.  En el menú **Inicio** , haga clic en **Panel de control**.  
  
2.  En el Panel de control, haga doble clic en **Sistema**.  
  
3.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en la pestaña **Opciones avanzadas** y, a continuación, haga clic en **Variables de entorno**.  
  
4.  En el cuadro de diálogo **Variables de entorno** , en el marco **Variables del sistema** , haga clic en **Nueva**.  
  
5.  En el cuadro de diálogo **Nueva variable del sistema** , escriba **DataTransfer** en el cuadro **Nombre de variable** y **C:\DeploymentTutorial\datatransferconfig.dtsconfig** en el cuadro **Valor de variable** .  
  
6.  Haga clic en **Aceptar**.  
  
7.  Haga clic en **Nueva** otra vez y escriba **LoadXMLData** en el cuadro **Nombre de variable** y **C:\DeploymentTutorial\loadxmldataconfig.dtsconfig** en el cuadro **Valor de variable** .  
  
8.  Haga clic en **Aceptar** para salir del cuadro de diálogo **Variables de entorno** .  
  
9. Haga clic en **Aceptar** para salir del cuadro de diálogo **Propiedades del sistema** .\  
  
10. Opcionalmente, reinicie el equipo. Si no reinicia el equipo, el nombre de la nueva variable no se mostrará en el Asistente para la configuración de paquetes, pero podrá usarla.  
  
### <a name="to-create-destination-environment-variables"></a>Para crear variables del entorno de destino  
  
1.  En el menú **Inicio** , haga clic en **Panel de control**.  
  
2.  En el Panel de control, haga doble clic en **Sistema**.  
  
3.  En el cuadro de diálogo **Propiedades del sistema** , haga clic en la pestaña **Opciones avanzadas** y, a continuación, haga clic en **Variables de entorno**.  
  
4.  En el cuadro de diálogo **Variables de entorno** , en el marco **Variables del sistema** , haga clic en **Nueva**.  
  
5.  En el cuadro de diálogo **Nueva variable del sistema** , escriba **DataTransfer** en el cuadro **Nombre de variable** y **C:\DeploymentTutorialInstall\datatransferconfig.dtsconfig** en el cuadro **Valor de variable** .  
  
6.  Haga clic en **Aceptar**.  
  
7.  Haga clic en **Nueva** otra vez y escriba **LoadXMLData** en el cuadro **Nombre de variable** y **C:\DeploymentTutorialInstall\loadxmldataconfig.dtsconfig** en el cuadro **Valor de variable** .  
  
8.  Haga clic en **Aceptar** para salir del cuadro de diálogo **Variables de entorno** .  
  
9. Haga clic en **Aceptar** para salir del cuadro de diálogo **Propiedades del sistema** .\  
  
10. Opcionalmente, reinicie el equipo.  
  
## <a name="next-task-in-lesson"></a>Siguiente tarea de la lección  
[Paso 2: Creación del proyecto de implementación](../integration-services/lesson-1-2-creating-the-deployment-project.md)  
  
  
  
