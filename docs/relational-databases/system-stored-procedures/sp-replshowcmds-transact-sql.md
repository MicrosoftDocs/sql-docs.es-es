---
description: sp_replshowcmds (Transact-SQL)
title: sp_replshowcmds (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replshowcmds
- sp_replshowcmds_TSQL
helpviewer_keywords:
- sp_replshowcmds
ms.assetid: 199f5a74-e08e-4d02-a33c-b8ab0db20f44
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb46a1c57dc44f9fb90bbee84906c7bcd8b86d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157512"
---
# <a name="sp_replshowcmds-transact-sql"></a>sp_replshowcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve los comandos de las transacciones marcadas para replicación en un formato legible. **sp_replshowcmds** solo se puede ejecutar cuando las conexiones de cliente (incluida la conexión actual) no leen transacciones replicadas del registro. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replshowcmds [ @maxtrans = ] maxtrans  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @maxtrans = ] maxtrans` Es el número de transacciones de las que se va a devolver información. *maxtrans* es de **tipo int** y su valor predeterminado es **1**, que especifica el número máximo de transacciones pendientes de replicación para las que **sp_replshowcmds** devuelve información.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 **sp_replshowcmds** es un procedimiento de diagnóstico que devuelve información acerca de la base de datos de publicación desde la que se ejecuta.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**xact_seqno**|**binary(10)**|Número de secuencia del comando.|  
|**originator_id**|**int**|IDENTIFICADOR del originador del comando; siempre es **0**.|  
|**publisher_database_id**|**int**|IDENTIFICADOR de la base de datos del publicador, siempre es **0**.|  
|**article_id**|**int**|IDENTIFICADOR del artículo.|  
|**type**|**int**|Tipo de comando.|  
|**command**|**nvarchar(1024)**|Comando [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
  
## <a name="remarks"></a>Observaciones  
 **sp_replshowcmds** se utiliza en la replicación transaccional.  
  
 Con **sp_replshowcmds**, puede ver las transacciones que actualmente no están distribuidas (aquellas transacciones que permanecen en el registro de transacciones que no se han enviado al distribuidor).  
  
 Los clientes que ejecutan **sp_replshowcmds** y **sp_replcmds** en la misma base de datos reciben el error 18752.  
  
 Para evitar este error, el primer cliente debe desconectarse o debe liberarse el rol del cliente como lector del registro mediante la ejecución de **sp_replflush**. Una vez que todos los clientes se han desconectado del lector del registro, **sp_replshowcmds** se pueden ejecutar correctamente.  
  
> [!NOTE]  
>  **sp_replshowcmds** solo debe ejecutarse para solucionar problemas de replicación.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_replshowcmds**.  
  
## <a name="see-also"></a>Consulte también  
 [Mensajes de error](../../relational-databases/native-client-odbc-error-messages/error-messages.md)   
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [sp_repltrans &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
