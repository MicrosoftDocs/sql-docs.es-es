---
description: sysdbmaintplan_history (Transact-SQL)
title: sysdbmaintplan_history (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdbmaintplan_history_TSQL
- sysdbmaintplan_history
dev_langs:
- TSQL
helpviewer_keywords:
- sysdbmaintplan_history system table
ms.assetid: 02d36f08-ac93-4463-bb59-284c5cd6ed04
author: cawrites
ms.author: chadam
ms.openlocfilehash: dd830c961b7e5c5f2d4cbc47557ef90a072570dc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093713"
---
# <a name="sysdbmaintplan_history-transact-sql"></a>sysdbmaintplan_history (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esta tabla se almacena en la base de datos **msdb** .  
  
 [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]  
  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**sequence_id**|**int**|Secuencia del historial llevada a cabo según los planes de mantenimiento de bases de datos.|  
|**plan_id**|**uniqueidentifier**|Id. del plan de mantenimiento de bases de datos.|  
|**plan_name**|**sysname**|Nombre del plan de mantenimiento de bases de datos.|  
|**database_name**|**sysname**|Nombre de la base de datos asociada al plan de mantenimiento de bases de datos.|  
|**server_name**|**sysname**|Nombre del sistema.|  
|**activity**|**nvarchar(128)**|Actividad llevada a cabo por el plan de mantenimiento de bases de datos (por ejemplo, el registro de transacciones de copia de seguridad).|  
|**succeeded**|**bit**|**0** = correcto **1** = error|  
|**end_time**|**datetime**|Hora en que finalizó la acción.|  
|**duration**|**int**|Tiempo necesario para finalizar la acción del plan de mantenimiento de bases de datos.|  
|**start_time**|**datetime**|Hora en que comenzó la acción.|  
|**error_number**|**int**|Número de error generado.|  
|**message**|**nvarchar(512)**|Mensaje generado por **SQLMAINT**.|  
  
  
