---
description: Requisitos del sistema para la aplicación de la libreta de direcciones
title: Requisitos del sistema para la aplicación de libreta de direcciones | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO]
- RDS scenarios [ADO]
ms.assetid: da385405-1c9a-478b-9bf6-fba70015324c
author: rothja
ms.author: jroth
ms.openlocfilehash: 0fa1eb879a536a2958184942c5d779ebb1c998b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036085"
---
# <a name="system-requirements-for-the-address-book-application"></a>Requisitos del sistema para la aplicación de la libreta de direcciones
Para configurar la aplicación de ejemplo de libreta de direcciones, debe cumplir los siguientes requisitos de software y base de datos:  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="software-requirements"></a>Requisitos de software  
 Los requisitos de software del equipo servidor para ejecutar esta aplicación web incluyen:  
  
-   Microsoft Windows NT® Server 4,0, con Service Pack 3 o posterior, o Microsoft Windows® 2000 Server.  
  
-   Microsoft Internet Information Services (IIS) versión 3,0 o posterior, con Active Server páginas.  
  
 Los requisitos de software del equipo cliente para ejecutar esta aplicación web incluyen:  
  
-   Microsoft Internet Explorer 4,0 o posterior.  
  
-   Microsoft Windows NT 4,0 Workstation o Server, Windows 2000 o Microsoft Windows 98.  
  
## <a name="database-requirements"></a>Requisitos de base de datos  
 Para usar este ejemplo, debe tener:  
  
-   Un servidor de base de datos operativo Microsoft® SQL Server versión 6,5 o posterior.  
  
-   Privilegios para crear la base de datos y rellenarla con los datos de ejemplo.  
  
-   Comprobación de los datos rellenados a través del Administrador corporativo o las utilidades ISQL (denominadas analizador de consultas en SQL Server 7,0).  
  
 Si no tiene privilegios, es posible que el administrador de la base de datos tenga que configurar el sistema y concederle permiso de acceso a la base de datos o configurar la base de datos automáticamente.  
  
## <a name="see-also"></a>Consulte también  
 [Ejecutar el script SQL de la libreta de direcciones](./running-the-address-book-sql-script.md)   
 [Objeto DataControl (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [Ejecución de la aplicación de ejemplo de la libreta de direcciones](./running-the-address-book-sample-application.md)