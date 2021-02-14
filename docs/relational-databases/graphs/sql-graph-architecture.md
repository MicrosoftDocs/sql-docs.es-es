---
description: Arquitectura de SQL Graph
title: Arquitectura de SQL Graph | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: language-reference
helpviewer_keywords:
- SQL graph
- SQL graph, architecture
ms.assetid: ''
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab08692b1e221e589cc6283e2800a695044f79b7
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100351410"
---
# <a name="sql-graph-architecture"></a>Arquitectura de SQL Graph  
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb-asdbmi.md)]

Obtenga información sobre cómo se diseña SQL Graph. Conocer los aspectos básicos le facilitará la comprensión de otros artículos de SQL Graph.
 
## <a name="sql-graph-database"></a>Base de datos de SQL Graph
Los usuarios pueden crear un grafo por cada base de datos. Un gráfico es una colección de tablas perimetrales y de nodo. Las tablas de nodo o perimetrales se pueden crear en cualquier esquema de la base de datos, pero todos pertenecen a un gráfico lógico. Una tabla de nodos es una colección de tipo de nodos similar. Por ejemplo, una tabla de nodos de persona contiene todos los nodos de persona que pertenecen a un gráfico. Del mismo modo, una tabla irregular es una colección de tipo de bordes similar. Por ejemplo, una tabla de borde de amigos contiene todos los bordes que conectan a una persona con otra persona. Como los nodos y los bordes se almacenan en tablas, la mayoría de las operaciones admitidas en las tablas normales se admiten en tablas de nodo o perimetrales. 
 
 
![Diagrama que muestra la arquitectura de base de datos de SQL Graph.](../../relational-databases/graphs/media/sql-graph-architecture.png "Arquitectura de base de datos de SQL Graph")   

Figura 1: arquitectura de base de datos de SQL Graph
 
## <a name="node-table"></a>Tabla de nodos
Una tabla de nodos representa una entidad en un esquema de gráfico. Cada vez que se crea una tabla de nodos, junto con las columnas definidas por el usuario, `$node_id` se crea una columna implícita que identifica de forma única un nodo determinado en la base de datos. Los valores de `$node_id` se generan automáticamente y son una combinación de `object_id` de esa tabla de nodos y un valor BIGINT generado internamente. Sin embargo, cuando `$node_id` se selecciona la columna, se muestra un valor calculado en forma de cadena JSON. Además, `$node_id` es una pseudo columna, que se asigna a un nombre interno con una cadena hexadecimal. Al seleccionar `$node_id` en la tabla, el nombre de la columna aparecerá como `$node_id_<hex_string>` . El uso de nombres de pseudo columnas en las consultas es el método recomendado para realizar consultas en la `$node_id` columna interna y debe evitarse el uso de un nombre interno con una cadena hexadecimal.

Se recomienda que los usuarios creen una restricción o índice único en la `$node_id` columna en el momento de la creación de la tabla de nodos, pero si no se crea una, se creará automáticamente un índice no clúster único predeterminado. Sin embargo, se crea cualquier índice en una columna de gráfico en las columnas internas subyacentes. Es decir, un índice creado en la `$node_id` columna aparecerá en la `graph_id_<hex_string>` columna interna.   


## <a name="edge-table"></a>Tabla irregular
Una tabla irregular representa una relación en un gráfico. Los bordes siempre se dirigen y conectan dos nodos. Una tabla irregular permite a los usuarios modelar las relaciones de varios a varios en el gráfico. Una tabla irregular puede tener o no atributos definidos por el usuario. Cada vez que se crea una tabla irregular, junto con los atributos definidos por el usuario, se crean tres columnas implícitas en la tabla irregular:

|Nombre de la columna    |Descripción  |
|---   |---  |
|`$edge_id`   |Identifica de forma única un borde determinado en la base de datos. Es una columna generada y el valor es una combinación de object_id de la tabla irregular y un valor BIGINT generado internamente. Sin embargo, cuando `$edge_id` se selecciona la columna, se muestra un valor calculado en forma de cadena JSON. `$edge_id` es una pseudo columna, que se asigna a un nombre interno con una cadena hexadecimal en ella. Al seleccionar `$edge_id` en la tabla, el nombre de la columna aparecerá como `$edge_id_\<hex_string>` . El uso de nombres de pseudo columnas en las consultas es el método recomendado para realizar consultas en la `$edge_id` columna interna y debe evitarse el uso de un nombre interno con una cadena hexadecimal. |
|`$from_id`   |Almacena el `$node_id` del nodo en el que se origina el borde.  |
|`$to_id`   |Almacena el `$node_id` del nodo, en el que finaliza el borde. |

Los nodos a los que se puede conectar un borde determinado se rigen por los datos insertados en las `$from_id` `$to_id` columnas y. En la primera versión, no es posible definir restricciones en la tabla irregular para impedir que se conecten dos tipos de nodos cualesquiera. Es decir, un borde puede conectar dos nodos cualesquiera en el gráfico, independientemente de sus tipos.

De forma similar a la `$node_id` columna, se recomienda que los usuarios creen un índice o una restricción únicos en la `$edge_id` columna en el momento de la creación de la tabla irregular, pero si no se crea ninguno, se creará automáticamente un índice no clúster único predeterminado en esta columna. Sin embargo, se crea cualquier índice en una columna de gráfico en las columnas internas subyacentes. Es decir, un índice creado en la `$edge_id` columna aparecerá en la `graph_id_<hex_string>` columna interna. También se recomienda que los usuarios creen un índice en las `$from_id` columnas (, `$to_id` ) para las búsquedas más rápidas en la dirección del borde.  

En la figura 2 se muestra cómo se almacenan en la base de datos las tablas perimetrales y de nodo. 

![Diagrama que muestra la representación de la tabla perimetral y el nodo.](../../relational-databases/graphs/media/person-friends-tables.png "Tablas de los límites de amigos y nodos de personas")   

Figura 2: representación de la tabla perimetral y de nodo



## <a name="metadata"></a>Metadatos
Use estas vistas de metadatos para ver los atributos de un nodo o una tabla perimetral.
 
### <a name="systables"></a>sys.tables
El siguiente tipo de bits nuevo, se agregarán columnas a SYS. Tablas. Si `is_node` se establece en 1, indica que la tabla es una tabla de nodos y si `is_edge` está establecida en 1, que indica que la tabla es una tabla irregular.
 
|Nombre de columna |Tipo de datos |Descripción |
|--- |---|--- |
|is_node |bit |1 = esta es una tabla de nodos |
|is_edge |bit |1 = se trata de una tabla irregular |
 
### <a name="syscolumns"></a>sys.columns
La `sys.columns` vista contiene columnas adicionales `graph_type` y `graph_type_desc` , que indican el tipo de la columna en las tablas de nodos y perimetrales.
 
|Nombre de columna |Tipo de datos |Descripción |
|--- |---|--- |
|graph_type |int |Columna interna con un conjunto de valores. Los valores se encuentran entre 1-8 para las columnas del gráfico y NULL para los demás.  |
|graph_type_desc |nvarchar(60)  |columna interna con un conjunto de valores |
 
En la tabla siguiente se enumeran los valores válidos para la `graph_type` columna

|Valor de columna  |Descripción  |
|---   |---   |
|1  |GRAPH_ID  |
|2  |GRAPH_ID_COMPUTED  |
|3  |GRAPH_FROM_ID  |
|4  |GRAPH_FROM_OBJ_ID  |
|5  |GRAPH_FROM_ID_COMPUTED  |
|6  |GRAPH_TO_ID  |
|7  |GRAPH_TO_OBJ_ID  |
|8  |GRAPH_TO_ID_COMPUTED  |


`sys.columns` también almacena información sobre las columnas implícitas creadas en tablas de nodo o perimetrales. La información siguiente se puede recuperar de sys. Columns; sin embargo, los usuarios no pueden seleccionar estas columnas de una tabla de nodo o perimetral. 

Columnas implícitas en una tabla de nodos

