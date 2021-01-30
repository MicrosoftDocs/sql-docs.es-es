---
description: sp_msx_set_account (Transact-SQL)
title: sp_msx_set_account (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_msx_set_account
- sp_msx_set_account_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_msx_set_account
ms.assetid: 314ec720-3a37-48f7-bb6b-8d5b894bf843
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 29a92920e81f65beabdc0837b85d19484d2e35f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192003"
---
# <a name="sp_msx_set_account-transact-sql"></a>sp_msx_set_account (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece el nombre y la contraseña de la cuenta del servidor maestro del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servidor de destino.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_msx_set_account [ @credential_name = ] 'credential_name'  | [ @credential_id = ] credential_id  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @credential_name = ] 'credential_name'` El nombre de la credencial que se va a usar para iniciar sesión en el servidor maestro. El nombre proporcionado debe ser el nombre de una credencial existente. Se debe especificar *credential_name* o *credential_id* .  
  
`[ @credential_id = ] credential_id` Identificador de la credencial que se va a usar para iniciar sesión en el servidor maestro. Debe ser el identificador de una credencial existente. Se debe especificar *credential_name* o *credential_id* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Ninguno.  
  
## <a name="remarks"></a>Observaciones  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza credenciales para almacenar la información de nombre de usuario y contraseña que utiliza un servidor de destino para iniciar sesión en un servidor maestro. Este procedimiento establece la credencial que utiliza el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de este servidor de destino para iniciar sesión en el servidor maestro.  
  
 La credencial especificada debe existir. Para obtener más información acerca de la creación de credenciales, vea [Create credential &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para **sp_msx_set_account** valor predeterminado a los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se establece este servidor de manera que utilice la credencial `MsxAccount` para iniciar sesión en el servidor maestro.  
  
```  
USE msdb ;  
GO  
  
EXECUTE dbo.sp_msx_set_account @credential_name = MsxAccount ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_msx_get_account &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-msx-get-account-transact-sql.md)  
  
  
