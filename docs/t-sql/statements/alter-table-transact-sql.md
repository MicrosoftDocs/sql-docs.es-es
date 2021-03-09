---
description: ALTER TABLE (Transact-SQL)
title: ALTER TABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WAIT_AT_LOW_PRIORITY
- ABORT_AFTER_WAIT
- ABORT_AFTER_WAIT_TSQL
- ALTER_TABLE_TSQL
- ALTER TABLE
- WAIT_AT_LOW_PRIORITY_TSQL
- ALTER_COLUMN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- columns [SQL Server], resizing
- changing column size
- MAXDOP index option, ALTER TABLE statement
- table modifications [SQL Server], ALTER TABLE
- ALTER TABLE statement
- modifying tables
- partitioned tables [SQL Server], lock escalation
- resizing columns
- removing columns
- switching partitions
- reassigning partitions
- removing constraints
- triggers [SQL Server], disabling
- columns [SQL Server], adding
- LOCK_ESCALATION option of ALTER TABLE
- constraints [SQL Server], deleting
- constraints [SQL Server], disabling
- triggers [SQL Server], enabling
- re-enabling constraints
- index modifications [SQL Server]
- disabling constraints
- columns [SQL Server], removing
- max degree of parallelism option
- locking [SQL Server], tables
- ONLINE option
- disabling triggers
- constraints [SQL Server], adding
- deleting constraints
- adding constraints
- adding columns
- SWITCH partitions
- partitioned tables [SQL Server], switching
- lock escalation [SQL Server], option of ALTER TABLE
- constraints [SQL Server], enabling
- dropping constraints
- dropping columns
- data retention policy
- table changes [SQL Server]
ms.assetid: f1745145-182d-4301-a334-18f799d361d1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6815d21b6a75216f4fc4a8bfccf2d2d3b0aa585e
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186535"
---
# <a name="alter-table-transact-sql"></a>ALTER TABLE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Modifica una definición de tabla mediante la alteración, adición o retirada de columnas y restricciones. ALTER TABLE también vuelve a asignar y compilar particiones, o deshabilita y habilita restricciones y desencadenadores.

Para obtener más información sobre las convenciones de sintaxis, vea [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!IMPORTANT]
> La sintaxis de ALTER TABLE es diferente para las tablas basadas en disco y las tablas optimizadas para memoria. Utilice los siguientes vínculos para acceder directamente al bloque de sintaxis apropiado para los tipos de tabla y a los ejemplos de sintaxis apropiados:
>
> - Tablas basadas en disco:
>
>   - [Sintaxis](#syntax-for-disk-based-tables)
>   - [Ejemplos](#Example_Top)
> - Tablas optimizadas para memoria
>
>   - [Sintaxis](#syntax-for-memory-optimized-tables)
>   - [Ejemplos](../../relational-databases/in-memory-oltp/altering-memory-optimized-tables.md)

## <a name="syntax-for-disk-based-tables"></a>Sintaxis para las tablas basadas en disco

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                 | max
                 | xml_schema_collection
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ] [ SPARSE ]
      | { ADD | DROP }
          { ROWGUIDCOL | PERSISTED | NOT FOR REPLICATION | SPARSE | HIDDEN }
      | { ADD | DROP } MASKED [ WITH ( FUNCTION = ' mask_function ') ]
    }
    [ WITH ( ONLINE = ON | OFF ) ]
    | [ WITH { CHECK | NOCHECK } ]
  
    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <column_set_definition>
    } [ ,...n ]
      | [ system_start_time_column_name datetime2 GENERATED ALWAYS AS ROW START
                [ HIDDEN ] [ NOT NULL ] [ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
                system_end_time_column_name datetime2 GENERATED ALWAYS AS ROW END
                   [ HIDDEN ] [ NOT NULL ][ CONSTRAINT constraint_name ]
            DEFAULT constant_expression [WITH VALUES] ,
        ]
       PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )
    | DROP
     [ {
         [ CONSTRAINT ][ IF EXISTS ]
         {
              constraint_name
              [ WITH
               ( <drop_clustered_constraint_option> [ ,...n ] )
              ]
          } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }
  
    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | { ENABLE | DISABLE } CHANGE_TRACKING
        [ WITH ( TRACK_COLUMNS_UPDATED = { ON | OFF } ) ]
  
    | SWITCH [ PARTITION source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]

    | SET
        (
            [ FILESTREAM_ON =
                { partition_scheme_name | filegroup | "default" | "NULL" } ]
            | SYSTEM_VERSIONING =
                  {
                      OFF
                  | ON
                      [ ( HISTORY_TABLE = schema_name . history_table_name
                          [, DATA_CONSISTENCY_CHECK = { ON | OFF } ]
                          [, HISTORY_RETENTION_PERIOD =
                          {
                              INFINITE | number {DAY | DAYS | WEEK | WEEKS
                  | MONTH | MONTHS | YEAR | YEARS }
                          }
                          ]
                        )
                      ]
                  }
            | DATA_DELETION =  
                {
                      OFF 
                    | ON  
                        [(  [ FILTER_COLUMN = column_name ]   
                            [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS 
                                    | MONTH | MONTHS | YEAR | YEARS }}]   
                        )]
                   }
    | REBUILD
      [ [PARTITION = ALL]
        [ WITH ( <rebuild_option> [ ,...n ] ) ]
      | [ PARTITION = partition_number
           [ WITH ( <single_partition_rebuild_option> [ ,...n ] ) ]
        ]
      ]
  
    | <table_option>
    | <filetable_option>
    | <stretch_configuration>
}
[ ; ]
  
-- ALTER TABLE options
  
<column_set_definition> ::=
    column_set_name XML COLUMN_SET FOR ALL_SPARSE_COLUMNS

<drop_clustered_constraint_option> ::=
    {
        MAXDOP = max_degree_of_parallelism
      | ONLINE = { ON | OFF }
      | MOVE TO
         { partition_scheme_name ( column_name ) | filegroup | "default" }
    }
<table_option> ::=
    {
        SET ( LOCK_ESCALATION = { AUTO | TABLE | DISABLE } )
    }
  
<filetable_option> ::=
    {
       [ { ENABLE | DISABLE } FILETABLE_NAMESPACE ]
       [ SET ( FILETABLE_DIRECTORY = directory_name ) ]
    }
  
<stretch_configuration> ::=
    {
      SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options>)
          | = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED )
          | ( <table_stretch_options> [, ...n] )
        }
            )
    }
  
<table_stretch_options> ::=
    {
     [ FILTER_PREDICATE = { null | table_predicate_function } , ]
       MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }
    }
  
<single_partition_rebuild__option> ::=
{
      SORT_IN_TEMPDB = { ON | OFF }
    | MAXDOP = max_degree_of_parallelism
    | DATA_COMPRESSION = { NONE | ROW | PAGE | COLUMNSTORE | COLUMNSTORE_ARCHIVE} }
    | ONLINE = { ON [( <low_priority_lock_wait> ) ] | OFF }
}
  
<low_priority_lock_wait>::=
{
    WAIT_AT_LOW_PRIORITY ( MAX_DURATION = <time> [ MINUTES ],
        ABORT_AFTER_WAIT = { NONE | SELF | BLOCKERS } )
}
```

> [!NOTE]
> Para información adicional, consulte:
>
> - [ALTER TABLE column_constraint](alter-table-column-constraint-transact-sql.md)
> - [ALTER TABLE column_definition](alter-table-column-definition-transact-sql.md)
> - [ALTER TABLE computed_column_definition](alter-table-computed-column-definition-transact-sql.md)
> - [ALTER TABLE index_option](alter-table-index-option-transact-sql.md)
> - [ALTER TABLE table_constraints](alter-table-table-constraint-transact-sql.md)

## <a name="syntax-for-memory-optimized-tables"></a>Sintaxis para las tablas optimizadas para memoria

```syntaxsql
ALTER TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
{
    ALTER COLUMN column_name
    {
        [ type_schema_name. ] type_name
            [ (
                {
                   precision [ , scale ]
                }
            ) ]
        [ COLLATE collation_name ]
        [ NULL | NOT NULL ]
    }

    | ALTER INDEX index_name
    {
        [ type_schema_name. ] type_name
        REBUILD
        [ [ NONCLUSTERED ] WITH ( BUCKET_COUNT = bucket_count )
        ]
    }

    | ADD
    {
        <column_definition>
      | <computed_column_definition>
      | <table_constraint>
      | <table_index>
      | <column_index>
    } [ ,...n ]
  
    | DROP
     [ {
         CONSTRAINT [ IF EXISTS ]
         {
              constraint_name
          } [ ,...n ]
        | INDEX [ IF EXISTS ]
      {
         index_name
       } [ ,...n ]
          | COLUMN [ IF EXISTS ]
          {
              column_name
          } [ ,...n ]
          | PERIOD FOR SYSTEM_TIME
     } [ ,...n ]
    | [ WITH { CHECK | NOCHECK } ] { CHECK | NOCHECK } CONSTRAINT
        { ALL | constraint_name [ ,...n ] }

    | { ENABLE | DISABLE } TRIGGER
        { ALL | trigger_name [ ,...n ] }
  
    | SWITCH [ [ PARTITION ] source_partition_number_expression ]
        TO target_table
        [ PARTITION target_partition_number_expression ]
        [ WITH ( <low_priority_lock_wait> ) ]
    
}
[ ; ]

-- ALTER TABLE options

< table_constraint > ::=
 [ CONSTRAINT constraint_name ]
{
   {PRIMARY KEY | UNIQUE }
     {
       NONCLUSTERED (column [ ASC | DESC ] [ ,... n ])
       | NONCLUSTERED HASH (column [ ,... n ] ) WITH ( BUCKET_COUNT = bucket_count )
                    }
    | FOREIGN KEY
        ( column [ ,...n ] )
        REFERENCES referenced_table_name [ ( ref_column [ ,...n ] ) ]
    | CHECK ( logical_expression )
}

<column_index> ::=
  INDEX index_name
{ [ NONCLUSTERED ] | [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count)}

<table_index> ::=
  INDEX index_name
{[ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count)
  | [ NONCLUSTERED ] (column [ ASC | DESC ] [ ,... n ] )
      [ ON filegroup_name | default ]
  | CLUSTERED COLUMNSTORE [WITH ( COMPRESSION_DELAY = {0 | delay [Minutes]})]
      [ ON filegroup_name | default ]
}

```

## <a name="syntax-for-azure-synapse-analytics-and-parallel-data-warehouse"></a>Sintaxis para Azure Synapse Analytics y Almacenamiento de datos paralelos

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

ALTER TABLE { database_name.schema_name.source_table_name | schema_name.source_table_name | source_table_name }
{
    ALTER COLUMN column_name
        {
            type_name [ ( precision [ , scale ] ) ]
            [ COLLATE Windows_collation_name ]
            [ NULL | NOT NULL ]
        }
    | ADD { <column_definition> | <column_constraint> FOR column_name} [ ,...n ]
    | DROP { COLUMN column_name | [CONSTRAINT] constraint_name } [ ,...n ]
    | REBUILD {
            [ PARTITION = ALL [ WITH ( <rebuild_option> ) ] ]
          | [ PARTITION = partition_number [ WITH ( <single_partition_rebuild_option> ] ]
      }
    | { SPLIT | MERGE } RANGE (boundary_value)
    | SWITCH [ PARTITION source_partition_number
        TO target_table_name [ PARTITION target_partition_number ] [ WITH ( TRUNCATE_TARGET = ON | OFF )
}
[;]

<column_definition>::=
{
    column_name
    type_name [ ( precision [ , scale ] ) ]
    [ <column_constraint> ]
    [ COLLATE Windows_collation_name ]
    [ NULL | NOT NULL ]
}

<column_constraint>::=
    [ CONSTRAINT constraint_name ] 
    {
        DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE (column_name [ ,... n ]) NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }
<rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
        [ ON PARTITIONS ( {<partition_number> [ TO <partition_number>] } [ , ...n ] ) ]
}

<single_partition_rebuild_option > ::=
{
    DATA_COMPRESSION = { COLUMNSTORE | COLUMNSTORE_ARCHIVE }
}
```

[!INCLUDE[synapse-analytics-od-supported-tables](../../includes/synapse-analytics-od-supported-tables.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*database_name*  
El nombre de la base de datos en la que se creó la tabla.

*schema_name*  
El nombre del esquema al que pertenece la tabla.

*table_name*  
El nombre de la tabla que se va a modificar. Si la tabla no escla base de datos actual o no está contenida en el esquema propiedad del usuario actual, la base de datos y el esquema deben especificarse explícitamente.

ALTER COLUMN  
Especifica que la columna con nombre va a cambiarse o modificarse.

La columna modificada no puede ser:

- Una columna con un tipo de datos **timestamp**.
- La columna ROWGUIDCOL de la tabla.
- Una columna calculada o usada en una columna calculada.
- Se usa en las estadísticas generadas por la instrucción CREATE STATISTICS. Los usuarios deben ejecutar DROP STATISTICS para quitar las estadísticas a fin de que ALTER COLUMN pueda realizarse correctamente.  Ejecute esta consulta para obtener todas las estadísticas y las columnas de estadísticas creadas por el usuario para una tabla.

``` sql

SELECT s.name AS statistics_name  
      ,c.name AS column_name  
      ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<table_name>'); 

```
   > [!NOTE]
   > Las estadísticas generadas automáticamente por el optimizador de consultas se quitan automáticamente con ALTER COLUMN.

- Una columna utilizada en una restricción PRIMARY KEY o [FOREIGN KEY] REFERENCES.
- Una columna utilizada en una restricción CHECK o UNIQUE. Sin embargo, se permite el cambio de longitud de una columna de longitud variable en una restricción CHECK o UNIQUE.
- Una columna asociada a la definición predeterminada. No obstante, la longitud, precisión o escala de una columna puede cambiarse si el tipo de datos no se cambia.

El tipo de datos de las columnas **text**, **ntext** e **image** solo se puede cambiar de las formas siguientes:

- **text** por **varchar(max)** , **nvarchar(max)** o **xml**
- **ntext** por **varchar(max)** , **nvarchar(max)** o **xml**
- **image** por **varbinary(max)**

Algunos cambios del tipo de datos podrían suponer un cambio en los datos. Por ejemplo, cambiar una columna **nchar** o **nvarchar** por **char** o **varchar** puede provocar la conversión de caracteres extendidos. Para obtener más información, vea [CAST y CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md). Reducir la precisión o escala de una columna puede dar como resultado que se trunquen los datos.

> [!NOTE]
> El tipo de datos de una columna de una tabla con particiones no puede cambiarse.
>
> El tipo de datos de las columnas incluidas en un índice no puede cambiarse, a menos que la columna sea del tipo de datos **varchar**, **nvarchar** o **varbinary** y que el nuevo tamaño sea igual o mayor que el tamaño anterior.
>
> Las columnas incluidas en una restricción de clave principal no se pueden cambiar de **NOT NULL** a **NULL**.

Al utilizar Always Encrypted (sin enclaves seguros), si la columna que se está modificando se cifra mediante "ENCRYPTED WITH", puede cambiar el tipo de datos por un tipo de datos compatible (como INT por BIGINT), pero no se puede cambiar cualquier valor de cifrado.

Al usar Always Encrypted con enclaves seguros, puede cambiar cualquier configuración de cifrado, si la clave de cifrado de columna que protege la columna (y la nueva clave de cifrado de columna, si va a cambiar la clave) admita los cálculos de enclave (cifrados con claves maestras de columna habilitadas para el enclave). Para más información, vea [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

*column_name*  
El nombre de la columna que se va a modificar, agregar o quitar. La longitud máxima de *column_name* es 128 caracteres. Si se trata de columnas nuevas, puede omitir *column_name* para las columnas creadas con un tipo de datos **timestamp**. Si no se especifica el argumento *column_name* en una columna con un tipo de datos **timestamp**, se usa el nombre **timestamp**.

> [!NOTE]
> Se agregan nuevas columnas después de que se modifiquen todas las columnas existentes en la tabla.

[ _type\_schema\_name_ **.** ] _type\_name_  
El nuevo tipo de datos de la columna modificada o el tipo de datos de la columna agregada. No puede especificar *type_name* para columnas existentes de tablas con particiones. *type_name* puede ser cualquiera de los siguientes tipos:

- Un tipo de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Un tipo de datos del alias basado en el tipo de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los tipos de datos de alias se crean con la instrucción CREATE TYPE antes de que puedan utilizarse en una definición de tabla.
- Un tipo definido por el usuario de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] y el esquema al que pertenece. Los tipos definidos por el usuario se crean con la instrucción CREATE TYPE antes de que puedan utilizarse en una definición de tabla.

A continuación se indican los criterios de *type_name* en una columna modificada:

- El tipo de datos anterior debe poderse convertir implícitamente al nuevo tipo de datos.
- *type_name* no puede ser **timestamp**.
- Los valores predeterminados de ANSI_NULL están siempre activados para ALTER COLUMN; si no se especifican, la columna admite valores NULL.
- El relleno ANSI_PADDING está siempre activado para ALTER COLUMN.
- Si la columna alterada es una columna de identidad, *new_data_type* debe ser de un tipo de datos compatible con la propiedad de la identidad.
- La configuración actual de SET ARITHABORT se ignora. ALTER TABLE opera como si la opción ARITHABORT estuviera activada.

> [!NOTE]
> Si no se especifica la cláusula COLLATE, la modificación de un tipo de datos de columna tiene como resultado un cambio de intercalación a la intercalación predeterminada de la base de datos.

*precisión*  
La precisión del tipo de datos especificado. Para más información sobre los valores de precisión válidos, vea [Precisión, escala y longitud](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

*scale*  
La escala del tipo de datos especificado. Para más información sobre los valores de escala válidos, vea [Precisión, escala y longitud](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).

**max**  
Solo se aplica a los tipos de datos **varchar**, **nvarchar** y **varbinary** para el almacenamiento de 2^31-1 bytes de caracteres, datos binarios y datos Unicode.

*xml_schema_collection*  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Solo se aplica al tipo de datos **xml** para asociar un esquema XML con el tipo. Antes de escribir una columna **xml** en una colección de esquemas, cree primero la colección de esquema en la base de datos mediante el uso de [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).

COLLATE \< *collation_name* >  
Especifica la nueva intercalación para la columna alterada. Si no se especifica, se asigna a la columna la intercalación predeterminada de la base de datos. El nombre de intercalación puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Para obtener una lista y más información, vea [Windows Collation Name](../../t-sql/statements/windows-collation-name-transact-sql.md) [Nombre de intercalación de Windows (Transact-SQL)] y [SQL Server Collation Name](../../t-sql/statements/sql-server-collation-name-transact-sql.md) [Nombre de intercalación de SQL Server (Transact-SQL)].

La cláusula COLLATE cambia únicamente las intercalaciones de las columnas con tipos de datos **char**, **varchar**, **nchar** y **nvarchar**. Para cambiar la intercalación de una columna de tipo de datos de alias definido por el usuario, use distintas instrucciones ALTER TABLE para cambiar la columna a un tipo de datos del sistema [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. A continuación, cambie su intercalación y cambie la columna de nuevo a un tipo de datos de alias.

ALTER COLUMN no puede tener un cambio de intercalación si existe alguna de las siguientes condiciones:

- Si una restricción CHECK, una restricción FOREIGN KEY o las columnas calculadas hacen referencia a la columna cambiada.
- Si se ha creado algún índice, estadística o índice de texto completo en la columna. Las estadísticas creadas automáticamente en la columna cambiada se quitarán si se altera la intercalación de columna.
- Si una función o vista enlazada a esquema hace referencia a la columna.

Para obtener más información, vea [COLLATE](~/t-sql/statements/collations.md).

NULL | NOT NULL  
Especifica si la columna puede aceptar valores NULL. Las columnas que no permiten valores NULL solo se pueden agregar con ALTER TABLE si tienen especificado un valor predeterminado o si la tabla está vacía. Puede especificar NOT NULL para columnas calculadas solo si también especificó PERSISTED. Si la nueva columna permite valores NULL y no se especifica un valor predeterminado, la nueva columna contendrá un valor NULL en cada fila de la tabla. Si la nueva columna permite valores NULL y agrega una definición predeterminada con la nueva columna, se puede utilizar la opción WITH VALUES para almacenar el valor predeterminado en la nueva columna para cada fila existente en la tabla.

Si la nueva columna no permite valores NULL y la tabla no está vacía, debe agregar una definición DEFAULT con la nueva columna. Y la columna nueva se carga automáticamente con el valor predeterminado en cada fila existente de las columnas nuevas.

Puede especificar NULL en ALTER COLUMN para forzar que una columna NOT NULL permita valores NULL, excepto en el caso de las columnas de las restricciones PRIMARY KEY. Puede especificar NOT NULL en ALTER COLUMN solo si la columna no contiene valores NULL. Los valores NULL deben actualizarse a algún valor para poder permitir ALTER COLUMN NOT NULL, como:

```sql
UPDATE MyTable SET NullCol = N'some_value' WHERE NullCol IS NULL;
ALTER TABLE MyTable ALTER COLUMN NullCOl NVARCHAR(20) NOT NULL;
```

Cuando cree o altere una tabla con las instrucciones CREATE TABLE o ALTER TABLE, los valores de sesión y de la base de datos influirán en la nulabilidad, y posiblemente la invalidarán, para el tipo de datos utilizado en la definición de una columna. Asegúrese de definir siempre explícitamente una columna como NULL o como NOT NULL en el caso de las columnas no calculadas.

Si agrega una columna con un tipo de datos definidos por el usuario, asegúrese de definir la columna con la misma nulabilidad del tipo de datos definidos por el usuario. Y especifique un valor predeterminado para la columna. Para obtener más información, vea [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md).

> [!NOTE]
> Si se especifica NULL o NOT NULL con ALTER COLUMN, también se debe especificar *new_data_type* [(*precision* [, *scale* ])]. Si el tipo de datos, la precisión y la escala no se cambian, especifique los valores actuales de la columna.

[ {ADD | DROP} ROWGUIDCOL ]  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica que la propiedad ROWGUIDCOL se agrega a la columna indicada o se quita de ella. ROWGUIDCOL indica que la columna es una columna GUID de fila. Puede designar solo una columna **uniqueidentifier** por tabla como columna ROWGUIDCOL. Y solo puede asignar la propiedad ROWGUIDCOL a una columna **uniqueidentifier**. ROWGUIDCOL no puede asignarse a una columna de un tipo de datos definido por el usuario.

ROWGUIDCOL no exige la exclusividad de los valores almacenados en la columna y no genera automáticamente valores para las nuevas filas que se insertan en la tabla. Si desea generar valores únicos para cada columna, use la función NEWID o NEWSEQUENTIALID en instrucciones INSERT. O bien, especifique la función NEWID o NEWSEQUENTIALID como valor predeterminado para la columna.

[ {ADD | DROP} PERSISTED ]  
Especifica que la propiedad PERSISTED se agrega a la columna indicada o se quita de ella. La columna debe ser una columna calculada definida mediante una expresión determinista. Para las columnas especificadas como PERSISTED, [!INCLUDE[ssDE](../../includes/ssde-md.md)] almacena físicamente los valores calculados en la tabla y actualiza los valores cuando se actualiza cualquiera de las otras columnas de las que depende la columna calculada. Al marcar una columna calculada como PERSISTED, puede crear índices sobre columnas calculadas definidas sobre expresiones que son deterministas, pero no precisas. Para obtener más información, vea [Indexes on Computed Columns](../../relational-databases/indexes/indexes-on-computed-columns.md).

Las columnas calculadas que se utilizan como columna de partición de una tabla con particiones deben marcarse explícitamente como PERSISTED.

DROP NOT FOR REPLICATION  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica que los valores de columnas de identidad se incrementan cuando los agentes de replicación realizan operaciones de inserción. Solo puede especificar esta cláusula si *column_name* es una columna de identidad.

SPARSE  
Indica que la columna es una columna dispersa. El almacenamiento de columnas dispersas está optimizado para los valores NULL. No se puede establecer las columnas dispersas como NOT NULL. Al convertir una columna de dispersa a no dispersa o viceversa, se bloquea la tabla durante la ejecución del comando. Es posible que tenga que usar la cláusula REBUILD para recuperar espacio. Para conocer otras restricciones y leer más información sobre columnas dispersas, vea [Usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).

ADD MASKED WITH ( FUNCTION = ' *mask_function* ')  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica una máscara dinámica de datos. *mask_function* es el nombre de la función de máscara con los parámetros adecuados. Hay tres funciones disponibles:

- default()
- email()
- partial()
- random()

Para quitar una máscara, utilice `DROP MASKED`. Para conocer más parámetros de función, vea [Enmascaramiento de datos dinámicos](../../relational-databases/security/dynamic-data-masking.md).

WITH ( ONLINE = ON | OFF) \<as applies to altering a column>  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Permite realizar numerosas acciones de alteración de columna mientras la tabla sigue estando disponible. El valor predeterminado es OFF. La alteración de columna se puede realizar en línea para los cambios de columna relacionados con el tipo de datos, la precisión o la longitud de la columna, la nulabilidad, la escasez y la intercalación.

La alteración de columna en línea permite a las estadísticas automáticas y a las creadas por el usuario hacer referencia a la columna alterada durante la operación ALTER COLUMN, lo que requiere que las consultas se ejecuten del modo habitual. Al final de la operación, se quitan las estadísticas automáticas que hacen referencia a la columna y se invalidan las estadísticas creadas por el usuario. El usuario debe actualizar manualmente las estadísticas generadas por el usuario una vez completada la operación. Si la columna forma parte de una expresión de filtro para estadísticas o índices, no podrá realizar una operación para alterar la columna.

- Mientras se ejecuta la operación de alteración en línea de columna, todas las operaciones que podrían tomar una dependencia en la columna (índices, vistas, etc.) se bloquearán o devolverán el error correspondiente. Este comportamiento garantiza que no se produzcan problemas en la alteración de columna en línea debido a las dependencias introducidas durante la ejecución de la operación.
- No se admite la alteración de columnas NOT NULL a NULL como una operación en línea cuando los índices no agrupados en clúster hacen referencia a la columna alterada.
- No se admite la alteración en línea cuando una restricción CHECK hace referencia a la columna y la operación de alteración limita la precisión de esta (numéricos o fecha y hora).
- La opción `WAIT_AT_LOW_PRIORITY` no puede utilizarse con alteración de columna en línea.
- `ALTER COLUMN ... ADD/DROP PERSISTED` no se admite para la alteración de columna en línea.
- `ALTER COLUMN ... ADD/DROP ROWGUIDCOL/NOT FOR REPLICATION` no se ve afectado por la alteración de columna en línea.
- La alteración de columna en línea no admite la modificación de una tabla con el seguimiento de cambios habilitado que sea un publicador de replicación de mezcla.
- La alteración de columna en línea no admite la alteración desde tipos de datos CLR o a estos.
- La alteración de columna en línea no admite la alteración a un tipo de datos XML con una colección de esquemas diferente a la colección de esquemas actual.
- La alteración de columna en línea no reduce las restricciones sobre cuándo se puede modificar una columna. Las referencias de las estadísticas/índice, etc. podrían provocar el error de la alteración.
- La alteración de columna en línea no admite la modificación de más de una columna simultáneamente.
- La alteración de columna en línea no tiene ningún efecto en caso de tablas temporales con versiones del sistema. La columna ALTER no se ejecuta en línea, independientemente del valor que se especificó para la opción ONLINE.

La alteración de columna en línea tiene requisitos, restricciones y funciones similares a los de la regeneración de índice en línea, lo cual incluye lo siguiente:

- No se admite la regeneración de índices en línea cuando la tabla contiene columnas LOB o filestream heredadas, o cuando tiene un índice de almacén de columnas. Las mismas limitaciones se aplican para las alteraciones de columna en línea.
- Una columna existente que se va a modificar requiere dos veces la misma asignación de espacio; para la columna original y para la columna oculta recién creada.
- La estrategia de bloqueo durante una operación de alteración de columna en línea sigue el mismo patrón de bloqueo usado para la generación de índice en línea.

WITH CHECK | WITH NOCHECK  
Especifica si los datos de la tabla se han validado o no frente a una restricción FOREIGN KEY o CHECK recién agregada o habilitada de nuevo. Si no se especifica, se supone WITH CHECK para las restricciones nuevas y WITH NOCHECK para las restricciones que se han habilitado otra vez.

Si no desea volver a comprobar las restricciones CHECK o FOREIGN KEY nuevas con los datos existentes, utilice WITH NOCHECK. No se recomienda que haga esto, excepto en casos muy contados. La nueva restricción se evalúa en todas las actualizaciones futuras. Las infracciones de restricción que se supriman mediante WITH NOCHECK cuando se agrega la restricción pueden hacer que las actualizaciones futuras no se puedan llevar a cabo si actualizan filas con datos que no cumplan la restricción.

> [!NOTE]
> El optimizador de consultas no considera las restricciones definidas como WITH NOCHECK. Estas restricciones se pasan por alto hasta que se vuelven a habilitar mediante `ALTER TABLE table WITH CHECK CHECK CONSTRAINT ALL`.

ALTER INDEX *nombre_de_índice*  
Especifica que es necesario cambiar o modificar el número de cubos de *nombre_de_índice*.

La sintaxis ALTER TABLE ... ADD/DROP/ALTER INDEX solo se admite para tablas optimizadas para memoria.

> [!IMPORTANT]
> Si no se usa una instrucción ALTER TABLE, las instrucciones [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) y [PAD_INDEX](alter-table-index-option-transact-sql.md) no son compatibles con los índices de las tablas optimizadas para memoria.

ADD  
Especifica que se agregan una o más definiciones de columna, definiciones de columnas calculadas o restricciones de tabla. O bien, se agregan las columnas que el sistema usa para el control de versiones del sistema. Puede agregar un índice para las tablas optimizadas para memoria.

> [!NOTE]
> Se agregan nuevas columnas después de que se modifiquen todas las columnas existentes en la tabla.

> [!IMPORTANT]
> Si no se usa una instrucción ALTER TABLE, las instrucciones [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) y [PAD_INDEX](alter-table-index-option-transact-sql.md) no son compatibles con los índices de las tablas optimizadas para memoria.

PERIOD FOR SYSTEM_TIME ( system_start_time_column_name, system_end_time_column_name )  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica los nombres de las columnas que el sistema usa para registrar el período durante el que un registro es válido. Puede especificar las columnas existentes o crear nuevas columnas como parte del argumento ADD PERIOD FOR SYSTEM_TIME. Configure las columnas con el tipo de datos de datetime2 y defínalas como NOT NULL. Si una columna de período se define como NULL, se producirá un error. Puede definir [column_constraint](../../t-sql/statements/alter-table-column-constraint-transact-sql.md) o [especificar valores predeterminados para las columnas](../../relational-databases/tables/specify-default-values-for-columns.md) en las columnas system_start_time y system_end_time. Vea el ejemplo A en la sección [Control de versiones del sistema](#system_versioning), en que se muestra el uso de un valor predeterminado para la columna system_end_time.

Utilice este argumento con el argumento SET SYSTEM_VERSIONING para habilitar el control de versiones del sistema en una tabla existente. Para obtener más información, consulte [Tablas temporales](../../relational-databases/tables/temporal-tables.md) e [Introducción a las tablas temporales de Base de datos SQL de Azure](/azure/azure-sql/temporal-tables).

A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], los usuarios pueden marcar una o ambas columnas de período con la marca **HIDDEN** para ocultarlas implícitamente, de modo que **SELECT \* FROM \<table_name>** no devuelva ningún valor para esas columnas. De forma predeterminada, no se ocultan las columnas de período. Para poder usar las columnas ocultas, deben incluirse explícitamente en todas las consultas que hacen referencia directa a la tabla temporal.

DROP  
Especifica que se quitan una o más definiciones de columna, definiciones de columnas calculadas o restricciones de tabla, o que se quita la especificación para las columnas que el sistema utiliza para el control de versiones del sistema.

CONSTRAINT *constraint_name*  
Especifica que *constraint_name* se ha quitado de la tabla. Se pueden enumerar múltiples restricciones.

Puede determinar el nombre de la restricción, ya sea proporcionado por el sistema o definido por el usuario, mediante la consulta de las vistas de catálogo **sys.check_constraint**, **sys.default_constraints**, **sys.key_constraints** y **sys.foreign_keys**.

Una restricción PRIMARY KEY no puede quitarse si existe un índice XML en la tabla.

INDEX *nombre_de_índice*  
Especifica que *nombre_de_índice* se ha quitado de la tabla.

La sintaxis ALTER TABLE ... ADD/DROP/ALTER INDEX solo se admite para tablas optimizadas para memoria.

> [!IMPORTANT]
> Si no se usa una instrucción ALTER TABLE, las instrucciones [CREATE INDEX](create-index-transact-sql.md), [DROP INDEX](drop-index-transact-sql.md), [ALTER INDEX](alter-index-transact-sql.md) y [PAD_INDEX](alter-table-index-option-transact-sql.md) no son compatibles con los índices de las tablas optimizadas para memoria.

COLUMN *column_name*  
Especifica que *constraint_name* o *column_name* se quita de la tabla. Pueden especificarse varias columnas.

 Una columna no puede quitarse cuando:

- Se usa en un índice, ya sea como una columna de clave o como una cláusula INCLUDE
- Se utiliza en una restricción CHECK, FOREIGN KEY, UNIQUE o PRIMARY KEY.
- Está asociada con un valor predeterminado definido con la palabra clave DEFAULT o enlazada a un objeto predeterminado.
- Está enlazada a una regla.

> [!NOTE]
> Quitar una columna no recupera el espacio en disco de la columna. Tendrá que recuperar el espacio en disco de una columna quitada cuando el tamaño de fila de una tabla esté a punto de superar su límite o lo haya hecho ya. Recupere el espacio mediante la creación de un índice agrupado en la tabla o regenerando un índice agrupado existente mediante la utilización de [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md). Para obtener más información acerca del impacto de quitar los tipos de datos LOB, vea esta [entrada del blog de CSS](/archive/blogs/psssql/how-it-works-gotcha-varcharmax-caused-my-queries-to-be-slower).

PERIOD FOR SYSTEM_TIME  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Quita la especificación de las columnas que va a utilizar el sistema para el control de versiones del sistema.

WITH \<drop_clustered_constraint_option>  
Especifica que se han establecido una o más opciones para quitar restricciones en clúster.

MAXDOP = *max_degree_of_parallelism*  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Invalida la opción de configuración **grado máximo de paralelismo** solo durante la operación. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

Utilice la opción MAXDOP para limitar el número de procesadores utilizados en la ejecución de planes paralelos. El máximo es 64 procesadores.

*max_degree_of_parallelism* puede ser uno de los siguientes valores:

1  
Suprime la generación de planes paralelos.

\>1  
Restringe el número máximo de procesadores utilizados en una operación de índice paralelo para el número especificado.

0 (predeterminado)  
Usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.

Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md).

> [!NOTE]
> Las operaciones de índices en paralelo no están disponibles en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Ediciones y características admitidas de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) y [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

ONLINE **=** { ON | **OFF** } \<as applies to drop_clustered_constraint_option>  
Especifica si las tablas subyacentes y los índices asociados están disponibles para realizar consultas y modificar datos durante la operación de índice. El valor predeterminado es OFF. Puede ejecutar REBUILD como una operación ONLINE.

ACTIVAR  
Los bloqueos de tabla de larga duración no se mantienen durante la operación de indización. Durante la fase principal de la operación de índice, solo se mantiene un bloqueo preventivo en la tabla de origen. De esta forma, las consultas o actualizaciones realizadas sobre la tabla y los índices subyacentes pueden continuar. Al principio de la operación, se mantiene un bloqueo compartido (S) sobre el objeto de origen durante un breve espacio de tiempo. Al final de la operación, durante un breve período, se adquiere un bloqueo S (compartido) en el origen si se está creando un índice no agrupado. O bien, se adquiere un bloqueo SCH-M (modificación del esquema) cuando se crea o se pone en línea un índice agrupado y cuando se vuelve a compilar un índice agrupado o no. ONLINE no se puede establecer en ON cuando se crea un índice en una tabla temporal local. Solo se permite la operación de regeneración de montón de un único subproceso.

Para ejecutar DDL para **SWITCH** o para regenerar el índice en línea, todas las transacciones activas de bloqueo que se ejecutan en una tabla determinada deben completarse. Cuando se ejecuta, **SWITCH** o la operación de regeneración impide que la nueva transacción se inicie y podría afectar significativamente al rendimiento de la carga de trabajo y retrasar temporalmente el acceso a la tabla base.

Apagado  
Los bloqueos de tabla se aplican durante la operación de índice. Una operación de índice sin conexión para crear, volver a crear o quitar un índice clúster, o para volver a crear o quitar un índice no clúster, adquiere un bloqueo de modificación del esquema (Sch-M) de la tabla. Este bloqueo evita que todos los usuarios tengan acceso a la tabla subyacente durante la operación. Una operación de índice sin conexión que crea un índice no clúster adquiere un bloqueo compartido (S) en la tabla. Este bloqueo evita que se realicen actualizaciones en la tabla subyacente, pero permite la realización de operaciones de lectura, como instrucciones SELECT. Se permiten operaciones multiproceso de regeneración del montón.

Para más información, vea [Cómo funcionan las operaciones de índice en línea](../../relational-databases/indexes/how-online-index-operations-work.md).

> [!NOTE]
> Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Ediciones y características admitidas de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md) y [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md).

MOVE TO { _partition\_scheme\_name_ **(** _column\_name_ [ ,...*n* ] **)**  | *filegroup* |  **"** default **"** }  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica una ubicación a la que mover las filas de datos que se encuentran en el nivel hoja del índice clúster. La tabla se mueve a la nueva ubicación. Esta opción solo se aplica a las restricciones que crean un índice clúster.

> [!NOTE]
> En este contexto, el valor predeterminado no es una palabra clave. Es un identificador para el grupo de archivos predeterminado y debe delimitarse, como en MOVE TO **"** default **"** o MOVE TO **[** default **]** . Si se especifica **"** default **"** , la opción QUOTED_IDENTIFIER debe ser ON para la sesión actual. Esta es la configuración predeterminada. Para más información, consulte [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

{ CHECK | NOCHECK } CONSTRAINT  
Especifica si *constraint_name* está habilitado o deshabilitado. Esta opción solo se puede utilizar con las restricciones FOREIGN KEY y CHECK. Cuando se especifica NOCHECK, la restricción se deshabilita y las posteriores inserciones o actualizaciones de la columna no se validan con las condiciones de la restricción. Las restricciones DEFAULT, PRIMARY KEY y UNIQUE no se pueden deshabilitar.

ALL  
Especifica que todas las restricciones están deshabilitadas con la opción NOCHECK o habilitadas con la opción CHECK.

{ ENABLE | DISABLE } TRIGGER  
Especifica si *trigger_name* está habilitado o deshabilitado. Cuando se deshabilita un desencadenador, sigue definido para la tabla. Sin embargo, cuando las instrucciones INSERT, UPDATE o DELETE se ejecutan en la tabla, las acciones del desencadenador no se llevan a cabo hasta que se vuelva a habilitar.

ALL  
Especifica si todos los desencadenadores de la tabla están habilitados o deshabilitados.

*trigger_name*  
Especifica el nombre del desencadenador que se va a habilitar o deshabilitar.

{ ENABLE | DISABLE } CHANGE_TRACKING  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica si el seguimiento de cambios está habilitado o deshabilitado para la tabla. El seguimiento de cambios está deshabilitado de manera predeterminada.

Esta opción solo está disponible cuando el seguimiento de cambios está habilitado para la base de datos. Para obtener más información, vea [Opciones de ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md).

Para habilitar el seguimiento de cambios, la tabla debe tener una clave principal.

WITH **(** TRACK_COLUMNS_UPDATED **=** { ON | **OFF** } **)**  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica si [!INCLUDE[ssDE](../../includes/ssde-md.md)] realiza el seguimiento de las columnas sometidas a seguimiento de cambios que se actualizaron. El valor predeterminado es OFF.

SWITCH [ PARTITION *source_partition_number_expression* ] TO [ _schema\_name_ **.** ] *target_table* [ PARTITION *target_partition_number_expression* ]  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Modifica un bloqueo de datos de una de las formas siguientes:

- Vuelve a asignar todos los datos de una tabla como una partición en una tabla con particiones ya existente.
- Modifica una partición de una tabla con particiones a otra.
- Vuelve a asignar todos los datos de una partición de una tabla con particiones a una tabla sin particiones ya existente.

Si *table* es una tabla con particiones, debe especificar *source_partition_number_expression*. Si *target_table* es una tabla con particiones, debe especificar *target_partition_number_expression*. Si se vuelven a asignar los datos de una tabla como partición a una tabla con particiones ya existente, o se modifica una partición de una tabla con particiones a otra, la partición de destino debe existir y debe estar vacía.

Si se vuelven a asignar los datos de una partición para formar una sola tabla, la tabla de destino debe ya estar creada y vacía. Tanto la tabla o partición de origen como la tabla o partición de destino deben residir en el mismo grupo de archivos. Los índices correspondientes, o particiones de índice, también deben residir en el mismo grupo de archivos. Son muchas las restricciones adicionales que se aplican a las particiones que se modifican. *table* y *target_table* no pueden ser iguales. *target_table* puede ser un identificador de varias partes.

*source_partition_number_expression* y *target_partition_number_expression* son expresiones constantes que pueden hacer referencia a funciones y variables. Incluyen las variables de tipos definidos por el usuario y las funciones definidas por el usuario. No pueden hacer referencia a expresiones de [!INCLUDE[tsql](../../includes/tsql-md.md)].

Una tabla con particiones con un índice de almacén de columnas agrupado se comporta como un montón con particiones:

- La clave principal debe incluir la clave de partición.
- Un índice único debe incluir la clave de partición. Sin embargo, incluir la clave de partición con un índice único existente puede cambiar la unicidad.
- Para cambiar de partición, todos los índices no agrupados deben incluir la clave de partición.

Para **CAMBIAR** de restricción al utilizar la replicación, vea [Replicar tablas e índices con particiones](../../relational-databases/replication/publish/replicate-partitioned-tables-and-indexes.md).

Los índices de almacén de columnas no agrupados compilados para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016 CTP1 y para las versiones de SQL Database anteriores a V12 estaban en un formato de solo lectura. Debe volver a compilar los índices de almacén de columnas no agrupados en el formato actual (que se puede actualizar) para poder ejecutar cualquier operación de partición.

SET **(** FILESTREAM_ON = { *partition_scheme_name* \| *filestream_filegroup_name* \| **"** default **"** \| **"** NULL **"** } **)**  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no es compatible con `FILESTREAM`.

Especifica dónde se almacenan los datos FILESTREAM.

La ejecución de ALTER TABLE con la cláusula SET FILESTREAM_ON es correcta únicamente si la tabla no tiene columnas FILESTREAM. Puede agregar columnas FILESTREAM utilizando una segunda instrucción ALTER TABLE.

Si especifica *partition_scheme_name*, se aplican las reglas para [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md). Asegúrese de que la tabla tenga ya particiones para los datos de la fila y de que su esquema de partición utilice la misma función de partición y columnas que el esquema de partición de FILESTREAM.

*filestream_filegroup_name* especifica el nombre de un grupo de archivos FILESTREAM. El grupo de archivos debe tener un archivo que esté definido para el grupo de archivos, utilizando para ello las instrucciones [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md) o [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md); de lo contrario, se producirá un error.

**"** default **"** especifica el grupo de archivos FILESTREAM con la propiedad DEFAULT establecida. Si no hay ningún grupo de archivos FILESTREAM, se produce un error.

**"** NULL **"** especifica que se quitarán todas las referencias a los grupos de archivos FILESTREAM para la tabla. Se deben quitar primero todas las columnas FILESTREAM. Use SET FILESTREAM_ON = "**NULL**" para eliminar todos los datos FILESTREAM que estén asociados a una tabla.

SET **(** SYSTEM_VERSIONING **=** { OFF | ON [ ( HISTORY_TABLE = schema_name . history_table_name [ , DATA_CONSISTENCY_CHECK = { **ON** | OFF } ] ) ] } **)**  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Habilita o deshabilita el control de versiones del sistema de una tabla. Para habilitar el control de versiones del sistema de una tabla, el sistema comprueba que se cumplan los requisitos de restricción del tipo de datos, de la restricción de nulabilidad y de la clave principal para el control de versiones del sistema. Si no utiliza el argumento HISTORY_TABLE, el sistema genera una nueva tabla de historial que coincide con el esquema de la tabla actual, crea un vínculo entre las dos tablas y permite que el sistema registre el historial de cada registro de la tabla actual en la tabla de historial. El nombre de esta tabla de historial será `MSSQL_TemporalHistoryFor<primary_table_object_id>`. Si usa el argumento HISTORY_TABLE para crear un vínculo a un historial existente y usarlo, el sistema crea un vínculo entre la tabla actual y la tabla especificada. Al crear un vínculo a una tabla de historial existente, puede realizar una comprobación de coherencia de datos. Esta comprobación de coherencia de datos garantiza que los registros existentes no se superponen. La comprobación de coherencia de datos se ejecuta de manera predeterminada. Para obtener más información, consulte [Temporal Tables](../../relational-databases/tables/temporal-tables.md).

HISTORY_RETENTION_PERIOD = { **INFINITE** \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS} }  
**Se aplica a**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Especifica la retención finita o infinita de los datos del historial en la tabla temporal. Si se omite, se presupone la retención infinita.

SET (DATA_DELETION =  
      { OFF | ON  
                [(  [ FILTER_COLUMN = column_name ]   
                    [, RETENTION_PERIOD = { INFINITE | number {DAY | DAYS | WEEK | WEEKS | MONTH | MONTHS | YEAR | YEARS }}]   
                )] }   
**Se aplica a:** *solo* para Azure SQL Edge

Habilita la limpieza basada en la directiva de retención de los datos obsoletos o antiguos de las tablas de una base de datos. Para obtener más información, vea [Habilitación y deshabilitación de la retención de datos](/azure/azure-sql-edge/data-retention-enable-disable). Para habilitar la retención de datos se deben especificar los parámetros siguientes. 

- FILTER_COLUMN = { nombre_columna }  
Especifica la columna que se debe usar para determinar si las filas de la tabla son obsoletas o no. Se permiten los tipos de datos siguientes para la columna de filtro.
  - Date
  - DateTime
  - DateTime2
  - SmallDateTime
  - DateTimeOffset

- RETENTION_PERIOD = { INFINITE \| number {DAY \| DAYS \| WEEK \| WEEKS \| MONTH \| MONTHS \| YEAR \| YEARS }}       
Especifica la directiva del período de retención para la tabla. El período de retención se especifica como una combinación de un valor entero positivo y la unidad de la parte de la fecha. 

SET **(** LOCK_ESCALATION = { AUTO \| TABLE \| DISABLE } **)**  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica los métodos permitidos de extensión de bloqueo para una tabla.

AUTO  
Esta opción permite a [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] seleccionar la granularidad de la extensión de bloqueo que sea adecuada para el esquema de tabla.

- Si la tabla tiene particiones, se permitirá la extensión de bloqueo en la granularidad de árbol b (HoBT) o montón. En otras palabras, se permitirá la escalación al nivel de partición. Una vez realizada la extensión del bloqueo hasta el nivel de HoBT, el bloqueo no se extenderá a la granularidad de TABLE más adelante.
- Si la tabla no tiene particiones, la extensión del bloqueo se aplica a la granularidad de TABLE.

TABLE  
La extensión de bloqueo se aplica a la granularidad en el nivel de tabla, independientemente de que la tabla tenga o no particiones. TABLE es el valor predeterminado.

DISABLE  
Evita la extensión de bloqueo en la mayoría de los casos. No siempre se evitan los bloqueos de nivel de la tabla. Por ejemplo, si está examinando una tabla que no tiene ningún índice agrupado en el nivel de aislamiento serializable, [!INCLUDE[ssDE](../../includes/ssde-md.md)] debe realizar un bloqueo de la tabla para proteger la integridad de los datos.

REBUILD  
Utilice la sintaxis de REBUILD WITH para volver a generar una tabla completa que incluya todas las particiones en una tabla con particiones. Si la tabla tiene un índice clúster, la opción REBUILD vuelve a generarlo. REBUILD se puede ejecutar como una operación ONLINE.

Utilice la sintaxis de REBUILD PARTITION para volver a generar una partición única en una tabla con particiones.

PARTITION = ALL  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Vuelve a generar todas las particiones al cambiar los valores de compresión de la partición.

REBUILD WITH ( \<rebuild_option> )  
Todas las opciones se aplican a una tabla con un índice clúster. Si la tabla no tiene un índice agrupado, solo algunas de las opciones afectan a la estructura del montón.

Cuando no se especifica un valor de compresión específico con la operación REBUILD, se usa el valor de compresión actual de la partición. Para devolver el valor actual, realice una consulta en la columna **data_compression** de la vista del catálogo **sys.partitions**.

Para obtener una descripción completa de las opciones de recompilación, vea [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md).

DATA_COMPRESSION  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica la opción de compresión de datos para la tabla, el número de partición o el intervalo de particiones especificados. Las opciones son las siguientes:

NONE No se comprimen la tabla ni las particiones especificadas. Esta opción no se aplica a las tablas del almacén de columnas.

ROW La tabla o las particiones especificadas se comprimen utilizando la compresión de fila. Esta opción no se aplica a las tablas del almacén de columnas.

PAGE La tabla o las particiones especificadas se comprimen utilizando la compresión de página. Esta opción no se aplica a las tablas del almacén de columnas.

COLUMNSTORE  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Solo se aplica a tablas de almacén de columnas. COLUMNSTORE especifica que se descomprima una partición que se comprimió con la opción COLUMNSTORE_ARCHIVE. Cuando se restauran los datos, siguen estando comprimidos con la compresión de almacén de columnas que se usa para todas las tablas de almacén de columnas.

COLUMNSTORE_ARCHIVE  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Solo se aplica a las tablas de almacén de columnas almacenadas con un índice clúster de almacén de columnas. COLUMNSTORE_ARCHIVE comprimirá aún más la partición especificada a un tamaño mínimo. Use esta opción para el archivado o para otras situaciones que requieran menos almacenamiento y en las que pueda permitirse más tiempo para el almacenamiento y recuperación.

Para volver a generar al mismo tiempo varias particiones, vea [index_option](../../t-sql/statements/alter-table-index-option-transact-sql.md). Si la tabla no tiene un índice agrupado, al cambiar la compresión de datos se vuelven a generar el montón y los índices no agrupados. Para más información sobre la compresión, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).

ONLINE **=** { ON | **OFF** } \<as applies to single_partition_rebuild_option>  
Especifica si una única partición de las tablas subyacentes y los índices asociados están disponibles para realizar consultas y modificar datos durante la operación de indización. El valor predeterminado es OFF. Puede ejecutar REBUILD como una operación ONLINE.

ACTIVAR  
Los bloqueos de tabla de larga duración no se mantienen durante la operación de indización. Requiere un bloqueo S en la tabla al principio de la recompilación del índice y un bloqueo Sch-M en la tabla en el extremo de la recompilación de índice en línea. Aunque ambos bloqueos son bloqueos de metadatos cortos, el bloqueo Sch-M debe esperar a que todas las transacciones de bloqueo se completen. Durante el tiempo de espera, el bloqueo Sch-M bloquea las demás transacciones que esperen a este bloqueo al tener acceso a la misma tabla.

> [!NOTE]
> La regeneración de índice en línea puede establecer las opciones *low_priority_lock_wait* que se describen más adelante en esta sección.

Apagado  
Los bloqueos de tabla se aplican durante la operación de índice. Esto evita que todos los usuarios tengan acceso a la tabla subyacente durante la operación.

*column_set_name* XML COLUMN_SET FOR ALL_SPARSE_COLUMNS  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

El nombre del conjunto de columnas. Un conjunto de columnas es una representación XML sin tipo que combina todas las columnas dispersas de una tabla en una salida estructurada. No se puede agregar un conjunto de columnas a una tabla que contenga columnas dispersas. Para obtener más información sobre los conjuntos de columnas, vea [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md).

{ ENABLE | DISABLE } FILETABLE_NAMESPACE  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores)

Habilita o deshabilita las restricciones definidas por el sistema en un objeto FileTable. Solo se puede utilizar con un objeto FileTable.

SET ( FILETABLE_DIRECTORY = *directory_name* )  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores) [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no es compatible con `FILETABLE`.

Especifica el nombre de directorio de FileTable compatible con Windows. Este nombre debe ser único entre todos los nombres de directorio de FileTable en la base de datos. La comparación de unicidad no distingue mayúsculas de minúsculas, a pesar de la configuración de intercalación de SQL. Solo se puede utilizar con un objeto FileTable.

```syntaxsql
 SET (
        REMOTE_DATA_ARCHIVE
        {
            = ON (<table_stretch_options> )
          | = OFF_WITHOUT_DATA_RECOVERY
          ( MIGRATION_STATE = PAUSED ) | ( <table_stretch_options> [, ...n] )
        } )
```

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y versiones posteriores)

Habilita o deshabilita Stretch Database para una tabla. Para obtener más información, vea [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

**Habilitar Stretch Database para una tabla**

Al habilitar Stretch para una tabla especificando `ON`, también tiene que especificar `MIGRATION_STATE = OUTBOUND` para empezar a migrar los datos inmediatamente o `MIGRATION_STATE = PAUSED` para posponer la migración de datos. El valor predeterminado es `MIGRATION_STATE = OUTBOUND`. Para más información sobre la habilitación de Stretch para una tabla, vea [Enable Stretch Database for a table](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) (Habilitar Stretch Database para una tabla).

**Requisitos previos**. Para poder habilitar Stretch para una tabla, primero tiene que habilitar Stretch en el servidor y en la base de datos. Para obtener más información, vea [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md) (Habilitar Stretch Database para una tabla).

**Permisos**. Para habilitar Stretch para una base de datos o una tabla se necesitan permisos db_owner. Además, para habilitar Stretch para una tabla se necesitan permisos ALTER en la tabla.

**Deshabilitar Stretch Database para una tabla**

Al deshabilitar Stretch para una tabla, tiene dos opciones para los datos remotos que ya se han migrado a Azure. Para obtener más información, vea [Deshabilitar Stretch Database y recuperar datos remotos](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

- Si quiere deshabilitar Stretch para una tabla y copiar los datos remotos de ella de Azure a SQL Server, ejecute el siguiente comando. Este comando no se puede cancelar.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;
    ```

Esta operación provoca costos de transferencia de datos y no se puede cancelar. Para obtener más información, consulte [Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/).

Una vez que todos los datos remotos se han copiado desde Azure en SQL Server, se deshabilita Stretch para la tabla.

- Para deshabilitar Stretch para una tabla y abandonar los datos remotos, ejecute el siguiente comando.

    ```sql
    ALTER TABLE <table_name>
       SET ( REMOTE_DATA_ARCHIVE = OFF_WITHOUT_DATA_RECOVERY ( MIGRATION_STATE = PAUSED ) ) ;
    ```

Después de deshabilitar Stretch Database para una tabla, se detiene la migración de datos y los resultados de la consulta dejan de incluir los resultados de la tabla remota.

Al deshabilitar Stretch no se quita la tabla remota. Si quiere eliminar la tabla remota, tiene que quitarla mediante Azure Portal.

[ FILTER_PREDICATE = { null | *predicate* } ]  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y versiones posteriores)

Especifica opcionalmente un predicado de filtro para seleccionar las filas que se migrarán desde una tabla que contiene datos históricos y datos actuales. El predicado debe llamar a una función determinista con valores de tabla insertada. Para obtener más información, consulte [Enable Stretch Database para una tabla](../../sql-server/stretch-database/enable-stretch-database-for-a-table.md) (Habilitar Stretch Database para una tabla) y [Seleccionar las filas que se van a migrar mediante una función de filtro (Stretch Database)](../../sql-server/stretch-database/select-rows-to-migrate-by-using-a-filter-function-stretch-database.md).

> [!IMPORTANT]
> Si se indica un predicado de filtro que tiene un rendimiento bajo, la migración de datos también tendrá un rendimiento bajo. Stretch Database aplica el predicado de filtro a la tabla mediante el operador CROSS APPLY.

Si no se especifica un predicado de filtro, se migrará toda la tabla.

Cuando se especifica un predicado de filtro, también hay que especificar *MIGRATION_STATE*.

MIGRATION_STATE = { OUTBOUND | INBOUND | PAUSED }  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y versiones posteriores)

- Especifique `OUTBOUND` para migrar datos de SQL Server a Azure.
- Especifique `INBOUND` para copiar los datos remotos de la tabla de Azure a SQL Server y deshabilite Stretch para la tabla. Para obtener más información, vea [Deshabilitar Stretch Database y recuperar datos remotos](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

    Esta operación provoca costos de transferencia de datos y no se puede cancelar.

- Especifique `PAUSED` para pausar o posponer la migración de datos. Para obtener más información, vea [Pausa y reanudación de la migración de datos (Stretch Database)](../../sql-server/stretch-database/pause-and-resume-data-migration-stretch-database.md).

WAIT_AT_LOW_PRIORITY  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Una recompilación de índices en línea tiene que esperar a las operaciones de bloqueo en esta tabla. **WAIT_AT_LOW_PRIORITY** indica que la operación de regeneración de índice en línea espera a los bloqueos de prioridad baja, de forma que otras operaciones pueden continuar mientras la operación de generación de índice en línea está en espera. Omitir la opción **WAIT AT LOW PRIORITY** equivale a `WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 0 minutes, ABORT_AFTER_WAIT = NONE)`.

MAX_DURATION = *time* [**MINUTES** ]  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

El tiempo de espera (valor entero especificado en minutos) que **SWITCH** o los bloqueos de la operación de recompilación de índices en línea esperan con prioridad baja al ejecutar el comando DDL. Si la operación se bloquea durante el tiempo de **MAX_DURATION**, se ejecutará una de las acciones **ABORT_AFTER_WAIT**. El tiempo de **MAX_DURATION** siempre es en minutos y la palabra **MINUTES** puede omitirse.

ABORT_AFTER_WAIT = [**NONE** | **SELF** | **BLOCKERS** } ]  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Ninguno  
Se continúa esperando al bloqueo con prioridad normal.

SELF  
Sale de la operación DDL **SWITCH** o regeneración de índices en línea que se está ejecutando actualmente sin realizar ninguna acción.

BLOCKERS  
Elimina todas las transacciones de usuario que están bloqueando la operación DDL **SWITCH** o la regeneración de índices en línea de forma que la operación pueda continuar.

Requiere el permiso **ALTER ANY CONNECTION**.

IF EXISTS  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Quita condicionalmente la columna o restricción solo si ya existe.

## <a name="remarks"></a>Observaciones

Para agregar nuevas filas de datos, utilice [INSERT](../../t-sql/statements/insert-transact-sql.md). Para quitar filas de datos, utilice [DELETE](../../t-sql/statements/delete-transact-sql.md) o [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md). Para cambiar los valores de las filas existentes, utilice [UPDATE](../../t-sql/queries/update-transact-sql.md).

Si hubiera algún plan de ejecución en la memoria caché del procedimiento que hace referencia a la tabla, ALTER TABLE los marca para que se vuelvan a compilar en la siguiente ejecución.

## <a name="changing-the-size-of-a-column"></a>Cambiar el tamaño de una columna

Puede cambiar la longitud, precisión o escala de una columna especificando un nuevo tamaño para el tipo de datos de columna. Utilice la cláusula ALTER COLUMN. Si hay datos en la columna, el nuevo tamaño no puede ser menor que el tamaño máximo de los datos. Asimismo, no puede definir la columna en un índice, a menos que sea de un tipo de datos **varchar**, **nvarchar** o **varbinary**, y el índice no sea el resultado de una restricción KEY PRIMARY. Vea el ejemplo en la breve sección titulada [Modificación de una definición de columna](#alter_column).

## <a name="locks-and-alter-table"></a>Bloqueos y ALTER TABLE

Los cambios que especifique en ALTER TABLE se implementan inmediatamente. Si los cambios requieren modificaciones de las filas de la tabla, ALTER TABLE actualiza las filas. ALTER TABLE adquiere un bloqueo de modificación del esquema (SCH-M) sobre la tabla para asegurar que ninguna otra conexión haga referencia ni a los metadatos de la tabla durante el cambio, excepto las operaciones de indización en línea que precisen un breve bloqueo SCH-M al final. En una operación `ALTER TABLE...SWITCH`, el bloqueo se adquiere tanto en las tablas de origen como en las de destino. Las modificaciones realizadas en la tabla se registran y son completamente recuperables. Los cambios que afectan a todas las filas de tablas grandes, como quitar una columna o, en algunas ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], agregar una columna NOT NULL con un valor predeterminado, pueden tardar mucho tiempo en completarse y generan muchos registros. Ejecute estas instrucciones ALTER TABLE con el mismo cuidado que cualquier instrucción INSERT, UPDATE o DELETE que afecte a un gran número de filas.

### <a name="adding-not-null-columns-as-an-online-operation"></a>Agregar columnas NOT NULL como una operación en línea

A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Enterprise Edition, agregar una columna NOT NULL con un valor predeterminado es una operación en línea cuando el valor predeterminado es una *constante de tiempo de ejecución*. Esto significa que la operación se ha completado de forma casi instantánea, a pesar del número de filas de la tabla. Esto se debe a que las filas existentes en la tabla no se actualizan durante la operación. En su lugar, el valor predeterminado se almacena únicamente en los metadatos de la tabla y el valor se busca según sea necesario en las consultas que tienen acceso a estas filas. Este comportamiento es automático. No se requiere sintaxis adicional alguna para implementar la operación en línea aparte de la sintaxis de COLUMN ADD. Una constante de tiempo de ejecución es una expresión que genera el mismo valor en tiempo de ejecución para cada fila de la tabla, a pesar de su determinismo. Por ejemplo, la expresión constante “mis datos temporales” o la función del sistema GETUTCDATETIME() son constantes de tiempo de ejecución. En cambio, las funciones `NEWID()` o `NEWSEQUENTIALID()` no son constantes de tiempo de ejecución porque se genera un valor único para cada fila de la tabla. Cuando se agrega una columna NOT NULL con un valor predeterminado que no es una constante de tiempo de ejecución, se ejecuta siempre sin conexión y se adquiere un bloqueo exclusivo (SCH-M) mientras dura la operación.

Mientras que las filas existentes hacen referencia al valor almacenado en los metadatos, el valor predeterminado se almacena en la fila para aquellas filas nuevas que se inserten y que no especifiquen otro valor para la columna. El valor predeterminado que se almacena en los metadatos se traslada a una fila existente cuando se actualiza la fila (aunque la columna real no se especifique en la instrucción UPDATE) o bien, si la tabla o el índice agrupado se vuelve a generar.

Las columnas de tipo **varchar(max)** , **nvarchar(max)** , **varbinary(max)** , **xml**, **text**, **ntext**, **image**, **hierarchyid**, **geometry**, **geography** o CLR UDTS no se pueden agregar en una operación en línea. Una columna no se puede agregar en línea si al hacerlo provoca que el tamaño máximo posible de fila supere el límite de 8.060 bytes. En este caso, la columna se agrega como una operación sin conexión.

## <a name="parallel-plan-execution"></a>Ejecutar planes paralelos

En [!INCLUDE[ssEnterpriseEd11](../../includes/ssenterpriseed11-md.md)] y versiones posteriores, el número de procesadores utilizados para ejecutar una sola instrucción ALTER TABLE ADD (basada en índices) CONSTRAINT o DROP (índice agrupado) CONSTRAINT viene determinado por la opción de configuración **grado máximo de paralelismo** y la carga de trabajo actual. Si el [!INCLUDE[ssDE](../../includes/ssde-md.md)] detecta que el sistema está ocupado, el grado de paralelismo de la operación se reduce automáticamente antes de comenzar la ejecución de la instrucción. Puede configurar manualmente el número de procesadores que se utilizan para ejecutar la instrucción si especifica la opción MAXDOP. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

## <a name="partitioned-tables"></a>Tablas con particiones

Además de realizar operaciones SWITCH que implican a tablas con particiones, use ALTER TABLE para cambiar el estado de las columnas, restricciones y desencadenadores de una tabla con particiones, de la misma forma que se utiliza para las tablas sin particiones. Sin embargo, esta instrucción no puede utilizarse para cambiar la forma en que se realizan las particiones de la tabla misma. Para volver a realizar las particiones de una tabla con particiones, utilice [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md) y [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md). Además, no puede cambiar el tipo de datos de una columna de una tabla con particiones.

## <a name="restrictions-on-tables-with-schema-bound-views"></a>Realizar restricciones en tablas con vistas enlazadas a esquema

Las restricciones que se aplican a instrucciones ALTER TABLE en tablas con vistas enlazadas a esquema son las mismas que las restricciones que se aplican actualmente cuando se alteran tablas con un solo índice. Se permite agregar una columna. No obstante, no se permite quitar ni cambiar una columna que participa en una vista enlazada a esquema. Si la instrucción ALTER TABLE requiere la alteración de una columna que se utiliza en una vista enlazada a esquema, se produce un error en ALTER TABLE y el [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera un mensaje de error. Para obtener más información acerca de los enlaces a esquemas y vistas indizadas, vea [CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md).

La creación de una vista enlazada a esquema que hace referencia a las tablas no afecta a la adición ni a la eliminación de desencadenadores en las tablas base.

## <a name="indexes-and-alter-table"></a>Índices y ALTER TABLE

Los índices creados como parte de una restricción se quitan cuando se quita la restricción. Los índices que se crearon con CREATE INDEX deben quitarse con DROP INDEX. Use la instrucción ALTER INDEX para volver a crear un índice que es parte de una definición de restricción; no es necesario quitar o agregar de nuevo la restricción con ALTER TABLE.

Todos los índices y restricciones basados en una columna deben eliminarse para que se pueda quitar la columna.

Cuando elimina una restricción que ha creado un índice agrupado, las filas de datos que se han almacenado en el nivel hoja del índice agrupado se almacenan en una tabla no agrupada. Puede quitar el índice clúster y mover la tabla resultante a otro grupo de archivos o esquema de partición en una sola transacción especificando la opción MOVE TO. La opción MOVE TO tiene las siguientes restricciones:

- MOVE TO no es válido para vistas indizadas o índices no agrupados.
- El esquema de partición o el grupo de archivos debe existir previamente.
- Si no se especifica MOVE TO, la tabla se ubica en el mismo esquema de partición o grupo de archivos que se definió para el índice agrupado.

Cuando quite un índice agrupado, especifique la opción ONLINE **=** ON para que la transacción DROP INDEX no bloquee las consultas y modificaciones en los datos subyacentes y en los índices no agrupados asociados.

ONLINE **=** ON tiene las siguientes restricciones:

- ONLINE **=** ON no es válido para índices agrupados que también estén deshabilitados. Los índices deshabilitados deben quitarse con ONLINE **=** OFF.
- Solo un índice puede quitarse cada vez.
- ONLINE **=** ON no es válido para las vistas indizadas, índices no agrupados ni índices de tablas temporales locales.
- ONLINE **=** ON no es válido para los índices de almacén de columnas.

Para quitar un índice clúster, se necesita un espacio temporal en disco del mismo tamaño que el del índice clúster existente. Este espacio adicional se libera en cuanto se completa la operación.

> [!NOTE]
> Las opciones que aparecen en *\<drop_clustered_constraint_option>* se aplican a índices agrupados en tablas y no se pueden aplicar a índices agrupados en vistas o índices no agrupados.

## <a name="replicating-schema-changes"></a>Replicar cambios de esquema

De forma predeterminada, cuando se ejecuta ALTER TABLE en una tabla publicada en un publicador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el cambio se propaga a todos los suscriptores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta funcionalidad tiene algunas restricciones. La puede deshabilitar. Para más información, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).

## <a name="data-compression"></a>Data Compression

En las tablas del sistema no se puede habilitar la compresión. Si la tabla es un montón, la operación de regeneración en modo ONLINE será de un solo subproceso. Utilice el modo OFFLINE para una operación de regeneración de montón de varios subprocesos. Para obtener más información sobre la compresión de datos, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).

Para evaluar cómo afecta el cambio del estado de compresión a una tabla, índice o partición, use el procedimiento almacenado [sp_estimate_data_compression_savings](../../relational-databases/system-stored-procedures/sp-estimate-data-compression-savings-transact-sql.md) .

Las restricciones siguientes se aplican a las tablas con particiones:

- No se puede cambiar la configuración de compresión de una partición única si la tabla tiene índices no alineados.
- La sintaxis ALTER TABLE \<table> REBUILD PARTITION ... recompila la partición especificada.
- La sintaxis ALTER TABLE \<table> REBUILD WITH ... recompila todas las particiones.

## <a name="dropping-ntext-columns"></a>Quitar columnas NTEXT

Al quitar las columnas NTEXT, la limpieza de los datos eliminados se produce como una operación serializada en todas las filas. La limpieza puede requerir una gran cantidad de tiempo. Al quitar una columna NTEXT de una tabla con un gran número de filas, actualice la columna NTEXT con el valor NULL en primer lugar y luego quite la columna. Puede ejecutar esta opción con operaciones en paralelo para que sea mucho más rápida.

## <a name="online-index-rebuild"></a>Volver a generar índice en línea

Para ejecutar la instrucción DDL a fin de regenerar el índice en línea, todas las transacciones activas de bloqueo que se ejecutan en una tabla determinada deben completarse. Cuando la regeneración de índice en línea se inicia, bloquea todas las nuevas transacciones que están listas para iniciar la ejecución en esta tabla. Aunque la vigencia del bloqueo para volver a generar el índice en línea es muy corta, si se espera a que las transacciones abiertas en una tabla dada se completen y se bloquean las nuevas transacciones que se inician, se podría afectar significativamente al rendimiento. Esto puede provocar un retraso o un tiempo de espera en la carga de trabajo y limitar mucho el acceso a la tabla base. Con la opción **WAIT_AT_LOW_PRIORITY**, el DBA puede administrar los bloqueos S-lock y Sch-M necesarios para las regeneraciones de índice en línea y seleccionar una de las tres opciones. En los tres casos, si durante el tiempo de espera (`(MAX_DURATION =n [minutes])`) no hay actividades de bloqueo, la regeneración de índice en línea se ejecuta inmediatamente sin esperar y la instrucción DDL se completa.

## <a name="compatibility-support"></a>Soporte de compatibilidad

La instrucción ALTER TABLE solo permite nombres de tabla de dos partes (esquema.objeto). En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], la especificación de un nombre de tabla con uno de los siguientes formatos produce el error 117 en tiempo de compilación.

- servidor.baseDeDatos.esquema.tabla
- .baseDeDatos.esquema.tabla
- ..esquema.tabla

En versiones anteriores, al especificar el formato servidor.baseDeDatos.esquema.tabla se devolvía el error 4902. La especificación del formato .baseDeDatos.esquema.tabla o ..esquema.tabla se realizaba correctamente.

Para resolver el problema, quite el uso de un prefijo de cuatro partes.

## <a name="permissions"></a>Permisos

Requiere el permiso ALTER en la tabla.

Los permisos ALTER TABLE se aplican a las tablas relacionadas con una instrucción ALTER TABLE SWITCH. Los datos que se modifican heredan la seguridad de la tabla de destino.

Si ha definido alguna columna de la instrucción ALTER TABLE para que sea un tipo definido por el usuario de CLR (Common Language Runtime) o un tipo de datos del alias, se necesita el permiso REFERENCES sobre el tipo.

La adición o modificación de una columna que actualiza las filas de la tabla requiere el permiso **UPDATE** en ella. Por ejemplo, agregar una columna **NOT NULL** con un valor predeterminado o agregar una columna de identidad si la tabla no está vacía.

## <a name="examples"></a><a name="Example_Top"></a> Ejemplos

|Category|Elementos de sintaxis ofrecidos|
|--------------|------------------------------|
|[Agregar columnas y restricciones](#add)|ADD * PRIMARY KEY con opciones de índice * columnas dispersas y conjuntos de columnas *|
|[Quitar columnas y restricciones](#Drop)|DROP|
|[Modificar una definición de columna](#alter_column)|cambiar tipo de datos * cambiar tamaño de columna * intercalación|
|[Modificar una definición de tabla](#alter_table)|DATA_COMPRESSION * SWITCH PARTITION * LOCK ESCALATION * seguimiento de cambios|
|[Deshabilitar y habilitar restricciones y desencadenadores](#disable_enable)|CHECK * NO CHECK * ENABLE TRIGGER * DISABLE TRIGGER|
| &nbsp; | &nbsp; |

### <a name="adding-columns-and-constraints"></a><a name="add"></a>Agregar columnas y restricciones

En los ejemplos de esta sección se muestra cómo agregar columnas y restricciones a una tabla.

#### <a name="a-adding-a-new-column"></a>A. Agregar una columna nueva

En el ejemplo siguiente se agrega una columna que permite valores NULL y a la que no se han proporcionado valores mediante una definición DEFAULT. En la nueva columna, cada fila tendrá valores `NULL`.

```sql
CREATE TABLE dbo.doc_exa (column_a INT) ;
GO
ALTER TABLE dbo.doc_exa ADD column_b VARCHAR(20) NULL ;
GO
```

#### <a name="b-adding-a-column-with-a-constraint"></a>B. Agregar una columna con una restricción

En el ejemplo siguiente se agrega una nueva columna con una restricción `UNIQUE`.

```sql
CREATE TABLE dbo.doc_exc (column_a INT) ;
GO
ALTER TABLE dbo.doc_exc ADD column_b VARCHAR(20) NULL
    CONSTRAINT exb_unique UNIQUE ;
GO
EXEC sp_help doc_exc ;
GO
DROP TABLE dbo.doc_exc ;
GO
```

#### <a name="c-adding-an-unverified-check-constraint-to-an-existing-column"></a>C. Agregar una restricción CHECK no comprobada a una columna existente

En el ejemplo siguiente se agrega una restricción a una columna existente de la tabla. La columna tiene un valor que infringe la restricción. Por tanto, `WITH NOCHECK` se usa para evitar que la restricción se valide en las filas existentes y para poder agregar la restricción.

```sql
CREATE TABLE dbo.doc_exd (column_a INT) ;
GO
INSERT INTO dbo.doc_exd VALUES (-1) ;
GO
ALTER TABLE dbo.doc_exd WITH NOCHECK
ADD CONSTRAINT exd_check CHECK (column_a > 1) ;
GO
EXEC sp_help doc_exd ;
GO
DROP TABLE dbo.doc_exd ;
GO
```

#### <a name="d-adding-a-default-constraint-to-an-existing-column"></a>D. Agregar una restricción DEFAULT a una columna existente

En el ejemplo siguiente se crea una tabla con dos columnas y se inserta un valor en la primera columna, y la otra columna sigue siendo NULL. A continuación se agrega una restricción `DEFAULT` a la segunda columna. Para comprobar que se aplica el valor predeterminado, se inserta otro valor en la primera columna y se consulta la tabla.

```sql
CREATE TABLE dbo.doc_exz (column_a INT, column_b INT) ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (7) ;
GO
ALTER TABLE dbo.doc_exz
  ADD CONSTRAINT col_b_def
  DEFAULT 50 FOR column_b ;
GO
INSERT INTO dbo.doc_exz (column_a) VALUES (10) ;
GO
SELECT * FROM dbo.doc_exz ;
GO
DROP TABLE dbo.doc_exz ;
GO
```

#### <a name="e-adding-several-columns-with-constraints"></a>E. Agregar varias columnas con restricciones

En el ejemplo siguiente se agregan varias columnas con restricciones que se definen con la nueva columna. La primera columna nueva tiene una propiedad `IDENTITY`. Cada fila de la tabla tiene nuevos valores incrementales en la columna de identidad.

```sql
CREATE TABLE dbo.doc_exe (column_a INT CONSTRAINT column_a_un UNIQUE) ;
GO
ALTER TABLE dbo.doc_exe ADD

-- Add a PRIMARY KEY identity column.
column_b INT IDENTITY
CONSTRAINT column_b_pk PRIMARY KEY,

-- Add a column that references another column in the same table.
column_c INT NULL
CONSTRAINT column_c_fk
REFERENCES doc_exe(column_a),

-- Add a column with a constraint to enforce that
-- nonnull data is in a valid telephone number format.
column_d VARCHAR(16) NULL
CONSTRAINT column_d_chk
CHECK
(column_d LIKE '[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]' OR
column_d LIKE
'([0-9][0-9][0-9]) [0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]'),

-- Add a nonnull column with a default.
column_e DECIMAL(3,3)
CONSTRAINT column_e_default
DEFAULT .081 ;
GO
EXEC sp_help doc_exe ;
GO
DROP TABLE dbo.doc_exe ;
GO
```

#### <a name="f-adding-a-nullable-column-with-default-values"></a>F. Agregar una columna que admite valores NULL con valores predeterminados

En el ejemplo siguiente se agrega una columna que acepta valores NULL con una definición `DEFAULT` y se usa `WITH VALUES` para proporcionar valores para cada fila existente en la tabla. Si no se utiliza WITH VALUES, cada fila tiene el valor NULL en la nueva columna.

```sql
CREATE TABLE dbo.doc_exf (column_a INT) ;
GO
INSERT INTO dbo.doc_exf VALUES (1) ;
GO
ALTER TABLE dbo.doc_exf
ADD AddDate smalldatetime NULL
CONSTRAINT AddDateDflt
DEFAULT GETDATE() WITH VALUES ;
GO
DROP TABLE dbo.doc_exf ;
GO
```

#### <a name="g-creating-a-primary-key-constraint-with-index-or-data-compression-options"></a>G. Creación de una restricción PRIMARY KEY con opciones de índice o compresión de datos

En el ejemplo siguiente se crea la restricción PRIMARY KEY `PK_TransactionHistoryArchive_TransactionID` y se establecen las opciones `FILLFACTOR`, `ONLINE` y `PAD_INDEX`. El índice clúster resultante tendrá el mismo nombre que la restricción.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (FILLFACTOR = 75, ONLINE = ON, PAD_INDEX = ON);
GO
```

En este ejemplo similar se aplica la compresión de página mientras se aplica la clave principal agrupada.

```sql
USE AdventureWorks;
GO
ALTER TABLE Production.TransactionHistoryArchive WITH NOCHECK
ADD CONSTRAINT PK_TransactionHistoryArchive_TransactionID PRIMARY KEY CLUSTERED (TransactionID)
WITH (DATA_COMPRESSION = PAGE);
GO
```

#### <a name="h-adding-a-sparse-column"></a>H. Agregar una columna dispersa

En los ejemplos siguientes se muestra cómo agregar y modificar columnas dispersas en la tabla T1. El código para crear la tabla `T1` es el siguiente.

```sql
CREATE TABLE T1 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) SPARSE NULL,
  C3 INT SPARSE NULL,
  C4 INT) ;
GO
```

Para agregar una columna dispersa adicional `C5`, ejecute la instrucción siguiente.

```sql
ALTER TABLE T1
ADD C5 CHAR(100) SPARSE NULL ;
GO
```

Para convertir la columna no dispersa `C4` en una columna dispersa, ejecute la instrucción siguiente.

```sql
ALTER TABLE T1
ALTER COLUMN C4 ADD SPARSE ;
GO
```

Para convertir la columna dispersa `C4` en una columna no dispersa, ejecute la instrucción siguiente.

```sql
ALTER TABLE T1
ALTER COLUMN C4 DROP SPARSE ;
GO
```

#### <a name="i-adding-a-column-set"></a>I. Agregar un conjunto de columnas

En los ejemplos siguientes se muestra cómo agregar una columna a la tabla `T2`. No se puede agregar un conjunto de columnas a una tabla si esta ya contiene columnas dispersas. El código para crear la tabla `T2` es el siguiente.

```sql
CREATE TABLE T2 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Las tres instrucciones siguientes agregan un conjunto de columnas denominado `CS` y, a continuación, modifican las columnas `C2` y `C3` a `SPARSE`.

```sql
ALTER TABLE T2
ADD CS XML COLUMN_SET FOR ALL_SPARSE_COLUMNS ;
GO

ALTER TABLE T2
ALTER COLUMN C2 ADD SPARSE ;
GO

ALTER TABLE T2
ALTER COLUMN C3 ADD SPARSE ;
GO
```

#### <a name="j-adding-an-encrypted-column"></a>J. Agregar una columna cifrada

La siguiente instrucción agrega una columna cifrada con el nombre `PromotionCode`.

```sql
ALTER TABLE Customers ADD
    PromotionCode nvarchar(100)
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = MyCEK,
    ENCRYPTION_TYPE = RANDOMIZED,
    ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') ;
```

### <a name="dropping-columns-and-constraints"></a><a name="Drop"></a>Quitar columnas y restricciones

En los ejemplos de esta sección se muestra cómo quitar columnas y restricciones.

#### <a name="a-dropping-a-column-or-columns"></a>A. Quitar una o varias columnas

En el primer ejemplo se modifica una tabla para quitar una columna. En el segundo ejemplo se quitan varias columnas.

```sql
CREATE TABLE dbo.doc_exb (
     column_a INT,
     column_b VARCHAR(20) NULL,
     column_c DATETIME,
     column_d INT) ;
GO  
-- Remove a single column.
ALTER TABLE dbo.doc_exb DROP COLUMN column_b ;
GO
-- Remove multiple columns.
ALTER TABLE dbo.doc_exb DROP COLUMN column_c, column_d;
```

#### <a name="b-dropping-constraints-and-columns"></a>B. Quitar restricciones y columnas

En el primer ejemplo se quita una restricción `UNIQUE` de una tabla. En el segundo ejemplo se quitan dos restricciones y una sola columna.

```sql
CREATE TABLE dbo.doc_exc (column_a INT NOT NULL CONSTRAINT my_constraint UNIQUE) ;
GO

-- Example 1. Remove a single constraint.
ALTER TABLE dbo.doc_exc DROP my_constraint ;
GO

DROP TABLE dbo.doc_exc;
GO

CREATE TABLE dbo.doc_exc ( column_a INT
                          NOT NULL CONSTRAINT my_constraint UNIQUE
                          ,column_b INT
                          NOT NULL CONSTRAINT my_pk_constraint PRIMARY KEY) ;
GO

-- Example 2. Remove two constraints and one column
-- The keyword CONSTRAINT is optional. The keyword COLUMN is required.
ALTER TABLE dbo.doc_exc
DROP CONSTRAINT my_constraint, my_pk_constraint, COLUMN column_b ;
GO
```

#### <a name="c-dropping-a-primary-key-constraint-in-the-online-mode"></a>C. Quitar una restricción PRIMARY KEY en modo ONLINE

En el ejemplo siguiente se elimina una restricción KEY PRIMARY con la opción `ONLINE` establecida en `ON`.

```sql
ALTER TABLE Production.TransactionHistoryArchive
DROP CONSTRAINT PK_TransactionHistoryArchive_TransactionID
WITH (ONLINE = ON) ;
GO
```

#### <a name="d-adding-and-dropping-a-foreign-key-constraint"></a>D. Agregar y quitar una restricción FOREIGN KEY

En el ejemplo siguiente se crea la tabla `ContactBackup` y, a continuación, se modifica la tabla; primero se agrega una restricción `FOREIGN KEY` que hace referencia a la tabla `Person.Person` y, a continuación, se quita la restricción `FOREIGN KEY`.

```sql
CREATE TABLE Person.ContactBackup
    (ContactID INT) ;
GO

ALTER TABLE Person.ContactBackup
ADD CONSTRAINT FK_ContactBackup_Contact FOREIGN KEY (ContactID)
    REFERENCES Person.Person (BusinessEntityID) ;
GO

ALTER TABLE Person.ContactBackup
DROP CONSTRAINT FK_ContactBackup_Contact ;
GO

DROP TABLE Person.ContactBackup ;
```

![Icono de flecha usado con el vínculo Volver al principio](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "Icono de flecha usado con el vínculo Volver al principio") [Ejemplos](#Example_Top)

### <a name="altering-a-column-definition"></a><a name="alter_column"></a> Modificar una definición de columna

#### <a name="a-changing-the-data-type-of-a-column"></a>A. Cambiar el tipo de datos de una columna

En el ejemplo siguiente se modifica una columna de una tabla de `INT` a `DECIMAL`.

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy ALTER COLUMN column_a DECIMAL (5, 2) ;
GO
DROP TABLE dbo.doc_exy ;
GO
```

#### <a name="b-changing-the-size-of-a-column"></a>B. Cambiar el tamaño de una columna

En el ejemplo siguiente se aumenta el tamaño de una columna **varchar** y la precisión y la escala de una columna **decimal**. Dado que las columnas contienen datos, solo se puede aumentar el tamaño de columna. Observe también que `col_a` se define en un índice único. Aún se puede aumentar el tamaño de `col_a` porque el tipo de datos es **varchar** y el índice no es el resultado de una restricción KEY PRIMARY.

```sql
-- Create a two-column table with a unique index on the varchar column.
CREATE TABLE dbo.doc_exy (col_a varchar(5) UNIQUE NOT NULL, col_b decimal (4,2)) ;
GO
INSERT INTO dbo.doc_exy VALUES ('Test', 99.99) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
GO
-- Increase the size of the varchar column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_a varchar(25) ;
GO
-- Increase the scale and precision of the decimal column.
ALTER TABLE dbo.doc_exy ALTER COLUMN col_b decimal (10,4) ;
GO
-- Insert a new row.
INSERT INTO dbo.doc_exy VALUES ('MyNewColumnSize', 99999.9999) ;
GO
-- Verify the current column size.
SELECT name, TYPE_NAME(system_type_id), max_length, precision, scale
FROM sys.columns WHERE object_id = OBJECT_ID(N'dbo.doc_exy') ;
```

#### <a name="c-changing-column-collation"></a>C. Cambiar la intercalación de columnas

En el siguiente ejemplo se muestra cómo cambiar la intercalación de una columna. Primero, se crea una tabla con la intercalación predeterminada del usuario.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Después, la intercalación de la columna `C2` se cambia a Latin1_General_BIN. El tipo de datos se requiere, incluso aunque no se cambie.

```sql
ALTER TABLE T3
ALTER COLUMN C2 varchar(50) COLLATE Latin1_General_BIN ;
GO
```

#### <a name="d-encrypting-a-column"></a>D. Cifrado de una columna

El ejemplo siguiente muestra cómo cifrar una columna mediante [Always Encrypted con enclaves seguros](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

En primer lugar, se crea una tabla sin columnas cifradas.

```sql
CREATE TABLE T3 (
  C1 INT PRIMARY KEY,
  C2 VARCHAR(50) NULL,
  C3 INT NULL,
  C4 INT) ;
GO
```

Después, se cifra la columna "C2" con una clave de cifrado de columna, denominada CEK1, y cifrado aleatorio. Para que la siguiente instrucción se realice correctamente:

- La clave de cifrado de la columna debe estar habilitada para el enclave. Esto significa que debe cifrarse con una clave maestra de columna que permita los cálculos de enclave.
- La instancia de SQL Server de destino debe admitir Always Encrypted con enclaves seguros.
- La instrucción debe emitirse a través de una conexión configurada para Always Encrypted con enclaves seguros y con un controlador cliente compatible.
- La aplicación que realiza la llamada debe tener acceso a la clave maestra de columna que protege CEK1.

```sql
ALTER TABLE T3
ALTER COLUMN C2 VARCHAR(50) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NULL;
GO
```

### <a name="altering-a-table-definition"></a><a name="alter_table"></a> Modificar una definición de tabla

En los ejemplos de esta sección se muestra cómo modificar la definición de una tabla.

#### <a name="a-modifying-a-table-to-change-the-compression"></a>A. Modificar una tabla para cambiar la compresión

En el ejemplo siguiente se cambia la compresión de una tabla sin particiones. Se volverá a generar el montón o el índice clúster. Si la tabla es un montón, se volverán a generar todos los índices no clúster.

```sql
ALTER TABLE T1
REBUILD WITH (DATA_COMPRESSION = PAGE) ;
```

En el ejemplo siguiente se cambia la compresión de una tabla con particiones. La sintaxis `REBUILD PARTITION = 1` hace que solo se vuelva a generar la partición número `1`.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION =NONE) ;
GO
```

La misma operación que utiliza la sintaxis alternativa siguiente hace que se vuelvan a generar todas las particiones de la tabla.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = ALL
WITH (DATA_COMPRESSION = PAGE ON PARTITIONS(1)) ;
```

Para obtener más ejemplos de compresión de datos, vea [Compresión de datos](../../relational-databases/data-compression/data-compression.md).

#### <a name="b-modifying-a-columnstore-table-to-change-archival-compression"></a>B. Modificar una tabla de almacén de columnas para cambiar la compresión de archivo

En el ejemplo siguiente se comprime aún más una partición de tabla de almacén de columnas aplicando un algoritmo de compresión adicional. Esta compresión reduce la tabla a un tamaño mínimo, pero también aumenta el tiempo necesario para el almacenamiento y la recuperación. Esto resulta útil para el archivado o para otras situaciones que requieran menos espacio y en las que pueda permitirse más tiempo para el almacenamiento y recuperación.

**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE) ;
GO
```

En el siguiente ejemplo se descomprime una partición de tabla de almacén de columnas que se comprimió con la opción COLUMNSTORE_ARCHIVE. Cuando se restauran los datos, seguirán estando comprimidos con la compresión de almacén de columnas que se usa para todas las tablas de almacén de columnas.

**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
ALTER TABLE PartitionTable1
REBUILD PARTITION = 1 WITH (DATA_COMPRESSION = COLUMNSTORE) ;
GO
```

#### <a name="c-switching-partitions-between-tables"></a>C. Cambiar las particiones entre tablas

En el ejemplo siguiente se crea una tabla con particiones, suponiendo que el esquema de partición `myRangePS1` ya esté creado en la base de datos. A continuación, se crea una tabla sin particiones con la misma estructura que la tabla con particiones y en el mismo grupo de archivos que `PARTITION 2` de la tabla `PartitionTable`. Los datos de `PARTITION 2` de la tabla `PartitionTable` se cambian entonces a la tabla `NonPartitionTable`.

```sql
CREATE TABLE PartitionTable (col1 INT, col2 CHAR(10))
ON myRangePS1 (col1) ;
GO
CREATE TABLE NonPartitionTable (col1 INT, col2 CHAR(10))
ON test2fg ;
GO
ALTER TABLE PartitionTable SWITCH PARTITION 2 TO NonPartitionTable ;
GO
```

#### <a name="d-allowing-lock-escalation-on-partitioned-tables"></a>D. Permitir la extensión de bloqueo en tablas con particiones

En el ejemplo siguiente se habilita la extensión de bloqueo al nivel de partición en una tabla con particiones. Si la tabla no tiene particiones, la extensión de bloqueo se establece en el nivel TABLE.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
ALTER TABLE dbo.T1 SET (LOCK_ESCALATION = AUTO) ;
GO
```

#### <a name="e-configuring-change-tracking-on-a-table"></a>E. Configurar el seguimiento de cambios en una tabla

En el ejemplo siguiente se habilita el seguimiento de cambios en la tabla `Person.Person`.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
USE AdventureWorks;
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING ;
```

El ejemplo siguiente habilita el seguimiento de cambios y el seguimiento de las columnas que se actualizan durante un cambio.

**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
ENABLE CHANGE_TRACKING
WITH (TRACK_COLUMNS_UPDATED = ON)
```

En el ejemplo siguiente se deshabilita el seguimiento de cambios en la tabla `Person.Person`.

**Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
USE AdventureWorks;
GO
ALTER TABLE Person.Person
DISABLE CHANGE_TRACKING ;
```

### <a name="disabling-and-enabling-constraints-and-triggers"></a><a name="disable_enable"></a>Deshabilitar y habilitar restricciones y desencadenadores

#### <a name="a-disabling-and-re-enabling-a-constraint"></a>A. Deshabilitar y volver a habilitar una restricción

En el ejemplo siguiente se deshabilita una restricción que limita los salarios aceptados en los datos. `NOCHECK CONSTRAINT` se usa con `ALTER TABLE` para deshabilitar la restricción y permitir una inserción que normalmente infringiría la restricción. `CHECK CONSTRAINT` vuelve a habilitar la restricción.

```sql
CREATE TABLE dbo.cnst_example (
  id INT NOT NULL,
  name VARCHAR(10) NOT NULL,
  salary MONEY NOT NULL
  CONSTRAINT salary_cap CHECK (salary < 100000)) ;

-- Valid inserts
INSERT INTO dbo.cnst_example VALUES (1,'Joe Brown',65000) ;
INSERT INTO dbo.cnst_example VALUES (2,'Mary Smith',75000) ;

-- This insert violates the constraint.
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Disable the constraint and try again.
ALTER TABLE dbo.cnst_example NOCHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (3,'Pat Jones',105000) ;

-- Re-enable the constraint and try another insert; this will fail.
ALTER TABLE dbo.cnst_example CHECK CONSTRAINT salary_cap;
INSERT INTO dbo.cnst_example VALUES (4,'Eric James',110000) ;
```

#### <a name="b-disabling-and-re-enabling-a-trigger"></a>B. Deshabilitar y volver a habilitar un desencadenador

En el ejemplo siguiente se usa la opción `DISABLE TRIGGER` de `ALTER TABLE` para deshabilitar el desencadenador y permitir una inserción que normalmente infringiría el desencadenador. Después se usa `ENABLE TRIGGER` para volver a habilitar el desencadenador.

```sql
CREATE TABLE dbo.trig_example (
  id INT,
  name VARCHAR(12),
  salary MONEY) ;
GO
-- Create the trigger.
CREATE TRIGGER dbo.trig1 ON dbo.trig_example FOR INSERT
AS
IF (SELECT COUNT(*) FROM INSERTED
WHERE salary > 100000) > 0
BEGIN
    print 'TRIG1 Error: you attempted to insert a salary > $100,000'
    ROLLBACK TRANSACTION
END ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (1,'Pat Smith',100001) ;
GO
-- Disable the trigger.
ALTER TABLE dbo.trig_example DISABLE TRIGGER trig1 ;
GO
-- Try an insert that would typically violate the trigger.
INSERT INTO dbo.trig_example VALUES (2,'Chuck Jones',100001) ;
GO
-- Re-enable the trigger.
ALTER TABLE dbo.trig_example ENABLE TRIGGER trig1 ;
GO
-- Try an insert that violates the trigger.
INSERT INTO dbo.trig_example VALUES (3,'Mary Booth',100001) ;
GO
```

### <a name="online-operations"></a><a name="online"></a>Operaciones en línea

#### <a name="a-online-index-rebuild-using-low-priority-wait-options"></a>A. Volver a generar el índice en línea mediante opciones de espera de prioridad baja

En el ejemplo siguiente se muestra cómo realizar una regeneración de índice en línea que especifica las opciones de espera de prioridad baja.

**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
ALTER TABLE T1
REBUILD WITH
(
    PAD_INDEX = ON,
    ONLINE = ON ( WAIT_AT_LOW_PRIORITY ( MAX_DURATION = 4 MINUTES,
                                         ABORT_AFTER_WAIT = BLOCKERS ) )
) ;
```

#### <a name="b-online-alter-column"></a>B. Alteración de columna en línea

En el ejemplo siguiente se muestra cómo ejecutar una operación de alteración de columna con la opción ONLINE.

**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

```sql
CREATE TABLE dbo.doc_exy (column_a INT) ;
GO
INSERT INTO dbo.doc_exy (column_a) VALUES (10) ;
GO
ALTER TABLE dbo.doc_exy
    ALTER COLUMN column_a DECIMAL (5, 2) WITH (ONLINE = ON) ;
GO
sp_help doc_exy;
DROP TABLE dbo.doc_exy ;
GO
```

### <a name="system-versioning"></a><a name="system_versioning"></a> Control de versiones del sistema

Los cuatro ejemplos siguientes le ayudarán a familiarizarse con la sintaxis para usar el control de versiones del sistema. Para obtener más ayuda, consulte [Introducción a las tablas temporales con versión del sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md).

**Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

#### <a name="a-add-system-versioning-to-existing-tables"></a>A. Agregar el control de versiones del sistema a las tablas existentes

En el ejemplo siguiente se muestra cómo agregar el control de versiones del sistema a una tabla existente y cómo crear una futura tabla de historial. En este ejemplo se supone que hay una tabla existente denominada `InsurancePolicy` con una clave principal definida. En este ejemplo se rellenan las columnas de período recién creadas para las versiones de sistema utilizando los valores predeterminados para los tiempos de inicio y finalización porque estos valores no pueden ser null. En este ejemplo se utiliza la cláusula HIDDEN para garantizar que las aplicaciones existentes que interactúan con la tabla actual no se vean afectadas. También se usa HISTORY_RETENTION_PERIOD, que solo está disponible en [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].

```sql
--Alter non-temporal table to define periods for system versioning
ALTER TABLE InsurancePolicy
ADD PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime),
SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
    DEFAULT SYSUTCDATETIME(),
SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN NOT NULL
    DEFAULT CONVERT(DATETIME2, '9999-12-31 23:59:59.99999999') ;

--Enable system versioning with 1 year retention for historical data
ALTER TABLE InsurancePolicy
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 1 YEAR)) ;
```

#### <a name="b-migrate-an-existing-solution-to-use-system-versioning"></a>B. Migrar una solución existente para usar el control de versiones del sistema

En el ejemplo siguiente se muestra cómo migrar a las versiones del sistema desde una solución que utilice desencadenadores para imitar una compatibilidad temporal. En el ejemplo se da por supuesto que hay una solución existente en la que se usa una tabla `ProjectTask` y una tabla `ProjectTaskHistory` para su solución existente, que usa las columnas `Changed Date` y `Revised Date` para sus períodos, que estas columnas de período no usan el tipo de datos `datetime2` y que la tabla `ProjectTask` tiene definida una clave principal.

```sql
-- Drop existing trigger
DROP TRIGGER ProjectTask_HistoryTrigger;

-- Adjust the schema for current and history table
-- Change data types for existing period columns
ALTER TABLE ProjectTask ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTask ALTER COLUMN [Revised Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Changed Date] datetime2 NOT NULL ;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [Revised Date] datetime2 NOT NULL ;

-- Add SYSTEM_TIME period and set system versioning with linking two existing tables
-- (a certain set of data checks happen in the background)
ALTER TABLE ProjectTask
ADD PERIOD FOR SYSTEM_TIME ([Changed Date], [Revised Date])

