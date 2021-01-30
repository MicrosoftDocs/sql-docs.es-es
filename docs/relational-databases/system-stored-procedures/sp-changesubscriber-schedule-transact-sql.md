---
description: sp_changesubscriber_schedule (Transact-SQL)
title: sp_changesubscriber_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubscriber_schedule
- sp_changesubscriber_schedule_TSQL
helpviewer_keywords:
- sp_changesubscriber_schedule
ms.assetid: ff84e8e2-d496-482c-b23e-38a6626596e6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9e93079a747c3766a47ababf3fdd866053b40968
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171768"
---
# <a name="sp_changesubscriber_schedule-transact-sql"></a>sp_changesubscriber_schedule (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia la programación del Agente de distribución o del Agente de mezcla para un suscriptor. Este procedimiento almacenado se ejecuta en el publicador de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changesubscriber_schedule [ @subscriber = ] 'subscriber', [ @agent_type = ] type  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. el *suscriptor* es de **tipo sysname**. El nombre del suscriptor tiene que ser único en la base de datos, no puede existir previamente y no puede ser NULL.  
  
`[ @agent_type = ] type` Es el tipo de agente. el *tipo* es **smallint** y su valor predeterminado es **0**. **0** indica un agente de distribución. **1** indica un agente de mezcla.  
  
`[ @frequency_type = ] frequency_type` Es la frecuencia con la que se programa la tarea de distribución. *frequency_type* es de **tipo int** y su valor predeterminado es **64**. Hay 10 columnas de programación.  
  
`[ @frequency_interval = ] frequency_interval` Es el valor que se aplica a la frecuencia establecida por *frequency_type*. *frequency_interval* es de **tipo int** y su valor predeterminado es **1**.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Es la fecha de la tarea de distribución. *frequency_relative_interval* es de **tipo int** y su valor predeterminado es **1**.  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Es el factor de periodicidad utilizado por *frequency_type*. *frequency_recurrence_factor* es de **tipo int** y su valor predeterminado es **0**.  
  
`[ @frequency_subday = ] frequency_subday` Es la frecuencia, en minutos, con la que se vuelve a programar durante el período definido. *frequency_subday* es de **tipo int** y su valor predeterminado es **4**.  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Es el intervalo de *frequency_subday*. *frequency_subday_interval* es de **tipo int** y su valor predeterminado es **5**.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Es la hora del día en la que la tarea de distribución se programa por primera vez. *active_start_time_of_day* es de **tipo int** y su valor predeterminado es **0**.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Es la hora del día a la que la tarea de distribución deja de estar programada. *active_end_time_of_day* es de **tipo int** y su valor predeterminado es **235959**, lo que significa 11:59:59 P.M. en un reloj de 24 horas.  
  
`[ @active_start_date = ] active_start_date` Es la fecha en la que la tarea de distribución se programa por primera vez, con el formato AAAAMMDD. *active_start_date* es de **tipo int** y su valor predeterminado es **0**.  
  
`[ @active_end_date = ] active_end_date` Es la fecha en la que la tarea de distribución deja de estar programada, con el formato AAAAMMDD. *active_end_date* es de **tipo int**, con un valor predeterminado de **99991231**, que significa el 31 de diciembre de 9999.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al cambiar las propiedades de un artículo en un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changesubscriber_schedule** se utiliza en todos los tipos de replicación.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_changesubscriber_schedule**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addsubscriber_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscriber-schedule-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
