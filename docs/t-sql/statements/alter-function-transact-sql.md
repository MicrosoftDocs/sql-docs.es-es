---
description: ALTER FUNCTION (Transact-SQL)
title: ALTER FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_FUNCTION_TSQL
- ALTER FUNCTION
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER FUNCTION statement
- modifying functions
- functions [SQL Server], modifying
ms.assetid: 89f066ee-05ac-4439-ab04-d8c3d5911179
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 60bba3f7a4ed2f4efd816c1fa46b0098005be270
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204185"
---
# <a name="alter-function-transact-sql"></a>ALTER FUNCTION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-pdw-md.md)]

  Modifica una función [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR existente, creada anteriormente por medio de la ejecución de la instrucción CREATE FUNCTION, sin cambiar los permisos y sin que afecte a ninguna otra función, procedimiento almacenado o desencadenador dependiente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Transact-SQL Scalar Function Syntax    
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]
```  

```syntaxsql
-- Transact-SQL Inline Table-Valued Function Syntax
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS TABLE  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    RETURN [ ( ] select_stmt [ ) ]  
[ ; ]  
```
  
```syntaxsql
-- Transact-SQL Multistatement Table-valued Function Syntax
ALTER FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS @return_variable TABLE <table_type_definition>  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN  
    END  
