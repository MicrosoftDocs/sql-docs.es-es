---
description: EXECUTE (Transact-SQL)
title: EXECUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EXEC
- EXECUTE_TSQL
- EXECUTE
- EXEC_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote stored procedures [SQL Server]
- command strings [SQL Server]
- extended stored procedures [SQL Server], executing
- stored procedures [SQL Server], executing
- Transact-SQL statements, executing
- strings [SQL Server], executing
- statements [SQL Server], executing
- context switching [SQL Server], execution context
- user-defined functions [SQL Server], executing
- character strings [SQL Server], executing
- switching execution context
- EXECUTE statement
ms.assetid: bc806b71-cc55-470a-913e-c5f761d5c4b7
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8713080c2b234c55fb81227e9a7f8823abf66691
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342599"
---
# <a name="execute-transact-sql"></a>EXECUTE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Ejecuta una cadena de comandos o una cadena de caracteres dentro de un lote de [!INCLUDE[tsql](../../includes/tsql-md.md)] o uno de los siguientes módulos: procedimiento almacenado del sistema, procedimiento almacenado definido por el usuario, procedimiento almacenado CLR, función escalar definida por el usuario o procedimiento almacenado extendido. La instrucción EXECUTE se puede usar para enviar comandos de paso a través a los servidores vinculados. Adicionalmente, el contexto en el que se ejecuta una cadena o un comando se puede establecer de forma explícita. Los metadatos para el conjunto de resultados se pueden definir usando las opciones de WITH RESULT SETS.
  
> [!IMPORTANT]  
>  Antes de llamar a EXECUTE con una cadena de caracteres, valide la cadena de caracteres. Nunca ejecute un comando construido desde la entrada de usuario que no se haya validado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  

::: moniker range=">=sql-server-ver15"
En el siguiente bloque de código se muestra la sintaxis de SQL Server 2019. Como alternativa, vea [Sintaxis de SQL Server 2017 y anterior](execute-transact-sql.md?view=sql-server-2017&preserve-view=true) en su lugar. 

```syntaxsql
-- Syntax for SQL Server 2019
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name [ ;number ] | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH <execute_option> [ ,...n ] ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS { LOGIN | USER } = ' name ' ]  
[;]  
  
Execute a pass-through command against a linked server  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'command_string [ ? ]' } [ + ...n ]  
        [ { , { value | @variable [ OUTPUT ] } } [ ...n ] ]  
    )   
    [ AS { LOGIN | USER } = ' name ' ]  
    [ AT linked_server_name ]  
    [ AT DATA_SOURCE data_source_name ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML   
}  
```  
::: moniker-end

::: moniker range=">=sql-server-2016"

En el siguiente bloque de código se muestra la sintaxis de SQL Server 2017 y anterior. Como alternativa, vea [Sintaxis de SQL Server 2019](execute-transact-sql.md?view=sql-server-ver15&preserve-view=true) en su lugar.


```syntaxsql
-- Syntax for SQL Server 2017 and earleir  
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name [ ;number ] | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH <execute_option> [ ,...n ] ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS { LOGIN | USER } = ' name ' ]  
[;]  
  
Execute a pass-through command against a linked server  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'command_string [ ? ]' } [ + ...n ]  
        [ { , { value | @variable [ OUTPUT ] } } [ ...n ] ]  
    )   
    [ AS { LOGIN | USER } = ' name ' ]  
    [ AT linked_server_name ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML   
}  
```  
::: moniker-end

  
```syntaxsql
-- In-Memory OLTP   

Execute a natively compiled, scalar user-defined function  
[ { EXEC | EXECUTE } ]   
    {   
      [ @return_status = ]   
      { module_name | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable   
                           | [ DEFAULT ]   
                           }  
        ]   
      [ ,...n ]   
      [ WITH <execute_option> [ ,...n ] ]   
    }  
<execute_option>::=  
{  
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}  
```  
  
```syntaxsql
-- Syntax for Azure SQL Database   
  
Execute a stored procedure or function  
[ { EXEC | EXECUTE } ]  
    {   
      [ @return_status = ]  
      { module_name  | @module_name_var }   
        [ [ @parameter = ] { value   
                           | @variable [ OUTPUT ]   
                           | [ DEFAULT ]   
                           }  
        ]  
      [ ,...n ]  
      [ WITH RECOMPILE ]  
    }  
[;]  
  
Execute a character string  
{ EXEC | EXECUTE }   
    ( { @string_variable | [ N ]'tsql_string' } [ + ...n ] )  
    [ AS {  USER } = ' name ' ]  
[;]  
  
<execute_option>::=  
{  
        RECOMPILE   
    | { RESULT SETS UNDEFINED }   
    | { RESULT SETS NONE }   
    | { RESULT SETS ( <result_sets_definition> [,...n ] ) }  
}   
  
<result_sets_definition> ::=   
{  
    (  
         { column_name   
           data_type   
         [ COLLATE collation_name ]   
         [ NULL | NOT NULL ] }  
         [,...n ]  
    )  
    | AS OBJECT   
        [ db_name . [ schema_name ] . | schema_name . ]   
        {table_name | view_name | table_valued_function_name }  
    | AS TYPE [ schema_name.]table_type_name  
    | AS FOR XML  
  
```

  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Execute a stored procedure  
[ { EXEC | EXECUTE } ]  
    procedure_name   
        [ { value | @variable [ OUT | OUTPUT ] } ] [ ,...n ] }  
[;]  
  
-- Execute a SQL string  
{ EXEC | EXECUTE }  
    ( { @string_variable | [ N ] 'tsql_string' } [ +...n ] )  
