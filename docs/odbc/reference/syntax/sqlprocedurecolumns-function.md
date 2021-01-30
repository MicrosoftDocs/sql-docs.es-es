---
description: Función SQLProcedureColumns
title: Función SQLProcedureColumns | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLProcedureColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLProcedureColumns
helpviewer_keywords:
- SQLProcedureColumns function [ODBC]
ms.assetid: 4ca37b28-a6df-465b-8988-d422d37fc025
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 63025751b1ec5e93f5889d77c373cbedb19bf543
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192574"
---
# <a name="sqlprocedurecolumns-function"></a>Función SQLProcedureColumns
**Conformidad**  
 Versión introducida: ODBC 1,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLProcedureColumns** devuelve la lista de parámetros de entrada y salida, así como las columnas que componen el conjunto de resultados para los procedimientos especificados. El controlador devuelve la información como un conjunto de resultados en la instrucción especificada.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLProcedureColumns(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     ProcName,  
     SQLSMALLINT   NameLength3,  
     SQLCHAR *     ColumnName,  
     SQLSMALLINT   NameLength4);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *Nombrecatálogo*  
 Entradas Nombre del catálogo de procedimientos. Si un controlador admite catálogos para algunos procedimientos, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") denota los procedimientos que no tienen catálogos. *Nombrecatálogo* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *nombrecatálogo* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *nombrecatálogo* es un argumento normal; se trata literalmente y su caso es significativo. Para obtener más información, vea [argumentos en funciones de catálogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entradas Longitud en caracteres de **nombrecatálogo*.  
  
 *Equivale*  
 Entradas Patrón de búsqueda de cadenas para los nombres de esquema de procedimiento. Si un controlador admite esquemas para algunos procedimientos, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") denota los procedimientos que no tienen esquemas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *SchemaName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *SchemaName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength2*  
 Entradas Longitud en caracteres de **SchemaName*.  
  
 *NombreProc*  
 Entradas Patrón de búsqueda de cadenas para los nombres de procedimiento.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *ProcName* se trata como un identificador y su caso no es significativo. Si está SQL_FALSE, *ProcName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength3*  
 Entradas Longitud en caracteres de **NombreProc*.  
  
 *ColumnName*  
 Entradas Patrón de búsqueda de cadenas para los nombres de columna.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *columnName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *columnName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength4*  
 Entradas Longitud en caracteres de **columnName*.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLProcedureColumns** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLProcedureColumns** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|24000|Estado de cursor no válido|Un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** . Este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.<br /><br /> Un cursor estaba abierto en el *StatementHandle*, pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLError** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|El atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE, el argumento *nombrecatálogo* era un puntero nulo y el SQL_CATALOG_NAME *InfoType* devuelve los nombres de catálogo que se admiten.<br /><br /> (DM) el atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE y el argumento *SchemaName*, *ProcName* o *columnName* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función aynschronous todavía se estaba ejecutando cuando se llamó a la función SQLProcedureColumns.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor de uno de los argumentos de longitud de nombre era menor que 0 pero no es igual a SQL_NTS.<br /><br /> El valor de uno de los argumentos de longitud de nombre ha superado el valor de longitud máxima para el nombre de catálogo, esquema, procedimiento o columna correspondiente.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|Se ha especificado un catálogo de procedimientos y el controlador o el origen de datos no admiten catálogos.<br /><br /> Se ha especificado un esquema de procedimiento y el controlador o el origen de datos no admiten esquemas.<br /><br /> Se especificó un patrón de búsqueda de cadenas para el esquema de procedimiento, el nombre de procedimiento o el nombre de columna, y el origen de datos no admite patrones de búsqueda para uno o más de esos argumentos.<br /><br /> La combinación de los valores actuales de los atributos de instrucción SQL_ATTR_CONCURRENCY y SQL_ATTR_CURSOR_TYPE no era compatible con el controlador o el origen de datos.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera expira antes de que el origen de datos devolviera el conjunto de resultados. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 Esta función se utiliza normalmente antes de la ejecución de la instrucción para recuperar información acerca de los parámetros de procedimiento y las columnas que componen el conjunto de resultados o los conjuntos devueltos por el procedimiento, si hay alguno. Para más información, vea [Procedimientos en Visual Basic](../../../odbc/reference/develop-app/procedures-odbc.md).  
  
