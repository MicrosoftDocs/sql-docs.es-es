---
title: Índices de tablas con optimización para memoria | Microsoft Docs
description: Obtenga información sobre la diferencia de un índice en una tabla optimizada para memoria de un índice tradicional en una tabla basada en disco en SQL Server y Azure SQL Database.
ms.custom: ''
ms.date: 09/16/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: eecc5821-152b-4ed5-888f-7c0e6beffed9
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 15cabb6877e249a6f0be9d38449c85c13bbda78b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351240"
---
# <a name="indexes-on-memory-optimized-tables"></a>Índices de las tablas con optimización para memoria

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Todas las tablas optimizadas para memoria deben tener como mínimo un índice porque son los índices los que conectan las filas. En una tabla optimizada para memoria, todos los índices también son optimizados para memoria. Hay varias formas de diferenciar un índice en una tabla optimizada para memoria de un índice tradicional en una tabla basada en disco:  

- Las filas de datos no se almacenan en páginas, por lo que no existe ninguna colección de páginas o extensiones, ni unidades de asignación o particiones a las que se pueda hacer referencia para obtener todas las páginas de una tabla. Existe el concepto de páginas de índice para uno de los tipos de índices disponibles, pero se almacenan de una manera distinta a los índices para las tablas basadas en disco. No acumulan el tipo tradicional de fragmentación dentro de una página, por lo que no tienen ningún factor de relleno.
- Los cambios que se realizan en los índices de las tablas optimizadas para memoria durante la manipulación de los datos nunca se escriben en el disco. Solo las filas de datos, y los cambios en los datos, se escriben en el registro de transacciones. 
- Los índices con optimización para memoria se vuelven a generar cuando la base de datos vuelve a estar en línea. 

Todos los índices en las tablas optimizadas para memoria se crean en función de las definiciones de índice durante la recuperación de la base de datos.

El índice debe ser uno de los siguientes:  
  
- Índice de hash  
- Índice no agrupado optimizado para memoria (es decir, la estructura interna predeterminada de un árbol B) 
  
Los índices de *hash* se analizan con más detalle en [Índices de hash de tablas optimizadas para memoria](../../relational-databases/sql-server-index-design-guide.md#hash_index).  
Los índices *no agrupados* se analizan con más detalle en [Índice no agrupado de tablas optimizadas para memoria](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index).  
Los índices de *almacén de columnas* se tratan en [otro artículo](../../relational-databases/indexes/columnstore-indexes-overview.md).  

## <a name="syntax-for-memory-optimized-indexes"></a>Sintaxis de índices optimizados para memoria  
  
Cada instrucción CREATE TABLE para una tabla optimizada para memoria debe incluir e indexar, ya sea explícitamente a través de un elemento INDEX, o implícitamente a través de un elemento PRIMARY KEY o una restricción UNIQUE.
  
Para declararse con el valor predeterminado de DURABILITY = SCHEMA\_AND_DATA, la tabla optimizada para memoria debe tener una clave principal. La cláusula PRIMARY KEY NONCLUSTERED de la siguiente instrucción CREATE TABLE cumple dos requisitos:  
  
- Proporciona un índice para satisfacer el requisito mínimo de tener un índice en la instrucción CREATE TABLE.  
- Proporciona la clave principal necesaria para la cláusula SCHEMA\_AND_DATA.  

    ```sql
    CREATE TABLE SupportEvent  
    (  
        SupportEventId   int NOT NULL  
            PRIMARY KEY NONCLUSTERED,  
        ...  
    )  
        WITH (  
            MEMORY_OPTIMIZED = ON,  
            DURABILITY = SCHEMA_AND_DATA);  
    ```

> [!NOTE]  
> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] tienen un límite de 8 índices por tabla optimizada para memoria o tipo de tabla. A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], ya no hay un límite para el número de índices específicos para tablas optimizadas para memoria y tipos de tabla.
  
### <a name="code-sample-for-syntax"></a>Ejemplo de código para sintaxis  
  
En este apartado se incluye un bloque de código de Transact-SQL que muestra la sintaxis para crear varios índices en una tabla optimizada para memoria. El código muestra lo siguiente:  
  