[;]  
```  


  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 @*return_status*  
 Es una variable entera opcional que almacena el estado de retorno de un módulo. Esta variable debe declararse en el proceso por lotes, en el procedimiento almacenado o en la función para que se pueda utilizar en una instrucción EXECUTE.  
  
 Cuando se usa para invocar una función escalar definida por el usuario, la variable @*return_status* puede ser de cualquier tipo de datos escalar.  
  
 *module_name*  
 Es el nombre, completo o no, del procedimiento almacenado o la función escalar definida por el usuario a la que debe llamar. Los nombres de módulo deben cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md). Los nombres de los procedimientos almacenados extendidos distinguen siempre entre mayúsculas y minúsculas, sin tener en cuenta la intercalación del servidor.  
  
 Un módulo que se haya creado en otra base de datos se puede ejecutar si el usuario que lo ejecuta es el propietario del módulo o dispone de los permisos adecuados para ejecutar el módulo en esa base de datos. Un módulo puede ejecutarse en otro servidor que esté ejecutando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si el usuario que ejecuta el módulo tiene los permisos adecuados para utilizar ese servidor (acceso remoto) y para ejecutar el módulo en dicha base de datos. Si se especifica un nombre de servidor, pero no se especifica nombre de base de datos, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] busca el módulo en la base de datos predeterminada del usuario.  
  
 ;*number*  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Es un entero opcional que se utiliza para agrupar procedimientos que tengan el mismo nombre. Este parámetro no se utiliza para los procedimientos almacenados extendidos.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Para más información sobre los grupos de procedimientos, vea [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md).  
  
 @*module_name_var*  
 Es el nombre de la variable definida localmente, que representa el nombre de un módulo.  
  
 Puede ser una variable que contiene el nombre de una función escalar definida por el usuario y compilada de forma nativa.  
  
 @*parameter*  
 Es el parámetro de *module_name*, como se define en el módulo. Los nombres de parámetro deben precederse del signo (@). Cuando se usa con el formato @*parameter_name*=*value*, los nombres de parámetro y las constantes no tienen que proporcionarse en el orden en el que se han definido en el módulo. Sin embargo, si el formato @*parameter_name*=*value* se usa para cualquier parámetro, se debe usar para todos los parámetros siguientes.  
  
 De manera predeterminada, los parámetros admiten valores NULL.  
  
 *value*  
 Es el valor del parámetro que se va a pasar al módulo o a un comando de paso a través. Si no se especifican los nombres de los parámetros, sus valores deben proporcionarse en el orden definido en el módulo.  
  
 Cuando se ejecutan comandos de paso a través contra servidores vinculados, el orden de los valores de los parámetros depende del proveedor OLE DB del servidor vinculado. La mayoría de proveedores OLE DB enlazan valores a parámetros de izquierda a derecha.  
  
 Si el valor de un parámetro es un nombre de objeto o cadena de caracteres, o está calificado mediante un nombre de base de datos o nombre de esquema, el nombre completo debe escribirse entre comillas simples. Si el valor de un parámetro es una palabra clave, ésta debe escribirse entre comillas dobles.  
  
Si pasa una sola palabra que no comienzan por `@` y que no está incluida entre comillas (por ejemplo, si se olvida `@` en un nombre de parámetro) la palabra se trata como una cadena nvarchar, a pesar de que faltan las comillas.

 Si se define un valor predeterminado en el módulo, un usuario podrá ejecutar el módulo sin especificar ningún parámetro.  
  
 El valor predeterminado puede ser también NULL. Generalmente, la definición del módulo especifica la acción que debe realizarse si el valor del parámetro es NULL.  
  
 @*variable*  
 Es la variable que almacena un parámetro o un parámetro devuelto.  
  
 OUTPUT  
 Especifica que el módulo o la cadena de comandos devuelve un parámetro. El parámetro coincidente del módulo o de la cadena de comandos debe haberse creado también con la palabra clave OUTPUT. Utilice esta palabra clave cuando use variables de cursor como parámetros.  
  
 Si *value* se define como OUTPUT de un módulo que se ejecuta contra un servidor vinculado, cualquier cambio realizado por el proveedor OLE DB en el @*parameter* correspondiente se volverá a copiar en la variable al final de la ejecución del módulo.  
  
 Si se usan parámetros OUTPUT y la intención es usar los valores devueltos en otras instrucciones del lote o módulo que llama, el valor del parámetro se debe pasar como variable, por ejemplo, @*parameter* = @*variable*. No se puede ejecutar un módulo en el que se especifique OUTPUT para un parámetro que no se ha definido como parámetro OUTPUT en el módulo. Las constantes no se pueden pasar al módulo mediante OUTPUT; el parámetro devuelto requiere un nombre de variable. El tipo de datos de la variable debe estar declarado y se le debe haber asignado un valor para poder ejecutar el procedimiento.  
  
 Cuando se usa EXECUTE contra un procedimiento almacenado remoto, o se ejecuta un comando de paso a través contra un servidor vinculado, los parámetros OUTPUT no pueden ser ninguno de los tipos de datos de objeto grande (LOB).  
  
 Los parámetros devueltos pueden ser de cualquier tipo de datos, excepto del tipo de datos LOB.  
  
 DEFAULT  
 Proporciona el valor predeterminado del parámetro tal como se define en el módulo. Cuando el módulo espera un valor para un parámetro que no tiene un valor predeterminado definido y no se encuentra un parámetro o se ha especificado la palabra clave DEFAULT, se produce un error.  
  
 @*string_variable*  
 Es el nombre de una variable local. @*string_variable* puede ser cualquier tipo de datos **char**, **varchar**, **nchar** o **nvarchar**. Esto engloba los tipos de datos **(max)** .  
  
 [N] '*tsql_string*'  
 Es una cadena constante. *tsql_string* puede ser cualquier tipo de datos **nvarchar** o **varchar**. Si se incluye N, la cadena se interpreta como del tipo de datos **nvarchar**.  
  
 AS \<context_specification>.  
 Especifica el contexto en el que se ejecuta la instrucción.  
  
 LOGIN  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Especifica que el contexto que se va a suplantar es un inicio de sesión. El ámbito de la suplantación es el servidor.  
  
 USER  
 Especifica que el contexto de ejecución que se va a suplantar es un usuario de la base de datos actual. El ámbito de la suplantación se restringe a la base de datos actual. Un cambio de contexto a un usuario de base de datos no hereda los permisos de nivel de servidor de ese usuario.  
  
> [!IMPORTANT]  
>  Mientras el cambio de contexto al usuario de base de datos esté activo, cualquier intento de acceso a recursos fuera de la base de datos provocará que la instrucción genere errores. Esto incluye instrucciones USE *database*, consultas distribuidas y consultas que hacen referencia a otra base de datos por medio de identificadores de tres o cuatro partes.  
  
 '*name*'  
 Es un nombre válido de inicio de sesión o de usuario. *name* debe ser miembro del rol fijo de servidor o existir como entidad de seguridad en [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) o [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) respectivamente.  
  
 *name* no puede ser una cuenta integrada, como NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService o NT AUTHORITY\LocalSystem.  
  
 Para más información, vea [Especificar un nombre de inicio de sesión o usuario](#_user) más adelante en este tema.  
  
 [N] '*command_string*'  
 Es una cadena constante que contiene el comando que se va a pasar a través al servidor vinculado. Si se incluye N, la cadena se interpreta como del tipo de datos **nvarchar**.  
  
 [?]  
 Indica los parámetros para los que se suministran valores en el elemento \<arg-list> de los comandos de paso a través que se usan en una instrucción EXEC('...', \<arg-list>) AT \<linkedsrv>.  
  
 AT *linked_server_name*  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
 Especifica que *command_string* se ejecuta en *linked_server_name* y los resultados, si hay alguno, se devuelven al cliente. *linked_server_name* debe hacer referencia a una definición de servidor vinculado existente en el servidor local. Los servidores vinculados se definen por medio de [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
 WITH \<execute_option>  
 Opciones de ejecución posibles. Las opciones de RESULT SETS no se pueden especificar en una instrucción INSERT… EXEC.  
 
AT DATA_SOURCE data_source_name **Se aplica a**: [!INCLUDE[sssql19](../../includes/sssql19-md.md)] y posterior
  
 Especifica que *command_string* se ejecuta en *data_source_name* y los resultados, si los hubiera, se devuelven al cliente. *data_source_name* debe hacer referencia a una definición de EXTERNAL DATA SOURCE existente en la base de datos. Solo se admiten los orígenes de datos que apuntan a SQL Server. Además, en los orígenes de datos del clúster de macrodatos de SQL Server que apuntan al grupo de proceso, se admite el grupo de datos o el bloque de almacenamiento. Los orígenes de datos se definen mediante [CREATE EXTERNAL DATA SOURCE](../statements/create-external-data-source-transact-sql.md).  
  
 WITH \<execute_option>  
 Opciones de ejecución posibles. Las opciones de RESULT SETS no se pueden especificar en una instrucción INSERT… EXEC.  
  
|Término|Definición|  
|----------|----------------|  
|RECOMPILE|Fuerza que se compile, use y descarte un nuevo plan después de ejecutar el módulo. Si hay algún plan de consulta existente para el módulo, este plan permanece en la memoria caché.<br /><br /> Utilice esta opción si el parámetro que está proporcionando es atípico o si los datos han cambiado de forma significativa. Esta opción no se utiliza para los procedimientos almacenados extendidos. Se recomienda que use esta opción con cautela, porque es costosa.<br /><br /> **Nota:** No puede utilizarse WITH RECOMPILE para llamar a un procedimiento almacenado que utiliza la sintaxis OPENDATASOURCE. La opción WITH RECOMPILE se omite cuando se especifica un nombre de objeto de cuatro partes.<br /><br /> **Nota:** RECOMPILE no es compatible con las funciones escalares definidas por el usuario y compiladas de forma nativa. Si necesita volver a compilar, use [sp_recompile &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-recompile-transact-sql.md).|  
|**RESULT SETS UNDEFINED**|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /><br /> Esta opción no proporciona ninguna garantía de que se devuelvan los resultados, si hay alguno, y no se proporciona ninguna definición. La instrucción se ejecuta sin errores si se devuelve algún resultado o si no se devuelve ninguno. RESULT SETS UNDEFINED es el comportamiento predeterminado si no se proporciona una opción de conjuntos de resultados.<br /><br /> En el caso de las funciones escalares definidas por el usuario tanto interpretadas como compiladas de forma nativa, esta opción no está operativa porque dichas funciones no devuelven nunca un conjunto de resultados.|  
|RESULT SETS NONE|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /><br /> Garantiza que la instrucción de ejecución no devolverá ningún resultado. Si se devuelve algún resultado, se anula el lote.<br /><br /> En el caso de las funciones escalares definidas por el usuario tanto interpretadas como compiladas de forma nativa, esta opción no está operativa porque dichas funciones no devuelven nunca un conjunto de resultados.|  
|*\<result_sets_definition>*|**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]<br /><br /> Proporciona una garantía de que el resultado se devolverá tal y como se especifique en result_sets_definition. En el caso de instrucciones que devuelvan varios conjuntos de resultados, proporcione varias secciones *result_sets_definition*. Agregue cada *result_sets_definition* entre paréntesis, separados por comas. Para obtener más información, vea \<result_sets_definition> más adelante en este tema.<br /><br /> Esta opción siempre genera un error con las funciones escalares definidas por el usuario y compiladas de forma nativa, porque este tipo de funciones no devuelve nunca un conjunto de resultados.|
  
\<result_sets_definition>
**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y posterior, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
 Describe los conjuntos de resultados devueltos por las instrucciones ejecutadas. Las cláusulas de result_sets_definition tienen el significado siguiente  
  
|Término|Definición|  
|----------|----------------|  
|{<br /><br /> column_name<br /><br /> data_type<br /><br /> [ COLLATE collation_name]<br /><br /> [NULL &#124; NOT NULL]<br /><br /> }|Vea la tabla siguiente.|  
|db_name|El nombre de la base de datos que contiene la tabla, vista o función con valores de tabla.|  
|schema_name|El nombre del esquema propietario de la tabla, vista o función con valores de tabla.|  
|table_name &#124; view_name &#124; table_valued_function_name|Especifica que las columnas devueltas serán aquellas especificadas en la tabla, vista o función con valores de tabla con nombre. Las variables de tabla, tablas temporales y sinónimos no se admiten en la sintaxis de objeto AS.|  
|AS TYPE [schema_name.]table_type_name|Especifica que las columnas devueltas serán aquellas especificadas en el tipo de tabla.|  
|AS FOR XML|Especifica que los resultados XML de la instrucción o el procedimiento almacenado llamado por la instrucción EXECUTE se convertirán al formato como si se hubieran generado mediante una instrucción SELECT ... FOR XML …. Todo el formato de las directivas de tipo en la instrucción original se quita y los resultados devueltos son como si no se hubiera especificado ninguna directiva de tipo. AS FOR XML no convierte los resultados tabulares que no son XML de la instrucción o el procedimiento almacenado ejecutado en XML.|  
  
|Término|Definición|  
|----------|----------------|  
|column_name|Los nombres de cada columna. Si el número de columnas es diferente al del conjunto de resultados, se produce un error y se anula el lote. Si el nombre de una columna es diferente al del conjunto de resultados, el nombre de columna devuelto se establecerá en el nombre definido.|  
|data_type|Los tipos de datos de cada columna. Si los tipos de datos son diferentes, se realiza una conversión implícita al tipo de datos definido. Si la conversión produce un error, se anula el lote|  
|COLLATE collation_name|La intercalación de cada columna. Si se produce un error de coincidencia de la intercalación, se intenta una intercalación implícita. Si se produce un error, se anula el lote.|  
|NULL &#124; NOT NULL|La nulabilidad de cada columna. Si la nulabilidad definida es NOT NULL y los datos devueltos contienen valores NULL, se produce un error y se anula el lote. Si no se especifica, el valor predeterminado es conforme al valor de las opciones ANSI_NULL_DFLT_ON y ANSI_NULL_DFLT_OFF.|  
  
 El conjunto de resultados real devuelto durante la ejecución puede ser diferente del definido mediante la cláusula WITH RESULT SETS de una de las formas siguientes: número de conjuntos de resultados, número de columnas, nombre de columna, nulabilidad y tipo de datos. Si el número de conjuntos de resultados es diferente, se produce un error y se anula el lote.  
  
## <a name="remarks"></a>Observaciones  
 Los parámetros se pueden proporcionar a través de *value* o usando @*parameter_name*=*value*. Un parámetro no forma parte de una transacción; por tanto, si se cambia un parámetro en una transacción que se revierte posteriormente, el valor del parámetro no vuelve a su valor anterior. El valor devuelto al procedimiento llamante es siempre el valor del parámetro en el momento en que finaliza el módulo llamado.  
  
 El anidamiento tiene lugar cuando un módulo llama a otro o ejecuta código administrado que hace referencia a un módulo CLR, un tipo definido por el usuario o un agregado. El nivel de anidamiento se aumenta cuando el código administrado o el módulo llamado comienzan la ejecución y disminuye cuando el código administrado o el módulo llamado terminan. Si se supera el máximo de 32 niveles de anidamiento, habrá un error de la cadena completa de llamada. El nivel actual de anidamiento se almacena en la función del sistema @@NESTLEVEL.  
  
 Debido a que ni los procedimientos almacenados ni los procedimientos almacenados extendidos se encuentran dentro del ámbito de una transacción (a menos que se ejecuten en una instrucción BEGIN DISTRIBUTED TRANSACTION o que se utilicen con varias opciones de configuración), los comandos que se ejecutan mediante llamadas a ellos no se pueden revertir. Para más información, vea [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) y [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md).  
  
 Se genera un error si usa y ejecuta un procedimiento que pasa, como parámetro de entrada, una variable de cursor con cursor asignado.  
  
 Cuando ejecute módulos, no es necesario que especifique la palabra clave EXECUTE si la instrucción es la primera de un lote.  
  
 Para obtener información adicional específica de los procedimientos almacenados de CLR, consulte Procedimientos almacenados de CLR.  
  
## <a name="using-execute-with-stored-procedures"></a>Utilizar EXECUTE con procedimientos almacenados  
 Cuando ejecute procedimientos almacenados, no es necesario que especifique la palabra clave EXECUTE si la instrucción es la primera de un lote.  
  
 Los procedimientos almacenados del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] empiezan con los caracteres sp_. Se almacenan físicamente en la [base de datos de recursos](../../relational-databases/databases/resource-database.md), pero aparecen lógicamente en el esquema sys de cada base de datos definida por el usuario y el sistema. Cuando se ejecuta un procedimiento almacenado del sistema, ya sea en un lote o en un módulo, como una función o un procedimiento almacenado y definido por el usuario, se recomienda calificar el nombre del procedimiento almacenado con el nombre del esquema sys.  
  
 Los procedimientos almacenados extendidos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] empiezan con los caracteres xp_, y se encuentran en el esquema dbo de la base de datos maestra. Cuando se ejecuta un procedimiento almacenado extendido del sistema, ya sea en un lote o en un módulo, como una función o un procedimiento almacenado y definido por el usuario, se recomienda calificar el nombre del procedimiento almacenado con master.dbo.  
  
 Cuando se ejecuta un procedimiento almacenado definido por el usuario, ya sea en un proceso por lotes o en un módulo como una función o un procedimiento almacenado definidos por el usuario, se recomienda que califique el nombre del procedimiento almacenado con un nombre de esquema. No se recomienda que denomine al procedimiento almacenado definido por el usuario con el mismo nombre que un procedimiento almacenado del sistema. Para más información sobre cómo ejecutar procedimientos almacenados, vea [Ejecutar un procedimiento almacenado](../../relational-databases/stored-procedures/execute-a-stored-procedure.md).  
  
## <a name="using-execute-with-a-character-string"></a>Utilizar EXECUTE con una cadena de caracteres  
 En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], las cadenas de caracteres se limitaban a 8.000 bytes. Esto requería la concatenación de grandes cadenas para la ejecución dinámica. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se pueden especificar los tipos de datos **varchar(max)** y **nvarchar(max)** que permiten cadenas de caracteres de hasta 2 gigabytes de datos.  
  
 Los cambios en el contexto de la base de datos solo duran hasta el final de la instrucción EXECUTE. Por ejemplo, después de ejecutar `EXEC` en la siguiente instrucción, el contexto de la base de datos será el de base de datos maestra.  
  
```sql  
USE master; EXEC ('USE AdventureWorks2012; SELECT BusinessEntityID, JobTitle FROM HumanResources.Employee;');  
```  
  
## <a name="context-switching"></a>Cambio de contexto  
 Puede utilizar la cláusula `AS { LOGIN | USER } = ' name '` para cambiar el contexto de ejecución de una instrucción dinámica. Cuando el cambio de contexto se especifica como `EXECUTE ('string') AS <context_specification>`, la duración del cambio de contexto se limita al ámbito de la consulta que se está ejecutando.  
  
###  <a name="specifying-a-user-or-login-name"></a><a name="_user"></a> Especificar un nombre de inicio de sesión o usuario  
 El nombre de inicio de sesión o usuario especificado en `AS { LOGIN | USER } = ' name '` debe existir como una entidad de seguridad en sys.database_principals o sys.server_principals respectivamente, o se producirá un error en la instrucción. Además, se deben conceder permisos IMPERSONATE en la entidad de seguridad. A menos que el autor de la llamada sea el propietario de la base de datos o un miembro del rol fijo de servidor sysadmin, la entidad de seguridad debe existir aun cuando el usuario tenga acceso a la base de datos o una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante la pertenencia a un grupo de Windows. Por ejemplo, supongamos las siguientes condiciones:  
  
-   El grupo CompanyDomain\SQLUsers tiene acceso a la base de datos Sales.  
  
-   CompanyDomain\SqlUser1 es un miembro de SQLUsers y, por tanto, tiene acceso implícito a la base de datos Sales.  
  
 Aunque CompanyDomain\SqlUser1 tiene acceso a la base de datos a través de la pertenencia al grupo SQLUsers, se producirá un error en la instrucción `EXECUTE @string_variable AS USER = 'CompanyDomain\SqlUser1'` porque `CompanyDomain\SqlUser1` no existe como una entidad de seguridad en la base de datos.  
  
### <a name="best-practices"></a>Prácticas recomendadas  
 Especifique un inicio de sesión o usuario que tenga al menos los privilegios requeridos para realizar las operaciones definidas en la instrucción o el módulo. Por ejemplo, no especifique un nombre de inicio de sesión que tiene permisos de nivel de servidor, si solo se necesitan permisos de nivel de base de datos; o no especifique una cuenta de propietario de base de datos a menos que se requieran esos permisos.  
  
## <a name="permissions"></a>Permisos  
 No se requieren permisos para ejecutar la instrucción EXECUTE. Sin embargo, se requieren permisos para los elementos protegibles a los que se hace referencia en la cadena EXECUTE. Por ejemplo, si la cadena contiene una instrucción INSERT, el autor de llamada de la instrucción EXECUTE debe tener el permiso INSERT en la tabla de destino. Los permisos se comprueban cuando se encuentra la instrucción EXECUTE, incluso si la instrucción EXECUTE está incluida en un módulo.  
  
 Los permisos EXECUTE de un módulo son, de forma predeterminada, del propietario del módulo, que puede transferirlos a otros usuarios. Cuando se ejecuta un módulo que ejecuta una cadena, los permisos se comprueban en el contexto del usuario que ejecuta el módulo, no en el contexto del usuario que creó el módulo. No obstante, si el mismo usuario es propietario del módulo que llama y el módulo que se va a llamar, no se realiza la comprobación del permiso EXECUTE en el segundo módulo.  
  
 Si el módulo tiene acceso a otros objetos de base de datos, la ejecución es correcta cuando tienen el permiso EXECUTE en el módulo y una de las opciones siguientes es cierta:  
  
-   El módulo está marcado como EXECUTE AS USER o SELF y el propietario del módulo tiene los permisos correspondientes en el objeto referenciado. Para más información sobre la suplantación dentro de un módulo, vea [Cláusula EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md).  
  
-   El módulo está marcado como EXECUTE AS CALLER y tiene los permisos correspondientes en el objeto.  
  
-   El módulo está marcado como EXECUTE AS *user_name* y *user_name* y tiene los permisos correspondientes en el objeto.  
  
### <a name="context-switching-permissions"></a>Permisos para cambiar de contexto  
 Para especificar EXECUTE AS en un inicio de sesión, el autor de llamada debe tener permisos IMPERSONATE en el nombre de inicio de sesión especificado. Para especificar EXECUTE AS en un usuario de base de datos, el autor de llamada debe tener permisos IMPERSONATE en el nombre de usuario especificado. Cuando no se especifica ningún contexto de ejecución o se especifica EXECUTE AS CALLER, no se requieren permisos IMPERSONATE.  
  
## <a name="examples-sql-server"></a>Ejemplos: SQL Server
  
### <a name="a-using-execute-to-pass-a-single-parameter"></a>A. Usar EXECUTE para pasar un único parámetro  
 El procedimiento almacenado `uspGetEmployeeManagers` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] espera un parámetro (`@EmployeeID`). En el siguiente ejemplo se ejecuta el procedimiento almacenado`uspGetEmployeeManagers` con `Employee ID 6` como valor del parámetro.  
  
```sql    
EXEC dbo.uspGetEmployeeManagers 6;  
GO  
```  
  
 La variable se puede llamar explícitamente en la ejecución:  
  
```sql    
EXEC dbo.uspGetEmployeeManagers @EmployeeID = 6;  
GO  
```  
  
 Si la siguiente instrucción es la primera de un lote o de un script **osql** o **sqlcmd**, no se necesita EXEC.  
  
```sql    
dbo.uspGetEmployeeManagers 6;  
GO  
--Or  
dbo.uspGetEmployeeManagers @EmployeeID = 6;  
GO  
```  
  
### <a name="b-using-multiple-parameters"></a>B. Usar varios parámetros  
 En el ejemplo siguiente se ejecuta el procedimiento almacenado `spGetWhereUsedProductID` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Pasa dos parámetros: el primero es el Id. de producto (`819`) y el segundo parámetro, `@CheckDate,`, es un valor `datetime`.  
  
```sql    
DECLARE @CheckDate DATETIME;  
SET @CheckDate = GETDATE();  
EXEC dbo.uspGetWhereUsedProductID 819, @CheckDate;  
GO  
```  
  
### <a name="c-using-execute-tsql_string-with-a-variable"></a>C. Usar EXECUTE 'tsql_string' con una variable  
 En el ejemplo siguiente se muestra cómo trata `EXECUTE` las cadenas construidas dinámicamente que contienen variables. Este ejemplo crea el cursor `tables_cursor` para que mantenga una lista de todas las tablas definidas por el usuario en la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y, a continuación, usa esa lista para volver a generar todos los índices de las tablas.  
  
```sql    
DECLARE tables_cursor CURSOR  
   FOR  
   SELECT s.name, t.name   
   FROM sys.objects AS t  
   JOIN sys.schemas AS s ON s.schema_id = t.schema_id  
   WHERE t.type = 'U';  
