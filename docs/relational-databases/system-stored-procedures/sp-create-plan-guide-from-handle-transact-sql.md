---
description: sp_create_plan_guide_from_handle (Transact-SQL)
title: sp_create_plan_guide_from_handle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_plan_guide_from_handle_TSQL
- sp_create_plan_guide_from_handle
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_plan_guide_from_handle
ms.assetid: 02cfb76f-a0f9-4b42-a880-1c3e7d64fe41
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2020abc49ccd29491f99adc77910868071e16e00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186223"
---
# <a name="sp_create_plan_guide_from_handle-transact-sql"></a>sp_create_plan_guide_from_handle (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea una o varias guías de plan a partir de un plan de consulta en la memoria caché del plan. Puede utilizar este procedimiento almacenado para asegurarse de que el optimizador de consultas siempre utiliza un plan de consulta concreto para la consulta especificada. Para obtener más información acerca de las guías de plan, vea [Plan Guides](../../relational-databases/performance/plan-guides.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_create_plan_guide_from_handle [ @name = ] N'plan_guide_name'  
    , [ @plan_handle = ] plan_handle  
    , [ [ @statement_start_offset = ] { statement_start_offset | NULL } ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @name =] N '*plan_guide_name*'  
 Es el nombre de la guía de plan. Los nombres de guía de plan se encuentran en el ámbito de la base de datos actual. *plan_guide_name* debe cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md) y no puede comenzar por el signo de número (#). La longitud máxima de *plan_guide_name* es de 124 caracteres.  
  
 [ @plan_handle =] *plan_handle*  
 Identifica un lote en la memoria caché del plan. *plan_handle* es **varbinary (64)**. *plan_handle* se pueden obtener de la vista de administración dinámica [Sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) .  
  
 [ @statement_start_offset =] { *statement_start_offset* | NULL}]  
 Identifica la posición inicial de la instrucción dentro del lote del *plan_handle* especificado. *statement_start_offset* es de **tipo int** y su valor predeterminado es NULL.  
  
 El desplazamiento de la instrucción corresponde a la columna statement_start_offset de la vista de administración dinámica [Sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) .  
  
 Si se especifica NULL o no se especifica un desplazamiento de instrucción, se crea una guía de plan para cada instrucción del lote utilizando el plan de consulta para el identificador de plan especificado. Las guías de plan resultantes son equivalentes a las guías de plan que utilizan la sugerencia de consulta USE PLAN para forzar el uso de un plan concreto.  
  
## <a name="remarks"></a>Observaciones  
 No se puede crear una guía de plan para todos los tipos de instrucción. Si no puede crearse una guía de plan para una instrucción del lote, el procedimiento almacenado omite la instrucción y continúa en la instrucción siguiente del lote. Si una instrucción aparece varias veces en el mismo lote, se habilita el plan para la última aparición y se deshabilitan los planes anteriores para la instrucción. Si no se puede utilizar ninguna instrucción del lote en una guía de plan, se producirá el error 10532 y la instrucción producirá un error. Se recomienda obtener siempre el identificador de plan a partir de la vista de administración dinámica sys.dm_exec_query_stats para evitar en lo posible la aparición de este error.  
  
> [!IMPORTANT]  
>  sp_create_plan_guide_from_handle crea guías de plan basadas en planes según aparecen en la caché del plan. Esto significa que el texto por lotes, las instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] y el plan de presentación XML se toman carácter a carácter (incluidos los valores literales pasados a la consulta) desde la caché del plan hasta la guía de plan resultante. Estas cadenas de texto pueden contener información confidencial que se almacena en los metadatos de la base de datos. Los usuarios con los permisos adecuados pueden ver esta información mediante la vista de catálogo sys.plan_guides y el cuadro de diálogo Propiedades de la **Guía de plan** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . Para asegurarse de que dicha información confidencial no se divulga a través de una guía de plan, se recomienda revisar las guías de plan creadas a partir de la memoria caché del plan.  
  
