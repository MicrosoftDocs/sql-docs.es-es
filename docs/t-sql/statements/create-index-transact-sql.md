---
description: CREATE INDEX (Transact-SQL)
title: CREATE INDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE INDEX
- INDEX
- INDEX_TSQL
- CREATE_INDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE XML INDEX statement
- PRIMARY XML INDEX statement
- indexes [SQL Server], creating
- online index operations
- index keys [SQL Server]
- PROPERTY INDEX statement
- computed columns, index creation
- clustered indexes, creating
- indexed views [SQL Server], index creation
- partitioned indexes [SQL Server], creating
- VALUE INDEX statement
- index creation [SQL Server], CREATE INDEX statement
- DROP_EXISTING clause
- row locks [SQL Server]
- composite indexes
- MAXDOP index option, CREATE INDEX statement
- filtered indexes [SQL Server], creating
- PATH INDEX statement
- locking [SQL Server], indexes
- unique indexes, creating
- primary indexes [SQL Server]
- CREATE PRIMARY XML INDEX statement
- SET statement, index creation
- index options [SQL Server]
- included columns
- ONLINE option
- nonclustered indexes [SQL Server], creating
- CREATE INDEX statement
- IGNORE_DUP_KEY option
- deterministic functions
- keys [SQL Server], index
- SECONDARY XML INDEX statement
- page locks [SQL Server]
- secondary indexes [SQL Server]
- XML indexes [SQL Server], creating
ms.assetid: d2297805-412b-47b5-aeeb-53388349a5b9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e02d3676b1b303ef6dbbae4a509ed0db0c608071
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525239"
---
# <a name="create-index-transact-sql"></a>CREATE INDEX (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Crea un índice relacional en una tabla o una vista. También se denomina índice de almacén de filas porque es un índice de árbol B agrupado o no agrupado. Puede crear un índice de almacén de filas antes de que haya datos en la tabla. Utilice un índice de almacén de filas para mejorar el rendimiento de las consultas, especialmente cuando las consultas hacen la selección en columnas específicas o requieren que los valores se ordenen en un orden concreto.

> [!NOTE]
> En estos momentos, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] no admiten las restricciones Unique. Los ejemplos que hacen referencia a las restricciones Unique solo son aplicables a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
> [!TIP]
> Para obtener más información sobre las directrices de diseño de índices, vea la [Guía de diseño de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md).

 **Ejemplos sencillos:**

```sql
-- Create a nonclustered index on a table or view
CREATE INDEX i1 ON t1 (col1);

-- Create a clustered index on a table and use a 3-part name for the table
CREATE CLUSTERED INDEX i1 ON d1.s1.t1 (col1);

-- Syntax for SQL Server and Azure SQL Database
-- Create a nonclustered index with a unique constraint
-- on 3 columns and specify the sort order for each column
CREATE UNIQUE INDEX i1 ON t1 (col1 DESC, col2 ASC, col3 DESC);
```

**Escenario clave:**

A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)], utilice un índice no agrupado en un índice de almacén de columnas para mejorar el rendimiento de las consultas de almacenamiento de datos. Para obtener más información, vea [Almacenamiento de datos de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md).

Para los tipos adicionales de índices, consulte:

- [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)
- [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)
- [CREATE COLUMNSTORE INDEX](../../t-sql/statements/create-columnstore-index-transact-sql.md)

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

### <a name="syntax-for-sql-server-and-azure-sql-database"></a>Sintaxis de SQL Server y Azure SQL Database

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column [ ASC | DESC ] [ ,...n ] )
    [ INCLUDE ( column_name [ ,...n ] ) ]
    [ WHERE <filter_predicate> ]
    [ WITH ( <relational_index_option> [ ,...n ] ) ]
    [ ON { partition_scheme_name ( column_name )
         | filegroup_name
         | default
         }
    ]
    [ FILESTREAM_ON { filestream_filegroup_name | partition_scheme_name | "NULL" } ]
  
[ ; ]
  
<object> ::=
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }

<relational_index_option> ::=
{
    PAD_INDEX = { ON | OFF }
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB = { ON | OFF }
  | IGNORE_DUP_KEY = { ON | OFF }
  | STATISTICS_NORECOMPUTE = { ON | OFF }
  | STATISTICS_INCREMENTAL = { ON | OFF }
  | DROP_EXISTING = { ON | OFF }
  | ONLINE = { ON | OFF }
  | RESUMABLE = { ON | OFF }
  | MAX_DURATION = <time> [MINUTES]
  | ALLOW_ROW_LOCKS = { ON | OFF }
  | ALLOW_PAGE_LOCKS = { ON | OFF }
  | OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | OFF }
  | MAXDOP = max_degree_of_parallelism
  | DATA_COMPRESSION = { NONE | ROW | PAGE }
     [ ON PARTITIONS ( { <partition_number_expression> | <range> }
     [ , ...n ] ) ]
}

<filter_predicate> ::=
    <conjunct> [ AND <conjunct> ]

<conjunct> ::=
    <disjunct> | <comparison>

<disjunct> ::=
        column_name IN (constant ,...n)

<comparison> ::=
        column_name <comparison_op> constant

<comparison_op> ::=
    { IS | IS NOT | = | <> | != | > | >= | !> | < | <= | !< }

