---
description: dbo.sysalerts (Transact-SQL)
title: Alertas de dbo.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dbo.sysalerts
- sysalerts_TSQL
- dbo.sysalerts_TSQL
- sysalerts
dev_langs:
- TSQL
helpviewer_keywords:
- sysalerts system table
ms.assetid: a2c2f50d-61f3-4951-996a-add5ad092cc2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 69250eb1576daa37aeb4b633f86ee7dc019255c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195838"
---
# <a name="dbosysalerts-transact-sql"></a>dbo.sysalerts (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada alerta. Una alerta es un mensaje enviado como respuesta a un evento. Una alerta puede reenviar mensajes fuera del entorno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y puede consistir en un mensaje enviado por correo electrónico o a un buscapersonas. Una alerta también puede generar una tarea.  Esta tabla se almacena en la base de datos **msdb** .
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. de la alerta.|  
|**name**|**sysname**|Nombre de la alerta.|  
|**event_source**|**nvarchar(100**|Origen del evento: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**event_category_id**|**int**|Reservado para un uso futuro.|  
|**event_id**|**int**|Reservado para un uso futuro.|  
|**message_id**|**int**|IDENTIFICADOR de mensaje definido por el usuario o referencia al mensaje **sysmessages** que desencadena esta alerta.|  
|**severity**|**int**|Gravedad que desencadena esta alerta.|  
|**enabled**|**tinyint**|Estado de la alerta:<br /><br /> **0** = deshabilitado.<br /><br /> **1** = habilitada.|  
|**delay_between_responses**|**int**|Intervalo de espera, en segundos, entre las notificaciones de esta alerta.|  
|**last_occurrence_date**|**int**|Última repetición (fecha) de la alerta.|  
|**last_occurrence_time**|**int**|Última repeticiónn (hora) de la alerta.|  
|**last_response_date**|**int**|Última notificación (fecha) de la alerta.|  
|**last_response_time**|**int**|Última notificación (hora) de la alerta.|  
|**notification_message**|**nvarchar(512)**|Información adicional enviada con la alerta.|  
|**include_event_description**|**tinyint**|Máscara de red que representa si la descripción del evento se envía por correo electrónico, buscapersonas o net send. Consulte el siguiente gráfico para ver los valores.|  
|**database_name**|**nvarchar(512)**|Base de datos en la que se debe producir el evento para que se desencadene esta alerta.|  
|**event_description_keyword**|**nvarchar(100**|Patrón al que se debe ajustar el error para que se desencadene la alerta.|  
|**occurrence_count**|**int**|Número de repeticiones de esta alerta.|  
|**count_reset_date**|**int**|El recuento Day (Date) se restablecerá a **0**.|  
|**count_reset_time**|**int**|El recuento de la hora del día se restablecerá a **0**.|  
|**job_id**|**uniqueidentifier**|Id. de la tarea ejecutada cuando se produce esta alerta.|  
|**has_notification**|**int**|Número de operadores que reciben notificación por correo electrónico cuando se produce la alerta.|  
|**flags**|**int**|Reservado.|  
|**performance_condition**|**nvarchar(512)**|Reservado.|  
|**category_id**|**int**|Reservado.|  
  
 ## <a name="remarks"></a>Observaciones

En la tabla siguiente se muestran los valores de la máscara de include_event_description. dbo.sysalertas devuelve el valor decimal. 

|Decimal | binary | significado |
|------|------|------|
|0 |0000 |sin mensaje |
|1 |0,001 |email |
|2 |milésima |pager |
|3 |0011 |buscapersonas y correo electrónico |
|4 |0100 |Net send |
|5 |0101 |Net send y correo electrónico |
|6 |0110 |Net send y buscapersonas |
|7 |0111 |Net send, buscapersonas y correo electrónico |
  
