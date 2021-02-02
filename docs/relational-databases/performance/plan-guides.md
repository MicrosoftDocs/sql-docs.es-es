---
title: Guías de plan | Microsoft Docs
description: Obtenga información sobre las guías de plan, que le permiten optimizar el rendimiento de las consultas sin cambiar directamente el texto de la consulta en SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- TEMPLATE plan guide
- SQL plan guides
- OPTIMIZE FOR query hint
- RECOMPILE query hint
- OBJECT plan guide
- plan guides [SQL Server], about plan guides
- OPTION clause
- plan guides [SQL Server]
- USE PLAN query hint
ms.assetid: bfc97632-c14c-4768-9dc5-a9c512f6b2bd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e230b163fdbe9f48ec4f436b4ef2453040b89b24
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048865"
---
# <a name="plan-guides"></a>Guías de plan
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Las guías de plan permiten optimizar el rendimiento de las consultas cuando no pueda o no desee cambiar directamente el texto de la consulta real en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Las guías de plan influyen en la optimización de las consultas adjuntando sugerencias de consulta o un plan de consulta fijo para ellas. Las guías de plan pueden ser de gran utilidad cuando el rendimiento de un pequeño subconjunto de consultas de una aplicación de base de datos proporcionado por otro proveedor no es el esperado. En la guía de plan, se especifica la instrucción Transact-SQL que se desea optimizar y además una cláusula OPTION que incluye las sugerencias de consulta que se desean usar o un plan de consulta específico con el que desea optimizar la consulta. Cuando la consulta se ejecuta, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hace coincidir la instrucción Transact-SQL con la guía de plan y además adjunta en tiempo de ejecución la cláusula OPTION a la consulta o usa el plan de consulta especificado.  
  
 El número total de guías de plan que se pueden crear solo está limitado por los recursos de los que disponga el sistema. No obstante, las guías de plan deberían limitarse a aquellas consultas de gran importancia cuyo rendimiento se desea mejorar o estabilizar. No se deben usar las guías de plan para influenciar la mayor parte de la carga de la consulta de una aplicación implementada.  
  
> [!NOTE]
>  Las guías de plan no se pueden usar en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). Las guías de plan son visibles en todas las ediciones. También se pueden adjuntar bases de datos que incluyen guías de plan a cualquier versión. Las guías de plan permanecen intactas cuando se restaura o adjunta una base de datos a una versión actualizada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="types-of-plan-guides"></a>Tipos de guías de plan  
 Se pueden crear los siguientes tipos de guías de plan.  
  
 ### <a name="object-plan-guide"></a>OBJECT [guía de plan]  
 Una guía de plan OBJECT compara las consultas que se ejecutan en el contexto de procedimientos almacenados de [!INCLUDE[tsql](../../includes/tsql-md.md)] , funciones escalares definidas por el usuario, funciones definidas por el usuario con valores de tabla de múltiples instrucciones y desencadenadores DML.  
  
 Suponga que el siguiente procedimiento almacenado, que usa el parámetro `@Country_region`, está en una aplicación de base de datos que se implementa con la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]:  
  