<range> ::=
<partition_number_expression> TO <partition_number_expression>
```

### <a name="backward-compatible-relational-index"></a>Índice relacional compatible con versiones anteriores

> [!IMPORTANT]
> La estructura de sintaxis de índice relacional compatible con versiones anteriores se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
> Evite usar esta estructura de sintaxis en los nuevos trabajos de desarrollo y piense en modificar las aplicaciones que la usan actualmente.
> En su lugar, use la estructura de sintaxis especificada en <relational_index_option>.

```syntaxsql
CREATE [ UNIQUE ] [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON <object> ( column_name [ ASC | DESC ] [ ,...n ] )
    [ WITH <backward_compatible_index_option> [ ,...n ] ]
    [ ON { filegroup_name | "default" } ]

<object> ::=
{
    [ database_name. [ owner_name ] . | owner_name. ]
    table_or_view_name
}

<backward_compatible_index_option> ::=
{
    PAD_INDEX
  | FILLFACTOR = fillfactor
  | SORT_IN_TEMPDB
  | IGNORE_DUP_KEY
  | STATISTICS_NORECOMPUTE
  | DROP_EXISTING
}
```

### <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Sintaxis para Azure Synapse Analytics y Almacenamiento de datos paralelos

```syntaxsql

CREATE CLUSTERED COLUMNSTORE INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
    [ORDER (column[,...n])]
    [WITH ( DROP_EXISTING = { ON | OFF } )]
[;]


CREATE [ CLUSTERED | NONCLUSTERED ] INDEX index_name
    ON [ database_name . [ schema ] . | schema . ] table_name
        ( { column [ ASC | DESC ] } [ ,...n ] )
    WITH ( DROP_EXISTING = { ON | OFF } )
[;]


```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

UNIQUE      
Crea un índice único en una tabla o una vista. Un índice único es aquel en el que no se permite que dos filas tengan el mismo valor de clave del índice. El índice clúster de una vista debe ser único.

[!INCLUDE[ssDE](../../includes/ssde-md.md)] no admite la creación de un índice único sobre columnas que ya contengan valores duplicados, independientemente de si se ha establecido o no IGNORE_DUP_KEY en ON. Si se intenta, [!INCLUDE[ssDE](../../includes/ssde-md.md)] muestra un mensaje de error. Se deben quitar los valores duplicados para poder crear un índice único en la columna o columnas. Las columnas que se utilizan en un índice único se deben establecer en NOT NULL, dado que varios valores NULL se consideran duplicados cuando se crea un índice único.

CLUSTERED      
Crea un índice en el que el orden lógico de los valores de clave determina el orden físico de las filas correspondientes de la tabla. El nivel inferior, u hoja, de un índice clúster contiene las filas de datos reales de la tabla. Una tabla o vista permite un índice clúster al mismo tiempo.

Una vista con un índice clúster único se denomina vista indizada. La creación de un índice clúster único en una vista materializa físicamente la vista. Es necesario crear un índice clúster único en una vista para poder definir otros índices en la misma vista. Para obtener más información, vea [Crear vistas indexadas](../../relational-databases/views/create-indexed-views.md).

Cree el índice clúster antes de crear los índices no clúster. Los índices no clúster existentes en las tablas se vuelven a generar al crear un índice clúster.

Si no se especifica `CLUSTERED`, se crea un índice no agrupado.

> [!NOTE]
> Debido a que el nivel hoja de un índice clúster y sus páginas de datos son, por definición, lo mismo, la creación de un índice clúster y la utilización de la cláusula ON *partition_scheme_name* u ON *filegroup_name* mueven una tabla desde el grupo de archivos en el que se creó la tabla al nuevo grupo de archivos o esquema de partición. Antes de crear tablas o índices en grupos de archivos específicos, compruebe cuáles están disponibles y que esos grupos de archivos tengan suficiente espacio disponible para el índice.

En algunos casos, al crear un índice clúster se pueden habilitar previamente los índices deshabilitados. Para obtener más información, consulte [Habilitar índices y restricciones](../../relational-databases/indexes/enable-indexes-and-constraints.md) y [Deshabilitar índices y restricciones](../../relational-databases/indexes/disable-indexes-and-constraints.md).

NONCLUSTERED      
Crea un índice que especifica la ordenación lógica de una tabla. Con un índice no clúster, el orden físico de las filas de datos es independiente del orden indizado.

Cada tabla puede tener hasta 999 índices no clúster, independientemente de cómo se crean: de forma implícita con las restricciones PRIMARY KEY y UNIQUE, o explícita con CREATE INDEX.

Para las vistas indizadas, solo se pueden crear índices no clúster en una vista que ya tenga definido un índice clúster único.

Si no se especifica, el tipo de índice predeterminado es NONCLUSTERED.

*index_name*      
 Es el nombre del índice. Los nombres de índice deben ser únicos en una tabla o vista, pero no es necesario que sean únicos en una base de datos. Los nombres de índice deben seguir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).

*column*      
 Es la columna o columnas en las que se basa el índice. Especifique dos o más nombres de columna para crear un índice compuesto sobre los valores combinados de las columnas especificadas. Enumere las columnas que desee incluir en el índice compuesto (en orden de prioridad) entre paréntesis después de *table_or_view_name*.

Se pueden combinar hasta 32 columnas en la clave de un único índice compuesto. Todas las columnas de una clave del índice compuesto deben encontrarse en la misma tabla o vista. El tamaño máximo permitido de los valores de índice combinados es de 900 bytes para un índice agrupado o de 1700 para un índice no agrupado. Los límites son 16 columnas y 900 bytes para las versiones anteriores a [!INCLUDE[ssSDS](../../includes/sssds-md.md)] y [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].

Las columnas de los tipos de datos de objetos grandes (LOB) **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml** o **image** no pueden especificarse como columnas clave para un índice. Además, una definición de vista no puede incluir columnas **ntext**, **text** ni **image**, aunque no se haga referencia a ellas en la instrucción CREATE INDEX.

Puede crear índices en columnas de tipo definido por el usuario CLR si el tipo admite el orden binario. También puede crear índices en columnas calculadas que están definidas como invocaciones de método de una columna de tipo definido por el usuario, siempre que los métodos estén marcados como deterministas y no realicen operaciones de acceso a datos. Para obtener más información sobre la indización de columnas de tipo definido por el usuario CLR, vea [Tipos definidos por el usuario de CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).

[ **ASC** | DESC ]      
Determina la dirección ascendente o descendente del orden de la columna de índice determinada. El valor predeterminado es ASC.

INCLUDE **(** _column_ [ **,** ... *n* ] **)**       
Especifica las columnas que no son de clave que se agregarán en el nivel hoja del índice no clúster. El índice no clúster puede ser único o no único.

Los nombres de columna no se pueden repetir en la lista INCLUDE y no se pueden utilizar simultáneamente como columnas de clave y que no son de clave. Los índices no clúster siempre contienen las columnas de índice clúster si se define un índice clúster en la tabla. Para más información, consulte [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md).

Se admiten todos los tipos de datos, a excepción de **text**, **ntext** e **image**. El índice se debe crear o regenerar sin conexión (ONLINE = OFF) si el tipo de datos de alguna de las columnas que no son de clave especificadas es **varchar(max)** , **nvarchar(max)** o **varbinary(max)** .

Las columnas calculadas que son deterministas, y precisas o imprecisas, pueden ser columnas incluidas. Las columnas calculadas derivadas de los tipos de datos **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** y **xml** se pueden incluir en columnas que no son clave, siempre que el tipo de datos de la columna calculada esté disponible como una columna incluida. Para obtener más información, vea [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

Para obtener información sobre cómo crear un índice XML, vea [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md).

WHERE \<filter_predicate>      
Crea un índice filtrado especificando qué filas se van a incluir en el índice. El índice filtrado debe ser un índice no clúster en una tabla. Crea las estadísticas filtradas para las filas de datos en el índice filtrado.

El predicado de filtro utiliza la lógica de comparación simple y no puede hacer referencia a una columna calculada, a una columna UDT, a una columna de tipo de datos espacial o a una columna de tipo de datos hierarchyID. Las comparaciones que utilizan literales NULL no se admiten con los operadores de comparación. En su lugar, use los operadores IS NULL e IS NOT NULL.

A continuación, se muestran algunos ejemplos de predicados de filtro para la tabla `Production.BillOfMaterials`:

```sql
WHERE StartDate > '20000101' AND EndDate <= '20000630'

WHERE ComponentID IN (533, 324, 753)

WHERE StartDate IN ('20000404', '20000905') AND EndDate IS NOT NULL
```

Los índices filtrados no se aplican a los índices XML ni a los índices de texto completo. Para los índices UNIQUE, solo las filas seleccionadas deben tener valores de índice únicos. Los índices filtrados no admiten la opción IGNORE_DUP_KEY.

ON *partition_scheme_name* **( _column_name_ )**      

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica el esquema de partición que define los grupos de archivos a los que se asignarán las particiones de un índice con particiones. El esquema de partición debe existir dentro de la base de datos mediante la ejecución de [CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md) o [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md). *column_name* especifica la columna en la que se van a crear las particiones de un índice con particiones. Esta columna debe coincidir con el tipo de datos, la longitud y la precisión del argumento de la función de partición que *partition_scheme_name* emplea. *column_name* no está limitado a las columnas de la definición de índice. Se puede especificar cualquier columna de la tabla base, excepto en el caso de partición de un índice UNIQUE, en el que se debe elegir *column_name* entre las columnas usadas como clave única. Esta restricción permite que [!INCLUDE[ssDE](../../includes/ssde-md.md)] compruebe la unicidad de los valores de clave en una única partición solamente.

> [!NOTE]
> Cuando se crean particiones en un índice clúster no único, [!INCLUDE[ssDE](../../includes/ssde-md.md)] agrega de forma predeterminada la columna de partición a la lista de claves del índice clúster, en caso de que aún no se hubiera especificado. Cuando se crean particiones en un índice no clúster que tampoco es único, [!INCLUDE[ssDE](../../includes/ssde-md.md)] agrega la columna de partición como una columna sin clave (incluida) del índice, si aún no se especificó.

Si no se especificó _partition_scheme_name_ o _filegroup_ y se crearon particiones en la tabla, el índice se coloca en el mismo esquema de partición y usa la misma columna de partición que en la tabla subyacente.

> [!NOTE]
> No se puede especificar un esquema de partición en un índice XML. Si se crean particiones en la tabla base, el índice XML usa el mismo esquema de partición que la tabla.

Para más información sobre los índices con particiones, vea [Tablas e índices con particiones](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

ON _filegroup_name_      

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores)

Crea el índice especificado en el grupo de archivos especificado. Si no se ha especificado una ubicación y la tabla o vista no tiene particiones, el índice utiliza el mismo grupo de archivos que la tabla o vista subyacente. El grupo de archivos debe existir previamente.

ON **"** default **"**      

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Crea el índice especificado en el mismo grupo de archivos o esquema de partición que la tabla o la vista.

El término predeterminado (default), en este contexto, no es una palabra clave. Es un identificador para el grupo de archivos predeterminado y debe delimitarse, como en ON **"** default **"** u ON **[** default **]** . Si se especifica "default", la opción QUOTED_IDENTIFIER debe tener el valor ON para la sesión actual. Esta es la configuración predeterminada. Para más información, consulte [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

> [!NOTE]
> "default" no indica el grupo de archivos de base de datos predeterminado en el contexto de CREATE INDEX. Esto difiere de CREATE TABLE, donde "default" busca la tabla en el grupo de archivos predeterminado de la base de datos.

[ FILESTREAM_ON { *filestream_filegroup_name* | *partition_scheme_name* | "NULL" } ]      

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores)

Especifica la posición de datos FILESTREAM para la tabla cuando se crea un índice clúster. La cláusula FILESTREAM_ON permite mover los datos FILESTREAM a otro esquema de partición o a otro grupo de archivos FILESTREAM.

_filestream_filegroup_name_ es el nombre de un grupo de archivos FILESTREAM. El grupo de archivos debe tener un archivo definido para el grupo de archivos, usando para ello las instrucciones [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) o [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md); en caso contrario, se produce un error.

Si se crean particiones de la tabla, la cláusula FILESTREAM_ON deberá incluirse y especificar un esquema de partición de grupos de archivos FILESTREAM que utilice la misma función de partición y columnas de partición que el esquema de partición para la tabla. En caso contrario, se produce un error.

Si la tabla no tiene particiones, no se pueden crear particiones en la columna FILESTREAM. Los datos FILESTREAM para la tabla deben estar almacenados en un grupo de archivos único que se especifica en la cláusula FILESTREAM_ON.

`FILESTREAM_ON NULL` se puede especificar en una instrucción CREATE INDEX si se va a crear un índice clúster y la tabla no contiene una columna FILESTREAM.

Para obtener más información, vea [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).

**\<object>::=**      

Es el objeto completo o no que se indizará.

_database_name_      
Es el nombre de la base de datos.

*schema_name*      
Es el nombre del esquema al que pertenece la tabla o la vista.

*table_or_view_name*      
Es el nombre de la tabla o la vista que se va a indizar.

La vista debe definirse con SCHEMABINDING para crear un índice en ella. Es necesario crear un índice clúster único en una vista antes de crear los índices no clúster. Para obtener más información acerca de las vistas indizadas, vea la sección Comentarios.

A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el objeto puede ser una tabla almacenada con un índice de almacén de columnas agrupado.

[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] admite el formato de nombre de tres partes _database_name_.[_schema_name_]._object_name_ cuando *database_name* es la base de datos actual o _database_name_ es `tempdb` y _object_name_ comienza por #.

**\<relational_index_option\>::=**       
Especifica las opciones que se van a utilizar en la creación del índice.

PAD_INDEX = { ON | **OFF** }      

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica el relleno del índice. El valor predeterminado es OFF.

ACTIVAR      
El porcentaje de espacio disponible especificado por *fillfactor* se aplica a páginas de nivel intermedio del índice.

No se especifica OFF ni _fillfactor_.      
Las páginas de nivel intermedio se llenan casi al máximo de su capacidad y dejan espacio suficiente para al menos una fila del tamaño máximo que puede tener el índice, considerando el conjunto de claves incluidas en las páginas de nivel intermedio.

La opción PAD_INDEX solamente resulta útil si también se especifica FILLFACTOR, porque PAD_INDEX utiliza el mismo porcentaje especificado por FILLFACTOR. Si el porcentaje especificado para FILLFACTOR no es lo suficientemente grande como para admitir una fila, [!INCLUDE[ssDE](../../includes/ssde-md.md)] invalida internamente el porcentaje para permitir el valor mínimo. El número de filas de una página de nivel intermedio del índice no es nunca inferior a dos, independientemente de lo bajo que sea el valor de _fillfactor_.

En la sintaxis compatible con versiones anteriores, WITH PAD_INDEX es equivalente a WITH PAD_INDEX = ON.

FILLFACTOR **=** _fillfactor_      

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica un porcentaje que indica cuánto debe llenar el [!INCLUDE[ssDE](../../includes/ssde-md.md)] el nivel hoja de cada página de índice durante la creación o nueva generación de los índices. *fillfactor* debe ser un valor entero comprendido entre 1 y 100. Si *fillfactor* es 100, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] crea índices con las páginas hoja llenas al máximo de su capacidad.

La configuración de FILLFACTOR solo se aplica cuando se crea o se vuelve a generar el índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] no mantiene dinámicamente el porcentaje especificado de espacio disponible de las páginas. Para ver la configuración del factor de relleno, use la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

> [!IMPORTANT]
> La creación de un índice clúster con un valor de FILLFACTOR menor que 100 afecta a la cantidad de espacio de almacenamiento que ocupan los datos, porque [!INCLUDE[ssDE](../../includes/ssde-md.md)] vuelve a distribuir los datos cuando crea el índice clúster.

Para obtener más información, vea [Especificar el factor de relleno para un índice](../../relational-databases/indexes/specify-fill-factor-for-an-index.md).

SORT_IN_TEMPDB = { ON | **OFF** }      

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Indica si deben almacenarse resultados temporales de orden en **tempdb**. El valor predeterminado es OFF menos para Hiperescalado de Azure SQL Database. Para todas las operaciones de creación de índices en Hiperescalado, SORT_IN_TEMPDB siempre es ON, con independencia de la opción especificada, a menos que se use la reconstrucción de índices reanudable.

ACTIVAR      
Los resultados de ordenación intermedios utilizados para generar el índice se almacenan en **tempdb**. Esto puede reducir el tiempo necesario para crear un índice si **tempdb** y la base de datos de usuarios están en conjuntos de discos distintos. Sin embargo, esto aumenta la cantidad de espacio en disco utilizado durante la generación del índice.

Apagado      
Los resultados de orden intermedios se almacenan en la misma base de datos que el índice.

Además del espacio necesario en la base de datos del usuario para crear el índice, **tempdb** debe tener la misma cantidad de espacio adicional para almacenar los resultados de orden intermedio. Para más información, vea [Opción SORT_IN_TEMPDB para índices](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md).

En la sintaxis compatible con versiones anteriores, WITH SORT_IN_TEMPDB es equivalente a WITH SORT_IN_TEMPDB = ON.

IGNORE_DUP_KEY = { ON | **OFF** }      
Especifica la respuesta de error cuando una operación de inserción intenta insertar valores de clave duplicados en un índice único. La opción IGNORE_DUP_KEY se aplica solamente a operaciones de inserción realizadas tras crear o volver a generar el índice. La opción no tiene efecto cuando se ejecutan [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md), [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) o [UPDATE](../../t-sql/queries/update-transact-sql.md). El valor predeterminado es OFF.

ACTIVAR      
Se producirá un mensaje de advertencia cuando se inserten valores de clave duplicados en un índice único. Solo las filas que infrinjan la restricción de unicidad darán error.

Apagado      
Se producirá un mensaje de error cuando se inserten valores de clave duplicados en un índice único. Toda la operación INSERT se revertirá.

IGNORE_DUP_KEY no se puede establecer en ON para los índices creados en una vista, los índices que no sean únicos, los índices XML, los índices espaciales y los índices filtrados.

Para ver IGNORE_DUP_KEY, use [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

En la sintaxis compatible con versiones anteriores, WITH IGNORE_DUP_KEY es equivalente a WITH IGNORE_DUP_KEY = ON.

STATISTICS_NORECOMPUTE = { ON | **OFF**}      
Especifica si se vuelven a calcular las estadísticas de distribución. El valor predeterminado es OFF.

ACTIVAR      
Las estadísticas obsoletas no se vuelven a calcular automáticamente.

Apagado      
Se habilita la actualización automática de las estadísticas.

Para restaurar la actualización automática de estadísticas, establezca STATISTICS_NORECOMPUTE en OFF o ejecute UPDATE STATISTICS sin la cláusula NORECOMPUTE.

> [!IMPORTANT]
> Deshabilitar el cálculo automático de estadísticas de distribución puede impedir que el optimizador de consultas elija los planes de ejecución óptimos de las consultas relativas a la tabla.

En la sintaxis compatible con versiones anteriores, WITH STATISTICS_NORECOMPUTE es equivalente a WITH STATISTICS_NORECOMPUTE = ON.

STATISTICS_INCREMENTAL = { ON | **OFF** }     

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Cuando se establece en **ON**, se crean estadísticas por cada partición. Cuando se establece en **OFF**, se quita el árbol de estadísticas y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recalcula las estadísticas. El valor predeterminado es **OFF**.

Si no se admiten las estadísticas por partición, la opción se omite y se genera una advertencia. Las estadísticas incrementales no se admiten para los siguientes tipos de estadísticas:

- Estadísticas creadas con índices que no están alineados por partición con la tabla base.
- Estadísticas creadas sobre bases de datos secundarias legibles AlwaysOn.
- Estadísticas creadas sobre bases de datos de solo lectura.
- Estadísticas creadas sobre índices filtrados.
- Estadísticas creadas sobre vistas.
- Estadísticas creadas sobre tablas internas.
- Estadísticas creadas con índices espaciales o índices XML.

DROP_EXISTING = { ON | **OFF** }      
Es una opción para quitar y volver a generar el índice agrupado o no agrupado existente con las especificaciones de la columna modificada y mantener el mismo nombre para el índice. El valor predeterminado es OFF.

ACTIVAR      
Especifica que se debe quitar y volver a generar el índice existente, que debe tener el mismo nombre que el parámetro *index_name*.

Apagado      
Especifica que no se debe quitar y volver a generar el índice existente. SQL Server muestra un error si ya existe el nombre de índice especificado.

Con DROP_EXISTING, puede cambiar:

- Un índice no agrupado de almacén de filas por un índice agrupado de almacén de filas.

Con DROP_EXISTING, no puede cambiar:

- Un índice agrupado de almacén de filas por un índice no agrupado de almacén de filas.
- Un índice de almacén de columnas agrupado por cualquier tipo de índice de almacén de filas.

En la sintaxis compatible con versiones anteriores, WITH DROP_EXISTING es equivalente a WITH DROP_EXISTING = ON.

ONLINE = { ON | **OFF** }      
Especifica si las tablas subyacentes y los índices asociados están disponibles para realizar consultas y modificar datos durante la operación de índice. El valor predeterminado es OFF.

> [!IMPORTANT]
> Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md).

ACTIVAR      
Los bloqueos de tabla de larga duración no se mantienen durante la operación de indización. Durante la fase principal de la operación de índice, solo se mantiene un bloqueo preventivo en la tabla de origen. Esto habilita las consultas o actualizaciones en la tabla subyacente y en los índices. Al inicio de la operación, se mantiene un bloqueo compartido (S) en el objeto de origen durante un período de tiempo muy corto. Al final de la operación, se adquiere un bloqueo S (compartido) sobre el origen durante un corto período, si se está creando un índice no clúster; o bien, se adquiere un bloqueo SCH-M (modificación del esquema) cuando se crea o se quita un índice clúster en línea, y cuando se vuelve a crear un índice clúster o no clúster. ONLINE no se puede establecer en ON cuando se crea un índice en una tabla temporal local.

Apagado      
Los bloqueos de tabla se aplican durante la operación de índice. Una operación de índice sin conexión para crear, volver a crear o quitar un índice clúster, o para volver a crear o quitar un índice no clúster, adquiere un bloqueo de modificación del esquema (Sch-M) de la tabla. Esto evita que todos los usuarios tengan acceso a la tabla subyacente durante la operación. Una operación de índice sin conexión que crea un índice no clúster adquiere un bloqueo compartido (S) en la tabla. Esto evita que se realicen actualizaciones en la tabla subyacente, pero permite la realización de operaciones de lectura, como instrucciones SELECT.

Para más información, consulte [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  

Los índices, incluidos los índices de las tablas temp globales, se pueden crear en línea, salvo en los casos siguientes:

- Índice XML
- Índice de una tabla temporal local
- Índice clúster único inicial en una vista
- Índices clúster deshabilitados
- Índices de almacén de columnas
- Índice clúster, si la tabla subyacente contiene tipos de datos LOB (**image**, **ntext**, **text**) y tipos de datos espaciales
- Las columnas **varchar(max)** y **varbinary(max)** no pueden formar parte de un índice. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], cuando una tabla contiene columnas **varchar(max)** o **varbinary(max)** , puede crearse o volver a crearse un índice agrupado que contiene otras columnas con la opción **ONLINE**. [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no permite la opción **ONLINE** cuando la tabla contiene columnas **varchar(max)** o **varbinary(max)**

Para más información, vea [Cómo funcionan las operaciones de índice en línea](../../relational-databases/indexes/how-online-index-operations-work.md).

RESUMABLE **=** { ON | **OFF**}      

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

 Especifica si una operación de índice en línea se puede reanudar.

 ACTIVAR      
la operación de índice se puede reanudar.

 Apagado      
la operación de índice no se puede reanudar.

MAX_DURATION **=** *time* [**MINUTES**] se usa con **RESUMABLE = ON** (requiere **ONLINE = ON**)   

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Indica el tiempo (valor entero especificado en minutos) durante el cual se ejecuta una operación de índice en línea reanudable antes de ponerse en pausa.

> [!IMPORTANT]
> Para saber más sobre las operaciones de índices que pueden realizarse en línea, vea [Directrices para operaciones de índices en línea](../../relational-databases/indexes/guidelines-for-online-index-operations.md).

> [!NOTE] 
> No se admiten las recompilaciones de índices en línea reanudables en los índices de almacén de columnas.

ALLOW_ROW_LOCKS = { **ON** | OFF }      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica si se permiten los bloqueos de fila. El valor predeterminado es ON.

ACTIVAR      
Los bloqueos de fila se admiten al obtener acceso al índice. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina cuándo se usan los bloqueos de fila.

Apagado      
No se usan los bloqueos de fila.

ALLOW_PAGE_LOCKS = { **ON** | OFF }      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica si se permiten bloqueos de página. El valor predeterminado es ON.

ACTIVAR      
Los bloqueos de página se permiten al obtener acceso al índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina el momento en que se usan los bloqueos de página.

Apagado      
No se utilizan bloqueos de página.

OPTIMIZE_FOR_SEQUENTIAL_KEY = { ON | **OFF** }      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica si se deben optimizar la contención de inserción de la última página. El valor predeterminado es OFF. Consulte la sección [Claves secuenciales](#sequential-keys) para obtener más información.

MAXDOP = *max_degree_of_parallelism*      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Reemplaza la opción de configuración de **max_degree_of_parallelism** mientras dure la operación de índice. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.

*max_degree_of_parallelism* puede tener estos valores:

1      
Suprime la generación de planes paralelos.

\>1      
Restringe el número máximo de procesadores utilizados en una operación de índice paralelo al número especificado o a un número inferior, en función de la actual carga de trabajo del sistema.

0 (predeterminado)      
Usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.

 Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Las operaciones de índices en paralelo no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Ediciones y características admitidas de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) y [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

DATA_COMPRESSION      
Especifica la opción de compresión de datos para el índice, número de partición o intervalo de particiones especificado. Las opciones son las siguientes:

Ninguno      
No se comprimen el índice ni las particiones especificadas.

ROW      
El índice o las particiones especificadas se comprimen mediante la compresión de fila.

PAGE      
El índice o las particiones especificadas se comprimen mediante la compresión de página.

Para más información sobre la compresión, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).

ON PARTITIONS **(** { \<partition_number_expression> | \<range> } [ **,** ..._n_ ] **)**       
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica las particiones a las que se aplica el valor DATA_COMPRESSION. Si el índice no tiene particiones, el argumento ON PARTITIONS generará un error. Si no se proporciona la cláusula ON PARTITIONS, la opción DATA_COMPRESSION se aplica a todas las particiones de un índice con particiones.

\<partition_number_expression> se puede especificar de las siguientes maneras:

- Proporcionar el número de una partición, por ejemplo: EN PARTICIONES (2).
- Proporcionar los números de partición para varias particiones individuales separadas por comas, por ejemplo: EN PARTICIONES (1, 5).
- Proporcione ambos rangos y las particiones individuales, por ejemplo: EN PARTICIONES (2, 4, 6 A 8).

\<range> se puede especificar como números de partición separados por la palabra TO, como por ejemplo: `ON PARTITIONS (6 TO 8)`.

 Para establecer diferentes tipos de compresión de datos para distintas particiones, especifique la opción DATA_COMPRESSION más de una vez, por ejemplo:

```sql
REBUILD WITH
(
  DATA_COMPRESSION = NONE ON PARTITIONS (1),
  DATA_COMPRESSION = ROW ON PARTITIONS (2, 4, 6 TO 8),
  DATA_COMPRESSION = PAGE ON PARTITIONS (3, 5)
);
```

## <a name="remarks"></a>Observaciones
La instrucción CREATE INDEX se optimiza como cualquier otra consulta. Para guardar en operaciones de E/S, el procesador de consultas puede elegir examinar otro índice en lugar de realizar un recorrido de tabla. La operación de orden se puede eliminar en algunos casos. En equipos con varios procesadores, CREATE INDEX puede utilizar más procesadores para realizar las operaciones de examen y orden asociadas a la creación del índice, al igual que hacen otras consultas. Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).

La operación de creación de índices se registra al mínimo si el modelo de recuperación de base de datos se establece en Registro masivo o Sencillo.

Los índices se pueden crear en una tabla temporal. Cuando se quita la tabla o finaliza la sesión, se quitan los índices.

Se puede crear un índice agrupado en una variable de tabla cuando se crea una clave principal. Cuando se completa la consulta o se termina la sesión, se quita el índice.

Los índices admiten propiedades extendidas.

## <a name="clustered-indexes"></a>Índices clúster
La creación de un índice clúster en una tabla (montón) o la eliminación y nueva creación de un índice clúster existente requiere área de espacio adicional disponible en la base de datos para acomodar la ordenación de datos y una copia temporal de la tabla original o datos del índice clúster existente. Para obtener más información sobre los índices agrupados, vea [Creación de índices agrupados](../../relational-databases/indexes/create-clustered-indexes.md) y la [Guía de diseño y de arquitectura de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="nonclustered-indexes"></a>Índices no agrupados
A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], puede crear un índice no agrupado en una tabla almacenada como un índice de almacén de columnas agrupado. Si primero crea un índice no agrupado en una tabla almacenada como un índice agrupado o montón, el índice se conservará si más adelante convierte la tabla a un índice de almacén de columnas agrupado. Además, no es necesario quitar el índice no agrupado al volver a generar el índice de almacén de columnas agrupado.

Limitaciones y restricciones:

- La opción `FILESTREAM_ON` no es válida para crear un índice no agrupado en una tabla almacenada como un índice de almacén de columnas agrupado.

## <a name="unique-indexes"></a>Índices únicos
Cuando existe un índice único, [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba si hay valores duplicados cada vez que se agregan datos con una operación de inserción. Las operaciones de inserción que generarían valores de clave duplicados se revierten y el [!INCLUDE[ssDE](../../includes/ssde-md.md)] muestra un mensaje de error. Esto se cumple incluso si la operación de inserción cambia muchas filas pero crea un único duplicado. Si se intenta indicar datos donde existe un índice único y se ha especificado la cláusula IGNORE_DUP_KEY en ON, solo causarán un error las filas que infrinjan el índice UNIQUE.

## <a name="partitioned-indexes"></a>Índices con particiones
La creación y el mantenimiento de los índices con particiones son similares a los de las tablas con particiones pero, al igual que en índices ordinarios, éstos son tratados como objetos de base de datos independientes. Puede tener un índice con particiones en una tabla que carezca de particiones, y puede tener un índice sin particiones en una tabla que tenga particiones.

Si crea un índice en una tabla con particiones y no especifica un grupo de archivos en el que desea ubicar el índice, se crean particiones en el índice de la misma manera que en la tabla subyacente. Esto se debe a que, de manera predeterminada, los índices se ubican en los mismos grupos de archivos que sus tablas subyacentes, y en una tabla con particiones del mismo esquema de partición que usa las mismas columnas de partición. Cuando el índice usa el mismo esquema y columna de partición que la tabla, el índice está *alineado* con la tabla.

> [!WARNING]
> La creación y regeneración de índices no alineados en una tabla con más de 1.000 particiones es posible, pero no se admite. Si se hace, se puede degradar el rendimiento o consumir excesiva memoria durante estas operaciones. Se recomienda usar solo índices alineados cuando el número de particiones sea superior a 1.000.

Cuando se crean particiones en un índice clúster no único, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] agrega de forma predeterminada las columnas de partición a la lista de claves del índice clúster, en caso de que no se hubieran especificado aún.

Se pueden crear vistas indizadas en tablas con particiones de la misma manera que se hace con índices en tablas. Para obtener más información acerca de los índices con particiones, vea [Índices y tablas con particiones](../../relational-databases/partitions/partitioned-tables-and-indexes.md) y [Guía de diseño y arquitectura de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md).

En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], las estadísticas no se crean examinando todas las filas de la tabla cuando se crea o se vuelve a generar un índice con particiones. En su lugar, el optimizador de consultas usa el algoritmo de muestreo predeterminado para generar estadísticas. Para obtener estadísticas sobre índices con particiones examinando todas las filas de la tabla, use `CREATE STATISTICS` o `UPDATE STATISTICS` con la cláusula `FULLSCAN`.

## <a name="filtered-indexes"></a>Índices filtrados
Un índice filtrado es un índice no clúster optimizado, adecuado para las consultas que seleccionan un porcentaje pequeño de las filas de una tabla. Utiliza un predicado de filtro para indizar una parte de los datos de la tabla. Un índice filtrado bien diseñado puede mejorar el rendimiento de las consultas, reducir los costos de almacenamiento y de mantenimiento.

### <a name="required-set-options-for-filtered-indexes"></a>Opciones SET requeridas para los índices filtrados

Las opciones SET de la columna de valor requerido son necesarias siempre que se dé alguna de las condiciones siguientes:

- Se crea un índice filtrado.
- La operación INSERT, UPDATE, DELETE o MERGE modifica los datos de un índice filtrado.
- El optimizador de consultas usa el índice filtrado para crear el plan de consulta.

    |Opciones de Set|Valor requerido|Valor de servidor predeterminado|Valor predeterminado<br /><br /> Valor de OLE DB y ODBC|Valor predeterminado<br /><br /> predeterminado|
    |-----------------|--------------------|--------------------------|---------------------------------------|-----------------------------------|
    |ANSI_NULLS|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|
    |ANSI_PADDING|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|
    |ANSI_WARNINGS*|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|
    |ARITHABORT|ACTIVAR|ACTIVAR|Apagado|Apagado|
    |CONCAT_NULL_YIELDS_NULL|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|
    |NUMERIC_ROUNDABORT|Apagado|Apagado|Apagado|Apagado|
    |QUOTED_IDENTIFIER|ACTIVAR|ACTIVAR|ACTIVAR|Apagado|
  
     * Al establecer ANSI_WARNINGS en ON, ARITHABORT se establece de forma implícita en ON cuando el nivel de compatibilidad de base de datos está establecido en 90 o un valor superior. Si el nivel de compatibilidad de la base de datos está establecido en 80 o en un nivel inferior, debe configurarse explícitamente la opción ARITHABORT en ON.

Si las opciones SET son incorrectas, se pueden producir las condiciones siguientes:

- El índice filtrado no se crea.
- El [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un error y revierte cualquier instrucción INSERT, UPDATE, DELETE o MERGE que cambia los datos del índice.
- El optimizador de consultas no tiene en cuenta el índice en el plan de ejecución de ninguna instrucción Transact-SQL.

 Para obtener más información acerca de los índices con filtros, vea [Creación de índices con filtros](../../relational-databases/indexes/create-filtered-indexes.md) y [Guía de diseño y arquitectura de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="spatial-indexes"></a>Índices espaciales
Para obtener información sobre los índices espaciales, vea [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md) e [Información general sobre los índices espaciales](../../relational-databases/spatial/spatial-indexes-overview.md).

## <a name="xml-indexes"></a>Índices XML
Para obtener información sobre los índices XML, vea [CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md) e [Índices XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md).

## <a name="index-key-size"></a>Tamaño de clave de índice
El tamaño máximo de una clave de índice es de 900 bytes para un índice agrupado y de 1700 bytes para un índice no agrupado. (Antes de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] y [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el límite era siempre de 900 bytes). Se pueden crear índices en las columnas **varchar** cuyo tamaño sea superior al límite de bytes si los datos que contienen no superan ese tamaño al crearse el índice. Sin embargo, se producirá un error en las acciones de inserción o actualización posteriores en las columnas que hagan que el tamaño total sea mayor que el límite. La clave de índice de un índice agrupado no puede contener columnas **varchar** con datos existentes en la unidad de asignación ROW_OVERFLOW_DATA. Si se crea un índice agrupado en una columna **varchar** y los datos existentes están en la unidad de asignación IN_ROW_DATA, no se realizarán correctamente las siguientes acciones de inserción o actualización en la columna que intenten insertar los datos de manera no consecutiva.

Los índices no clúster pueden incluir columnas que no son de clave en el nivel de hoja del índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] no tiene en cuenta estas columnas al calcular el tamaño de clave de índice. Para obtener más información vea [Creación de índices con columnas incluidas](../../relational-databases/indexes/create-indexes-with-included-columns.md) y [Guía de diseño y arquitectura de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md).

> [!NOTE]
> Cuando se dividen las tablas, si las columnas de clave de la partición no están aún presentes en un índice clúster no único, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] las agrega al índice. El tamaño combinado de las columnas indizadas (sin contar las columnas incluidas) más cualquier columna de partición agregada no puede exceder 1800 bytes en un índice clúster no único.

## <a name="computed-columns"></a>Columnas calculadas
Los índices se pueden crear en columnas calculadas. Además, las columnas calculadas pueden tener la propiedad PERSISTED. Esto significa que [!INCLUDE[ssDE](../../includes/ssde-md.md)] almacena los valores calculados en la tabla y los actualiza cuando se actualiza cualquier otra columna de la que depende la columna calculada. [!INCLUDE[ssDE](../../includes/ssde-md.md)] utiliza estos valores persistentes cuando crea un índice en la columna y cuando se hace referencia al índice en una consulta.

Para indizar una columna calculada, ésta debe ser determinista y precisa. No obstante, si se usa la propiedad PERSISTED, se amplía el tipo de columnas calculadas indizables para incluir:

- Las columnas calculadas basadas en [!INCLUDE[tsql](../../includes/tsql-md.md)], funciones CLR y métodos de tipos definidos por el usuario CLR que el usuario ha marcado como deterministas.
- Las columnas calculadas basadas en expresiones que son deterministas, como se definen en [!INCLUDE[ssDE](../../includes/ssde-md.md)], aunque imprecisas.

Las columnas calculadas persistentes requieren que se establezcan las siguientes opciones SET de la manera indicada en la sección anterior, [Opciones SET requeridas para los índices filtrados](#required-set-options-for-filtered-indexes).

Las restricciones UNIQUE o PRIMARY KEY pueden contener una columna calculada siempre que cumplan con todas las condiciones de creación del índice. En concreto, la columna calculada debe ser determinista y precisa, o determinista y persistente. Para obtener más información sobre el determinismo, vea [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md).

Las columnas calculadas derivadas de los tipos de datos **image**, **ntext**, **text**, **varchar(max)** , **nvarchar(max)** , **varbinary(max)** y **xml** se pueden indexar como una columna de clave o columna sin clave incluida, siempre que el tipo de datos de la columna calculada esté disponible como una columna índice de clave o sin clave. Por ejemplo, no puede crear un índice XML principal en una columna **xml** calculada. Si el tamaño de clave de índice supera los 900 bytes, se muestra un mensaje de advertencia.

La creación de un índice en una columna calculada puede producir un error en una operación de inserción o actualización que antes funcionaba. Este error podría ocurrir cuando la columna calculada produce un error aritmético. Por ejemplo, aunque la columna calculada `c` de la tabla siguiente produzca un error aritmético, la instrucción INSERT funcionará.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
INSERT INTO t1 VALUES (1, 0);
```

En cambio, si después de crear la tabla crea un índice en la columna calculada `c`, la misma instrucción `INSERT` producirá un error.

```sql
CREATE TABLE t1 (a INT, b INT, c AS a/b);
CREATE UNIQUE CLUSTERED INDEX Idx1 ON t1(c);
INSERT INTO t1 VALUES (1, 0);
```

Para obtener más información, vea [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

## <a name="included-columns-in-indexes"></a>Columnas incluidas en índices
Las columnas que no son de clave, denominadas columnas incluidas, se pueden agregar en el nivel hoja de un índice no clúster para mejorar el rendimiento de las consultas al cubrir la consulta. Es decir, todas las columnas a las que se hace referencia en la consulta se incluyen en el índice como columnas de clave o que no son de clave. De este modo, el optimizador de consultas puede ubicar toda la información requerida con un examen del índice; no se tiene acceso a los datos de la tabla o del índice clúster. Para obtener más información vea [Creación de índices con columnas incluidas](../../relational-databases/indexes/create-indexes-with-included-columns.md) y [Guía de diseño y arquitectura de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md).

## <a name="specifying-index-options"></a>Especificar opciones de índice
[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] incluye opciones de índice nuevas y también modifica el modo en que se especifican las opciones. En la sintaxis compatible con versiones anteriores, WITH *option_name* es equivalente a WITH **(** \<option_name> **= ON )** . Al establecer opciones de índice, se aplican las siguientes reglas:

- Las nuevas opciones de índice solo se pueden especificar mediante WITH (**_option\_name_ = ON | OFF**).
- Las opciones no se pueden especificar utilizando la sintaxis compatible con versiones anteriores y la nueva sintaxis en la misma instrucción. Por ejemplo, al especificar WITH (**DROP_EXISTING, ONLINE = ON**), se genera un error en la instrucción.
- Cuando se crea un índice XML, las opciones se deben especificar mediante WITH (**_option_name_= ON | OFF**).

## <a name="drop_existing-clause"></a>Cláusula DROP_EXISTING
Puede utilizar la cláusula DROP_EXISTING para volver a generar el índice, agregar o quitar columnas, modificar opciones, modificar el criterio de ordenación de las columnas o cambiar el grupo de archivos o el esquema de partición.

Si el índice exige una restricción PRIMARY KEY o UNIQUE, y la definición de índice no se ha modificado en absoluto, se quita el índice y se vuelve a crear conservando la restricción existente. Sin embargo, si se ha modificado la definición de índice, se genera un error en la instrucción. Para cambiar la definición de una restricción PRIMARY KEY o UNIQUE, quite la restricción y agregue una restricción con la nueva definición.

DROP_EXISTING mejora el rendimiento cuando se vuelve a crear un índice clúster (con el mismo conjunto de claves o con uno distinto) en una tabla que también tiene índices no clúster. DROP_EXISTING reemplaza la ejecución de una instrucción DROP INDEX en el antiguo índice clúster seguida de la ejecución de una instrucción CREATE INDEX para el nuevo índice clúster. Los índices no clúster se vuelven a generar una vez, siempre que la definición de índice haya cambiado. La cláusula DROP_EXISTING no vuelve a generar los índices no clúster cuando la definición de índice posee los mismos nombres de índice, clave y columnas de partición, atributo de unicidad y criterio de ordenación que el índice original.

Independientemente de si se vuelven a generar o no los índices no clúster, éstos siempre permanecen en sus esquemas de partición o grupos de archivos originales, y utilizan las funciones de partición originales. Si un índice clúster se vuelve a generar en un esquema de partición o grupo de archivos diferente, los índices no clúster no se mueven para coincidir con la nueva ubicación del índice clúster. Por lo tanto, es posible que incluso los índices no clúster alineados previamente con el índice clúster no se puedan alinear con éste. Para más información sobre la alineación de índices con particiones, vea [Tablas e índices con particiones](../../relational-databases/partitions/partitioned-tables-and-indexes.md).

La cláusula DROP_EXISTING no volverá a ordenar los datos si se utilizan las mismas columnas de clave de índice en el mismo orden y con la misma disposición ascendente o descendente, a menos que la instrucción del índice especifique un índice no clúster y la opción ONLINE se establezca en OFF. Si se deshabilita el índice clúster, se debe establecer ONLINE en OFF para la operación CREATE INDEX WITH DROP_EXISTING. Si se deshabilita un índice no clúster y no se asocia con un índice clúster deshabilitado, se puede establecer ONLINE en OFF u ON para la operación CREATE INDEX WITH DROP_EXISTING.

> [!NOTE]
> Cuando se quitan o se vuelven a generar índices con 128 o más extensiones, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] aplaza las cancelaciones de asignación de página reales y los bloqueos asociados, hasta después de que se confirme la transacción.

## <a name="online-option"></a>Opción ONLINE
Las directrices siguientes se aplican para el desarrollo de operaciones de índice en línea:

- La tabla subyacente no se podrá alterar, truncar ni quitar mientras haya una operación de índice en línea en curso.
- La operación de índice requiere un espacio en disco temporal adicional.
- Las operaciones en línea se pueden realizar en índices con particiones e índices que contienen columnas calculadas persistentes, o columnas incluidas.

Para más información, consulte [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).

### <a name="resumable-index-operations"></a><a name="resumable-indexes"></a> Operaciones de índice reanudable
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Se aplican las directrices siguientes para las operaciones de índice reanudable:

- ONLINE INDEX CREATE se especifica como RESUMABLE con la opción `RESUMABLE = ON`.
- La opción RESUMABLE no se conserva en los metadatos de un índice dado y solo se aplica a la duración de una instrucción DDL actual. Por tanto, la cláusula `RESUMABLE = ON` debe especificarse explícitamente para habilitar la capacidad de reanudación.
- La opción MAX_DURATION solo se admite para la opción `RESUMABLE = ON`.
- MAX_DURATION para la opción RESUMABLE especifica el intervalo de tiempo para compilar un índice. Una vez pasado este tiempo, la operación de compilación de índice se pausa o completa su ejecución. El usuario decide cuándo se puede reanudar una compilación de un índice en pausa. El valor **time** en minutos para MAX_DURATION debe ser mayor que 0 minutos o menor o igual que una semana (7 \* 24 \* 60 = 10080 minutos). Si se hace una pausa larga en una operación de índice puede afectar al rendimiento de DML en una tabla específica, así como a la capacidad de disco de base de datos, dado que ambos índices, el original y el que se acaba de crear, necesitan espacio en disco y deben actualizarse durante las operaciones de DML. Si se omite la opción MAX_DURATION, la operación de índice continuará hasta su finalización o hasta que se produzca un error.
- Para pausar inmediatamente la operación de índice, puede detener (Ctrl+C) el comando en curso, ejecutar el comando [ALTER INDEX](alter-index-transact-sql.md) PAUSE o ejecutar el comando `KILL <session_id>`. Una vez que el comando está en pausa, se puede reanudar mediante el comando [ALTER INDEX](alter-index-transact-sql.md).
- Al volver a ejecutar la instrucción CREATE INDEX para el índice reanudable, se reanuda automáticamente una operación de creación de índice en pausa.
- La opción `SORT_IN_TEMPDB = ON` no es compatible con el índice reanudable.
- El comando DDL con `RESUMABLE = ON` no se puede ejecutar dentro de una transacción explícita (no puede formar parte del bloque begin TRAN ... COMMIT).
- Para reanudar o anular una creación o recompilación de índice, use la sintaxis de T-SQL [ALTER INDEX](alter-index-transact-sql.md)

> [!NOTE]
> El comando DDL se ejecuta hasta que se completa, se pone en pausa o genera un error. En caso de que el comando se ponga en pausa, se emite un error que indica que se pausó la operación y que no se completó la creación de índice. Para más información sobre el estado actual del índice, vea [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md). Como ocurrió antes, en caso de fallo se generará también un error.

Para indicar que la creación de un índice se ejecuta como una operación reanudable y para comprobar su estado de ejecución actual, consulte [sys.index_resumable_operations](../../relational-databases/system-catalog-views/sys-index-resumable-operations.md).

#### <a name="resources"></a>Recursos
Los recursos siguientes son necesarios para la operación de creación de índice en línea:

- el espacio adicional necesario para mantener el índice que se está generando, incluida la hora en que se pausó el índice;
- Rendimiento de registro adicional durante la fase de ordenación. El uso de espacio del registro general para el índice reanudable es menor en comparación con la creación del índice en línea habitual y permite truncar el registro durante esta operación.
- un estado DDL que impida cualquier modificación de DDL.
- La limpieza de registros fantasma se bloquea en el índice de la compilación durante la operación, tanto en pausa como cuando la operación está en ejecución.

#### <a name="current-functional-limitations"></a>Limitaciones funcionales actuales
Abajo se detallan las funciones que se deshabilitan para las operaciones de creación de índices reanudables:

- Después de poner en pausa una operación de creación de índice reanudable en línea, no es posible modificar el valor inicial de MAXDOP
- Crear un índice que contiene:

  - Columna calculada o columna TIMESTAMP como columnas de clave
  - Columna LOB como columna incluida para la creación de índices reanudables
  - Índice filtrado

## <a name="row-and-page-locks-options"></a>Opciones de bloqueo de fila y página
Si `ALLOW_ROW_LOCKS = ON` y `ALLOW_PAGE_LOCK = ON`, se permiten los bloqueos de nivel de fila, página y tabla al obtener acceso al índice. [!INCLUDE[ssDE](../../includes/ssde-md.md)] elige el bloqueo apropiado y puede cambiar de escala el bloqueo: de un bloqueo de fila o página a un bloqueo de tabla.

Si `ALLOW_ROW_LOCKS = OFF` y `ALLOW_PAGE_LOCK = OFF`, solo se permite un bloqueo de nivel de tabla al obtener acceso al índice.

## <a name="sequential-keys"></a>Claves secuenciales
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

La contención de inserción de la última página es un problema común de rendimiento que se produce cuando un gran número de subprocesos simultáneos intentan insertar filas en un índice con una clave secuencial. Un índice se considera secuencial cuando la columna de clave inicial contiene valores que siempre aumentan (o disminuyen), como una columna de identidad o una fecha que toma como valor predeterminado la fecha y hora actuales. Dado que las claves que se insertan son secuenciales, todas las nuevas filas se insertarán al final de la estructura del índice, es decir, en la misma página. Esta situación conduce a la contención de la página en memoria que se puede observar cuando varios subprocesos están a la espera de PAGELATCH_EX para la página en cuestión.

Al activar la opción de índice OPTIMIZE_FOR_SEQUENTIAL_KEY es posible una optimización en el motor de base de datos que ayuda a mejora el rendimiento de las inserciones de alta simultaneidad en el índice. Está concebida para los índices que tienen una clave secuencial y, por tanto, son propensos a la contención de inserción de la última página, pero también puede ayudar con índices que tienen zonas activas en otras áreas de la estructura del índice de árbol B.

## <a name="viewing-index-information"></a>Ver información de índice
Para devolver información sobre índices, puede utilizar vistas de catálogo, funciones del sistema y procedimientos almacenados del sistema.

## <a name="data-compression"></a>Data Compression
La compresión de datos se describe en el tema [Compresión de datos](../../relational-databases/data-compression/data-compression.md). A continuación se muestran los puntos clave que se deben tener en cuenta:

- La compresión puede permitir que se almacenen más filas en una página, pero no cambia el tamaño máximo de la fila.
- Las páginas no hoja de un índice no tienen compresión de página pero pueden tener compresión de fila.
- Cada índice no clúster tiene una configuración de compresión individual y no hereda la configuración de compresión de la tabla subyacente.
- Cuando se crea un índice clúster en un montón, el índice clúster hereda el estado de compresión del montón, a menos que se especifique otro estado de compresión.

Las restricciones siguientes se aplican a los índices con particiones:

- No se puede cambiar la configuración de compresión de una partición única si la tabla tiene índices no alineados.
- La sintaxis ALTER INDEX \<index> ... REBUILD PARTITION ... vuelve a generar la partición especificada del índice.
- La sintaxis ALTER INDEX \<index> ... REBUILD WITH ... vuelve a generar todas las particiones del índice.

Para evaluar cómo afecta el cambio del estado de compresión a una tabla, índice o partición, use el procedimiento almacenado [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .

## <a name="permissions"></a>Permisos
Requiere el permiso `ALTER` en la tabla, o bien la vista o pertenencia en el rol fijo de base de datos `db_ddladmin`.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones
En [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], no puede crear lo siguiente:

- Un índice de almacén de filas agrupado o no agrupado en una tabla de almacén de datos cuando ya existe un índice de almacén de columnas. Este comportamiento es diferente de SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], que permite que los índices de almacén de filas y columnas coexistan en la misma tabla.
- No puede crear un índice en una vista.

## <a name="metadata"></a>Metadatos
Para ver información sobre los índices existentes, puede consultar la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).