[ ; ]  
```

```syntaxsql
-- Transact-SQL Function Clauses   
<function_option>::=   
{  
    [ ENCRYPTION ]  
  | [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
  | [ EXECUTE_AS_Clause ]  
} 

<table_type_definition>:: =   
( { <column_definition> <column_constraint>   
  | <computed_column_definition> }   
    [ <table_constraint> ] [ ,...n ]  
)   
<column_definition>::=  
{  
    { column_name data_type }  
    [ [ DEFAULT constant_expression ]   
      [ COLLATE collation_name ] | [ ROWGUIDCOL ]  
    ]  
    | [ IDENTITY [ (seed , increment ) ] ]  
    [ <column_constraint> [ ...n ] ]   
}  

<column_constraint>::=   
{  
    [ NULL | NOT NULL ]   
    { PRIMARY KEY | UNIQUE }  
      [ CLUSTERED | NONCLUSTERED ]   
        [ WITH FILLFACTOR = fillfactor   
        | WITH ( < index_option > [ , ...n ] )  
      [ ON { filegroup | "default" } ]  
  | [ CHECK ( logical_expression ) ] [ ,...n ]  
}  
  
<computed_column_definition>::=  
column_name AS computed_column_expression   
  
<table_constraint>::=  
{   
    { PRIMARY KEY | UNIQUE }  
      [ CLUSTERED | NONCLUSTERED ]   
      ( column_name [ ASC | DESC ] [ ,...n ] )  
        [ WITH FILLFACTOR = fillfactor   
        | WITH ( <index_option> [ , ...n ] )  
  | [ CHECK ( logical_expression ) ] [ ,...n ]  
}  
  
<index_option>::=  
{   
    PAD_INDEX = { ON | OFF }   
  | FILLFACTOR = fillfactor   
  | IGNORE_DUP_KEY = { ON | OFF }  
  | STATISTICS_NORECOMPUTE = { ON | OFF }   
  | ALLOW_ROW_LOCKS = { ON | OFF }  
  | ALLOW_PAGE_LOCKS ={ ON | OFF }   
}  
```
  
```syntaxsql
-- CLR Scalar and Table-Valued Function Syntax
ALTER FUNCTION [ schema_name. ] function_name   
( { @parameter_name [AS] [ type_schema_name. ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
)  
RETURNS { return_data_type | TABLE <clr_table_type_definition> }  
    [ WITH <clr_function_option> [ ,...n ] ]  
    [ AS ] EXTERNAL NAME <method_specifier>  
[ ; ]  
```
  
```syntaxsql
-- CLR Function Clauses
<method_specifier>::=  
    assembly_name.class_name.method_name  
 
  
<clr_function_option>::=  
}  
    [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
  | [ EXECUTE_AS_Clause ]  
}  
  
<clr_table_type_definition>::=   
( { column_name data_type } [ ,...n ] )  
  
```  
  
```syntaxsql
-- Syntax for In-Memory OLTP: Natively compiled, scalar user-defined function  
ALTER FUNCTION [ schema_name. ] function_name    
 ( [ { @parameter_name [ AS ][ type_schema_name. ] parameter_data_type   
    [ NULL | NOT NULL ] [ = default ] }   
    [ ,...n ]   
  ]   
)   
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]   
    [ AS ]   
    BEGIN ATOMIC WITH (set_option [ ,... n ])  
        function_body   
        RETURN scalar_expression  
    END  
    
<function_option>::=   
{ |  NATIVE_COMPILATION   
  |  SCHEMABINDING   
  | [ EXECUTE_AS_Clause ]   
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]   
}  
```   
  
## <a name="arguments"></a>Argumentos  
 *schema_name*  
 Nombre del esquema al que pertenece la función definida por el usuario.  
  
 *function_name*  
 Es la función definida por el usuario que se va a cambiar.  
  
> [!NOTE]  
>  Los paréntesis después del nombre de la función son necesarios, aunque no se especifique un parámetro.  
  
 **@** _parameter_name_  
 Es un parámetro de la función definida por el usuario. Es posible declarar uno o varios parámetros.  
  
 Una función puede tener un máximo de 2.100 parámetros. El usuario debe proporcionar el valor de cada parámetro declarado cuando se ejecuta la función, a menos que se defina un valor predeterminado para el parámetro.  
  
 Especifique un nombre de parámetro con una arroba ( **@** ) como primer carácter. El nombre del parámetro debe cumplir las mismas reglas para [identifiers](../../relational-databases/databases/database-identifiers.md). Los parámetros son locales para la función; los mismos nombres de parámetro se pueden utilizar en otras funciones. Los parámetros solamente pueden ocupar el lugar de constantes; no se pueden utilizar en lugar de nombres de tablas, nombres de columnas o nombres de otros objetos de base de datos.  
  
> [!NOTE]  
>  No se respeta ANSI_WARNINGS al pasar parámetros de un procedimiento almacenado, una función definida por el usuario o al declarar y establecer variables en una instrucción de lote. Por ejemplo, si una variable se define como **char(3)** y después se establece en un valor de más de tres caracteres, los datos se truncan hasta el tamaño definido y la instrucción INSERT o UPDATE se ejecuta correctamente.  
  
 [ *type_schema_name.* ] *parameter_data_type*  
 Es el tipo de datos del parámetro y, de forma opcional, el esquema al que pertenece. Para las funciones [!INCLUDE[tsql](../../includes/tsql-md.md)], se permiten todos los tipos de datos, incluidos los tipos definidos por el usuario CLR, a excepción del tipo de datos **timestamp**. Para las funciones CLR, se permiten todos los tipos de datos, incluidos los tipos CLR definidos por el usuario, a excepción de los tipos de datos **text**, **ntext**, **image** y **timestamp**. Los tipos de datos no escalares, **cursor** y **table**, no se pueden especificar como tipos de datos de parámetro en funciones [!INCLUDE[tsql](../../includes/tsql-md.md)] ni en funciones CLR.  
  
 Si no se especifica *type_schema_name*, [!INCLUDE[ssDEversion2005](../../includes/ssdeversion2005-md.md)] busca *parameter_data_type* en este orden:  
  
-   El esquema que contiene los nombres de los tipos de datos del sistema de SQL Server.  
  
-   El esquema predeterminado del usuario actual en la base de datos actual.  
  
-   El esquema **dbo** de la base de datos actual.  
  
 [ **=** _default_ ]  
 Es un valor predeterminado para el parámetro. Si se define un valor *default*, la función se puede ejecutar sin especificar un valor para ese parámetro.  
  
> [!NOTE]  
>  Se pueden especificar valores predeterminados de parámetros para las funciones CLR, excepto para los tipos de datos **varchar(max)** y **varbinary(max)** .  
  
 Cuando un parámetro de la función tiene un valor predeterminado, se debe especificar la palabra clave DEFAULT al llamar a la función para recuperar el valor predeterminado. Este comportamiento es distinto del uso de parámetros con valores predeterminados en los procedimientos almacenados, donde la omisión del parámetro implica especificar el valor predeterminado.  
  
 *return_data_type*  
 Es el valor devuelto de una función escalar definida por el usuario. Para las funciones [!INCLUDE[tsql](../../includes/tsql-md.md)], se permiten todos los tipos de datos, incluidos los tipos definidos por el usuario CLR, a excepción del tipo de datos **timestamp**. Para las funciones CLR, se permiten todos los tipos de datos, incluidos los tipos CLR definidos por el usuario, a excepción de los tipos de datos **text**, **ntext**, **image** y **timestamp**. Los tipos de datos no escalares, **cursor** y **table**, no se pueden especificar como tipos de datos devueltos en funciones [!INCLUDE[tsql](../../includes/tsql-md.md)] ni en funciones CLR.  
  
 *function_body*  
 Especifica que una serie de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)], que juntas no producen ningún efecto secundario (como modificar una tabla), definen el valor de la función. *function_body* solamente se usa en funciones escalares y en funciones con valores de tabla de múltiples instrucciones.  
  
 En las funciones escalares, *function_body* es una serie de instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se evalúan como un valor escalar.  
  
 En las funciones con valores de tabla de múltiples instrucciones, *function_body* es una serie de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que rellenan una variable devuelta de TABLE.  
  
 *scalar_expression*  
 Especifica que la función escalar devuelve un valor escalar.  
  
 TABLE  
 Especifica que el valor devuelto de la función con valores de tabla es una tabla. Solamente se pueden pasar constantes y **@** _local\_variables_ a las funciones con valores de tabla.  
  
 En las funciones insertadas con valores de tabla, el valor devuelto de TABLE se define mediante una única instrucción SELECT. Las funciones insertadas no tienen variables devueltas asociadas.  
  
 En las funciones con valores de tabla de múltiples instrucciones, **@** _return\_variable_ es una variable de TABLE, que se usa para almacenar y acumular las filas que se deben devolver como valor de la función. **@** _return\_variable_ solamente se puede especificar para funciones [!INCLUDE[tsql](../../includes/tsql-md.md)], no para funciones CLR.  
  
 *select-stmt*  
 Es la instrucción SELECT individual que define el valor devuelto de una función insertada con valores de tabla.  
  
 EXTERNAL NAME \<method_specifier>*nombre_ensamblado.nombre_clase*.*nombre_método*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica el método de un ensamblado que se enlaza a la función. *assembly_name* debe coincidir con un ensamblado existente en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos actual con la visibilidad activada. *class_name* debe ser un identificador [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] válido y debe existir como clase en el ensamblado. Si la clase tiene un nombre completo de espacio de nombres que usa un punto ( **.** ) para separar las partes del espacio de nombres, el nombre de la clase debe delimitarse mediante paréntesis ( **[]** ) o comillas ( **""** ). *method_name* debe ser un identificador [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] válido y debe existir como método estático en la clase especificada.  
  
> [!NOTE]  
>  De manera predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede ejecutar código CLR. Se pueden crear, modificar y quitar objetos de base de datos que hagan referencia a módulos de Common Language Runtime, pero estas referencias no se pueden ejecutar en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hasta que se habilite la [opción clr enabled](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md). Para habilitar esta opción, use [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 _\<_table\_type\_definition_\>_ **(** { \<column_definition\> \<column\_constraint\> | \<computed\_column\_definition\> } [ \<table\_constraint\> ] [ **,** ...*n* ] **)**  
 Define el tipo de datos de tabla para una función [!INCLUDE[tsql](../../includes/tsql-md.md)]. La declaración de tabla incluye definiciones de columna y restricciones de columna o de tabla.  
  
\< clr_table_type_definition \> **(** { *nombre_columna**tipo_datos* } [ **,** ...*n* ] **)** **Se aplica a**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] ([Versión preliminar en algunas regiones](/azure/azure-sql/database/features-comparison?WT.mc_id=TSQL_GetItTag)).  
  
 Define los tipos de datos de tabla para una función CLR. La declaración de tabla solamente incluye nombres de columna y tipos de datos.  
  
 NULL|NOT NULL  
 Solo admite funciones escalares definidas por el usuario compiladas de forma nativa. Para obtener más información, vea [Funciones escalares definidas por el usuario para OLTP en memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
 NATIVE_COMPILATION  
 Indica si una función definida por el usuario se compila de forma nativa. Este argumento es obligatorio en funciones escalares definidas por el usuario compiladas de forma nativa.  
  
 El argumento NATIVE_COMPILATION es necesario para modificar la función y solo se puede usar si la función se ha creado con el argumento NATIVE_COMPILATION.  
  
 BEGIN ATOMIC WITH  
 Solo admite funciones escalares definidas por el usuario compiladas de forma nativa y es obligatorio. Para obtener más información, consulte [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md).  
  
 SCHEMABINDING  
 El argumento SCHEMABINDING se requiere para las funciones escalares definidas por el usuario compiladas de forma nativa.  
  
 **\<function_option>::= y \<clr_function_option>::=**  
  
 Especifica si la función tendrá una o más de las siguientes opciones.  
  
 ENCRYPTION  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Indica que [!INCLUDE[ssDE](../../includes/ssde-md.md)] cifra las columnas de vista de catálogo que contienen el texto de la instrucción ALTER FUNCTION. El uso de ENCRYPTION impide que la función se publique como parte de la replicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. ENCRYPTION no se puede especificar para funciones CLR.  
  
 SCHEMABINDING  
 Especifica que la función está enlazada a los objetos de base de datos a los que hace referencia. Cuando se especifica SCHEMABINDING, los objetos base no se pueden modificar de una forma que afecte a la definición de la función. En primer lugar, se debe modificar o quitar la propia definición de la función para quitar las dependencias en el objeto que se va a modificar.  
  
 El enlace de la función a los objetos a los que hace referencia solamente se quita cuando se ejecuta una de estas acciones:  
  
-   Se quita la función.  
  
-   La función se modifica con la instrucción ALTER sin especificar la opción SCHEMABINDING.  
  
Para obtener una lista de las condiciones que se deben cumplir para poder enlazar un esquema a la función, vea [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md).  
  
 RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT  
 Especifica el atributo **OnNULLCall** de una función con valores escalares. Si no se especifica, se utiliza CALLED ON NULL INPUT de manera predeterminada. Esto significa que el cuerpo de la función se ejecuta aunque se envíe NULL como argumento.  
  
 Si se especifica RETURNS NULL ON NULL INPUT en una función CLR, esto indica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede devolver NULL cuando cualquiera de los argumentos que recibe sea NULL, sin invocar realmente el cuerpo de la función. Si el método especificado en \<method_specifier> ya tiene un atributo personalizado que indica RETURNS NULL ON NULL INPUT, pero la instrucción ALTER FUNCTION indica CALLED ON NULL INPUT, la instrucción ALTER FUNCTION tiene prioridad. El atributo **OnNULLCall** no se puede especificar para las funciones CLR con valores de tabla.  
  
 Cláusula EXECUTE AS  
 Especifica el contexto de seguridad en el que se ejecuta la función definida por el usuario. Por lo tanto, es posible controlar la cuenta de usuario que utiliza [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para validar los permisos en los objetos de base de datos a los que hace referencia la función.  
  
> [!NOTE]  
>  EXECUTE AS no se puede especificar para las funciones insertadas definidas por el usuario.  
  
 Para obtener más información, vea [EXECUTE AS &#40;cláusula de Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
**\< column_definition >::=**
  
 Define el tipo de datos de tabla. La declaración de tabla incluye definiciones de columna y restricciones. En las funciones CLR, solo se pueden especificar *column_name* y *data_type*.  
  
 *column_name*  
 Es el nombre de una columna de la tabla. Los nombres de columna se deben ajustar a las reglas para los identificadores y deben ser únicos en la tabla. *column_name* puede tener entre 1 y 128 caracteres.  
  
 *data_type*  
 Especifica el tipo de datos de la columna. En las funciones [!INCLUDE[tsql](../../includes/tsql-md.md)], se permiten todos los tipos de datos, incluidos los tipos CLR definidos por el usuario, a excepción de **timestamp**. Para las funciones CLR, se permiten todos los tipos de datos, incluidos los tipos CLR definidos por el usuario, a excepción de **text**, **ntext**, **image**, **char**, **varchar**, **varchar(max)** y **timestamp**. El tipo no escalar **cursor** no se puede especificar como tipo de datos de columna en funciones [!INCLUDE[tsql](../../includes/tsql-md.md)] o CLR.  
  
 DEFAULT *constant_expression*  
 Especifica el valor suministrado para la columna cuando no se ha especificado explícitamente un valor durante una inserción. *constant_expression* es una constante, un valor NULL o un valor de función del sistema. Se pueden aplicar definiciones con el valor DEFAULT a cualquier columna, excepto las que incluyen la propiedad IDENTITY. No se puede especificar DEFAULT para las funciones CLR con valores de tabla.  
  
 COLLATE *collation_name*  
 Especifica la intercalación de la columna. Si no se especifica, se asigna a la columna la intercalación predeterminada de la base de datos. El nombre de intercalación puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Para obtener una lista y más información, vea [Nombre de intercalación de Windows &#40;Transact-SQL&#41;](../../t-sql/statements/windows-collation-name-transact-sql.md) y [Nombre de intercalación de SQL Server &#40;Transact-SQL&#41;](../../t-sql/statements/sql-server-collation-name-transact-sql.md).  
  
 La cláusula COLLATE se puede usar para cambiar únicamente las intercalaciones de las columnas con tipos de datos **char**, **varchar**, **nchar** y **nvarchar**.  
  
 No se puede especificar COLLATE para las funciones con valores de tabla CLR.  
  
 ROWGUIDCOL  
 Indica que la nueva columna es una columna de identificador único global de fila. Solo se puede designar una columna **uniqueidentifier** por tabla como columna ROWGUIDCOL. La propiedad ROWGUIDCOL únicamente se puede asignar a una columna **uniqueidentifier**.  
  
 La propiedad ROWGUIDCOL no exige que los valores almacenados en la columna sean únicos. Del mismo modo, tampoco genera automáticamente valores para nuevas filas insertadas en la tabla. Si desea generar valores únicos para cada columna, use la función NEWID en instrucciones INSERT. Puede especificar un valor predeterminado; sin embargo, no puede especificar NEWID como valor predeterminado.  
  
 IDENTITY  
 Indica que la nueva columna es una columna de identidad. Cuando se agrega una nueva fila a la tabla, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona un valor incremental único para la columna. Las columnas de identidad se utilizan normalmente junto con las restricciones PRIMARY KEY como identificadores de fila exclusivos de la tabla. La propiedad IDENTITY se puede asignar a columnas **tinyint**, **smallint**, **int**, **bigint**, **decimal(p,0)** o **numeric(p,0)** . Solo se puede crear una columna de identidad para cada tabla. Las restricciones DEFAULT y los valores predeterminados enlazados no se pueden utilizar en las columnas de identidad. Se debe especificar los dos argumentos, *seed* e *increment*, o ninguno. Si no se especifica ninguno, el valor predeterminado es (1,1).  
  
 No se puede especificar IDENTITY para las funciones CLR con valores de tabla.  
  
 *seed*  
 Se trata del valor entero que se asignará a la primera fila de la tabla.  
  
 *increment*  
 Se trata del incremento que se debe agregar al valor de *seed* en las sucesivas filas de la tabla.  
  
**\< column_constraint >::= y \< table_constraint>::=**
  
 Define la restricción para la columna o tabla especificada. Para las funciones CLR, el único tipo de restricción permitido es NULL. No se permiten las restricciones con nombre.  
  
 NULL | NOT NULL  
 Determina si se permiten valores NULL en la columna. NULL no es estrictamente una restricción, pero se puede especificar, al igual que NOT NULL. No se puede especificar NOT NULL para las funciones CLR con valores de tabla.  
  
 PRIMARY KEY  
 Es una restricción que exige la integridad de entidad para la columna especificada a través de un índice único. En las funciones con valores de tabla definidas por el usuario, la restricción PRIMARY KEY solamente se puede crear en una columna de cada tabla. No se puede especificar PRIMARY KEY para las funciones CLR con valores de tabla.  
  
 UNIQUE  
 Es una restricción que proporciona la integridad de entidad para una o varias columnas especificadas a través de un índice único. Las tablas pueden tener varias restricciones UNIQUE. No se puede especificar UNIQUE para las funciones CLR con valores de tabla.  
  
 CLUSTERED | NONCLUSTERED  
 Indica que se ha creado un índice clúster o no clúster para la restricción PRIMARY KEY o UNIQUE. Las restricciones PRIMARY KEY utilizan CLUSTERED y las restricciones UNIQUE, NONCLUSTERED.  
  
 CLUSTERED solamente se puede especificar para una restricción. Si se especifica CLUSTERED para una restricción UNIQUE y también se especifica una restricción PRIMARY KEY, esta última utilizará NONCLUSTERED.  
  
 No se pueden especificar CLUSTERED y NONCLUSTERED para las funciones CLR con valores de tabla.  
  
 CHECK  
 Es una restricción que exige la integridad del dominio al limitar los valores posibles que se pueden escribir en una o varias columnas. No se pueden especificar restricciones CHECK para las funciones CLR con valores de tabla.  
  
 *logical_expression*  
 Es una expresión lógica que devuelve TRUE o FALSE.  
  
 **\<computed_column_definition>::=**  
  
 Especifica una columna calculada. Para obtener más información sobre las columnas calculadas, consulte [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).  
  
 *column_name*  
 Es el nombre de la columna calculada.  
  
 *computed_column_expression*  
 Es una expresión que define el valor de una columna calculada.  
  
 **\<index_option>::=**  
  
 Especifica las opciones de índice para el índice PRIMARY KEY o UNIQUE. Para más información sobre las opciones de índice, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
 PAD_INDEX = { ON | OFF }  
 Especifica el relleno del índice. El valor predeterminado es OFF.  
  
 FILLFACTOR = *fillfactor*  
 Especifica un porcentaje que indica cuánto debe llenar el [!INCLUDE[ssDE](../../includes/ssde-md.md)] el nivel hoja de cada página de índice durante la creación o modificación de los índices. *fillfactor* debe ser un valor entero comprendido entre 1 y 100. El valor predeterminado es 0.  
  
 IGNORE_DUP_KEY = { ON | OFF }  
 Especifica la respuesta de error cuando una operación de inserción intenta insertar valores de clave duplicados en un índice único. La opción IGNORE_DUP_KEY se aplica solamente a operaciones de inserción realizadas tras crear o volver a generar el índice. El valor predeterminado es OFF.  
  
 STATISTICS_NORECOMPUTE = { ON | OFF }  
 Especifica si se vuelven a calcular las estadísticas de distribución. El valor predeterminado es OFF.  
  
 ALLOW_ROW_LOCKS = { ON | OFF }  
 Especifica si se permiten los bloqueos de fila. El valor predeterminado es ON.  
  
 ALLOW_PAGE_LOCKS = { ON | OFF }  
 Especifica si se permiten bloqueos de página. El valor predeterminado es ON.  
  
## <a name="remarks"></a>Observaciones  
 No es posible utilizar ALTER FUNCTION para cambiar una función escalar por una función con valores de tabla, ni viceversa. Tampoco es posible utilizar ALTER FUNCTION para cambiar una función insertada por una función de múltiples instrucciones o viceversa. No se puede utilizar ALTER FUNCTION para cambiar una función [!INCLUDE[tsql](../../includes/tsql-md.md)] por una función CLR o viceversa.  
  
 Las siguientes instrucciones de Service Broker no se pueden incluir en la definición de una función [!INCLUDE[tsql](../../includes/tsql-md.md)] definida por el usuario:  
  
-   EMPEZAR CONVERSACIÓN DE DIÁLOGO  
-   FINALIZAR CONVERSACIÓN  
-   GET CONVERSATION GROUP  
-   MOVE CONVERSATION  
-   RECEIVE  
-   ENVIAR  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER para la función o para el esquema. Si la función especifica un tipo definido por el usuario, requiere el permiso EXECUTE para ese tipo.  
  
## <a name="see-also"></a>Consulte también  
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)   
 [Realizar cambios de esquema en bases de datos de publicaciones](../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
