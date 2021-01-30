---
description: sp_addlinkedsrvlogin (Transact-SQL)
title: sp_addlinkedsrvlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addlinkedsrvlogin_TSQL
- sp_addlinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlinkedsrvlogin
ms.assetid: eb69f303-1adf-4602-b6ab-f62e028ed9f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bcefbe0acef90deb488a1d95f162a4d94f60aa95
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202382"
---
# <a name="sp_addlinkedsrvlogin-transact-sql"></a>sp_addlinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea o actualiza una asignación entre un inicio de sesión en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y una cuenta de seguridad en un servidor remoto.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_addlinkedsrvlogin [ @rmtsrvname = ] 'rmtsrvname'   
     [ , [ @useself = ] { 'TRUE' | 'FALSE' | NULL } ]   
     [ , [ @locallogin = ] 'locallogin' ]   
     [ , [ @rmtuser = ] 'rmtuser' ]   
     [ , [ @rmtpassword = ] 'rmtpassword' ]   
```  
  
## <a name="arguments"></a>Argumentos  
 `[ @rmtsrvname = ] 'rmtsrvname'`  
 Es el nombre de un servidor vinculado al que se aplica la asignación de inicio de sesión. *rmtsrvname* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
 `[ @useself = ] { 'TRUE' | 'FALSE' | NULL }'`  
 Determina si se va a conectar a *rmtsrvname* mediante la suplantación de inicios de sesión locales o el envío explícito de un inicio de sesión y una contraseña. El tipo de datos es **VARCHAR (** 8 **)** y su valor predeterminado es true.  
  
 Un valor de TRUE especifica que los inicios de sesión usan sus propias credenciales para conectarse a *rmtsrvname*, con los argumentos *rmtuser* y *rmtpassword* que se omiten. FALSE especifica que los argumentos *rmtuser* y *rmtpassword* se usan para conectarse a *rmtsrvname* para el *locallogin* especificado. Si *rmtuser* y *rmtpassword* también se establecen en null, no se usa ningún inicio de sesión ni contraseña para conectarse al servidor vinculado.  
  
 `[ @locallogin = ] 'locallogin'`  
 Es un inicio de sesión en el servidor local. *locallogin* es de **tipo sysname y su** valor predeterminado es NULL. NULL especifica que esta entrada se aplica a todos los inicios de sesión locales que se conectan a *rmtsrvname*. Si no es NULL, *locallogin* puede ser un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un inicio de sesión de Windows. El inicio de sesión de Windows debe disponer de acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ya sea directamente o en calidad de miembro de un grupo de Windows al que se ha concedido acceso.  
  
 `[ @rmtuser = ] 'rmtuser'`  
 Es el inicio de sesión remoto utilizado para conectarse a *rmtsrvname* cuando @useself es false. Cuando el servidor remoto es una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que no utiliza la autenticación de Windows, *rmtuser* es un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión. *rmtuser* es de **tipo sysname y su** valor predeterminado es NULL.  
  
 `[ @rmtpassword = ] 'rmtpassword'`  
 Es la contraseña asociada a *rmtuser*. *rmtpassword* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Cuando un usuario inicia la sesión en el servidor local y ejecuta una consulta distribuida que obtiene acceso a una tabla del servidor vinculado, el servidor local debe iniciar la sesión en el servidor vinculado en nombre del usuario para obtener acceso a esa tabla. Utilice sp_addlinkedsrvlogin para especificar las credenciales de inicio de sesión que utiliza el servidor local para iniciar sesión en el servidor vinculado.  
  
