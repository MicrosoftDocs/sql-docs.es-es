---
description: sp_help_publication_access (Transact-SQL)
title: sp_help_publication_access (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_help_publication_access
- sp_help_publication_access_TSQL
helpviewer_keywords:
- sp_help_publication_access
ms.assetid: 9408fa13-54a0-4cb1-8fb0-845e5536ef50
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b33b79c6e632eef28feaa5fa8b3f605018b09953
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199601"
---
# <a name="sp_help_publication_access-transact-sql"></a>sp_help_publication_access (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve una lista de todos los inicios de sesión concedidos para una publicación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_publication_access [ @publication = ] 'publication'  
    [ , [ @return_granted = ] 'return_granted' ]   
    [ , [ @login = ] 'login' ]  
    [ , [ @initial_list = ] initial_list ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación a la que se va a obtener acceso. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @return_granted = ] 'return_granted'` Es el identificador de inicio de sesión. *return_granted* es de **bit** y su valor predeterminado es 1. Si se especifica **0** y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se utiliza la autenticación de, se devuelven los inicios de sesión disponibles que aparecen en el publicador pero no en el distribuidor. Si se especifica **0** y se utiliza la autenticación de Windows, se devuelven los inicios de sesión que no deniegan específicamente el acceso en el publicador o el distribuidor.  
  
`[ @login = ] 'login'` Es el identificador de inicio de sesión de seguridad estándar. *login* es de **tipo sysname y su** valor predeterminado es **%** .  
  
`[ @initial_list = ] initial_list` Especifica si se devuelven todos los miembros con acceso de publicación o solo aquellos que tenían acceso antes de que se agregaran nuevos miembros a la lista. *initial_list* es de bit y su valor predeterminado es **0**.  
  
 **1** devuelve información para todos los miembros del rol fijo de servidor **sysadmin** con inicios de sesión válidos en el distribuidor que existían al crear la publicación, así como el inicio de sesión actual.  
  
 **0** devuelve información para todos los miembros del rol fijo de servidor **sysadmin** con inicios de sesión válidos en el distribuidor que existían al crear la publicación, así como a todos los usuarios de la lista de acceso a la publicación que no pertenecen al rol fijo de servidor **sysadmin** .  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**LoginName**|**nvarchar(256)**|Nombre de inicio de sesión real.|  
|**Isntname**|**int**|**0** = el inicio de sesión no es un usuario de Windows.<br /><br /> **1** = el inicio de sesión es un usuario de Windows.|  
|**Del**|**int**|**0** = el inicio de sesión no es un grupo de Windows.<br /><br /> **1** = el inicio de sesión es un grupo de Windows.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_publication_access** se utiliza en todos los tipos de replicación.  
  
 Cuando **Isntname** y **del** en el conjunto de resultados son **0**, se supone que el inicio de sesión es un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_help_publication_access**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_grant_publication_access &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md)   
 [sp_revoke_publication_access &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