```sql  
CREATE PROCEDURE Sales.GetSalesOrderByCountry (@Country_region nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h, Sales.Customer AS c,   
        Sales.SalesTerritory AS t  
    WHERE h.CustomerID = c.CustomerID  
        AND c.TerritoryID = t.TerritoryID  
        AND CountryRegionCode = @Country_region  
END;  
```  
  
 Asuma que este procedimiento almacenado se ha compilado y optimizado para `@Country_region = N'AU'` (Australia). Sin embargo, dado hay relativamente pocos pedidos de ventas que se originen en Australia, el rendimiento se reduce cuando la consulta ejecuta usando valores para los parámetros que se corresponden con países con más pedidos de ventas. Dado que el mayor número de pedidos de ventas se origina en Estados Unidos, el rendimiento de un plan de consulta generado para `@Country_region = N'US'` será probablemente mejor para todos los valores posibles del parámetro `@Country_region`.  
  
 Puede solucionar este problema modificando el procedimiento almacenado y agregando la sugerencia de consulta `OPTIMIZE FOR` a la consulta. No obstante, puesto que el procedimiento almacenado se encuentra en una aplicación implementada, no puede modificar directamente el código de la aplicación. En su lugar, puede crear la guía de plan siguiente en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```sql  
sp_create_plan_guide   
@name = N'Guide1',  
@stmt = N'SELECT *FROM Sales.SalesOrderHeader AS h,  
        Sales.Customer AS c,  
        Sales.SalesTerritory AS t  
        WHERE h.CustomerID = c.CustomerID   
            AND c.TerritoryID = t.TerritoryID  
            AND CountryRegionCode = @Country_region',  
@type = N'OBJECT',  
@module_or_batch = N'Sales.GetSalesOrderByCountry',  
@params = NULL,  
@hints = N'OPTION (OPTIMIZE FOR (@Country_region = N''US''))';  
```  
  
 Cuando se ejecute la consulta especificada en la instrucción `sp_create_plan_guide` , se modificará la consulta antes de la optimización para incluir la cláusula `OPTIMIZE FOR (@Country = N''US'')` .  
  
 ### <a name="sql-plan-guide"></a>Guía de plan SQL  
 Una guía de plan de SQL compara las consultas que se ejecutan en el contexto de instrucciones independientes de [!INCLUDE[tsql](../../includes/tsql-md.md)] y lotes que no forman parte de un objeto de base de datos. Las guías de plan basadas en SQL también se pueden usar para comparar consultas que se parametrizan en un formulario especificado. Las guías de plan de SQL se aplican a las instrucciones y lotes independientes de [!INCLUDE[tsql](../../includes/tsql-md.md)] . Con frecuencia, las aplicaciones envían esas instrucciones usando el procedimiento almacenado del sistema [sp_executesql](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md) . Considere, por ejemplo, el siguiente lote independiente:  
  
```sql  
SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC;  
```  
  
 Para evitar que se genere un plan de ejecución paralelo en esta consulta, cree la siguiente guía de plan y establezca la sugerencia de consulta `MAXDOP` en `1` en el parámetro `@hints` .  
  
```sql  
sp_create_plan_guide   
@name = N'Guide2',   
@stmt = N'SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC',  
@type = N'SQL',  
@module_or_batch = NULL,   
@params = NULL,   
@hints = N'OPTION (MAXDOP 1)';  
```  
Como ejemplo adicional, considere la siguiente instrucción SQL enviada mediante [sp_executesql](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md).

```sql  
exec sp_executesql N'SELECT * FROM Sales.SalesOrderHeader
where SalesOrderID =  @so_id', N'@so_id int', @so_id = 43662;  
```  
 Para crear un plan único para cada ejecución de esta consulta, cree la guía de plan siguiente y use la sugerencia de consulta `OPTION (RECOMPILE)` en el parámetro `@hints`. 

```sql  
exec sp_create_plan_guide   
@name = N'PlanGuide1_SalesOrders',   
@stmt = N'SELECT * FROM Sales.SalesOrderHeader
where SalesOrderID =  @so_id',
@type = N'SQL',  
@module_or_batch = NULL,   
@params = N'@so_id int',   
@hints = N'OPTION (recompile)';
```

> [!IMPORTANT]  
>  Los valores que se proporcionan para los argumentos `@module_or_batch` y `@params` de la instrucción `sp_create_plan guide` deben coincidir con el texto correspondiente enviado en la consulta real. Para obtener más información, vea [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md) y [Usar SQL Server Profiler para crear y probar guías de plan](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md).  
  
 También se pueden crear guías de plan SQL en consultas que se parametrizan en el mismo formulario cuando se establece el valor de la opción SET de base de datos PARAMETERIZATION en FORCED, o cuando se crea una guía de plan TEMPLATE en la que se especifica que debe parametrizarse una clase de consultas.  
  
 ### <a name="template-plan-guide"></a>TEMPLATE, guía de plan  
 Una guía de plan TEMPLATE compara consultas independientes que se parametrizan en un formulario especificado. Estas guías de plan se usan para reemplazar la opción PARAMETERIZATION actual de una base de datos para una clase de consultas por medio de SET.  
  
 Puede crear una guía de plan TEMPLATE en cualquiera de las situaciones siguientes:  
  
-   Se ha establecido el valor de la opción PARAMETERIZATION de la base de datos en FORCED mediante el comando SET, pero hay consultas que quiere compilar según las reglas de la [parametrización SIMPLE](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).  
  