|Nombre de columna    |Tipo de datos  |is_hidden  |Comentario  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |`graph_id`columna interna  |
|$node _id_\<hex_string> |NVARCHAR   |0  |Columna de nodo externo `node_id`  |

Columnas implícitas en una tabla irregular

|Nombre de columna    |Tipo de datos  |is_hidden  |Comentario  |
|---  |---|---|---  |
|graph_id_\<hex_string> |bigint |1  |`graph_id`columna interna  |
|$edge _id_\<hex_string> |NVARCHAR   |0  |`edge_id`columna externa  |
|from_obj_id_\<hex_string>  |INT    |1  |interno desde nodo `object_id`  |
|from_id_\<hex_string>  |bigint |1  |Interno desde nodo `graph_id`  |
|$from _id_\<hex_string> |NVARCHAR   |0  |externo desde nodo `node_id`  |
|to_obj_id_\<hex_string>    |INT    |1  |interno a nodo `object_id`  |
|to_id_\<hex_string>    |bigint |1  |Interno a nodo `graph_id`  |
|$to _id_\<hex_string>   |NVARCHAR   |0  |externo a nodo `node_id`  |
 
### <a name="system-functions"></a>Funciones del sistema
Se han agregado las siguientes funciones integradas. Estos ayudarán a los usuarios a extraer información de las columnas generadas. Tenga en cuenta que estos métodos no validarán la entrada del usuario. Si el usuario especifica un no válido `sys.node_id` , el método extraerá la parte adecuada y lo devolverá. Por ejemplo, OBJECT_ID_FROM_NODE_ID tomará `$node_id` como entrada y devolverá el object_id de la tabla, este nodo pertenece a. 
 
|Integrada   |Descripción  |
|---  |---  |
|OBJECT_ID_FROM_NODE_ID |Extraiga el object_id de un `node_id`  |
|GRAPH_ID_FROM_NODE_ID  |Extraiga el graph_id de un `node_id`  |
|NODE_ID_FROM_PARTS |Construir un node_id a partir de un `object_id` y un `graph_id`  |
|OBJECT_ID_FROM_EDGE_ID |Extraer `object_id` de `edge_id`  |
|GRAPH_ID_FROM_EDGE_ID  |Extraer identidad de `edge_id`  |
|EDGE_ID_FROM_PARTS |Construcción `edge_id` de `object_id` e Identity  |



## <a name="transact-sql-reference"></a>Referencia de Transact-SQL 
Obtenga información sobre las [!INCLUDE[tsql-md](../../includes/tsql-md.md)] extensiones introducidas en SQL Server y Azure SQL Database, que permiten crear y consultar objetos de grafos. Las extensiones de lenguaje de consulta ayudan a consultar y recorrer el gráfico mediante la sintaxis de arte ASCII.
 
### <a name="data-definition-language-ddl-statements"></a>Instrucciones del Lenguaje de definición de datos (DDL)

