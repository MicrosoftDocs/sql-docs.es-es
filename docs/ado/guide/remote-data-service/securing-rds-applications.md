---
description: Protección de aplicaciones de RDS
title: Proteger aplicaciones RDS | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS security [ADO]
ms.assetid: 82fb1330-d6c6-4c17-ad3e-d417ff822b25
author: rothja
ms.author: jroth
ms.openlocfilehash: d91b2ef0d4efba7453e3acedb12c8f55d528b1b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031807"
---
# <a name="securing-rds-applications"></a>Protección de aplicaciones de RDS
En este tema se proporciona información de seguridad para RDS.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="microsoft-internet-explorer-security-issues"></a>Problemas de seguridad de Microsoft Internet Explorer  
 Con nuevas mejoras de seguridad agregadas a Microsoft Internet Explorer, algunos objetos ADO y RDS están restringidos a ejecutarse solo en un entorno de modo "seguro". Para ello, es necesario tener en cuenta estos problemas, entre los que se incluyen diferentes zonas, niveles de seguridad, comportamiento restrictivo, operaciones no seguras y configuraciones de seguridad personalizadas.  
  
## <a name="security-and-your-web-server"></a>Seguridad y su servidor Web  
 Si utiliza el objeto [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) en el servidor Web de Internet, recuerde que, al hacerlo, se crea un posible riesgo de seguridad. Los usuarios externos que obtienen la información de nombre de origen de datos (DSN), ID. de usuario y contraseña válidos podrían escribir páginas para enviar cualquier consulta a ese origen de datos. Si desea un acceso más restringido a un origen de datos, una opción es anular el registro y eliminar el objeto **RDSServer. DataFactory** (msadcf.dll) y, en su lugar, usar objetos comerciales personalizados con consultas codificadas de forma rígida.  
  
 Para obtener más información sobre las implicaciones de seguridad del uso del objeto RDSServer. DataFactory, vea el boletín de seguridad de Microsoft MS99-025 en el sitio web de seguridad de Microsoft.  
  
## <a name="client-impersonation-and-security"></a>Suplantación y seguridad de cliente  
 Si la propiedad de **autenticación de contraseña** para el servidor Web de IIS está establecida en autenticación desafío/respuesta de Windows NT (para windows NT 4,0) o en la autenticación integrada de Windows (para Windows 2000), los objetos de negocio se invocan en el contexto de seguridad del cliente. Se trata de una nueva característica de RDS 1,5 que permite la suplantación de cliente a través de HTTP. Cuando se trabaja en este modo, el inicio de sesión en el servidor Web (IIS) no es anónimo, sino que usa el ID. de usuario y la contraseña en los que se ejecuta el equipo cliente. Si los DSN de ODBC están configurados para utilizar una conexión de confianza, el acceso a las bases de datos, como SQL Server, también se produce en el contexto de seguridad del cliente. Pero esto solo funciona si la base de datos está en el mismo equipo que el IIS; las credenciales del cliente no se pueden transferir a otro equipo.  
  
 Por ejemplo, un cliente, John Doe, con userid = "JohnD" y password = "Secret" inicia sesión en un equipo cliente. Ejecuta una aplicación basada en explorador que necesita tener acceso al objeto **RDSServer. DataFactory** para crear un conjunto de [registros](../../reference/ado-api/recordset-object-ado.md) ejecutando una consulta SQL en el equipo "servidor" que ejecuta IIS. Mi servidor, un sistema que ejecuta Windows NT Server 4,0, está configurado para usar la autenticación desafío/respuesta de Windows NT, el DSN de ODBC tiene seleccionada la opción "usar conexión de confianza" y el servidor también contiene el SQL Server origen de datos. Cuando se recibe una solicitud en el servidor Web, pide al cliente el identificador de usuario y la contraseña. Por lo tanto, la solicitud se registra en el servidor como procedente de "JohnD"/"Secret" en lugar de IUSER_MyServer (que es el valor predeterminado cuando la autenticación de contraseña anónima está activada). Del mismo modo, al iniciar sesión en SQL Server, se usa "JohnD"/"Secret".  
  
 Por consiguiente, el modo de autenticación desafío/respuesta de Windows NT de IIS permite crear páginas HTML sin que se pida explícitamente al usuario la información de ID. de usuario y contraseña necesaria para iniciar sesión en la base de datos. Si se usaba la autenticación básica de IIS, también sería necesario.  
  
## <a name="password-authentication"></a>Autenticación de contraseña  
 RDS puede comunicarse con un servidor Web de IIS que se ejecute en cualquiera de los tres modos de autenticación de contraseña: autenticación de desafío/respuesta de NT anónimo, básico o NT (denominada autenticación integrada de Windows en Windows 2000). Esta configuración define el modo en que un servidor Web controla el acceso a través de él, por ejemplo, requiriendo que un equipo cliente tenga privilegios de acceso explícitos en el servidor Web NT.