OPEN tables_cursor;  
DECLARE @schemaname sysname;  
DECLARE @tablename sysname;  
FETCH NEXT FROM tables_cursor INTO @schemaname, @tablename;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN;  
   EXECUTE ('ALTER INDEX ALL ON ' + @schemaname + '.' + @tablename + ' REBUILD;');  
   FETCH NEXT FROM tables_cursor INTO @schemaname, @tablename;  
END;  
PRINT 'The indexes on all tables have been rebuilt.';  
CLOSE tables_cursor;  
DEALLOCATE tables_cursor;  
GO  
  
```  
  
### <a name="d-using-execute-with-a-remote-stored-procedure"></a>D. Usar EXECUTE con un procedimiento almacenado remoto  
 En el ejemplo siguiente se ejecuta el procedimiento almacenado `uspGetEmployeeManagers` en el servidor remoto `SQLSERVER1` y se almacena el estado de retorno, que indica éxito o fracaso, en `@retstat`.  
  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
```sql    
DECLARE @retstat INT;  
EXECUTE @retstat = SQLSERVER1.AdventureWorks2012.dbo.uspGetEmployeeManagers @BusinessEntityID = 6;  
```  
  
### <a name="e-using-execute-with-a-stored-procedure-variable"></a>E. Usar EXECUTE con una variable de procedimiento almacenado  
 En el ejemplo siguiente se crea una variable que representa un nombre de procedimiento almacenado.  
  
```sql  
DECLARE @proc_name VARCHAR(30);  
SET @proc_name = 'sys.sp_who';  
EXEC @proc_name;  
  