> [!NOTE]  
>  Para crear los mejores planes de consulta cuando utiliza una tabla en un servidor vinculado, el procesador de consultas debe disponer de estadísticas de distribución de datos del servidor vinculado. Los usuarios con permisos limitados en las columnas de la tabla puede que no tengan permisos suficientes para obtener todas las estadísticas de utilidad, y puede que reciban un plan de consulta menos eficaz y experimenten un bajo rendimiento. Si el servidor vinculado es una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], para obtener todas las estadísticas disponibles, el usuario debe ser propietario de la tabla o miembro del rol fijo de servidor sysadmin, del rol fijo de base de datos db_owner o del rol fijo de base de datos db_ddladmin en el servidor vinculado. SQL Server 2012 SP1 modifica las restricciones de permisos para obtener estadísticas y permite a los usuarios que disponen del permiso SELECT tener acceso a las estadísticas disponibles mediante DBCC SHOW_STATISTICS. Para obtener más información, vea la sección permisos de [DBCC SHOW_STATISTICS &#40;&#41;de Transact-SQL ](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).  
  
 Al ejecutar sp_addlinkedserver, se crea automáticamente una asignación predeterminada entre todos los inicios de sesión del servidor local y los inicios de sesión remotos del servidor vinculado. La asignación predeterminada indica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza las credenciales de usuario del inicio de sesión local al conectarse al servidor vinculado en nombre del inicio de sesión. Esto es equivalente a ejecutar sp_addlinkedsrvlogin con @useself establecido en **true** para el servidor vinculado, sin especificar un nombre de usuario local. Utilice sp_addlinkedsrvlogin solo para cambiar la asignación predeterminada o para agregar asignaciones nuevas para inicios de sesión locales específicos. Para eliminar la asignación predeterminada o cualquier otra asignación, utilice sp_droplinkedsrvlogin.  
  
 En lugar de tener que usar sp_addlinkedsrvlogin para crear una asignación de inicio de sesión predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede utilizar automáticamente las credenciales de seguridad de Windows (nombre de inicio de sesión y contraseña de Windows) de un usuario que emita la consulta para conectarse a un servidor vinculado cuando se den todas estas condiciones:  
  
-   Un usuario se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el Modo de autenticación de Windows.  
  
-   La delegación de cuentas de seguridad está disponible en el cliente y en el servidor de envío.  
  
-   El proveedor admite el Modo de autenticación de Windows (por ejemplo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta en Windows).  
  
> [!NOTE]  
>  La delegación no tiene que estar habilitada en escenarios de un solo salto, pero es necesaria en escenarios de varios saltos.  
  
 Cuando el servidor vinculado ya ha realizado la autenticación utilizando las asignaciones que se definen al ejecutar sp_addlinkedsrvlogin en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los permisos para cada objeto individual de la base de datos remota los determina el servidor vinculado, no el servidor local.  
  
 sp_addlinkedsrvlogin no se puede ejecutar desde una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY LOGIN en el servidor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-connecting-all-local-logins-to-the-linked-server-by-using-their-own-user-credentials"></a>A. Conectar todos los inicios de sesión locales al servidor vinculado mediante sus propias credenciales de usuario  
 En el ejemplo siguiente se crea una asignación para asegurar que todos los inicios de sesión en el servidor local se conectan a través del servidor vinculado `Accounts` mediante sus propias credenciales de usuario.  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts';  
```  
  
 Or  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts', 'true';  
```  
  
> [!NOTE]  
>  Si hay asignaciones explícitas creadas para inicios de sesión individuales, éstas tienen prioridad sobre cualquier asignación global que exista para ese servidor vinculado.  
  
### <a name="b-connecting-a-specific-login-to-the-linked-server-by-using-different-user-credentials"></a>B. Conectar un inicio de sesión específico al servidor vinculado mediante credenciales de usuario distintas  
 En el ejemplo siguiente se crea una asignación para asegurar que solo el usuario de Windows `Domain\Mary` se conecta a través del servidor vinculado `Accounts` mediante el inicio de sesión `MaryP` y la contraseña `d89q3w4u`.  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts', 'false', 'Domain\Mary', 'MaryP', 'd89q3w4u';  
```  
  
> [!IMPORTANT]  
>  En este ejemplo no se utiliza la autenticación de Windows. Las contraseñas se transmiten sin cifrar. Las contraseñas pueden estar visibles en las definiciones de origen de datos y en los scripts que se guardan en disco, en copias de seguridad y en archivos de registro. No utilice nunca una contraseña de administrador en este tipo de conexión. Solicite a su administrador de red directrices de seguridad específicas para este entorno.  
  
## <a name="see-also"></a>Consulte también  
 [Servidores vinculados vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
