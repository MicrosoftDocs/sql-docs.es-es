---
description: Función SQLGetTypeInfo
title: Función SQLGetTypeInfo | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetTypeInfo
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTypeInfo
helpviewer_keywords:
- SQLGetTypeInfo function [ODBC]
ms.assetid: bdedb044-8924-4ca4-85f3-8b37578e0257
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 55de323b6eace8fa2aa8b138f30ee93938122bc0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189722"
---
# <a name="sqlgettypeinfo-function"></a>Función SQLGetTypeInfo
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLGetTypeInfo** devuelve información acerca de los tipos de datos admitidos por el origen de datos. El controlador devuelve la información en forma de un conjunto de resultados de SQL. Los tipos de datos están diseñados para usarse en instrucciones de lenguaje de definición de datos (DDL).  
  
> [!IMPORTANT]  
>  Las aplicaciones deben usar los nombres de tipo devueltos en la columna TYPE_NAME del conjunto de resultados **SQLGetTypeInfo** en las instrucciones **ALTER TABLE** y **CREATE TABLE** . **SQLGetTypeInfo** puede devolver más de una fila con el mismo valor en la columna data_type.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetTypeInfo(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   DataType);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción para el conjunto de resultados.  
  
 *DataType*  
 Entradas El tipo de datos SQL. Debe ser uno de los valores de la sección [tipos de datos de SQL](../../../odbc/reference/appendixes/sql-data-types.md) del Apéndice D: tipos de datos o un tipo de datos SQL específico del controlador. SQL_ALL_TYPES especifica que se debe devolver información sobre todos los tipos de datos.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetTypeInfo** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLGetTypeInfo** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S02|Valor de opción cambiado|Un atributo de instrucción especificado no era válido debido a condiciones de trabajo de implementación, por lo que un valor similar se sustituyó temporalmente. (Llame a **SQLGetStmtAttr** para determinar el valor sustituido temporalmente). El valor sustituto es válido para *StatementHandle* hasta que se cierre el cursor. Los atributos de instrucción que se pueden cambiar son: SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_KEYSET_SIZE, SQL_ATTR_MAX_LENGTH, SQL_ATTR_MAX_ROWS, SQL_ATTR_QUERY_TIMEOUT y SQL_ATTR_SIMULATE_CURSOR. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|24000|Estado de cursor no válido|Un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** . Este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.<br /><br /> Un conjunto de resultados estaba abierto en el *StatementHandle*, pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY004|Tipo de datos SQL no válido|El valor especificado para el *tipo* de datos del argumento no era un identificador de tipo de datos SQL de ODBC válido ni un identificador de tipo de datos específico del controlador compatible con el controlador.|  
|HY008|Operación cancelada|Se ha habilitado el procesamiento asincrónico para *StatementHandle*, después se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLGetTypeInfo** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|La combinación de los valores actuales de los atributos de instrucción SQL_ATTR_CONCURRENCY y SQL_ATTR_CURSOR_TYPE no era compatible con el controlador o el origen de datos.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador correspondiente a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 **SQLGetTypeInfo** devuelve los resultados como un conjunto de resultados estándar, ordenados por data_type y, a continuación, en qué grado se asigna el tipo de datos al tipo de datos SQL de ODBC correspondiente. Los tipos de datos definidos por el origen de datos tienen prioridad sobre los tipos de datos definidos por el usuario. Por consiguiente, el criterio de ordenación no es necesariamente coherente, pero se puede generalizar como DATA_TYPE primero, seguido de TYPE_NAME, ambos ascendentes. Por ejemplo, supongamos que un origen de datos define los tipos de datos enteros y de contador, donde COUNTER es de incremento automático, y también se ha definido un tipo de datos WHOLENUM definido por el usuario. Se devolverán en el orden Integer, WHOLENUM y COUNTER, ya que WHOLENUM se asigna estrechamente al tipo de datos SQL de ODBC SQL_INTEGER, mientras que el tipo de datos de incremento automático, aunque sea compatible con el origen de datos, no se asigna estrechamente a un tipo de datos de ODBC SQL. Para obtener información sobre cómo se puede usar esta información, vea [instrucciones DDL](../../../odbc/reference/develop-app/ddl-statements.md).  
  
 Si el argumento *DataType* especifica un tipo de datos válido para la versión de ODBC admitida por el controlador, pero no es compatible con el controlador, devolverá un conjunto de resultados vacío.  
  