1. Crear una tabla optimizada para memoria.  
2. Usar instrucciones ALTER TABLE para agregar dos índices.  
3. Usar INSERT para insertar algunas filas de datos.  
   
    ```sql
    DROP TABLE IF EXISTS SupportEvent;  
    go  

    CREATE TABLE SupportEvent  
    (  
        SupportEventId   int               not null   identity(1,1)  
        PRIMARY KEY NONCLUSTERED,  

        StartDateTime        datetime2     not null,  
        CustomerName         nvarchar(16)  not null,  
        SupportEngineerName  nvarchar(16)      null,  
        Priority             int               null,  
        Description          nvarchar(64)      null  
    )  
        WITH (  
        MEMORY_OPTIMIZED = ON,  
        DURABILITY = SCHEMA_AND_DATA);  
    go  
        
        --------------------  
        
    ALTER TABLE SupportEvent  
        ADD CONSTRAINT constraintUnique_SDT_CN  
        UNIQUE NONCLUSTERED (StartDateTime DESC, CustomerName);  
    go  

    ALTER TABLE SupportEvent  
        ADD INDEX idx_hash_SupportEngineerName  
        HASH (SupportEngineerName) WITH (BUCKET_COUNT = 64);  -- Nonunique.  
    go  
        
        --------------------  
        
    INSERT INTO SupportEvent  
        (StartDateTime, CustomerName, SupportEngineerName, Priority, Description)  
        VALUES  
        ('2016-02-23 13:40:41:123', 'Abby', 'Zeke', 2, 'Display problem.'     ),  
        ('2016-02-24 13:40:41:323', 'Ben' , null  , 1, 'Cannot find help.'    ),  
        ('2016-02-25 13:40:41:523', 'Carl', 'Liz' , 2, 'Button is gray.'      ),  
        ('2016-02-26 13:40:41:723', 'Dave', 'Zeke', 2, 'Cannot unhide column.');  
    go 
    ``` 
  
## <a name="duplicate-index-key-values"></a>Valores de clave de índice duplicados

Los valores duplicados de una clave de índice pueden reducir el rendimiento de las tablas optimizadas para memoria. Valores duplicados del sistema que atraviesan cadenas de entrada en la mayoría operaciones de lectura y escritura de índices. Cuando una cadena de entradas duplicadas supera las 100 entradas, la degradación del rendimiento puede ser palpable.

### <a name="duplicate-hash-values"></a>Valores hash duplicados

Este problema es más patente en el caso de los índices hash. Los índices hash sufren más debido a las siguientes consideraciones:

- El menor coste por operación de los índices hash.
- La interferencia de grandes cadenas de duplicados con la cadena de colisión de hash.

Para reducir la duplicación en un índice, pruebe a realizar los siguientes ajustes:

- Utilice un índice no agrupado.
- Agregue más columnas al final de la clave de índice para reducir el número de duplicados.
  - Por ejemplo, podría agregar columnas que estén también en la clave principal.

