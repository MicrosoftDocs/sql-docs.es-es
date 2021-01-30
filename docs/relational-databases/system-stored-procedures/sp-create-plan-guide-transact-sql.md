---
description: sp_create_plan_guide (Transact-SQL)
title: sp_create_plan_guide (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_plan_guide
- sp_create_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_create_plan_guide
ms.assetid: 5a8c8040-4f96-4c74-93ab-15bdefd132f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0203cbe0539d0733740cf495aa0c149409468fa8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205221"
---
# <a name="sp_create_plan_guide-transact-sql"></a>sp_create_plan_guide (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea una guía de plan para asociar sugerencias de consulta o planes de consulta actuales a las consultas de una base de datos. Para obtener más información acerca de las guías de plan, vea [Plan Guides](../../relational-databases/performance/plan-guides.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_create_plan_guide [ @name = ] N'plan_guide_name'  
    , [ @stmt = ] N'statement_text'  
    , [ @type = ] N'{ OBJECT | SQL | TEMPLATE }'  
    , [ @module_or_batch = ]  
      {   
                    N'[ schema_name. ] object_name'  
        | N'batch_text'  
        | NULL  
      }  
    , [ @params = ] { N'@parameter_name data_type [ ,...n ]' | NULL }   
    , [ @hints = ] { N'OPTION ( query_hint [ ,...n ] )'   
                 | N'XML_showplan'  
                 | NULL }  
```  
  
## <a name="arguments"></a>Argumentos  
 [ \@ Name =] N '*plan_guide_name*'  
 Es el nombre de la guía de plan. Los nombres de guía de plan se encuentran en el ámbito de la base de datos actual. *plan_guide_name* debe cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md) y no puede comenzar por el signo de número (#). La longitud máxima de *plan_guide_name* es de 124 caracteres.  
  
 [ \@ stmt =] N '*statement_text*'  
 Es una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] que se va a utilizar en la creación de una guía de plan. Cuando el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] optimizador de consultas reconoce una consulta que coincide con *statement_text*, *plan_guide_name* surte efecto. Para que la creación de una guía de plan sea correcta, *statement_text* debe aparecer en el contexto especificado por los \@ parámetros Type, \@ module_or_batch y \@ params.  
  
 *statement_text* debe proporcionarse de forma que permita que el optimizador de consultas coincida con la instrucción correspondiente suministrada dentro del lote o módulo identificado por \@ module_or_batch y los \@ parámetros. Para obtener más información, vea la sección "Comentarios". El tamaño de *statement_text* solo está limitado por la memoria disponible del servidor.  
  
 [ \@ type =] N ' {Object | SQL | PLANTILLA} '  
 Es el tipo de entidad en la que aparece *statement_text* . Especifica el contexto para la coincidencia de *statement_text* que se va a *plan_guide_name*.  
  
 OBJECT  
 Indica *statement_text* aparece en el contexto de un [!INCLUDE[tsql](../../includes/tsql-md.md)] procedimiento almacenado, una función escalar, una función con valores de tabla de múltiples instrucciones o un [!INCLUDE[tsql](../../includes/tsql-md.md)] desencadenador DML en la base de datos actual.  
  
 SQL  
 Indica *statement_text* aparece en el contexto de un lote o instrucción independiente que se puede enviar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de cualquier mecanismo. [!INCLUDE[tsql](../../includes/tsql-md.md)] las instrucciones enviadas por objetos de Common Language Runtime (CLR) o procedimientos almacenados extendidos, o mediante el uso de EXEC N '*sql_string*', se procesan como lotes en el servidor y, por tanto, deben identificarse como \@ tipo **=** ' SQL '. Si se especifica SQL, la PARAMETRIZAción de la sugerencia de consulta {forzada | SIMPLE} no se puede especificar en el \@ parámetro hints.  
  
 TEMPLATE  
 Indica que la guía de plan se aplica a cualquier consulta que parametriza al formulario indicado en *statement_text*. Si se especifica TEMPLATE, solo la PARAMETRIZAción {forzada | SIMPLE} la sugerencia de consulta se puede especificar en el \@ parámetro hints. Para obtener más información acerca de las guías de plan de plantilla, vea [especificar el comportamiento de parametrización de consultas mediante guías de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md).  
  
 [ \@ module_or_batch =] {N ' [ *schema_name*. ] *object_name*' | N '*batch_text*' | ACEPTA  
 Especifica el nombre del objeto en el que aparece *statement_text* o el texto del lote en el que aparece *statement_text* . El texto del lote no puede incluir una instrucción USE *Database* .  
  
 Para que una guía de plan coincida con un lote enviado desde una aplicación, se debe proporcionar *batch_tex* t en el mismo formato, carácter a carácter, al que se envía [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para facilitar esta concordancia no se realiza ninguna conversión interna. Para obtener más información, vea la sección Comentarios.  
  
 [*schema_name*.] *object_name* especifica el nombre de un [!INCLUDE[tsql](../../includes/tsql-md.md)] procedimiento almacenado, una función escalar, una función con valores de tabla de múltiples instrucciones o un [!INCLUDE[tsql](../../includes/tsql-md.md)] desencadenador DML que contiene *statement_text*. Si no se especifica *schema_name* , *schema_name* utiliza el esquema del usuario actual. Si se especifica NULL y \@ type = ' SQL ', el valor de \@ module_or_batch se establece en el valor de \@ stmt. Si \@ type = ' template **\'** , \@ MODULE_OR_BATCH debe ser null.  
  
 [ \@ params =] {N '*\@ parameter_name data_type* [,*... n* ] ' | ACEPTA  
 Especifica las definiciones de todos los parámetros que están incrustados en *statement_text*. \@los parámetros solo se aplican cuando se cumple alguna de las siguientes condiciones:  
  
-   \@Type = ' SQL ' o ' TEMPLATE '. Si es ' TEMPLATE ', los \@ parámetros no deben ser null.  
  
-   *statement_text* se envía mediante sp_executesql y se especifica un valor para el \@ parámetro params, o bien [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] envía internamente una instrucción después de la parametrización. Las consultas con parámetros enviadas desde las API de base de datos (incluidas ODBC, OLE DB y ADO.NET) se muestran en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como llamadas a sp_executesql o a rutinas de cursor de servidor API; por tanto, se pueden comparar con guías de plan de tipo SQL o TEMPLATE.  
  
 *\@ parameter_name data_type* se debe proporcionar exactamente en el mismo formato que se envía a mediante [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sp_executesql o se envía internamente después de la parametrización. Para obtener más información, vea la sección Comentarios. Si el lote no contiene parámetros, debe especificarse NULL. El tamaño de los \@ parámetros solo está limitado por la memoria del servidor disponible.  
  
 [ \@ hints =] {N'OPTION (*query_hint* [,*... n* ]) ' | N '*XML_showplan*' | ACEPTA  
 N'OPTION (*query_hint* [,*... n* ])  
 Especifica una cláusula OPTION que se va a adjuntar a una consulta que coincide con \@ stmt. \@ las sugerencias deben ser sintácticamente iguales que una cláusula OPTION en una instrucción SELECT y pueden contener cualquier secuencia válida de sugerencias de consulta.  
  
 N '*XML_showplan*'  
 Es el plan de consulta en formato XML que se va a aplicar como una sugerencia.  
  
 Se recomienda asignar el plan de presentación XML a una variable; de lo contrario, hay que anteponer una comilla sencilla a de cada comilla sencilla presente en el plan de presentación. Vea el ejemplo E.  
  
 NULL  
 Indica que no se aplica a la consulta ninguna sugerencia existente especificada en la cláusula OPTION de la consulta. Para obtener más información, vea [cláusula OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones  
 Los argumentos de sp_create_plan_guide deben indicarse en el orden que se muestra. Cuando se incluyen valores para los parámetros de **sp_create_plan_guide**, deben especificarse todos los nombres de parámetro de forma explícita, o bien no especificarse ninguno. Por ejemplo, si se especifica **\@ Name =** , también se debe especificar **\@ stmt =** , **\@ Type =**, etc. Del mismo modo, si se omite **\@ Name =** y solo se proporciona el valor del parámetro, también deben omitirse los demás nombres de parámetro y solo se proporcionarán sus valores. Los nombres de argumento solo se incluyen con fines de descripción, para ayudar a entender la sintaxis. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no comprueba si el nombre del parámetro especificado coincide con el nombre del parámetro en la posición donde se utiliza.  
  
 Puede crearse más de una guía de plan OBJECT o SQL para la misma consulta y lote o módulo. Sin embargo, en un momento dado, solo puede estar habilitada una guía de plan.  
  
 No se pueden crear guías de plan de tipo OBJECT para un \@ valor module_or_batch que hace referencia a un procedimiento almacenado, una función o un desencadenador DML que especifica la cláusula with Encryption o que es temporal.  
  
 Se producirá un error si se intenta quitar o modificar una función, procedimiento almacenado o desencadenador DML al que una guía de plan, habilitada o deshabilitada, haga referencia. También se producirá un error si se intenta quitar una tabla que tenga definido un desencadenador al que haga referencia una guía de plan.  
  
> [!NOTE]
> Las guías de plan no se pueden usar en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md). Las guías de plan son visibles en todas las ediciones. También se pueden adjuntar bases de datos que incluyen guías de plan a cualquier versión. Las guías de plan permanecen intactas cuando se restaura o adjunta una base de datos a una versión actualizada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Después de realizar una actualización de servidor, debe comprobar la idoneidad de las guías de plan en cada base de datos.  
  
## <a name="plan-guide-matching-requirements"></a>Requisitos de coincidencia de la guía de plan  
 En el caso de las guías de plan que especifican \@ type = ' SQL ' o \@ type = ' template ' para que coincidan correctamente con una consulta, los valores de *batch_text* y *\@ parameter_name data_type* [,*... n* ] debe proporcionarse exactamente en el mismo formato que sus homólogos enviados por la aplicación. Esto significa que es necesario suministrar el texto del lote exactamente como lo recibe el compilador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para capturar el texto real del lote y del parámetro, se puede utilizar el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Para obtener más información, vea [usar SQL Server Profiler para crear y probar guías de plan](../../relational-databases/performance/use-sql-server-profiler-to-create-and-test-plan-guides.md).  
  
 Cuando \@ type = ' SQL ' y \@ module_or_batch está establecido en null, el valor de \@ module_or_batch se establece en el valor de \@ stmt. Esto significa que el valor de *statement_text* debe proporcionarse exactamente en el mismo formato, carácter a carácter, a medida que se envía a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para facilitar esta concordancia no se realiza ninguna conversión interna.  
  
 Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] coincida con el valor de *statement_text* para *batch_text* y *\@ parameter_name data_type* [,*... n* ], o si \@ Type = **\'** Object ', al texto de la consulta correspondiente dentro de *object_name*, no se tienen en cuenta los siguientes elementos de cadena:  
  
-   Los caracteres de espacio en blanco (tabulaciones, espacios, retornos de carro o avances de línea) dentro de una cadena.  
  
-   Comentarios ( **--** o **/\*   \*/** ).  
  
-   Los signos de punto y coma al final.  
  
 Por ejemplo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede hacer coincidir la cadena de *statement_text* `N'SELECT * FROM T WHERE a = 10'` con la *batch_text* siguiente:  
  
 ```
 N'SELECT *
 FROM T
 WHERE a = 10' 
 ```
 
 Sin embargo, la misma cadena no coincidiría con esta *batch_text*:  
  
 `N'SELECT * FROM T WHERE b = 10'`  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] omite los caracteres de retorno de carro, avance de línea y espacio dentro de la primera consulta. En la segunda consulta, la secuencia `WHERE b = 10` se interpreta de manera diferente que `WHERE a = 10`. En la comparación se distinguen mayúsculas, minúsculas y acentos (a pesar de que en la intercalación de la base de datos no se hace distinción entre mayúsculas y minúsculas), excepto en el caso de las palabras clave, donde no se hace distinción entre mayúsculas y minúsculas. En la comparación no se tienen en cuenta las formas abreviadas de las palabras clave. Por ejemplo, las palabras clave `EXECUTE`, `EXEC` y `execute` se consideran equivalentes.  
  
## <a name="plan-guide-effect-on-the-plan-cache"></a>Efecto de la guía de plan en la memoria caché del plan  
 Al crear una guía de plan en un módulo, se quita el plan de consulta para dicho módulo de la caché del plan. Al crear una guía de plan de tipo OBJECT o SQL en un lote, se quita el plan de consulta para un lote que tiene el mismo valor hash. Al crear una guía de plan de tipo TEMPLATE, se quitan todos los lotes de instrucción única de la memoria caché del plan dentro de esa base de datos.  
  
## <a name="permissions"></a>Permisos  
 Para crear una guía de plan de tipo OBJECT, se requiere `ALTER` el permiso en el objeto al que se hace referencia. Para crear una guía de plan de tipo SQL o TEMPLATE, se requiere `ALTER` el permiso en la base de datos actual.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-plan-guide-of-type-object-for-a-query-in-a-stored-procedure"></a>A. Crear una guía de plan de tipo OBJECT para una consulta en un procedimiento almacenado  
 En el ejemplo siguiente se crea una guía de plan que coincide con una consulta ejecutada en el contexto de un procedimiento almacenado basado en aplicación, y se aplica la sugerencia `OPTIMIZE FOR` a la consulta.  
  
 Éste es el procedimiento almacenado:  
  
```sql  
IF OBJECT_ID(N'Sales.GetSalesOrderByCountry', N'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetSalesOrderByCountry;  
GO  
CREATE PROCEDURE Sales.GetSalesOrderByCountry   
    (@Country_region nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h   
    INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
    INNER JOIN Sales.SalesTerritory AS t   
        ON c.TerritoryID = t.TerritoryID  
    WHERE t.CountryRegionCode = @Country_region;  
END  
GO  
```  
  
 Ésta es la guía de plan creada en la consulta del procedimiento almacenado:  
  
```sql  
EXEC sp_create_plan_guide   
    @name =  N'Guide1',  
    @stmt = N'SELECT *  
              FROM Sales.SalesOrderHeader AS h   
              INNER JOIN Sales.Customer AS c   
                 ON h.CustomerID = c.CustomerID  
              INNER JOIN Sales.SalesTerritory AS t   
                 ON c.TerritoryID = t.TerritoryID  
              WHERE t.CountryRegionCode = @Country_region',  
    @type = N'OBJECT',  
    @module_or_batch = N'Sales.GetSalesOrderByCountry',  
    @params = NULL,  
    @hints = N'OPTION (OPTIMIZE FOR (@Country_region = N''US''))';  
```  
  
### <a name="b-creating-a-plan-guide-of-type-sql-for-a-stand-alone-query"></a>B. Crear una guía de plan de tipo SQL para una consulta independiente  
 En el ejemplo siguiente se crea una guía de plan que coincide con una consulta de un lote enviado por una aplicación que utiliza el procedimiento almacenado del sistema sp_executesql.  
  
 Este es el lote:  
  
```sql  
SELECT TOP 1 * FROM Sales.SalesOrderHeader ORDER BY OrderDate DESC;  
```  
  
 Para evitar que se genere un plan de ejecución en paralelo en esta consulta, cree la siguiente guía de plan:  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT TOP 1 *   
              FROM Sales.SalesOrderHeader   
              ORDER BY OrderDate DESC',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (MAXDOP 1)';  
```  
  
### <a name="c-creating-a-plan-guide-of-type-template-for-the-parameterized-form-of-a-query"></a>C. Crear una guía de plan de tipo TEMPLATE para una consulta con parámetros  
 En el ejemplo siguiente se crea una guía de plan que coincida con cualquier consulta que se parametrice de una forma específica, e indica a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que fuerce la parametrización de la consulta. Las dos consultas siguientes son equivalentes desde el punto de vista sintáctico, pero se diferencian solo en los valores literales de las constantes.  
  
```sql  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45639;  
  
SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
    ON h.SalesOrderID = d.SalesOrderID  
WHERE h.SalesOrderID = 45640;  
```  
  
 Ésta es una guía de plan creada en la consulta con parámetros:  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'TemplateGuide1',  
    @stmt = N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
              INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
                  ON h.SalesOrderID = d.SalesOrderID  
              WHERE h.SalesOrderID = @0',  
    @type = N'TEMPLATE',  
    @module_or_batch = NULL,  
    @params = N'@0 int',  
    @hints = N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
 En el ejemplo anterior, el valor del parámetro `@stmt` representa la forma de la consulta con parámetros. La única manera confiable de obtener este valor para usarlo en sp_create_plan_guide es recurrir al procedimiento almacenado del sistema [sp_get_query_template](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md) . El script siguiente se puede utilizar para obtener la consulta con parámetros y, después crear una guía de plan basada en ella.  
  
```sql  
DECLARE @stmt nvarchar(max);  
DECLARE @params nvarchar(max);  
EXEC sp_get_query_template   
    N'SELECT * FROM AdventureWorks2012.Sales.SalesOrderHeader AS h  
      INNER JOIN AdventureWorks2012.Sales.SalesOrderDetail AS d   
          ON h.SalesOrderID = d.SalesOrderID  
      WHERE h.SalesOrderID = 45639;',  
    @stmt OUTPUT,   
    @params OUTPUT  
EXEC sp_create_plan_guide N'TemplateGuide1',   
    @stmt,   
    N'TEMPLATE',   
    NULL,   
    @params,   
    N'OPTION(PARAMETERIZATION FORCED)';  
```  
  
> [!IMPORTANT]  
>  El valor de los literales de constante del parámetro `@stmt` pasado a sp_get_query_template podría afectar al tipo de datos elegido para el parámetro que reemplaza al valor literal. Esto afectaría a la correspondencia de la guía de plan. Puede que tenga que crear más de una guía de plan para abarcar los distintos intervalos de valores de parámetros.  
  
### <a name="d-creating-a-plan-guide-on-a-query-submitted-by-using-an-api-cursor-request"></a>D. Crear una guía de plan en una consulta enviada mediante una solicitud de cursor API  
 Las guías de plan pueden coincidir con consultas enviadas desde rutinas de cursor de servidor API. Estas rutinas son sp_cursorprepare, sp_cursorprepexec y sp_cursoropen. Las aplicaciones que utilizan las API de ADO, OLE DB y ODBC interactúan frecuentemente con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante cursores de servidor API. Para ver la invocación de las rutinas de cursor de servidor API en los seguimientos del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], vea el evento de seguimiento de analizador RPC:Starting.  
  
 Supongamos que aparecen los datos siguientes en un evento de seguimiento de analizador RPC:Starting para una consulta que desea optimizar con una guía de plan:  
  
```sql  
DECLARE @p1 int;  
SET @p1=-1;  
DECLARE @p2 int;  
SET @p2=0;  
DECLARE @p5 int;  
SET @p5=4104;  
DECLARE @p6 int;  
SET @p6=8193;  
DECLARE @p7 int;  
SET @p7=0;  
EXEC sp_cursorprepexec @p1 output,@p2 output,N'@P1 varchar(255),@P2 varchar(255)',N'SELECT * FROM Sales.SalesOrderHeader AS h INNER JOIN Sales.SalesOrderDetail AS d ON h.SalesOrderID = d.SalesOrderID WHERE h.OrderDate BETWEEN @P1 AND @P2',@p5 OUTPUT,@p6 OUTPUT,@p7 OUTPUT,'20040101','20050101'  
SELECT @p1, @p2, @p5, @p6, @p7;  
```  
  
 Como puede observar, el plan de la consulta `SELECT` en la llamada a `sp_cursorprepexec` utiliza una combinación de mezcla, pero desea utilizar una combinación hash. La consulta enviada mediante `sp_cursorprepexec` contiene parámetros, e incluye una cadena de consulta y una cadena de parámetro. Puede crear la siguiente guía de plan para cambiar la opción de plan utilizando las cadenas de consulta y parámetro exactamente como aparecen, carácter por carácter, en la llamada a `sp_cursorprepexec`.  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'APICursorGuide',  
    @stmt = N'SELECT * FROM Sales.SalesOrderHeader AS h   
              INNER JOIN Sales.SalesOrderDetail AS d   
                ON h.SalesOrderID = d.SalesOrderID   
              WHERE h.OrderDate BETWEEN @P1 AND @P2',  
    @type = N'SQL',  
    @module_or_batch = NULL,  
    @params = N'@P1 varchar(255),@P2 varchar(255)',  
    @hints = N'OPTION(HASH JOIN)';  
```  
  
 Cuando la aplicación vuelva a ejecutar esta consulta en el futuro, dicha ejecución se verá afectada por esta guía de plan, y se utilizará una combinación hash para procesar la consulta.  
  
### <a name="e-creating-a-plan-guide-by-obtaining-the-xml-showplan-from-a-cached-plan"></a>E. Crear una guía de plan mediante la obtención del plan de presentación XML a partir de un plan en caché  
 El ejemplo siguiente crea una guía de plan para una instrucción SQL ad hoc sencilla. El plan de consulta deseado para esta instrucción se proporciona en la guía de plan si se especifica el plan de presentación XML para la consulta directamente en el parámetro `@hints` . El ejemplo ejecuta primero la instrucción SQL para generar un plan en la memoria caché del plan. Para los fines de este ejemplo, se supone que el plan generado es el plan deseado y que no se requiere ninguna optimización adicional de la consulta. El plan de presentación XML para la consulta se obtiene consultando las vistas de administración dinámica `sys.dm_exec_query_stats`, `sys.dm_exec_sql_text`y `sys.dm_exec_text_query_plan` y está asignado a la variable `@xml_showplan` . A continuación, la variable `@xml_showplan` se pasa a la instrucción `sp_create_plan_guide` en el parámetro `@hints` . O bien, puede crear una guía de plan a partir de un plan de consulta de la memoria caché del plan con el procedimiento almacenado [sp_create_plan_guide_from_handle](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md) .  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;  
GO  
DECLARE @xml_showplan nvarchar(max);  
SET @xml_showplan = (SELECT query_plan  
    FROM sys.dm_exec_query_stats AS qs   
    CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
    CROSS APPLY sys.dm_exec_text_query_plan(qs.plan_handle, DEFAULT, DEFAULT) AS qp  
    WHERE st.text LIKE N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;%');  
  
EXEC sp_create_plan_guide   
    @name = N'Guide1_from_XML_showplan',   
    @stmt = N'SELECT City, StateProvinceID, PostalCode FROM Person.Address ORDER BY PostalCode DESC;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints =@xml_showplan;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Guías de plan](../../relational-databases/performance/plan-guides.md)   
 [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)   
 [sys.plan_guides &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)   
 [sys.dm_exec_cached_plans &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md)   
 [sys.dm_exec_query_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)   
 [sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)   
 [sys.fn_validate_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-validate-plan-guide-transact-sql.md)   
 [sp_get_query_template &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-get-query-template-transact-sql.md)  
  
  