-   Se ha establecido el valor de la opción PARAMETERIZATION de la base de datos en SIMPLE (el valor predeterminado), pero quiere probar la [parametrización FORCED](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) en una clase de consultas.  
  
## <a name="plan-guide-matching-requirements"></a>Requisitos de coincidencia de la guía de plan  
 Las guías de plan tienen como ámbito la base de datos en la que se crean. Por tanto, solo se pueden buscar las coincidencias con la consulta de las guías de plan que existen en la base de datos actual cuando se ejecuta una consulta. Por ejemplo, si [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] es la base de datos actual y se ejecuta la consulta siguiente:  
  
 ```sql
 SELECT FirstName, LastName FROM Person.Person;
 ```  
  
 Solo las guías de plan de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] serán aptas para buscar las coincidencias con esta consulta. No obstante, si la base de datos actual es [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y se ejecutan las instrucciones siguientes:  
  
 ```sql
 USE DB1; 
 SELECT FirstName, LastName FROM Person.Person;
 ```  
  
 Solo las guías de plan de `DB1` serán aptas para buscar las coincidencias con la consulta, puesto que la consulta se ejecuta en el contexto de `DB1`.  
  
 En el caso de las guías de plan basadas en SQL o TEMPLATE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] examina los valores para los argumentos @module_or_batch y @params con una consulta, comparando ambos valores carácter a carácter. Esto significa que se debe proporcionar el texto exactamente como lo recibe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el lote real.  
  
 Cuando @type = 'SQL' y @module_or_batch se establece en NULL, el valor de @module_or_batch se establece en el valor de @stmt. Esto significa que el valor de *statement_text* debe proporcionarse en formato idéntico, carácter a carácter, en que se envía a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para facilitar esta concordancia no se realiza ninguna conversión interna.  
  
 Cuando una guía de plan normal (SQL u OBJECT) y una guía de plan TEMPLATE se pueden aplicar a una instrucción, solo se utilizará la guía de plan normal.  
  
> [!NOTE]  
>  El lote que contiene la instrucción en la que quiere crear una guía de plan no puede contener una instrucción USE *database* .  
  
## <a name="plan-guide-effect-on-the-plan-cache"></a>Efecto de la guía de plan en la caché del plan  
 Al crear una guía de plan en un módulo, se quita el plan de consulta para dicho módulo de la caché del plan. Al crear una guía de plan de tipo OBJECT o SQL en un lote, se quita el plan de consulta para un lote que tiene el mismo valor hash. Al crear una guía de plan de tipo TEMPLATE, se quitan todos los lotes de instrucción única de la memoria caché del plan dentro de esa base de datos.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Tarea|Tema|  
|----------|-----------|  
|Describe cómo crear una guía de plan.|[Crear una nueva guía de plan](../../relational-databases/performance/create-a-new-plan-guide.md)|  
|Describe cómo crear una guía de plan para consultas con parámetros.|[Crear una guía de plan para consultas con parámetros](../../relational-databases/performance/create-a-plan-guide-for-parameterized-queries.md)|  
|Describe cómo controlar el comportamiento de parametrización de consultas mediante guías de plan.|[Especificar el comportamiento de parametrización de consultas por medio de guías de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md)|  
|Describe cómo incluir un plan de consulta fijo en una guía de plan.|[Aplicar un plan de consulta fijo a una guía de plan](../../relational-databases/performance/apply-a-fixed-query-plan-to-a-plan-guide.md)|  
|Describe cómo especificar sugerencias de consulta en una guía de plan.|[Asociar sugerencias de consulta a una guía de plan](../../relational-databases/performance/attach-query-hints-to-a-plan-guide.md)|  
|Describe cómo ver las propiedades de la guía de plan.|[Ver propiedades de la guía de plan](../../relational-databases/performance/view-plan-guide-properties.md)|  
|Describe cómo usar SQL Server Profiler para crear y probar guías de plan.|[Usar SQL Server Profiler para crear y probar guías de plan](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md)|  
|Describe cómo validar las guías de plan.|[Validar guías de planes tras una actualización](../../relational-databases/performance/validate-plan-guides-after-upgrade.md)|  
  
## <a name="see-also"></a>Consulte también  
 [sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)   
 [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)   
 [sys.plan_guides &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [sys.fn_validate_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md)  
  
  
