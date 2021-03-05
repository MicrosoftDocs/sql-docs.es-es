---
title: Cláusula FROM más JOIN, APPLY y PIVOT (T-SQL)
description: Cláusula FROM más JOIN, APPLY, PIVOT (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- JOIN
- FROM_TSQL
- FROM
- JOIN_TSQL
- OUTER_JOIN_TSQL
- INNER_JOIN_TSQL
- CROSS_TSQL
- CROSS_APPLY_TSQL
- APPLY_TSQL
- CROSS_JOIN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OUTER APPLY operator
- hints [SQL Server], FROM clause
- SELECT statement [SQL Server], FROM clause
- ISO syntax
- DELETE statement [SQL Server], FROM clause
- CROSS APPLY operator
- FROM clause
- APPLY operator
- joins [SQL Server], FROM clause
- UPDATE statement [SQL Server], FROM clause
- derived tables
ms.assetid: 36b19e68-94f6-4539-aeb1-79f5312e4263
author: VanMSFT
ms.author: vanto
ms.custom: ''
ms.date: 06/01/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 03bd9ad046be57d9fedea50ae310956b26a32581
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186496"
---
# <a name="from-clause-plus-join-apply-pivot-transact-sql"></a>Cláusula FROM más JOIN, APPLY, PIVOT (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

En Transact-SQL, la cláusula FROM está disponible en las siguientes instrucciones:

- [DELETE](../statements/delete-transact-sql.md)
- [UPDATE](update-transact-sql.md)
- [SELECT](select-transact-sql.md)

Normalmente, la cláusula FROM es necesaria en la instrucción SELECT. La excepción se produce cuando no hay columnas de tablas enumeradas, y los únicos elementos que se muestran son literales, variables o expresiones aritméticas.

En este artículo también se tratan las siguientes palabras clave, que se pueden usar en la cláusula FROM:

- [JOIN](../../relational-databases/performance/joins.md)
- APPLY
- [PIVOT](from-using-pivot-and-unpivot.md)

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ FROM { <table_source> } [ ,...n ] ]   
<table_source> ::=   
{  
    table_or_view_name [ FOR SYSTEM_TIME <system_time> ] [ AS ] table_alias ]   
        [ <tablesample_clause> ]   
        [ WITH ( < table_hint > [ [ , ]...n ] ) ]   
    | rowset_function [ [ AS ] table_alias ]   
        [ ( bulk_column_alias [ ,...n ] ) ]   
    | user_defined_function [ [ AS ] table_alias ]  
    | OPENXML <openxml_clause>   
    | derived_table [ [ AS ] table_alias ] [ ( column_alias [ ,...n ] ) ]   
    | <joined_table>   
    | <pivoted_table>   
    | <unpivoted_table>  
    | @variable [ [ AS ] table_alias ]  
    | @variable.function_call ( expression [ ,...n ] )   
        [ [ AS ] table_alias ] [ (column_alias [ ,...n ] ) ]     
}  
<tablesample_clause> ::=  
    TABLESAMPLE [SYSTEM] ( sample_number [ PERCENT | ROWS ] )   
        [ REPEATABLE ( repeat_seed ) ]   
  
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON <search_condition>   
    | <table_source> CROSS JOIN <table_source>   
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
<join_type> ::=   
    [ { INNER | { { LEFT | RIGHT | FULL } [ OUTER ] } } [ <join_hint> ] ]  
    JOIN  
  
<pivoted_table> ::=  
    table_source PIVOT <pivot_clause> [ [ AS ] table_alias ]  
  
<pivot_clause> ::=  
        ( aggregate_function ( value_column [ [ , ]...n ])   
        FOR pivot_column   
        IN ( <column_list> )   
    )   
  
<unpivoted_table> ::=  
    table_source UNPIVOT <unpivot_clause> [ [ AS ] table_alias ]  
  
<unpivot_clause> ::=  
    ( value_column FOR pivot_column IN ( <column_list> ) )   
  
<column_list> ::=  
    column_name [ ,...n ]   
  
<system_time> ::=  
{  
       AS OF <date_time>  
    |  FROM <start_date_time> TO <end_date_time>  
    |  BETWEEN <start_date_time> AND <end_date_time>  
    |  CONTAINED IN (<start_date_time> , <end_date_time>)   
    |  ALL  
}  
  
    <date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <start_date_time>::=  
        <date_time_literal> | @date_time_variable  
  
    <end_date_time>::=  
        <date_time_literal> | @date_time_variable  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
FROM { <table_source> [ ,...n ] }  
  
<table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [<tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
    | <joined_table>  
}  
  
<tablesample_clause> ::=
    TABLESAMPLE ( sample_number [ PERCENT ] ) -- Azure Synapse Analytics Dedicated SQL pool only  
 
<joined_table> ::=   
{  
    <table_source> <join_type> <table_source> ON search_condition   
    | <table_source> CROSS JOIN <table_source> 
    | left_table_source { CROSS | OUTER } APPLY right_table_source   
    | [ ( ] <joined_table> [ ) ]   
}  
  
<join_type> ::=   
    [ INNER ] [ <join hint> ] JOIN  
    | LEFT  [ OUTER ] JOIN  
    | RIGHT [ OUTER ] JOIN  
    | FULL  [ OUTER ] JOIN  
  
<join_hint> ::=   
    REDUCE  
    | REPLICATE  
    | REDISTRIBUTE  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
\<table_source>  
 Especifica el origen de una tabla, una vista, una tabla variable o una tabla derivada, con o sin alias, para utilizarlo en la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]. Se pueden utilizar hasta 256 orígenes de tabla en una instrucción, aunque el límite varía en función de la memoria disponible y de la complejidad del resto de las expresiones de la consulta. Las consultas individuales pueden no admitir un máximo de 256 orígenes de tabla.  
  
