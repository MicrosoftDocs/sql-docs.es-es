---
description: sp_helpxactsetjob (Transact-SQL)
title: sp_helpxactsetjob (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpxactsetjob
- sp_helpxactsetjob_TSQL
helpviewer_keywords:
- sp_helpxactsetjob
ms.assetid: 242cea3e-e6ac-4f84-a072-b003b920eb33
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39092ea96ed0f143af7322fea3f04d38d3e4eef0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194728"
---
# <a name="sp_helpxactsetjob-transact-sql"></a>sp_helpxactsetjob (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra información acerca del trabajo Xactset para un publicador de Oracle. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpxactsetjob [ @publisher = ] 'publisher'   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al que pertenece el trabajo. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**jobnumber**|**int**|Número de trabajo de Oracle.|  
|**lastdate**|**VARCHAR (22)**|Fecha en que se ejecutó el trabajo por última vez.|  
|**thisdate**|**VARCHAR (22)**|Hora de cambio.|  
|**nextdate**|**VARCHAR (22)**|Fecha en que se ejecutará el trabajo de nuevo.|  
|**deteriora**|**varchar(1)**|Marca de interrupción del trabajo.|  
|**interval**|**VARCHAR (200)**|Intervalo del trabajo.|  
|**falla**|**int**|Número de errores del trabajo.|  
|**xactsetjobwhat**|**VARCHAR (200)**|Nombre del procedimiento ejecutado por el trabajo.|  
|**xactsetjob**|**varchar(1)**|Estado del trabajo, que puede ser uno de los siguientes:<br /><br /> **1** : el trabajo está habilitado.<br /><br /> **0** : el trabajo está deshabilitado.|  
|**xactsetlonginterval**|**int**|Intervalo largo del trabajo.|  
|**xactsetlongthreshold**|**int**|Umbral largo del trabajo.|  
|**xactsetshortinterval**|**int**|Intervalo corto del trabajo.|  
|**xactsetshortthreshold**|**int**|Umbral corto del trabajo.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpxactsetjob** se utiliza en la replicación de instantáneas y en la replicación transaccional para un publicador de Oracle.  
  
 **sp_helpxactsetjob** siempre devuelve la configuración actual del trabajo Xactset (HREPL_XactSetJob) en el publicador. Si el trabajo Xactset se encuentra en la cola de trabajos, devuelve además los atributos del trabajo de la vista de diccionario de datos USER_JOB creada con la cuenta de administrador en el publicador de Oracle.  
  
## <a name="permissions"></a>Permisos  
 Solo un miembro del rol fijo de servidor **sysadmin** puede ejecutar **sp_helpxactsetjob**.  
  
## <a name="see-also"></a>Consulte también  
 [Configuración del trabajo del conjunto de transacciones para un publicador de Oracle&#40;programación de la replicación con Transact-SQL&#41;](../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)   
 [sp_publisherproperty &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-publisherproperty-transact-sql.md)  
  
  
