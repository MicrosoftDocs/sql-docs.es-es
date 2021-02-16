---
description: 'Sugerencias (Transact-SQL): consulta'
title: Sugerencias de consulta (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/27/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Query_Hint_TSQL
- Query_TSQL
- Query
- Query Hint
- MAX_GRANT_PERCENT
- MAX_GRANT_PERCENT_TSQL
- MIN_GRANT_PERCENT
- MIN_GRANT_PERCENT_TSQL
- EXTERNALPUSHDOWN
- EXTERNALPUSHDOWN_TSQL
- NOLOCK_TSQL
- MAXDOP_TSQL
- USE_HINT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REPORT PLAN query hint
- FORCE ORDER query hint
- HASH JOIN query hint
- query hints [SQL Server]
- OPTIMIZE FOR query hint
- FORCESCAN query hint
- RECOMPILE query hint
- MAXRECURSION query hint
- MERGE JOIN query hint
- HASH GROUP query hint
- KEEP PLAN query hint
- UNION query hint
- FORCESEEK query hint
- ORDER GROUP query hint
- LOOP JOIN query hint
- KEEPFIXED PLAN query hint
- FAST query hint
- MAXDOP query hint
- PARAMETERIZATION query hint
- hints [SQL Server], query
- JOIN query hint
- USE PLAN query hint
- EXPAND VIEWS query hint
- MAX_GRANT_PERCENT query hint
- MIN_GRANT_PERCENT query hint
- EXTERNALPUSHDOWN query hint
- USE HINT query hint
- QUERY_PLAN_PROFILE query hint
ms.assetid: 66fb1520-dcdf-4aab-9ff1-7de8f79e5b2d
author: pmasl
ms.author: pelopes
ms.openlocfilehash: fd24bf1add1746687edfd86679813718050236ba
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341597"
---
# <a name="hints-transact-sql---query"></a>Sugerencias (Transact-SQL): consulta
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Las sugerencias de consulta especifican que las sugerencias indicadas se utilizan en el ámbito de una consulta. Afectan a todos los operadores de la instrucción. Si hay un argumento UNION implicado en la consulta principal, solo la última consulta que implique una operación UNION puede contener la cláusula OPTION. Las sugerencias de consulta se especifican como parte de la [cláusula OPTION](../../t-sql/queries/option-clause-transact-sql.md). El error 8622 se produce si una o varias sugerencias de consulta provocan que el optimizador de consultas no genere un plan válido.  
  
> [!CAUTION]  
> Como el optimizador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suele seleccionar el mejor plan de ejecución para las consultas, se recomienda que solo los desarrolladores y administradores de bases de datos experimentados usen estas sugerencias y que lo hagan como último recurso.  
  
**Se aplica a:**  
  
[DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
[INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
[SELECT](../../t-sql/queries/select-transact-sql.md)  
  
[UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
[MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
<query_hint> ::=   
{ { HASH | ORDER } GROUP   
  | { CONCAT | HASH | MERGE } UNION   
  | { LOOP | MERGE | HASH } JOIN   
  | EXPAND VIEWS   
  | FAST <integer_value>   
  | FORCE ORDER   
  | { FORCE | DISABLE } EXTERNALPUSHDOWN
  | { FORCE | DISABLE } SCALEOUTEXECUTION
  | IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX  
  | KEEP PLAN   
  | KEEPFIXED PLAN  
  | MAX_GRANT_PERCENT = <numeric_value>  
  | MIN_GRANT_PERCENT = <numeric_value>  
  | MAXDOP <integer_value>   
  | MAXRECURSION <integer_value>   
  | NO_PERFORMANCE_SPOOL   
  | OPTIMIZE FOR ( @variable_name { UNKNOWN | = <literal_constant> } [ , ...n ] )  
  | OPTIMIZE FOR UNKNOWN  
  | PARAMETERIZATION { SIMPLE | FORCED }   
  | QUERYTRACEON <integer_value>   
  | RECOMPILE  
  | ROBUST PLAN   
  | USE HINT ( <use_hint_name> [ , ...n ] )
  | USE PLAN N'<xml_plan>'  
  | TABLE HINT ( <exposed_object_name> [ , <table_hint> [ [, ]...n ] ] )  
}  
  
<table_hint> ::=  
{ NOEXPAND [ , INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> ) ]  
  | INDEX ( <index_value> [ ,...n ] ) | INDEX = ( <index_value> )
  | FORCESEEK [ ( <index_value> ( <index_column_name> [,... ] ) ) ]  
  | FORCESCAN  
  | HOLDLOCK   
  | NOLOCK   
  | NOWAIT  
  | PAGLOCK   
  | READCOMMITTED   
  | READCOMMITTEDLOCK   
  | READPAST   
  | READUNCOMMITTED   
  | REPEATABLEREAD   
  | ROWLOCK   
  | SERIALIZABLE   
  | SNAPSHOT  
  | SPATIAL_WINDOW_MAX_CELLS = <integer_value>  
  | TABLOCK   
  | TABLOCKX   
  | UPDLOCK   
  | XLOCK  
}  

<use_hint_name> ::=
{ 'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS'
  | 'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES'
  | 'DISABLE_BATCH_MODE_ADAPTIVE_JOINS'
  | 'DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_DEFERRED_COMPILATION_TV'
  | 'DISABLE_INTERLEAVED_EXECUTION_TVF'
  | 'DISABLE_OPTIMIZED_NESTED_LOOP'
  | 'DISABLE_OPTIMIZER_ROWGOAL'
  | 'DISABLE_PARAMETER_SNIFFING'
  | 'DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK'
  | 'DISABLE_TSQL_SCALAR_UDF_INLINING'
  | 'DISALLOW_BATCH_MODE'
  | 'ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS'
  | 'ENABLE_QUERY_OPTIMIZER_HOTFIXES'
  | 'FORCE_DEFAULT_CARDINALITY_ESTIMATION'
  | 'FORCE_LEGACY_CARDINALITY_ESTIMATION'
  | 'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'
  | 'QUERY_PLAN_PROFILE' 
}
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
{ HASH | ORDER } GROUP  
Especifica que las agregaciones que la cláusula GROUP BY o DISTINCT de la consulta describe deben usar hash o un orden.  
  
{ MERGE | HASH | CONCAT } UNION  
Especifica que todas las operaciones UNION se deben ejecutar mediante la combinación, hash o concatenación de conjuntos UNION. Si se especifica más de una sugerencia UNION, el optimizador de consultas seleccionará la estrategia menos costosa entre las sugerencias especificadas.  
  
{ LOOP | MERGE | HASH } JOIN  
Especifica que todas las operaciones de combinación se realicen mediante LOOP JOIN, MERGE JOIN o HASH JOIN en toda la consulta. Si especifica más de una sugerencia de combinación, el optimizador seleccionará la estrategia menos costosa de entre las permitidas.  
  
Si especifica una sugerencia de combinación en la cláusula FROM de la misma consulta para un par de tablas específico, esta sugerencia de combinación tendrá prioridad en la combinación de las dos tablas. Las sugerencias de consulta, sin embargo, todavía se deben respetar. La sugerencia de combinación para el par de tablas solo puede restringir la selección de los métodos de combinación permitidos en la sugerencia de consulta. Para obtener más información, vea [Sugerencias de combinación &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-join.md).  
  
EXPAND VIEWS  
Especifica que las vistas indexadas se expanden. También especifica que el optimizador de consultas no considerará ninguna vista indexada como reemplazo de ninguna parte de la consulta. Una vista se expande cuando la definición de la vista reemplaza el nombre de la vista en el texto de la consulta.  
  
Esta sugerencia de consulta virtualmente no permite el uso directo de vistas indizadas ni índices en vistas indizadas en el plan de consulta.  
  
> [!NOTE]
> La vista indexada sigue contraída si hay una referencia directa a la vista en la parte SELECT de la consulta. La vista también permanece contraída si se especifica WITH (NOEXPAND) o WITH (NOEXPAND, INDEX( _<index\_value>_ [ **,** _...n_ ] ) ). Para más información sobre la sugerencia de consulta NOEXPAND, vea [Uso de NOEXPAND](../../t-sql/queries/hints-transact-sql-table.md#using-noexpand).  
  
La sugerencia solo afecta a las vistas de la parte SELECT de las instrucciones, incluidas las vistas de las instrucciones INSERT, UPDATE, MERGE y DELETE.  
  
FAST _<integer\_value>_  
Especifica que la consulta está optimizada para una recuperación rápida de las primeras _<integer\_value>_ filas. Este resultado es un entero no negativo. Después de que se devuelven las primeras _<integer\_value>_ filas, la consulta continúa la ejecución y presenta su conjunto de resultados completo.  
  
FORCE ORDER  
Especifica que el orden de combinación que indica la sintaxis de la consulta se mantenga durante la optimización de la consulta. El uso de FORCE ORDER no afecta al posible comportamiento de inversión de roles del optimizador de consultas.  
  
> [!NOTE]  
> En una instrucción MERGE, se obtiene acceso a la tabla de origen antes que a la tabla de destino como el orden de combinación predeterminado, a menos que se especifique la cláusula WHEN SOURCE NOT MATCHED. Al especificar FORCE ORDER, se conserva este comportamiento predeterminado.  
  
{ FORCE | DISABLE } EXTERNALPUSHDOWN  
Fuerza o deshabilita la aplicación del cálculo de expresiones válidas en Hadoop. Solo se aplica a las consultas que usan PolyBase. No se aplicará a Azure Storage.  

{FORCE | DISABLE} SCALEOUTEXECUTION Fuerce o deshabilite la ejecución de escalado horizontal de consultas de PolyBase que usan tablas externas en Clústeres de macrodatos de SQL Server 2019. Esta sugerencia solo se respetará con una consulta que use la instancia maestra de un clúster de macrodatos de SQL. El escalado horizontal se producirá en todo el grupo de procesos del clúster de macrodatos. 

KEEP PLAN  
Fuerza al optimizador de consultas a aumentar el umbral estimado para volver a compilar una consulta. El umbral estimado para volver a compilar inicia una nueva compilación automática para la consulta cuando se ha realizado el número estimado de cambios de columnas indexados en una tabla mediante la ejecución de una de las siguientes instrucciones:

* UPDATE
* Delete
* MERGE
* INSERT

Al especificar KEEP PLAN, se asegura de que no se volverá a compilar una consulta con tanta frecuencia cuando se producen varias actualizaciones en una tabla.  
  
KEEPFIXED PLAN  
Fuerza al optimizador de consultas a no compilar de nuevo una consulta debido a cambios en las estadísticas. Al especificar KEEPFIXED PLAN, se asegura de que una consulta solo se vuelve a compilar si el esquema de las tablas subyacentes cambia o si **sp_recompile** se ejecuta en estas tablas.  
  
IGNORE_NONCLUSTERED_COLUMNSTORE_INDEX       
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]).  
  
Impide que la consulta use un índice no agrupado de almacén de columnas optimizado para memoria. Si la consulta contiene la sugerencia de consulta para evitar el uso del índice de almacén de columnas y una sugerencia de índice para usar un índice de almacén de columnas, las sugerencias están en conflicto y la consulta devuelve un error.  
  
MAX_GRANT_PERCENT = <numeric_value>     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  

El tamaño máximo de concesión de memoria en porcentaje del límite de memoria configurado. Se garantiza que la consulta no superará este límite. El límite real puede ser menor si la configuración de Resource Governor es inferior al valor especificado por esta sugerencia. Los valores válidos están comprendidos entre 0,0 y 100,0.  
  
MIN_GRANT_PERCENT = <numeric_value>        
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   

El tamaño mínimo de concesión de memoria en porcentaje del límite de memoria configurado. Se garantiza que la consulta obtendrá `MAX(required memory, min grant)` porque se requiere al menos la memoria necesaria para iniciar una consulta. Los valores válidos están comprendidos entre 0,0 y 100,0.  
 
MAXDOP <integer_value>      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].  
  
Invalida la opción de configuración de **grado máximo de paralelismo** de **sp_configure**. También invalida Resource Governor para la consulta que especifica esta opción. La sugerencia de consulta MAXDOP puede superar el valor configurado con sp_configure. Si MAXDOP supera el valor configurado con Resource Governor, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa el valor MAXDOP de Resource Governor, descrito en [ALTER WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/alter-workload-group-transact-sql.md). Se pueden aplicar todas las reglas semánticas usadas con la opción de configuración **Grado máximo de paralelismo** cuando se usa la sugerencia de consulta MAXDOP. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
  
> [!WARNING]     
> Si MAXDOP se establece en cero, el servidor elige el grado máximo de paralelismo.  
  
MAXRECURSION <integer_value>     
Especifica el número máximo de recursiones permitidas para esta consulta. _number_ es un entero no negativo entre 0 y 32 767. Cuando se especifica 0, no se aplica ningún límite. Si no se especifica esta opción, el límite predeterminado para el servidor es 100.  
  
Cuando se alcanza el número predeterminado o especificado para el límite de MAXRECURSION durante la ejecución de la consulta, dicha consulta finaliza y se devuelve un error.  
  
Debido a este error, todos los efectos de la instrucción se revierten. Si la instrucción es una instrucción SELECT, es posible que no se devuelva ningún resultado o que los resultados sean parciales. Puede que los resultados parciales no incluyan todas las filas de los niveles de recursividad que superen el nivel de recursividad máximo especificado.  
  
Para más información, vea [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).     
  
NO_PERFORMANCE_SPOOL    
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
  
Impide que se agregue un operador de cola de impresión a planes de consulta (excepto a los planes en los que se requiere que la cola de impresión garantice una semántica de actualización válida). En algunos escenarios, el operador de cola de impresión puede reducir el rendimiento. Por ejemplo, la cola de impresión usa tempdb, y se puede producir la contención de tempdb si se ejecutan muchas consultas simultáneas con las operaciones de cola de impresión.  
  
OPTIMIZE FOR ( _\@variable\_name_ { UNKNOWN | = <literal_constant> }_ [ **,** ..._n_ ] )     
Indica al optimizador de consultas que utilice un valor concreto para una variable local cuando la consulta se compila y optimiza. El valor se utiliza solo durante la optimización de la consulta y no durante la ejecución de la misma.  
  
_\@nombre\_de variable_  
Es el nombre de una variable local que se utiliza en una consulta, a la que se puede asignar un valor para utilizarlo con la sugerencia de consulta OPTIMIZE FOR.  
  
_UNKNOWN_  
Indica al optimizador de consultas que use datos estadísticos en lugar del valor inicial para determinar el valor de una variable local durante la optimización de la consulta.  
  
_literal\_constant_  
Es un valor constante literal al que se asigna _\@nombre\_de variable_ para su uso con la sugerencia de consulta OPTIMIZE FOR. _constante\_literal_ se usa solo durante la optimización de la consulta y no como el valor de _\@nombre\_de variable_ durante la ejecución de la consulta. _literal\_constant_ puede tener cualquier tipo de datos de sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se pueda expresar como una constante literal. El tipo de datos de _constante\_literal_ se debe convertir de forma implícita al tipo de datos al que _\@nombre\_de variable_ hace referencia en la consulta.  
  
OPTIMIZE FOR puede contrarrestar el comportamiento de detección de parámetros predeterminado del optimizador. Use también OPTIMIZE FOR para crear guías de plan. Para más información, vea [Volver a compilar un procedimiento almacenado](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md).  
  
OPTIMIZE FOR UNKNOWN  
Indica al optimizador de consultas que use la selectividad promedio del predicado en todos los valores de columna en lugar del valor del parámetro en tiempo de ejecución al compilar y optimizar la consulta.  
  
Si usa OPTIMIZE FOR @variable_name = _literal\_constant_ y OPTIMIZE FOR UNKNOWN en la misma sugerencia de consulta, el optimizador de consultas usará el valor _literal\_constant_ especificado para un valor determinado. El optimizador de consultas usará UNKNOWN para los valores de las variables restantes. Los valores se usan solo durante la optimización de la consulta y no durante la ejecución de la misma.  

PARAMETERIZATION { SIMPLE | FORCED }     
Especifica las reglas de parametrización que aplica el optimizador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando se compila la consulta.  
  
> [!IMPORTANT]  
> La sugerencia de consulta PARAMETERIZATION solo se puede especificar en una guía de plan para invalidar la configuración actual de la opción SET de base de datos PARAMETERIZATION. No se puede especificar directamente en una consulta.    
> Para más información, vea [Especificar el comportamiento de parametrización de consultas por medio de guías de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md).
  
SIMPLE indica al optimizador de consultas que intente la parametrización simple. FORCED indica al optimizador de consultas que intente la parametrización forzada. Para más información, vea [Parametrización forzada en la guía de arquitectura de procesamiento de consultas](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) y [Parametrización simple en la guía de arquitectura de procesamiento de consultas](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).  

QUERYTRACEON <integer_value>    
Esta opción permite habilitar una marca de seguimiento que afecte al plan solo durante la compilación de una única consulta. Como sucede con otras opciones de nivel de consulta, se puede usar junto con guías de plan para hacer coincidir el texto de una consulta que se ejecuta desde cualquier sesión y aplicar automáticamente una marca de seguimiento que afecte al plan al compilar esta consulta. La opción QUERYTRACEON solo se admite en las marcas de seguimiento del optimizador de consultas. Para obtener más información, vea [Marcas de seguimiento](../database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). 

> [!NOTE]  
> Esta opción no devolverá ningún error ni advertencia si se usa un número de marca de seguimiento no admitido. Si la marca de seguimiento especificada no afecta a un plan de ejecución de consulta, la opción se omitirá de forma silenciosa.

> [!NOTE]  
> Para usar más de una marca de seguimiento en una consulta, especifique una sugerencia QUERYTRACEON para cada número de marca de seguimiento diferente.

RECOMPILE  
Indica a [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que genere un plan nuevo y temporal para la consulta y descarte de inmediato ese plan una vez que se completa la ejecución de la consulta. El plan de consulta generado no reemplaza un plan almacenado en caché cuando la misma consulta se ejecuta sin la sugerencia RECOMPILE. Sin especificar RECOMPILE, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] almacena en la memoria caché planes de consulta y los reutiliza. Al compilar los planes de consulta, la sugerencia de consulta RECOMPILE utiliza los valores actuales de cualquier variable local en la consulta. Si la consulta está dentro de un procedimiento almacenado, los valores actuales se pasan a cualquier parámetro.  
  
RECOMPILE es una alternativa útil a la creación de un procedimiento almacenado. RECOMPILE utiliza la cláusula WITH RECOMPILE cuando solo se debe volver a compilar un subconjunto de consultas del procedimiento almacenado, en lugar de todo el procedimiento almacenado. Para más información, vea [Volver a compilar un procedimiento almacenado](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md). RECOMPILE también es útil al crear guías de plan.  
  
ROBUST PLAN  
Fuerza al optimizador de consultas a intentar aplicar un plan que funcione para el tamaño máximo de fila posible en detrimento del rendimiento. Cuando se procesa la consulta, es posible que las tablas intermedias y los operadores necesiten guardar y procesar filas más anchas que las filas de entrada cuando la consulta se procesa. Las filas pueden llegar a ser tan anchas que, en algunos casos, el operador especificado no puede procesar la fila. Si las filas son tan anchas, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error durante la ejecución de la consulta. Mediante la utilización de ROBUST PLAN, puede indicar al optimizador de consultas que no tenga en cuenta los planes de consulta que puedan encontrarse con este problema.  
  
Si no es posible realizar tal plan, el optimizador de consultas devuelve un error en lugar de diferir la detección de errores hasta la ejecución de la consulta. Las filas pueden contener columnas de longitud variable; el [!INCLUDE[ssDE](../../includes/ssde-md.md)] permite definir filas con un tamaño potencial máximo que supere la capacidad del [!INCLUDE[ssDE](../../includes/ssde-md.md)] para procesarlas. Normalmente, a pesar del tamaño potencial máximo, una aplicación almacena filas cuyo tamaño real se encuentra dentro de los límites que puede procesar el [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Si el [!INCLUDE[ssDE](../../includes/ssde-md.md)] se encuentra con una fila demasiado larga, devuelve un error de ejecución.  
 
<a name="use_hint"></a> USE HINT ( **'** _hint\_name_ **'** )    
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].
 
Proporciona una o varias sugerencias adicionales para el procesador de consultas. Las sugerencias adicionales se especifican mediante un nombre de la sugerencia **dentro de comillas simples**.   

Se admiten los siguientes nombres de sugerencia:    
 
*  'ASSUME_JOIN_PREDICATE_DEPENDS_ON_FILTERS' <a name="use_hint_join_containment"></a>       
   Hace que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genere un plan de consulta mediante la hipótesis de contención simple en lugar de la hipótesis de contención de base predeterminada para las combinaciones en el modelo de [estimación de la cardinalidad](../../relational-databases/performance/cardinality-estimation-sql-server.md) de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o versiones más recientes. Es equivalente a la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9476. 
*  'ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES' <a name="use_hint_correlation"></a>      
   Hace que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genere un plan con una selectividad mínima al estimar predicados AND para que los filtros tengan en cuenta la correlación. Es equivalente a la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4137 cuando se usa con el modelo de estimación de cardinalidad de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones anteriores, y tiene un efecto similar cuando se usa la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9471 con el modelo de estimación de cardinalidad de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o versiones posteriores.
*  "DISABLE_BATCH_MODE_ADAPTIVE_JOINS"       
   Deshabilita las combinaciones adaptables del modo por lotes. Para obtener más información, vea [Combinaciones adaptables del modo por lotes](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-adaptive-joins).     
   **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  "DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK"       
   Deshabilita los comentarios de concesión de memoria en modo por lotes. Para obtener más información, vea [Comentarios de concesión de memoria de modo de proceso por lotes](../../relational-databases/performance/intelligent-query-processing.md#batch-mode-memory-grant-feedback).     
   **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
* "DISABLE_DEFERRED_COMPILATION_TV"    
  Deshabilita la compilación diferida de variables de tabla. Para obtener más información, consulte [Compilación diferida de variables de tabla](../../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation).     
  **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  "DISABLE_INTERLEAVED_EXECUTION_TVF"      
   Deshabilita la ejecución intercalada de las funciones con valores de tabla de múltiples instrucciones. Para más información, consulte [Ejecución intercalada de funciones con valores de tabla de múltiples instrucciones](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).     
   **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].   
*  "DISABLE_OPTIMIZED_NESTED_LOOP"      
   Indica al procesador de consultas que no use una operación de ordenación (ordenación por lotes) para las combinaciones de bucle anidado optimizadas cuando se genera un plan de consulta. Es equivalente a la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 2340.
*  'DISABLE_OPTIMIZER_ROWGOAL' <a name="use_hint_rowgoal"></a>      
   Hace que SQL Server genere un plan que no usa las modificaciones del objetivo de fila con las consultas que contienen estas palabras clave: 
   
   * TOP
   * OPTION (FAST N)
   * IN
   * EXISTS
   
   Es equivalente a la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4138.
*  "DISABLE_PARAMETER_SNIFFING"      
   Indica al optimizador de consultas que utilice el promedio de distribución de datos al compilar una consulta con uno o más parámetros. Esta instrucción hace que el plan de consulta sea independiente en el valor del parámetro que se utilizó en primer lugar cuando se compiló la consulta. Es equivalente a la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4136 o a la opción `PARAMETER_SNIFFING = OFF` de [Configuración de ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
* "DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK"    
  Deshabilita los comentarios de concesión de memoria del modo de fila. Para más información, consulte [Comentarios de concesión de memoria de modo de proceso por lotes](../../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback).      
  **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
* "DISABLE_TSQL_SCALAR_UDF_INLINING"    
  Deshabilita la inserción de UDF escalar. Para obtener más información, vea [Scalar UDF inlining](../../relational-databases/user-defined-functions/scalar-udf-inlining.md) (Inserción de UDF escalar).     
  **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]).    
* "DISALLOW_BATCH_MODE"    
  Deshabilita la ejecución del modo por lotes. Para más información, consulte [Modos de ejecución](../../relational-databases/query-processing-architecture-guide.md#execution-modes).     
  **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].     
*  "ENABLE_HIST_AMENDMENT_FOR_ASC_KEYS"      
   Habilita las estadísticas rápidas generadas automáticamente (modificación de histograma) para las columnas de índice iniciales para las que se necesite la estimación de cardinalidad. El histograma usado para calcular la cardinalidad se ajustará en tiempo de compilación de la consulta para tener en cuenta el valor máximo o mínimo real de esta columna. Es equivalente a la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4139. 
*  "ENABLE_QUERY_OPTIMIZER_HOTFIXES"     
   Permite revisiones del optimizador de consultas (cambios publicados en las actualizaciones acumulativas y Service Packs de SQL Server). Es equivalente a la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4199 o a la opción `QUERY_OPTIMIZER_HOTFIXES = ON` de [Configuración de ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
*  "FORCE_DEFAULT_CARDINALITY_ESTIMATION"      
   Fuerza al optimizador de consultas a usar el modelo de [estimación de la cardinalidad](../../relational-databases/performance/cardinality-estimation-sql-server.md) que se corresponde con el nivel de compatibilidad de la base de datos actual. Use esta sugerencia para invalidar la opción `LEGACY_CARDINALITY_ESTIMATION = ON` de [Configuración de ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) o la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481.
*  'FORCE_LEGACY_CARDINALITY_ESTIMATION' <a name="use_hint_ce70"></a>      
   Fuerza al optimizador de consultas a usar el modelo de [estimación de la cardinalidad](../../relational-databases/performance/cardinality-estimation-sql-server.md) de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones anteriores. Es equivalente a la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 9481 o a la opción `LEGACY_CARDINALITY_ESTIMATION = ON` de [Configuración de ámbito de base de datos](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
*  'QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n'          
 Fuerza el comportamiento del optimizador de consultas en un nivel de consulta. Este comportamiento se produce como si la consulta se compilara con nivel de compatibilidad de base de datos _n_, donde _n_ es un nivel de compatibilidad de base de datos admitido (por ejemplo, 100, 130, etc.). Consulte [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md) para ver una lista de los valores admitidos actualmente para _n_.      
   **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU10).    

   > [!NOTE]
   > La sugerencia QUERY_OPTIMIZER_COMPATIBILITY_LEVEL_n no invalida la configuración de la estimación de cardinalidad heredada o predeterminada, si se fuerza a través de la configuración con ámbito de base de datos, marca de seguimiento u otra sugerencia de consulta como QUERYTRACEON.   
   > Esta sugerencia solo afecta al comportamiento del optimizador de consultas. No afecta a otras características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que pueden depender del [nivel de compatibilidad de base de datos](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md), como la disponibilidad de determinadas características de base de datos.  
   > Para más información sobre esta sugerencia, vea [Developer's Choice: Hinting Query Execution model](/archive/blogs/sql_server_team/developers-choice-hinting-query-execution-model) (Elección del desarrollador: modelo de ejecución de consultas de sugerencias).
    
*  'QUERY_PLAN_PROFILE'      
 Habilita la generación de perfiles ligera para la consulta. Cuando finaliza una consulta que contiene esta nueva sugerencia, se activa un nuevo evento extendido: query_plan_profile. Este evento extendido expone las estadísticas de ejecución y el plan de ejecución real XML similar al evento extendido query_post_execution_showplan, pero solo para las consultas que contiene la nueva sugerencia.    
   **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU3 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU11). 

   > [!NOTE]
   > Si habilita la recopilación del evento extendido query_post_execution_showplan, se agregará la infraestructura de generación de perfiles estándar para todas las consultas que se están ejecutando en el servidor, por lo que podría resultar afectado el rendimiento global del servidor.      
   > Si habilita la recopilación del evento extendido *query_thread_profile* para usar en su lugar la generación de perfiles ligera, habrá una sobrecarga de rendimiento mucho mejor, pero seguirá resultando afectado el rendimiento global del servidor.       
   > Si habilita el evento extendido query_plan_profile, solo se habilitará la infraestructura ligera de generación de perfiles para una consulta que se ejecutó con QUERY_PLAN_PROFILE, con lo que las otras cargas de trabajo del servidor no resultarán afectadas. Use esta sugerencia para generar perfiles de una consulta específica sin que esto afecte a otras partes de la carga de trabajo del servidor.
   > Para más información sobre la generación de perfiles de baja intensidad, vea [Infraestructura de generación de perfiles de consultas](../../relational-databases/performance/query-profiling-infrastructure.md).
 
Puede consultar la lista de todos los nombres de USE HINT compatibles mediante la vista de administración dinámica [sys.dm_exec_valid_use_hints](../../relational-databases/system-dynamic-management-views/sys-dm-exec-valid-use-hints-transact-sql.md).    

> [!TIP]
> Los nombres de sugerencia no distinguen mayúsculas de minúsculas.   
  
> [!IMPORTANT] 
> Algunas sugerencias USE HINT pueden entrar en conflicto con las marcas de seguimiento habilitadas a nivel global o de sesión, o con las opciones de configuración con ámbito de base de datos. En este caso, la sugerencia de nivel de consulta (USE HINT) siempre tiene prioridad. Si una sugerencia USE HINT entra en conflicto con otra sugerencia de consulta o una marca de seguimiento habilitada en el nivel de consulta (por ejemplo, mediante QUERYTRACEON), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generará un error al intentar ejecutar la consulta. 

<a name="use-plan"></a> USE PLAN N' _<xml\_plan>_ '  
 Fuerza al optimizador de consultas a usar un plan de consulta existente en una consulta especificada por **"** _xml\_plan_ **"** . USE PLAN no puede especificarse con las instrucciones INSERT, UPDATE, MERGE ni DELETE.  
  
TABLE HINT **(** _<exposed\_object\_name>_ [ **,** \<table_hint> [ [ **,** ]..._n_ ] ] **)** Aplica la sugerencia de la tabla especificada a la tabla o vista que corresponde a _exposed\_object\_name_. Se recomienda usar una sugerencia de tabla como una sugerencia de consulta únicamente en el contexto de una [guía de plan](../../relational-databases/performance/plan-guides.md).  
  
 _<exposed\_object\_name>_ puede ser una de las referencias siguientes:  
  
-   Cuando se usa un alias para la tabla o vista en la cláusula [FROM](../../t-sql/queries/from-transact-sql.md) de la consulta, el alias es _exposed\_object\_name_.  
  
-   Cuando no se usa un alias, _exposed\_object\_name_ es la coincidencia exacta de la tabla o vista a la que se hace referencia en la cláusula FROM. Por ejemplo, si se hace referencia a la tabla o vista con un nombre de dos partes, _exposed\_object\_name_ es el mismo nombre de dos partes.  
  
 Al especificar _exposed\_object\_name_ sin especificar también una sugerencia de tabla, se descartan todos los índices que especifique en la consulta como parte de una sugerencia de tabla para el objeto. Después, el optimizador de consultas determina el uso de los índices. Puede emplear esta técnica para eliminar el efecto de una sugerencia de tabla INDEX cuando no se puede modificar la consulta original. Vea el ejemplo J.  
  
**\<table_hint> ::=** { NOEXPAND [ , INDEX ( _<index\_value>_ [ ,...n ] ) | INDEX = ( _<index\_value>_ ) ] \| INDEX ( _<index\_value>_ [ ,...n ] ) | INDEX = ( _<index\_value>_ ) \| FORCESEEK [ **(** _<index\_value>_ **(** _<index\_column\_name>_ [ **,** ... ] **))** ] \| FORCESCAN \| HOLDLOCK \| NOLOCK \| NOWAIT \| PAGLOCK \| READCOMMITTED \| READCOMMITTEDLOCK \| READPAST \| READUNCOMMITTED \| REPEATABLEREAD \| ROWLOCK \| SERIALIZABLE \| SNAPSHOT \| SPATIAL_WINDOW_MAX_CELLS = _<integer\_value>_ \| TABLOCK \| TABLOCKX \| UPDLOCK \| XLOCK }    
Es la sugerencia de tabla que se aplica a la tabla o vista que corresponde a *exposed_object_name* como una sugerencia de consulta. Para obtener una descripción de estas sugerencias, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
 Las sugerencias de tabla distintas de INDEX, FORCESCAN y FORCESEEK no están permitidas como sugerencias de consulta, a menos que la consulta ya tenga una cláusula WITH que especifique la sugerencia de tabla. Para obtener más información, vea la [sección Notas](#remarks).  
  
> [!CAUTION]
> Al especificar FORCESEEK con parámetros se limita el número de planes que el optimizador de consultas puede considerar en comparación con cuando se especifica FORCESEEK sin parámetros. Esto puede producir un error "No se puede generar el plan" en más casos. En una versión futura, las modificaciones internas realizadas en el optimizador de consultas pueden permitir que se consideren más planes.  
  
## <a name="remarks"></a>Observaciones  
 No se pueden especificar sugerencias de consulta en una instrucción INSERT, excepto cuando se usa una cláusula SELECT en la instrucción.  
  
 Solo se pueden especificar sugerencias de consulta en la consulta de nivel superior, no en las subconsultas. Cuando se especifica una sugerencia de tabla como una sugerencia de consulta, la sugerencia puede especificarse en la consulta de nivel superior o en una subconsulta. Sin embargo, el valor especificado para _<exposed\_object\_name>_ en la cláusula TABLE HINT debe coincidir exactamente con el nombre expuesto en la consulta o subconsulta.  
  
## <a name="specifying-table-hints-as-query-hints"></a>Especificar sugerencias de tabla como sugerencias de consulta  
 Se recomienda usar la sugerencia de tabla INDEX, FORCESCAN o FORCESEEK como sugerencia de consulta únicamente en el contexto de una [guía de plan](../../relational-databases/performance/plan-guides.md). Las guías de plan son útiles cuando no se puede modificar la consulta original, por ejemplo, porque es una aplicación de otro fabricante. La sugerencia de consulta especificada en la guía de plan se agrega a la consulta antes de compilarla y optimizarla. Para las consultas ad hoc, utilice la cláusula TABLE HINT únicamente en las pruebas de instrucciones de guías de plan. Para todas las demás consultas ad hoc, se recomienda especificar estas sugerencias únicamente como sugerencias de tabla.  
  
 Cuando se especifican como una sugerencia de consulta, las sugerencias de tabla INDEX, FORCESCAN y FORCESEEK son válidas para los objetos siguientes:  
  
-   Tablas  
-   Vistas  
-   Vistas indizadas  
-   Expresiones de tabla comunes (la sugerencia se debe especificar en la instrucción SELECT cuyo conjunto de resultados rellena la expresión de tabla común)  
-   Vistas de administración dinámica (DMV)
-   Subconsultas con nombre  
  
Puede especificar las sugerencias de tabla INDEX, FORCESCAN, y FORCESEEK como sugerencias de consulta para una consulta que no tenga ninguna sugerencia de tabla existente. También puede utilizarlas para reemplazar las sugerencias INDEX, FORCESCAN o FORCESEEK existentes en la consulta, respectivamente. 

Las sugerencias de tabla distintas de INDEX, FORCESCAN y FORCESEEK no están permitidas como sugerencias de consulta, a menos que la consulta ya tenga una cláusula WITH que especifique la sugerencia de tabla. En este caso, también debe especificarse una sugerencia coincidente como sugerencia de consulta. Especifique la sugerencia coincidente como sugerencia de consulta mediante TABLE HINT en la cláusula OPTION. Esta especificación conserva la semántica de la consulta. Por ejemplo, si la consulta contiene la sugerencia de tabla NOLOCK, la cláusula OPTION del parámetro **\@hints** de la guía de plan también debe contener la sugerencia NOLOCK. Vea el ejemplo K. 
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-merge-join"></a>A. Usar MERGE JOIN  
 En el siguiente ejemplo se especifica que una combinación MERGE JOIN ejecuta la operación JOIN de la consulta. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT *   
FROM Sales.Customer AS c  
INNER JOIN Sales.CustomerAddress AS ca ON c.CustomerID = ca.CustomerID  
WHERE TerritoryID = 5  
OPTION (MERGE JOIN);  
GO    
```  
  
### <a name="b-using-optimize-for"></a>B. Usar OPTIMIZE FOR  
 En el ejemplo siguiente se indica al optimizador de consultas que use el valor `'Seattle'` para `@city_name` y la selectividad promedio del predicado en todos los valores de columna para `@postal_code` al optimizar la consulta. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
CREATE PROCEDURE dbo.RetrievePersonAddress
@city_name NVARCHAR(30),  
 @postal_code NVARCHAR(15)
AS
SELECT * FROM Person.Address  
WHERE City = @city_name AND PostalCode = @postal_code  
OPTION ( OPTIMIZE FOR (@city_name = 'Seattle', @postal_code UNKNOWN) );  
GO
```  
  
### <a name="c-using-maxrecursion"></a>C. Usar MAXRECURSION  
 MAXRECURSION se puede utilizar para impedir que una expresión de tabla común recursiva con formato incorrecto entre en un bucle infinito. En el ejemplo siguiente se crea un bucle infinito intencionadamente y se usa la sugerencia MAXRECURSION para limitar el número de niveles de recursividad a dos. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
--Creates an infinite loop  
WITH cte (CustomerID, PersonID, StoreID) AS  
(  
    SELECT CustomerID, PersonID, StoreID  
    FROM Sales.Customer  
    WHERE PersonID IS NOT NULL  
  UNION ALL  
    SELECT cte.CustomerID, cte.PersonID, cte.StoreID  
    FROM cte   
    JOIN  Sales.Customer AS e   
        ON cte.PersonID = e.CustomerID  
)  
--Uses MAXRECURSION to limit the recursive levels to 2  
SELECT CustomerID, PersonID, StoreID  
FROM cte  
OPTION (MAXRECURSION 2);  
GO  
```  
  
 Después de corregir el error de código, ya no se requiere MAXRECURSION.  
  
### <a name="d-using-merge-union"></a>D. Usar MERGE UNION  
 En el ejemplo siguiente se usa la sugerencia de consulta MERGE UNION. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT *  
FROM HumanResources.Employee AS e1  
UNION  
SELECT *  
FROM HumanResources.Employee AS e2  
OPTION (MERGE UNION);  
GO  
```  
  
### <a name="e-using-hash-group-and-fast"></a>E. Usar HASH GROUP y FAST  
 En el ejemplo siguiente se usan las sugerencias de consulta HASH GROUP y FAST. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO    
```  
  
### <a name="f-using-maxdop"></a>F. Usar MAXDOP  
 En el ejemplo siguiente se usa la sugerencia de consulta MAXDOP. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (MAXDOP 2);    
GO
```  
  
### <a name="g-using-index"></a>G. Usar INDEX  
 Los ejemplos siguientes usan la sugerencia INDEX. El primer ejemplo especifica un índice único. El segundo ejemplo especifica varios índices para obtener una única referencia de tabla. En ambos ejemplos, como la sugerencia INDEX se aplica en una tabla que usa un alias, la cláusula TABLE HINT también debe especificar el mismo alias que el nombre del objeto expuesto. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide1',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX (IX_Employee_ManagerID)))';  
GO  
EXEC sp_create_plan_guide   
    @name = N'Guide2',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e, INDEX(PK_Employee_EmployeeID, IX_Employee_ManagerID)))';  
GO    
```  
  
### <a name="h-using-forceseek"></a>H. Usar FORCESEEK  
 En el siguiente ejemplo se utiliza la sugerencia de tabla FORCESEEK. La cláusula TABLE HINT también debe especificar el mismo nombre de dos partes como el nombre del objeto expuesto. Especifique el nombre cuando aplique la sugerencia INDEX en una tabla que usa un nombre de dos partes. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide3',   
    @stmt = N'SELECT c.LastName, c.FirstName, HumanResources.Employee.Title  
              FROM HumanResources.Employee  
              JOIN Person.Contact AS c ON HumanResources.Employee.ContactID = c.ContactID  
              WHERE HumanResources.Employee.ManagerID = 3  
              ORDER BY c.LastName, c.FirstName;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT( HumanResources.Employee, FORCESEEK))';  
GO    
```  
  
### <a name="i-using-multiple-table-hints"></a>I. Usar varias sugerencias de tabla  
 El ejemplo siguiente aplica la sugerencia INDEX a una tabla y la sugerencia FORCESEEK a otra. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide4',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX( IX_Employee_ManagerID))   
                       , TABLE HINT (c, FORCESEEK))';  