> [!NOTE]  
>  El rendimiento de las consultas se puede ver afectado si se hace referencia a un número elevado de tablas en ellas. El tiempo de compilación y optimización también se puede ver afectado por factores adicionales. Estos incluyen la presencia de índices y vistas indexadas en cada \<table_source> y el tamaño de \<select_list> en la instrucción SELECT.  
  
 El orden de los orígenes de tabla después de la palabra clave FROM no afecta al conjunto de resultados devuelto. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve errores si aparecen nombres duplicados en la cláusula FROM.  
  
 *table_or_view_name*  
 Es el nombre de una tabla o una vista.  
  
 Si la tabla o la vista existen en otra base de datos de la misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], use un nombre completo con el formato *database*.*schema*.*object_name*.  
  
 Si la tabla o la vista existen fuera de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], use un nombre de cuatro partes con el formato *linked_server*.*catalog*.*schema*.*object*. Para obtener más información, vea [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Un nombre de cuatro partes creado con la función [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) como la parte de servidor del nombre también se puede usar para especificar el origen de tabla remoto. Cuando se especifica OPENDATASOURCE, es posible que *database_name* y *schema_name* no se apliquen a todos los orígenes de datos y dependan de las capacidades del proveedor OLE DB que accede al objeto remoto.  
  
 [AS] *table_alias*  
 Es un alias para *table_source* que se puede usar por comodidad o para distinguir una tabla o una vista en una autocombinación o una subconsulta. El alias suele ser un nombre de tabla abreviado que se utiliza para hacer referencia a columnas específicas de las tablas en una combinación. Si el mismo nombre de columna existe en más de una tabla en una combinación, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere que el nombre de columna sea calificado mediante un nombre de tabla, un nombre de vista o un alias. No se puede utilizar el nombre de la tabla si se ha definido un alias.  
  
 Si se usa una tabla derivada, una función de conjuntos de filas o con valores de tabla, o una cláusula de operador (como PIVOT o UNPIVOT), el parámetro *table_alias* requerido al final de la cláusula es el nombre de tabla asociado para todas las columnas devueltas, incluidas las columnas de agrupación.  
  
 WITH (\<table_hint> )  
 Especifica que el optimizador de consultas utiliza una estrategia de optimización o bloqueo con esta tabla y para esta instrucción. Para obtener más información, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
*rowset_function*  
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
Especifica una de las funciones de conjuntos de filas, como OPENROWSET, que devuelve un objeto que se puede utilizar en lugar de una referencia de tabla. Para obtener más información sobre la lista de funciones de conjuntos de filas, vea [Funciones de conjuntos de filas &#40;Transact-SQL&#41;](../functions/opendatasource-transact-sql.md).  
  
 El uso de las funciones OPENROWSET y OPENQUERY para especificar que un objeto remoto depende de las capacidades del proveedor OLE DB que tiene acceso al objeto.  
  
 *bulk_column_alias*  
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
 Es un alias opcional para sustituir el nombre de una columna en el conjunto de resultados. Los alias de columna se permiten solo en las instrucciones SELECT que utilizan la función OPENROWSET con la opción BULK. Si usa *bulk_column_alias*, especifique un alias para cada columna de tabla en el mismo orden que las columnas del archivo.  
  
> [!NOTE]  
>  Este alias invalida al atributo NAME de los elementos COLUMN de un archivo de formato XML si está presente.  
  
 *user_defined_function*  
 Especifica una función con valores de tabla.  
  
OPENXML \<openxml_clause>  
**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
Proporciona una vista de un conjunto de filas en un documento XML. Para obtener más información, vea [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md).  
  
 *derived_table*  
 Es una subconsulta que recupera filas de la base de datos. *derived_table* se usa como entrada para la consulta externa.  
  
 *derived_table* puede usar la característica de constructor con valores de tabla de [!INCLUDE[tsql](../../includes/tsql-md.md)] para especificar varias filas. Por ejemplo, `SELECT * FROM (VALUES (1, 2), (3, 4), (5, 6), (7, 8), (9, 10) ) AS MyTable(a, b);`. Para más información, vea [Constructor con valores de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md).  
  
 *column_alias*  
 Es un alias opcional para sustituir el nombre de una columna en el conjunto de resultados de la tabla derivada. Incluya un alias de columna para cada columna de la lista de selección y delimite la lista de alias de columna con paréntesis.  
  
 *nombre_de_tabla_o_vista* FOR SYSTEM_TIME \<system_time> se aplica a **: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
Especifica que se devuelva una versión determinada de los datos de la tabla temporal especificada y su tabla de historial de versiones del sistema vinculada.  
  
### <a name="tablesample-clause"></a>Cláusula Tablesample
**Se aplica a:** SQL Server y SQL Database. Especifica que se devuelve un ejemplo de los datos de la tabla. El ejemplo puede ser aproximado. Esta cláusula se puede usar en cualquier tabla principal o combinada de una instrucción SELECT o UPDATE. TABLESAMPLE no se puede especificar con vistas.  
  
> [!NOTE]  
>  Cuando se utiliza TABLESAMPLE en bases de datos que se actualizan a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el nivel de compatibilidad de la base de datos se establece en 110 o superior y no se permite PIVOT en una consulta de expresión de tabla común (CTE) recursiva. Para obtener más información, vea [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
 SYSTEM  
 Se trata de un método de muestreo dependiente de la implementación especificado por los estándares ISO. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], es el único método de muestreo disponible y se aplica de forma predeterminada. SYSTEM aplica un método de muestreo basado en páginas en el que se elige un conjunto de páginas aleatorio de la tabla para el ejemplo y todas las filas de dichas páginas se devuelven como el subconjunto de ejemplo.  
  
 *sample_number*  
 Es una expresión numérica constante exacta o aproximada que representa el porcentaje o el número de filas. Si se especifica con PERCENT, *sample_number* se convierte de forma implícita en un valor **float**; en caso contrario, se convierte en **bigint**. PERCENT es el valor predeterminado.  
  
 PERCENT  
 Especifica que se debe recuperar de la tabla el porcentaje *sample_number* de filas. Si se especifica PERCENT, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un valor aproximado del porcentaje especificado. Si se especifica PERCENT, la expresión *sample_number* debe evaluarse como un valor comprendido entre 0 y 100.  
  
 ROWS  
 Especifica que se recupere aproximadamente el valor *sample_number* de filas. Si se especifica ROWS, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve una aproximación del número de filas especificado. Si se especifica ROWS, la expresión *sample_number* debe evaluarse como un valor entero mayor que cero.  
  
 REPEATABLE  
 Indica que el ejemplo seleccionado se puede devolver de nuevo. Si se especifica con el mismo valor de *repeat_seed*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve el mismo subconjunto de filas, siempre que no se hayan realizado cambios en las filas de la tabla. Si se especifica con otro valor de *repeat_seed*, es probable que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelva una muestra distinta de filas de la tabla. Se consideran cambios en la tabla las siguientes acciones: insertar, actualizar, eliminar, volver a generar o desfragmentar índices, y restaurar o adjuntar bases de datos.  
  
 *repeat_seed*  
 Es una expresión de tipo entero constante utilizada por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para generar un número aleatorio. *repeat_seed* es **bigint**. Si no se especifica *repeat_seed*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna un valor de forma aleatoria. Para un valor *repeat_seed* específico, el resultado del muestreo es siempre el mismo si no se ha aplicado ningún cambio a la tabla. La expresión *repeat_seed* debe evaluarse como un entero mayor que cero.  
  
### <a name="tablesample-clause"></a>Cláusula Tablesample
**Se aplica a:** [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].

 Especifica que se devuelva un ejemplo de los datos de la tabla. El ejemplo puede ser aproximado. Esta cláusula se puede usar en cualquier tabla principal o combinada de una instrucción SELECT o UPDATE. TABLESAMPLE no se puede especificar con vistas. 

 PERCENT  
 Especifica que se debe recuperar de la tabla el porcentaje *sample_number* de filas. Si se especifica PERCENT, [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] devuelve un valor aproximado del porcentaje especificado. Si se especifica PERCENT, la expresión *sample_number* debe evaluarse como un valor comprendido entre 0 y 100.  


### <a name="joined-table"></a>Tabla combinada 
Las tablas combinadas son un conjunto de resultados producto de dos o más tablas. Para varias combinaciones, utilice paréntesis para cambiar el orden natural de las combinaciones.  
  
### <a name="join-type"></a>Tipo de combinación
Especifica el tipo de operación de combinación.  
  
 INNER  
 Especifica que se devuelvan todos los pares de filas coincidentes. Rechaza las filas no coincidentes de las dos tablas. Si no se especifica ningún tipo de combinación, éste es el valor predeterminado.  
  
 FULL [ OUTER ]  
 Especifica que una fila de la tabla de la derecha o de la izquierda, que no cumpla la condición de combinación, se incluya en el conjunto de resultados y que las columnas que correspondan a la otra tabla se establezcan en NULL. De esta forma se agregan más filas a las que se suelen devolver con INNER JOIN.  
  
 LEFT [ OUTER ]  
 Especifica que todas las filas de la tabla izquierda que no cumplan la condición de combinación se incluyan en el conjunto de resultados, con las columnas de resultados de la otra tabla establecidas en NULL, además de todas las filas devueltas por la combinación interna.  
  
 RIGHT [OUTER]  
 Especifica que todas las filas de la tabla derecha que no cumplan la condición de combinación se incluyan en el conjunto de resultados, con las columnas de resultados de la otra tabla establecidas en NULL, además de todas las filas devueltas por la combinación interna.  
  
### <a name="join-hint"></a>Sugerencia de combinación  
En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)], especifica que el optimizador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe usar una sugerencia de combinación o un algoritmo de ejecución por cada combinación especificada en la cláusula FROM de la consulta. Para obtener más información, vea [Sugerencias de combinación &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-join.md).  
  
 En [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], estas sugerencias de combinación se aplican a combinaciones INNER en dos columnas no compatibles de distribución. Para mejorar el rendimiento de las consultas, puede restringir la cantidad de movimiento de datos que se produce durante el procesamiento de consultas. Las sugerencias de combinación permitidas para [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] son:  
  
 REDUCE  
 Reduce el número de filas que se van a mover de la tabla del lado derecho de la combinación con el fin de que dos tablas no compatibles de distribución sean compatibles. La sugerencia REDUCE también se denomina sugerencia de semicombinación.  
  
 REPLICATE  
 Hace que los valores de la columna de combinación de la tabla del lado izquierdo de la combinación se repliquen en todos los nodos. La tabla de la derecha se combina con la versión replicada de esas columnas.  
  
 REDISTRIBUTE  
 Fuerza a la distribución de dos orígenes de datos en columnas especificadas en la cláusula JOIN. En el caso de una tabla distribuida, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] realiza un movimiento de orden aleatorio. En el caso de una tabla replicada, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] realiza un movimiento de recorte. Para entender estos tipos de movimiento, vea la sección "DMS Query Plan Operations" (Operaciones del plan de consulta DMS) del tema "Understanding Query Plans" (Descripción de los planes de consulta) de la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]. Esta sugerencia puede mejorar el rendimiento si el plan de consulta usa un movimiento de difusión para resolver una combinación no compatible de distribución.  
  
 JOIN  
 Indica que se va a ejecutar la operación de combinación especificada entre los orígenes de tabla o vistas indicados.  
  
 ON \<search_condition>  
 Especifica la condición en la que se basa la combinación. La condición puede especificar cualquier predicado, aunque se suelen utilizar columnas y operadores de comparación; por ejemplo:  
  