ALTER TABLE ProjectTask
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON))
```

#### <a name="c-disabling-and-re-enabling-system-versioning-to-change-table-schema"></a>C. Deshabilitar y volver a habilitar el control de versiones del sistema para cambiar el esquema de la tabla de cambios

En este ejemplo se muestra cómo deshabilitar el control de versiones del sistema en la tabla `Department`, agregar una columna y volver a habilitar el control de versiones del sistema. Es necesario deshabilitar el control de versiones del sistema para modificar el esquema de la tabla. Siga estos pasos en una transacción para impedir las actualizaciones de ambas tablas al mismo tiempo que se actualiza el esquema de tabla, que permite que el DBA omita la comprobación de coherencia de los datos al volver a habilitar el control de versiones del sistema y obtener una ventaja de rendimiento. Tareas como la creación de estadísticas, la conmutación de particiones o la aplicación de la compresión en una o ambas tablas no requieren la deshabilitación del control de versiones del sistema.

```sql
BEGIN TRAN
/* Takes schema lock on both tables */
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
/* expand table schema for temporal table */
ALTER TABLE Department  
     ADD Col5 int NOT NULL DEFAULT 0 ;
/* Expand table schema for history table */
ALTER TABLE DepartmentHistory
    ADD Col5 int NOT NULL DEFAULT 0 ;
/* Re-establish versioning again*/
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE=dbo.DepartmentHistory,
                                 DATA_CONSISTENCY_CHECK = OFF)) ;
COMMIT
```

#### <a name="d-removing-system-versioning"></a>D. Quitar el control de versiones del sistema

En este ejemplo se muestra cómo quitar completamente el control de versiones del sistema de la tabla del departamento y cómo quitar la tabla `DepartmentHistory`. Si lo desea, también puede quitar las columnas de período utilizadas por el sistema para registrar información de versiones del sistema. No se puede quitar la tabla `Department` ni `DepartmentHistory` mientras el control de versiones del sistema esté habilitado.

```sql
ALTER TABLE Department
    SET (SYSTEM_VERSIONING = OFF) ;
ALTER TABLE Department
DROP PERIOD FOR SYSTEM_TIME ;
DROP TABLE DepartmentHistory ;
```

## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

En todos los ejemplos siguientes, del A al C, se usa la tabla `FactResellerSales` de la base de datos [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)].

### <a name="a-determining-if-a-table-is-partitioned"></a>A. Determinar si se crean particiones de una tabla

La consulta siguiente devuelve una o más filas si la tabla tiene particiones `FactResellerSales` . Si la tabla no tiene particiones, no se devuelve ninguna fila.

```sql
SELECT * FROM sys.partitions AS p
JOIN sys.tables AS t
    ON p.object_id = t.object_id
WHERE p.partition_id IS NOT NULL
    AND t.name = 'FactResellerSales' ;
```

### <a name="b-determining-boundary-values-for-a-partitioned-table"></a>B. Determinar los valores de límite para una tabla con particiones

La consulta siguiente devuelve los valores de límite para cada partición de la tabla `FactResellerSales` .

```sql
SELECT t.name AS TableName, i.name AS IndexName, p.partition_number,
    p.partition_id, i.data_space_id, f.function_id, f.type_desc,
    r.boundary_id, r.value AS BoundaryValue
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.partitions AS p
    ON i.object_id = p.object_id AND i.index_id = p.index_id
JOIN  sys.partition_schemes AS s
    ON i.data_space_id = s.data_space_id
JOIN sys.partition_functions AS f
    ON s.function_id = f.function_id
LEFT JOIN sys.partition_range_values AS r
    ON f.function_id = r.function_id and r.boundary_id = p.partition_number
WHERE t.name = 'FactResellerSales' AND i.type <= 1
ORDER BY p.partition_number ;
```

### <a name="c-determining-the-partition-column-for-a-partitioned-table"></a>C. Determinar la columna de partición de una tabla con particiones

La consulta siguiente devuelve el nombre de la columna de partición de la tabla. `FactResellerSales`.

```sql
SELECT t.object_id AS Object_ID, t.name AS TableName,
    ic.column_id as PartitioningColumnID, c.name AS PartitioningColumnName