## <a name="version-notes"></a>Notas de la versión

[!INCLUDE[ssSDS](../../includes/sssds-md.md)] no admite opciones de grupo de archivos ni secuencia de archivos.

## <a name="examples-all-versions-uses-the-adventureworks-database"></a>Ejemplos: Todas las versiones. Utiliza la base de datos AdventureWorks

### <a name="a-create-a-simple-nonclustered-rowstore-index"></a>A. Crear un índice no clúster de almacén de filas simple
El ejemplo siguiente crea un índice no agrupado en la columna `VendorID` de la tabla `Purchasing.ProductVendor`.

```sql
CREATE INDEX IX_VendorID ON ProductVendor (VendorID);
CREATE INDEX IX_VendorID ON dbo.ProductVendor (VendorID DESC, Name ASC, Address DESC);
CREATE INDEX IX_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="b-create-a-simple-nonclustered-rowstore-composite-index"></a>B. Crear un índice compuesto de almacén de filas no agrupado
En el ejemplo siguiente se crea un índice compuesto no agrupado en las columnas `SalesQuota` y `SalesYTD` de la tabla `Sales.SalesPerson`.

```sql
CREATE NONCLUSTERED INDEX IX_SalesPerson_SalesQuota_SalesYTD ON Sales.SalesPerson (SalesQuota, SalesYTD);
```

### <a name="c-create-an-index-on-a-table-in-another-database"></a>C. Crear un índice en una tabla de otra base de datos
En el ejemplo siguiente se crea un índice agrupado en la columna `VendorID` de la tabla `ProductVendor` en la base de datos `Purchasing`.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID ON Purchasing..ProductVendor (VendorID);
```

