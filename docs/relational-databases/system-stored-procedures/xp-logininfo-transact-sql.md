---
description: xp_logininfo (Transact-SQL)
title: xp_logininfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_logininfo_TSQL
- xp_logininfo
dev_langs:
- TSQL
helpviewer_keywords:
- xp_logininfo
ms.assetid: ee7162b5-e11f-4a0e-a09c-1878814dbbbd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 6df69edd25c5c2f451e8a4aa657caf99a4c8103d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99124106"
---
# <a name="xp_logininfo-transact-sql"></a>xp_logininfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre usuarios y grupos de Windows.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
xp_logininfo [ [ @acctname = ] 'account_name' ]   
     [ , [ @option = ] 'all' | 'members' ]   
     [ , [ @privilege = ] variable_name OUTPUT]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @acctname = ] 'account_name'` Es el nombre de un usuario o grupo de Windows al que se ha concedido acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *account_name* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *account_name* , se indicarán todos los grupos de Windows y los usuarios de Windows a los que se haya concedido explícitamente permiso de inicio de sesión. *account_name* debe ser completo. Por ejemplo, 'ADVWKS4\macraes' o 'BUILTIN\Administrators'.  
  
 **' All '**  |  **' Members '**  
 Especifica si se presenta información de todas las rutas de acceso a permisos de la cuenta o si se presenta información de los miembros del grupo de Windows. **\@ Option** es de tipo **VARCHAR (10)** y su valor predeterminado es NULL. A menos que se especifique **All** , solo se muestra la primera ruta de acceso de permisos.  
  
`[ @privilege = ] variable_name` Es un parámetro de salida que devuelve el nivel de privilegios de la cuenta de Windows especificada. *variable_name* es de tipo **VARCHAR (10)** y su valor predeterminado es ' no se desea '. El nivel de privilegio devuelto es **User**, **admin** o **null**.  
  
 OUTPUT  
 Cuando se especifica, coloca *variable_name* en el parámetro de salida.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**nombre de cuenta**|**sysname**|Nombre completo de la cuenta de Windows.|  
|**type**|**Char (8)**|Tipo de cuenta de Windows. Los valores válidos son **User** o **Group**.|  
|**privilegia**|**char(9)**|Privilegio de acceso para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los valores válidos son **admin**, **User** o **null**.|  
|**mapped login name**|**sysname**|En el caso de las cuentas de usuario que tienen privilegios de usuario, **nombre de inicio de sesión asignado** muestra el nombre de inicio de sesión asignado que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta usar al iniciar sesión con esta cuenta mediante las reglas asignadas con el nombre de dominio agregado antes.|  
|**permission path**|**sysname**|Pertenencia al grupo que permite que la cuenta tenga acceso.|  
  
## <a name="remarks"></a>Observaciones  
 Si se especifica *account_name* , **xp_logininfo** notifica el nivel de privilegios más alto del usuario o grupo de Windows especificado. Si un usuario de Windows tiene acceso como administrador del sistema y como usuario del dominio, se le notificará como administrador del sistema. Si el usuario es miembro de varios grupos de Windows del mismo nivel de privilegio, solo se le notificará en el grupo al que se concediera primero acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si *account_name* es un grupo o usuario de Windows válido que no está asociado a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión, se devuelve un conjunto de resultados vacío. Si no se puede identificar *account_name* como un usuario o grupo de Windows válido, se devuelve un mensaje de error.  
  
 Si se especifican *account_name* y **All** , se devuelven todas las rutas de acceso de permisos para el usuario o grupo de Windows. Si *account_name* es un miembro de varios grupos, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se devuelven varias filas a las que se les ha concedido acceso a. Las filas con privilegios de **Administrador** se devuelven antes que las filas de privilegios de **usuario** y, dentro de una fila de nivel de privilegios, se devuelven en el orden en que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se crearon los inicios de sesión correspondientes.  
  
 Si se especifican *account_name* y **miembros** , se devuelve una lista de los miembros de nivel siguiente del grupo. Si *account_name* es un grupo local, la lista puede incluir usuarios locales, usuarios del dominio y grupos. Si *account_name* es una cuenta de dominio, la lista está formada por usuarios del dominio. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe conectarse al controlador de dominio para recuperar la información de pertenencia a grupos. Si el servidor no puede contactar con el controlador de dominio, no se obtendrá ninguna información.  
  
 **xp_logininfo** solo devuelve información de Active Directory grupos globales, no de grupos universales.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** o la pertenencia al rol fijo de base de datos **Public** en la base de datos **maestra** con el permiso Execute concedido.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra información sobre el `BUILTIN\Administrators` grupo de Windows.  
  
```  
EXEC xp_logininfo 'BUILTIN\Administrators';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_denylogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_revokelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados extendidos generales &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)  
  
  