FROM sys.tables AS t
JOIN sys.indexes AS i
    ON t.object_id = i.object_id
JOIN sys.columns AS c
    ON t.object_id = c.object_id
JOIN sys.partition_schemes AS ps
    ON ps.data_space_id = i.data_space_id
JOIN sys.index_columns AS ic
    ON ic.object_id = i.object_id
    AND ic.index_id = i.index_id AND ic.partition_ordinal > 0
WHERE t.name = 'FactResellerSales'
AND i.type <= 1
AND c.column_id = ic.column_id ;
```

### <a name="d-merging-two-partitions"></a>D. Combinar dos particiones

En el ejemplo siguiente se combinan dos particiones en una tabla.

La tabla `Customer` tiene la definición siguiente:

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100))) ;
```

El comando siguiente combina los límites de partición 10 y 25.

```sql
ALTER TABLE Customer MERGE RANGE (10);
```

El nuevo archivo DDL para la tabla es:

```sql
CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 25, 50, 100))) ;
```

### <a name="e-splitting-a-partition"></a>E. Dividir una partición

En el ejemplo siguiente se divide una partición de una tabla.

La tabla `Customer` tiene el siguiente archivo DDL:

```sql
DROP TABLE Customer;

CREATE TABLE Customer (
    id INT NOT NULL,
    lastName VARCHAR(20),
    orderCount INT,
    orderDate DATE)
WITH
    ( DISTRIBUTION = HASH(id),
    PARTITION ( orderCount RANGE LEFT
    FOR VALUES (1, 5, 10, 25, 50, 100 ))) ;
```

