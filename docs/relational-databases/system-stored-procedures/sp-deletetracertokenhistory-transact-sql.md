---
description: sp_deletetracertokenhistory (Transact-SQL)
title: sp_deletetracertokenhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_deletetracertokenhistory
- sp_deletetracertokenhistory_TSQL
helpviewer_keywords:
- sp_deletetracertokenhistory
ms.assetid: 9ae1be14-0d2f-40b1-9d6e-22d79726abf4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5bc5a6d5191f89d8b31b48939c83674bf0302e15
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178154"
---
# <a name="sp_deletetracertokenhistory-transact-sql"></a>sp_deletetracertokenhistory (Transact-SQL)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

Quita los registros del token de seguimiento de la [MStracer_tokens &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) y MStracer_history &#40;las tablas del sistema [transact-SQL&#41;](../../relational-databases/system-tables/mstracer-history-transact-sql.md) . Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación o en el distribuidor de la base de datos de distribución.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```
sp_deletetracertokenhistory [ @publication = ] 'publication'
    [ , [ @tracer_id = ] tracer_id ]
    [ , [ @cutoff_date = ] cutoff_date ]
    [ , [ @publisher = ] 'publisher' ]
    [ , [ @publisher_db = ] 'publisher_db' ]
```

## <a name="arguments"></a>Argumentos

`@publication= 'publication'`  
Es el nombre de la publicación en la que se ha insertado el token de seguimiento. El tipo de datos es **sysname**. Este parámetro es obligatorio.

`[ @tracer_id= ] tracer_id`  
Es el identificador del token de seguimiento que se va a eliminar. El tipo de datos es **int**. El valor predeterminado es *null*. Si *es null*, se eliminan todos los tokens de seguimiento que pertenecen a la publicación.

`[ @cutoff_date= ] cutoff_date`  
Se eliminan los testigos de seguimiento insertados en la publicación antes de esta fecha. El tipo de datos es **DateTime**. El valor predeterminado es *null*.

`[ @publisher= ] 'publisher'`  
Es el nombre del publicador. El tipo de datos es **sysname**. El valor predeterminado es *null*.

> [!NOTE]
> Este parámetro solo debe especificarse para los publicadores que no sean de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o cuando se ejecute el procedimiento almacenado desde el distribuidor.

`[ @publisher_db= ] 'publisher_db'`  
Es el nombre de la base de datos de publicación. El tipo de datos es **sysname**. El valor predeterminado es NULL. Si el procedimiento almacenado se ejecuta en el publicador, se omite este parámetro.

> [!NOTE]
> Este parámetro debe especificarse al ejecutar el procedimiento almacenado desde el distribuidor.

## <a name="return-code-values"></a>Valores de código de retorno

**0** (correcto) o **1** (error)

## <a name="remarks"></a>Observaciones

**sp_deletetracertokenhistory** se utiliza en la replicación transaccional.  

Se produce un error si especifica los dos parámetros *tracer_id* y *cutoff_date*.

Si no ejecuta **sp_deletetracertokenhistory** para eliminar los metadatos del token de seguimiento, la información se eliminará cuando se produzca la limpieza de historial programado con regularidad.

Los identificadores de los testigos de seguimiento se pueden determinar mediante la ejecución de [sp_helptracertokens &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) o mediante la consulta del MStracer_tokens &#40;tabla del sistema [de transact-SQL&#41;](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) .

## <a name="permissions"></a>Permisos

Solo el personal siguiente tiene la autoridad para ejecutar **sp_deletetracertokenhistory**:

- Miembros de los roles **replmonitor** , en la base de datos de distribución
- Miembros del rol fijo de servidor **sysadmin** .
- Miembros del rol fijo de base de datos **db_owner** en la base de datos de publicación.
- El **db_owner** de la base de datos fija.

## <a name="see-also"></a>Consulte también

[Medir la latencia y validar las conexiones de la replicación transaccional](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)

[sp_helptracertokenhistory &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md)
