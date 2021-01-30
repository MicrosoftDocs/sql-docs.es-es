---
description: sp_helpqreader_agent (Transact-SQL)
title: sp_helpqreader_agent (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpqreader_agent_TSQL
- sp_helpqreader_agent
helpviewer_keywords:
- sp_helpqreader_agent
ms.assetid: 8e74e1aa-e95b-4183-8017-bf123439b08d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d4d0a96017b6bc336f73535648ed8a411b465c05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210829"
---
# <a name="sp_helpqreader_agent-transact-sql"></a>sp_helpqreader_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve las propiedades del Agente de lectura de cola. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución o en el publicador en cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpqreader_agent [ [ @frompublisher = ] frompublisher ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @frompublisher = ] frompublisher` Especifica si se llama al procedimiento almacenado en el publicador o en el distribuidor. *frompublisher* es de bit y su valor predeterminado es 0. **1** significa que se llama al procedimiento almacenado desde el publicador y **0** significa que se llama al procedimiento almacenado desde el distribuidor.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|IDENTIFICADOR del agente.|  
|**name**|**nvarchar(100**|Nombre del agente.|  
|**job_id**|**uniqueidentifier**|Id. único del trabajo del agente.|  
|**job_login**|**nvarchar(512)**|Es la cuenta de Windows con la que se ejecuta el agente de distribución, que se devuelve con el formato *dominio* \\ *nombreDeUsuario*.|  
|**job_password**|**sysname**|Por motivos de seguridad, **\*\*\*\*\*\*\*\*\*\*** siempre se devuelve un valor de.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpqreader_agent** se utiliza en la replicación transaccional.  
  
## <a name="permissions"></a>Permisos  
 Cuando el valor de *frompublisher* es **1**, solo los miembros del rol fijo de servidor **sysadmin** en el publicador o los miembros del rol fijo de base de datos **db_owner** en la base de datos de publicación pueden ejecutar **sp_helpqreader_agent**. De lo contrario, solo los miembros del rol fijo de servidor **sysadmin** en el distribuidor o los miembros del rol fijo de base de datos **db_owner** en la base de datos de distribución pueden ejecutar **sp_helpqreader_agent**.  
  
## <a name="see-also"></a>Consulte también  
 [Habilitar suscripciones actualizables para publicaciones transaccionales](../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
  