Para más información sobre las colisiones de hash, consulte el artículo sobre los [índices de hash de tablas optimizadas para memoria](../../relational-databases/sql-server-index-design-guide.md#hash_index).

### <a name="example-improvement"></a>Ejemplo de mejora

Este es un ejemplo en el que se indica cómo evitar que se produzca un rendimiento ineficaz en el índice.

Considere una tabla `Customers` que tiene una clave principal en `CustomerId` y un índice en la columna `CustomerCategoryID`. En una categoría determinada suele haber muchos clientes. Por tanto, habrá muchos valores duplicados de CustomerCategoryID dentro de una determinada clave del índice.

En este escenario, el procedimiento recomendado es usar un índice no agrupado en `(CustomerCategoryID, CustomerId)`. Este índice se puede usar en consultas que usan un predicado donde `CustomerCategoryID` existe, pero la clave de índice no contiene duplicados. Por tanto, no surgen ineficiencias en el mantenimiento de índices provocadas por valores duplicados de CustomerCategoryID o por la columna extra en el índice.

La consulta siguiente muestra el promedio de valores clave de índice duplicados para el índice en `CustomerCategoryID` en la tabla `Sales.Customers`, en la base de datos de ejemplo [WideWorldImporters](../../samples/wide-world-importers-what-is.md).

```sql
SELECT AVG(row_count) FROM
    (SELECT COUNT(*) AS row_count 
        FROM Sales.Customers
        GROUP BY CustomerCategoryID) a
```

Para evaluar el promedio de duplicados de clave de índice para su propia tabla e índice, reemplace `Sales.Customers` por su nombre de la tabla y reemplace `CustomerCategoryID` por la lista de columnas de clave de índice.

## <a name="comparing-when-to-use-each-index-type"></a>Comparación del uso de cada tipo de índice  
  
La naturaleza de las consultas concretas determina qué tipo de índice es la mejor opción.  

Al implementar tablas optimizadas para memoria en una aplicación existente, la recomendación general es comenzar por los índices no agrupados, ya que sus capacidades se parecen más a las de los índices agrupados y no agrupados tradicionales de las tablas basadas en disco. 
  
### <a name="recommendations-for-nonclustered-index-use"></a>Recomendaciones para el uso de índices no agrupados  
  
Un índice no agrupado es preferible a un índice de hash cuando:  
  
- Las consultas tienen una cláusula `ORDER BY` en la columna indexada.  
- Las consultas en las que solo se comprueban las primeras columnas de un índice con varias columnas.  
- Las consultas prueban la columna indexada mediante el uso de una cláusula `WHERE` con:  
  - Una desigualdad: `WHERE StatusCode != 'Done'`  
  - Un examen de intervalo de valores: `WHERE Quantity >= 100`  
  
En todas las instrucciones SELECT siguientes, es preferible un índice no agrupado a un índice de hash:  

```sql
SELECT CustomerName, Priority, Description 
FROM SupportEvent  
WHERE StartDateTime > DateAdd(day, -7, GetUtcDate());  

SELECT StartDateTime, CustomerName  
FROM SupportEvent  
ORDER BY StartDateTime DESC; -- ASC would cause a scan.

SELECT CustomerName  
FROM SupportEvent  
WHERE StartDateTime = '2016-02-26';  
```
  
### <a name="recommendations-for-hash-index-use"></a>Recomendaciones para el uso de índices de hash   
  
Los [índices de hash](../../relational-databases/sql-server-index-design-guide.md#hash_index) se usan principalmente para búsquedas de puntos y no para exámenes de intervalos.

Es preferible un índice de hash sobre un índice no agrupado cuando las consultas usan predicados de igualdad y la cláusula `WHERE` se asigna a todas las columnas de clave de índice, como se muestra en el ejemplo siguiente:  
  
```sql
SELECT CustomerName 
FROM SupportEvent  
WHERE SupportEngineerName = 'Liz';
```  

### <a name="multi-column-index"></a>Índice de varias columnas  
  
Un índice de varias columnas puede ser un índice no agrupado o un índice de hash. Supongamos que las columnas de índice son col1 y col2. Si tenemos la siguiente instrucción `SELECT`, solo el índice no agrupado sería útil para el optimizador de consultas:  
  
```sql
SELECT col1, col3  
FROM MyTable_memop  
WHERE col1 = 'dn';  
```

El índice de hash requiere que la cláusula `WHERE` contenga una prueba de igualdad para cada una de las columnas en la clave. De lo contrario, dicho índice no tendrá ninguna utilidad para el optimizador de consultas.  
  
Ningún tipo de índice resultará útil si solo se especifica la segunda columna de la clave de índice en la cláusula `WHERE`.  

## <a name="summary-table-to-compare-index-use-scenarios"></a>Tabla de resumen para comparar los escenarios de uso de los índices  
  
En la tabla siguiente se enumeran todas las operaciones que son compatibles con distintos tipos de índices. *Sí* se refiere a que el índice puede atender la solicitud con eficiencia y *No* se refiere a que el índice no puede satisfacer la solicitud con eficiencia. 
  
| Operación | Con optimización para memoria, <br/> hash | Con optimización para memoria, <br/> no agrupados | Basada en disco, <br/> (no)agrupados |  
| :-------- | :--------------------------- | :----------------------------------- | :------------------------------------ |  
| Index scan, recupera todas las filas de la tabla. | Sí | Sí | Sí |  
| Index seek en predicados de igualdad (=). | Sí <br/> (Se requiere la clave completa). | Sí  | Sí |  
| Index seek en predicados de desigualdad y de intervalo <br/> (>, <, <=, >=, `BETWEEN`). | No <br/> (Resultados de un examen de índice). | Sí <sup>1</sup> | Sí |  
| Recuperar filas según un criterio de ordenación que coincida con la definición de índice. | No | Sí | Sí |  
| Recuperar filas según un criterio de ordenación que coincida con el opuesto de la definición de índice. | No | No | Sí |  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> Para un índice optimizado para memoria no agrupado, no es necesaria la clave completa para realizar la búsqueda de índice.  

## <a name="automatic-index-and-statistics-management"></a>Administración automática de índice y estadísticas

Aproveche soluciones como la [desfragmentación de índice adaptable](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) para administrar automáticamente las actualizaciones de estadísticas y la desfragmentación de índices para una o varias bases de datos. Este procedimiento elige automáticamente si se debe volver a generar o reorganizar un índice según su nivel de fragmentación, entre otros parámetros y actualiza las estadísticas con un umbral lineal.

## <a name="see-also"></a><a name="Additional_Reading"></a> Consulte también   
 [Guía de diseño de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md)   
 [Índices de hash para tablas optimizadas para memoria](../../relational-databases/sql-server-index-design-guide.md#hash_index)   
 [Índices no agrupados para tablas optimizadas para memoria](../../relational-databases/sql-server-index-design-guide.md#inmem_nonclustered_index)    
 [Desfragmentación de índice adaptable](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag)