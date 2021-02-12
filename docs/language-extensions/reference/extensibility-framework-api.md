---
title: API de marco de extensibilidad
titleSuffix: SQL Server Language Extensions
description: Puede usar el marco de extensibilidad para escribir extensiones de lenguaje de programación para SQL Server. La API Extensibility Framework para Microsoft SQL Server se puede usar con una extensión de lenguaje para interactuar con SQL Server e intercambiar datos.
author: dphansen
ms.author: davidph
ms.date: 10/09/2020
ms.topic: reference
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 21099123d8065fad63e1526a3377f1904aed87b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016535"
---
# <a name="extensibility-framework-api-for-sql-server"></a>API Extensibility Framework para SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Puede usar el marco de extensibilidad para escribir extensiones de lenguaje de programación para SQL Server. La API Extensibility Framework para Microsoft SQL Server se puede usar con una extensión de lenguaje para interactuar con SQL Server e intercambiar datos.

Como autor de extensiones de lenguaje, puede usar esta referencia junto con las extensiones de código abierto para entender cómo usar la API para escribir las suyas propias. Puede encontrar el código fuente de las extensiones de lenguaje en [aka.ms/mssql-lang-extensions](https://aka.ms/mssql-lang-extensions).

Busque la información sobre la sintaxis y los argumentos relativos a todas las funciones de la API siguientes.

## <a name="return-value"></a>Valor devuelto

Todas las funciones devuelven un parámetro *SQLRETURN*. Si el valor es distinto de *SQL_SUCCESS*, se tratará como un error y se detendrá la ejecución del script.

## <a name="standard-output"></a>Salida estándar

Se realizará el seguimiento de todas las salidas de la extensión a los flujos de salida o de error estándar en el archivo de registro de la sesión y, finalmente, se realizará el seguimiento hasta SQL Server (similar a lo que se muestra en la pestaña de mensajes de SSMS).


## <a name="init"></a>Init

A esta función solo se llama una vez, y se usa para inicializar el entorno de ejecución. 

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN Init(
    SQLCHAR *ExtensionParams,
    SQLULEN ExtensionParamsLength,
    SQLCHAR *ExtensionPath,
    SQLULEN ExtensionPathLength,
    SQLCHAR *PublicLibraryPath,
    SQLULEN PublicLibraryPathLength,
    SQLCHAR *PrivateLibraryPath,
    SQLULEN PrivateLibraryPathLength
);
```

### <a name="arguments"></a>Argumentos

*ExtensionParams*  
\[Entrada\] Cadena terminada en NULL que contiene el valor `PARAMETERS` proporcionado durante [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) o [ALTER EXTERNAL LANGUAGE](../../t-sql/statements/alter-external-language-transact-sql.md).

*ExtensionParamsLength*  
\[Entrada\] Longitud en bytes de *ExtensionParams* (excepto el carácter de terminación NULL).

*ExtensionPath*  
\[Entrada\] Cadena UTF-8 terminada en NULL que contiene la ruta de acceso absoluta al directorio de instalación de la extensión.

*ExtensionPathLength*  
\[Entrada\] Longitud en bytes de *ExtensionPath* (excepto el carácter de terminación NULL).

*PublicLibraryPath*  
\[Entrada\] Cadena UTF-8 terminada en NULL que contiene la ruta de acceso absoluta al directorio de bibliotecas externas públicas para este lenguaje externo.

*PublicLibraryPathLength*  
\[Entrada\] Longitud en bytes de *PublicLibraryPath* (excepto el carácter de terminación NULL).

*PrivateLibraryPath*  
\[Entrada\] Cadena UTF-8 terminada en NULL que contiene la ruta de acceso absoluta al directorio de bibliotecas externas privadas para este usuarios y este lenguaje externo.

*PrivateLibraryPathLength*  
\[Entrada\] Longitud en bytes de *PrivateLibraryPath* (excepto el carácter de terminación NULL).

## <a name="initsession"></a>InitSession

A esta función solo se llama una vez por sesión, y se usa para inicializar la configuración específica de la sesión.

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN InitSession(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLCHAR*        Script,
    SQLULEN         ScriptLength,
    SQLUSMALLINT    InputSchemaColumnsNumber,
    SQLUSMALLINT    ParametersNumber
    SQLCHAR*        InputDataName,
    SQLUSMALLINT    InputDataNameLength,
    SQLCHAR*        OutputDataName,
    SQLUSMALLINT    OutputDataNameLength
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*TaskId*  
\[Entrada\] Entero que identifica de forma única este proceso de ejecución.

En el caso de `@parallel = 1`, en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), este valor oscila entre 0 y el grado de paralelismo de la consulta.

*Script*  
\[Entrada\] Cadena UTF-8 terminada en NULL que contiene el elemento `@script` de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*ScriptLength*  
\[Entrada\] Longitud en bytes de *ScriptScript* (excepto el carácter de terminación NULL).

*InputSchemaColumnsNumber*  
\[Entrada\] Número de columnas del conjunto de resultados de `@input_data_1` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*ParametersNumber*  
\[Entrada\] Número de parámetros de entrada de `@params` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*InputDataName*  
\[Entrada\] Cadena UTF-8 terminada en NULL que contiene el elemento `@input_data_1_name` de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*InputDataNameLength*  
\[Entrada\] Longitud en bytes de *InputDataName* (excepto el carácter de terminación NULL).

*OutputDataName*  
\[Entrada\] Cadena UTF-8 terminada en NULL que contiene el elemento `@output_data_1_name` de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

*OutputDataNameLength*  
\[Entrada\] Longitud en bytes de *OutputDataName* (excepto el carácter de terminación NULL).

## <a name="initcolumn"></a>InitColumn

Inicializa la información de una columna determinada para una sesión concreta.

A esta función se llama para cada columna del conjunto de resultados de `@input_data_1` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

La estructura de columnas de este conjunto de resultados se denominará *esquema de entrada*.

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN InitColumn(
    SQLGUID       SessionId,
    SQLUSMALLINT  TaskId,
    SQLUSMALLINT  ColumnNumber,
    SQLCHAR*      ColumnName,
    SQLSMALLINT   ColumnNameLength,
    SQLSMALLINT   DataType,
    SQLULEN       ColumnSize,
    SQLSMALLINT   DecimalDigits,
    SQLSMALLINT   Nullable,
    SQLSMALLINT   PartitionByNumber,
    SQLSMALLINT   OrderByNumber
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*TaskId*  
\[Entrada\] Entero que identifica de forma única este proceso de ejecución.

En el caso de `@parallel = 1`, en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), este valor oscila entre 0 y el grado de paralelismo de la consulta.

*ColumnNumber*  
\[Entrada\] Entero que identifica el índice de esta columna en el esquema de entrada. Las columnas se numeran de forma secuencial en orden creciente, comenzando en el 0.

*ColumnName*  
\[Entrada\] Cadena UTF-8 terminada en NULL que contiene el nombre de la columna.

*ColumnNameLength*  
\[Entrada\] Longitud en bytes de *ColumnName* (excepto el carácter de terminación NULL).

*DataType*  
\[Entrada\] Tipo C de ODBC que identifica el tipo de datos de esta columna.

*ColumnSize*  
\[Entrada\] Tamaño máximo en bytes de los datos subyacentes en esta columna.

Para los tipos de datos SQL_C_CHAR, SQL_C_WCHAR y SQL_C_BINARY, los valores mayores que 8000 indican que esta columna representa objetos LOB con tamaños de hasta 2 GB.

*DecimalDigits*  
\[Entrada\] Dígitos decimales de los datos subyacentes en esta columna, como se define en [Dígitos decimales](../../odbc/reference/appendixes/decimal-digits.md).

*Admisión de valores NULL*  
\[Entrada\] Valor que indica si esta columna puede contener valores NULL. Valores posibles:

- SQL_NO_NULLS: la columna no puede contener valores NULL.
- SQL_NULLABLE: la columna puede contener valores NULL.

*PartitionByNumber*  
\[Entrada\] Valor que indica el índice de esta columna en la secuencia de `@input_data_1_partition_by_columns` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Las columnas se numeran de forma secuencial en orden creciente, comenzando en el 0. Si esta columna no se incluye en la secuencia, el valor es -1.

*OrderByNumber*  
\[Entrada\] Valor que indica el índice de esta columna en la secuencia de `@input_data_1_order_by_columns` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Las columnas se numeran de forma secuencial en orden creciente, comenzando en el 0. Si esta columna no se incluye en la secuencia, el valor es -1.

## <a name="initparam"></a>InitParam

Inicializa la información relacionada con un parámetro de entrada determinado para una sesión concreta.

A esta función se llama para cada parámetro de `@params` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN InitParam(
    SQLGUID      SessionId,
    SQLUSMALLINT TaskId,
    SQLUSMALLINT ParamNumber,
    SQLCHAR*     ParamName,
    SQLSMALLINT  ParamNameLength,
    SQLSMALLINT  DataType,
    SQLULEN      ParamSize,
    SQLSMALLINT  DecimalDigits,
    SQLPOINTER   ParamValue,
    SQLINTEGER   StrLen_or_Ind,
    SQLSMALLINT  InputOutputType
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*TaskId*  
\[Entrada\] Entero que identifica de forma única este proceso de ejecución.

En el caso de `@parallel = 1`, en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), este valor oscila entre 0 y el grado de paralelismo de la consulta.

*ParamNumber*  
\[Entrada\] Entero que identifica el índice de este parámetro. Los parámetros se numeran de forma secuencial en orden creciente, comenzando en el 0.

*ParamName*  
\[Entrada\] Cadena UTF-8 terminada en NULL que contiene el nombre del parámetro.

*ParamNameLength*  
\[Entrada\] Longitud en bytes de *ParamName* (excepto el carácter de terminación NULL).

*DataType*  
\[Entrada\] Tipo C de ODBC que identifica el tipo de datos de este parámetro.

*ParamSize*  
\[Entrada\] Tamaño máximo en bytes de los datos subyacentes en este parámetro.

Para los tipos de datos SQL_C_CHAR, SQL_C_WCHAR y SQL_C_BINARY, los valores mayores de 8000 indican que este parámetro representa objetos LOB con tamaños de hasta 2 GB.

*DecimalDigits*  
\[Entrada\] Dígitos decimales de los datos subyacentes en este parámetro, como se define en [Dígitos decimales](../../odbc/reference/appendixes/decimal-digits.md).

*ParamValue*  
\[Entrada\] Puntero a un búfer que contiene el valor del parámetro.

*StrLen_or_Ind*  
\[Entrada\] Valor entero que indica la longitud en bytes de *ParamValue*, o SQL_NULL_DATA para indicar que los datos son NULL.

StrLen_or_Ind\[col\] se puede omitir si una columna no admite un valor NULL y no representa uno de los tipos de datos siguientes: SQL_C_CHAR, SQL_C_WCHAR y SQL_C_BINARY, SQL_C_NUMERIC o SQL_C_TYPE_TIMESTAMP. En caso contrario, apunta a una matriz válida con elementos \[RowsNumber\], donde cada elemento contiene su longitud o datos de indicador NULL.

*InputOutputType*  
\[Entrada\] Tipo del parámetro. El argumento *InputOutputType* es uno de los valores siguientes:

- SQL_PARAM_INPUT
- SQL_PARAM_INPUT_OUTPUT

## <a name="execute"></a>Execute

Ejecute `@script` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

A esta función se puede llamar varias veces. Una vez para cada fragmento de secuencia y para cada partición de `@input_data_1_partition_by_columns` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN Execute(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN         RowsNumber,
    SQLPOINTER*     Data,
    SQLINTEGER**    StrLen_or_Ind,
    SQLUSMALLINT*   OutputSchemaColumnsNumber
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*TaskId*  
\[Entrada\] Entero que identifica de forma única este proceso de ejecución.

En el caso de `@parallel = 1`, en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), este valor oscila entre 0 y el grado de paralelismo de la consulta.

*RowsNumber*  
\[Entrada\] Número de filas de *Data*.

*Data*  
\[Entrada\] Matriz bidimensional que contiene el conjunto de resultados de `@input_data_1` n [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

El número total de columnas es *InputSchemaColumnsNumber* que se ha recibido en la llamada a [InitSession](#initsession). Cada columna contiene elementos *RowsNumber* que se deben interpretar según el tipo de columna de [InitColumn](#initcolumn).

No se garantiza que los elementos indicados como NULL en *StrLen_or_Ind* sean válidos y se deben omitir.

*StrLen_or_Ind*  
\[Entrada\] Matriz bidimensional que contiene el indicador de longitud/NULL para cada valor de *Data*. Valores posibles de cada celda:

- n, donde n > 0. Indica la longitud de los datos, en bytes.
- SQL_NULL_DATA, que indica un valor NULL.

El número total de columnas es *InputSchemaColumnsNumber* que se ha recibido en la llamada a [InitSession](#initsession). Cada columna contiene elementos *RowsNumber* que se deben interpretar según el tipo de columna de [InitColumn](#initcolumn).

StrLen_or_Ind\[col\] se puede omitir si una columna no admite un valor NULL y no representa uno de los tipos de datos siguientes: SQL_C_CHAR, SQL_C_WCHAR y SQL_C_BINARY, SQL_C_NUMERIC o SQL_C_TYPE_TIMESTAMP. En caso contrario, apunta a una matriz válida con elementos *RowsNumber*, donde cada elemento contiene su longitud o datos de indicador NULL.

*OutputSchemaColumnsNumber*  
\[Salida\] Puntero a un búfer en el que se va a devolver el número de columnas del conjunto de resultados esperado de `@script` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

## <a name="getresultcolumn"></a>GetResultColumn

Recupera la información relativa a una columna de salida determinada para una sesión concreta.

A esta función se llama para cada columna del conjunto de resultados de `@script` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).
La estructura de columnas de este conjunto de resultados se denominará "esquema de salida".

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN GetResultColumn(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLUSMALLINT    ColumnNumber,
    SQLSMALLINT*    DataType,
    SQLINTEGER*     ColumnSize,
    SQLSMALLINT*    DecimalDigits,
    SQLSMALLINT*    Nullable
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*TaskId*  
\[Entrada\] Entero que identifica de forma única este proceso de ejecución.

En el caso de `@parallel = 1`, en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), este valor oscila entre 0 y el grado de paralelismo de la consulta.

*ColumnNumber*  
\[Entrada\] Entero que identifica el índice de esta columna en el esquema de salida. Las columnas se numeran de forma secuencial en orden creciente, comenzando en el 0.

*DataType*  
\[Salida\] Puntero al búfer que contiene el tipo C de ODBC que identifica el tipo de datos de esta columna.

*ColumnSize*  
\[Salida\] Puntero a un búfer que contiene el tamaño máximo en bytes de los datos subyacentes de esta columna.

*DecimalDigits*  
\[Salida\] Puntero a un búfer que contiene los dígitos decimales de los datos subyacentes en esta columna, como se define en [Dígitos decimales](../../odbc/reference/appendixes/decimal-digits.md). Si no se puede determinar el número de dígitos decimales o no es aplicable, el valor se descarta.

*Admisión de valores NULL*  
\[Salida\] Puntero a un búfer que contiene un valor, que indica si esta columna puede contener valores NULL. Valores posibles:

- SQL_NO_NULLS: la columna no puede contener valores NULL.
- SQL_NULLABLE: la columna puede contener valores NULL.

Si se pasan otros valores, se detiene la ejecución.

## <a name="getresults"></a>GetResults

Recupera el conjunto de resultados de la ejecución de `@script` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

A esta función se puede llamar varias veces. Una vez para cada fragmento de secuencia y para cada partición de `@input_data_1_partition_by_columns` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).


### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN GetResults(
    SQLGUID         SessionId,
    SQLUSMALLINT    TaskId,
    SQLULEN*        RowsNumber,
    SQLPOINTER**    Data,
    SQLINTEGER***   StrLen_or_Ind
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*TaskId*  
\[Entrada\] Entero que identifica de forma única este proceso de ejecución.

En el caso de `@parallel = 1`, en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), este valor oscila entre 0 y el grado de paralelismo de la consulta.

*RowsNumber*  
\[Salida\] Puntero a un búfer que contiene el número de filas del *Data*.

*Data*  
\[Salida\] Puntero a una matriz bidimensional asignada por la extensión que contiene el conjunto de resultados de `@script` n [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

El número total de columnas debe ser *OutputSchemaColumnsNumber* que se ha recuperado en la llamada a [Execute](#execute). Cada columna debe contener elementos *RowsNumber* que se deben interpretar según el tipo de columna de [GetResultColumn](#getresultcolumn).

*StrLen_or_Ind*  
\[Salida\] Puntero a una matriz bidimensional asignada por la extensión que contiene el indicador de longitud/NULL de *Data*. Valores posibles de cada celda:

- n, donde n > 0. Indica la longitud de los datos, en bytes.
- SQL_NULL_DATA, que indica un valor NULL.

El número total de columnas debe ser *OutputSchemaColumnsNumber* que se ha recibido en la llamada a [Execute](#execute). Cada columna contiene elementos *RowsNumber* que se deben interpretar según el tipo de columna de [GetResultColumn](#getresultcolumn).

StrLen_or_Ind\[col\] se omitirá si una columna no admite un valor NULL y no representa uno de los tipos de datos siguientes: SQL_C_CHAR, SQL_C_WCHAR y SQL_C_BINARY [agregar fechas]. En caso contrario, apunta a una matriz válida con elementos *RowsNumber*, donde cada elemento contiene su longitud o datos de indicador NULL.

## <a name="getoutputparam"></a>GetOutputParam

Recupera la información relativa a un parámetro de salida determinado para una sesión concreta.

A esta función se llama para cada parámetro de `@params` en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) marcado con OUTPUT.

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN GetOutputParam(
    SQLGUID        SessionId,
    SQLUSMALLINT   ParamNumber,
    SQLPOINTER*    ParamValue,
    SQLINTEGER*    StrLen_or_Ind
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*ParamValue*  
\[Salida\] Puntero a un búfer que contiene el valor del parámetro.

*StrLen_or_Ind* \[Salida\] Puntero a un búfer que un valor entero que indica la longitud en bytes de *ParamValue*, o SQL_NULL_DATA para indicar que los datos son NULL.

## <a name="getinterfaceversion"></a>GetInterfaceVersion

Recupera la versión de la interfaz.
Esta función devuelve un entero que representa la versión de la interfaz de la extensión. Valores admitidos:
1. La versión 1 es la versión de API inicial. Se admite en SQL Server 2019 RTM.
1. La versión 2 tiene compatibilidad adicional con InstallExternalLibrary y UninstallExternalLibrary API, y se admite a partir de SQL Server 2019 CU3.                            

### <a name="syntax"></a>Sintaxis

```cpp
SQLUSMALLINT GetInterfaceVersion();
```

## <a name="cleanupsession"></a>CleanupSession

Información de limpieza por cada sesión.

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN CleanupSession(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*TaskId*  
\[Entrada\] Entero que identifica de forma única este proceso de ejecución.

En el caso de `@parallel = 1`, en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), este valor oscila entre 0 y el grado de paralelismo de la consulta.

## <a name="cleanup"></a>Limpieza

Limpia la información global y compartida (por ejemplo, JVM).

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN Cleanup();
```

## <a name="gettelemetryresults"></a>GetTelemetryResults

Recupera datos de telemetría (pares clave-valor) de la extensión. La función es opcional y no requiere implementación. La telemetría se expone mediante la vista de administración dinámica (DMV) `dm_db_external_script_execution_stats`.

El marco de trabajo envía un contador denominado script_executions. La extensión no debe usar este nombre.

Cada entrada de telemetría es un par clave-valor. Las claves son cadenas y los valores son contadores enteros de 64 bits. Por tanto, la salida se compone de dos matrices lógicas: los nombres y sus contadores correspondientes. Se representa cada matriz.

La longitud de cada matriz es *RowsNumber*, que es una salida. La primera salida lógica contiene punteros a cadenas, por lo que se representa mediante dos matrices: *CounterNames* (los datos de cadena reales) y *CounterNamesLength* (la longitud de cada cadena). La segunda salida lógica se almacena en el puntero *CounterValues*.

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN GetTelemetryResults(
    SQLGUID        SessionId,
    SQLUSMALLINT   TaskId,
    SQLUINTEGER    *RowsNumber,
    SQLCHAR        ***CounterNames,
    SQLINTEGER      **CounterNamesLength,
    SQLBIGINT       **CounterValues
);
```

### <a name="arguments"></a>Argumentos

*SessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*TaskId*  
\[Entrada\] Entero que identifica de forma única este proceso de ejecución.

En el caso de `@parallel = 1`, en [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), este valor oscila entre 0 y el grado de paralelismo de la consulta.

*RowsNumber*  
\[Salida\] Número de pares clave-valor.

*CounterNames*  
\[Salida\] Datos de cadena que contienen las claves.

*CounterNamesLength*  
\[Salida\] Longitud de cada cadena de clave.

*CounterValues*  
\[Salida\] Datos enteros de 64 bits que contienen los valores.

## <a name="installexternallibrary"></a>InstallExternalLibrary

Instala una biblioteca. La función es opcional y no requiere implementación. La implementación predeterminada consiste en copiar el contenido de la biblioteca (vea [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)) en un archivo en la ubicación adecuada. El nombre de archivo es el nombre de la biblioteca.

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN InstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryFile,
    SQLINTEGER LibraryFileLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Argumentos

*SetupSessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*LibraryName*  
\[Entrada\] Nombre de la biblioteca.

*LibraryNameLength*  
\[Entrada\] Longitud del nombre de biblioteca.

*LibraryFile*  
\[Entrada\] Ruta de acceso (como una cadena) al archivo de biblioteca que contiene el contenido binario especificado por [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md).

*LibraryFileLength*  
\[Entrada\] Longitud de la cadena LibraryFile.

*LibraryInstallDirectory:*  
\[Entrada\] Directorio raíz para instalar la biblioteca.

*LibraryInstallDirectoryLength*  
\[Entrada\] Longitud de la cadena LibraryInstallDirectory.

*LibraryError*  
\[Salida\] Parámetro de salida opcional. En caso de que se haya producido un error durante la instalación de la biblioteca, LibraryError apuntaría a una cadena que describe el error.

*LibraryErrorLength*  
\[Salida\] Longitud de la cadena LibraryError.

## <a name="uninstallexternallibrary"></a>UninstallExternalLibrary

Desinstala una biblioteca. La función es opcional y no requiere implementación. La implementación predeterminada es deshacer el trabajo realizado por la implementación predeterminada de InstallExternalLibrary. La implementación predeterminada elimina el contenido del archivo *LibraryName* en *LibraryInstallDirectory*.

### <a name="syntax"></a>Sintaxis

```cpp
SQLRETURN UninstallExternalLibrary(
    SQLGUID    SetupSessionId,
    SQLCHAR    *LibraryName,
    SQLINTEGER LibraryNameLength,
    SQLCHAR    *LibraryInstallDirectory,
    SQLINTEGER LibraryInstallDirectoryLength,
    SQLCHAR    **LibraryError,
    SQLINTEGER *LibraryErrorLength
);
```

### <a name="arguments"></a>Argumentos

*SetupSessionId*  
\[Entrada\] GUID que identifica de forma única esta sesión de script.

*LibraryName*  
\[Entrada\] Nombre de la biblioteca

*LibraryNameLength*  
\[Entrada\] Longitud del nombre de biblioteca

*LibraryFile*  
\[Entrada\] Ruta de acceso (como una cadena) al archivo de biblioteca que contiene el contenido binario especificado por CREATE EXTERNAL LIBRARY

*LibraryFileLength*  
\[Entrada\] Longitud de la cadena LibraryFile

*LibraryInstallDirectory*  
\[Entrada\] Directorio raíz para instalar la biblioteca

*LibraryInstallDirectoryLength*  
\[Entrada\] Longitud de la cadena LibraryInstallDirectory.

*LibraryError*  
\[Salida\] Cadena de error de la biblioteca.

*LibraryErrorLength*  
\[Salida\] Longitud de la cadena LibraryError.

## <a name="next-steps"></a>Pasos siguientes

- [SDK de extensibilidad de Microsoft para Java para SQL Server](../how-to/extensibility-sdk-java-sql-server.md)
- [Tiempo de ejecución personalizado de Python](../../machine-learning/install/custom-runtime-python.md)
- [Tiempo de ejecución personalizado de R](../../machine-learning/install/custom-runtime-r.md).