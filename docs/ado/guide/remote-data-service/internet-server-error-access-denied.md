---
description: 'Error del servidor de Internet: Acceso denegado'
title: 'Error de servidor de Internet: acceso denegado | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- access denied error in RDS [ADO]
ms.assetid: e5b43cfa-da8d-430d-a2ab-5443dda47a16
author: rothja
ms.author: jroth
ms.openlocfilehash: f0fa2a2e3ee7d28bab239980710440cbd375d52a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036485"
---
# <a name="internet-server-error-access-denied"></a>Error del servidor de Internet: Acceso denegado
Si recibe este error, normalmente significa que Microsoft Internet Information Services (IIS) devolvió el siguiente estado:  
  
 HTTP_STATUS_DENIED 401  
  
 Asegúrese de que los directorios a los que tiene acceso IIS tienen los permisos adecuados. RDS puede comunicarse con un servidor Web de IIS que se ejecute en cualquiera de los tres modos de autenticación de contraseñas: desafío/respuesta de NT anónimo, básico o NT (denominado autenticación integrada de Windows en Windows 2000). Además, el servidor Web debe tener permisos para el equipo de origen de datos si se trata de un equipo con Windows NT/Windows 2000.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Consulte también  
 [Aspectos básicos de RDS](./rds-fundamentals.md)