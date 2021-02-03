---
description: MERGE (Transact-SQL)
title: MERGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MERGE
- MERGE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- updating data [SQL Server]
- modifying data [SQL Server], MERGE statement
- MERGE statement [SQL Server]
- adding data
- DML [SQL Server], MERGE statement
- table modifications [SQL Server], MERGE statement
- data manipulation language [SQL Server], MERGE statement
- inserting data
ms.assetid: c17996d6-56a6-482f-80d8-086a3423eecc
author: XiaoyuMSFT
ms.author: XiaoyuL
ms.openlocfilehash: cbc1286bbbced3a546c1057433dba5bbc4289281
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199388"
---
# <a name="merge-transact-sql"></a>MERGE (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asa.md)]

Ejecuta operaciones de inserción, actualización o eliminación en una tabla de destino a partir de los resultados de una combinación con una tabla de origen. Por ejemplo, sincronice dos tablas mediante la inserción, actualización o eliminación de las filas de una tabla según las diferencias que se encuentren en la otra. 

> [!NOTE]
> El comando MERGE se encuentra en versión preliminar para Azure Synapse Analytics.
  
**Sugerencia de rendimiento:** el comportamiento condicional descrito para la instrucción MERGE funciona mejor cuando las dos tablas tienen una mezcla compleja de características coincidentes. Por ejemplo, insertar una fila si no existe o actualizar una fila si coincide. Cuando simplemente se actualiza una tabla basada en las filas de otra tabla, mejore el rendimiento y la escalabilidad con las instrucciones básicas INSERT, UPDATE y DELETE. Por ejemplo:  
  
```sql
INSERT tbl_A (col, col2)  
SELECT col, col2
FROM tbl_B
WHERE NOT EXISTS (SELECT col FROM tbl_A A2 WHERE A2.col = tbl_B.col);  
```  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql

-- SQL Server and Azure SQL Database
[ WITH <common_table_expression> [,...n] ]  
MERGE
    [ TOP ( expression ) [ PERCENT ] ]
    [ INTO ] <target_table> [ WITH ( <merge_hint> ) ] [ [ AS ] table_alias ]  
    USING <table_source> [ [ AS ] table_alias ]
    ON <merge_search_condition>  
    [ WHEN MATCHED [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]  
        THEN <merge_not_matched> ]  
    [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ <output_clause> ]  
    [ OPTION ( <query_hint> [ ,...n ] ) ]
;  
  
<target_table> ::=  
{
    [ database_name . schema_name . | schema_name . ]  
  target_table  
}  
  
<merge_hint>::=  
{  
    { [ <table_hint_limited> [ ,...n ] ]  
    [ [ , ] INDEX ( index_val [ ,...n ] ) ] }  
}  

<merge_search_condition> ::=  
    <search_condition>  
  
<merge_matched>::=  
    { UPDATE SET <set_clause> | DELETE }  
  
<merge_not_matched>::=  
{  
    INSERT [ ( column_list ) ]
        { VALUES ( values_list )  
        | DEFAULT VALUES }  
}  
  
<clause_search_condition> ::=  
    <search_condition> 
