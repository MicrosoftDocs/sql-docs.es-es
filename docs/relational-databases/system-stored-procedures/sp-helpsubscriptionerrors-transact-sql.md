---
description: sp_helpsubscriptionerrors (Transact-SQL)
title: sp_helpsubscriptionerrors (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscriptionerrors_TSQL
- sp_helpsubscriptionerrors
helpviewer_keywords:
- sp_helpsubscriptionerrors
ms.assetid: 01c8bc21-939e-490d-8cc8-219c068be31e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ccf568629cc56f3ce5187fe9e4739192caf53722
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192887"
---
# <a name="sp_helpsubscriptionerrors-transact-sql"></a>sp_helpsubscriptionerrors (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve todos los errores de la replicación transaccional de una suscripción determinada. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpsubscriptionerrors [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'   
        , [ @publication = ] 'publication'   
        , [ @subscriber = ] 'subscriber'   
        , [ @subscriber_db = ] 'subscriber_db'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="result-set"></a>Tipo de cursor  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. del error.|  
|**time**|**datetime**|Hora en que se produjo el error.|  
|**error_type_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**source_type_id**|**int**|Id. del tipo de origen del error.|  
|**source_name**|**nvarchar(100**|Nombre del origen del error.|  
|**error_code**|**sysname**|Código de error.|  
|**error_text**|**ntext**|Mensaje de error.|  
|**xact_seqno**|**varbinary(16)**|Número de secuencia del registro de transacciones de inicio del lote de ejecución fallido. Solo lo utilizan los Agentes de distribución; se trata del número de secuencia del registro de transacciones de la primera transacción en el proceso por lotes de ejecución errónea.|  
|**command_id**|**int**|IDENTIFICADOR de comando del lote de ejecución con errores. Solo lo utilizan los Agentes de distribución, y se trata del Id. del comando del primer comando del proceso por lotes de ejecución fallida.|  
|**session_id**|**int**|IDENTIFICADOR de la sesión del agente en la que se produjo el error.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpsubscriptionerrors** se utiliza con la replicación de instantáneas y transaccional.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helpsubscriptionerrors**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_helpsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
