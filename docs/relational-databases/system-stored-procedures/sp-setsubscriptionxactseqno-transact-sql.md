---
description: sp_setsubscriptionxactseqno (Transact-SQL)
title: sp_setsubscriptionxactseqno (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_setsubscriptionxactseqno
- sp_setsubscriptionxactseqno_TSQL
helpviewer_keywords:
- sp_setsubscriptionxactseqno
ms.assetid: cdb4e0ba-5370-4905-b03f-0b0c6f080ca6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3ec2589b3cacb2e4426793b6adb7814c03e19fce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176409"
---
# <a name="sp_setsubscriptionxactseqno-transact-sql"></a>sp_setsubscriptionxactseqno (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Se utiliza durante la solución de problemas para especificar la última transacción entregada mediante el número de secuencia de registro (LSN), lo que permite que el Agente de distribución comience a entregar en la siguiente transacción. Tras el reinicio, el Agente de distribución devuelve las transacciones superiores a esta marca de agua (LSN) de la memoria caché de la base de datos de distribución (msrepl_commands). Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones. No se admite para suscriptores que no sean de SQL Server.  
  
> [!CAUTION]  
>  El uso incorrecto de este procedimiento almacenado o la especificación de un valor LSN incorrecto puede ocasionar que el Agente de distribución revierta los cambios ya aplicados en el suscriptor o que omita todos los cambios restantes.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_setsubscriptionxactseqno [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @xact_seqno = ] xact_seqno   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado. En el caso de un publicador que no sea de SQL Server, *publisher_db* es el nombre de la base de datos de distribución.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado. Cuando el Agente de distribución se comparte con más de una publicación, debe especificar un valor de ALL para la *publicación*.  
  
`[ @xact_seqno = ] xact_seqno` Es el LSN de la siguiente transacción en el distribuidor que se va a aplicar en el suscriptor. *xact_seqno* es **varbinary (16)** y no tiene ningún valor predeterminado.  
  
## <a name="result-set"></a>Tipo de cursor  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**ORIGINAL XACT_SEQNO**|**varbinary(16)**|El LSN original de la siguiente transacción que se va a aplicar en el suscriptor.|  
|**UPDATED XACT_SEQNO**|**varbinary(16)**|El LSN actualizado de la siguiente transacción que se va a aplicar en el suscriptor.|  
|**SUBSCRIPTION STREAM COUNT**|**int**|El número de flujos de suscripción utilizados durante la última sincronización.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_setsubscriptionxactseqno** se utiliza en la replicación transaccional.  
  
 no se puede usar **sp_setsubscriptionxactseqno** en una topología de replicación transaccional punto a punto.  
  
 **sp_setsubscriptionxactseqno** se puede utilizar para omitir una transacción específica que está causando un error cuando se aplica en el suscriptor. Cuando se produce un error y después de que se detenga el Agente de distribución, llame a [sp_helpsubscriptionerrors &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-helpsubscriptionerrors-transact-sql.md) en el distribuidor para recuperar el valor xact_seqno de la transacción con errores y, a continuación, llame a **sp_setsubscriptionxactseqno**, pasando este valor para *xact_seqno*. Así se garantizará que se procesen solamente los comandos después de este LSN.  
  
 Especifique un valor de **0** para que *xact_seqno* entregue todos los comandos pendientes en la base de datos de distribución al suscriptor.  
  
 **sp_setsubscriptionxactseqno** puede producir un error si el agente de distribución utiliza secuencias de varias suscripciones.  
  
 Cuando se produzca este error, debe ejecutar el Agente de distribución con un flujo de suscripción único. Para más información, consulte [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_setsubscriptionxactseqno**.  
  
## <a name="see-more"></a>Ver más

[Blog: Cómo omitir una transacción](https://repltalk.com/2019/05/28/how-to-skip-a-transaction/)  
