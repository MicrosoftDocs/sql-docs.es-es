---
description: syspolicy_policy_execution_history_details (Transact-SQL)
title: syspolicy_policy_execution_history_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_policy_execution_history_details
- syspolicy_policy_execution_history_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_policy_execution_history_details view
ms.assetid: 97ef6573-5e8b-4ba5-8ae0-7901e79a9683
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d10e65b6a701acaabc80418d59da019250acf24b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171828"
---
# <a name="syspolicy_policy_execution_history_details-transact-sql"></a>syspolicy_policy_execution_history_details (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra las expresiones de condiciones que se ejecutaron, los destinos de las expresiones, el resultado de cada ejecución y detalles sobre los errores, si se produjo alguno. En la tabla siguiente se describen las columnas de la vista syspolicy_execution_history_details.  
  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|detail_id|**bigint**|Identificador de este registro. Cada registro representa el intento para evaluar o exigir una expresión de condición en una directiva. Si se aplica a varios destinos, cada condición tendrá un registro de detalle para cada uno.|  
|history_id|**bigint**|Identificador del evento del historial. Cada evento del historial representa un intento de ejecutar una directiva. Dado que una condición puede tener varias expresiones de condiciones y varios destinos, un identificador history_id puede crear varios registros de detalle. Utilice la columna history_id para unir esta vista a la vista de [syspolicy_policy_execution_history](../../relational-databases/system-catalog-views/syspolicy-policy-execution-history-transact-sql.md) .|  
|target_query_expression|**nvarchar(max)**|Destino de la directiva y vista syspolicy_policy_execution_history.|  
|execution_date|**datetime**|Fecha y hora cuando se creó este registro de detalle.|  
|resultado|**bit**|Corrección o error de este destino y evaluación de la expresión de condición:<br /><br /> 0 (correcto) o 1 (error).|  
|result_detail|**nvarchar(max)**|Mensaje del resultado. Solo está disponible si lo proporciona la faceta.|  
|exception_message|**nvarchar(max)**|Mensaje generado por la excepción, si se produjo alguna.|  
|excepción|**nvarchar(max)**|Descripción de la excepción, si se produjo alguna.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando solucione problemas de la administración basada en directivas, consulte la vista  syspolicy_policy_execution_history_details para determinar en qué combinaciones de expresión de condición y destino se produjo un error y cuándo, y revise los errores relacionados.  
  
 La consulta siguiente combina la vista `syspolicy_policy_execution_history_details` con las vistas `syspolicy_policy_execution_history_details` y `syspolicy_policies` para mostrar el nombre de la directiva, el nombre de la condición y detalles sobre los errores.  
  
```  
SELECT Pol.name AS Policy,   
Cond.name AS Condition,   
PolHistDet.target_query_expression,   
PolHistDet.execution_date,   
PolHistDet.result,   
PolHistDet.result_detail,   
PolHistDet.exception_message,   
PolHistDet.exception   
FROM msdb.dbo.syspolicy_policies AS Pol  
JOIN msdb.dbo.syspolicy_conditions AS Cond  
    ON Pol.condition_id = Cond.condition_id  
JOIN msdb.dbo.syspolicy_policy_execution_history AS PolHist  
    ON Pol.policy_id = PolHist.policy_id  
JOIN msdb.dbo.syspolicy_policy_execution_history_details AS PolHistDet  
    ON PolHist.history_id = PolHistDet.history_id  
WHERE PolHistDet.result = 0 ;  
```  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
## <a name="see-also"></a>Consulte también  
 [Administrar servidores mediante administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Vistas de administración basada en directivas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
