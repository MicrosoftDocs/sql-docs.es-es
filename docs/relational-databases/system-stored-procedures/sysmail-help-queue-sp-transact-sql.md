---
description: sysmail_help_queue_sp (Transact-SQL)
title: sysmail_help_queue_sp (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_queue_sp
- sysmail_help_queue_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_queue_sp
ms.assetid: 94840482-112c-4654-b480-9b456c4c2bca
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6aece593fd32d5ee563c7b1fe37dc039aa3eec5c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181925"
---
# <a name="sysmail_help_queue_sp-transact-sql"></a>sysmail_help_queue_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  El Correo electrónico de base de datos tiene dos colas: la de correo y la de estado. En la cola de correo se almacenan los elementos de correo que están a la espera de ser enviados. En la de estado se almacena el estado de los elementos ya enviados. Este procedimiento almacenado permite ver el estado de las colas de correo o estado. Si no se especifica el parámetro **\@ queue_type** , el procedimiento almacenado devuelve una fila por cada una de las colas.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sysmail_help_queue_sp  [ @queue_type = ] 'queue_type'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @queue_type = ] 'queue_type'` Argumento opcional elimina los mensajes de correo electrónico del tipo especificado como *queue_type*. *queue_type* es de tipo **nvarchar (6)** y no tiene ningún valor predeterminado. Las entradas válidas son **mail** y **status**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-set"></a>Tipo de cursor  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**queue_type**|**nvarchar (6)**|Tipo de cola. Los valores posibles son **mail** y **status**.|  
|**length**|**int**|Número de elementos de correo de la cola especificada.|  
|**state**|**nvarchar (64)**|Estado del monitor. Los valores posibles son **INactivos** (la cola está inactiva), se **notifican** (se ha notificado a la cola que se ha recibido) y **RECEIVES_OCCURRING** (la cola está recibiendo).|  
|**last_empty_rowset_time**|**DATETIME**|Fecha y hora en que la cola estuvo vacía por última vez. En formato de hora militar y zona horaria GMT.|  
|**last_activated_time**|**DATETIME**|Fecha y hora en que la cola se activó por última vez. En formato de hora militar y zona horaria GMT.|  
  
## <a name="remarks"></a>Observaciones  
 Para solucionar problemas de Correo electrónico de base de datos, utilice **sysmail_help_queue_sp** para ver cuántos elementos hay en la cola, el estado de la cola y cuándo se activó por última vez.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, solo los miembros del rol fijo de servidor **sysadmin** pueden obtener acceso a este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente devuelve las colas de correo y estado.  
  
```  
EXECUTE msdb.dbo.sysmail_help_queue_sp ;  
GO  
```  
  
 Se trata de un conjunto de resultados de ejemplo cuya longitud se ha modificado.  
  
```  
queue_type length      state              last_empty_rowset_time  last_activated_time  
---------- -------- ------------------ ----------------------- -----------------------  
mail       0        RECEIVES_OCCURRING 2005-10-07 21:14:47.010 2005-10-10 20:52:51.517  
status     0        INACTIVE           2005-10-07 21:04:47.003 2005-10-10 21:04:47.003  
  
(2 row(s) affected)  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md)  
  
  