### <a name="d-add-a-column-to-an-index"></a>D. Agregar una columna a un índice
En el ejemplo siguiente se crea el índice IX_FF con dos columnas de la tabla dbo.FactFinance. La instrucción siguiente vuelve a generar el índice con una columna más y mantiene el nombre existente.

```sql
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey ASC, DateKey ASC);

-- Rebuild and add the OrganizationKey
CREATE INDEX IX_FF ON dbo.FactFinance (FinanceKey, DateKey, OrganizationKey DESC)
  WITH (DROP_EXISTING = ON);
```

## <a name="examples-sql-server-azure-sql-database"></a>Ejemplos: SQL Server, Azure SQL Database

### <a name="e-create-a-unique-nonclustered-index"></a>E. Crear un índice no agrupado único
En el ejemplo siguiente se crea un índice no clúster único en la columna `Name` de la tabla `Production.UnitMeasure` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. El índice exigirá unicidad en los datos insertados en la columna `Name`.

```sql
CREATE UNIQUE INDEX AK_UnitMeasure_Name
  ON Production.UnitMeasure(Name);
```

La consulta siguiente prueba la restricción de unicidad intentando insertar una fila con el mismo valor que el de una fila existente.

```sql
-- Verify the existing value.
SELECT Name FROM Production.UnitMeasure WHERE Name = N'Ounces';
GO

INSERT INTO Production.UnitMeasure (UnitMeasureCode, Name, ModifiedDate)
  VALUES ('OC', 'Ounces', GETDATE());
```