> [!NOTE]  
>  Es posible que **SQLProcedureColumns** no devuelva todas las columnas usadas por un procedimiento. Por ejemplo, un controlador podría devolver solo información acerca de los parámetros utilizados por un procedimiento y no las columnas de un conjunto de resultados que genera.  
  
 Los argumentos *SchemaName*, *ProcName* y *columnName* aceptan patrones de búsqueda. Para obtener más información sobre los patrones de búsqueda válidos, vea [argumentos de valor de patrón](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
> [!NOTE]  
>  Para obtener más información sobre el uso general, los argumentos y los datos devueltos de las funciones de catálogo de ODBC, vea [funciones de catálogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLProcedureColumns** devuelve los resultados como un conjunto de resultados estándar, ordenados por PROCEDURE_CAT, PROCEDURE_SCHEM, PROCEDURE_NAME y COLUMN_TYPE. Los nombres de columna se devuelven para cada procedimiento en el orden siguiente: el nombre del valor devuelto, los nombres de cada parámetro de la invocación del procedimiento (en orden de llamada) y, a continuación, los nombres de cada columna del conjunto de resultados devuelto por el procedimiento (en el orden de las columnas).  
  
 Las aplicaciones deben enlazar columnas específicas del controlador con respecto al final del conjunto de resultados. Para obtener más información, vea [datos devueltos por las funciones de catálogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
 Para determinar las longitudes reales de las columnas PROCEDURE_CAT, PROCEDURE_SCHEM, PROCEDURE_NAME y COLUMN_NAME, una aplicación puede llamar a **SQLGetInfo** con las opciones SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_PROCEDURE_NAME_LEN y SQL_MAX_COLUMN_NAME_LEN.  
  
 Se ha cambiado el nombre de las siguientes columnas para ODBC 3. *x*. Los cambios de nombre de columna no afectan a la compatibilidad con versiones anteriores porque las aplicaciones se enlazan por número de columna.  
  
|Columna ODBC 2,0|ODBC 3. columna *x*|  
|---------------------|-----------------------|  
|PROCEDURE_QUALIFIER|PROCEDURE_CAT|  
|_OWNER DE PROCEDIMIENTOS|PROCEDURE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 Las columnas siguientes se han agregado al conjunto de resultados devuelto por **SQLProcedureColumns** para ODBC 3. *x*:  
  
-   COLUMN_DEF  
  
-   DATETIME_CODE  
  
-   CHAR_OCTET_LENGTH  
  
-   ORDINAL_POSITION  
  
-   IS_NULLABLE  
  
 En la tabla siguiente se enumeran las columnas del conjunto de resultados. El controlador puede definir más columnas más allá de la columna 19 (IS_NULLABLE). Una aplicación debe obtener acceso a las columnas específicas del controlador contando desde el final del conjunto de resultados en lugar de especificar una posición ordinal explícita. Para obtener más información, vea [datos devueltos por las funciones de catálogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nombre de la columna|Número de columna|Tipo de datos|Comentarios|  
|-----------------|-------------------|---------------|--------------|  
|PROCEDURE_CAT (ODBC 2,0)|1|Varchar|Nombre del catálogo de procedimientos; ES NULL si no es aplicable al origen de datos. Si un controlador admite catálogos para algunos procedimientos, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para los procedimientos que no tienen catálogos.|  
|PROCEDURE_SCHEM (ODBC 2,0)|2|Varchar|Nombre del esquema de procedimientos; ES NULL si no es aplicable al origen de datos. Si un controlador admite esquemas para algunos procedimientos, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para los procedimientos que no tienen esquemas.|  
|PROCEDURE_NAME (ODBC 2,0)|3|VARCHAR NOT NULL|Nombre del procedimiento. Se devuelve una cadena vacía para un procedimiento que no tiene un nombre.|  
|COLUMN_NAME (ODBC 2,0)|4|VARCHAR NOT NULL|Nombre de la columna de procedimiento. El controlador devuelve una cadena vacía para una columna de procedimiento que no tiene un nombre.|  
|COLUMN_TYPE (ODBC 2,0)|5|Smallint no NULL|Define la columna de procedimiento como un parámetro o una columna de conjunto de resultados:<br /><br /> SQL_PARAM_TYPE_UNKNOWN: la columna de procedimiento es un parámetro cuyo tipo es desconocido. (ODBC 1,0)<br /><br /> SQL_PARAM_INPUT: la columna de procedimiento es un parámetro de entrada. (ODBC 1,0)<br /><br /> SQL_PARAM_INPUT_OUTPUT: la columna de procedimiento es un parámetro de entrada/salida. (ODBC 1,0)<br /><br /> SQL_PARAM_OUTPUT: la columna de procedimiento es un parámetro de salida. (ODBC 2,0)<br /><br /> SQL_RETURN_VALUE: la columna procedure es el valor devuelto del procedimiento. (ODBC 2,0)<br /><br /> SQL_RESULT_COL: la columna procedure es una columna de conjunto de resultados. (ODBC 1,0)|  
|DATA_TYPE (ODBC 2,0)|6|Smallint no NULL|Tipo de datos SQL. Puede ser un tipo de datos SQL de ODBC o un tipo de datos SQL específico del controlador. En el caso de los tipos de datos datetime e Interval, esta columna devuelve los tipos de datos concisos (por ejemplo, SQL_TYPE_TIME o SQL_INTERVAL_YEAR_TO_MONTH). Para obtener una lista de tipos de datos ODBC SQL válidos, vea tipos de datos [SQL](../../../odbc/reference/appendixes/sql-data-types.md) en el Apéndice D: tipos de datos. Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador.|  
|TYPE_NAME (ODBC 2,0)|7|VARCHAR NOT NULL|Nombre del tipo de datos dependiente del origen de datos; por ejemplo, "CHAR", "VARCHAR", "MONEY", "LONG VARBINARY" o "CHAR () FOR BIT DATA".|  
|COLUMN_SIZE (ODBC 2,0)|8|Entero|Tamaño de columna de la columna de procedimiento en el origen de datos. Se devuelve NULL para los tipos de datos en los que el tamaño de columna no es aplicable. Para obtener más información sobre la precisión, vea tamaño de la [columna, dígitos decimales, longitud del octeto de transferencia y tamaño](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) de la presentación en el Apéndice D: tipos de datos.|  
|BUFFER_LENGTH (ODBC 2,0)|9|Entero|La longitud en bytes de los datos transferidos en una operación **SQLGetData** o **SQLFetch** si se especifica SQL_C_DEFAULT. En el caso de los datos numéricos, este tamaño puede ser diferente del tamaño de los datos almacenados en el origen de datos. Para obtener más información, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md), en el Apéndice D: tipos de datos.|  
|DECIMAL_DIGITS (ODBC 2,0)|10|Smallint|Los dígitos decimales de la columna de procedimiento en el origen de datos. Se devuelve NULL para los tipos de datos en los que los dígitos decimales no son aplicables. Para obtener más información sobre los dígitos decimales, vea tamaño de la [columna, dígitos decimales, longitud del octeto de transferencia y tamaño](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)de la presentación, en el Apéndice D: tipos de datos.|  
|NUM_PREC_RADIX (ODBC 2,0)|11|Smallint|Para tipos de datos numéricos, 10 o 2.<br /><br /> Si es 10, los valores de COLUMN_SIZE y DECIMAL_DIGITS proporcionan el número de dígitos decimales permitidos para la columna. Por ejemplo, una columna DECIMAL (12,5) devolverá un NUM_PREC_RADIX de 10, un COLUMN_SIZE de 12 y un DECIMAL_DIGITS de 5; una columna FLOAT podría devolver un NUM_PREC_RADIX de 10, un COLUMN_SIZE de 15 y un DECIMAL_DIGITS de NULL.<br /><br /> Si es 2, los valores de COLUMN_SIZE y DECIMAL_DIGITS proporcionan el número de bits permitido en la columna. Por ejemplo, una columna FLOAT podría devolver un NUM_PREC_RADIX de 2, un COLUMN_SIZE de 53 y un DECIMAL_DIGITS de NULL.<br /><br /> Se devuelve NULL para los tipos de datos en los que NUM_PREC_RADIX no es aplicable.|  
|ACEPTA VALORES NULL (ODBC 2,0)|12|Smallint no NULL|Indica si la columna de procedimiento acepta un valor NULL:<br /><br /> SQL_NO_NULLS: la columna procedure no acepta valores NULL.<br /><br /> SQL_NULLABLE: la columna de procedimiento acepta valores NULL.<br /><br /> SQL_NULLABLE_UNKNOWN: no se conoce si la columna de procedimiento acepta valores NULL.|  
|COMENTARIOS (ODBC 2,0)|13|Varchar|Descripción de la columna de procedimiento.|  
|COLUMN_DEF (ODBC 3,0)|14|Varchar|Valor predeterminado de la columna.<br /><br /> Si se especificó NULL como valor predeterminado, esta columna es la palabra NULL, no entre comillas. Si el valor predeterminado no se puede representar sin truncamiento, esta columna contiene TRUNCAdo, sin comillas simples. Si no se especificó ningún valor predeterminado, esta columna es NULL.<br /><br /> El valor de COLUMN_DEF se puede utilizar para generar una nueva definición de columna, excepto cuando contiene el valor TRUNCAdo.|  
|SQL_DATA_TYPE (ODBC 3,0)|15|Smallint no NULL|El valor del tipo de datos SQL tal como aparece en el campo SQL_DESC_TYPE del descriptor. Esta columna es igual que la columna DATA_TYPE, excepto los tipos de datos datetime e Interval.<br /><br /> En el caso de los tipos de datos datetime e Interval, el campo SQL_DATA_TYPE del conjunto de resultados devolverá SQL_INTERVAL o SQL_DATETIME y el campo SQL_DATETIME_SUB devolverá el subcódigo para el tipo de datos Interval o DateTime específico. (Consulte [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md)).|  
|SQL_DATETIME_SUB (ODBC 3,0)|16|Smallint|Código de subtipo para los tipos de datos datetime y Interval. Para otros tipos de datos, esta columna devuelve un valor NULL.|  
|CHAR_OCTET_LENGTH (ODBC 3,0)|17|Entero|La longitud máxima en bytes de una columna de tipo de datos de caracteres o binarios. Para todos los demás tipos de datos, esta columna devuelve NULL.|  
|ORDINAL_POSITION (ODBC 3,0)|18|Integer no NULL|Para los parámetros de entrada y salida, la posición ordinal del parámetro en la definición del procedimiento (en aumento del orden de los parámetros, empezando por 1). Para un valor devuelto (si existe), se devuelve 0. En el caso de las columnas del conjunto de resultados, la posición ordinal de la columna en el conjunto de resultados, con la primera columna del conjunto de resultados en el número 1. Si hay varios conjuntos de resultados, las posiciones ordinales de las columnas se devuelven de una manera específica del controlador.|  
|IS_NULLABLE (ODBC 3,0)|19|Varchar|"NO" si la columna no incluye valores NULL.<br /><br /> "Sí" si la columna puede incluir valores NULL.<br /><br /> Esta columna devuelve una cadena de longitud cero si no se conoce la nulabilidad.<br /><br /> Se siguen las normas ISO para determinar la nulabilidad. Un DBMS que cumpla la norma ISO SQL no puede devolver una cadena vacía.<br /><br /> El valor devuelto para esta columna es diferente del valor devuelto para la columna NULLABLE. (Vea la descripción de la columna que admite valores NULL).|  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea [llamadas a procedimientos](../../../odbc/reference/develop-app/procedure-calls.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Capturar una sola fila o un bloque de datos en una dirección de solo avance|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Devolver una lista de procedimientos en un origen de datos|[Función SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
