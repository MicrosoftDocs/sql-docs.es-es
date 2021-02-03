---
description: SETUSER (Transact-SQL)
title: SETUSER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SETUSER_TSQL
- SETUSER
dev_langs:
- TSQL
helpviewer_keywords:
- delegation [SQL Server]
- impersonation [SQL Server]
- SETUSER statement
- user impersonation [SQL Server]
ms.assetid: 7acfac5c-9ad6-4226-b874-7add36c4ea43
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 3cfa4dfcd316a91fa6224ef4ea9a8807740cca74
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189393"
---
# <a name="setuser-transact-sql"></a>SETUSER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permite a un miembro del rol fijo de servidor **sysadmin** o al propietario de una base de datos suplantar a otro usuario.  
  
> [!IMPORTANT]  
>  SETUSER se incluye únicamente por motivos de compatibilidad con versiones anteriores. Es posible que SETUSER deje de admitirse en versiones futuras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por eso, es preferible usar [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
SETUSER [ 'username' [ WITH NORESET ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **'** *username* **'**  
 Es el nombre de un usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de Windows en la base de datos actual al que se suplanta. Cuando no se especifica el parámetro *username*, se restablece la identidad original del administrador del sistema o propietario de la base de datos que suplantaba al usuario.  
  
 WITH NORESET  
 Especifica que las instrucciones SETUSER siguientes (que no especifican *username*) no deben restablecer la identidad del usuario como administrador del sistema o propietario de la base de datos.  
  
## <a name="remarks"></a>Comentarios  
 Los miembros del rol fijo de servidor **sysadmin** o el propietario de una base de datos pueden usar la identidad de otro usuario con el fin de probar los permisos de ese usuario. No basta con pertenecer al rol fijo de base de datos db_owner.  
  
 Solo se debe utilizar SETUSER con usuarios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. SETUSER no se admite con usuarios de Windows. Cuando se ha utilizado SETUSER para asumir la identidad de otro usuario, los objetos que crea el usuario que realiza la suplantación serán propiedad del usuario suplantado. Por ejemplo, si el propietario de la base de datos adopta la identidad de la usuaria **Margaret** y crea una tabla llamada **orders**, la propietaria de la tabla **orders** será **Margaret**, en lugar del administrador del sistema.  
  
 SETUSER sigue teniendo efecto hasta que se ejecute otra instrucción SETUSER o hasta que se cambie la base de datos actual con la instrucción USE.  
  
> [!NOTE]  
>  Si se utiliza SETUSER WITH NORESET, la única forma de que el administrador del sistema o el propietario de la base de datos pueda restablecer sus propios derechos es cerrar la sesión e iniciar otra.  
  
## <a name="permissions"></a>Permisos  
 Es necesario pertenecer al rol fijo de servidor **sysadmin** o ser propietario de la base de datos. No basta con pertenecer al rol fijo de base de datos **db_owner**.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente muestra cómo el propietario de la base de datos puede adoptar la identidad de otro usuario. La usuaria `mary` ha creado una tabla llamada `computer_types`. Mediante SETUSER, el propietario de la base de datos suplanta a `mary` para garantizar al usuario `joe` el acceso a la tabla `computer_types` y, a continuación, restablece su propia identidad.  
  
```sql
SETUSER 'mary';  
GO  
GRANT SELECT ON computer_types TO joe;  
GO  
--To revert to the original user  
SETUSER;  
```  
  
## <a name="see-also"></a>Consulte también  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)  
  
  
