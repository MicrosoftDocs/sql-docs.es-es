---
description: INSERT (Transact-SQL)
title: INSERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INSERT_TSQL
- INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- inserting multiple rows
- user-defined types [SQL Server], inserting values
- DML [SQL Server], INSERT statement
- bulk load [SQL Server]
- row additions [SQL Server], INSERT statement
- inserting rows
- table value constructor [SQL Server]
- adding data
- INSERT statement [SQL Server], about INSERT statement
- INSERT statement [SQL Server]
- UDTs [SQL Server], inserting values
- adding rows
- INSERT INTO statement
- data manipulation language [SQL Server], INSERT statement
- inserting data
ms.assetid: 1054c76e-0fd5-4131-8c07-a6c5d024af50
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b5a6fd7131b63741a82c120e1e0b222405865a9d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237183"
---
# <a name="insert-transact-sql"></a>INSERT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


Agrega una o varias filas a una tabla o una vista en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener ejemplos, vea [Ejemplos](#InsertExamples).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  

[ WITH <common_table_expression> [ ,...n ] ]  
INSERT   
{  
        [ TOP ( expression ) [ PERCENT ] ]   
        [ INTO ]   
        { <object> | rowset_function_limited   
          [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
        }  
    {  
        [ ( column_list ) ]   
        [ <OUTPUT Clause> ]  
        { VALUES ( { DEFAULT | NULL | expression } [ ,...n ] ) [ ,...n     ]   
        | derived_table   
        | execute_statement  
        | <dml_table_source>  
        | DEFAULT VALUES   
        }  
    }  
}  
[;]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
      | database_name .[ schema_name ] .   
      | schema_name .   
    ]  
  table_or_view_name  
}  
  
<dml_table_source> ::=  
    SELECT <select_list>  
    FROM ( <dml_statement_with_output_clause> )   
      [AS] table_alias [ ( column_alias [ ,...n ] ) ]  
    [ WHERE <search_condition> ]  
        [ OPTION ( <query_hint> [ ,...n ] ) ]  
```  
  
```syntaxsql
-- External tool only syntax  

