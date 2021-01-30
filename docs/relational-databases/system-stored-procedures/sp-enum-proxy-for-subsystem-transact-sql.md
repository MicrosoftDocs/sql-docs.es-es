---
description: sp_enum_proxy_for_subsystem (Transact-SQL)
title: sp_enum_proxy_for_subsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_proxy_for_subsystem_TSQL
- sp_enum_proxy_for_subsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_proxy_for_subsystems
ms.assetid: 580cc3be-1068-4a96-8d15-78ca3a5bb719
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: bac523afb9f802e3f6feb8a23c47835c82bac807
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157079"
---
# <a name="sp_enum_proxy_for_subsystem-transact-sql"></a>sp_enum_proxy_for_subsystem (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra los permisos de acceso a subsistemas concedidos a los servidores proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_enum_proxy_for_subsystem  
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @proxy_id = ] proxy_id` Número de identificación del proxy para el que se va a mostrar información. La *proxy_id* es de **tipo int** y su valor predeterminado es NULL. Se puede especificar el *identificador* o el *proxy_name* .  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy del que se va a mostrar información. La *proxy_name* es de **tipo sysname y su** valor predeterminado es NULL. Se puede especificar el *identificador* o el *proxy_name* .  
  
`[ @subsystem_id = ] subsystem_id` Número de identificación del subsistema del que se va a mostrar información. La *subsystem_id* es de **tipo int** y su valor predeterminado es NULL. Se puede especificar el *subsystem_id* o el *subsystem_name* .  
  
`[ @subsystem_name = ] 'subsystem_name'` Nombre del subsistema del que se va a mostrar información. La *subsystem_name* es de **tipo sysname y su** valor predeterminado es NULL. Se puede especificar el *subsystem_id* o el *subsystem_name* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**subsystem_id**|**int**|Número de identificación del subsistema.|  
|**subsystem_name**|**sysname**|Nombre del subsistema.|  
|**proxy_id**|**int**|Número de identificación del proxy.|  
|**proxy_name**|**sysname**|Nombre del proxy.|  
| &nbsp; | &nbsp; | &nbsp; |
  
## <a name="remarks"></a>Observaciones  
 Cuando no se proporcionan parámetros, **sp_enum_proxy_for_subsystem** muestra información acerca de todos los servidores proxy de la instancia para cada subsistema.  
  
 Cuando se proporciona un identificador de proxy o un nombre de proxy, **sp_enum_proxy_for_subsystem** muestra los subsistemas a los que tiene acceso el proxy. Cuando se proporciona un identificador de subsistema o un nombre de subsistema, **sp_enum_proxy_for_subsystem** muestra los servidores proxy que tienen acceso a ese subsistema.  
  
 Cuando se suministra información acerca del proxy y del subsistema, el conjunto de resultados devuelve una fila si el proxy especificado tiene acceso al subsistema especificado.  
  
 Este procedimiento almacenado se encuentra en **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-all-associations"></a>A. Mostrar todas las asociaciones  
 En el ejemplo siguiente se muestran todos los permisos establecidos entre los servidores proxy y los subsistemas de la instancia actual.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem ;  
GO  
```  
  
### <a name="b-determining-if-a-proxy-has-access-to-a-specific-subsystem"></a>B. Determinar si un proxy tiene acceso a un subsistema específico  
 En el ejemplo siguiente se devuelve una fila si el proxy `Catalog application proxy` tiene acceso al subsistema `ActiveScripting`. En caso contrario, se devuelve un conjunto de resultados vacío.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_proxy_for_subsystem  
    @subsystem_name = 'ActiveScripting',  
    @proxy_name = 'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_grant_proxy_to_subsystem &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