El siguiente comando crea un nuevo límite de partición por el valor 75, entre 50 y 100.

```sql
ALTER TABLE Customer SPLIT RANGE (75);
```

El nuevo archivo DDL para la tabla es:

```sql
CREATE TABLE Customer (
   id INT NOT NULL,
   lastName VARCHAR(20),
   orderCount INT,
   orderDate DATE)
   WITH DISTRIBUTION = HASH(id),
   PARTITION ( orderCount (RANGE LEFT
      FOR VALUES (1, 5, 10, 25, 50, 75, 100))) ;
```

### <a name="f-using-switch-to-move-a-partition-to-a-history-table"></a>F. Usar SWITCH para mover una partición a una tabla de historial

En el siguiente ejemplo se mueven los datos de una partición de la tabla `Orders` a una partición de la tabla `OrdersHistory`.

La tabla `Orders` tiene el siguiente archivo DDL:

```sql
CREATE TABLE Orders (
    id INT,
    city VARCHAR (25),
    lastUpdateDate DATE,
    orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01', '2005-01-01', '2006-01-01', '2007-01-01'))) ;
```

En este ejemplo, la tabla `Orders` tiene las siguientes particiones. Cada partición contiene datos.

|Partition|¿Tiene datos?|Intervalo de límite|
|---------------|---------------|--------------------|
|1|Sí|OrderDate < '2004-01-01'|
|2|Sí|'2004-01-01' <= OrderDate < '2005-01-01'|
|3|Sí|'2005-01-01' <= OrderDate< '2006-01-01'|
|4|Sí|'2006-01-01'<= OrderDate < '2007-01-01'|
|5|Sí|'2007-01-01' <= OrderDate|
| &nbsp; | &nbsp; | &nbsp; |

- Partición 1 (tiene datos): OrderDate < '2004-01-01'
- Partición 2 (tiene datos): '2004-01-01' <= OrderDate < '2005-01-01'
- Partición 3 (tiene datos): '2005-01-01' <= OrderDate< '2006-01-01'
- Partición 4 (tiene datos): '2006-01-01'<= OrderDate < '2007-01-01'
- Partición 5 (tiene datos): '2007-01-01' <= OrderDate

La tabla `OrdersHistory` tiene el siguiente DDL, que tiene columnas y nombres de columna idénticos a los de la tabla `Orders`. Ambos tienen una distribución de hash en la columna `id`.

```sql
CREATE TABLE OrdersHistory (
   id INT,
   city VARCHAR (25),
   lastUpdateDate DATE,
   orderDate DATE)
WITH
    (DISTRIBUTION = HASH (id),
    PARTITION ( orderDate RANGE RIGHT
    FOR VALUES ('2004-01-01'))) ;
```

Aunque las columnas y los nombres de columna deben ser iguales, no es necesario que los límites de partición sean iguales. En este ejemplo, la tabla `OrdersHistory` tiene las dos siguientes particiones y ambas están vacías:

- Partición 1 (sin datos): OrderDate < '2004-01-01'
- Partición 2 (vacía): "2004-01-01" <= OrderDate

En las dos tablas anteriores, el siguiente comando mueve todas las filas con `OrderDate < '2004-01-01'` de la tabla `Orders` a la tabla `OrdersHistory`.

```sql
ALTER TABLE Orders SWITCH PARTITION 1 TO OrdersHistory PARTITION 1;
```

Como resultado, la primera partición de `Orders` está vacía y la primera partición de `OrdersHistory` contiene datos. Ahora, las tablas aparecen como se muestra a continuación:

 Tabla `Orders`

- Partición 1 (vacía): OrderDate < '2004-01-01'
- Partición 2 (tiene datos): '2004-01-01' <= OrderDate < '2005-01-01'
- Partición 3 (tiene datos): '2005-01-01' <= OrderDate< '2006-01-01'
- Partición 4 (tiene datos): '2006-01-01'<= OrderDate < '2007-01-01'
- Partición 5 (tiene datos): '2007-01-01' <= OrderDate

Tabla `OrdersHistory`

- Partición 1 (tiene datos): OrderDate < '2004-01-01'
- Partición 2 (vacía): "2004-01-01" <= OrderDate

Para limpiar la tabla `Orders`, puede quitar la partición vacía combinando las particiones 1 y 2 de la forma siguiente:

```sql
ALTER TABLE Orders MERGE RANGE ('2004-01-01');
```

Después de la combinación, la tabla `Orders` tiene las siguientes particiones:

Tabla `Orders`

- Partición 1 (tiene datos): OrderDate < "2005-01-01"
- Partición 2 (tiene datos): '2005-01-01' <= OrderDate< '2006-01-01'
- Partición 3 (tiene datos): '2006-01-01'<= OrderDate < '2007-01-01'
- Partición 4 (tiene datos): '2007-01-01' <= OrderDate

Supongamos que termina otro año y quiere archivar el año 2005. Puede asignar una partición vacía al año 2005 en la tabla `OrdersHistory` dividiendo la partición vacía como se indica a continuación:

```sql
ALTER TABLE OrdersHistory SPLIT RANGE ('2005-01-01');
```

Después de la división, la tabla `OrdersHistory` tiene las siguientes particiones:

 Tabla `OrdersHistory`

- Partición 1 (tiene datos): OrderDate < '2004-01-01'
- Partición 2 (vacía): "2004-01-01" < "2005-01-01"
- Partición 3 (vacía): "2005-01-01" <= OrderDate

## <a name="see-also"></a>Consulte también

- [sys.tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)
- [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [DROP TABLE](../../t-sql/statements/drop-table-transact-sql.md)
- [sp_help](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)
- [ALTER PARTITION SCHEME](../../t-sql/statements/alter-partition-scheme-transact-sql.md)
- [ALTER PARTITION FUNCTION](../../t-sql/statements/alter-partition-function-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