```  
  
### <a name="f-using-execute-with-default"></a>F. Usar EXECUTE con DEFAULT  
 En el ejemplo siguiente se crea un procedimiento almacenado con valores predeterminados para el primer y tercer parámetro. Cuando se ejecuta el procedimiento, estos valores predeterminados se insertan como parámetros primero y tercero si no se pasa ningún valor en la llamada o si se especifica el valor predeterminado. Observe las distintas formas en las que se puede usar la palabra clave `DEFAULT`.  
  
```sql    
IF OBJECT_ID(N'dbo.ProcTestDefaults', N'P')IS NOT NULL  
   DROP PROCEDURE dbo.ProcTestDefaults;  
GO  
-- Create the stored procedure.  
CREATE PROCEDURE dbo.ProcTestDefaults (  
@p1 SMALLINT = 42,   
@p2 CHAR(1),   
@p3 VARCHAR(8) = 'CAR')  
AS   
   SET NOCOUNT ON;  
   SELECT @p1, @p2, @p3  
;  
GO  
```  
  
 El procedimiento almacenado `Proc_Test_Defaults` se puede ejecutar en muchas combinaciones.  
  
```sql    
-- Specifying a value only for one parameter (@p2).  
EXECUTE dbo.ProcTestDefaults @p2 = 'A';  
-- Specifying a value for the first two parameters.  
EXECUTE dbo.ProcTestDefaults 68, 'B';  
-- Specifying a value for all three parameters.  
EXECUTE dbo.ProcTestDefaults 68, 'C', 'House';  
-- Using the DEFAULT keyword for the first parameter.  
EXECUTE dbo.ProcTestDefaults @p1 = DEFAULT, @p2 = 'D';  
-- Specifying the parameters in an order different from the order defined in the procedure.  
EXECUTE dbo.ProcTestDefaults DEFAULT, @p3 = 'Local', @p2 = 'E';  
-- Using the DEFAULT keyword for the first and third parameters.  
EXECUTE dbo.ProcTestDefaults DEFAULT, 'H', DEFAULT;  
EXECUTE dbo.ProcTestDefaults DEFAULT, 'I', @p3 = DEFAULT;  
  
