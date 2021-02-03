---
description: GET_TRANSMISSION_STATUS (Transact-SQL)
title: GET_TRANSMISSION_STATUS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATUS_TSQL
- TRANSMISSION
- TRANSMISSION_TSQL
- GET_TRANSMISSION_STATUS
- STATUS
- GET_TRANSMISSION_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker], transmission status
- Service Broker errors, transmission status
- transmission status information
- status information [SQL Server], conversations
- GET_TRANSMISSION_STATUS statement
ms.assetid: 621805d5-49ed-4764-b3cb-2ae4a3bf797e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 09666ba2e475f2f830654402603e4abde798e7dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186641"
---
# <a name="get_transmission_status-transact-sql"></a>GET_TRANSMISSION_STATUS (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve el estado de la última transmisión de un lado de la conversación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GET_TRANSMISSION_STATUS ( conversation_handle )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *conversation_id*  
 Es el identificador de conversación. Este parámetro es de tipo **uniqueidentifier**.  
  
## <a name="return-types"></a>Tipos de valor devueltos  
 **nchar**  
  
## <a name="remarks"></a>Observaciones  
 Devuelve una cadena que describe el estado del último intento de transmisión de la conversación especificada. Devuelve una cadena vacía si el último intento de transmisión fue correcto, si aún no se ha realizado ningún intento de transmisión o si *conversation_handle* no existe.  
  
 La información devuelta por esta función es la misma que se muestra en la columna last_transmission_error de la vista de administración sys.transmission_queue. No obstante, esta función se puede utilizar para encontrar el estado de transmisión de conversaciones que actualmente no tienen mensajes en la cola de transmisión.  
  
> [!NOTE]  
>  GET_TRANSMISSION_STATUS no proporciona información de mensajes que no tienen un extremo de conversación en la instancia actual. Es decir, no hay información disponible de los mensajes que se reenvían.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se notifica el estado de transmisión de la conversación con el identificador de conversación `58ef1d2d-c405-42eb-a762-23ff320bddf0`.  
  
```sql  
SELECT Status =  
    GET_TRANSMISSION_STATUS('58ef1d2d-c405-42eb-a762-23ff320bddf0') ;  
```  
  
 El siguiente es un conjunto de resultados de ejemplo con la longitud de línea editada:  
  
 ```
 Status  
 ------------------------------- 
 The Service Broker protocol transport is disabled or not configured.
 ```  
  
 En este caso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no está configurado para permitir que [!INCLUDE[ssSB](../../includes/sssb-md.md)] se comunique en la red.  
  
## <a name="see-also"></a>Consulte también  
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.transmission_queue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)  
  
  
