---
description: sp_browsereplcmds (Transact-SQL)
title: sp_browsereplcmds (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_browsereplcmds_TSQL
- sp_browsereplcmds
helpviewer_keywords:
- sp_browsereplcmds
ms.assetid: 30abcb41-1d18-4f43-a692-4c80914c0450
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f798dcb9689221d9a8ef1964d4237a1fe649a4c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206626"
---
# <a name="sp_browsereplcmds-transact-sql"></a>sp_browsereplcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve un conjunto de resultados en una versión legible de los comandos replicados almacenados en la base de datos de distribución y se utiliza como herramienta de diagnóstico. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_browsereplcmds [ [ @xact_seqno_start = ] 'xact_seqno_start' ]  
    [ , [ @xact_seqno_end = ] 'xact_seqno_end' ]   
    [ , [ @originator_id = ] 'originator_id' ]  
    [ , [ @publisher_database_id = ] 'publisher_database_id' ]  
    [ , [ @article_id = ] 'article_id' ]  
    [ , [ @command_id= ] command_id ]  
    [ , [ @agent_id = ] agent_id ]  
    [ , [ @compatibility_level = ] compatibility_level ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @xact_seqno_start = ] 'xact_seqno_start'` Especifica el número de secuencia exacto de menor valor que se va a devolver. *xact_seqno_start* es **NCHAR (22)** y su valor predeterminado es es 0x00000000000000000000.  
  
`[ @xact_seqno_end = ] 'xact_seqno_end'` Especifica el número de secuencia exacto más alto que se va a devolver. *xact_seqno_end* es **NCHAR (22)** y su valor predeterminado es es 0xffffffffffffffffffff.  
  
`[ @originator_id = ] 'originator_id'` Especifica si se devuelven los comandos con el *originator_id* especificado. *originator_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @publisher_database_id = ] 'publisher_database_id'` Especifica si se devuelven los comandos con el *publisher_database_id* especificado. *publisher_database_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @article_id = ] 'article_id'` Especifica si se devuelven los comandos con el *article_id* especificado. *article_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @command_id = ] command_id` Es la ubicación del comando en [MSrepl_commands &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/msrepl-commands-transact-sql.md) que se va a descodificar. *command_id* es de **tipo int** y su valor predeterminado es NULL. Si se especifica, también se deben especificar todos los demás parámetros y *xact_seqno_start* deben ser idénticos a *xact_seqno_end*.  
  
`[ @agent_id = ] agent_id` Especifica que solo se devuelvan comandos para un agente de replicación específico. *agent_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @compatibility_level = ] compatibility_level` Es la versión de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que el *COMPATIBILITY_LEVEL* es de **tipo int**, con un valor predeterminado de 9 millones.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**xact_seqno**|**varbinary(16)**|Número de secuencia del comando.|  
|**originator_srvname**|**sysname**|Servidor en el que se originó la transacción.|  
|**originator_db**|**sysname**|Base de datos en la que se originó la transacción.|  
|**article_id**|**int**|IDENTIFICADOR del artículo.|  
|**type**|**int**|Tipo de comando.|  
|**partial_command**|**bit**|Indica si se trata de un comando parcial.|  
|**hashkey**|**int**|Solo para uso interno.|  
|**originator_publication_id**|**int**|Id. de la publicación en la que se originó la transacción.|  
|**originator_db_version**|**int**|Versión de la base de datos en la que se originó la transacción.|  
|**originator_lsn**|**varbinary(16)**|Identifica el número de flujo de registro (LSN) para el comando de la publicación en la que se origina. Se usa en la replicación transaccional punto a punto.|  
|**command**|**nvarchar(1024)**|Comando [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|**command_id**|**int**|IDENTIFICADOR del comando en [MSrepl_commands](../../relational-databases/system-tables/msrepl-commands-transact-sql.md).|  
  
 Los comandos largos se pueden dividir en varias filas en el conjunto de resultados.  
  
## <a name="remarks"></a>Observaciones  
 **sp_browsereplcmds** se utiliza en la replicación transaccional.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o los miembros de los roles fijos de base de datos **db_owner** o **replmonitor** en la base de datos de distribución pueden ejecutar **sp_browsereplcmds**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_replshowcmds &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replshowcmds-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
