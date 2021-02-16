---
description: Actualizar paquetes de Integration Services mediante el Asistente para actualizar paquetes SSIS
title: Actualizar paquetes de Integration Services mediante el asistente para actualizar paquetes SSIS | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services packages, upgrading
- upgrading Integration Services packages
ms.assetid: 9359275a-48f5-4d1e-8ae7-e797759e3ccf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4c82e88ea4ec23db6c1e1c3e21de3551a2283150
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352033"
---
# <a name="upgrade-integration-services-packages-using-the-ssis-package-upgrade-wizard"></a>Actualizar paquetes de Integration Services mediante el Asistente para actualizar paquetes SSIS

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Puede actualizar los paquetes que se crearon en versiones anteriores de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] al formato de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] utiliza. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona el Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] para ayudar en este proceso. Como puede configurar el asistente para realizar una copia de seguridad de los paquetes originales, puede continuar utilizando los paquetes originales si experimenta dificultades en la actualización.  
  
 El Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] se instala cuando se instala [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
> [!NOTE]  
>  El Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] está disponible en las ediciones Standard, Enterprise y Developer de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Para obtener más información sobre cómo actualizar paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vea [Actualizar paquetes de Integration Services](../../integration-services/install-windows/upgrade-integration-services-packages.md).  
  
 El resto de este tema describe cómo ejecutar el asistente y hacer una copia de seguridad de los paquetes originales.  
  
## <a name="running-the-ssis-package-upgrade-wizard"></a>Ejecutar el Asistente para actualización del paquete SSIS  
 Puede ejecutar el Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] desde [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]o en el símbolo del sistema.  
  
#### <a name="to-run-the-wizard-from-sql-server-data-tools"></a>Para ejecutar el asistente desde las herramientas de datos de SQL Server  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], cree o abra un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
2.  En el Explorador de soluciones, haga clic con el botón derecho en el nodo **Paquetes SSIS** y, después, haga clic en **Actualizar todos los paquetes** para actualizar todos los paquetes que estén debajo de este nodo.  
  
    > [!NOTE]  
    >  Al abrir un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contenga paquetes de [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o posterior, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] abre automáticamente el Asistente para actualizar paquetes [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
#### <a name="to-run-the-wizard-from-sql-server-management-studio"></a>Para ejecutar el asistente desde SQL Server Management Studio  
  
-   En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], conéctese a [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], expanda el nodo **Paquetes almacenados** , haga clic con el botón derecho en el nodo **MSDB** o en **Sistema de archivos** y, después, haga clic en **Actualizar paquetes**.  
  
#### <a name="to-run-the-wizard-at-the-command-prompt"></a>Para ejecutar el asistente en el símbolo del sistema  
  
-   En el símbolo del sistema, ejecute el archivo SSISUpgrade.exe desde la carpeta **C:\Archivos de programa\Microsoft SQL Server\130\DTS\Binn** .  
  
## <a name="backing-up-the-original-packages"></a>Hacer una copia de seguridad de los paquetes originales  
 Para hacer una copia de seguridad de los paquetes originales, tanto estos como los paquetes actualizados deben estar almacenados en la misma carpeta del sistema de archivos. En función de cómo ejecute el asistente, esta ubicación de almacenamiento podría seleccionarse automáticamente.  
  
-   Al ejecutar el Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] desde [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], el asistente almacena automáticamente tanto los paquetes originales como los actualizados en la misma carpeta del sistema de archivos.  
  
-   Al ejecutar el Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o en el símbolo del sistema, puede especificar ubicaciones de almacenamiento diferentes para los paquetes originales y para los actualizados. Para hacer una copia de seguridad de los paquetes originales, asegúrese de especificar que los paquetes originales y los actualizados estén almacenados en la misma carpeta del sistema de archivos. Si especifica cualquier otra opción de almacenamiento, el asistente no podrá hacer una copia de seguridad de los paquetes originales.  
  
 Cuando el asistente hace una copia de seguridad de los paquetes originales, almacena una copia de los paquetes originales en una carpeta **SSISBackupFolder** . El asistente crea esta carpeta **SSISBackupFolder** como una subcarpeta de la carpeta que contiene los paquetes originales y los actualizados.  
  
#### <a name="to-back-up-the-original-packages-in-sql-server-management-studio-or-at-the-command-prompt"></a>Para hacer una copia de seguridad de los paquetes originales en SQL Server Management Studio o en el símbolo del sistema  
  
1.  Guarde los paquetes originales en una ubicación del sistema de archivos.  
  
    > [!NOTE]  
    >  La opción de copia de seguridad en el asistente solo funciona con los paquetes que han sido almacenados en el sistema de archivos.  
  
2.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o en el símbolo del sistema, ejecute el Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
3.  En la página **Seleccionar ubicación de origen** del asistente, establezca la propiedad **Origen del paquete** en **Sistema de archivos**.  
  
4.  En la página **Seleccionar ubicación de destino** del asistente, seleccione **Guardar en ubicación de origen** para guardar los paquetes actualizados en la misma ubicación que los originales.  
  
    > [!NOTE]  
    >  La opción de copia de seguridad en el asistente solo está disponible cuando los paquetes actualizados están almacenados en la misma carpeta que los originales.  
  
5.  En la página **Seleccionar opciones de administración de paquetes** del asistente, seleccione la opción **Realizar copia de seguridad de paquetes originales**.  
  
#### <a name="to-back-up-the-original-packages-in-sql-server-data-tools"></a>Para hacer una copia de seguridad de los paquetes originales en las herramientas de datos de SQL Server  
  
1.  Guarde los paquetes originales en una ubicación del sistema de archivos.  
  
2.  En la página **Seleccionar opciones de administración de paquetes** del asistente, seleccione la opción **Realizar copia de seguridad de paquetes originales**.  
  
    > [!WARNING]  
    >  La opción **Realizar copia de seguridad de paquetes originales** no se muestra cuando se abre un proyecto de [!INCLUDE[ssISversion10](../../includes/ssisversion10-md.md)] o posterior en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], que inicia el asistente automáticamente.  
  
3.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ejecute el Asistente para actualización del paquete [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
  
