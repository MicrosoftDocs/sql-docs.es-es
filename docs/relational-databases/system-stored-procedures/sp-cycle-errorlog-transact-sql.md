---
description: sp_cycle_errorlog (Transact-SQL)
title: sp_cycle_errorlog (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cycle_errorlog_TSQL
- sp_cycle_errorlog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cycle_errorlog
ms.assetid: 61a12cbf-78a3-4052-8604-3b29d07573fd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b75b9e0c056a86a8ad67d1a739214a46f88533f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203172"
---
# <a name="sp_cycle_errorlog-transact-sql"></a>sp_cycle_errorlog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cierra el archivo del registro de errores actual y continúa el ciclo de números de extensión de registro de errores, como ocurre al reiniciar el servidor. El nuevo registro de errores contiene información de versión y de copyright, y una línea que indica que se ha creado un nuevo registro.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_cycle_errorlog  
```  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Cada vez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se inicia, se cambia el nombre del registro de errores actual a **ErrorLog. 1**; **ErrorLog. 1** se convierte en **ErrorLog. 2**, **ErrorLog. 2** se convierte en **ErrorLog. 3**, etc. **sp_cycle_errorlog** permite recorrer los archivos de registro de errores sin tener que detener e iniciar el servidor.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para **sp_cycle_errorlog** están restringidos a los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se recorre el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
EXEC sp_cycle_errorlog ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_cycle_agent_errorlog &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-cycle-agent-errorlog-transact-sql.md)  
  
  
