---
description: sp_helpntgroup (Transact-SQL)
title: sp_helpntgroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpntgroup
- sp_helpntgroup_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpntgroup
ms.assetid: 02b4f7c1-480a-436c-8bae-7a2488be45d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc378f5c07c4aa24c37c0212fd9f84fb6ef39f41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210878"
---
# <a name="sp_helpntgroup-transact-sql"></a>sp_helpntgroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Presenta información acerca de grupos de Windows que tienen cuentas en la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpntgroup [ [ @ntname= ] 'name' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @ntname = ] 'name'` Es el nombre del grupo de Windows. *Name* es de **tipo sysname y su** valor predeterminado es NULL. *Name* debe ser un grupo de Windows válido con acceso a la base de datos actual. Si no se especifica *Name* , todos los grupos de Windows con acceso a la base de datos actual se incluyen en la salida.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**NTGroupName**|**sysname**|Nombre del grupo de Windows.|  
|**NTGroupId**|**smallint**|Id. del grupo.|  
|**SID**|**varbinary(85)**|Identificador de seguridad (SID) de **NTGroupName**.|  
|**HasDbAccess**|**int**|1 = el grupo de Windows tiene permiso de acceso a la base de datos.|  
  
## <a name="remarks"></a>Observaciones  
 Para ver una lista de los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] roles en la base de datos actual, use **sp_helprole**.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se presenta la lista de los grupos de Windows con acceso a la base de datos actual.  
  
```  
EXEC sp_helpntgroup;  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [sp_helprole &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_revokedbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
