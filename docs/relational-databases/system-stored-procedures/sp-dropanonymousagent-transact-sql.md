---
description: sp_dropanonymousagent (Transact-SQL)
title: sp_dropanonymousagent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropanonymousagent
- sp_dropanonymousagent_TSQL
helpviewer_keywords:
- sp_dropanonymousagent
ms.assetid: 4cb96efa-9358-44a3-a8ee-a7e181bed089
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: b75f8b4bc355fd71c469ee2c0593f56feaf9148a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200715"
---
# <a name="sp_dropanonymousagent-transact-sql"></a>sp_dropanonymousagent (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita un agente anónimo de la supervisión de replicación en el distribuidor del publicador. Este procedimiento almacenado se ejecuta en el publicador de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_dropanonymousagent [ @subid= ] sub_id    , [ @type= ] type  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @subid = ] sub_id` Es el identificador global de una suscripción anónima. *sub_id* es de tipo **uniqueidentifier** y no tiene ningún valor predeterminado. Este identificador se puede recuperar en el suscriptor mediante **sp_helppullsubscription**. El valor del campo **subid** del conjunto de resultados devuelto es este identificador global.  
  
`[ @type = ] type` Es el tipo de suscripción. el *tipo* es **int** y no tiene ningún valor predeterminado. Los valores válidos son **1** o **2**. Especifique **1**, si la replicación de instantáneas o la replicación transaccional utilizan el agente de distribución. Especifique **2**, si la replicación de mezcla utiliza el agente de mezcla.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_dropanonymousagent** se utiliza en todos los tipos de replicación.  
  
 Este procedimiento almacenado solo se utiliza para quitar agentes de suscripción anónima y no se puede utilizar para quitar suscripciones conocidas.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de base de datos **db_owner** en la base de datos de distribución pueden ejecutar **sp_dropanonymousagent**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
