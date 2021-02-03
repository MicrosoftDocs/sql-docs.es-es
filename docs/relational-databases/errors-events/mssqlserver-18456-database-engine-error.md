---
title: MSSQLSERVER_18456 | Microsoft Docs
description: Se ha rechazado un intento de conexión debido a un error con una contraseña o un nombre de usuario incorrectos en SQL Server. Vea una explicación del error y las posibles resoluciones.
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 18456 (Database Engine error)
ms.assetid: c417631d-be1f-42e0-8844-9f92c77e11f7
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4c1e2f5aaf5198530b5d7c93ecffb088e7145b9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196514"
---
# <a name="mssqlserver_18456"></a>MSSQLSERVER_18456
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|18456|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|LOGON_FAILED|  
|Texto del mensaje|Error de inicio de sesión del usuario '%.*ls'.%.\*ls|  
  
## <a name="explanation"></a>Explicación  
Si se rechaza un intento de conexión como consecuencia de un error de autenticación porque el nombre de usuario o la contraseña no son válidos, el cliente recibe el siguiente mensaje de error:  "Error de inicio de sesión del usuario '<nombreDeUsuario>'. (Microsoft SQL Server, Error: 18456)".  
  
El cliente recibe la siguiente información adicional:  
  
"Error de inicio de sesión del usuario '<nombreDeUsuario>'. (Proveedor de datos .Net SqlClient)"  
  
-----------------------------\-  
  
"Nombre del servidor: <nombre_equipo>"  
  
"Número de error: 18456"  
  
"Gravedad: 14"  
  
"Estado: 1"  
  
"Número de línea: 65536"  
  
También podría mostrarse el error siguiente:  
  
"Mensaje 18456, nivel 14, estado 1, servidor <nombre_equipo>, línea 1"  
  
"Error de inicio de sesión del usuario '<nombreDeUsuario>'".  
  
## <a name="additional-error-information"></a>Información adicional sobre errores  
Para aumentar la seguridad, en el mensaje de error que se devuelve al cliente se oculta la naturaleza del error de autenticación. Con todo, en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el error correspondiente contiene el estado del error que indica la condición del error de autenticación Compare el estado del error en la lista siguiente para determinar la causa del error de inicio de sesión.  
  
|State|Descripción|  
|---------|---------------|  
|1|La información sobre el error no está disponible. Este estado significa normalmente que no se tiene el permiso necesario para recibir los detalles del error. Póngase en contacto con su administrador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para obtener más información.|  
|2|El identificador de usuario no es válido.|  
|5|El identificador de usuario no es válido.|  
|6|Se ha intentado usar un nombre de inicio de sesión de Windows con la autenticación de SQL Server.|  
|7|El inicio de sesión está deshabilitado y la contraseña no es correcta.|  
|8|La contraseña no es correcta.|  
|9|La contraseña no es válida.|  
|11|El inicio de sesión es válido, pero se ha producido un error de acceso al servidor. Una causa posible de este error se produce cuando el usuario de Windows tiene acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como miembro del grupo de administradores locales, pero Windows no proporciona las credenciales de administrador. Para conectarse, inicie el programa de conexión mediante la opción **Ejecutar como administrador** y, a continuación, agregue el usuario de Windows a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como inicio de sesión específico.|  
|12|El inicio de sesión es válido, pero se ha producido un error de acceso al servidor.|  
|18|Se debe cambiar la contraseña.|  
|38, 46|No se pudo encontrar la base de datos solicitada por el usuario.|
|58| Cuando SQL Server está establecido para usar solo la autenticación de Windows y un cliente intenta iniciar sesión con la autenticación de SQL. Otro motivo es que los SID no coincidan.|
|102 - 111|Error de AAD.|
|122 - 124|Error debido a que el nombre de usuario o la contraseña están vacíos.|
|126|La base de datos solicitada por el usuario no existe.|
|132 - 133|Error de AAD.|
  
Hay otros estados de error e indican un error de procesamiento interno inesperado.  
  
**Una causa posible inusual adicional**  
  
El motivo del error: **se ha producido un error al intentar iniciar sesión mediante autenticación de SQL. El servidor está configurado solo para la autenticación de Windows.** Puede devolverse en las siguientes situaciones.  
  
-   Cuando el servidor está configurado para la autenticación de modo mixto y una conexión ODBC utiliza el protocolo TCP, pero esta no especifica explícitamente que se debe utilizar una conexión de confianza.  
  
-   Cuando el servidor está configurado para la autenticación de modo mixto, una conexión ODBC utiliza canalizaciones con nombre, las credenciales utilizadas por el cliente para abrir la canalización con nombre se usan para suplantar automáticamente al usuario y la conexión no especifica de forma explícita que se debe utilizar una conexión de confianza.  
  
Para solucionar este problema, incluya **TRUSTED_CONNECTION = TRUE** en la cadena de conexión.  
  
## <a name="examples"></a>Ejemplos  
En este ejemplo, el estado de error de autenticación es 8. Esto indica que la contraseña no es correcta.  
  
|Date|Source|Message|  
|--------|----------|-----------|  
|2007-12-05 20:12:56.34|Inicio de sesión|Error: 18456, Gravedad: 14, Estado: 8.|  
|2007-12-05 20:12:56.34|Inicio de sesión|Error de inicio de sesión del usuario '<nombreDeUsuario>' [CLIENT: <ip address>]|  
  
> [!NOTE]  
> Cuando se instala [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el modo de autenticación de Windows y posteriormente se cambia al modo de autenticación de Windows y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el inicio de sesión **sa** está deshabilitado inicialmente. Esto provoca el estado de error 7: "Error de inicio de sesión del usuario 'sa'". Para habilitar el inicio de sesión **sa**, vea [Cambiar el modo de autenticación del servidor](~/database-engine/configure-windows/change-server-authentication-mode.md).  
  
## <a name="user-action"></a>Acción del usuario  
Si intenta establecer conexión usando la Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], compruebe que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está configurado en modo de autenticación mixto. Para obtener más información sobre cómo cambiar el modo de autenticación de SQL Server, revise [Cambio del modo de autenticación del servidor](../../database-engine/configure-windows/change-server-authentication-mode.md).
  
Si intenta establecer conexión usando la Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], compruebe que el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existe y que lo ha escrito correctamente.  
  
Si intenta establecer conexión usando la Autenticación de Windows, compruebe que está registrado correctamente en el dominio correcto.  
  
Si el estado de su error es 1, póngase en contacto con su administrador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Si intenta conectarse con sus credenciales de administrador, inicie la aplicación mediante la opción **Ejecutar como administrador**. Cuando se haya conectado, agregue el usuario de Windows como inicio de sesión individual.  
  
Si el [!INCLUDE[ssDE](../../includes/ssde-md.md)] admite bases de datos independientes, confirme que el inicio de sesión no se eliminó tras la migración a un usuario de base de datos independiente.  
  
Al conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de forma local, las conexiones de los servicios que se ejecutan en **NT AUTHORITY\NETWORK SERVICE** deben autenticarse con el nombre de dominio completo del equipo. Para obtener más información, consulte: [Procedimientos: Usar la cuenta de servicio de red para obtener acceso a recursos en ASP.NET](/previous-versions/msp-n-p/ff647402(v=pandp.10))