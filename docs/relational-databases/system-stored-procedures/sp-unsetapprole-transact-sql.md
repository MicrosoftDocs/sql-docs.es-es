---
description: sp_unsetapprole (Transact-SQL)
title: sp_unsetapprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_unsetapprole_TSQL
- sp_unsetapprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_unsetapprole
ms.assetid: 4c4033d3-1a34-4dfb-835d-e3293d1a442d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f1de3a86c7e23956d9c41b13e2f1aef9c933bda3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209658"
---
# <a name="sp_unsetapprole-transact-sql"></a>sp_unsetapprole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Desactiva un rol de aplicación y vuelve al contexto de seguridad anterior.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_unsetapprole @cookie   
```  
  
## <a name="arguments"></a>Argumentos  
 **\@ellas**  
 Especifica la cookie que se creó cuando se activó el rol de aplicación. La cookie se crea mediante [sp_setapprole &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md). **varbinary (8000)**.  
  
> [!NOTE]  
>  El parámetro **OUTPUT** de la cookie para **sp_setapprole** está documentado actualmente como **varbinary(8000)** , que es la longitud máxima correcta. Pero la implementación actual devuelve **varbinary(50)** . Las aplicaciones deben seguir reservando **varbinary (8000)** para que la aplicación siga funcionando correctamente si el tamaño de retorno de la cookie aumenta en una versión futura.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) y 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Después de activar un rol de aplicación mediante **sp_setapprole**, el rol permanece activo hasta que el usuario se desconecta del servidor o ejecuta **sp_unsetapprole**.  
  
 Para obtener información general sobre los roles de aplicación, consulte [roles de aplicación](../../relational-databases/security/authentication-access/application-roles.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al **público** y el conocimiento de la cookie guardada cuando se activó el rol de aplicación.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="activating-an-application-role-with-a-cookie-then-reverting-to-the-previous-context"></a>Activar un rol de aplicación con una cookie y volver al contexto original  
 En el siguiente ejemplo se habilita el rol de aplicación `Sales11` con la contraseña `fdsd896#gfdbfdkjgh700mM` y se crea una cookie. En el ejemplo se devuelve el nombre del usuario actual y, a continuación, se revierte al contexto original ejecutando **sp_unsetapprole**.  
  
```  
DECLARE @cookie varbinary(8000);  
EXEC sp_setapprole 'Sales11', 'fdsd896#gfdbfdkjgh700mM'  
    , @fCreateCookie = true, @cookie = @cookie OUTPUT;  
-- The application role is now active.  
SELECT USER_NAME();  
-- This will return the name of the application role, Sales11.  
EXEC sp_unsetapprole @cookie;  
-- The application role is no longer active.  
-- The original context has now been restored.  
GO  
SELECT USER_NAME();  
-- This will return the name of the original user.   
GO   
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_setapprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)  
  
  