```  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

```syntaxsql
-- MERGE (Preview) for Azure Synapse Analytics 
[ WITH <common_table_expression> [,...n] ]  
MERGE
    [ INTO ] <target_table> [ [ AS ] table_alias ]  
    USING <table_source> [ [ AS ] table_alias ]
    ON <merge_search_condition>  
    [ WHEN MATCHED [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]  
    [ WHEN NOT MATCHED [ BY TARGET ] [ AND <clause_search_condition> ]  
        THEN <merge_not_matched> ]  
    [ WHEN NOT MATCHED BY SOURCE [ AND <clause_search_condition> ]  
        THEN <merge_matched> ] [ ...n ]
    [ OPTION ( <query_hint> [ ,...n ] ) ]
;  -- The semi-colon is required, or the query will return syntax  error. 
```
 
## <a name="arguments"></a>Argumentos

WITH \<common_table_expression>  
Especifica la vista o el conjunto de resultados temporal indicado, que también se conoce como expresión de tabla común, definido en el ámbito de la instrucción MERGE. El conjunto de resultados deriva de una consulta simple. La instrucción MERGE hace referencia al conjunto de resultados. Para más información, vea [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
TOP ( *expression* ) [ PERCENT ]  
Especifica el número o porcentaje de filas afectadas. *expression* puede ser un número o un porcentaje de las filas. Las filas a las que se hace referencia en la expresión TOP no están organizadas en ningún orden. Para más información, vea [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
La cláusula TOP se aplica después de que se combinen toda la tabla de origen y toda la tabla de destino, y se quiten las filas combinadas que no reúnan las condiciones para las acciones de inserción, actualización o eliminación. La cláusula TOP reduce aún más el número de filas unidas al valor especificado. Las acciones de inserción, actualización o eliminación se aplican a las filas unidas restantes de forma desordenada. Es decir, no hay ningún orden en el que las filas se distribuyan entre las acciones definidas en las cláusulas WHEN. Por ejemplo, si se especifica TOP (10), afectará a 10 filas. De estas filas, 7 se pueden actualizar y 3 insertar, o se puede eliminar 1, actualizar 5 e insertar 4, y así sucesivamente.  
  
Dado que la instrucción MERGE realiza recorridos de tabla completos de ambas tablas, de destino y de origen, el rendimiento de E/S a veces se ve afectado al utilizar la cláusula TOP para modificar una tabla grande mediante la creación de varios lotes. En este escenario, es importante asegurarse de que todos los lotes sucesivos tengan como destino nuevas filas.  
  
*database_name*  
El nombre de la base de datos donde se encuentra *target_table*.  
  
*schema_name*  
El nombre del esquema al que pertenece *target_table*.  
  
*target_table*  
La tabla o vista con la que se hacen coincidir las filas de datos de \<table_source> según \<clause_search_condition>. *target_table* es el destino de las operaciones de inserción, actualización o eliminación especificado por las cláusulas WHEN de la instrucción MERGE.  
  
Si *target_table* es una vista, cualquier acción con ella debe satisfacer las condiciones para actualizar las vistas. Para más información, vea [Modificar datos mediante una vista](../../relational-databases/views/modify-data-through-a-view.md).  
  
*target_table* no puede ser una tabla remota. *target_table* no puede tener ninguna regla definida.  
  
[ AS ] *table_alias*  
Nombre alternativo para hacer referencia a una tabla para *target_table*.  
  
USING \<table_source>  
Especifica el origen de datos que se hace coincidir con las filas de datos de *target_table* en función de \<merge_search condition>. El resultado de esta coincidencia dicta las acciones que tomarán las cláusulas WHEN de la instrucción MERGE. \<table_source> puede ser una tabla remota o una tabla derivada que acceda a las tablas remotas.
  
\<table_source> puede ser una tabla derivada que use el [constructor con valores de tabla](../../t-sql/queries/table-value-constructor-transact-sql.md) de [!INCLUDE[tsql](../../includes/tsql-md.md)] para construir una tabla mediante la especificación de varias filas.  
  
 [ AS ] *table_alias*  
Nombre alternativo para hacer referencia a una tabla para table_source.   
  
Para más información sobre la sintaxis y los argumentos de esta cláusula, vea [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
ON \<merge_search_condition>  
Especifica las condiciones en las que \<table_source> se combina con *target_table* para determinar dónde coinciden.
  
> [!CAUTION]  
> Es importante especificar solamente las columnas de la tabla de destino que se utilizan para los propósitos de la coincidencia. Es decir, especifique las columnas de la tabla de destino que se comparan con la correspondiente columna de la tabla de origen. No intente mejorar el rendimiento de las consultas filtrando las filas de la tabla de destino en la cláusula ON; por ejemplo, según se especifica con `AND NOT target_table.column_x = value`. Si se hace esto, se pueden devolver resultados inesperados e incorrectos.  
  
WHEN MATCHED THEN \<merge_matched>  
Especifica que todas las filas de *target_table que coinciden con las filas devueltas por \<table_source> ON \<merge_search_condition> y que satisfacen alguna condición de búsqueda adicional se actualizan o eliminan según la cláusula \<merge_matched>.  
  
La instrucción MERGE puede tener, a lo sumo, dos cláusulas WHEN MATCHED. Si se especifican dos cláusulas, la primera debe ir acompañada de una cláusula AND \<search_condition>. Para una fila determinada, la segunda cláusula WHEN MATCHED se aplica solamente si no se aplica la primera. Si hay dos cláusulas WHEN MATCHED, una debe especificar una acción UPDATE y la otra una acción DELETE. Si se especifica UPDATE en la cláusula \<merge_matched> y más de una fila de \<table_source> coincide con una de *target_table* según \<merge_search_condition>, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error. La instrucción MERGE no puede actualizar la misma fila más de una vez, ni actualizar o eliminar la misma fila.  
  
WHEN NOT MATCHED [ BY TARGET ] THEN \<merge_not_matched>  
Especifica que una fila se inserta en *target_table* para cada fila devuelta por \<table_source> ON \<merge_search_condition> que no coincide con una fila de *target_table*, pero satisface una condición de búsqueda adicional, si está presente. La cláusula \<merge_not_matched> especifica los valores que se van a insertar. La instrucción MERGE solo puede tener una cláusula WHEN NOT MATCHED [ BY TARGET ].

WHEN NOT MATCHED BY SOURCE THEN \<merge_matched>  
Especifica que todas las filas de *target_table que no coinciden con las filas devueltas por \<table_source> ON \<merge_search_condition> y que satisfacen alguna condición de búsqueda adicional se actualizan o eliminan según la cláusula \<merge_matched>.  
  
La instrucción MERGE puede tener a lo sumo dos cláusulas WHEN NOT MATCHED BY SOURCE. Si se especifican dos cláusulas, la primera debe ir acompañada de una cláusula AND \<clause_search_condition>. Para una fila determinada, la segunda cláusula WHEN NOT MATCHED BY SOURCE se aplica solamente si no se aplica la primera. Si hay dos cláusulas WHEN NOT MATCHED BY SOURCE, una debe especificar una acción UPDATE y la otra una acción DELETE. En \<clause_search_condition> solo se puede hacer referencia a las columnas de la tabla de destino.  
  
Cuando \<table_source> no devuelve ninguna fila, no se puede acceder a las columnas de la tabla de origen. Si la acción de actualización o eliminación especificada en la cláusula \<merge_matched> hace referencia a las columnas de la tabla de origen, se devuelve el error 207 (nombre de columna no válido). La cláusula `WHEN NOT MATCHED BY SOURCE THEN UPDATE SET TargetTable.Col1 = SourceTable.Col1` puede hacer que la instrucción genere un error porque `Col1` en la tabla de origen es inaccesible.  
  
AND \<clause_search_condition>  
Especifica cualquier condición de búsqueda válida. Para más información, vea [Condición de búsqueda &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  
  
\<table_hint_limited>  
Especifica una o más sugerencias de tabla que se aplican en la tabla de destino para cada una de las acciones de inserción, actualización o eliminación que realiza la instrucción MERGE. La palabra clave WITH y los paréntesis son obligatorios.  
  
No se permiten NOLOCK ni READUNCOMMITTED. Para más información sobre las sugerencias de tabla, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
Especificar la sugerencia TABLOCK en una tabla que es el destino de una instrucción INSERT tiene el mismo efecto que especificar la sugerencia TABLOCKX. Se realiza un bloqueo exclusivo en la tabla. Cuando se especifica FORCESEEK, se aplica a la instancia implícita de la tabla de destino combinada con la tabla de origen.  
  
> [!CAUTION]  
> Si se especifica READPAST con WHEN NOT MATCHED [ BY TARGET ] THEN INSERT, pueden producirse operaciones INSERT que infrinjan las restricciones UNIQUE.  
  
INDEX ( index_val [ ,...n ] )  
Especifica el nombre o identificador de uno o más índices de la tabla de destino para realizar una combinación implícita con la tabla de origen. Para obtener más información, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
\<output_clause>  
Devuelve una fila para cada fila de *target_table* que se actualiza, inserta o elimina, sin seguir ningún orden concreto. **$action** se puede especificar en la cláusula de salida. **$action** es una columna de tipo **nvarchar(10)** que devuelve uno de estos tres valores para cada fila: "INSERT", "UPDATE" o "DELETE", según la acción realizada en esa fila. Para más información sobre los argumentos y el comportamiento de esta cláusula, vea [Cláusula OUTPUT (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md).  
  
OPTION ( \<query_hint> [ ,...n ] )  
Especifica que se utilizan las sugerencias del optimizador para personalizar el modo en que el motor de base de datos procesa la instrucción. Para obtener más información, vea [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
\<merge_matched>  
Especifica la acción de actualización o eliminación que se aplica a todas las filas de *target_table* que no coinciden con las filas devueltas por \<table_source> ON \<merge_search_condition> y que satisfacen cualquier condición de búsqueda adicional.  
  
UPDATE SET \<set_clause>  
Especifica la lista de nombres de columna o de variable que se van a actualizar en la tabla de destino y los valores con los que se actualizan.  
  
Para más información sobre la sintaxis y los argumentos de esta cláusula, vea [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md). No se admite el establecimiento de una variable con el mismo valor que una columna.  
  
Delete  
Especifica que se eliminarán las filas que coincidan con las filas de *target_table*.  
  
\<merge_not_matched>  
Especifica los valores que insertar en la tabla de destino.  
  
(*column_list*)  
Una lista de una o varias columnas de la tabla de destino en la que insertar los datos. Las columnas se deben especificar como un nombre de una sola parte o, de lo contrario, se producirá un error en la instrucción MERGE. *column_list* debe ir entre paréntesis y delimitada con comas.  
  
VALUES ( *values_list*)  
Una lista separada por comas de constantes, variables o expresiones que devuelve los valores que se insertarán en la tabla de destino. Las expresiones no pueden contener una instrucción EXECUTE.  
  
DEFAULT VALUES  
Hace que la fila insertada contenga los valores predeterminados definidos para cada columna.  
  
Para más información sobre esta cláusula, vea [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).  
  
\<search_condition>  
Especifica las condiciones de búsqueda para especificar \<merge_search_condition> o \<clause_search_condition>. Para más información sobre los argumentos de esta cláusula, vea [Condiciones de búsqueda &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md).  

\<graph search pattern>  
Especifica el patrón de coincidencia de gráficos. Para obtener más información sobre los argumentos de esta cláusula, vea [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md)
  
## <a name="remarks"></a>Observaciones
>[!NOTE]
> En Azure Synapse Analytics, el comando MERGE (versión preliminar) tiene las siguientes diferencias en comparación con SQL Server y Azure SQL Database.  
> - Una actualización MERGE se implementa como un par de eliminación e inserción. El recuento de filas afectado de una actualización MERGE incluye las filas eliminadas e insertadas. 
> - Durante la versión preliminar, el comando MERGE no funciona con las tablas que tienen restricciones UNIQUE.  Este aspecto se corregirá pronto en una versión posterior.
> - En esta tabla se describe la compatibilidad de las tablas con distintos tipos de distribución:

>|MERGE CLAUSE en Azure Synapse Analytics|Tabla de distribución TARGET admitida| Tabla de distribución SOURCE admitida|Comentario|  
>|-----------------|---------------|-----------------|-----------|  
>|**WHEN MATCHED**| HASH, ROUND_ROBIN, REPLICATE |Todos los tipos de distribución||  
>|**NOT MATCHED BY TARGET**|HASH |Todos los tipos de distribución|Use UPDATE/DELETE FROM…JOIN para sincronizar dos tablas. |
>|**NOT MATCHED BY SOURCE**|Todos los tipos de distribución|Todos los tipos de distribución|Use UPDATE/DELETE FROM…JOIN para sincronizar dos tablas.||  

Al menos se debe especificar una de las tres cláusulas MATCHED, pero se pueden especificar en cualquier orden. Una variable no puede actualizarse más de una vez en la misma cláusula MATCHED.  
  
Cualquier acción de inserción, actualización o eliminación especificada en la tabla de destino por la instrucción MERGE está limitada por las restricciones definidas en ella, incluidas las restricciones de integridad referencial en cascada. Si IGNORE_DUP_KEY es ON para algún índice único de la tabla de destino, MERGE omite este valor.  
  
La instrucción MERGE requiere un punto y coma (;) como terminador. Se genera el error 10713 cuando una instrucción MERGE se ejecuta sin el terminador.  
  
Cuando se usa después de MERGE, [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md) devuelve el número total de filas insertadas, actualizadas y eliminadas al cliente.  
  
MERGE es una palabra clave totalmente reservada cuando el nivel de compatibilidad de la base de datos se establece en 100 o superior. La instrucción MERGE también está disponible en los niveles de compatibilidad 90 y 100 de la base de datos; sin embargo, la palabra clave no se reserva completamente cuando el nivel de compatibilidad se establece en 90.  
  
No use la instrucción **MERGE** cuando se usa la replicación de actualización en cola. **MERGE** y el desencadenador de actualización en cola no son compatibles. Reemplace la instrucción **MERGE** con una instrucción de inserción o de actualización.  


## <a name="trigger-implementation"></a>Implementación de desencadenadores

Para cada acción de inserción, actualización o eliminación especificada en la instrucción MERGE, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] activa los desencadenadores AFTER correspondientes definidos en la tabla de destino, pero no garantiza qué acción activará los desencadenadores primero o último. Los desencadenadores definidos para la misma acción cumplen el orden que especifique. Para más información sobre cómo establecer el orden de activación de los desencadenadores, vea [Especificar el primer y el último desencadenador](../../relational-databases/triggers/specify-first-and-last-triggers.md).  
  
Si la tabla de destino tiene habilitado un desencadenador INSTEAD OF definido en ella para una acción de inserción, actualización o eliminación realizada por una instrucción MERGE, debe tener habilitado un desencadenador INSTEAD OF para todas las acciones especificadas en la instrucción MERGE.  
  
Si se ha definido un desencadenador INSTEAD OF UPDATE o INSTEAD OF DELETE en *target_table*, las operaciones de actualización o eliminación no se ejecutan. En su lugar, se activan los desencadenadores y las tablas **inserted** y **deleted** se rellenan en consecuencia.  
  
Si se definen desencadenadores INSTEAD OF INSERT en *target_table*, la operación de inserción no se realiza. En su lugar, la tabla se rellena en consecuencia.  
  
## <a name="permissions"></a>Permisos

Requiere el permiso SELECT en la tabla de origen y los permisos INSERT, UPDATE o DELETE en la tabla de destino. Para más información, consulte la sección Permisos de los artículos [SELECT](../../t-sql/queries/select-transact-sql.md), [INSERT](../../t-sql/statements/insert-transact-sql.md), [UPDATE](../../t-sql/queries/update-transact-sql.md) y [DELETE](../../t-sql/statements/delete-transact-sql.md).  
  
## <a name="optimizing-merge-statement-performance"></a>Optimizar el rendimiento de la instrucción MERGE

Mediante la instrucción MERGE, puede reemplazar las instrucciones DML individuales con una instrucción única. Esto puede mejorar el rendimiento de las consultas debido a que las operaciones se realizan dentro de una instrucción única y, por consiguiente, se reduce el número de veces que se procesan los datos en las tablas de destino y de origen. Sin embargo, las mejoras en el rendimiento dependerán de si hay índices, combinaciones y otras consideraciones correctas en su lugar.

### <a name="index-best-practices"></a>Prácticas recomendadas para índices

Para mejorar el rendimiento de la instrucción MERGE, recomendamos las siguientes instrucciones de índices:

- Cree un índice en las columnas de combinación de la tabla de origen que sea única y de cobertura.
- Cree un índice en un clúster único en las columnas de combinación de la tabla de destino.

Estos índices aseguran que las claves de combinación son únicas y los datos de las tablas están ordenados. Se mejora el rendimiento de las consultas debido a que el optimizador de consultas no necesita realizar un procesamiento de validación adicional para buscar y actualizar filas duplicadas, y no son necesarias operaciones de ordenación adicionales.

### <a name="join-best-practices"></a>Prácticas recomendadas para JOIN

Para mejorar el rendimiento de la instrucción MERGE y asegurarse de que se obtienen los resultados correctos, recomendamos las siguientes instrucciones de combinación:

- Especifique únicamente las condiciones de búsqueda en la cláusula ON <merge_search_condition> que determinan los criterios para que coincidan los datos en las tablas de origen y de destino. Es decir, especifique solo las columnas de la tabla de destino que se comparan con las correspondientes columnas de la tabla de origen. 
- No incluya comparaciones a otros valores como una constante.

Para filtrar las filas de las tablas de origen o de destino, use uno de los métodos siguientes.

- Especifique la condición de búsqueda para el filtrado de filas en la cláusula WHEN adecuada. Por ejemplo, WHEN NOT MATCHED AND S.EmployeeName LIKE 'S%' THEN INSERT...
- Defina una vista en el origen o destino que devuelva las filas filtradas y haga referencia a la vista como la tabla de origen o de destino. Si se define la vista en la tabla de destino, cualquier acción con ella debe satisfacer las condiciones para actualizar las vistas. Para obtener más información acerca de cómo actualizar datos mediante una vista, consulte Modificar datos mediante una vista.
- Use la cláusula `WITH <common table expression>` para filtrar filas de las tablas de origen o de destino. Este método es similar a especificar el criterio de búsqueda adicional en la cláusula ON y puede generar resultados incorrectos. Se recomienda evitar el uso de este método o prueba de manera exhaustiva antes de implementarlo.

La operación de combinación en la instrucción MERGE se optimiza de la misma manera que una combinación en una instrucción SELECT. Es decir, cuando SQL Server procesa combinaciones, el optimizador de consultas elige el método más eficaz entre varias posibilidades para procesar la combinación. Cuando el origen y el destino son de tamaño similar y las instrucciones de índice descritas anteriormente se aplican a las tablas de destino y de origen, el plan de consulta más eficaz es un operador de combinación de mezcla. Esto es debido a que ambas tablas se examinan una vez y no hay necesidad de ordenar los datos. Cuando el origen es menor que la tabla de destino, es preferible usar un operador de bucles anidados.

Puede exigir el uso de una combinación concreta especificando la cláusula `OPTION (<query_hint>)` en la instrucción MERGE. Se recomienda no usar la combinación hash como una sugerencia de consulta para las instrucciones MERGE porque este tipo de combinación no usa índices.

### <a name="parameterization-best-practices"></a>Prácticas recomendadas para parametrización

Si una instrucción SELECT, INSERT, UPDATE o DELETE se ejecuta sin parámetros, el optimizador de consultas de SQL Server puede decidir parametrizar la instrucción internamente. Esto indica que todos los valores literales incluidos en la consulta se sustituirán por parámetros. Por ejemplo, la instrucción INSERT dbo.MyTable (Col1, Col2) VALUES (1, 10) se puede implementar internamente como INSERT dbo.MyTable (Col1, Col2) VALUES (@p1, @p2). Este proceso, denominado parametrización simple, aumenta la capacidad del motor relacional para hacer coincidir nuevas instrucciones SQL con los planes de ejecución existentes compilados previamente. Se puede mejorar el rendimiento de las consultas debido a que se reduce la frecuencia de las compilaciones y recompilaciones de la consulta. El optimizador de consultas no aplica el proceso de parametrización simple a las instrucciones MERGE. Por consiguiente, puede que no se realicen las instrucciones MERGE que contienen los valores literales, además de las instrucciones INSERT, DELETE o UPDATE individuales, porque se compila un plan nuevo cada vez que se ejecuta la instrucción MERGE.

Para mejorar el rendimiento de las consultas, recomendamos las siguientes instrucciones de parametrización:

- Parametrice todos los valores literales en la cláusula `ON <merge_search_condition>` y en las cláusulas `WHEN` de la instrucción MERGE. Por ejemplo, puede incorporar la instrucción MERGE en un procedimiento almacenado que reemplaza los valores literales con parámetros de entrada adecuados.
- Si no puede parametrizar la instrucción, cree una guía de plan de tipo `TEMPLATE` y especifique la sugerencia de consulta `PARAMETERIZATION FORCED` en la guía de plan.
- Si las instrucciones MERGE se ejecutan con frecuencia en la base de datos, considere la posibilidad de establecer en FORCED la opción PARAMETERIZATION en la base de datos. Actúe con precaución cuando establezca esta opción. La opción `PARAMETERIZATION` es un valor de nivel de base de datos y afecta a la manera en que se procesan todas las consultas a la base de datos.

### <a name="top-clause-best-practices"></a>Prácticas recomendadas para la cláusula TOP

En la instrucción MERGE, la cláusula TOP especifica el número o porcentaje de filas afectadas después de que la tabla de origen y la tabla de destino se combinen, y después de quitar las filas que no cumplen los requisitos para una acción de inserción, actualización o eliminación. La cláusula TOP reduce aún más el número de filas combinadas al valor especificado y se aplican las acciones de inserción, actualización o eliminación a las filas combinadas restantes de una manera desordenada. Es decir, no hay ningún orden en el que las filas se distribuyan entre las acciones definidas en las cláusulas WHEN. Por ejemplo, cuando se especifica TOP (10) afecta a 10 filas; de estas filas, 7 se pueden actualizar y 3 insertar, o se pueden eliminar 1, actualizar 5 e insertar 4, etc.

Es habitual usar la cláusula TOP para realizar operaciones del lenguaje de manipulación de datos (DML) en una tabla grande en lotes. Cuando se usa la cláusula TOP en la instrucción MERGE con este fin, es importante comprender las implicaciones siguientes.

- Puede afectar al rendimiento de la E/S.

  La instrucción MERGE realiza exámenes de tabla completos, tanto de las tablas de destino como de origen. Al dividir la operación en lotes, se reduce el número de operaciones de escritura realizadas por lote; sin embargo, cada lote realiza exámenes de tabla completos, tanto de las tablas de destino como de origen. La actividad de lectura resultante puede afectar al rendimiento de la consulta.

- Se pueden producir resultados incorrectos.

  Es importante asegurarse de que todos los lotes sucesivos se destinen a filas nuevas o puede producirse un comportamiento no deseado como la inserción incorrecta de filas duplicadas en la tabla de destino. Esto puede ocurrir cuando la tabla de origen incluye una fila que no estaba en un lote de destino pero estaba en la tabla de destino total.

- Para asegurar que los resultados son correctos:

  - Use la cláusula ON para determinar qué filas de origen afectan a las filas de destino existentes y cuáles son auténticamente nuevas.
  - Use una condición adicional en la cláusula WHEN MATCHED para determinar si un lote anterior ya ha actualizado la fila de destino.

Dado que la cláusula TOP solo se aplica una vez aplicadas estas cláusulas, cada ejecución inserta una fila no coincidente inigualable o actualiza una fila existente.

### <a name="bulk-load-best-practices"></a>Prácticas recomendadas para carga masiva

La instrucción MERGE se puede usar para cargar eficazmente datos de manera masiva del archivo de datos de origen en una tabla de destino especificando la cláusula `OPENROWSET(BULK…)` como el origen de la tabla. De esta forma, el archivo completo se procesa en un lote único.

Para mejorar el rendimiento del proceso de mezcla masiva, recomendamos las siguientes instrucciones:

- Cree un índice clúster en las columnas de combinación de la tabla de destino.
- Use las sugerencias ORDER y UNIQUE en la cláusula `OPENROWSET(BULK…)` para especificar cómo se ordena el archivo de datos de origen.

  De forma predeterminada, la operación masiva presupone que los datos del archivo no están ordenados. Por consiguiente, es importante que los datos del origen estén ordenados según el índice clúster de la tabla de destino y que la sugerencia ORDER se use para indicar el orden, de manera que el optimizador de consultas pueda generar un plan de consultas más eficaz. Las sugerencias se validan en tiempo de ejecución; si el flujo de datos no se ajusta a las sugerencias especificadas, se produce un error.

Estas instrucciones garantizan que las claves de unión son únicas y que el criterio de ordenación de los datos en el archivo de origen coincide con la tabla de destino. Se mejora el rendimiento de las consultas debido a que las operaciones de ordenación adicionales no son necesarias y no se requieren copias innecesarias de datos.

### <a name="measuring-and-diagnosing-merge-performance"></a>Medir y diagnosticar el rendimiento de MERGE

Las características siguientes están disponibles para ayudarle a medir y diagnosticar el rendimiento de las instrucciones MERGE.

- Use el contador merge stmt en la vista de administración dinámica [sys.dm_exec_query_optimizer_info](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-optimizer-info-transact-sql.md) para devolver el número de optimizaciones de consulta para las instrucciones MERGE.
- Use el atributo merge_action_type en la vista de administración dinámica [sys.dm_exec_plan_attributes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-plan-attributes-transact-sql.md) para devolver el tipo de plan de ejecución de desencadenadores que se usa como el resultado de una instrucción MERGE.
- Use Seguimiento de SQL para recopilar datos de la solución de problemas de la instrucción MERGE de la misma manera que haría para otras instrucciones del lenguaje de manipulación de datos (DML). Para más información, consulte [SQL Trace](../../relational-databases/sql-trace/sql-trace.md).

## <a name="examples"></a>Ejemplos  

### <a name="a-using-merge-to-do-insert-and-update-operations-on-a-table-in-a-single-statement"></a>A. Usar MERGE para realizar operaciones INSERT y UPDATE en una tabla en una sola instrucción

Un escenario común es la actualización de una o más columnas de una tabla si existe una línea coincidente. O bien, con la inserción de los datos como una nueva fila si no existe una fila coincidente. Normalmente, se realiza cualquiera de los dos escenarios mediante el paso de los parámetros a un procedimiento almacenado que contiene las instrucciones INSERT y UPDATE adecuadas. Con la instrucción MERGE puede realizar ambas tareas en una sola instrucción. En el ejemplo siguiente se muestra un procedimiento almacenado de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] que contiene una instrucción INSERT y una instrucción UPDATE. A continuación, el procedimiento se modifica para ejecutar las operaciones equivalentes utilizando una sola instrucción MERGE.  
  
```sql  
CREATE PROCEDURE dbo.InsertUnitMeasure  
    @UnitMeasureCode nchar(3),  
    @Name nvarchar(25)  
AS
BEGIN  
    SET NOCOUNT ON;  
-- Update the row if it exists.
    UPDATE Production.UnitMeasure  
SET Name = @Name  
WHERE UnitMeasureCode = @UnitMeasureCode  
-- Insert the row if the UPDATE statement failed.  
IF (@@ROWCOUNT = 0 )  
BEGIN  
    INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name)  
    VALUES (@UnitMeasureCode, @Name)  