El mensaje de error resultante es:

```
Server: Msg 2601, Level 14, State 1, Line 1
Cannot insert duplicate key row in object 'UnitMeasure' with unique index 'AK_UnitMeasure_Name'. The statement has been terminated.
```

### <a name="f-use-the-ignore_dup_key-option"></a>F. Usar la opción IGNORE_DUP_KEY
El ejemplo siguiente muestra el efecto de la opción `IGNORE_DUP_KEY` al insertar varias filas en una tabla temporal primero con la opción establecida en `ON` y luego con la opción establecida en `OFF`. Se inserta una única fila en la tabla `#Test` que intencionadamente proporcionará un valor duplicado cuando se ejecuta la segunda instrucción `INSERT` de varias filas. Un recuento de las filas de la tabla devuelve el número de filas insertadas.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = ON);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

A continuación se muestran los resultados de la segunda instrucción `INSERT`.

```cmd
Server: Msg 3604, Level 16, State 1, Line 5 Duplicate key was ignored.

Number of rows
--------------
38
```

Observe que las filas insertadas desde la tabla `Production.UnitMeasure` que no infringieron la restricción de unicidad se insertaron correctamente. Se emitió una advertencia y se omitió la fila duplicada, pero no se revirtió la transacción completa.

Las mismas instrucciones se ejecutan nuevamente, pero con `IGNORE_DUP_KEY` establecido en `OFF`.

