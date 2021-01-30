---
description: sp_revoke_publication_access (Transact-SQL)
title: sp_revoke_publication_access (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_revoke_publication_access_TSQL
- sp_revoke_publication_access
helpviewer_keywords:
- sp_revoke_publication_access
ms.assetid: 84ed9e77-991f-4fa5-a21f-7c6bfec1b3e3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 287f696d9e68cbef8bbe180f287952206ef755bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211844"
---
# <a name="sp_revoke_publication_access-transact-sql"></a>sp_revoke_publication_access (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita el inicio de sesión de la lista de acceso de una publicación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_revoke_publication_access [ @publication = ] 'publication' , [ @login = ] 'login'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación a la que se va a obtener acceso. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @login = ] 'login'` Es el identificador de inicio de sesión. *login* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_revoke_publication_access** se utiliza en la replicación de instantáneas, transaccional y de mezcla.  
  
 se puede llamar a **sp_revoke_publication_access** de forma repetida.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_revoke_publication_access**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_grant_publication_access &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md)   
 [sp_help_publication_access &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-publication-access-transact-sql.md)   
 [Proteger el publicador](../../relational-databases/replication/security/secure-the-publisher.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
