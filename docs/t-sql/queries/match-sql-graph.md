---
description: MATCH (Transact-SQL)
title: MATCH (SQL Graph) | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- MATCH
- MATCH_TSQL
- SHORTEST_PATH
dev_langs:
- TSQL
helpviewer_keywords:
- MATCH statement [SQL Server], SQL graph
- SQL graph, MATCH statement
- Shortest Path, shortest_path
author: shkale-msft
ms.author: shkale
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d55511e11750d29184124aa25b00a952eaf20ae7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270522"
---
# <a name="match-transact-sql"></a>MATCH (Transact-SQL)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

  Especifica una condición de búsqueda para un gráfico. MATCH puede usarse solo con tablas perimetrales o de nodo del gráfico, en la instrucción SELECT y como parte de la cláusula WHERE. 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
MATCH (<graph_search_pattern>)

<graph_search_pattern>::=
  {  
      <simple_match_pattern> 
    | <arbitrary_length_match_pattern>  
    | <arbitrary_length_match_last_node_predicate> 
  }

<simple_match_pattern>::=
  {
      LAST_NODE(<node_alias>) | <node_alias>   { 
          { <-( <edge_alias> )- } 
        | { -( <edge_alias> )-> }
        <node_alias> | LAST_NODE(<node_alias>)
        } 
  }
  [ { AND } { ( <simple_match_pattern> ) } ]
  [ ,...n ]

<node_alias> ::=
  node_table_name | node_table_alias 

<edge_alias> ::=
  edge_table_name | edge_table_alias


<arbitrary_length_match_pattern>  ::=
  { 
    SHORTEST_PATH( 
      <arbitrary_length_pattern> 
      [ { AND } { <arbitrary_length_pattern> } ] 
      [ ,…n] 
    )
  } 

<arbitrary_length_match_last_node_predicate> ::=
  {  LAST_NODE( <node_alias> ) = LAST_NODE( <node_alias> ) }


<arbitrary_length_pattern> ::=
    {  LAST_NODE( <node_alias> )   | <node_alias>
     ( <edge_first_al_pattern> [<edge_first_al_pattern>…,n] )
     <al_pattern_quantifier> 
  }
    |  ( {<node_first_al_pattern> [<node_first_al_pattern> …,n] )
        <al_pattern_quantifier> 
        LAST_NODE( <node_alias> ) | <node_alias> 
 }
    
<edge_first_al_pattern> ::=
  { (  
        { -( <edge_alias> )->   } 
      | { <-( <edge_alias> )- } 
      <node_alias>
      ) 
  } 

<node_first_al_pattern> ::=
  { ( 
      <node_alias> 
        { <-( <edge_alias> )- } 
      | { -( <edge_alias> )-> }
      ) 
  } 


<al_pattern_quantifier> ::=
  {
        +
      | { 1 , n }
  }

n -  positive integer only.
 
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*graph_search_pattern*  
Especifica el patrón que buscar o la ruta de acceso que atravesar en el gráfico. Este patrón usa sintaxis de arte ASCII para atravesar una ruta de acceso en el gráfico. El patrón va de un nodo a otro a través de un borde, en la dirección de la flecha proporcionada. Los nombres o alias de borde se suministran entre paréntesis. Los nombres o alias de nodo aparecen en los dos extremos de la flecha. La flecha puede ir en ambas direcciones en el patrón.

*node_alias*  
Nombre o alias de una tabla de nodo que se proporciona en la cláusula FROM.

*edge_alias*  
Nombre o alias de una tabla perimetral que se proporciona en la cláusula FROM.

*SHORTEST_PATH*   
La función de ruta de acceso más corta se usa para encontrar la ruta más corta entre dos nodos dados de un grafo o entre un nodo dado y todos los demás nodos del grafo. Esta función toma como entrada un patrón de longitud arbitraria, que se busca varias veces en un grafo. 

*arbitrary_length_match_pattern*  
Especifica los nodos y bordes que se deben recorrer repetidamente hasta que se alcance el nodo deseado o hasta que se cumpla el número máximo de iteraciones especificadas en el patrón. 

