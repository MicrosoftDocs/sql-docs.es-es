---
description: IS_MEMBER (Transact-SQL)
title: IS_MEMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_MEMBER
- IS_MEMBER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], members
- current member status
- roles [SQL Server], members
- testing member status
- members [SQL Server]
- checking member status
- IS_MEMBER function
- verifying member status
- groups [SQL Server], members
- members [SQL Server], verifying
ms.assetid: 77cb68a0-19b7-4fe1-ab17-e5587699631b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e938e2cfbe93c137ce24d77d7f942b1764f521be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179663"
---
# <a name="is_member-transact-sql"></a>IS_MEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indica si el usuario actual es miembro del grupo de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows o del rol de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificados. La función IS_MEMBER no se admite para los grupos de Azure Active Directory.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
IS_MEMBER ( { 'group' | 'role' } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **'** *group* **'**  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Nombre del grupo de Windows que se está comprobando; su formato debe ser *Dominio*\\*Grupo*. *group* es **sysname**.  
  
 **'** *role* **'**  
 Es el nombre del rol [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se va comprobar. *role* es **sysname** y puede contener los roles fijos de base de datos o los roles definidos por el usuario, pero no los roles de servidor.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
## <a name="remarks"></a>Observaciones  
 IS_MEMBER devuelve los siguientes valores.  
  
|Valor devuelto|Descripción|  
|------------------|-----------------|  
|0|El usuario actual no es miembro de *group* ni *role*.|  
|1|El usuario actual es miembro de *group* o *role*.|  
|NULL|*group* o *role* no son válidos. Cuando se consulta en un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en un inicio de sesión que usa un rol de aplicación, devuelve NULL para un grupo de Windows.|  
  
 IS_MEMBER determina la pertenencia al grupo de Windows examinando un token de acceso creado por Windows. El token de acceso no refleja los cambios en la pertenencia a grupos que se realizan después de que un usuario se conecte a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La pertenencia a un grupo de Windows no se puede consultar en un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ni en un rol de aplicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Para agregar y quitar miembros de un rol de base de datos, use [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md). Para agregar y quitar miembros de un rol de servidor, use [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 Esta función evalúa la pertenencia al rol, no el permiso subyacente. Por ejemplo, el rol fijo de base de datos **db_owner** tiene el permiso **CONTROL DATABASE**. Si el usuario tiene el permiso **CONTROL DATABASE** pero no es miembro del rol, esta función informará correctamente de que el usuario no es miembro del rol **db_owner**, aunque tenga los mismos permisos.  
  
 Los miembros del rol fijo de servidor **sysadmin** acceden a cada base de datos con el usuario **dbo**. Al comprobar los permisos para el rol fijo de servidor **sysadmin**, se comprueban los permisos para **dbo**, en lugar del inicio de sesión original. Como **dbo** no se puede agregar a un rol de base de datos y no existe en los grupos de Windows, **dbo** siempre devolverá 0 (o NULL si no existe el rol).  
  
## <a name="related-functions"></a>Funciones relacionadas  
 Para determinar si otro inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es miembro de un rol de base de datos, use [IS_ROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-rolemember-transact-sql.md). Para determinar si un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es miembro de un rol de servidor, use [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se comprueba si el usuario actual es miembro de un rol de base de datos o de un grupo de dominio de Windows.  
  
```sql  
-- Test membership in db_owner and print appropriate message.  
IF IS_MEMBER ('db_owner') = 1  
   PRINT 'Current user is a member of the db_owner role'  
ELSE IF IS_MEMBER ('db_owner') = 0  
   PRINT 'Current user is NOT a member of the db_owner role'  
ELSE IF IS_MEMBER ('db_owner') IS NULL  
   PRINT 'ERROR: Invalid group / role specified';  
GO  
  
-- Execute SELECT if user is a member of ADVWORKS\Shipping.  
IF IS_MEMBER ('ADVWORKS\Shipping') = 1  
   SELECT 'User ' + USER + ' is a member of ADVWORKS\Shipping.';   
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