|Tarea   |Artículo relacionado  |Notas
|---  |---  |---  |
|CREATE TABLE |[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-sql-graph.md)|`CREATE TABLE` ahora se ha ampliado para admitir la creación de una tabla como nodo o como borde. Tenga en cuenta que una tabla irregular puede tener o no atributos definidos por el usuario.  |
|ALTER TABLE    |[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)|Las tablas de nodos y perimetrales se pueden modificar de la misma manera que una tabla relacional, mediante `ALTER TABLE` . Los usuarios pueden agregar o modificar columnas, índices o restricciones definidos por el usuario. Sin embargo, la modificación de las columnas internas del gráfico, como `$node_id` o `$edge_id` , producirá un error.  |
|CREATE INDEX   |[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  |Los usuarios pueden crear índices en columnas y columnas definidas por el usuario en tablas de nodo y perimetrales. Se admiten todos los tipos de índice, incluidos los índices de almacén de columnas agrupados y no agrupados.  |
|CREAR RESTRICCIONES PERIMETRALES    |[RESTRICCIONES PERIMETRALes &#40;&#41;de Transact-SQL ](../../relational-databases/tables/graph-edge-constraints.md)  |Los usuarios ahora pueden crear restricciones perimetrales en tablas perimetrales para aplicar semántica específica y mantener la integridad de los datos.  |
|DROP TABLE |[DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)  |Las tablas de nodos y perimetrales se pueden quitar de la misma manera que una tabla relacional, mediante `DROP TABLE` . Sin embargo, en esta versión, no hay ninguna restricción para asegurarse de que ningún borde señale a un nodo eliminado y la eliminación en cascada de los bordes, cuando no se admite la eliminación de un nodo o una tabla de nodos. Se recomienda que, si se quita una tabla de nodos, los usuarios coloquen los bordes conectados a los nodos de esa tabla de nodos manualmente para mantener la integridad del gráfico.  |


### <a name="data-manipulation-language-dml-statements"></a>Instrucciones del Lenguaje de manipulación de datos (DML)

|Tarea   |Artículo relacionado  |Notas
|---  |---  |---  |
|INSERT |[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-sql-graph.md)|La inserción en una tabla de nodos no es diferente a la inserción en una tabla relacional. Los valores de la `$node_id` columna se generan automáticamente. Si intenta insertar un valor en `$node_id` la `$edge_id` columna o, se producirá un error. Los usuarios deben proporcionar valores `$from_id` para `$to_id` las columnas y al insertarlos en una tabla irregular. `$from_id` y `$to_id` son los `$node_id` valores de los nodos a los que se conecta un borde determinado.  |
|Delete | [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)|Los datos de las tablas perimetrales o de nodo se pueden eliminar de la misma manera que se eliminan de las tablas relacionales. Sin embargo, en esta versión, no hay ninguna restricción para asegurarse de que ningún borde señale a un nodo eliminado y la eliminación en cascada de los bordes, cuando no se admite la eliminación de un nodo. Se recomienda que cada vez que se elimine un nodo, también se eliminen todos los bordes de conexión a ese nodo, para mantener la integridad del gráfico.  |
|UPDATE |[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  |Los valores de las columnas definidas por el usuario se pueden actualizar mediante la instrucción UPDATE. No se permite actualizar las columnas internas del gráfico, `$node_id` , `$edge_id` `$from_id` y `$to_id` .  |
|MERGE |[MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)  |`MERGE` la instrucción se admite en un nodo o una tabla perimetral.  |


### <a name="query-statements"></a>Instrucciones de consulta

|Tarea   |Artículo relacionado  |Notas
|---  |---  |---  |
|SELECT |[SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)|Los nodos y los bordes se almacenan como tablas internamente; por lo tanto, la mayoría de las operaciones admitidas en una tabla en SQL Server o Azure SQL Database se admiten en las tablas de nodo y perimetrales.  |
|MATCH  | [COINCIDENCIA &#40;&#41;de Transact-SQL ](../../t-sql/queries/match-sql-graph.md)|La coincidencia integrada se incluye para admitir la coincidencia de patrones y el recorrido a través del gráfico.  |



## <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos  
Existen ciertas limitaciones en las tablas de nodos y perimetrales en esta versión:
* Las tablas temporales locales o globales no pueden ser tablas de nodo o perimetrales.
* Los tipos de tabla y las variables de tabla no se pueden declarar como una tabla de nodo o perimetral. 
* Las tablas de nodo y perimetrales no se pueden crear como tablas temporales con versión del sistema.   
* Las tablas de nodo y perimetrales no pueden ser tablas con optimización para memoria.  
* Los usuarios no pueden actualizar las `$from_id` `$to_id` columnas y de un borde mediante la instrucción UPDATE. Para actualizar los nodos a los que se conecta un borde, los usuarios tendrán que insertar el nuevo borde que apunta a los nuevos nodos y eliminar el anterior.
* No se admiten las consultas entre bases de datos en objetos de grafo. 


## <a name="next-steps"></a>Pasos siguientes
Para empezar a trabajar con la nueva sintaxis, consulte [base de datos de SQL Graph: ejemplo](./sql-graph-sample.md)
 