END  
END;  
GO  
-- Test the procedure and return the results.  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'Test Value';  
SELECT UnitMeasureCode, Name FROM Production.UnitMeasure  
WHERE UnitMeasureCode = 'ABC';  
GO  
  
-- Rewrite the procedure to perform the same operations using the
-- MERGE statement.  
-- Create a temporary table to hold the updated or inserted values
-- from the OUTPUT clause.  
CREATE TABLE #MyTempTable  
    (ExistingCode nchar(3),  
     ExistingName nvarchar(50),  
     ExistingDate datetime,  
     ActionTaken nvarchar(10),  
     NewCode nchar(3),  
     NewName nvarchar(50),  
     NewDate datetime  
    );  
GO  
ALTER PROCEDURE dbo.InsertUnitMeasure  
    @UnitMeasureCode nchar(3),  
    @Name nvarchar(25)  
AS
BEGIN  
    SET NOCOUNT ON;  
  
    MERGE Production.UnitMeasure AS target  
    USING (SELECT @UnitMeasureCode, @Name) AS source (UnitMeasureCode, Name)  
    ON (target.UnitMeasureCode = source.UnitMeasureCode)  
    WHEN MATCHED THEN
        UPDATE SET Name = source.Name  
    WHEN NOT MATCHED THEN  
        INSERT (UnitMeasureCode, Name)  
        VALUES (source.UnitMeasureCode, source.Name)  
    OUTPUT deleted.*, $action, inserted.* INTO #MyTempTable;  
