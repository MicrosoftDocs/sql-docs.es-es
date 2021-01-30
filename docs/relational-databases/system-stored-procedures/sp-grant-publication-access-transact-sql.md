---
description: sp_grant_publication_access (Transact-SQL)
title: sp_grant_publication_access (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_grant_publication_access_TSQL
- sp_grant_publication_access
helpviewer_keywords:
- sp_grant_publication_access
ms.assetid: 17993952-def6-4a16-b1c1-323ec42967f8
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 1bea8096d3d0d57b6044edc26bb5f64572487157
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208994"
---
# <a name="sp_grant_publication_access-transact-sql"></a>sp_grant_publication_access (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega un inicio de sesión a la lista de acceso de la publicación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_grant_publication_access [ @publication = ] 'publication', [ @login = ] 'login'   
    [ , [ @reserved = ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación a la que se va a obtener acceso. **'**_Publication_*_'_* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @login = ] 'login'` Es el identificador de inicio de sesión. **'**_login_*_'_* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_grant_publication_access** se utiliza en la replicación de instantáneas, transaccional y de mezcla.  
  
 Es posible llamar repetidamente a este procedimiento almacenado.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_grant_publication_access**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_help_publication_access &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-publication-access-transact-sql.md)   
 [sp_revoke_publication_access &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md)   
 [Proteger el publicador](../../relational-databases/replication/security/secure-the-publisher.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
