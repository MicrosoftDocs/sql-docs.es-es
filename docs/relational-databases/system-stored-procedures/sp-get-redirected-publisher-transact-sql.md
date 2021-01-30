---
description: sp_get_redirected_publisher (Transact-SQL)
title: sp_get_redirected_publisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_get_redirected_publisher_TSQL
- sp_get_redirected_publisher
ms.assetid: d47a9ab5-f2cc-42a8-8be9-a33895ce44f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fa1a1b8f7cffe3f98435ebd48a404fb76df34c8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204761"
---
# <a name="sp_get_redirected_publisher-transact-sql"></a>sp_get_redirected_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Lo utilizan los agentes de replicación para consultar a un distribuidor a fin de determinar si se ha redirigido el publicador original.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_get_redirected_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @bypass_publisher_validation = ] [0 | 1 ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @original_publisher = ] 'original_publisher'` Nombre de la instancia de SQL Server que publicó originalmente la base de datos. *original_publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.
  
`[ @publisher_db = ] 'publisher_db'` Nombre de la base de datos que se está publicando. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @bypass_publisher_validation = ] [0 | 1 ]` Se utiliza para omitir la validación del publicador redirigido. Si es 0, se realiza la validación. Si es 1, no se realiza la validación. *bypass_publisher_validation* es de **bit** y su valor predeterminado es 0.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**redirected_publisher**|**sysname**|El nombre del publicador después de la redirección.|  
|**error_number**|**int**|El número de error del error de validación.|  
|**error_severity**|**int**|La gravedad del error de validación.|  
|**error_message**|**nvarchar(4000)**|El texto del mensaje de error de validación.|  
  
## <a name="remarks"></a>Observaciones  
 *redirected_publisher* devuelve el nombre del publicador actual. Devuelve NULL si el publicador y las bases de datos de publicación no se han redirigido mediante **sp_redirect_publisher**.  
  
 Si no se solicita la validación o si no existe ninguna entrada para el publicador y la base de datos de publicación, *error_number* y *error_severity* devuelven 0 y *error_message* devuelve NULL.  
  
 Si se solicita la validación, se llama al procedimiento almacenado de validación [sp_validate_redirected_publisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md) para comprobar que el destino de la redirección es un host adecuado para la base de datos de publicación. Si la validación se realiza correctamente, **sp_get_redirected_publisher** devuelve el nombre del publicador Redirigido, 0 para las columnas *error_number* y *error_severity* , y null en la columna *error_message* .  
  
 Si se solicita la validación y se produce un error, se devuelve el nombre del publicador redirigido junto con información sobre el error.  
  
## <a name="permissions"></a>Permisos  
 El autor de la llamada debe ser miembro del rol fijo de servidor **sysadmin** , el rol fijo de base de datos **db_owner** para la base de datos de distribución, o un miembro de una lista de acceso a la publicación para una publicación definida asociada a la base de datos del publicador.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