END;  
GO  
-- Test the procedure and return the results.  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'New Test Value';  
EXEC InsertUnitMeasure @UnitMeasureCode = 'XYZ', @Name = 'Test Value';  
EXEC InsertUnitMeasure @UnitMeasureCode = 'ABC', @Name = 'Another Test Value';  
  
SELECT * FROM #MyTempTable;  
-- Cleanup
DELETE FROM Production.UnitMeasure WHERE UnitMeasureCode IN ('ABC','XYZ');  
DROP TABLE #MyTempTable;  
GO  
```  
  
### <a name="b-using-merge-to-do-update-and-delete-operations-on-a-table-in-a-single-statement"></a>B. Usar MERGE para realizar operaciones UPDATE y DELETE en una tabla en una sola instrucción

El siguiente ejemplo usa MERGE para actualizar a diario la tabla `ProductInventory` de la base de datos de muestra [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], en función de los pedidos procesados en la tabla `SalesOrderDetail`. La columna `Quantity` de la tabla `ProductInventory` se actualiza restando el número de pedidos realizados cada día para cada producto de la tabla `SalesOrderDetail`. Si el número de pedidos de un producto baja el nivel de inventario del mismo hasta 0 o un valor menor, la fila correspondiente a ese producto se elimina de la tabla `ProductInventory`.  
  
```sql  
CREATE PROCEDURE Production.usp_UpdateInventory  
    @OrderDate datetime  
AS  
MERGE Production.ProductInventory AS target  
USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
    JOIN Sales.SalesOrderHeader AS soh  
    ON sod.SalesOrderID = soh.SalesOrderID  
    AND soh.OrderDate = @OrderDate  
    GROUP BY ProductID) AS source (ProductID, OrderQty)  
ON (target.ProductID = source.ProductID)  
WHEN MATCHED AND target.Quantity - source.OrderQty <= 0  
    THEN DELETE  
WHEN MATCHED
    THEN UPDATE SET target.Quantity = target.Quantity - source.OrderQty,
                    target.ModifiedDate = GETDATE()  
OUTPUT $action, Inserted.ProductID, Inserted.Quantity,
    Inserted.ModifiedDate, Deleted.ProductID,  
    Deleted.Quantity, Deleted.ModifiedDate;  
GO  
  
EXECUTE Production.usp_UpdateInventory '20030501'  
```  
  
### <a name="c-using-merge-to-do-update-and-insert-operations-on-a-target-table-by-using-a-derived-source-table"></a>C. Usar MERGE para realizar operaciones INSERT y UPDATE en una tabla de destino mediante una tabla de origen derivada

En el ejemplo siguiente se usa MERGE para modificar la tabla `SalesReason` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], actualizando o insertando las filas. Cuando el valor de `NewName` de la tabla de origen coincide con un valor de la columna `Name` de la tabla de destino, (`SalesReason`), la columna `ReasonType` se actualiza en la tabla de destino. Cuando el valor de `NewName` no coincide, la fila de origen se inserta en la tabla de destino. La tabla de origen es una tabla derivada que usa la característica de constructor con valores de tabla de [!INCLUDE[tsql](../../includes/tsql-md.md)] para especificar varias filas en la tabla de origen. Para saber más sobre cómo usar el constructor con valores de tabla en una tabla derivada, vea [Constructor con valores de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md). El ejemplo también muestra cómo almacenar los resultados de la cláusula OUTPUT en una variable de tabla. Y, después, se resumen los resultados de la instrucción MERGE mediante la ejecución de una simple operación de selección que devuelve el número de filas insertadas y actualizadas.  
  
```sql  
-- Create a temporary table variable to hold the output actions.  
DECLARE @SummaryOfChanges TABLE(Change VARCHAR(20));  
  
MERGE INTO Sales.SalesReason AS Target  
USING (VALUES ('Recommendation','Other'), ('Review', 'Marketing'),
              ('Internet', 'Promotion'))  
       AS Source (NewName, NewReasonType)  
ON Target.Name = Source.NewName  
WHEN MATCHED THEN  
UPDATE SET ReasonType = Source.NewReasonType  
WHEN NOT MATCHED BY TARGET THEN  
INSERT (Name, ReasonType) VALUES (NewName, NewReasonType)  
OUTPUT $action INTO @SummaryOfChanges;  
  
-- Query the results of the table variable.  
SELECT Change, COUNT(*) AS CountPerChange  
FROM @SummaryOfChanges  
GROUP BY Change;  
```  
  
### <a name="d-inserting-the-results-of-the-merge-statement-into-another-table"></a>D. Insertar los resultados de la instrucción MERGE en otra tabla

En el ejemplo siguiente se capturan los datos devueltos por la cláusula OUTPUT de una instrucción MERGE y se insertan en otra tabla. La instrucción MERGE actualiza diariamente la columna `Quantity` de la tabla `ProductInventory` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], en función de los pedidos procesados en la tabla `SalesOrderDetail`. En el ejemplo se capturan las filas actualizadas y se insertan en otra tabla que se usa para realizar el seguimiento de los cambios del inventario.  
  
```sql  
CREATE TABLE Production.UpdatedInventory  
    (ProductID INT NOT NULL, LocationID int, NewQty int, PreviousQty int,  
     CONSTRAINT PK_Inventory PRIMARY KEY CLUSTERED (ProductID, LocationID));  
GO  
INSERT INTO Production.UpdatedInventory  
SELECT ProductID, LocationID, NewQty, PreviousQty
FROM  
(    MERGE Production.ProductInventory AS pi  
     USING (SELECT ProductID, SUM(OrderQty)
            FROM Sales.SalesOrderDetail AS sod  
            JOIN Sales.SalesOrderHeader AS soh  
            ON sod.SalesOrderID = soh.SalesOrderID  
            AND soh.OrderDate BETWEEN '20030701' AND '20030731'  
            GROUP BY ProductID) AS src (ProductID, OrderQty)  
     ON pi.ProductID = src.ProductID  
    WHEN MATCHED AND pi.Quantity - src.OrderQty >= 0
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0
        THEN DELETE  
    OUTPUT $action, Inserted.ProductID, Inserted.LocationID,
        Inserted.Quantity AS NewQty, Deleted.Quantity AS PreviousQty)  
 AS Changes (Action, ProductID, LocationID, NewQty, PreviousQty)
 WHERE Action = 'UPDATE';  
GO  
```  

### <a name="e-using-merge-to-do-insert-or-update-on-a-target-edge-table-in-a-graph-database"></a>E. Usar MERGE para realizar una operación INSERT o UPDATE en una tabla perimetral de destino en una base de datos de gráficos

En este ejemplo, se crean las tablas de nodo `Person` y `City`, así como una tabla perimetral `livesIn`. Utilice la instrucción MERGE en la tabla perimetral `livesIn` para insertar una fila nueva si aún no existe el perímetro entre `Person` y `City`. Si ya existe el perímetro, simplemente se actualiza el atributo StreetAddress en la tabla perimetral `livesIn`.

```sql
-- CREATE node and edge tables
CREATE TABLE Person
    (
        ID INTEGER PRIMARY KEY,
        PersonName VARCHAR(100)
    )
