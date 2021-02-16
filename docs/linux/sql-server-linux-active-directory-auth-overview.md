---
title: Autenticación de Active Directory para SQL Server en Linux
titleSuffix: SQL Server
description: En este artículo, se proporciona información general sobre la autenticación de Active Directory para SQL Server en Linux.
ms.date: 04/01/2019
author: Dylan-MSFT
ms.author: dygray
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, AAD authentication
ms.openlocfilehash: da2de7b66923ce82813fd8863556ac08b422bded
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350948"
---
# <a name="active-directory-authentication-for-sql-server-on-linux"></a>Autenticación de Active Directory para SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo, se proporciona información general sobre la autenticación de Active Directory (AD) para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Linux. La autenticación de AD también se conoce como autenticación integrada en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

## <a name="ad-authentication-overview"></a>Información general sobre la autenticación de AD

La autenticación de AD permite que los clientes unidos a un dominio en Windows o Linux puedan autenticarse en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mediante sus credenciales de dominio y el protocolo Kerberos.

La autenticación de AD tiene las siguientes ventajas sobre la autenticación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]:

- Los usuarios se autentican mediante el inicio de sesión único, sin que se les pida una contraseña.
- Al crear inicios de sesión para grupos de AD, puede administrar el acceso y los permisos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mediante pertenencias a grupos de AD.  
- Cada usuario tiene una única identidad en toda la organización, por lo que no es necesario realizar un seguimiento de qué inicio de sesión de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se corresponde con cada usuario.   
- AD le permite implementar una directiva de contraseñas centralizada en toda su organización.

## <a name="configuration-steps"></a>Pasos de configuración

Para usar la autenticación de Active Directory, necesita tener un controlador de dominio de AD (Windows) en la red.

Para obtener más información sobre cómo configurar la autenticación de AD, vea [Tutorial: usar la autenticación de Active Directory con SQL Server en Linux](sql-server-linux-active-directory-authentication.md). En la lista siguiente, se proporciona un resumen con un vínculo a cada sección en el tutorial:

1. [Unir un host de SQL Server a un dominio de Active Directory](sql-server-linux-active-directory-join-domain.md).
1. [Crear un usuario de AD para SQL Server y establecer el elemento ServicePrincipalName](sql-server-linux-active-directory-authentication.md#createuser).
1. [Configurar la tabla de claves del servicio SQL Server](sql-server-linux-active-directory-authentication.md#configurekeytab).
1. [Proteger el archivo de tabla de claves](sql-server-linux-active-directory-authentication.md#configurekeytab).
1. [Configurar SQL Server para usar el archivo de tabla de claves para la autenticación Kerberos](sql-server-linux-active-directory-authentication.md#configurekeytab).
1. [Crear inicios de sesión de SQL Server basados en AD en Transact-SQL](sql-server-linux-active-directory-authentication.md#createsqllogins).
1. [Conectarse a SQL Server mediante la autenticación de AD](sql-server-linux-active-directory-authentication.md#connect).

## <a name="known-issues"></a>Problemas conocidos

- En este momento, el único método de autenticación admitido para el punto de conexión de la creación de reflejos de bases de datos es el de “CERTIFICADO”. El método de autenticación de “WINDOWS” se habilitará en una versión futura.
- SQL Server en Linux no admite el protocolo NTLM para las conexiones remotas. La conexión local puede funcionar con NTLM.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo implementar la autenticación de Active Directory para SQL Server en Linux, vea [Tutorial: usar la autenticación de Active Directory con SQL Server en Linux](sql-server-linux-active-directory-authentication.md).
