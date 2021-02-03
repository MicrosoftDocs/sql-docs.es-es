---
description: CREATE TYPE (Transact-SQL)
title: CREATE TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- sql13.swb.sysdatatype.properties.f1
- CREATE TYPE
- TYPE_TSQL
- TYPE
- CREATE_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- UDTs [SQL Server], creating
- CLR user-defined types [SQL Server]
- user-defined table types [SQL Server]
- user-defined types [SQL Server], creating
- CREATE TYPE statement
- alias data types [SQL Server], creating
- data types [SQL Server], creating
ms.assetid: 2202236b-e09f-40a1-bbc7-b8cff7488905
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2063543c497070bd8366cae9a8691b3701fc4330
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177912"
---
# <a name="create-type-transact-sql"></a>CREATE TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Crea un tipo de datos de alias o un tipo definido por el usuario en la base de datos actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. La implementación de un tipo de datos de alias se basa en un tipo nativo del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un tipo definido por el usuario se implementa a través de una clase de un ensamblado de Common Language Runtime (CLR) de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Para enlazar un tipo definido por el usuario a su implementación, el ensamblado CLR que contiene la implementación del tipo debe registrarse primero en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por medio de [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md).  
  
 De manera predeterminada, la posibilidad de ejecutar código de CLR está desactivada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se pueden crear, modificar y quitar objetos de base de datos que hagan referencia a módulos de código administrado, pero estas referencias no se ejecutarán en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a menos que se habilite la [opción clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) por medio de [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
 
> [!NOTE]  
>  En este tema se describe la integración de CLR de .NET Framework en SQL Server. La integración de CLR no se aplica a Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- User-defined Data Type Syntax    
CREATE TYPE [ schema_name. ] type_name  
{   
    [
      FROM base_type   
      [ ( precision [ , scale ] ) ]  
      [ NULL | NOT NULL ]
    ]
    | EXTERNAL NAME assembly_name [ .class_name ]   
    | AS TABLE ( { <column_definition> | <computed_column_definition> [ ,... n ] }
      [ <table_constraint> ] [ ,... n ]    
      [ <table_index> ] [ ,... n ] } )
 
} [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   
    [ NULL | NOT NULL ]  
    [   
        DEFAULT constant_expression ]   
      | [ IDENTITY [ ( seed ,increment ) ]   
    ]  
    [ ROWGUIDCOL ] [ <column_constraint> [ ...n ] ]   
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max |   
                [ { CONTENT | DOCUMENT } ] xml_schema_collection ) ]   
  
<column_constraint> ::=   
{     { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
        [   
            WITH ( <index_option> [ ,...n ] )   
        ]  
  | CHECK ( logical_expression )   
}   
  
<computed_column_definition> ::=  
  
column_name AS computed_column_expression   
[ PERSISTED [ NOT NULL ] ]  
[   
    { PRIMARY KEY | UNIQUE }  
        [ CLUSTERED | NONCLUSTERED ]  
        [   
            WITH ( <index_option> [ ,...n ] )  
        ]  
    | CHECK ( logical_expression )   
]   
  
<table_constraint> ::=  
{   
    { PRIMARY KEY | UNIQUE }   
        [ CLUSTERED | NONCLUSTERED ]   
    ( column [ ASC | DESC ] [ ,...n ] )   
        [   
    WITH ( <index_option> [ ,...n ] )   
        ]  
    | CHECK ( logical_expression )   
}   
  
<index_option> ::=  
{  
    IGNORE_DUP_KEY = { ON | OFF }  
}  

< table_index > ::=  
  INDEX constraint_name  
     [ CLUSTERED | NONCLUSTERED ]   (column [ ASC | DESC ] [ ,... n ] )} }  
```  
  
```syntaxsql
-- User-defined Memory Optimized Table Types Syntax  
CREATE TYPE [schema_name. ] type_name  
AS TABLE ( { <column_definition> [ ,... n ] }  
    | [ <table_constraint> ] [ ,... n ]    
    | [ <table_index> ] [ ,... n ] } )
    [ WITH ( <table_option> [ ,... n ] ) ]  
 [ ; ]  
  