```sql
CREATE TABLE #Test (C1 NVARCHAR(10), C2 NVARCHAR(50), C3 DATETIME);
GO

CREATE UNIQUE INDEX AK_Index ON #Test (C2)
  WITH (IGNORE_DUP_KEY = OFF);
GO

INSERT INTO #Test VALUES (N'OC', N'Ounces', GETDATE());
INSERT INTO #Test SELECT * FROM Production.UnitMeasure;
GO

SELECT COUNT(*) AS [Number of rows] FROM #Test;
GO

DROP TABLE #Test;
GO
```

A continuación se muestran los resultados de la segunda instrucción `INSERT`.

```
Server: Msg 2601, Level 14, State 1, Line 5
Cannot insert duplicate key row in object '#Test' with unique index
'AK_Index'. The statement has been terminated.

Number of rows
--------------
1
```

Observe que ninguna de las filas de la tabla `Production.UnitMeasure` se insertó en la tabla aunque solo una fila de la tabla infringió la restricción de índice `UNIQUE`.

### <a name="g-using-drop_existing-to-drop-and-re-create-an-index"></a>G. Usar DROP_EXISTING para quitar y volver a crear un índice
En el ejemplo siguiente se quita y se vuelve a crear un índice existente en la columna `ProductID` de la tabla `Production.WorkOrder` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] utilizando la opción `DROP_EXISTING`. También se establecen las opciones `FILLFACTOR` y `PAD_INDEX` .