## <a name="creating-plan-guides-for-multiple-statements-within-a-query-plan"></a>Crear guías de plan para varias instrucciones dentro de un plan de consulta  
 Al igual que sp_create_plan_guide, sp_create_plan_guide_from_handle quita el plan de consulta para el módulo o lote concreto de la caché del plan. Esto se hace para asegurarse de que todos los usuarios empiezan a utilizar la nueva guía de plan. Al crear una guía de plan para varias instrucciones dentro de un único plan de consulta, puede posponer la eliminación del plan de caché mediante la creación de todas las guías de plan en una transacción explícita. Este método permite al plan permanecer en la memoria caché hasta que se completa la transacción y crear una guía de plan para cada instrucción especificada. Vea el ejemplo B.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW SERVER STATE`. Además, se requieren permisos individuales para cada guía de plan creada mediante sp_create_plan_guide_from_handle. Para crear una guía de plan de tipo OBJECT `ALTER` se requiere el permiso en el objeto al que se hace referencia. Para crear una guía de plan de tipo SQL o TEMPLATE, se requiere `ALTER` el permiso en la base de datos actual. Para determinar el tipo de guía de plan que se va a crear, ejecute la consulta siguiente:  
  
```sql  
SELECT cp.plan_handle, sql_handle, st.text, objtype   
FROM sys.dm_exec_cached_plans AS cp  
JOIN sys.dm_exec_query_stats AS qs ON cp.plan_handle = qs.plan_handle  
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS st;  
```  
  
 En la fila que contiene la instrucción para la que está creando la guía de plan, examine la columna `objtype` en el conjunto de resultados. Un valor de `Proc` indica que la guía de plan es de tipo OBJECT. Otros valores como `AdHoc` o `Prepared` indican que la guía de plan es de tipo SQL.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-plan-guide-from-a-query-plan-in-the-plan-cache"></a>A. Crear una guía de plan a partir de un plan de consulta en la caché del plan  
 En el ejemplo siguiente se especifica un plan de consulta desde la caché del plan para crear una guía de plan para una única instrucción SELECT. El ejemplo comienza ejecutando una sencilla instrucción `SELECT` para la que se creará la guía de plan. El plan para esta consulta se examina mediante las vistas de administración dinámica `sys.dm_exec_sql_text` y `sys.dm_exec_text_query_plan`. A continuación, se crea la guía de plan para la consulta después de especificar el plan de consulta en la caché del plan asociada a la consulta. La última instrucción del ejemplo comprueba que la guía de plan existe.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT WorkOrderID, p.Name, OrderQty, DueDate  
FROM Production.WorkOrder AS w   
JOIN Production.Product AS p ON w.ProductID = p.ProductID  
WHERE p.ProductSubcategoryID > 4  
ORDER BY p.Name, DueDate;  
GO  
-- Inspect the query plan by using dynamic management views.  
SELECT * FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(sql_handle)  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset) AS qp  
WHERE text LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
GO  
-- Create a plan guide for the query by specifying the query plan in the plan cache.  
DECLARE @plan_handle varbinary(64);  
DECLARE @offset int;  
SELECT @plan_handle = plan_handle, @offset = qs.statement_start_offset  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(sql_handle) AS st  
CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, qs.statement_start_offset, qs.statement_end_offset) AS qp  
WHERE text LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
  
EXECUTE sp_create_plan_guide_from_handle   
    @name =  N'Guide1',  
    @plan_handle = @plan_handle,  
    @statement_start_offset = @offset;  
GO  
-- Verify that the plan guide is created.  
SELECT * FROM sys.plan_guides  
WHERE scope_batch LIKE N'SELECT WorkOrderID, p.Name, OrderQty, DueDate%';  
GO  
```  
  
### <a name="b-creating-multiple-plan-guides-for-a-multistatement-batch"></a>B. Crear varias guías de plan para un lote de varias instrucciones  
 El ejemplo siguiente crea una guía de plan para dos instrucciones dentro de un lote de varias instrucciones. Las guías de plan se crean dentro de una transacción explícita de modo que el plan de consulta para el lote no se quite de la caché del plan una vez creada la primera guía de plan. El ejemplo comienza ejecutando un lote de varias instrucciones. El plan para el lote se examina mediante las vistas de administración dinámica. Observe que se devuelve una fila por cada instrucción del lote. A continuación, se crea una guía de plan para la primera y tercera instrucciones del lote mediante el parámetro `@statement_start_offset`. La última instrucción del ejemplo comprueba que las guías de plan existen.  
  
 [!code-sql[PlanGuides#Create_From_Handle2](../../relational-databases/system-stored-procedures/codesnippet/tsql/sp-create-plan-guide-fro_1.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [Guías de plan](../../relational-databases/performance/plan-guides.md)   
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_text_query_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-text-query-plan-transact-sql.md)   
 [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
  
  