```sql
SELECT p.ProductID, v.BusinessEntityID  
FROM Production.Product AS p   
JOIN Purchasing.ProductVendor AS v  
ON (p.ProductID = v.ProductID);  
  
```  
  
 Cuando la condición especifique columnas, no será necesario que tengan el mismo nombre o el mismo tipo de datos; sin embargo, si los tipos de datos no son idénticos, deben ser compatibles o tratarse de tipos que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueda convertir implícitamente. Si los tipos de datos no se pueden convertir implícitamente, la condición debe convertir de forma explícita el tipo de datos mediante la función CONVERT.  
  
 Puede haber predicados relacionados solamente con una de las tablas combinadas de la cláusula ON. Estos predicados también pueden estar en la cláusula WHERE de la consulta. Aunque la posición de estos predicados no produce ninguna diferencia en el caso de combinaciones INNER, podría generar un resultado diferente si estuvieran implicadas las combinaciones OUTER. La razón es que los predicados de la cláusula ON se aplican a la tabla antes de la combinación, mientras la cláusula WHERE se aplica de forma semántica al resultado de la combinación.  
  
 Para obtener más información sobre los predicados y las condiciones de búsqueda, vea [Condiciones de búsqueda &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
 CROSS JOIN  
 Especifica el producto resultante de dos tablas. Devuelve las mismas filas que se devolverían si no se especificara la cláusula WHERE en una combinación de estilo antiguo distinta del estilo de SQL-92.  
  
 *left_table_source* { CROSS | OUTER } APPLY *right_table_source*  
 Especifica que el argumento *right_table_source* del operador APPLY se evalúe con respecto a cada fila de *left_table_source*. Esta funcionalidad es útil si *right_table_source* contiene una función con valores de tabla que toma los valores de columna de *left_table_source* como uno de sus argumentos.  
  
 Se debe especificar OUTER o CROSS con APPLY. Si se especifica CROSS, no se genera ninguna fila cuando *right_table_source* se evalúa con respecto a una fila especificada de *left_table_source* y devuelve un conjunto de resultados vacío.  
  
 Si se especifica OUTER, se genera una fila para cada fila de *left_table_source*, incluso si *right_table_source* se evalúa con respecto a dicha fila y devuelve un conjunto de resultados vacío.  
  
 Para obtener más información, vea la sección Comentarios.  
  
 *left_table_source*  
 Es un origen de tabla según se ha definido en el argumento anterior. Para obtener más información, vea la sección Comentarios.  
  
 *right_table_source*  
 Es un origen de tabla según se ha definido en el argumento anterior. Para obtener más información, vea la sección Comentarios.  
  
### <a name="pivot-clause"></a>Cláusula PIVOT

 *table_source* PIVOT \<pivot_clause>  
 Especifica que el argumento *table_source* se dinamice según *pivot_column*. *table_source* es una tabla o expresión de tabla. El resultado es una tabla que contiene todas las columnas de *table_source*, excepto *pivot_column* y *value_column*. Las columnas de *table_source*, excepto *pivot_column* y *value_column*, se denominan columnas de agrupamiento del operador dinámico. Para obtener más información sobre PIVOT y UNPIVOT, vea [Usar PIVOT y UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
 PIVOT realiza una operación de agrupamiento en la tabla de entrada con respecto a las columnas de agrupamiento y devuelve una fila para cada grupo. Además, la salida contiene una columna para cada valor especificado en *column_list* que aparece en el parámetro *pivot_column* de *input_table*.  
  
 Para obtener más información, vea la sección Comentarios a continuación.  
  
 *aggregate_function*  
 Es una función de agregado del sistema o definida por el usuario que acepta una o más entradas. La función de agregado no puede variar con respecto a los valores NULL. Una función de agregado invariable con respecto a los valores NULL no tiene en cuenta los valores NULL del grupo mientras evalúa el valor del agregado.  
  
 No se permite la función de agregado del sistema COUNT(*).  
  
 *value_column*  
 Es la columna de valores del operador PIVOT. Si se usa con UNPIVOT, *value_column* no puede ser el nombre de una columna existente en el parámetro *table_source* de entrada.  
  
 FOR *pivot_column*  
 Es la columna dinámica del operador PIVOT. *pivot_column* debe ser de un tipo que se pueda convertir implícita o explícitamente en **nvarchar()** . Esta columna no puede ser **image** ni **rowversion**.  
  
 Si se usa UNPIVOT, *pivot_column* es el nombre de la columna de salida restringida a partir de *table_source*. No puede haber ninguna columna en *table_source* con ese nombre.  
  
 IN (*column_list* )  
 En la cláusula PIVOT, se incluyen los valores de *pivot_column* que se van a convertir en los nombres de columnas de la tabla de salida. La lista no puede especificar ningún nombre de columna que ya exista en el parámetro *table_source* de entrada que se está dinamizando.  
  
 En la cláusula UNPIVOT, se incluyen las columnas de *table_source* que se van a restringir en una sola *pivot_column*.  
  
 *table_alias*  
 Es el nombre de alias de la tabla de salida. Se debe especificar *pivot_table_alias*.  
  
 UNPIVOT \<unpivot_clause>  
 Especifica que la tabla de entrada se restrinja a partir de varias columnas de *column_list* en una sola columna denominada *pivot_column*. Para obtener más información sobre PIVOT y UNPIVOT, vea [Usar PIVOT y UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
AS OF \<date_time>  
**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
Devuelve una tabla con un único registro por cada fila que contenga los valores que fueran reales (actuales) en el momento determinado especificado en el pasado. Internamente, se realiza una unión entre la tabla temporal y su tabla de historial, y los resultados se filtran para devolver los valores de la fila que era válida en el momento especificado por el parámetro *\<date_time>* . El valor de una fila se considera válido si el valor de *system_start_time_column_name* es menor o igual que el valor del parámetro *\<date_time>* y el valor de *system_end_time_column_name* es mayor que el valor del parámetro *\<date_time>* .   
  
FROM \<start_date_time> TO \<end_date_time>
**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].
Devuelve una tabla con los valores de todas las versiones de registro que estaban activas dentro del intervalo de tiempo especificado, independientemente de si empezaron a estar activas antes del valor del parámetro *\<start_date_time>* en el argumento FROM o si dejaron de estarlo después del valor del parámetro *\<end_date_time>* en el argumento TO. Internamente, se realiza una unión entre la tabla temporal y su tabla de historial y los resultados se filtran para devolver los valores de todas las versiones de fila que estaban activas en cualquier momento dentro del intervalo de tiempo especificado. Se incluyen las filas que se activaron justamente en el límite inferior definido por el punto de conexión FROM y no se incluyen aquellas que se activaron exactamente en el límite superior definido por el punto de conexión TO.  
  
BETWEEN \<start_date_time> AND \<end_date_time>  
**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
Igual que la descripción anterior de **FROM \<start_date_time> TO \<end_date_time>** , salvo que incluye las filas que se activaron en el límite superior definido por el punto de conexión \<end_date_time>.  
  
CONTAINED IN (\<start_date_time> , \<end_date_time>)  
**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
Devuelve una tabla con los valores de todas las versiones de registro que se abrieron y cerraron dentro del intervalo de tiempo especificado definido por los dos valores de fecha y hora en el argumento CONTAINED IN. Se incluyen las filas que se activaron justamente en el límite inferior o que dejaron de estarlo exactamente en el límite superior.  
  
ALL  
Devuelve una tabla con los valores de todas las filas de la tabla actual y la tabla de historial.  
  
## <a name="remarks"></a>Observaciones  
 La cláusula FROM admite la sintaxis SQL-92-SQL para las tablas combinadas y las tablas derivadas. La sintaxis SQL-92 proporciona los operadores de combinación INNER, LEFT OUTER, RIGHT OUTER, FULL OUTER y CROSS.  
  
 En las vistas, tablas derivadas y subconsultas se admiten las operaciones UNION y JOIN dentro de una cláusula FROM.  
  
 Una autocombinación es una tabla que se combina consigo misma. Las inserciones o actualizaciones basadas en una autocombinación siguen el orden de la cláusula FROM.  
  
 Puesto que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera las estadísticas de cardinalidad y distribución de servidores vinculados que proporcionan estadísticas de distribución de columnas, no es necesaria la sugerencia de combinación REMOTE para exigir la evaluación de una combinación de forma remota. El procesador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera las estadísticas remotas y determina si es apropiada una estrategia de combinación remota. La sugerencia de combinación REMOTE es útil para los proveedores que no proporcionan estadísticas de distribución de columnas.  
  
## <a name="using-apply"></a>Usar APPLY  
 Los operandos izquierdo y derecho del operador APPLY son expresiones de tabla. La diferencia principal entre estos operandos es que *right_table_source* puede usar una función con valores de tabla que tome una columna de *left_table_source* como uno de los argumentos de la función. *left_table_source* puede incluir funciones con valores de tabla, pero no puede contener argumentos que sean columnas de *right_table_source*.  
  
El operador APPLY funciona del siguiente modo para generar el origen de tabla para la cláusula FROM:  
  
1.  Se evalúa como *right_table_source* con respecto a cada fila de *left_table_source* para generar conjuntos de filas.  
  
    Los valores de *right_table_source* dependen de *left_table_source*. *right_table_source* se puede representar aproximadamente de esta manera: `TVF(left_table_source.row)`, donde `TVF` es una función con valores de tabla.  
  
2.  Combina los conjuntos de resultados generados para cada fila en la evaluación de *right_table_source* con *left_table_source* mediante una operación UNION ALL.  
  
    La lista de columnas que genera el resultado del operador APPLY es el conjunto de columnas de *left_table_source* combinado con la lista de columnas de *right_table_source*.  
  
## <a name="using-pivot-and-unpivot"></a>Usar PIVOT y UNPIVOT  
 *pivot_column* y *value_column* son columnas de agrupamiento usadas por el operador PIVOT. Para obtener el conjunto de resultados de salida, PIVOT aplica el siguiente proceso:  
  
1.  Realiza una operación GROUP BY en *input_table* con respecto a las columnas de agrupamiento y genera una fila de salida para cada grupo.  
  
     Las columnas de agrupamiento de la fila de salida obtienen los valores de columna correspondientes para dicho grupo en *input_table*.  
  
2.  Genera valores para las columnas de la lista de columnas para cada fila de resultados mediante las siguientes operaciones:  
  
    1.  Agrupación adicional de las filas generadas en GROUP BY en el paso anterior con respecto a *pivot_column*.  
  
         Para cada columna de salida de *column_list*, se selecciona un subgrupo que cumple la condición:  
  
         `pivot_column = CONVERT(<data type of pivot_column>, 'output_column')`  
  
    2.  *aggregate_function* se evalúa con respecto a *value_column* en este subgrupo y su resultado se devuelve como el valor de *output_column* correspondiente. Si el subgrupo está vacío, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera un valor NULL para *output_column*. Si la función de agregado es COUNT y el subgrupo está vacío, se devuelve cero (0).  

> [!NOTE]
> Los identificadores de columna de la cláusula `UNPIVOT` siguen la intercalación del catálogo. Para [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], la intercalación es siempre `SQL_Latin1_General_CP1_CI_AS`. Para las bases de datos parcialmente independientes de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], la intercalación es siempre `Latin1_General_100_CI_AS_KS_WS_SC`. Si la columna se combina con otras columnas, se necesita una cláusula COLLATE (`COLLATE DATABASE_DEFAULT`) para evitar conflictos.   
  
 Para obtener más información sobre PIVOT y UNPIVOT, incluidos ejemplos, vea [Usar PIVOT y UNPIVOT](../../t-sql/queries/from-using-pivot-and-unpivot.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere los permisos para la instrucción DELETE, SELECT o UPDATE.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-a-simple-from-clause"></a>A. Usar una cláusula FROM sencilla  
 En el siguiente ejemplo se recuperan las columnas `TerritoryID` y `Name` de la tabla `SalesTerritory` de la base de datos de ejemplo [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql    
SELECT TerritoryID, Name  
FROM Sales.SalesTerritory  
ORDER BY TerritoryID ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
TerritoryID Name                            
----------- ------------------------------  
1           Northwest                       
2           Northeast                       
3           Central                         
4           Southwest                       
5           Southeast                       
6           Canada                          
7           France                          
8           Germany                         
9           Australia                       
10          United Kingdom                  
(10 row(s) affected)  
```  
  
### <a name="b-using-the-tablock-and-holdlock-optimizer-hints"></a>B. Usar las sugerencias del optimizador TABLOCK y HOLDLOCK  
 En la siguiente transacción parcial se muestra cómo colocar un bloqueo explícito de tabla compartida en `Employee` y cómo leer el índice. El bloqueo se mantiene durante toda la transacción.  
  
```sql    
BEGIN TRAN  
SELECT COUNT(*)   
FROM HumanResources.Employee WITH (TABLOCK, HOLDLOCK) ;  
```  
  
### <a name="c-using-the-sql-92-cross-join-syntax"></a>C. Usar la sintaxis CROSS JOIN de SQL-92  
 Este ejemplo devuelve el producto resultante de las tablas `Employee` y `Department` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Se devuelve la lista de todas las combinaciones posibles de las filas de `BusinessEntityID` y todas las filas con el nombre `Department`.  
  
```sql    
SELECT e.BusinessEntityID, d.Name AS Department  
FROM HumanResources.Employee AS e  
CROSS JOIN HumanResources.Department AS d  
ORDER BY e.BusinessEntityID, d.Name ;  
```  
  
### <a name="d-using-the-sql-92-full-outer-join-syntax"></a>D. Usar la sintaxis FULL OUTER JOIN de SQL-92  
 En el siguiente ejemplo se devuelve el nombre del producto y los pedidos de venta correspondientes de la tabla `SalesOrderDetail` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Además, se devuelven todos los pedidos de venta que no incluyen ningún producto en la tabla `Product` y todos los productos con un pedido de venta distinto del especificado en la tabla `Product`.  
  
```sql  
-- The OUTER keyword following the FULL keyword is optional.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
FULL OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="e-using-the-sql-92-left-outer-join-syntax"></a>E. Usar la sintaxis LEFT FULL OUTER de SQL-92  
 Este ejemplo combina dos tablas en `ProductID` y mantiene las filas no coincidentes de la tabla izquierda. La tabla `Product` coincide con la tabla `SalesOrderDetail` en las columnas `ProductID` de cada tabla. Todos los productos, ordenados y no ordenados, aparecen en el conjunto de resultados.  
  
```sql    
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
LEFT OUTER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="f-using-the-sql-92-inner-join-syntax"></a>F. Usar la sintaxis INNER JOIN de SQL-92  
 En el siguiente ejemplo se devuelven todos los nombres de productos e identificadores de pedidos de venta.  
  
```sql    
-- By default, SQL Server performs an INNER JOIN if only the JOIN   
-- keyword is specified.  
SELECT p.Name, sod.SalesOrderID  
FROM Production.Product AS p  
INNER JOIN Sales.SalesOrderDetail AS sod  
ON p.ProductID = sod.ProductID  
ORDER BY p.Name ;  
```  
  
### <a name="g-using-the-sql-92-right-outer-join-syntax"></a>G. Usar la sintaxis RIGHT OUTER JOIN de SQL-92  
 Este ejemplo combina dos tablas en `TerritoryID` y mantiene las filas no coincidentes de la tabla derecha. La tabla `SalesTerritory` coincide con la tabla `SalesPerson` en la columna `TerritoryID` de cada tabla. Todos los vendedores aparecen en el conjunto de resultados con independencia de que tengan un territorio asignado.  
  
```sql    
SELECT st.Name AS Territory, sp.BusinessEntityID  
FROM Sales.SalesTerritory AS st   
RIGHT OUTER JOIN Sales.SalesPerson AS sp  
ON st.TerritoryID = sp.TerritoryID ;  
```  
  
### <a name="h-using-hash-and-merge-join-hints"></a>H. Usar las sugerencias de combinación HASH y MERGE  
 En el siguiente ejemplo se realiza una combinación de tres tablas entre las tablas `Product`, `ProductVendor` y `Vendor` para generar una lista de productos y sus proveedores. El optimizador de consultas combina `Product` y `ProductVendor` (`p` y `pv`) mediante una combinación MERGE. A continuación, los resultados de la combinación MERGE de `Product` y `ProductVendor` (`p` y `pv`) se combinan mediante HASH con la tabla `Vendor` para generar (`p` y `pv`) y `v`.  
  
> [!IMPORTANT]  
>  Después de especificar una sugerencia de combinación, la palabra clave INNER ya no es opcional y se tiene que incluir explícitamente para hacer combinaciones INNER JOIN.  
  
```sql    
SELECT p.Name AS ProductName, v.Name AS VendorName  
FROM Production.Product AS p   
INNER MERGE JOIN Purchasing.ProductVendor AS pv   
ON p.ProductID = pv.ProductID  
INNER HASH JOIN Purchasing.Vendor AS v  
ON pv.BusinessEntityID = v.BusinessEntityID  
ORDER BY p.Name, v.Name ;  
```  
  
### <a name="i-using-a-derived-table"></a>I. Usar una tabla derivada  
 En el siguiente ejemplo se utiliza una tabla derivada y una instrucción `SELECT` después de la cláusula `FROM` para devolver los nombres y apellidos de todos los empleados y las ciudades en que residen.  
  
```sql    
SELECT RTRIM(p.FirstName) + ' ' + LTRIM(p.LastName) AS Name, d.City  
FROM Person.Person AS p  
INNER JOIN HumanResources.Employee e ON p.BusinessEntityID = e.BusinessEntityID   
INNER JOIN  
   (SELECT bea.BusinessEntityID, a.City   
    FROM Person.Address AS a  
    INNER JOIN Person.BusinessEntityAddress AS bea  
    ON a.AddressID = bea.AddressID) AS d  
ON p.BusinessEntityID = d.BusinessEntityID  
ORDER BY p.LastName, p.FirstName;  
```  
  
### <a name="j-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>J. Usar TABLESAMPLE para leer datos de un ejemplo de filas de una tabla  
 En el siguiente ejemplo se utiliza `TABLESAMPLE` en la cláusula `FROM` para devolver aproximadamente el `10` por ciento de todas las filas de la tabla `Customer`.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;  
```  
  
### <a name="k-using-apply"></a>K. Usar APPLY  
En el siguiente ejemplo se da por supuesto que las siguientes tablas y la función con valores de tabla existen en la base de datos:  

|Nombre de objeto|Nombres de columna|      
|---|---|   
|Departments|DeptID, DivisionID, DeptName, DeptMgrID|      
|EmpMgr|MgrID, EmpID|     
|Employees|EmpID, EmpLastName, EmpFirstName, EmpSalary|  
|GetReports(MgrID)|EmpID, EmpLastName, EmpSalary|     
  
La función con valores de tabla `GetReports` devuelve la lista de todos los empleados que dependen directa o indirectamente del `MgrID` especificado.  
  
En el ejemplo se utiliza `APPLY` para devolver todos los departamentos y todos los empleados de cada departamento. Si un departamento concreto no tiene ningún empleado, no se devuelve ninguna fila para dicho departamento.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d    
CROSS APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
Si desea que la consulta genere filas para los departamentos sin empleados, lo que genera valores NULL para las columnas `EmpID`, `EmpLastName` y `EmpSalary`, utilice `OUTER APPLY`.  
  
```sql
SELECT DeptID, DeptName, DeptMgrID, EmpID, EmpLastName, EmpSalary  
FROM Departments d   
OUTER APPLY dbo.GetReports(d.DeptMgrID) ;  
```  
  
### <a name="l-using-cross-apply"></a>L. Usar CROSS APPLY  
En el ejemplo siguiente se recupera una instantánea de todos los planes de consulta que residen en la memoria caché del plan mediante una consulta a la vista de administración dinámica `sys.dm_exec_cached_plans` para recuperar los identificadores de todos los planes de consulta almacenados en la memoria caché. A continuación se especifica el operador `CROSS APPLY` para pasar los identificadores del plan a `sys.dm_exec_query_plan`. La salida del plan de presentación XML de todos los planes almacenados actualmente en la caché del plan se muestra en la columna `query_plan` de la tabla devuelta.  
  
```sql
USE master;  
GO  
SELECT dbid, object_id, query_plan   
FROM sys.dm_exec_cached_plans AS cp   
CROSS APPLY sys.dm_exec_query_plan(cp.plan_handle);   
GO  
```  
  
### <a name="m-using-for-system_time"></a>M. Usar FOR SYSTEM_TIME  
  
**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]  
En el ejemplo siguiente se usa el argumento FOR SYSTEM_TIME AS OF date_time_literal_or_variable para devolver filas de tabla que eran reales (actuales) el 1 de enero de 2014.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME AS OF '2014-01-01'  
WHERE ManagerID = 5;
```  
  
En el ejemplo siguiente se usa el argumento FOR SYSTEM_TIME FROM date_time_literal_or_variable TO date_time_literal_or_variable para devolver todas las filas que estaban activas durante el período comprendido entre el 1 de enero de 2013 y el 1 de enero de 2014, excluyendo el límite superior.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM '2013-01-01' TO '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 En el ejemplo siguiente se usa el argumento FOR SYSTEM_TIME BETWEEN date_time_literal_or_variable AND date_time_literal_or_variable para devolver todas las filas que estaban activas durante el período comprendido entre el 1 de enero de 2013 y el 1 de enero de 2014, incluyendo el límite superior.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME BETWEEN '2013-01-01' AND '2014-01-01'  
WHERE ManagerID = 5;
```  
  
 En el ejemplo siguiente se usa el argumento FOR SYSTEM_TIME CONTAINED IN ( date_time_literal_or_variable, date_time_literal_or_variable ) para devolver todas las filas que se abrieron y cerraron durante el período comprendido entre el 1 de enero de 2013 y el 1 de enero de 2014.  
  
```sql
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME CONTAINED IN ( '2013-01-01', '2014-01-01' )  
WHERE ManagerID = 5;
```  
  
 En el ejemplo siguiente se usa una variable en lugar de un literal para proporcionar los valores de límite de fecha de la consulta.  
  
```sql
DECLARE @AsOfFrom datetime2 = dateadd(month,-12, sysutcdatetime());
DECLARE @AsOfTo datetime2 = dateadd(month,-6, sysutcdatetime());
  
SELECT DepartmentNumber,   
    DepartmentName,   
    ManagerID,   
    ParentDepartmentNumber   
FROM DEPARTMENT  
FOR SYSTEM_TIME FROM @AsOfFrom TO @AsOfTo  
WHERE ManagerID = 5;
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="n-using-the-inner-join-syntax"></a>Hora Usar la sintaxis INNER JOIN  
 El ejemplo siguiente devuelve las columnas `SalesOrderNumber`, `ProductKey` y `EnglishProductName` de las tablas `FactInternetSales` y `DimProduct` donde la clave de combinación, `ProductKey`, coincide en ambas tablas. Las columnas `SalesOrderNumber` y `EnglishProductName` existen en una de las tablas únicamente, por lo que no es necesario especificar el alias de tabla con estas columnas, como se muestra; estos alias se incluyen por motivos de legibilidad. La palabra **AS** antes de un alias de nombre no es necesaria, pero se recomienda por motivos de legibilidad y para ajustarse al estándar ANSI.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
INNER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Puesto que la palabra clave `INNER` no es necesaria para las combinaciones internas, esta misma consulta podría escribirse como:  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
ON dp.ProductKey = fis.ProductKey;  
```  
  
 También podría usarse una cláusula `WHERE` con esta consulta para limitar los resultados. Este ejemplo limita los resultados a los valores `SalesOrderNumber` superiores a “SO5000”:  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey  
WHERE fis.SalesOrderNumber > 'SO50000'  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="o-using-the-left-outer-join-and-right-outer-join-syntax"></a>O. Usar la sintaxis LEFT OUTER JOIN y RIGHT OUTER JOIN  
 En el ejemplo siguiente se combinan las tablas `FactInternetSales` y `DimProduct` en las columnas `ProductKey`. La sintaxis de combinación externa izquierda mantiene las filas no coincidentes de la tabla izquierda (`FactInternetSales`). Puesto que la tabla `FactInternetSales` no contiene ningún valor `ProductKey` que no coincida con la tabla `DimProduct`, esta consulta devuelve las mismas filas que el primer ejemplo de combinación interna anterior.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM FactInternetSales AS fis 
LEFT OUTER JOIN DimProduct AS dp  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 Esta consulta también se podría escribir sin la palabra clave `OUTER`.  
  
 En las combinaciones externas derechas, se conservan las filas no coincidentes de la tabla derecha. El ejemplo siguiente devuelve las mismas filas que el anterior ejemplo de combinación externa izquierda.  
  
```sql
-- Uses AdventureWorks  
  
SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
FROM DimProduct AS dp 
RIGHT OUTER JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  
  
 La consulta siguiente usa la tabla `DimSalesTerritory` como tabla izquierda en una combinación externa izquierda. Recupera los valores `SalesOrderNumber` de la tabla `FactInternetSales`. Si no hay ningún orden en un valor `SalesTerritoryKey` determinado, la consulta devuelve NULL para `SalesOrderNumber` para esa fila. Esta consulta se ordena conforme a la columna `SalesOrderNumber`, por lo que cualquier valor NULL en esta columna aparece en la parte superior de los resultados.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
LEFT OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Esta consulta se podría volver a escribir con una combinación externa derecha para recuperar los mismos resultados:  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM FactInternetSales AS fis 
RIGHT OUTER JOIN DimSalesTerritory AS dst  
    ON fis.SalesTerritoryKey = dst.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="p-using-the-full-outer-join-syntax"></a>P. Usar la sintaxis FULL OUTER JOIN  
 En el ejemplo siguiente se muestra una combinación externa completa que devuelve todas las filas de ambas tablas combinadas, pero que devuelve NULL para los valores que no coinciden de la otra tabla.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL OUTER JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
 Esta consulta también se podría escribir sin la palabra clave `OUTER`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, dst.SalesTerritoryRegion, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
FULL JOIN FactInternetSales AS fis  
    ON dst.SalesTerritoryKey = fis.SalesTerritoryKey  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="q-using-the-cross-join-syntax"></a>Q. Usar la sintaxis CROSS JOIN  
 Este ejemplo devuelve el producto resultante de las tablas `FactInternetSales` y `DimSalesTerritory`. Se devuelve la lista de todas las combinaciones posibles de `SalesOrderNumber` y `SalesTerritoryKey`. Observe la ausencia de la cláusula `ON` en la consulta de combinación cruzada.  
  
```sql
-- Uses AdventureWorks  
  
SELECT dst.SalesTerritoryKey, fis.SalesOrderNumber  
FROM DimSalesTerritory AS dst 
CROSS JOIN FactInternetSales AS fis  
ORDER BY fis.SalesOrderNumber;  
```  
  
### <a name="r-using-a-derived-table"></a>R. Usar una tabla derivada  
 En el ejemplo siguiente, se usa una tabla derivada (una instrucción `SELECT` después de la cláusula `FROM`) para devolver las columnas `CustomerKey` y `LastName` de todos los clientes de la tabla `DimCustomer` con valores `BirthDate` posteriores al 1 de enero de 1970 y el apellido “Smith”.  
  
```sql
-- Uses AdventureWorks  
  
SELECT CustomerKey, LastName  
FROM  
   (SELECT * FROM DimCustomer  
    WHERE BirthDate > '01/01/1970') AS DimCustomerDerivedTable  
WHERE LastName = 'Smith'  
ORDER BY LastName;  
```  
  
### <a name="s-reduce-join-hint-example"></a>S. Ejemplo de sugerencia de combinación REDUCE  
 En el ejemplo siguiente se usa la sugerencia de combinación `REDUCE` para modificar el procesamiento de la tabla derivada dentro de la consulta. Cuando se usa la sugerencia de combinación `REDUCE` en esta consulta, `fis.ProductKey` se proyecta, se replica y se distingue, y luego se combina con `DimProduct` durante la operación de orden aleatorio de `DimProduct` en `ProductKey`. La tabla derivada resultante se distribuye en `fis.ProductKey`.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REDUCE JOIN FactInternetSales AS fis   
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="t-replicate-join-hint-example"></a>T. Ejemplo de sugerencia de combinación REPLICATE  
 En el ejemplo siguiente se muestra la misma consulta que en el ejemplo anterior, salvo que se usa una sugerencia de combinación `REPLICATE` en lugar de la sugerencia de combinación `REDUCE`. El empleo de la sugerencia `REPLICATE` hace que los valores de la columna `ProductKey` (de combinación) de la tabla `FactInternetSales` se repliquen en todos los nodos. La tabla `DimProduct` se combina con la versión replicada de esos valores.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN SELECT SalesOrderNumber  
FROM  
   (SELECT fis.SalesOrderNumber, dp.ProductKey, dp.EnglishProductName  
    FROM DimProduct AS dp   
      INNER REPLICATE JOIN FactInternetSales AS fis  
          ON dp.ProductKey = fis.ProductKey  
   ) AS dTable  
ORDER BY SalesOrderNumber;  
```  
  
### <a name="u-using-the-redistribute-hint-to-guarantee-a-shuffle-move-for-a-distribution-incompatible-join"></a>U. Usar la sugerencia REDISTRIBUTE para garantizar un movimiento de orden aleatorio de una combinación no compatible de distribución  
 La siguiente consulta usa la sugerencia de consulta REDISTRIBUTE en una combinación no compatible de distribución. Esto garantiza que el optimizador de consultas use un movimiento de orden aleatorio en el plan de consulta. También garantiza que el plan de consulta no use un movimiento de difusión que mueva una tabla distribuida a una tabla replicada.  
  
 En el ejemplo siguiente, la sugerencia REDISTRIBUTE fuerza un movimiento de orden aleatorio en la tabla FactInternetSales, dado que ProductKey es la columna de distribución de DimProduct y no la columna de distribución de FactInternetSales.  
  
```sql
-- Uses AdventureWorks  
  
EXPLAIN  
SELECT dp.ProductKey, fis.SalesOrderNumber, fis.TotalProductCost  
FROM DimProduct AS dp 
INNER REDISTRIBUTE JOIN FactInternetSales AS fis  
    ON dp.ProductKey = fis.ProductKey;  
```  

### <a name="v-using-tablesample-to-read-data-from-a-sample-of-rows-in-a-table"></a>V. Usar TABLESAMPLE para leer datos de un ejemplo de filas de una tabla  
 En el siguiente ejemplo se utiliza `TABLESAMPLE` en la cláusula `FROM` para devolver aproximadamente el `10` por ciento de todas las filas de la tabla `Customer`.  
  
```sql    
SELECT *  
FROM Sales.Customer TABLESAMPLE SYSTEM (10 PERCENT) ;
```
  
## <a name="see-also"></a>Consulte también  
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [OPENQUERY &#40;Transact-SQL&#41;](../../t-sql/functions/openquery-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)
