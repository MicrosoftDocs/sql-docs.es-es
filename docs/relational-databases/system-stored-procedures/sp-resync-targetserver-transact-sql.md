---
description: sp_resync_targetserver (Transact-SQL)
title: sp_resync_targetserver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_resync_targetserver
- sp_resync_targetserver_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_resync_targetserver
ms.assetid: 40e44df7-d3e3-44ee-b149-08aba629a21f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3bd9b8b4b73cf79fc1adc9dcff8fd4754c944e35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194367"
---
# <a name="sp_resync_targetserver-transact-sql"></a>sp_resync_targetserver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Vuelve a sincronizar todos los trabajos multiservidor del servidor de destino especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_resync_targetserver  
     [ @server_name = ] 'server'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @server_name = ] 'server'` Nombre del servidor que se va a volver a sincronizar. *server* es de tipo **sysname** y no tiene ningún valor predeterminado. Si se especifica **All** , se vuelven a sincronizar todos los servidores de destino.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Informa del resultado de **sp_post_msx_operation** acciones.  
  
## <a name="remarks"></a>Observaciones  
 **sp_resync_targetserver** elimina el conjunto actual de instrucciones para el servidor de destino y envía un nuevo conjunto para que lo descargue el servidor de destino. El nuevo conjunto está compuesto de la instrucción que va a eliminar todos los trabajos multiservidor, seguida de una inserción por cada trabajo que haya actualmente en el servidor de destino.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento corresponden de forma predeterminada a los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se vuelve a sincronizar el servidor de destino `SEATTLE1`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_resync_targetserver  
    N'SEATTLE1' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_help_downloadlist &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-downloadlist-transact-sql.md)   
 [sp_post_msx_operation &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-post-msx-operation-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