```sql
CREATE NONCLUSTERED INDEX IX_WorkOrder_ProductID
  ON Production.WorkOrder(ProductID)
    WITH (FILLFACTOR = 80,
      PAD_INDEX = ON,
      DROP_EXISTING = ON);
GO
```

### <a name="h-create-an-index-on-a-view"></a>H. Crear un índice en una vista
Este ejemplo siguiente crea una vista y un índice en esa vista. Se incluyen dos consultas que utilizan la vista indizada.

```sql
-- Set the options to support indexed views
SET NUMERIC_ROUNDABORT OFF;
SET ANSI_PADDING, ANSI_WARNINGS, CONCAT_NULL_YIELDS_NULL, ARITHABORT,
  QUOTED_IDENTIFIER, ANSI_NULLS ON;
GO

-- Create view with schemabinding
IF OBJECT_ID ('Sales.vOrders', 'view') IS NOT NULL
  DROP VIEW Sales.vOrders;
GO

CREATE VIEW Sales.vOrders
  WITH SCHEMABINDING
AS
  SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Revenue,
    OrderDate, ProductID, COUNT_BIG(*) AS COUNT
  FROM Sales.SalesOrderDetail AS od, Sales.SalesOrderHeader AS o
  WHERE od.SalesOrderID = o.SalesOrderID
  GROUP BY OrderDate, ProductID;
GO

-- Create an index on the view
CREATE UNIQUE CLUSTERED INDEX IDX_V1
  ON Sales.vOrders (OrderDate, ProductID);
GO

-- This query can use the indexed view even though the view is
-- not specified in the FROM clause.
SELECT SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev,
  OrderDate, ProductID
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND ProductID BETWEEN 700 AND 800
    AND OrderDate >= CONVERT(DATETIME, '05/01/2002', 101)
GROUP BY OrderDate, ProductID
ORDER BY Rev DESC;
GO

-- This query can use the above indexed view
SELECT OrderDate, SUM(UnitPrice * OrderQty * (1.00 - UnitPriceDiscount)) AS Rev
FROM Sales.SalesOrderDetail AS od
  JOIN Sales.SalesOrderHeader AS o ON od.SalesOrderID = o.SalesOrderID
    AND DATEPART(mm, OrderDate) = 3
  AND DATEPART(yy, OrderDate) = 2002
GROUP BY OrderDate
ORDER BY OrderDate ASC;
GO
```

