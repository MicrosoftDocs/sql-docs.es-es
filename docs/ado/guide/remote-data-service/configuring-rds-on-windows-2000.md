---
description: Configuración de RDS en Windows 2000
title: Configurar RDS en Windows 2000 | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS configuration [ADO], Windows 2000
ms.assetid: ef37e858-c05f-4f52-a65f-3ce6037e0d03
author: rothja
ms.author: jroth
ms.openlocfilehash: caa8fe846651f65e4316c81f29f2f18078628658
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036580"
---
# <a name="configuring-rds-on-windows-2000"></a>Configuración de RDS en Windows 2000
Si tiene dificultades para que RDS funcione correctamente después de actualizar a Windows 2000, siga estos pasos para solucionar el problema:  
  
1.  Para asegurarse de que el servicio de publicación de World Wide Web se está ejecutando en primer lugar, vaya a https://Server con Internet Explorer. Si no puede tener acceso al servidor Web de esta manera, abra un símbolo del sistema y escriba el siguiente comando: NET START W3SVC.  
  
2.  En el menú Inicio, seleccione Ejecutar. Escriba msdfmap.ini y, a continuación, haga clic en Aceptar para abrir el archivo de msdfmap.ini en el Bloc de notas. Active la sección [CONNECT DEFAULT] y, si el parámetro de acceso está establecido en NoAccess, cámbielo a READONLY.  
  
3.  Con la utilidad regedit, vaya a "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DataFactory\HandlerInfo" y asegúrese de que **HandlerRequired** está establecido en 0 y el valor **predeterminado** es "" (cadena nula).  
  
     **Nota:** Si realiza cambios en esta sección del registro, debe detener y reiniciar el servicio de publicación de World Wide Web escribiendo los siguientes comandos en un símbolo del sistema: "NET STOP W3SVC" y "NET START W3SVC".  
  
4.  Con la utilidad regedit, navegue en el registro hasta "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC\Parameters\ADCLaunch" y compruebe que hay una clave denominada **RDSServer. DataFactory**. Si no es así, créelo.  
  
5.  Con Administrador de servicios Internet, busque el sitio web predeterminado y vea las propiedades de la raíz virtual de MSADC. Inspeccione las restricciones de seguridad de directorios/dirección IP y nombre de dominio. Si está activada la casilla "acceso denegado", seleccione "concedido".  
  
 Asegúrese de intentar reiniciar el servidor si los cambios en no parecen resolver el problema al principio.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/). A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows. Migre las aplicaciones que usan RDS a un [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Consulte también  
 [Aspectos básicos de RDS](./rds-fundamentals.md)