*al_pattern_quantifier*   
El patrón de longitud arbitraria toma los cuantificadores del patrón de estilo de expresión regular para especificar el número de veces que se repite un patrón de búsqueda dado. Los cuantificadores de patrón de búsqueda admitidos son:   
* **+** : repite el patrón una o más veces. Finaliza en cuanto encuentra una ruta de acceso más corta.    
* **{1, n}** : repite el patrón de 1 a "n" veces. Finaliza en cuanto encuentra una ruta de acceso más corta.     

## <a name="remarks"></a>Comentarios  
Los nombres de nodo dentro de MATCH se pueden repetir.  Dicho de otro, un nodo se puede atravesar un número arbitrario de veces en la misma consulta.  
Los nombres de borde no se pueden repetir dentro de MATCH.  
Un borde puede apuntar a cualquier dirección, pero debe tener una dirección explícita.  
Los operadores OR y NOT no se pueden usar en el patrón MATCH. MATCH se puede combinar con otras expresiones por medio de AND en la cláusula WHERE, pero esto no es posible con los operadores OR ni NOT. 

## <a name="examples"></a>Ejemplos  
### <a name="a--find-a-friend"></a>A.  Buscar un amigo 
 En el siguiente ejemplo se crea una tabla de nodos Person y una tabla perimetral friend, se insertan algunos datos y, tras ello, se usa MATCH para encontrar amigos de Alice, una persona del gráfico.

 ```sql
 -- Create person node table
 CREATE TABLE dbo.Person (ID INTEGER PRIMARY KEY, name VARCHAR(50)) AS NODE;
 CREATE TABLE dbo.friend (start_date DATE) AS EDGE;

 -- Insert into node table
 INSERT INTO dbo.Person VALUES (1, 'Alice');
 INSERT INTO dbo.Person VALUES (2,'John');
 INSERT INTO dbo.Person VALUES (3, 'Jacob');

-- Insert into edge table
INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'John'), '9/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'Alice'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2011');

INSERT INTO dbo.friend VALUES ((SELECT $node_id FROM dbo.Person WHERE name = 'John'),
        (SELECT $node_id FROM dbo.Person WHERE name = 'Jacob'), '10/15/2012');

-- use MATCH in SELECT to find friends of Alice
SELECT Person2.name AS FriendName
FROM Person Person1, friend, Person Person2
WHERE MATCH(Person1-(friend)->Person2)
AND Person1.name = 'Alice';
 ```

 ### <a name="b--find-friend-of-a-friend"></a>B.  Buscar un amigo de un amigo
 En el siguiente ejemplo se intenta encontrar un amigo de un amigo de Alice. 

 ```sql
SELECT Person3.name AS FriendName 
FROM Person Person1, friend, Person Person2, friend friend2, Person Person3
WHERE MATCH(Person1-(friend)->Person2-(friend2)->Person3)
AND Person1.name = 'Alice';
 ```

### <a name="c--more-match-patterns"></a>C.  Más patrones de `MATCH`
 Aquí encontrará algunas formas más en las que se puede especificar un patrón en MATCH.

 ```sql
 -- Find a friend
    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2
    WHERE MATCH(Person1-(friend)->Person2);
    
-- The pattern can also be expressed as below

    SELECT Person2.name AS FriendName
    FROM Person Person1, friend, Person Person2 
    WHERE MATCH(Person2<-(friend)-Person1);


-- Find 2 people who are both friends with same person
    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0<-(friend2)-Person2);
    
-- this pattern can also be expressed as below

    SELECT Person1.name AS Friend1, Person2.name AS Friend2
    FROM Person Person1, friend friend1, Person Person2, 
         friend friend2, Person Person0
    WHERE MATCH(Person1-(friend1)->Person0 AND Person2-(friend2)->Person0);
 ```
 

## <a name="see-also"></a>Vea también  
 [CREATE TABLE &#40;SQL Graph&#41;](../../t-sql/statements/create-table-sql-graph.md)   
 [INSERT (SQL Graph)](../../t-sql/statements/insert-sql-graph.md)]  
 [Graph processing with SQL Server 2017](../../relational-databases/graphs/sql-graph-overview.md) (Procesamiento de gráficos con SQL Server 2017)  