### <a name="i-create-an-index-with-included-non-key-columns"></a>I. Crear un índice con columnas incluidas (sin clave)
El ejemplo siguiente crea un índice no clúster con una columna de clave (`PostalCode`) y cuatro columnas que no son de clave (`AddressLine1`, `AddressLine2`, `City`, `StateProvinceID`). A continuación se presenta una consulta cubierta por el índice. Para mostrar el índice seleccionado con el optimizador de consultas, en el menú **Consulta** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], seleccione **Mostrar plan de ejecución estimado** antes de ejecutar la consulta.

```sql
CREATE NONCLUSTERED INDEX IX_Address_PostalCode
  ON Person.Address (PostalCode)
  INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);
GO

SELECT AddressLine1, AddressLine2, City, StateProvinceID, PostalCode
FROM Person.Address
WHERE PostalCode BETWEEN N'98000' and N'99999';
GO
```

### <a name="j-create-a-partitioned-index"></a>J. Crear un índice con particiones
En el ejemplo siguiente se crea un índice no clúster con particiones en `TransactionsPS1`, un esquema de partición existente en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. En este ejemplo se supone que se ha instalado el ejemplo de índice con particiones.

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
CREATE NONCLUSTERED INDEX IX_TransactionHistory_ReferenceOrderID
  ON Production.TransactionHistory (ReferenceOrderID)
  ON TransactionsPS1 (TransactionDate);
GO
```

### <a name="k-creating-a-filtered-index"></a>K. Crear un índice filtrado
En el ejemplo siguiente se crea un índice filtrado en la tabla Production.BillOfMaterials de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. El predicado de filtro puede incluir columnas que no son columnas de clave en el índice filtrado. El predicado de este ejemplo selecciona solo las filas en que EndDate no es NULL.

```sql
CREATE NONCLUSTERED INDEX "FIBillOfMaterialsWithEndDate"
  ON Production.BillOfMaterials (ComponentID, StartDate)
  WHERE EndDate IS NOT NULL;
```

### <a name="l-create-a-compressed-index"></a>L. Crear un índice comprimido
En el ejemplo siguiente se crea un índice en una tabla sin particiones utilizando la compresión de fila.

```sql
CREATE NONCLUSTERED INDEX IX_INDEX_1
  ON T1 (C2)
  WITH (DATA_COMPRESSION = ROW);
GO
```

En el ejemplo siguiente se crea un índice en una tabla con particiones utilizando la compresión de fila en todas las particiones del índice.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (DATA_COMPRESSION = ROW);
GO
```

 En el ejemplo siguiente se crea un índice en una tabla con particiones utilizando la compresión de página en la partición `1` del índice y la compresión de fila en las particiones `2` a `4` del índice.

```sql
CREATE CLUSTERED INDEX IX_PartTab2Col1
  ON PartitionTable1 (Col1)
  WITH (
    DATA_COMPRESSION = PAGE ON PARTITIONS(1),
    DATA_COMPRESSION = ROW ON PARTITIONS (2 TO 4)
  );
GO
```

### <a name="m-create-resume-pause-and-abort-resumable-index-operations"></a>M. Crear, reanudar, pausar y anular operaciones de índices reanudables
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx1 ON test_table (col1) WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx2 ON test_table (col2) WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx1 ON test_table PAUSE;
ALTER INDEX test_idx2 ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx1 ON test_table RESUME;
ALTER INDEX test_idx2 ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx1 ON test_table ABORT;
ALTER INDEX test_idx2 ON test_table ABORT;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="n-basic-syntax"></a>Hora Sintaxis básica
Crear, reanudar, pausar y anular operaciones de índices reanudables       

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
-- Execute a resumable online index create statement with MAXDOP=1
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, MAXDOP = 1, RESUMABLE = ON);

-- Executing the same command again (see above) after an index operation was paused, resumes automatically the index create operation.

-- Execute a resumable online index creates operation with MAX_DURATION set to 240 minutes. After the time expires, the resumable index create operation is paused.
CREATE INDEX test_idx ON test_table WITH (ONLINE = ON, RESUMABLE = ON, MAX_DURATION = 240);

-- Pause a running resumable online index creation
ALTER INDEX test_idx ON test_table PAUSE;

-- Resume a paused online index creation
ALTER INDEX test_idx ON test_table RESUME;

-- Abort resumable index create operation which is running or paused
ALTER INDEX test_idx ON test_table ABORT;
```

### <a name="o-create-a-nonclustered-index-on-a-table-in-the-current-database"></a>O. Creación de un índice no agrupado en una tabla en la base de datos actual
El ejemplo siguiente crea un índice no clúster en la columna `VendorID` de la tabla `ProductVendor`.

```sql
CREATE INDEX IX_ProductVendor_VendorID
  ON ProductVendor (VendorID);
```

### <a name="p-create-a-clustered-index-on-a-table-in-another-database"></a>P. Crear un índice agrupado en una tabla de otra base de datos
En el ejemplo siguiente se crea un índice no clúster en la columna `VendorID` de la tabla `ProductVendor`en la base de datos `Purchasing`.

```sql
CREATE CLUSTERED INDEX IX_ProductVendor_VendorID
  ON Purchasing..ProductVendor (VendorID);
```
### <a name="q-create-an-ordered-clustered-index-on-a-table"></a>Q. Creación de un índice agrupado ordenado en una tabla  
En el ejemplo siguiente se crea un índice agrupado ordenado en las columnas `c1` y `c2` de la tabla `T1` de la base de datos `MyDB`.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T1 
ORDER (c1, c2);

```

### <a name="r-convert-a-cci-to-an-ordered-clustered-index-on-a-table"></a>R. Conversión de un CCI en un índice agrupado ordenado en una tabla  
En el ejemplo siguiente se convierte el índice de almacén de columnas agrupado existente en un índice de almacén de columnas agrupado ordenado denominado `MyOrderedCCI` en las columnas `c1` y `c2` de la tabla `T2` en la base de datos `MyDB`.

```sql
CREATE CLUSTERED COLUMNSTORE INDEX MyOrderedCCI ON MyDB.dbo.T2
ORDER (c1, c2)
WITH (DROP_EXISTING = ON);

```

## <a name="see-also"></a>Consulte también
[Guía de diseño y de arquitectura de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md)     
[Realizar operaciones de índice en línea](../../relational-databases/indexes/perform-index-operations-online.md)  
[Índices y ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md#indexes-and-alter-table)     
[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)     
[CREATE PARTITION FUNCTION](../../t-sql/statements/create-partition-function-transact-sql.md)     
[CREATE PARTITION SCHEME](../../t-sql/statements/create-partition-scheme-transact-sql.md)     
[CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)      
[CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md)     
[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)    
[CREATE XML INDEX](../../t-sql/statements/create-xml-index-transact-sql.md)     
[Tipos de datos](../../t-sql/data-types/data-types-transact-sql.md)    
[DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)    
[DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)    
[Índices XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)     
[sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)     
[sys.index_columns](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)    
[sys.xml_indexes](../../relational-databases/system-catalog-views/sys-xml-indexes-transact-sql.md)     
[EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