AS NODE
GO

CREATE TABLE City
    (
        ID INTEGER PRIMARY KEY,
        CityName VARCHAR(100),
        StateName VARCHAR(100)
    )
AS NODE
GO

CREATE TABLE livesIn
    (
        StreetAddress VARCHAR(100)
    )
AS EDGE
GO

-- INSERT some test data into node and edge tables
INSERT INTO Person VALUES (1, 'Ron'), (2, 'David'), (3, 'Nancy')
GO

INSERT INTO City VALUES (1, 'Redmond', 'Washington'), (2, 'Seattle', 'Washington')
GO

INSERT livesIn SELECT P.$node_id, C.$node_id, c
FROM Person P, City C, (values (1,1, '123 Avenue'), (2,2,'Main Street')) v(a,b,c)
WHERE P.id = a AND C.id = b
GO

-- Use MERGE to update/insert edge data
CREATE OR ALTER PROCEDURE mergeEdge
    @PersonId integer,
    @CityId integer,
    @StreetAddress varchar(100)
AS
BEGIN
    MERGE livesIn
        USING ((SELECT @PersonId, @CityId, @StreetAddress) AS T (PersonId, CityId, StreetAddress)
                JOIN Person ON T.PersonId = Person.ID
                JOIN City ON T.CityId = City.ID)
        ON MATCH (Person-(livesIn)->City)
    WHEN MATCHED THEN
        UPDATE SET StreetAddress = @StreetAddress
    WHEN NOT MATCHED THEN
        INSERT ($from_id, $to_id, StreetAddress)
        VALUES (Person.$node_id, City.$node_id, @StreetAddress) ;
END
GO

-- Following will insert a new edge in the livesIn edge table
EXEC mergeEdge 3, 2, '4444th Avenue'
GO

-- Following will update the StreetAddress on the edge that connects Ron to Redmond
EXEC mergeEdge 1, 1, '321 Avenue'
GO

-- Verify that all the address were added/updated correctly
SELECT PersonName, CityName, StreetAddress
FROM Person , City , livesIn
WHERE MATCH(Person-(livesIn)->city)
GO
```
  
## <a name="see-also"></a>Consulte también

- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
- [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)
- [OUTPUT Clause &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)
- [MERGE en paquetes de Integration Services](../../integration-services/control-flow/merge-in-integration-services-packages.md)
- [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)
- [Table Value Constructor &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md) (Constructor con valores de tabla [Transact-SQL])  