```  
  
### <a name="g-using-execute-with-at-linked_server_name"></a>G. Usar EXECUTE con AT linked_server_name  
 En el siguiente ejemplo se pasa una cadena de comandos a un servidor remoto. Crea un servidor vinculado `SeattleSales` que apunta a otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y ejecuta una instrucción DDL (`CREATE TABLE`) contra ese servidor vinculado.  
  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
```sql    
EXEC sp_addlinkedserver 'SeattleSales', 'SQL Server'  
GO  
EXECUTE ( 'CREATE TABLE AdventureWorks2012.dbo.SalesTbl   
(SalesID int, SalesName varchar(10)) ; ' ) AT SeattleSales;  
GO  
```  
  
### <a name="h-using-execute-with-recompile"></a>H. Usar EXECUTE WITH RECOMPILE  
 En el siguiente ejemplo se ejecuta el procedimiento almacenado `Proc_Test_Defaults` y se exige que un nuevo plan de consulta se compile, se use y se descarte después de ejecutar el módulo.  
  
```sql    
EXECUTE dbo.Proc_Test_Defaults @p2 = 'A' WITH RECOMPILE;  
GO  
```  
  
### <a name="i-using-execute-with-a-user-defined-function"></a>I. Usar EXECUTE con una función definida por el usuario  
 En el siguiente ejemplo se ejecuta la función escalar definida por el usuario `ufnGetSalesOrderStatusText` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Usa la variable `@returnstatus` para almacenar el valor devuelto por la función. La función espera un parámetro de entrada, `@Status`. Se define como un tipo de datos **tinyint**.  
  
```sql    
DECLARE @returnstatus NVARCHAR(15);  
SET @returnstatus = NULL;  
EXEC @returnstatus = dbo.ufnGetSalesOrderStatusText @Status = 2;  
PRINT @returnstatus;  
GO  
```  
  
### <a name="j-using-execute-to-query-an-oracle-database-on-a-linked-server"></a>J. Usar EXECUTE para consultar una base de datos de Oracle en un servidor vinculado  
 En el siguiente ejemplo se ejecutan varias instrucciones de `SELECT` en el servidor Oracle remoto. El ejemplo empieza agregando el servidor Oracle como un servidor vinculado y creando el inicio de sesión del servidor vinculado.  
  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
```sql    
-- Setup the linked server.  
EXEC sp_addlinkedserver    
        @server='ORACLE',  
        @srvproduct='Oracle',  
        @provider='OraOLEDB.Oracle',   
        @datasrc='ORACLE10';  
  
EXEC sp_addlinkedsrvlogin   
    @rmtsrvname='ORACLE',  
    @useself='false',   
    @locallogin=null,   
    @rmtuser='scott',   
    @rmtpassword='tiger';  
  
EXEC sp_serveroption 'ORACLE', 'rpc out', true;  
GO  
  
-- Execute several statements on the linked Oracle server.  
EXEC ( 'SELECT * FROM scott.emp') AT ORACLE;  
GO  
EXEC ( 'SELECT * FROM scott.emp WHERE MGR = ?', 7902) AT ORACLE;  
GO  
DECLARE @v INT;   
SET @v = 7902;  
EXEC ( 'SELECT * FROM scott.emp WHERE MGR = ?', @v) AT ORACLE;  
GO   
```  
  
### <a name="k-using-execute-as-user-to-switch-context-to-another-user"></a>K. Usar EXECUTE AS USER para cambiar el contexto a otro usuario  
 En el siguiente ejemplo se ejecuta una cadena [!INCLUDE[tsql](../../includes/tsql-md.md)] que crea una tabla y se especifica la cláusula `AS USER` para cambiar el contexto de ejecución de la instrucción del autor de llamada a `User1`. El [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprobará los permisos de `User1` cuando se ejecute la instrucción. `User1` debe existir como un usuario en la base de datos y debe tener permiso para crear tablas en el esquema `Sales`; de lo contrario, se producirá un error en la instrucción.  
  
```sql    
EXECUTE ('CREATE TABLE Sales.SalesTable (SalesID INT, SalesName VARCHAR(10));')  
AS USER = 'User1';  
GO  
```  
  
### <a name="l-using-a-parameter-with-execute-and-at-linked_server_name"></a>L. Usar un parámetro con EXECUTE y AT linked_server_name  
 En el ejemplo siguiente se pasa una cadena de comando a un servidor remoto mediante un marcador de posición (signo de interrogación, `?`) para un parámetro. El ejemplo crea un servidor vinculado `SeattleSales` que apunta a otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y ejecuta una instrucción `SELECT` en ese servidor vinculado. La instrucción `SELECT` utiliza el signo de interrogación como marcador de posición para el parámetro `ProductID` (`952`), que se suministra a continuación de la instrucción.  
  
**Válido para** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.
  
```sql    
-- Setup the linked server.  
EXEC sp_addlinkedserver 'SeattleSales', 'SQL Server'  
GO  
-- Execute the SELECT statement.  
EXECUTE ('SELECT ProductID, Name   
    FROM AdventureWorks2012.Production.Product  
    WHERE ProductID = ? ', 952) AT SeattleSales;  
GO  
```  
  
### <a name="m-using-execute-to-redefine-a-single-result-set"></a>M. Usar EXECUTE para volver a definir un conjunto de resultados único  
 En algunos de los ejemplos anteriores se ejecutó `EXEC dbo.uspGetEmployeeManagers 6;`, que devolvió 7 columnas. En el ejemplo siguiente se muestra cómo usar la sintaxis `WITH RESULT SET` para cambiar los nombres y los tipos de datos del conjunto de resultados devuelto.  
  
**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
```sql    
EXEC uspGetEmployeeManagers 16  
WITH RESULT SETS  
(   
   ([Reporting Level] INT NOT NULL,  
    [ID of Employee] INT NOT NULL,  
    [Employee First Name] NVARCHAR(50) NOT NULL,  
    [Employee Last Name] NVARCHAR(50) NOT NULL,  
    [Employee ID of Manager] NVARCHAR(max) NOT NULL,  
    [Manager First Name] NVARCHAR(50) NOT NULL,  
    [Manager Last Name] NVARCHAR(50) NOT NULL )  
);  
  
```  
  
### <a name="n-using-execute-to-redefine-a-two-result-sets"></a>Hora Usar EXECUTE para volver a definir dos conjuntos de resultados  
 Al ejecutar una instrucción que devuelve más de un conjunto de resultados, defina cada conjunto de resultados esperado. En el siguiente ejemplo de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] se crea un procedimiento que devuelve dos conjuntos de resultados. Después, se ejecuta el procedimiento por medio de la cláusula **WITH RESULT SETS** y se especifican dos definiciones del conjunto de resultados.  
  
**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]
  
```sql    
--Create the procedure  
CREATE PROC Production.ProductList @ProdName NVARCHAR(50)  
AS  
-- First result set  
SELECT ProductID, Name, ListPrice  
    FROM Production.Product  
    WHERE Name LIKE @ProdName;  
-- Second result set   
SELECT Name, COUNT(S.ProductID) AS NumberOfOrders  
    FROM Production.Product AS P  
    JOIN Sales.SalesOrderDetail AS S  
        ON P.ProductID  = S.ProductID   
    WHERE Name LIKE @ProdName  
    GROUP BY Name;  
GO  
  
-- Execute the procedure   
EXEC Production.ProductList '%tire%'  
WITH RESULT SETS   
(  
    (ProductID INT,   -- first result set definition starts here  
    Name NAME,  
    ListPrice MONEY)  
    ,                 -- comma separates result set definitions  
    (Name NAME,       -- second result set definition starts here  
    NumberOfOrders INT)  
);  
  
```  
  ### <a name="o-using-execute-with-at-data_source-data_source_name-to-query-a-remote-sql-server"></a>O. Empleo de EXECUTE con AT DATA_SOURCE data_source_name para consultar una instancia remota de SQL Server 
  
 En el ejemplo siguiente se pasa una cadena de comandos a un origen de datos externo que apunta a una instancia de SQL Server. 
  
**Válido para** [!INCLUDE[sssql19](../../includes/sssql19-md.md)] y versiones posteriores.
  
```sql    
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE my_sql_server;  
GO  
```  
  
### <a name="p-using-execute-with-at-data_source-data_source_name-to-query-compute-pool-in-sql-server-big-data-cluster"></a>P. Empleo de EXECUTE con AT DATA_SOURCE data_source_name para consultar un grupo de proceso en un clúster de macrodatos de SQL Server 

 En el ejemplo siguiente se pasa una cadena de comandos a un origen de datos externo que apunta a un grupo de proceso de un clúster de macrodatos de SQL Server. En el ejemplo se crea un origen de datos `SqlComputePool` en un grupo de proceso de un clúster de macrodatos de SQL Server y se ejecuta una instrucción `SELECT` en el origen de datos. 
  
**Válido para** [!INCLUDE[sssql19](../../includes/sssql19-md.md)] y versiones posteriores.
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlComputePool 
WITH (LOCATION = 'sqlcomputepool://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlComputePool;  
GO  
```  

### <a name="q-using-execute-with-at-data_source-data_source_name-to-query-data-pool-in-sql-server-big-data-cluster"></a>Q. Empleo de EXECUTE con AT DATA_SOURCE data_source_name para consultar un grupo de datos en un clúster de macrodatos de SQL Server 
 En el ejemplo siguiente se pasa una cadena de comandos a un origen de datos externo que apunta a un grupo de proceso de un clúster de macrodatos de SQL Server. En el ejemplo se crea un origen de datos `SqlDataPool` en un grupo de datos de un clúster de macrodatos de SQL Server y se ejecuta una instrucción `SELECT` en el origen de datos. 
  
**Válido para** [!INCLUDE[sssql19](../../includes/sssql19-md.md)] y versiones posteriores.
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlDataPool 
WITH (LOCATION = 'sqldatapool://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlDataPool;  
GO  
```

### <a name="r-using-execute-with-at-data_source-data_source_name-to-query-storage-pool-in-sql-server-big-data-cluster"></a>R. Empleo de EXECUTE con AT DATA_SOURCE data_source_name para consultar un bloque de almacenamiento en un clúster de macrodatos de SQL Server 

 En el ejemplo siguiente se pasa una cadena de comandos a un origen de datos externo que apunta a un grupo de proceso de un clúster de macrodatos de SQL Server. En el ejemplo se crea un origen de datos `SqlStoragePool` en un grupo de datos de un clúster de macrodatos de SQL Server y se ejecuta una instrucción `SELECT` en el origen de datos. 
  
**Válido para** [!INCLUDE[sssql19](../../includes/sssql19-md.md)] y versiones posteriores.
  
```sql  
CREATE EXTERNAL DATA SOURCE SqlStoragePool
WITH (LOCATION = 'sqlhdfs://controller-svc/default');
EXECUTE ( 'SELECT @@SERVERNAME' ) AT DATA_SOURCE SqlStoragePool;  
GO  
```

  
## <a name="examples-azure-synapse-analytics"></a>Ejemplos: Azure Synapse Analytics 
  
### <a name="a-basic-procedure-execution"></a>A. ejecución de procedimiento básico  
 Ejecutar un procedimiento almacenado:  
  
```sql  
EXEC proc1;  
```  
  
 Llamar a un procedimiento almacenado con un nombre determinado en tiempo de ejecución:  
  
```sql    
EXEC ('EXEC ' + @var);  
```  
  
 Llamar a un procedimiento almacenado desde un procedimiento almacenado:  
  
```sql   
CREATE sp_first AS EXEC sp_second; EXEC sp_third;  
```  
  
### <a name="b-executing-strings"></a>B. ejecutar cadenas  
 Ejecutar una cadena SQL:  
  
```sql   
EXEC ('SELECT * FROM sys.types');  
```  
  
 Ejecutar una cadena anidada:  
  
```sql  
EXEC ('EXEC (''SELECT * FROM sys.types'')');  
```  
  
 Ejecutar una variable de cadena:  
  
```sql  
DECLARE @stringVar NVARCHAR(100);  
SET @stringVar = N'SELECT name FROM' + ' sys.sql_logins';  
EXEC (@stringVar);  
```  
  
### <a name="c-procedures-with-parameters"></a>C. procedimientos con parámetros  

 En el siguiente ejemplo se crea un procedimiento con parámetros y se muestran tres formas de ejecutarlo:  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROC ProcWithParameters  
    @name NVARCHAR(50),  
@color NVARCHAR(15)  
AS   
SELECT ProductKey, EnglishProductName, Color FROM [dbo].[DimProduct]  
WHERE EnglishProductName LIKE @name  
AND Color = @color;  
GO  
  
-- Executing using positional parameters  
EXEC ProcWithParameters N'%arm%', N'Black';  
-- Executing using named parameters in order  
EXEC ProcWithParameters @name = N'%arm%', @color = N'Black';  
-- Executing using named parameters out of order  
EXEC ProcWithParameters @color = N'Black', @name = N'%arm%';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [@@NESTLEVEL &#40;Transact-SQL&#41;](../../t-sql/functions/nestlevel-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [EXECUTE AS &#40;cláusula de Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [osql Utility](../../tools/osql-utility.md)  (Utilidad osql)  
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sqlcmd (utilidad)](../../tools/sqlcmd-utility.md)   
 [SUSER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-name-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)   
 [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)   
 [Funciones escalares definidas por el usuario para OLTP en memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)  
  
  