GO  
```  
  
### <a name="j-using-table-hint-to-override-an-existing-table-hint"></a>J. Usar TABLE HINT para invalidar una sugerencia de tabla existente  
En el ejemplo siguiente se muestra cómo usar la sugerencia TABLE HINT. Puede usar la sugerencia sin especificar una sugerencia para invalidar el comportamiento de sugerencia de tabla INDEX que especifica en la cláusula FROM de la consulta. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide5',   
    @stmt = N'SELECT e.ManagerID, c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e WITH (INDEX (IX_Employee_ManagerID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',   
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT(e))';  
GO    
```  
  
### <a name="k-specifying-semantics-affecting-table-hints"></a>K. Especificar sugerencias de tabla que afectan a la semántica  
El ejemplo siguiente presenta dos sugerencias de tabla en la consulta: NOLOCK, que afecta a la semántica, e INDEX, que no la afecta. Para conservar la semántica de la consulta, la sugerencia NOLOCK se especifica en la cláusula OPTIONS de la guía de plan. Junto con la sugerencia NOLOCK, especifique las sugerencias INDEX y FORCESEEK y reemplace la sugerencia INDEX que no afecta a la semántica en la consulta durante la compilación y optimización de la instrucción. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide6',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 3;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, INDEX(IX_Employee_ManagerID), NOLOCK, FORCESEEK))';  
GO    
```  
  
El ejemplo siguiente muestra un método alternativo para conservar la semántica de la consulta y permitir al optimizador elegir un índice distinto del índice que se especificó en la sugerencia de tabla. Permita que el optimizador elija especificando la sugerencia NOLOCK en la cláusula OPTIONS. Especifique la sugerencia porque afecta a la semántica. A continuación, especifique la palabra clave TABLE HINT con solo una referencia de tabla y ninguna sugerencia INDEX. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
EXEC sp_create_plan_guide   
    @name = N'Guide7',   
    @stmt = N'SELECT c.LastName, c.FirstName, e.Title  
              FROM HumanResources.Employee AS e   
                   WITH (NOLOCK, INDEX (PK_Employee_EmployeeID))  
              JOIN Person.Contact AS c ON e.ContactID = c.ContactID  
              WHERE e.ManagerID = 2;',  
    @type = N'SQL',  
    @module_or_batch = NULL,   
    @params = NULL,   
    @hints = N'OPTION (TABLE HINT (e, NOLOCK))';  
GO  
```  

### <a name="l-using-use-hint"></a>L. Usar la sugerencia USE HINT  
 En el ejemplo siguiente se usan las sugerencias de consulta RECOMPILE y USE HINT. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (RECOMPILE, USE HINT ('ASSUME_MIN_SELECTIVITY_FOR_FILTER_ESTIMATES', 'DISABLE_PARAMETER_SNIFFING')); 
GO  
```  

### <a name="m-using-querytraceon-hint"></a>M. Uso de QUERYTRACEON HINT  
 En el ejemplo siguiente se usan las sugerencias de consulta de QUERYTRACEON. En el ejemplo se usa la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Puede habilitar todas las revisiones que afectan al plan controladas por la marca de seguimiento 4199 para una consulta determinada mediante la consulta siguiente:
  
```sql  
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (QUERYTRACEON 4199);
```  

 También puede usar varias marcas de seguimiento como en la consulta siguiente:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION  (QUERYTRACEON 4199, QUERYTRACEON 4137);
```

## <a name="see-also"></a>Consulte también  
[Hints &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
[sp_create_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
[sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md)  
[Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)       
[Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)      
