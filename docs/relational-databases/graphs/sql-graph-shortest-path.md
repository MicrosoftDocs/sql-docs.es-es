---
description: SHORTEST_PATH (Transact-SQL)
title: Ruta de acceso más corta (gráfico SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/01/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
f1_keywords:
- SHORTEST PATH
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-ver15||=azuresqldb-mi-current
ms.openlocfilehash: 9e5a6f6506e4ce40b3b89f32dd58329eb4667760
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351400"
---
# <a name="shortest_path-transact-sql"></a>SHORTEST_PATH (Transact-SQL)

[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

  Especifica una condición de búsqueda para un gráfico, que se busca de forma recursiva o repetida. SHORTEST_PATH puede usarse dentro de la coincidencia con las tablas perimetrales y de nodo de Graph, en la instrucción SELECT. 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="shortest-path"></a>Ruta más corta
La función SHORTEST_PATH le permite encontrar:    
* Una ruta de acceso más corta entre dos nodos o entidades determinados
* Rutas de acceso más cortas de origen único.
* Ruta más corta de varios nodos de origen a varios nodos de destino.

Toma un patrón de longitud arbitraria como entrada y devuelve una ruta más corta que existe entre dos nodos. Esta función solo se puede usar dentro de la coincidencia. La función devuelve solo una ruta más corta entre dos nodos dados. Si existe, dos o más rutas de acceso más cortas de la misma longitud entre cualquier par de nodos de origen y de destino, la función devuelve solo una ruta de acceso que se encontró en primer lugar durante el recorrido. Tenga en cuenta que, un patrón de longitud arbitraria solo puede especificarse dentro de una función SHORTEST_PATH. 

Consulte la sintaxis de la [coincidencia (gráfico de SQL)](../../t-sql/queries/match-sql-graph.md) . 

## <a name="for-path"></a>PARA RUTA DE ACCESO
PARA PATH debe usarse con cualquier nombre de tabla perimetral o de nodo en la cláusula FROM, que participará en un patrón de longitud arbitraria. FOR PATH indica al motor que el nodo o la tabla perimetral devolverá una colección ordenada que representa la lista de nodos o bordes que se han encontrado a lo largo de la ruta de acceso que se atraviesa. Los atributos de estas tablas no se pueden proyectar directamente en la cláusula SELECT. Para proyectar los atributos de estas tablas, se deben usar las funciones de agregado de trazado de gráficos.  

## <a name="arbitrary-length-pattern"></a>Patrón de longitud arbitraria
Este patrón incluye los nodos y bordes que se deben atravesar repetidamente hasta que se alcance el nodo deseado o hasta que se cumpla el número máximo de iteraciones especificado en el patrón. Cada vez que se ejecuta la consulta, el resultado de la ejecución de este patrón será una colección ordenada de los nodos y los bordes recorridos a lo largo de la ruta de acceso desde el nodo de inicio hasta el nodo final. Se trata de un patrón de sintaxis de estilo de expresión regular y se admiten los siguientes dos cuantificadores de patrón:

* **' + '**: Repetir el patrón 1 o más veces. Finaliza en cuanto encuentra una ruta de acceso más corta.
* **{1, n}** : repite el patrón de 1 a "n" veces. Finaliza en cuanto se encuentra un más corto.

## <a name="last_node"></a>LAST_NODE
La función LAST_NODE () permite encadenar dos patrones de recorrido de longitud arbitraria. Se puede usar en escenarios donde:    
* En una consulta se utilizan más patrones de ruta de acceso más cortos y un patrón comienza en el último nodo del patrón anterior.
* Dos patrones de trazado más cortos se combinan en el mismo LAST_NODE ().

## <a name="graph-path-order"></a>Orden de rutas de grafos
El orden de las rutas de acceso de los gráficos hace referencia al orden de los datos de la ruta de acceso de salida. El orden de la ruta de acceso de salida siempre comienza en la parte no recursiva del patrón seguido de los nodos y bordes que aparecen en la parte recursiva. El orden en el que se recorre el gráfico durante la optimización/ejecución de la consulta no tiene nada que consultar con el orden impreso en la salida. Del mismo modo, la dirección de la flecha en el patrón recursivo tampoco afecta al orden de la ruta de acceso del gráfico. 

## <a name="graph-path-aggregate-functions"></a>Funciones de agregado de trazado de grafos
Puesto que los nodos y los bordes implicados en el patrón de longitud arbitraria devuelven una colección (de los nodos y los bordes recorridos en esa ruta de acceso), los usuarios no pueden proyectar los atributos directamente mediante la sintaxis de TableName. attributeName convencional. En el caso de las consultas en las que es necesario proyectar los valores de atributo del nodo intermedio o las tablas perimetrales, en la ruta de acceso recorrida, use las siguientes funciones de agregado de ruta de acceso de gráfico: STRING_AGG, LAST_VALUE, SUM, AVG, MIN, MAX y COUNT. La sintaxis general para usar estas funciones de agregado en la cláusula SELECT es la siguiente:

```syntaxsql
<GRAPH_PATH_AGGREGATE_FUNCTION>(<expression> , <separator>)  <order_clause>

    <order_clause> ::=
        { WITHIN GROUP (GRAPH PATH) }

    <GRAPH_PATH_AGGREGATE_FUNCTION> ::=
          STRING_AGG
        | LAST_VALUE
        | SUM
        | COUNT
        | AVG
        | MIN
        | MAX

```

### <a name="string_agg"></a>STRING_AGG
La función STRING_AGG toma una expresión y un separador como entrada y devuelve una cadena. Los usuarios pueden usar esta función en la cláusula SELECT para proyectar los atributos de los nodos o bordes intermedios de la ruta de acceso que se atraviesa. 

### <a name="last_value"></a>LAST_VALUE
Para proyectar los atributos del último nodo de trazado recorrido, se puede usar LAST_VALUE función de agregado. Es un error proporcionar el alias de la tabla irregular como entrada a esta función, solo se pueden usar nombres de tabla o alias de nodo.

**Último nodo**: el último nodo hace referencia al nodo que aparece en último lugar en la ruta de acceso recorrida, independientemente de la dirección de la flecha en el predicado de coincidencia. Por ejemplo: `MATCH(SHORTEST_PATH(n(-(e)->p)+) )`. Aquí el último nodo de la ruta de acceso será el último nodo P visitado. 

Mientras que el último nodo es el último nodo de la ruta de acceso del gráfico de salida para este patrón: `MATCH(SHORTEST_PATH((n<-(e)-)+p))`    

### <a name="sum"></a>SUM
Esta función devuelve la suma de los valores de atributo de nodo o perimetral proporcionados o la expresión que aparecía en la ruta de acceso recorrida.

### <a name="count"></a>COUNT
Esta función devuelve el número de valores no NULL del atributo node/Edge deseado en la ruta de acceso. La función COUNT admite el \* operador ' ' con un nodo o un alias de tabla perimetral. Sin el alias de tabla perimetral o de nodo, el uso de \* es ambiguo y producirá un error.

```syntaxsql
{  COUNT( <expression> | <node_or_edge_alias>.* )  <order_clause>  }
```

### <a name="avg"></a>MEDIA
Devuelve el promedio de los valores de atributo de nodo o perimetral proporcionados o la expresión que aparecía en la ruta de acceso recorrida.

### <a name="min"></a>MÍN
Devuelve el valor mínimo de los valores de atributo de nodo o perimetral proporcionados o la expresión que aparecía en la ruta de acceso recorrida.

### <a name="max"></a>MÁX
Devuelve el valor máximo de los valores de atributo de nodo o perimetral proporcionados o la expresión que aparecía en la ruta de acceso recorrida.

## <a name="remarks"></a>Comentarios  
shortest_path función solo se puede usar dentro de la coincidencia.     
LAST_NODE solo se admite dentro de shortest_path.     
No se admite la búsqueda de una ruta más corta ponderada, ni todas las rutas de acceso o todas las rutas más cortas.         
En algunos casos, se pueden generar planes incorrectos para las consultas con un número mayor de saltos, lo que da lugar a tiempos de ejecución de consultas mayores. El uso de una sugerencia de combinación hash puede servir de ayuda.    


## <a name="examples"></a>Ejemplos 
En las consultas de ejemplo que se muestran aquí, vamos a usar las tablas node y Edge creadas en el [ejemplo de SQL Graph](./sql-graph-sample.md) .

### <a name="a--find-shortest-path-between-2-people"></a>A.  Buscar la ruta más corta entre 2 personas
 En el ejemplo siguiente, encontramos la ruta más corta entre Jacob y Alice. Necesitaremos el nodo person y el perímetro Friend creado a partir del script de ejemplo Graph. 

```sql
SELECT PersonName, Friends
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

 ### <a name="b--find-shortest-path-from-a-given-node-to-all-other-nodes-in-the-graph"></a>B.  Buscar la ruta más corta de un nodo determinado a todos los demás nodos del gráfico. 
 En el ejemplo siguiente se buscan todas las personas a las que está conectado Jacob en el gráfico y la ruta más corta a partir de Jacob a todas esas personas. 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
AND Person1.name = 'Jacob'
```

### <a name="c--count-the-number-of-hopslevels-traversed-to-go-from-one-person-to-another-in-the-graph"></a>C.  Cuente el número de saltos y niveles que se recorren para pasar de una persona a otra en el gráfico.
 En el ejemplo siguiente se busca la ruta de acceso más corta entre Jacob y Alice y se imprime el número de saltos que se tarda en pasar de Jacob a Alice. 

```sql
 SELECT PersonName, Friends, levels
FROM (  
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        LAST_VALUE(Person2.name) WITHIN GROUP (GRAPH PATH) AS LastNode,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2)+))
    AND Person1.name = 'Jacob'
) AS Q
WHERE Q.LastNode = 'Alice'
```

### <a name="d-find-people-1-3-hops-away-from-a-given-person"></a>D. Buscar personas 1-3 saltos fuera de una persona determinada
En el ejemplo siguiente se busca la ruta de acceso más corta entre Jacob y todas las personas a las que está conectado en el gráfico 1-3 saltos fuera de ella. 

```sql
SELECT
    Person1.name AS PersonName, 
    STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends
FROM
    Person AS Person1,
    friendOf FOR PATH AS fo,
    Person FOR PATH  AS Person2
WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
AND Person1.name = 'Jacob'
```

### <a name="e-find-people-exactly-2-hops-away-from-a-given-person"></a>E. Buscar personas exactamente 2 saltos fuera de una persona determinada
En el ejemplo siguiente se busca la ruta más corta entre Jacob y las personas que tienen exactamente 2 saltos en el gráfico. 

```sql
SELECT PersonName, Friends
FROM (
    SELECT
        Person1.name AS PersonName, 
        STRING_AGG(Person2.name, '->') WITHIN GROUP (GRAPH PATH) AS Friends,
        COUNT(Person2.name) WITHIN GROUP (GRAPH PATH) AS levels
    FROM
        Person AS Person1,
        friendOf FOR PATH AS fo,
        Person FOR PATH  AS Person2
    WHERE MATCH(SHORTEST_PATH(Person1(-(fo)->Person2){1,3}))
    AND Person1.name = 'Jacob'
) Q
WHERE Q.levels = 2
```

## <a name="see-also"></a>Consulte también  
 [COINCIDENCIA (gráfico SQL)](../../t-sql/queries/match-sql-graph.md)    
 [CREATE TABLE &#40;SQL Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT (SQL Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Graph processing with SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md) (Procesamiento de gráficos con SQL Server 2017)     
 