INSERT   
{  
    [BULK]  
    { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
    ( <column_definition> )  
    [ WITH (  
        [ [ , ] CHECK_CONSTRAINTS ]  
        [ [ , ] FIRE_TRIGGERS ]  
        [ [ , ] KEEP_NULLS ]  
        [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]  
        [ [ , ] ROWS_PER_BATCH = rows_per_batch ]  
        [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]  
        [ [ , ] TABLOCK ]  
    ) ]  
}  
  
[; ] <column_definition> ::=  
 column_name <data_type>  
    [ COLLATE collation_name ]  
    [ NULL | NOT NULL ]  
  
<data type> ::=   
[ type_schema_name . ] type_name   
    [ ( precision [ , scale ] | max ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

INSERT INTO { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    {   
      VALUES ( { NULL | expression } )  
      | SELECT <select_criteria>  
    }  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 WITH \<common_table_expression>  
 Especifica el conjunto de resultados con nombre temporal, denominado también expresión de tabla común, definido en el ámbito de la instrucción INSERT. El conjunto de resultados se deriva de una instrucción SELECT. Para más información, vea [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP (*expression*) [ PERCENT ]  
 Especifica el número o el porcentaje de filas aleatorias que se van a insertar. *expression* puede ser un número o un porcentaje de las filas. Para más información, vea [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
  
 INTO  
 Es una palabra clave opcional que se puede utilizar entre INSERT y la tabla de destino.  
  
 *server_name*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Es el nombre del servidor vinculado en el que se encuentra la tabla o la vista. *server_name* se puede especificar como un nombre de [servidor vinculado](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) o usando la función [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md).  
  
 Cuando *server_name* se especifica como un servidor vinculado, se requiere *database_name* y *schema_name*. Cuando *server_name* se especifica con OPENDATASOURCE, es posible que *database_name* y *schema_name* no se apliquen a todos los orígenes de datos y dependan de las capacidades del proveedor OLE DB que accede al objeto remoto.  
  
 *database_name*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Es el nombre de la base de datos.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece la tabla o la vista.  
  
 *table_or view_name*  
 Es el nombre de la tabla o la vista que va a recibir los datos.  
  
 Se puede usar una variable de [tabla](../../t-sql/data-types/table-transact-sql.md), en su ámbito, como origen de tabla en una instrucción INSERT.  
  
 La vista a la que hace referencia *table_or_view_name* debe poderse actualizar y debe hacer referencia exactamente a una tabla base de la cláusula FROM de la vista. Por ejemplo, la instrucción INSERT de una vista de varias tablas debe usar una *column_list* que solamente haga referencia a columnas de una tabla base. Para más información sobre las vistas actualizables, vea [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica la función [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) u [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md). El uso de estas funciones está sujeto a las capacidades del proveedor OLE DB que tiene acceso al objeto remoto.  
  
 WITH ( \<table_hint_limited> [... *n* ] )  
 Especifica una o varias sugerencias de tabla que están permitidas en una tabla de destino. La palabra clave WITH y los paréntesis son obligatorios.  
  
 No se permiten READPAST, NOLOCK ni READUNCOMMITTED. Para más información sobre las sugerencias de tabla, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
> [!IMPORTANT]  
>  La posibilidad de especificar las sugerencias HOLDLOCK, SERIALIZABLE, READCOMMITTED, REPEATABLEREAD o UPDLOCK en tablas que son destinos de instrucciones INSERT se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Estas sugerencias no influyen en el rendimiento de las instrucciones INSERT. Evite el uso de dichas sugerencias en los nuevos trabajos de desarrollo y piense en modificar las aplicaciones que las utilizan actualmente.  
  
 Especificar la sugerencia TABLOCK en una tabla que es el destino de una instrucción INSERT tiene el mismo efecto que especificar la sugerencia TABLOCKX. Se realiza un bloqueo exclusivo en la tabla.  
  
 (*column_list*)  
 Es una lista de una o más columnas donde se van a insertar los datos. *column_list* debe ir entre paréntesis y delimitada con comas.  
  
 Si la columna no se incluye en *column_list*, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] debe ser capaz de proporcionar un valor basado en la definición de la columna; de lo contrario, no se puede cargar la fila. [!INCLUDE[ssDE](../../includes/ssde-md.md)] proporciona automáticamente un valor para la columna si esta:  
  
-   Tiene una propiedad IDENTITY. Se usa el valor de identidad incremental siguiente.  
  
-   Tiene un valor predeterminado. Se usa el valor predeterminado de la columna.  
  
-   Tiene un tipo de datos **timestamp**. Se utiliza el valor actual de marca de tiempo.  
  
-   Acepta valores NULL. Se usa un valor NULL.  
  
-   Es una columna calculada. Se utiliza el valor calculado.  
  
*column_list* se debe usar al insertar valores explícitos en una columna de identidad. La opción SET IDENTITY_INSERT debe ser ON para la tabla.  
  
Cláusula OUTPUT  
 Devuelve las filas insertadas como parte de la operación de inserción. Los resultados se pueden devolver a la aplicación de procesamiento o insertarse en una tabla o variable de tabla para su nuevo procesamiento.  
  
 La cláusula OUTPUT no se admite en las instrucciones DML que hacen referencia a vistas locales con particiones, vistas distribuidas con particiones, tablas remotas o instrucciones INSERT que contengan una función *execute_statement*. La cláusula OUTPUT INTO no se admite en instrucciones INSERT que contengan una cláusula \<dml_table_source>. Para más información sobre los argumentos y el comportamiento de esta cláusula, vea [Cláusula OUTPUT (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md).
  
 VALUES  
 Presenta la lista o listas de valores de datos que se van a insertar. Debe haber un valor de datos por cada columna en *column_list*, si se especifica, o en la tabla. La lista de valores debe ir entre paréntesis.  
  
 Si los valores de la lista Value no están en el mismo orden que las columnas de la tabla o no contienen un valor para cada columna de la tabla, se debe usar *column_list* para especificar de forma explícita la columna que almacenará cada valor de entrada.  
  
 Puede utilizar el constructor de filas de [!INCLUDE[tsql](../../includes/tsql-md.md)] (que también se denomina constructor con valores de tabla) para especificar varias filas en una única instrucción INSERT. El constructor de filas se compone de una única cláusula VALUES con varias listas de valores escritos entre paréntesis y separados por una coma. Para más información, vea [Constructor con valores de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/table-value-constructor-transact-sql.md).  
  
 DEFAULT  
 Hace que [!INCLUDE[ssDE](../../includes/ssde-md.md)] cargue el valor predeterminado definido para una columna. Si no existe ningún valor predeterminado para la columna y esta admite valores NULL, se inserta NULL. En una columna definida con el tipo de datos **timestamp**, se inserta el siguiente valor de marca de tiempo. DEFAULT no es un valor válido para una columna de identidad.  
  
 *expression*  
 Es una constante, variable o expresión. La expresión no puede contener una instrucción EXECUTE.  
  
 Cuando se hace referencia a los tipos de datos de caracteres Unicode **nchar**, **nvarchar** y **ntext**, debe agregarse como prefijo la letra mayúscula "N" a "*expression*". Si no se especifica 'N', [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convierte la cadena a la página de códigos que se corresponde con la intercalación predeterminada de la base de datos o columna. Los caracteres que no se encuentren en esta página de códigos se perderán.  
  
 *derived_table*  
 Es cualquier instrucción SELECT válida que devuelva filas con los datos que se van a cargar en la tabla. La instrucción SELECT no puede contener una expresión de tabla común (CTE).  
  
 *execute_statement*  
 Es cualquier instrucción EXECUTE válida que devuelva datos con instrucciones SELECT o READTEXT. Para obtener más información, vea [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md).  
  
 Las opciones de RESULT SETS de la instrucción EXECUTE no se pueden especificar en una instrucción INSERT…EXEC.  
  
 Si se usa *execute_statement* con INSERT, cada conjunto de resultados debe ser compatible con las columnas de la tabla o de *column_list*.  
  
 *execute_statement* se puede usar para ejecutar procedimientos almacenados en el mismo servidor o en un servidor remoto. Se ejecuta el procedimiento en el servidor remoto, se devuelven los conjuntos de resultados al servidor local y se cargan en la tabla del servidor local. En una transacción distribuida, *execute_statement* no se puede emitir en un servidor vinculado de bucle invertido cuando la conexión tiene varios conjuntos de resultados activos múltiples (MARS) habilitados.  
  
 If *execute_statement* devuelve datos con la instrucción READTEXT, cada instrucción READTEXT puede devolver un máximo de 1 MB (1024 KB) de datos. *execute_statement* también se puede usar con procedimientos extendidos. *execute_statement* inserta los datos devueltos por el subproceso principal del procedimiento extendido; no obstante, los resultados de los subprocesos distintos del subproceso principal no se insertan.  
  
 No puede especificar un parámetro con valores de tabla como el destino de una instrucción INSERT EXEC; sin embargo, se puede especificar como un origen en la cadena o procedimiento almacenado INSERT EXEC. Para obtener más información, vea[Usar parámetros con valores de tabla &#40;motor de base de datos&#41;](../../relational-databases/tables/use-table-valued-parameters-database-engine.md).  
  
 \<dml_table_source>  
 Especifica que las filas insertadas en la tabla de destino son las que ha devuelto la cláusula OUTPUT de una instrucción INSERT, UPDATE, DELETE o MERGE, filtradas opcionalmente por una cláusula WHERE. Si se especifica \<dml_table_source>, el destino de la instrucción INSERT externa debe cumplir las restricciones siguientes: 
  
-   Debe ser una tabla base, no una vista.  
  
-   No puede ser una tabla remota.  
  
-   No puede tener definido ningún desencadenador.  
  
-   No puede participar en ninguna relación clave principal-clave externa.  
  
-   No puede participar en la replicación de mezcla ni en las suscripciones actualizables para la replicación transaccional.  
  
 El nivel de compatibilidad de la base de datos debe estar establecido en 100 o superior. Para más información, vea [Cláusula OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md).  
  
 \<select_list>  
 Es una lista separada por comas que especifica las columnas devueltas por la cláusula OUTPUT que se tienen que insertar. Las columnas de \<select_list> deben ser compatibles con las columnas en las que se insertan los valores. \<select_list> no puede hacer referencia a funciones de agregado ni a TEXTPTR. 
  
> [!NOTE]  
>  Las variables enumeradas en la lista SELECT hacen referencia a sus valores originales, con independencia de los cambios realizados en ellas en \<dml_statement_with_output_clause>.  
  
 \<dml_statement_with_output_clause>  
 Es una instrucción INSERT, UPDATE, DELETE o MERGE válida que devuelve las filas afectadas en una cláusula OUTPUT. La instrucción no puede contener una cláusula WITH y no puede tener como destino tablas remotas o vistas con particiones. Si se especifica UPDATE o DELETE, no puede ser una instrucción UPDATE o DELETE basada en cursores. No se puede hacer referencia a las filas de origen como instrucciones DML anidadas.  
  
 WHERE \<search_condition>  
 Es cualquier cláusula WHERE que contiene una condición \<search_condition> válida que filtra las filas devueltas por \<dml_statement_with_output_clause>. Para más información, vea [Condición de búsqueda &#40;Transact-SQL&#41;](../../t-sql/queries/search-condition-transact-sql.md). Cuando se usa en este contexto, \<search_condition> no puede contener subconsultas, funciones escalares definidas por el usuario que accedan a datos, funciones de agregado, TEXTPTR ni predicados de búsqueda de texto completo. 
  
 DEFAULT VALUES  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Hace que la nueva fila contenga los valores predeterminados definidos para cada columna.  
  
 BULK  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 La usan las herramientas externas para cargar un flujo de datos binarios. Esta opción no está diseñada para usarse con herramientas tales como [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], SQLCMD, OSQL ni interfaces de programación de aplicaciones de acceso a datos como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
 FIRE_TRIGGERS  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica que se ejecutarán todos los desencadenadores de inserción definidos en la tabla de destino durante la operación de carga de flujos de datos binarios. Para obtener más información, vea [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 CHECK_CONSTRAINTS  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica que deben comprobarse todas las restricciones de la tabla o vista de destino durante la operación de carga de flujos de datos binarios. Para obtener más información, vea [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 KEEPNULLS  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Especifica que las columnas vacías deben conservar un valor nulo durante la operación de carga de flujos de datos binarios. Para obtener más información, vea [Mantener valores NULL o usar valores predeterminados durante la importación en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
 KILOBYTES_PER_BATCH = kilobytes_per_batch  
 Especifica el número aproximado de kilobytes (KB) de datos por lote como *kilobytes_per_batch*. Para obtener más información, vea [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
 ROWS_PER_BATCH =*rows_per_batch*  
 **Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Indica el número aproximado de filas de datos del flujo de datos binarios. Para obtener más información, vea [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
> [!NOTE]
>  Si no se proporciona una lista de columnas, se produce un error de sintaxis.  

## <a name="remarks"></a>Observaciones  
Para obtener información específica sobre cómo insertar datos en tablas de SQL Graph, vea [INSERT (SQL Graph)](../../t-sql/statements/insert-sql-graph.md). 

## <a name="best-practices"></a>Prácticas recomendadas  
 Use la función @@ROWCOUNT para devolver el número de filas insertadas a la aplicación cliente. Para más información, vea [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).  
  
### <a name="best-practices-for-bulk-importing-data"></a>Prácticas recomendadas para la importación masiva de datos  
  
#### <a name="using-insert-intoselect-to-bulk-import-data-with-minimal-logging-and-parallelism"></a>Usar INSERT INTO…SELECT para realizar una importación masiva de datos con registros y paralelismos mínimos 
Puede usar `INSERT INTO <target_table> SELECT <columns> FROM <source_table>` para transferir eficazmente un gran número de filas de una tabla, como una tabla de ensayo, a otra tabla con registro mínimo. El registro mínimo puede mejorar el rendimiento de la instrucción y reducir la posibilidad de que la operación rellene el espacio del registro de transacciones disponible durante la transacción.  
  
El registro mínimo para esta instrucción tiene los requisitos siguientes:  
-   El modelo de recuperación de la base de datos está establecido en registro simple o masivo.  
-   La tabla de destino es un montón vacío o no vacío.  
-   La tabla de destino no se usa en la replicación.  
-   La sugerencia `TABLOCK` se especifica para la tabla de destino.  
  
Las filas que se insertan en un montón como el resultado de una acción de inserción en una instrucción MERGE también se pueden registrar con un nivel mínimo.  
  
A diferencia de la instrucción `BULK INSERT`, que contiene un bloqueo de actualización masiva (BU) menos restrictivo, `INSERT INTO … SELECT` con la sugerencia `TABLOCK` retiene un bloqueo exclusivo (X) en la tabla. Esto significa que no se pueden insertar filas mediante varias operaciones de inserción que se ejecutan simultáneamente. 

Sin embargo, a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y del nivel de compatibilidad de la base de datos 130, una única instrucción `INSERT INTO … SELECT` se puede ejecutar en paralelo al realizar la inserción en montones o en índices de almacén de columnas (CCI) agrupados. Las inserciones paralelas son posibles cuando se usa la sugerencia `TABLOCK`.  

El paralelismo de la instrucción anterior tiene los siguientes requisitos, que son similares a los requisitos para el registro mínimo:  
-   La tabla de destino es un montón vacío o no vacío.  
-   La tabla de destino tiene un índice de almacén de columnas agrupado (CCI), pero no índices no agrupados.  
-   La tabla de destino no tiene una columna de identidad con IDENTITY_INSERT establecido en OFF.  
-   La sugerencia `TABLOCK` se especifica para la tabla de destino.

En los escenarios en los que se cumplen los requisitos de registro mínimo y de inserción en paralelo, ambas mejoras funcionarán juntas para garantizar el máximo rendimiento de las operaciones de carga de datos.

> [!NOTE]
> Las inserciones en las tablas temporales locales (identificadas por el prefijo #) y las tablas temporales globales (identificadas por prefijos ##) también se habilitan para el paralelismo mediante la sugerencia TABLOCK.
  
#### <a name="using-openrowset-and-bulk-to-bulk-import-data"></a>Usar OPENROWSET y BULK para datos de importación en bloque  
 La función OPENROWSET puede aceptar las siguientes sugerencias de tabla, que proporcionan optimizaciones de carga masiva con la instrucción INSERT:  
  
-   La sugerencia `TABLOCK` puede reducir al mínimo el número de registros para la operación de inserción. El modelo de recuperación de la base de datos debe establecerse en registro simple o masivo, y la tabla de destino no se puede utilizar en la replicación. Para más información, vea [Requisitos previos para el registro mínimo durante la importación en bloque](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
-   La sugerencia `TABLOCK` puede habilitar operaciones de inserción paralelas. La tabla de destino es un montón o un índice de almacén de columnas agrupado (CCI) sin índices no agrupados, y la tabla de destino no puede tener una columna de identidad especificada.  
-   La sugerencia `IGNORE_CONSTRAINTS` puede deshabilitar temporalmente la comprobación de restricciones FOREIGN KEY y CHECK.  
-   La sugerencia `IGNORE_TRIGGERS` puede deshabilitar temporalmente la ejecución de desencadenadores.  
-   La sugerencia `KEEPDEFAULTS` permite la inserción del valor predeterminado de la columna de una tabla, si existe, en lugar de NULL, cuando falta el valor del registro de datos de esa columna.  
-   La sugerencia `KEEPIDENTITY` permite que se usen los valores de identidad en el archivo de datos importado para la columna de identidad en la tabla de destino.  
  
Estas optimizaciones son similares a las que están disponibles con el comando `BULK INSERT`. Para obtener más información, vea [Sugerencias de tabla &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-table.md).  
  
## <a name="data-types"></a>Tipo de datos  
 Al insertar filas, considere el comportamiento de los tipos de datos siguientes:  
  
-   Si se va a cargar un valor en columnas con un tipo de datos **char**, **varchar** o **varbinary**, el relleno o el truncamiento de los espacios en blanco finales (espacios para **char** y **varchar**, ceros para **varbinary**) se determinan a partir del valor de la opción SET ANSI_PADDING definida para la columna al crear la tabla. Para obtener más información, vea [SET ANSI_PADDING &#40;Transact-SQL&#41;](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
     En la siguiente tabla se muestra la operación predeterminada cuando SET ANSI_PADDING es OFF.  
  
    |Tipo de datos|Operación predeterminada|  
    |---------------|-----------------------|  
    |**char**|Rellena el valor con espacios hasta el ancho definido de la columna.|  
    |**varchar**|Quita los espacios finales hasta el último carácter distinto de espacio o hasta un carácter de espacio único para las cadenas compuestas solamente de espacios.|  
    |**varbinary**|Quita los ceros finales.|  
  
-   Si se carga una cadena vacía (' ') en una columna con un tipo de datos **varchar** o **text**, la operación predeterminada consiste en cargar una cadena de longitud cero.  
  
-   Al insertar un valor NULL en una columna **text** o **image**, no se crea un puntero de texto válido ni se asigna previamente una página de texto de 8 KB.  
  
-   En las columnas creadas con el tipo de datos **uniqueidentifier** se almacenan valores binarios de 16 bytes con formato especial. A diferencia de las columnas de identidad, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] no genera automáticamente valores de columnas con el tipo de datos **uniqueidentifier**. Durante una operación de inserción, se pueden usar variables con un tipo de datos **uniqueidentifier** y constantes de cadena con el formato *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* (36 caracteres incluidos los guiones, donde *x* es un dígito hexadecimal de los intervalos 0-9 o a-f) de las columnas **uniqueidentifier**. Por ejemplo, 6F9619FF-8B86-D011-B42D-00C04FC964FF es un valor válido de una columna o variable **uniqueidentifier**. Use la función [NEWID()](../../t-sql/functions/newid-transact-sql.md) para obtener un identificador único global (GUID).  
  
### <a name="inserting-values-into-user-defined-type-columns"></a>Insertar valores en columnas de tipo definido por el usuario  
 Puede insertar valores en columnas de tipo definido por el usuario si:  
  
-   Proporciona un valor del tipo definido por el usuario.  
  
-   Suministrar un valor de un tipo de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], siempre y cuando el tipo definido por el usuario admita la conversión implícita o explícita desde ese tipo. En el siguiente ejemplo se muestra cómo insertar un valor en una columna de tipo definido por el usuario`Point` por medio de la conversión explícita a partir de una cadena.  
  
    ```sql
    INSERT INTO Cities (Location)  
    VALUES ( CONVERT(Point, '12.3:46.2') );  
    ```  
  
     También se puede suministrar un valor binario sin realizar ninguna conversión explícita, dado que todos los tipos definidos por el usuario se pueden convertir implícitamente a partir de este valor binario.  
  
-   Llama a una función definida por el usuario que devuelve un valor del tipo definido por el usuario. En el siguiente ejemplo se utiliza una función `CreateNewPoint()` definida por el usuario para crear un valor nuevo del tipo `Point` definido por el usuario e insertar el valor en la tabla `Cities`.  
  
    ```sql
    INSERT INTO Cities (Location)  
    VALUES ( dbo.CreateNewPoint(x, y) );  
    ```  
  
## <a name="error-handling"></a>Tratamiento de errores  
 Puede implementar el tratamiento de errores para la instrucción INSERT especificando la instrucción en una construcción TRY…CATCH.  
  
 Si una instrucción INSERT infringe una restricción o una regla, o si contiene un valor incompatible con el tipo de datos de la columna, la instrucción no se puede ejecutar y se recibe un mensaje de error.  
  
 Si INSERT carga varias filas con SELECT o EXECUTE, cualquier infracción de una regla o restricción que se produzca en los valores que se cargan provoca que se detenga la instrucción y que no se carguen filas.  
  
 Cuando una instrucción INSERT detecta un error aritmético (desbordamiento, división entre cero o error de dominio) al evaluar una expresión, [!INCLUDE[ssDE](../../includes/ssde-md.md)] trata dichos errores como si SET ARITHABORT estuviera establecido en ON. El lote se detiene y se devuelve un mensaje de error. Al evaluar una expresión con SET ARITHABORT y SET ANSI_WARNINGS en OFF, si una instrucción INSERT, DELETE o UPDATE encuentra un error aritmético, desbordamiento, división entre cero o error de dominio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inserta o actualiza un valor NULL. Si la columna de destino no acepta valores NULL, no se puede efectuar la acción de inserción o actualización y el usuario recibe un error.  
  
## <a name="interoperability"></a>Interoperabilidad  
 Cuando se define un desencadenador `INSTEAD OF` en las acciones INSERT en una tabla o vista, se ejecuta el desencadenador en lugar de la instrucción INSERT. Para obtener más información sobre los desencadenadores `INSTEAD OF`, vea [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 Cuando se insertan valores en tablas remotas y no se especifican todos los valores de todas las columnas, debe identificar las columnas en las que se deben insertar los valores especificados.  
  
 Cuando se utiliza TOP con INSERT las filas a las que hace referencia no están organizadas de ninguna manera y la cláusula ORDER BY no se puede especificar directamente en esta instrucción. Si necesita usar TOP para insertar las filas en un orden cronológico significativo, debe utilizar TOP junto con una cláusula ORDER BY que se especifica en una instrucción de subselección. Vea la sección Ejemplos que aparece más adelante en este tema.
 
Las consultas INSERT en las que se usa SELECT con ORDER BY para rellenar filas garantizan el modo en que se calculan los valores de identidad, pero no el orden en el que las filas se insertan.

En Almacenamiento de datos paralelos, la cláusula ORDER BY no es válida en VIES, CREATE TABLE AS SELECT, INSERT SELECT, funciones insertadas, tablas derivadas, subconsultas ni expresiones de tabla común, salvo que se especifique también TOP.
  
## <a name="logging-behavior"></a>Comportamiento del registro  
 La instrucción INSERT siempre se registra completamente excepto cuando se usa la función OPENROWSET con la palabra clave BULK o cuando se usa `INSERT INTO <target_table> SELECT <columns> FROM <source_table>`. Estas operaciones pueden ser registradas mínimamente. Para obtener más información, vea la sección "Prácticas recomendadas para la carga masiva de datos" anteriormente en este tema.  
  
## <a name="security"></a>Seguridad  
 Durante una conexión de servidores vinculados, el servidor de envío proporciona un nombre de inicio de sesión y una contraseña para conectarse en su nombre al servidor de recepción. Para que esta conexión funcione, debe crear una asignación de inicio de sesión entre los servidores vinculados usando [sp_addlinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md).  
  
 Cuando utilice OPENROWSET (BULK…), es importante que entienda el modo en el que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controla la suplantación. Para más información, vea "Consideraciones relativas a la seguridad" en [Importación en bloque de datos mediante las instrucciones BULK INSERT u OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).  
  
### <a name="permissions"></a>Permisos  
 El permiso INSERT es obligatorio en la tabla de destino.  
  
 Los permisos para INSERT se adjudican de forma predeterminada a los miembros del rol fijo de servidor `sysadmin`, a los roles fijos de base de datos `db_owner` y `db_datawriter`, y al propietario de la tabla. Los miembros de los roles `sysadmin`, `db_owner` y `db_securityadmin` y el propietario de la tabla pueden transferir permisos a otros usuarios.  
  
 Para ejecutar INSERT con la opción BULK de la función OPENROWSET, debe ser miembro de los roles fijos de servidor `sysadmin` o `bulkadmin`.  
  
##  <a name="examples"></a><a name="InsertExamples"></a> Ejemplos  
  
|Category|Elementos de sintaxis ofrecidos|  
|--------------|------------------------------|  
|[Sintaxis básica](#BasicSyntax)|INSERT • constructor con valores de tabla|  
|[Tratar los valores de columna](#ColumnValues)|IDENTITY • NEWID • valores predeterminados • tipos definidos por el usuario|  
|[Insertar datos de otras tablas](#OtherTables)|INSERT…SELECT • INSERT…EXECUTE • WITH expresión de tabla común • TOP • OFFSET FETCH|  
|[Especificar objetos de destino que no sean tablas estándar](#TargetObjects)|Vistas • variables de tabla|  
|[Insertar filas en una tabla remota](#RemoteTables)|Servidor vinculado • función de conjunto de filas OPENQUERY • función de conjunto de filas OPENDATASOURCE|  
|[Cargar datos de forma masiva de tablas o archivos de datos](#BulkLoad)|INSERT…SELECT • función OPENROWSET|  
|[Invalidar el comportamiento predeterminado del optimizador de consultas mediante sugerencias](#TableHints)|Sugerencias de tabla|  
|[Capturar los resultados de la instrucción INSERT](#CaptureResults)|Cláusula OUTPUT|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Sintaxis básica  
 Los ejemplos de esta sección demuestran la funcionalidad básica de la instrucción INSERT usando la sintaxis mínima requerida.  
  
#### <a name="a-inserting-a-single-row-of-data"></a>A. Insertar una sola fila de datos  
 En el siguiente ejemplo se inserta una fila en la tabla `Production.UnitMeasure` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Las columnas de esta tabla son `UnitMeasureCode`, `Name` y `ModifiedDate`. Dado que los valores para todas las columnas se suministran e incluyen en el mismo orden que las columnas de la tabla, no es necesario especificar los nombres de columna en la lista de columnas *.*  
  
```sql
INSERT INTO Production.UnitMeasure  
VALUES (N'FT', N'Feet', '20080414');  
```  
  
#### <a name="b-inserting-multiple-rows-of-data"></a>B. Insertar varias filas de datos  
 En el siguiente ejemplo se usa el [constructor de valores de tabla](../../t-sql/queries/table-value-constructor-transact-sql.md) para insertar tres filas en la tabla `Production.UnitMeasure` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] en una sola instrucción INSERT. Dado que los valores para todas las columnas se suministran e incluyen en el mismo orden que las columnas de la tabla, no es necesario especificar los nombres de columna en la lista de columnas.  
  
```sql
INSERT INTO Production.UnitMeasure  
VALUES (N'FT2', N'Square Feet ', '20080923'), (N'Y', N'Yards', '20080923')
    , (N'Y3', N'Cubic Yards', '20080923');  
```  
  
#### <a name="c-inserting-data-that-is-not-in-the-same-order-as-the-table-columns"></a>C. Insertar datos que no están en el mismo orden que las columnas de la tabla  
 En el siguiente ejemplo se utiliza una lista de columnas para especificar de forma explícita los valores insertados en cada columna. El orden de las columnas de la tabla `Production.UnitMeasure` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] es, `UnitMeasureCode`, `Name`, `ModifiedDate`; no obstante, las columnas no se incluyen en dicho orden en *column_list*.  
  
```sql
INSERT INTO Production.UnitMeasure (Name, UnitMeasureCode,  
    ModifiedDate)  
VALUES (N'Square Yards', N'Y2', GETDATE());  
```  
  
###  <a name="handling-column-values"></a><a name="ColumnValues"></a> Tratar los valores de columna  
 Los ejemplos de esta sección muestran métodos para insertar valores en columnas que se definen con una propiedad IDENTITY, un valor DEFAULT o se definen con tipos de datos como **uniqueidentifer** o columnas de un tipo definido por el usuario.  
  
#### <a name="d-inserting-data-into-a-table-with-columns-that-have-default-values"></a>D. Insertar datos en una tabla con columnas que tienen valores predeterminados  
 En el ejemplo siguiente se muestra la inserción de filas en una tabla con columnas que generan automáticamente un valor o tienen un valor predeterminado. `Column_1` es una columna calculada que genera automáticamente un valor concatenando una cadena con el valor insertado en `column_2`. `Column_2` se define con una restricción predeterminada. Si no se especifica un valor para esta columna, se usará el valor predeterminado. `Column_3` se define con el tipo de datos **rowversion**, que genera automáticamente un número binario único que se incrementa. `Column_4` no genera automáticamente ningún valor. Cuando no se especifica un valor para esta columna, se inserta NULL. La instrucción INSERT inserta filas que contienen valores para algunas de las columnas, pero no para todas. En la última instrucción INSERT, no se especifica ninguna columna y solamente se insertan los valores predeterminados con la cláusula DEFAULT VALUES.  
  
```sql
CREATE TABLE dbo.T1   
(  
    column_1 AS 'Computed column ' + column_2,   
    column_2 varchar(30)   
        CONSTRAINT default_name DEFAULT ('my column default'),  
    column_3 rowversion,  
    column_4 varchar(40) NULL  
);  
GO  
INSERT INTO dbo.T1 (column_4)   
    VALUES ('Explicit value');  
INSERT INTO dbo.T1 (column_2, column_4)   
    VALUES ('Explicit value', 'Explicit value');  
INSERT INTO dbo.T1 (column_2)   
    VALUES ('Explicit value');  
INSERT INTO T1 DEFAULT VALUES;   
GO  
SELECT column_1, column_2, column_3, column_4  
FROM dbo.T1;  
GO  
```  
  
#### <a name="e-inserting-data-into-a-table-with-an-identity-column"></a>E. Insertar datos en una tabla con una columna de identidad  
 En el siguiente ejemplo se muestran los distintos métodos para insertar datos en una columna de identidad. Las dos primeras instrucciones INSERT permiten generar valores de identidad para las filas nuevas. La tercera instrucción INSERT invalida la propiedad IDENTITY de la columna con la instrucción SET IDENTITY_INSERT e inserta un valor explícito en la columna de identidad.  
  
```sql
CREATE TABLE dbo.T1 ( column_1 int IDENTITY, column_2 VARCHAR(30));  
GO  
INSERT T1 VALUES ('Row #1');  
INSERT T1 (column_2) VALUES ('Row #2');  
GO  
SET IDENTITY_INSERT T1 ON;  
GO  
INSERT INTO T1 (column_1,column_2)   
    VALUES (-99, 'Explicit identity value');  
GO  
SELECT column_1, column_2  
FROM T1;  
GO  
```  
  
#### <a name="f-inserting-data-into-a-uniqueidentifier-column-by-using-newid"></a>F. Insertar datos en una columna uniqueidentifier mediante NEWID()  
 En el siguiente ejemplo se usa la función [NEWID](../../t-sql/functions/newid-transact-sql.md)() para obtener un GUID para `column_2`. A diferencia de las columnas de identidad, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] no genera automáticamente valores de columnas con el tipo de datos [uniqueidentifier](../../t-sql/data-types/uniqueidentifier-transact-sql.md), según se muestra en la segunda instrucción `INSERT`.  
  
```sql
CREATE TABLE dbo.T1   
(  
    column_1 int IDENTITY,   
    column_2 uniqueidentifier,  
);  
GO  
INSERT INTO dbo.T1 (column_2)   
    VALUES (NEWID());  
INSERT INTO T1 DEFAULT VALUES;   
GO  
SELECT column_1, column_2  
FROM dbo.T1;  
```  
  
#### <a name="g-inserting-data-into-user-defined-type-columns"></a>G. Insertar datos en columnas de tipo definido por el usuario  
 Las siguientes instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] insertan tres filas en la columna `PointValue` de la tabla `Points`. Esta columna usa un [tipo definido por el usuario CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) (UDT). El tipo de datos `Point` está compuesto por valores enteros X e Y que se exponen como propiedades del UDT. Debe utilizar las funciones CAST o CONVERT para convertir los valores X e Y separados por comas al tipo `Point`. Las dos primeras instrucciones usan la función CONVERT para convertir un valor de cadena al tipo `Point` y la tercera usa la función CAST. Para más información, vea [Manipulating UDT Data](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-manipulating-udt-data.md) (Manipular datos de UDT).  
  
```sql
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
###  <a name="inserting-data-from-other-tables"></a><a name="OtherTables"></a> Insertar datos de otras tablas  
 Los ejemplos de esta sección demuestran métodos para insertar filas de una tabla en otra.  
  
#### <a name="h-using-the-select-and-execute-options-to-insert-data-from-other-tables"></a>H. Usar las opciones SELECT y EXECUTE para insertar datos de otras tablas  
 En el siguiente ejemplo se muestra cómo insertar datos de una tabla en otra mediante INSERT…SELECT o INSERT…EXECUTE. Cada uno se basa en una instrucción SELECT con varias tablas que contiene una expresión y un valor literal en la lista de columnas.  
  
 La primera instrucción INSERT usa una instrucción SELECT para derivar los datos de las tablas de origen (`Employee`, `SalesPerson` y `Person`) en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] y almacenar el conjunto de resultados en la tabla `EmployeeSales`. La segunda instrucción INSERT usa la cláusula EXECUTE para llamar a un procedimiento almacenado que contiene la instrucción SELECT y la tercera instrucción INSERT usa la cláusula EXECUTE para hacer referencia a la instrucción SELECT como una cadena literal.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( DataSource   varchar(20) NOT NULL,  
  BusinessEntityID   varchar(11) NOT NULL,  
  LastName     varchar(40) NOT NULL,  
  SalesDollars money NOT NULL  
);  
GO  
CREATE PROCEDURE dbo.uspGetEmployeeSales   
AS   
    SET NOCOUNT ON;  
    SELECT 'PROCEDURE', sp.BusinessEntityID, c.LastName,   
        sp.SalesYTD   
    FROM Sales.SalesPerson AS sp    
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY sp.BusinessEntityID, c.LastName;  
GO  
--INSERT...SELECT example  
INSERT INTO dbo.EmployeeSales  
    SELECT 'SELECT', sp.BusinessEntityID, c.LastName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY sp.BusinessEntityID, c.LastName;  
GO  
--INSERT...EXECUTE procedure example  
INSERT INTO dbo.EmployeeSales   
EXECUTE dbo.uspGetEmployeeSales;  
GO  
--INSERT...EXECUTE('string') example  
INSERT INTO dbo.EmployeeSales   
EXECUTE   
('  
SELECT ''EXEC STRING'', sp.BusinessEntityID, c.LastName,   
    sp.SalesYTD   
    FROM Sales.SalesPerson AS sp   
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE ''2%''  
    ORDER BY sp.BusinessEntityID, c.LastName  
');  
GO  
--Show results.  
SELECT DataSource,BusinessEntityID,LastName,SalesDollars  
FROM dbo.EmployeeSales;  
```  
  
#### <a name="i-using-with-common-table-expression-to-define-the-data-inserted"></a>I. Usar la expresión de tabla común WITH para definir los datos insertados  
 En el siguiente ejemplo se crea la tabla `NewEmployee` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. Una expresión de tabla común (`EmployeeTemp`) define las filas de una o varias tablas que se van a insertar en la tabla `NewEmployee`. La instrucción INSERT hace referencia a las columnas de la expresión de tabla común.  
  
```sql
CREATE TABLE HumanResources.NewEmployee  
(  
    EmployeeID int NOT NULL,  
    LastName nvarchar(50) NOT NULL,  
    FirstName nvarchar(50) NOT NULL,  
    PhoneNumber Phone NULL,  
    AddressLine1 nvarchar(60) NOT NULL,  
    City nvarchar(30) NOT NULL,  
    State nchar(3) NOT NULL,   
    PostalCode nvarchar(15) NOT NULL,  
    CurrentFlag Flag  
);  
GO  
WITH EmployeeTemp (EmpID, LastName, FirstName, Phone,   
                   Address, City, StateProvince,   
                   PostalCode, CurrentFlag)  
AS (SELECT   
       e.BusinessEntityID, c.LastName, c.FirstName, pp.PhoneNumber,  
       a.AddressLine1, a.City, sp.StateProvinceCode,   
       a.PostalCode, e.CurrentFlag  
    FROM HumanResources.Employee e  
        INNER JOIN Person.BusinessEntityAddress AS bea  
        ON e.BusinessEntityID = bea.BusinessEntityID  
        INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
        INNER JOIN Person.PersonPhone AS pp  
        ON e.BusinessEntityID = pp.BusinessEntityID  
        INNER JOIN Person.StateProvince AS sp  
        ON a.StateProvinceID = sp.StateProvinceID  
        INNER JOIN Person.Person as c  
        ON e.BusinessEntityID = c.BusinessEntityID  
    )  
INSERT INTO HumanResources.NewEmployee   
    SELECT EmpID, LastName, FirstName, Phone,   
           Address, City, StateProvince, PostalCode, CurrentFlag  
    FROM EmployeeTemp;  
GO  
```  
  
#### <a name="j-using-top-to-limit-the-data-inserted-from-the-source-table"></a>J. Usar TOP para limitar los datos insertados de la tabla de origen  
 En el siguiente ejemplo se crea la tabla `EmployeeSales` y se insertan el nombre y los datos de ventas del año hasta la fecha para los primeros 5 empleados aleatorios de la tabla `HumanResources.Employee` en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La instrucción INSERT elige cualquiera de las 5 filas devueltas por la instrucción `SELECT`. La cláusula OUTPUT muestra las filas que se insertan en la tabla `EmployeeSales`. Observe que la cláusula ORDER BY de la instrucción SELECT no se utiliza para determinar los primeros 5 empleados.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   nvarchar(11) NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  YearlySales  money NOT NULL  
 );  
GO  
INSERT TOP(5)INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, 
        inserted.LastName, inserted.YearlySales  
    SELECT sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
```  
  
 Si necesita usar TOP para insertar las filas en un orden cronológico significativo, debe utilizar TOP junto con ORDER BY en una instrucción de subselección, tal y como se muestra en el siguiente ejemplo. La cláusula OUTPUT muestra las filas que se insertan en la tabla `EmployeeSales`. Observe que los primeros 5 empleados se insertan ahora según los resultados de la cláusula ORDER BY en lugar de las filas aleatorias.  
  
```sql
INSERT INTO dbo.EmployeeSales  
    OUTPUT inserted.EmployeeID, inserted.FirstName, 
        inserted.LastName, inserted.YearlySales  
    SELECT TOP (5) sp.BusinessEntityID, c.LastName, c.FirstName, sp.SalesYTD   
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.SalesYTD > 250000.00  
    ORDER BY sp.SalesYTD DESC;  
```  
  
###  <a name="specifying-target-objects-other-than-standard-tables"></a><a name="TargetObjects"></a> Especificar objetos de destino que no sean tablas estándar  
 En los ejemplos de esta sección se muestra cómo insertar filas especificando una variable de tabla o vista.  
  
#### <a name="k-inserting-data-by-specifying-a-view"></a>K. Insertar datos especificando una vista  
 En el siguiente ejemplo se especifica un nombre de vista como objeto de destino; sin embargo, la fila nueva se inserta en la tabla base subyacente. El orden de los valores de la instrucción `INSERT` debe coincidir con el orden de las columnas de la vista. Para más información, vea [Modificar datos mediante una vista](../../relational-databases/views/modify-data-through-a-view.md).  
  
```sql
CREATE TABLE T1 ( column_1 int, column_2 varchar(30));  
GO  
CREATE VIEW V1 AS   
SELECT column_2, column_1   
FROM T1;  
GO  
INSERT INTO V1   
    VALUES ('Row 1',1);  
GO  
SELECT column_1, column_2   
FROM T1;  
GO  
SELECT column_1, column_2  
FROM V1;  
GO  
```  
  
#### <a name="l-inserting-data-into-a-table-variable"></a>L. Insertar datos en una variable de tabla  
 En el siguiente ejemplo se especifica una variable de tabla como el objeto de destino en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql
-- Create the table variable.  
DECLARE @MyTableVar table(  
    LocationID int NOT NULL,  
    CostRate smallmoney NOT NULL,  
    NewCostRate AS CostRate * 1.5,  
    ModifiedDate datetime);  
  
-- Insert values into the table variable.  
INSERT INTO @MyTableVar (LocationID, CostRate, ModifiedDate)  
    SELECT LocationID, CostRate, GETDATE() 
    FROM Production.Location  
    WHERE CostRate > 0;  
  
-- View the table variable result set.  
SELECT * FROM @MyTableVar;  
GO  
```  
  
###  <a name="inserting-rows-into-a-remote-table"></a><a name="RemoteTables"></a> Insertar filas en una tabla remota  
 Los ejemplos de esta sección demuestran cómo insertar filas en una tabla de destino remota usando un [servidor vinculado](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) o una [función de conjunto de filas](../functions/opendatasource-transact-sql.md) para hacer referencia a la tabla remota.  
  
#### <a name="m-inserting-data-into-a-remote-table-by-using-a-linked-server"></a>M. Insertar datos en una tabla remota mediante un servidor vinculado  
 El ejemplo siguiente inserta filas en una tabla remota. En el ejemplo primero se crea un vínculo al origen de datos remoto mediante [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). El nombre del servidor vinculado, `MyLinkServer`, se especifica después como parte del nombre de objeto de cuatro partes con el formato *server.catalog.schema.object*.  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' 
-- or 'server_nameinstance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source in the FROM clause using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
INSERT INTO MyLinkServer.AdventureWorks2012.HumanResources.Department (Name, GroupName)  
VALUES (N'Public Relations', N'Executive General and Administration');  
GO  
```  
  
#### <a name="n-inserting-data-into-a-remote-table-by-using-the-openquery-function"></a>Hora Insertar datos en una tabla remota con una función OPENQUERY  
 En el siguiente ejemplo se inserta una fila en una tabla remota especificando la función de conjunto de filas [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md). En este ejemplo se usa el nombre del servidor vinculado creado en el ejemplo anterior.  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql
INSERT OPENQUERY (MyLinkServer, 
    'SELECT Name, GroupName 
     FROM AdventureWorks2012.HumanResources.Department')  
VALUES ('Environmental Impact', 'Engineering');  
GO  
```  
  
#### <a name="o-inserting-data-into-a-remote-table-by-using-the-opendatasource-function"></a>O. Insertar datos en una tabla remota con una función OPENDATASOURCE  
 En el ejemplo siguiente se inserta una fila en una tabla remota mediante la especificación de la función de conjunto de filas [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md). Especifique un nombre de servidor válido para el origen de datos con el formato *server_name* o *server_name\instance_name*.  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql
-- Use the OPENDATASOURCE function to specify the remote data source.  
-- Specify a valid server name for Data Source using the format 
-- server_name or server_nameinstance_name.  
  
INSERT INTO OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department (Name, GroupName)  
    VALUES (N'Standards and Methods', 'Quality Assurance');  
GO  
```  
  
#### <a name="p-inserting-into-an-external-table-created-using-polybase"></a>P. Insertar en una tabla externa creada con PolyBase  
 Exporte datos de SQL Server a Hadoop o Azure Storage. En primer lugar, cree una tabla externa que apunte al directorio o archivo de destino. A continuación, utilice INSERT INTO para exportar datos de una tabla de SQL Server local a un origen de datos externo. La instrucción INSERT INTO crea el archivo o directorio de destino si no existe y los resultados de la instrucción SELECT se exportan a la ubicación especificada en el formato de archivo especificado.  Para obtener más información, vea [Introducción a PolyBase](../../relational-databases/polybase/polybase-guide.md).  
  
**Se aplica a**: [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
```sql
-- Create an external table.   
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
        [FirstName] char(25) NOT NULL,   
        [LastName] char(25) NOT NULL,   
        [YearlyIncome] float NULL,   
        [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
        LOCATION='/old_data/2009/customerdata.tbl',  
        DATA_SOURCE = HadoopHDP2,  
        FILE_FORMAT = TextFileFormat,  
        REJECT_TYPE = VALUE,  
        REJECT_VALUE = 0  
);  
  
-- Export data: Move old data to Hadoop while keeping 
-- it query-able via external table.  

INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  
  
###  <a name="bulk-loading-data-from-tables-or-data-files"></a><a name="BulkLoad"></a> Cargar datos de forma masiva de tablas o archivos de datos  
 Los ejemplos de esta sección muestran dos métodos para cargar datos de forma masiva en una tabla mediante la instrucción INSERT.  
  
#### <a name="q-inserting-data-into-a-heap-with-minimal-logging"></a>Q. Insertar datos en un montón con registro mínimo  
 El ejemplo siguiente crea una tabla nueva (un montón) e inserta los datos en ella desde otra tabla con registro mínimo. El ejemplo supone que el modelo de recuperación de la base de datos `AdventureWorks2012` está establecido en FULL. Para asegurarse de que se utiliza el registro mínimo, el modelo de recuperación de la base de datos `AdventureWorks2012` se establece en BULK_LOGGED antes de que las filas se inserten y se restablece en FULL después de la instrucción INSERT INTO…SELECT. Además, se especifica la sugerencia TABLOCK para la tabla de destino `Sales.SalesHistory`. Esto asegura que la instrucción use el espacio mínimo en el registro de transacciones y funcione eficazmente.  
  
```sql
-- Create the target heap.  
CREATE TABLE Sales.SalesHistory(  
    SalesOrderID int NOT NULL,  
    SalesOrderDetailID int NOT NULL,  
    CarrierTrackingNumber nvarchar(25) NULL,  
    OrderQty smallint NOT NULL,  
    ProductID int NOT NULL,  
    SpecialOfferID int NOT NULL,  
    UnitPrice money NOT NULL,  
    UnitPriceDiscount money NOT NULL,  
    LineTotal money NOT NULL,  
    rowguid uniqueidentifier ROWGUIDCOL  NOT NULL,  
    ModifiedDate datetime NOT NULL );  
GO  
-- Temporarily set the recovery model to BULK_LOGGED.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY BULK_LOGGED;  
GO  
-- Transfer data from Sales.SalesOrderDetail to Sales.SalesHistory  
INSERT INTO Sales.SalesHistory WITH (TABLOCK)  
    (SalesOrderID,   
     SalesOrderDetailID,  
     CarrierTrackingNumber,   
     OrderQty,   
     ProductID,   
     SpecialOfferID,   
     UnitPrice,   
     UnitPriceDiscount,  
     LineTotal,   
     rowguid,   
     ModifiedDate)  
SELECT * FROM Sales.SalesOrderDetail;  
GO  
-- Reset the recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
#### <a name="r-using-the-openrowset-function-with-bulk-to-bulk-load-data-into-a-table"></a>R. Usar la función OPENROWSET con BULK para cargar datos de forma masiva en una tabla  
 En el ejemplo siguiente se insertan filas de un archivo de datos en una tabla especificando la función OPENROWSET. La sugerencia de tabla IGNORE_TRIGGERS se especifica para la optimización del rendimiento. Para obtener más ejemplos, vea [Importación en bloque de datos mediante las instrucciones BULK INSERT u OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
```sql
INSERT INTO HumanResources.Department WITH (IGNORE_TRIGGERS) (Name, GroupName)  
SELECT b.Name, b.GroupName   
FROM OPENROWSET (  
    BULK 'C:SQLFilesDepartmentData.txt',  
    FORMATFILE = 'C:SQLFilesBulkloadFormatFile.xml',  
    ROWS_PER_BATCH = 15000)AS b ;  
```  
  
###  <a name="overriding-the-default-behavior-of-the-query-optimizer-by-using-hints"></a><a name="TableHints"></a> Invalidar el comportamiento predeterminado del optimizador de consultas mediante sugerencias  
 Los ejemplos de esta sección demuestran cómo usar [sugerencias de tabla](../../t-sql/queries/hints-transact-sql-table.md) para invalidar de forma temporal el comportamiento predeterminado del optimizador de consultas cuando se procesa la instrucción INSERT.  
  
> [!CAUTION]  
>  Como el optimizador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suele seleccionar el mejor plan de ejecución de una consulta, se recomienda que únicamente los administradores de bases de datos y desarrolladores experimentados utilicen las sugerencias como último recurso.  
  
#### <a name="s-using-the-tablock-hint-to-specify-a-locking-method"></a>S. Usar la sugerencia TABLOCK para especificar un método de bloqueo  
 En el ejemplo siguiente se especifica que se aplique un bloqueo exclusivo (X) en la tabla Production.Location y que se mantenga hasta que finalice la instrucción INSERT.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].  
  
```sql
INSERT INTO Production.Location WITH (XLOCK)  
(Name, CostRate, Availability)  
VALUES ( N'Final Inventory', 15.00, 80.00);  
```  
  
###  <a name="capturing-the-results-of-the-insert-statement"></a><a name="CaptureResults"></a> Capturar los resultados de la instrucción INSERT  
 Los ejemplos de esta sección demuestran cómo usar la [cláusula OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) para devolver información de cada fila afectada por una instrucción INSERT o de expresiones que se basan en esta instrucción. Estos resultados se pueden devolver a la aplicación de procesamiento para que los utilice en mensajes de confirmación, archivado y otros requisitos similares de una aplicación.  
  
#### <a name="t-using-output-with-an-insert-statement"></a>T. Usar OUTPUT con una instrucción INSERT  
 En el siguiente ejemplo se inserta una fila en la tabla `ScrapReason` y se utiliza la cláusula `OUTPUT` para devolver los resultados de la instrucción a la variable de la tabla `@MyTableVar`. Dado que la columna `ScrapReasonID` se define con una propiedad `IDENTITY`, no se especifica ningún valor en la instrucción `INSERT` para dicha columna. No obstante, debe tener en cuenta que el valor generado por [!INCLUDE[ssDE](../../includes/ssde-md.md)] para la columna se devuelve en la cláusula `OUTPUT` de la columna `INSERTED.ScrapReasonID`.  
  
```sql
DECLARE @MyTableVar table( NewScrapReasonID smallint,  
                           Name varchar(50),  
                           ModifiedDate datetime);  
INSERT Production.ScrapReason  
    OUTPUT INSERTED.ScrapReasonID, INSERTED.Name, INSERTED.ModifiedDate  
        INTO @MyTableVar  
VALUES (N'Operator error', GETDATE());  
  
--Display the result set of the table variable.  
SELECT NewScrapReasonID, Name, ModifiedDate FROM @MyTableVar;  
--Display the result set of the table.  
SELECT ScrapReasonID, Name, ModifiedDate   
FROM Production.ScrapReason;  
```  
  
#### <a name="u-using-output-with-identity-and-computed-columns"></a>U. Usar OUTPUT con columnas de identidad y calculadas  
 En el siguiente ejemplo se crea la tabla `EmployeeSales` y, a continuación, se insertan varias filas en ella usando una instrucción INSERT con una instrucción SELECT para recuperar los datos de las tablas de origen. La tabla `EmployeeSales` contiene una columna de identidad (`EmployeeID`) y una columna calculada (`ProjectedSales`). Puesto que [!INCLUDE[ssDE](../../includes/ssde-md.md)] genera estos valores durante la operación de inserción, ninguna de estas columnas se puede definir en `@MyTableVar`.  
  
```sql
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   int IDENTITY (1,5)NOT NULL,  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  CurrentSales money NOT NULL,  
  ProjectedSales AS CurrentSales * 1.10   
);  
GO  
DECLARE @MyTableVar table(  
  LastName     nvarchar(20) NOT NULL,  
  FirstName    nvarchar(20) NOT NULL,  
  CurrentSales money NOT NULL  
  );  
  
INSERT INTO dbo.EmployeeSales (LastName, FirstName, CurrentSales)  
  OUTPUT INSERTED.LastName,   
         INSERTED.FirstName,   
         INSERTED.CurrentSales  
  INTO @MyTableVar  
    SELECT c.LastName, c.FirstName, sp.SalesYTD  
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY c.LastName, c.FirstName;  
  
SELECT LastName, FirstName, CurrentSales  
FROM @MyTableVar;  
GO  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM dbo.EmployeeSales;  
```  
  
#### <a name="v-inserting-data-returned-from-an-output-clause"></a>V. Insertar los datos devueltos por una cláusula OUTPUT  
 En el ejemplo siguiente se capturan los datos devueltos por la cláusula OUTPUT de una instrucción MERGE y se insertan en otra tabla. La instrucción MERGE actualiza diariamente la columna `Quantity` de la tabla `ProductInventory`, en función de los pedidos procesados en la tabla `SalesOrderDetail` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. También elimina las filas correspondientes a los productos cuyas existencias se colocan en el valor 0. En el ejemplo, se capturan las filas que se eliminan y se insertan en otra tabla, `ZeroInventory`, que realiza el seguimiento de los productos sin existencias.  
  
```sql
--Create ZeroInventory table.  
CREATE TABLE Production.ZeroInventory (DeletedProductID int, RemovedOnDate DateTime);  
GO  
  
INSERT INTO Production.ZeroInventory (DeletedProductID, RemovedOnDate)  
SELECT ProductID, GETDATE()  
FROM  
(   MERGE Production.ProductInventory AS pi  
    USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
           JOIN Sales.SalesOrderHeader AS soh  
           ON sod.SalesOrderID = soh.SalesOrderID  
           AND soh.OrderDate = '20070401'  
           GROUP BY ProductID) AS src (ProductID, OrderQty)  
    ON (pi.ProductID = src.ProductID)  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0  
        THEN DELETE  
    WHEN MATCHED  
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    OUTPUT $action, deleted.ProductID) AS Changes (Action, ProductID)  
WHERE Action = 'DELETE';  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were inserted';  
GO  
SELECT DeletedProductID, RemovedOnDate FROM Production.ZeroInventory;  
```  

#### <a name="w-inserting-data-using-the-select-option"></a>Hora Insertar datos con la opción SELECT  
 En el siguiente ejemplo se muestra cómo insertar varias filas de datos usando una instrucción INSERT con una opción SELECT. En la primera instrucción `INSERT` se usa directamente una instrucción `SELECT` para recuperar datos de la tabla de origen y, luego, almacenar el conjunto de resultados en la tabla `EmployeeTitles`.  
  
```sql
CREATE TABLE EmployeeTitles  
( EmployeeKey   INT NOT NULL,  
  LastName     varchar(40) NOT NULL,  
  Title      varchar(50) NOT NULL  
);  
INSERT INTO EmployeeTitles  
    SELECT EmployeeKey, LastName, Title   
    FROM ssawPDW.dbo.DimEmployee  
    WHERE EndDate IS NULL;  
```  
  
#### <a name="x-specifying-a-label-with-the-insert-statement"></a>X. Especificar una etiqueta con la instrucción INSERT  
 En el siguiente ejemplo se explica el uso de una etiqueta con una instrucción INSERT.  
  
```sql
-- Uses AdventureWorks  
  
INSERT INTO DimCurrency   
VALUES (500, N'C1', N'Currency1')  
OPTION ( LABEL = N'label1' );  
```  
  
#### <a name="y-using-a-label-and-a-query-hint-with-the-insert-statement"></a>Y. Usar una etiqueta y una sugerencia de consulta con la instrucción INSERT  
 Esta consulta muestra la sintaxis básica para usar una etiqueta y una sugerencia de combinación de consulta con la instrucción INSERT. Una vez enviada la consulta al nodo de control, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], que se ejecuta en los nodos de ejecución, se aplica la estrategia de combinación hash al generar el plan de consulta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información sobre las sugerencias de combinación y cómo usar la cláusula OPTION, vea [OPTION (PDW de SQL Server)](../../t-sql/queries/option-clause-transact-sql.md).  
  
```sql
-- Uses AdventureWorks  
  
INSERT INTO DimCustomer (CustomerKey, CustomerAlternateKey, 
    FirstName, MiddleName, LastName )   
SELECT ProspectiveBuyerKey, ProspectAlternateKey, 
    FirstName, MiddleName, LastName  
FROM ProspectiveBuyer p JOIN DimGeography g ON p.PostalCode = g.PostalCode  
WHERE g.CountryRegionCode = 'FR'  
OPTION ( LABEL = 'Add French Prospects', HASH JOIN);  
```  
  
## <a name="see-also"></a>Consulte también  
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [IDENTITY &#40;propiedad&#41; &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [Cláusula OUTPUT &#40;Transact-SQL&#41;](../../t-sql/queries/output-clause-transact-sql.md)   
 [Usar las tablas insertadas y eliminadas](../../relational-databases/triggers/use-the-inserted-and-deleted-tables.md)  
  
