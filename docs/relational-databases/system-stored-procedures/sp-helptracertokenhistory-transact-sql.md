---
description: sp_helptracertokenhistory (Transact-SQL)
title: sp_helptracertokenhistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helptracertokenhistory_TSQL
- sp_helptracertokenhistory
helpviewer_keywords:
- sp_helptracertokenhistory
ms.assetid: 96910d1c-be76-43eb-9c93-4477e6761749
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f701a1681281bfb63cf6c39ba42869ac83df204e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192879"
---
# <a name="sp_helptracertokenhistory-transact-sql"></a>sp_helptracertokenhistory (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve información detallada de latencia acerca de los testigos de seguimiento especificados. Se devuelve una fila por cada suscriptor. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación o en el distribuidor de la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helptracertokenhistory [ @publication = ] 'publication'   
        , [ @tracer_id = ] tracer_id  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db = ] 'publisher_db' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación en la que se insertó el testigo de seguimiento. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @tracer_id = ] tracer_id` Es el identificador del testigo de seguimiento en el [MStracer_tokens &#40;tabla de&#41;de Transact-SQL ](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) para la que se devuelve información del historial. *tracer_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @publisher = ] 'publisher'` Nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]
>  Este parámetro solo debe especificarse para los publicadores que no sean de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @publisher_db = ] 'publisher_db'` Nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL. Si el procedimiento almacenado se ejecuta en el publicador, se omite este parámetro.  
  
## <a name="result-set"></a>Tipo de cursor  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**distributor_latency**|**bigint**|Número de segundos entre la confirmación del registro del token de seguimiento en el publicador y la confirmación del registro en el distribuidor.|  
|**suscriptor**|**sysname**|Nombre del suscriptor que ha recibido el token de seguimiento.|  
|**subscriber_db**|**sysname**|Nombre de la base de datos de suscripciones en la que se ha insertado el testigo de seguimiento.|  
|**subscriber_latency**|**bigint**|Número de segundos entre la confirmación del registro del token de seguimiento en el distribuidor y la confirmación del registro en el suscriptor.|  
|**overall_latency**|**bigint**|Número de segundos entre la confirmación del registro del token de seguimiento en el publicador y la confirmación del registro del token en el suscriptor.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helptracertokenhistory** se utiliza en la replicación transaccional.  
  
 Ejecute [sp_helptracertokens &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) para obtener una lista de testigos de seguimiento para la publicación.  
  
 Un valor NULL en el conjunto de resultados significa que no es posible calcular las estadísticas de latencia. Esto es debido a que no se ha recibido el token de seguimiento en el distribuidor o en uno de los suscriptores.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_tracertokens](../../relational-databases/replication/codesnippet/tsql/sp-helptracertokenhistor_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , el rol fijo de base de datos **db_owner** en la base de datos de publicación, o **db_owner** roles fijos de base de datos o **replmonitor** en la base de datos de distribución pueden ejecutar **sp_helptracertokenhistory**.  
  
## <a name="see-also"></a>Consulte también  
 [Medir la latencia y validar las conexiones para la replicación transaccional](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)   
 [sp_deletetracertokenhistory &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md)  
  
  
