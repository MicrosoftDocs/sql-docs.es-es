---
description: sp_getqueuedrows (Transact-SQL)
title: sp_getqueuedrows (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getqueuedrows_TSQL
- sp_getqueuedrows
helpviewer_keywords:
- sp_getqueuedrows
ms.assetid: 139e834f-1988-4b4d-ac81-db1f89ea90e8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ad904065a8270266e7010ff49889d04a8625296f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183142"
---
# <a name="sp_getqueuedrows-transact-sql"></a>sp_getqueuedrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Recupera filas en el suscriptor que tienen actualizaciones pendientes en la cola. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_getqueuedrows [ @tablename = ] 'tablename'  
    [ , [ @owner = ] 'owner'  
    [ , [ @tranid = ] 'transaction_id' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @tablename = ] 'tablename'` Es el nombre de la tabla. *TableName* es de **tipo sysname** y no tiene ningún valor predeterminado. La tabla debe formar parte de una suscripción en cola.  
  
`[ @owner = ] 'owner'` Es el propietario de la suscripción. *Owner* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @tranid = ] 'transaction_id'` Permite filtrar el resultado por el identificador de la transacción. *transaction_id* es de tipo **nvarchar (70)** y su valor predeterminado es NULL. Si se especifica, se muestra el Id. de la transacción asociado con el comando en cola. Si es NULL, se muestran todos los comandos de la cola.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Muestra todas las filas que tienen actualmente al menos una transacción en cola para la tabla suscrita.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**Acción**|**nvarchar(10**|Tipo de acción que se llevará a cabo cuando tenga lugar la sincronización.<br /><br /> INS= insertar <br /><br /> DEL = eliminar<br /><br /> UPD = actualizar|  
|**Tranid**|**nvarchar (70)**|Id. de transacción con el que se ejecutó el comando.|  
|**table column1...n**||El valor de cada columna de la tabla especificada en *TableName*.|  
|**msrepl_tran_version**|**uniqueidentifier**|Esta columna se utiliza para realizar un seguimiento de los cambios de datos replicados y para llevar a cabo la detección de conflictos en el publicador. Esta columna se agrega a la tabla automáticamente.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_getqueuedrows** se utiliza en los suscriptores que participan en la actualización en cola.  
  
 **sp_getqueuedrows** encuentra filas de una tabla determinada en una base de datos de suscripciones que han participado en una actualización en cola, pero el agente de lectura de cola no las ha resuelto actualmente.  
  
## <a name="permissions"></a>Permisos  
 **sp_getqueuedrows** requiere permisos SELECT en la tabla especificada en *TableName*.  
  
## <a name="see-also"></a>Consulte también  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Detección y resolución de conflictos de actualización en cola](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
