---
description: Registro de un objeto de negocios personalizado
title: Registrando un objeto comercial personalizado | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- custom business object in RDS [ADO]
- registering custom business objects in RDS [ADO]
- business objects in RDS [ADO]
ms.assetid: e9032ad8-d14c-42e3-ba13-cb5f00084a79
author: rothja
ms.author: jroth
ms.openlocfilehash: 76f3311a622abbd249aa942e1ebdbda33ad06966
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036425"
---
# <a name="registering-a-custom-business-object"></a>Registro de un objeto de negocios personalizado
Para iniciar correctamente un objeto comercial personalizado (. dll o. exe) a través del servidor Web, el identificador de programa (ProgID) del objeto comercial debe especificarse en el registro tal y como se explica en este procedimiento. Esta característica de RDS protege la seguridad del servidor Web mediante la ejecución de solo ejecutables autorizados.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
> [!NOTE]
>  En el caso de MDAC 2,0 y versiones posteriores y Windows DAC, el objeto comercial predeterminado, [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md), no se registra de forma predeterminada durante la instalación de la DAC de MDAC/Windows. Sin embargo, si **RDSServer. DataFactory** se registró como seguro para su ejecución en el equipo antes de la instalación, la entrada del registro se mantiene para la nueva instalación.  
  
### <a name="to-register-a-custom-business-object"></a>Para registrar un objeto comercial personalizado:  
  
1.  Haga clic en **Inicio** y, a continuación, en **Ejecutar**.  
  
2.  Escriba **regedit** y haga clic en **Aceptar**.  
  
3.  En el editor del registro, navegue hasta la clave del registro **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch** .  
  
4.  Seleccione la clave **ADCLaunch** y, a continuación, en el menú **edición**, seleccione **nuevo** y haga clic en **clave**.  
  
5.  Escriba el ProgID del objeto comercial personalizado y haga clic en **entrar**. Deje la entrada **valor** en blanco.