<column_definition> ::=  
column_name <data_type>  
    [ COLLATE collation_name ]   [ NULL | NOT NULL ]    [  
      [ IDENTITY [ (1 , 1) ]  
    ]  
    [ <column_constraint> [ ... n ] ]    [ <column_index> ]  
  
<data type> ::=  
 [type_schema_name . ] type_name [ (precision [ , scale ]) ]  
  
<column_constraint> ::=  
{ PRIMARY KEY {   NONCLUSTERED HASH WITH (BUCKET_COUNT = bucket_count) 
                | NONCLUSTERED } }  
  
< table_constraint > ::=  
{ PRIMARY KEY { NONCLUSTERED HASH (column [ ,... n ] ) 
                   WITH (BUCKET_COUNT = bucket_count) 
               | NONCLUSTERED  (column [ ASC | DESC ] [ ,... n ] )  } }  
  
<column_index> ::=  
  INDEX index_name  
{ { [ NONCLUSTERED ] HASH WITH (BUCKET_COUNT = bucket_count) 
     | NONCLUSTERED } }  
  
< table_index > ::=  
  INDEX constraint_name  
{ { [ NONCLUSTERED ] HASH (column [ ,... n ] ) WITH (BUCKET_COUNT = bucket_count) 
 |  [NONCLUSTERED]  (column [ ASC | DESC ] [ ,... n ] )} }  
  
<table_option> ::=  
{  
    [MEMORY_OPTIMIZED = {ON | OFF}]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *schema_name*  
 Es el nombre del esquema al que pertenece el tipo de datos de alias o el tipo definido por el usuario.  
  
 *type_name*  
 Es el nombre del tipo de datos de alias o del tipo definido por el usuario. Los nombres de tipos deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
 *base_type*  
 Es el tipo de datos suministrado por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se basa el tipo de datos de alias. *base_type* es de tipo **sysname**, no tiene ningún valor predeterminado y puede tener uno de los valores siguientes:  
  
|||||  
|-|-|-|-|  
|**bigint**|**binary(** *n* **)**|**bit**|**char(** *n* **)**|  
|**date**|**datetime**|**datetime2**|**datetimeoffset**|  
|**decimal**|**float**|**image**|**int**|  
|**money**|**nchar(** *n* **)**|**ntext**|**numeric**|  
|**nvarchar(** *n* &#124; **max)**|**real**|**smalldatetime**|**smallint**|  
|**smallmoney**|**sql_variant**|**text**|**time**|  
|**tinyint**|**uniqueidentifier**|**varbinary(** *n* &#124; **max)**|**varchar(** *n* &#124; **max)**|  
  
 *base_type* también puede ser cualquier sinónimo de tipo de datos que esté asignado a uno de estos tipos de datos del sistema.  
  
 *precisión*  
 Para **decimal** o **numeric**, es un entero no negativo que indica el número máximo de dígitos decimales que se pueden almacenar en total, tanto a la derecha como a la izquierda del separador decimal. Para más información, vea [decimal y numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 *scale*  
 Para **decimal** o **numeric**, es un entero no negativo que indica el número máximo de dígitos decimales que se pueden almacenar a la derecha del separador decimal, y debe ser menor o igual que el valor de precisión. Para más información, vea [decimal y numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 **NULL** | NOT NULL  
 Especifica si el tipo puede contener un valor NULL. Si no se especifica, el valor predeterminado es NULL.  
  
 *assembly_name*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica el ensamblado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hace referencia a la implementación del tipo definido por el usuario en Common Language Runtime. *assembly_name* debe coincidir con un ensamblado existente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos actual.  
  
> [!NOTE]  
>  EXTERNAL_NAME no está disponible en una base de datos independiente.  
  
 **[.** *class_name*  **]**  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica la clase en el ensamblado que implementa el tipo definido por el usuario. *class_name* debe ser un identificador válido y debe existir como clase en el ensamblado con visibilidad de ensamblado. *class_name* distingue entre mayúsculas y minúsculas, independientemente de la intercalación de base de datos, y debe coincidir exactamente con el nombre de la clase del ensamblado correspondiente. El nombre de clase puede ser un nombre de espacio de nombres entre corchetes ( **[ ]** ) si el lenguaje de programación usado para escribir la clase usa el concepto de espacios de nombres, como es el caso de C#. Si no se especifica *class_name*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera que es el mismo que *type_name*.  
  
 \<column_definition>  
 Define las columnas para un tipo de tabla definido por el usuario.  
  
 \<data type>  
 Define el tipo de datos en una columna para un tipo de tabla definido por el usuario. Para más información sobre los tipos de datos, vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md). Para más información sobre las tablas, vea [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<column_constraint>  
 Define las restricciones de columna para un tipo de tabla definido por el usuario. Las restricciones compatibles incluyen PRIMARY KEY, UNIQUE y CHECK. Para más información sobre las tablas, vea [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<computed_column_definition>  
 Define una expresión de columna calculada como una columna en un tipo de tabla definido por el usuario. Para más información sobre las tablas, vea [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 \<table_constraint>  
 Define una restricción de tabla para un tipo de tabla definido por el usuario. Las restricciones compatibles incluyen PRIMARY KEY, UNIQUE y CHECK.  
  
 \<index_option>  
 Especifica la respuesta de error para valores de clave duplicados en una operación de inserción de varias filas en un índice clúster o no clúster único. Para más información sobre las opciones de índice, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
 
  `INDEX *index_name* [ CLUSTERED | NONCLUSTERED ] (*column_name* [ ASC | DESC ] [ ,... *n* ] )`  
     
**Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Especifica que se creará un índice en la tabla. Puede tratarse de un índice agrupado o un índice que no esté en clúster. El índice incluirá las columnas enumeradas y ordenará los datos en orden ascendente o descendente.
  
 INDEX  
 Debe especificar los índices de columna y de tabla como parte de la instrucción CREATE TABLE. CREATE INDEX y DROP INDEX no se admiten en las tablas optimizadas para memoria.  
  
 MEMORY_OPTIMIZED  
 **Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Indica si el tipo de tabla está con optimización para memoria. Esta opción está desactivada de forma predeterminada; la tabla (tipo) no es una tabla con optimización para memoria (tipo). Los tipos de tablas optimizadas para memoria son tablas de usuario cuyo esquema se conserva en el disco de forma similar al de otras tablas de usuario.  
  
 BUCKET_COUNT  
 **Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Indica el número de cubos que se deben crear en el índice hash. El valor máximo para BUCKET_COUNT en índices hash es 1 073 741 824. Para más información sobre los números de cubos, vea [Índices de las tablas con optimización para memoria](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md). *bucket_count* es un argumento obligatorio.  
  
 HASH  
 **Se aplica a**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores, y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 Indica que se ha creado un índice HASH. Los índices hash solo se admiten en las tablas optimizadas para memoria.  
  
## <a name="remarks"></a>Observaciones  
 La clase del ensamblado a la que se hace referencia en *assembly_name*, junto con sus métodos, debe cumplir todos los requisitos para implementar un tipo definido por el usuario en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información sobre estos requisitos, vea [CLR User-Defined Types](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) (Tipos definidos por el usuario CLR).  
  
 Existen algunas consideraciones adicionales, entre las que se pueden citar las siguientes:  
  
-   La clase puede tener métodos sobrecargados, pero éstos solo se pueden llamar desde el código administrado, no desde [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Los miembros estáticos se deben declarar como **const** o **readonly** si *assembly_name* es SAFE o EXTERNAL_ACCESS.  
  
 Dentro de una base de datos solo puede haber un tipo definido por el usuario registrado con cualquier tipo especificado que se haya cargado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde CLR. Si se crea un tipo definido por el usuario a partir de un tipo de CLR para el que ya existe un tipo definido por el usuario en la base de datos, CREATE TYPE genera un error. Esta restricción es necesaria para evitar la ambigüedad durante la resolución de tipos SQL si un tipo CLR se puede asignar a más de un tipo definido por el usuario.  
  
 Si un método mutador del tipo no devuelve *void*, la instrucción CREATE TYPE no se ejecuta.  
  
 Para modificar un tipo definido por el usuario, debe volver a crearlo después de haberlo quitado mediante una instrucción DROP TYPE.  
  
 A diferencia de los tipos definidos por el usuario y creados por medio de **sp_addtype**, el rol de base de datos **public** no recibe automáticamente el permiso REFERENCES para los tipos creados usando CREATE TYPE. Este permiso debe concederse por separado.  
  
 En los tipos de tabla definidos por el usuario, los tipos definidos por el usuario estructurados que se usan en *nombre_columna* \<data type> forman parte del ámbito del esquema de la base de datos en el que se define el tipo de tabla. Para tener acceso a los tipos definidos por el usuario estructurados en un ámbito diferente dentro de la base de datos, utilice nombres de dos partes.  
  
 En los tipos de tabla definidos por el usuario, la clave principal en las columnas calculadas debe ser PERSISTED y NOT NULL.  
  
## <a name="memory-optimized-table-types"></a>Tipos de tablas con optimización para memoria  
 A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], el proceso de los datos de un tipo de tabla se puede realizar en la memoria principal y no en el disco. Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Para ver códigos de ejemplo que ilustran cómo crear tablas optimizadas para memoria, vea [Crear una tabla con optimización para memoria y un procedimiento almacenado compilado de forma nativa](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CREATE TYPE en la base de datos actual y el permiso ALTER en *schema_name*. Si no se especifica *schema_name* , se aplican las reglas de resolución de nombres predeterminadas para determinar el esquema del usuario actual. Si se especifica *assembly_name*, el usuario debe ser propietario del ensamblado o tener el permiso REFERENCES en él.  

 Si las columnas de la instrucción CREATE TABLE se definen como un tipo definido por el usuario, se necesita el permiso REFERENCES en el tipo definido por el usuario.
 
   >[!NOTE]
  > Un usuario que crea una tabla con una columna que utiliza un tipo definido por el usuario necesita el permiso REFERENCES en el tipo definido por el usuario.
  > Si esta tabla se debe crear en TempDB, entonces el permiso REFERENCES debe concederse explícitamente cada vez **antes** de que se cree la tabla, o este tipo de datos y los permisos REFERENCES deben agregarse a la base de datos Model. Si esto se hace, entonces este tipo de datos y permisos estarán disponibles en TempDB permanentemente. De lo contrario, los permisos y el tipo de datos definidos por el usuario desaparecerán cuando se reinicie SQL Server. Para más información, consulte [CREATE TABLE](./create-table-transact-sql.md#permissions-1).
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-an-alias-type-based-on-the-varchar-data-type"></a>A. Crear un tipo de alias basado en el tipo de datos varchar  
 En el ejemplo siguiente se crea un tipo de alias basado en el tipo de datos `varchar` suministrado por el sistema.  
  
```sql  
CREATE TYPE SSN  
FROM varchar(11) NOT NULL ;  
```  
  
### <a name="b-creating-a-user-defined-type"></a>B. Crear un tipo definido por el usuario  
 En el siguiente ejemplo se crea un tipo `Utf8String` que hace referencia a la clase `utf8string` del ensamblado `utf8string`. Antes de crear el tipo, se registra el ensamblado `utf8string` en la base de datos local. Reemplace la parte binaria de la instrucción CREATE ASSEMBLY con una descripción válida.  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql  
CREATE ASSEMBLY utf8string  
AUTHORIZATION [dbi]   
FROM 0x4D... ;  
GO  
CREATE TYPE Utf8String   
EXTERNAL NAME utf8string.[Microsoft.Samples.SqlServer.utf8string] ;  
GO  
```  
  
### <a name="c-creating-a-user-defined-table-type"></a>C. Crear un tipo de tabla definido por el usuario  
 En el siguiente ejemplo se crea un tipo de tabla definido por el usuario con dos columnas. Para más información sobre cómo crear y usar parámetros con valores de tabla, vea[Usar parámetros con valores de tabla &#40;motor de base de datos&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
```sql  
CREATE TYPE LocationTableType AS TABLE   
    ( LocationName VARCHAR(50)  
    , CostRate INT );  
GO  
```  

### <a name="d-creating-a-user-defined-table-type-with-primary-key-and-index"></a>D. Creación de un tipo de tabla definido por el usuario con el índice y la clave principal
En el ejemplo siguiente se crea un tipo de tabla definido por el usuario que tiene tres columnas, una de las cuales (`Name`) es la clave principal y la otra (`Price`) tiene un índice no agrupado.  Para más información sobre cómo crear y usar parámetros con valores de tabla, vea[Usar parámetros con valores de tabla &#40;motor de base de datos&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).

```sql
CREATE TYPE InventoryItem AS TABLE
(
    [Name] NVARCHAR(50) NOT NULL,
    SupplierId BIGINT NOT NULL,
    Price DECIMAL (18, 4) NULL,
    PRIMARY KEY (
        Name
    ),
    INDEX IX_InventoryItem_Price (
        Price
    )
)
GO
```
  
## <a name="see-also"></a>Consulte también  
 [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)   
 [DROP TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)    
 [Tipos definidos por el usuario CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)     
 [Trabajar con tipos definidos por el usuario en SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)     