> [!NOTE]  
>  Para obtener más información sobre el uso general, los argumentos y los datos devueltos de las funciones de catálogo de ODBC, vea [funciones de catálogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Se ha cambiado el nombre de las siguientes columnas para ODBC 3. *x*. Los cambios de nombre de columna no afectan a la compatibilidad con versiones anteriores porque las aplicaciones se enlazan por número de columna.  
  
|Columna ODBC 2,0|ODBC 3. columna *x*|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|MONEY|FIXED_PREC_SCALE|  
|AUTO_INCREMENT|AUTO_UNIQUE_VALUE|  
  
 Las columnas siguientes se han agregado al conjunto de resultados devuelto por **SQLGetTypeInfo** para ODBC 3. *x*:  
  
-   SQL_DATA_TYPE  
  
-   INTERVAL_PRECISION  
  
-   SQL_DATETIME_SUB  
  
-   NUM_PREC_RADIX  
  
 En la tabla siguiente se enumeran las columnas del conjunto de resultados. El controlador puede definir más columnas más allá de la columna 19 (INTERVAL_PRECISION). Una aplicación debe obtener acceso a las columnas específicas del controlador contando desde el final del conjunto de resultados en lugar de especificar una posición ordinal explícita. Para obtener más información, vea [datos devueltos por las funciones de catálogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
> [!NOTE]  
>  Es posible que **SQLGetTypeInfo** no devuelva todos los tipos de datos. Por ejemplo, un controlador podría no devolver tipos de datos definidos por el usuario. Las aplicaciones pueden usar cualquier tipo de datos válido, independientemente de si se devuelve por **SQLGetTypeInfo**. Los tipos de datos devueltos por **SQLGetTypeInfo** son los que admite el origen de datos. Están pensadas para su uso en instrucciones de lenguaje de definición de datos (DDL). Los controladores pueden devolver datos del conjunto de resultados utilizando tipos de datos distintos de los tipos devueltos por **SQLGetTypeInfo**. Al crear el conjunto de resultados para una función de catálogo, el controlador podría usar un tipo de datos que no sea compatible con el origen de datos.  
  
|Nombre de la columna|Columna<br /><br /> número|Tipo de datos|Comentarios|  
|-----------------|-----------------------|---------------|--------------|  
|TYPE_NAME (ODBC 2,0)|1|VARCHAR NOT NULL|Nombre del tipo de datos dependiente del origen de datos; por ejemplo, "CHAR ()", "VARCHAR ()", "MONEY", "LONG VARBINARY" o "CHAR () FOR BIT DATA". Las aplicaciones deben utilizar este nombre en las instrucciones **CREATE TABLE** y **ALTER TABLE** .|  
|DATA_TYPE (ODBC 2,0)|2|Smallint no NULL|Tipo de datos SQL. Puede ser un tipo de datos SQL de ODBC o un tipo de datos SQL específico del controlador. En el caso de los tipos de datos datetime o Interval, esta columna devuelve el tipo de datos conciso (como SQL_TYPE_TIME o SQL_INTERVAL_YEAR_TO_MONTH). Para obtener una lista de tipos de datos ODBC SQL válidos, vea tipos de datos [SQL](../../../odbc/reference/appendixes/sql-data-types.md) en el Apéndice D: tipos de datos. Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador.|  
|COLUMN_SIZE (ODBC 2,0)|3|Entero|El tamaño de columna máximo que el servidor admite para este tipo de datos. Para datos numéricos, es la precisión máxima. En el caso de los datos de cadena, es la longitud en caracteres. En el caso de los tipos de datos DateTime, es la longitud en caracteres de la representación de cadena (suponiendo la precisión máxima permitida del componente de fracciones de segundo). Se devuelve NULL para los tipos de datos en los que el tamaño de columna no es aplicable. En el caso de los tipos de datos de intervalo, es el número de caracteres en la representación de caracteres del literal de intervalo (tal y como se define en la precisión inicial del intervalo; vea [longitud del tipo de datos de intervalo](../../../odbc/reference/appendixes/interval-data-type-length.md) en el Apéndice D: tipos de datos).<br /><br /> Para obtener más información sobre el tamaño de las columnas, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.|  
|LITERAL_PREFIX (ODBC 2,0)|4|Varchar|Carácter o caracteres utilizados para prefijar un literal; por ejemplo, una comilla simple (') para tipos de datos de caracteres o 0x para tipos de datos binarios; Se devuelve NULL para los tipos de datos en los que no se puede aplicar un prefijo literal.|  
|LITERAL_SUFFIX (ODBC 2,0)|5|Varchar|Carácter o caracteres utilizados para terminar un literal; por ejemplo, una comilla simple (') para tipos de datos de caracteres; Se devuelve NULL para los tipos de datos donde un sufijo literal no es aplicable.|  
|CREATE_PARAMS (ODBC 2,0)|6|Varchar|Una lista de palabras clave, separadas por comas, correspondientes a cada parámetro que la aplicación puede especificar entre paréntesis cuando se usa el nombre devuelto en el campo TYPE_NAME. Las palabras clave de la lista pueden ser cualquiera de las siguientes: longitud, precisión o escala. Aparecen en el orden en que la sintaxis requiere que se usen. Por ejemplo, CREATE_PARAMS para DECIMAL sería "precisión, escala". CREATE_PARAMS para VARCHAR sería "length". Se devuelve NULL si no hay ningún parámetro para la definición de tipo de datos; por ejemplo, integer.<br /><br /> El controlador proporciona el CREATE_PARAMS texto en el idioma del país o región donde se usa.|  
|ACEPTA VALORES NULL (ODBC 2,0)|7|Smallint no NULL|Si el tipo de datos acepta un valor NULL:<br /><br /> SQL_NO_NULLS si el tipo de datos no acepta valores NULL.<br /><br /> SQL_NULLABLE si el tipo de datos acepta valores NULL.<br /><br /> SQL_NULLABLE_UNKNOWN si no se sabe si la columna acepta valores NULL.|  
|CASE_SENSITIVE (ODBC 2,0)|8|Smallint no NULL|Si un tipo de datos de caracteres distingue entre mayúsculas y minúsculas en las intercalaciones y comparaciones:<br /><br /> SQL_TRUE si el tipo de datos es un tipo de datos de caracteres y distingue mayúsculas de minúsculas.<br /><br /> SQL_FALSE si el tipo de datos no es un tipo de datos de caracteres o no distingue entre mayúsculas y minúsculas.|  
|BÚSQUEDA (ODBC 2,0)|9|Smallint no NULL|Cómo se utiliza el tipo de datos en una cláusula **Where** :<br /><br /> SQL_PRED_NONE si la columna no se puede usar en una cláusula **Where** . (Es igual que el valor de SQL_UNSEARCHABLE en ODBC 2. *x*).<br /><br /> SQL_PRED_CHAR si la columna se puede utilizar en una cláusula **Where** , pero solo con el predicado **like** . (Es igual que el valor de SQL_LIKE_ONLY en ODBC 2. *x*).<br /><br /> SQL_PRED_BASIC si la columna se puede utilizar en una cláusula **Where** con todos los operadores de comparación **excepto (** comparación, comparación cuantificada, **entre**, **DISTINCT**, **in**, **Match** y **Unique**). (Es igual que el valor de SQL_ALL_EXCEPT_LIKE en ODBC 2. *x*).<br /><br /> SQL_SEARCHABLE si la columna se puede utilizar en una cláusula **Where** con cualquier operador de comparación.|  
|UNSIGNED_ATTRIBUTE (ODBC 2,0)|10|Smallint|Si el tipo de datos es sin signo:<br /><br /> SQL_TRUE si el tipo de datos es sin signo.<br /><br /> SQL_FALSE si el tipo de datos es signed.<br /><br /> Se devuelve NULL si el atributo no es aplicable al tipo de datos o el tipo de datos no es numérico.|  
|FIXED_PREC_SCALE (ODBC 2,0)|11|Smallint no NULL|Si el tipo de datos tiene una precisión y escala fijas predefinidas (que son específicas del origen de datos), como un tipo de datos Money:<br /><br /> SQL_TRUE si tiene una precisión y escala fijas predefinidas.<br /><br /> SQL_FALSE si no tiene una precisión y escala fijas predefinidas.|  
|AUTO_UNIQUE_VALUE (ODBC 2,0)|12|Smallint|Si el tipo de datos aumenta automáticamente:<br /><br /> SQL_TRUE si el tipo de datos aumenta automáticamente.<br /><br /> SQL_FALSE si el tipo de datos no es de incremento automático.<br /><br /> Se devuelve NULL si el atributo no es aplicable al tipo de datos o el tipo de datos no es numérico.<br /><br /> Una aplicación puede insertar valores en una columna que tenga este atributo, pero normalmente no puede actualizar los valores de la columna.<br /><br /> Cuando se realiza una inserción en una columna de incremento automático, se inserta un valor único en la columna en el momento de la inserción. El incremento no está definido, pero es específico del origen de datos. Una aplicación no debe suponer que una columna de incremento automático se inicia en un punto determinado o se incrementa en un determinado valor.|  
|LOCAL_TYPE_NAME (ODBC 2,0)|13|Varchar|Versión traducida del nombre del tipo de datos, dependiente del origen de datos. Se devuelve NULL si el origen de datos no admite un nombre traducido. Este nombre solo se ha diseñado para mostrarse, como en los cuadros de diálogo.|  
|MINIMUM_SCALE (ODBC 2,0)|14|Smallint|Escala mínima del tipo de datos en el origen de datos. Si un tipo de datos tiene una escala fija, las columnas MINIMUM_SCALE y MAXIMUM_SCALE contienen este valor. Por ejemplo, una columna SQL_TYPE_TIMESTAMP podría tener una escala fija para las fracciones de segundo. Se devuelve NULL cuando no se puede aplicar la escala. Para obtener más información, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.|  
|MAXIMUM_SCALE (ODBC 2,0)|15|Smallint|Escala máxima del tipo de datos en el origen de datos. Se devuelve NULL cuando no se puede aplicar la escala. Si la escala máxima no está definida por separado en el origen de datos, pero en su lugar se define igual que la precisión máxima, esta columna contiene el mismo valor que la COLUMN_SIZE columna. Para obtener más información, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.|  
|SQL_DATA_TYPE (ODBC 3,0)|16|Smallint NOT NULL|El valor del tipo de datos SQL tal como aparece en el campo SQL_DESC_TYPE del descriptor. Esta columna es igual que la columna DATA_TYPE, excepto los tipos de datos Interval y DateTime.<br /><br /> En el caso de los tipos de datos Interval y DateTime, el campo SQL_DATA_TYPE del conjunto de resultados devolverá SQL_INTERVAL o SQL_DATETIME y el campo SQL_DATETIME_SUB devolverá el subcódigo para el tipo de datos Interval o DateTime específico. (Consulte [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md)).|  
|SQL_DATETIME_SUB (ODBC 3,0)|17|Smallint|Cuando el valor de SQL_DATA_TYPE es SQL_DATETIME o SQL_INTERVAL, esta columna contiene el subcódigo de fecha y hora. En el caso de tipos de datos distintos de DateTime e Interval, este campo es NULL.<br /><br /> En el caso de los tipos de datos Interval o DateTime, el campo SQL_DATA_TYPE del conjunto de resultados devolverá SQL_INTERVAL o SQL_DATETIME y el campo SQL_DATETIME_SUB devolverá el subcódigo para el tipo de datos Interval o DateTime específico. (Consulte [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md)).|  
|NUM_PREC_RADIX (ODBC 3,0)|18|Entero|Si el tipo de datos es un tipo numérico aproximado, esta columna contiene el valor 2 para indicar que COLUMN_SIZE especifica un número de bits. En el caso de los tipos numéricos exactos, esta columna contiene el valor 10 para indicar que COLUMN_SIZE especifica un número de dígitos decimales. De lo contrario, esta columna es NULL.|  
|INTERVAL_PRECISION (ODBC 3,0)|19|Smallint|Si el tipo de datos es un tipo de datos de intervalo, esta columna contiene el valor de la precisión inicial del intervalo. (Consulte la [precisión del tipo de datos Interval](../../../odbc/reference/appendixes/interval-data-type-precision.md) en el Apéndice D: tipos de datos). De lo contrario, esta columna es NULL.|  
  
 La información de atributos se puede aplicar a tipos de datos o a columnas específicas en un conjunto de resultados. **SQLGetTypeInfo** devuelve información sobre los atributos asociados con los tipos de datos de. **SQLColAttribute** devuelve información sobre los atributos asociados a las columnas de un conjunto de resultados.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver información acerca de una columna de un conjunto de resultados|[Función SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Capturar una sola fila o un bloque de datos en una dirección de solo avance|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Devolver información acerca de un controlador o un origen de datos|[Función SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
