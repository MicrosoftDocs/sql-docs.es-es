---
description: Función SQLColumns
title: SQLColumns (función) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColumns
helpviewer_keywords:
- SQLColumns function [ODBC]
ms.assetid: 4a3618b7-d2b8-43c6-a1fd-7a4e6fa8c7d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 51a72000e071d89fc3f189b00c6d8491db1e6e9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201982"
---
# <a name="sqlcolumns-function"></a>Función SQLColumns
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: Open Group  
  
 **Resumen**  
 **SQLColumns** devuelve la lista de nombres de columna de las tablas especificadas. El controlador devuelve esta información como un conjunto de resultados en el *StatementHandle* especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLColumns(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      TableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      ColumnName,  
     SQLSMALLINT    NameLength4);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *Nombrecatálogo*  
 Entradas Nombre del catálogo. Si un controlador admite catálogos para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") indica las tablas que no tienen catálogos. *Nombrecatálogo* no puede contener un patrón de búsqueda de cadenas.  
  
> [!NOTE]  
>  Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *nombrecatálogo* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *nombrecatálogo* es un argumento normal; se trata literalmente y su caso es significativo. Para obtener más información, vea [argumentos en funciones de catálogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entradas Longitud en caracteres de **nombrecatálogo*.  
  
 *Equivale*  
 Entradas Patrón de búsqueda de cadenas para los nombres de esquema. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") indica las tablas que no tienen esquemas.  
  
> [!NOTE]  
>  Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *SchemaName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *SchemaName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength2*  
 Entradas Longitud en caracteres de **SchemaName*.  
  
 *TableName*  
 Entradas Patrón de búsqueda de cadenas para los nombres de tabla.  
  
> [!NOTE]  
>  Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *TableName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *TableName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength3*  
 Entradas Longitud en caracteres de **TableName*.  
  
 *ColumnName*  
 Entradas Patrón de búsqueda de cadenas para los nombres de columna.  
  
> [!NOTE]  
>  Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *columnName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *columnName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength4*  
 Entradas Longitud en caracteres de **columnName*.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLColumns** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLColumns** y se explica cada uno en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|24000|Estado de cursor no válido|Un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** . Este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.<br /><br /> Un cursor estaba abierto en el *StatementHandle* , pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|El atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE, el argumento *nombrecatálogo* era un puntero nulo y el SQL_CATALOG_NAME *InfoType* devuelve los nombres de catálogo que se admiten.<br /><br /> (DM) el atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE y el argumento *SchemaName*, *TableName* o *columnName* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLColumns** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor de uno de los argumentos de longitud de nombre era menor que 0 pero no es igual a SQL_NTS.|  
|||El valor de uno de los argumentos de longitud de nombre ha superado el valor de longitud máxima para el catálogo o el nombre correspondiente. La longitud máxima de cada catálogo o nombre se puede obtener llamando a **SQLGetInfo** con los valores de *InfoType* . (Vea "Comentarios").|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|Se ha especificado un nombre de catálogo y el controlador o el origen de datos no admiten catálogos.<br /><br /> Se ha especificado un nombre de esquema y el controlador o el origen de datos no admiten esquemas.<br /><br /> Se especificó un patrón de búsqueda de cadena para el nombre de esquema, el nombre de tabla o el nombre de columna, y el origen de datos no admite patrones de búsqueda para uno o varios de esos argumentos.<br /><br /> La combinación de los valores actuales de los atributos de instrucción SQL_ATTR_CONCURRENCY y SQL_ATTR_CURSOR_TYPE no era compatible con el controlador o el origen de datos.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 Normalmente, esta función se utiliza antes de la ejecución de la instrucción para recuperar información sobre las columnas de una tabla o tablas del catálogo del origen de datos. **SQLColumns** se puede usar para recuperar datos de todos los tipos de elementos devueltos por **SQLTables**. Además de las tablas base, esto puede incluir vistas, sinónimos, tablas del sistema y así sucesivamente. Por el contrario, las funciones **SQLColAttribute** y **SQLDescribeCol** describen las columnas de un conjunto de resultados y la función **SQLNumResultCols** devuelve el número de columnas de un conjunto de resultados. Para obtener más información, vea [usos de los datos del catálogo](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
> [!NOTE]  
>  Para obtener más información sobre el uso general, los argumentos y los datos devueltos de las funciones de catálogo de ODBC, vea [funciones de catálogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 **SQLColumns** devuelve los resultados como un conjunto de resultados estándar, ordenados por TABLE_CAT, TABLE_SCHEM, TABLE_NAME y ORDINAL_POSITION.  
  
> [!NOTE]  
>  Cuando una aplicación trabaja con ODBC 2. controlador *x* , no se devuelve ninguna columna de ORDINAL_POSITION en el conjunto de resultados. Como resultado, al trabajar con ODBC 2. los controladores *x* , el orden de las columnas en la lista de columnas devuelta por **SQLColumns** no es necesariamente el mismo que el orden de las columnas devueltas cuando la aplicación realiza una instrucción SELECT en todas las columnas de esa tabla.  
  
> [!NOTE]  
>  **SQLColumns** podría no devolver todas las columnas. Por ejemplo, es posible que un controlador no devuelva información acerca de las pseudo columnas, como el ROWID de Oracle. Las aplicaciones pueden utilizar cualquier columna válida, tanto si se devuelve mediante **SQLColumns**.  
>   
>  **SQLColumns** no devuelve algunas columnas que puede devolver **SQLStatistics** . Por ejemplo, **SQLColumns** no devuelve las columnas de un índice creado en una expresión o filtro, como salario + beneficios o Dept = 0012.  
  
 Las longitudes de las columnas VARCHAR no se muestran en la tabla; las longitudes reales dependen del origen de datos. Para determinar las longitudes reales de las columnas TABLE_CAT, TABLE_SCHEM, TABLE_NAME y COLUMN_NAME, una aplicación puede llamar a **SQLGetInfo** con las opciones SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN, SQL_MAX_TABLE_NAME_LEN y SQL_MAX_COLUMN_NAME_LEN.  
  
 Se ha cambiado el nombre de las siguientes columnas para ODBC 3. *x*. Los cambios de nombre de columna no afectan a la compatibilidad con versiones anteriores porque las aplicaciones se enlazan por número de columna.  
  
|Columna ODBC 2,0|ODBC 3. columna *x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 Las columnas siguientes se han agregado al conjunto de resultados devuelto por **SQLColumns** para ODBC 3. *x*:  

:::row:::
    :::column:::
        CHAR_OCTET_LENGTH  
        COLUMN_DEF  
    :::column-end:::
    :::column:::
        IS_NULLABLE  
        ORDINAL_POSITION  
    :::column-end:::
    :::column:::
        SQL_DATA_TYPE  
        SQL_DATETIME_SUB  
    :::column-end:::
:::row-end:::

 En la tabla siguiente se enumeran las columnas del conjunto de resultados. El controlador puede definir columnas adicionales más allá de la columna 18 (IS_NULLABLE). Una aplicación debe obtener acceso a las columnas específicas del controlador contando desde el final del conjunto de resultados en lugar de especificar una posición ordinal explícita. Para obtener más información, vea [datos devueltos por las funciones de catálogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nombre de la columna|Columna<br /><br /> número|Tipo de datos|Comentarios|  
|-----------------|-----------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nombre del catálogo; ES NULL si no es aplicable al origen de datos. Si un controlador admite catálogos para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para las tablas que no tienen catálogos.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nombre del esquema; ES NULL si no es aplicable al origen de datos. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para las tablas que no tienen esquemas.|  
|TABLE_NAME (ODBC 1,0)|3|VARCHAR NOT NULL|Nombre de la tabla.|  
|COLUMN_NAME (ODBC 1,0)|4|VARCHAR NOT NULL|Nombre de la columna. El controlador devuelve una cadena vacía para una columna que no tiene un nombre.|  
|DATA_TYPE (ODBC 1,0)|5|Smallint no NULL|Tipo de datos SQL. Puede ser un tipo de datos SQL de ODBC o un tipo de datos SQL específico del controlador. En el caso de los tipos de datos datetime e Interval, esta columna devuelve el tipo de datos conciso (como SQL_TYPE_DATE o SQL_INTERVAL_YEAR_TO_MONTH, en lugar del tipo de datos no conciso como SQL_DATETIME o SQL_INTERVAL). Para obtener una lista de tipos de datos ODBC SQL válidos, vea tipos de datos [SQL](../../../odbc/reference/appendixes/sql-data-types.md) en el Apéndice D: tipos de datos. Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador.<br /><br /> Los tipos de datos devueltos para ODBC 3. *x* y ODBC 2. las aplicaciones *x* pueden ser diferentes. Para obtener más información, consulte [compatibilidad con versiones anteriores y cumplimiento de estándares](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).|  
|TYPE_NAME (ODBC 1,0)|6|VARCHAR NOT NULL|Nombre del tipo de datos dependiente del origen de datos; por ejemplo, "CHAR", "VARCHAR", "MONEY", "LONG VARBINAR" o "CHAR () FOR BIT DATA".|  
|COLUMN_SIZE (ODBC 1,0)|7|Entero|Si DATA_TYPE es SQL_CHAR o SQL_VARCHAR, esta columna contiene la longitud máxima en caracteres de la columna. En el caso de los tipos de datos DateTime, es el número total de caracteres necesarios para mostrar el valor cuando se convierte en caracteres. En el caso de los tipos de datos numéricos, es el número total de dígitos o el número total de bits permitidos en la columna, de acuerdo con el NUM_PREC_RADIX columna. En el caso de los tipos de datos de intervalo, es el número de caracteres en la representación de caracteres del literal de intervalo (tal y como se define en la precisión inicial del intervalo, vea [longitud del tipo de datos de intervalo](../../../odbc/reference/appendixes/interval-data-type-length.md) en el Apéndice D: tipos de datos). Para obtener más información, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.|  
|BUFFER_LENGTH (ODBC 1,0)|8|Entero|La longitud en bytes de los datos transferidos en una operación SQLGetData, SQLFetch o SQLFetchScroll si se especifica SQL_C_DEFAULT. En el caso de los datos numéricos, este tamaño puede diferir del tamaño de los datos almacenados en el origen de datos. Este valor puede diferir de COLUMN_SIZE columna de los datos de caracteres. Para obtener más información acerca de la longitud, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.|  
|DECIMAL_DIGITS (ODBC 1,0)|9|Smallint|Número total de dígitos significativos a la derecha del separador decimal. En SQL_TYPE_TIME y SQL_TYPE_TIMESTAMP, esta columna contiene el número de dígitos del componente de fracciones de segundo. En el caso de los demás tipos de datos, estos son los dígitos decimales de la columna en el origen de datos. En el caso de los tipos de datos de intervalo que contienen un componente de hora, esta columna contiene el número de dígitos a la derecha del separador decimal (fracciones de segundo). En el caso de los tipos de datos de intervalo que no contienen un componente de hora, esta columna es 0. Para obtener más información sobre los dígitos decimales, vea tamaño de la [columna, dígitos decimales, longitud del octeto de transferencia y tamaño](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) de la presentación en el Apéndice D: tipos de datos. Se devuelve NULL para los tipos de datos en los que DECIMAL_DIGITS no es aplicable.|  
|NUM_PREC_RADIX (ODBC 1,0)|10|Smallint|Para tipos de datos numéricos, 10 o 2. Si es 10, los valores de COLUMN_SIZE y DECIMAL_DIGITS proporcionan el número de dígitos decimales permitidos para la columna. Por ejemplo, una columna DECIMAL (12,5) devolverá un NUM_PREC_RADIX de 10, un COLUMN_SIZE de 12 y un DECIMAL_DIGITS de 5; una columna FLOAT podría devolver un NUM_PREC_RADIX de 10, un COLUMN_SIZE de 15 y un DECIMAL_DIGITS de NULL.<br /><br /> Si es 2, los valores de COLUMN_SIZE y DECIMAL_DIGITS proporcionan el número de bits permitido en la columna. Por ejemplo, una columna FLOAT podría devolver una base de 2, una COLUMN_SIZE de 53 y una DECIMAL_DIGITS de NULL.<br /><br /> Se devuelve NULL para los tipos de datos en los que NUM_PREC_RADIX no es aplicable.|  
|ACEPTA VALORES NULL (ODBC 1,0)|11|Smallint no NULL|SQL_NO_NULLS si la columna no puede incluir valores NULL.<br /><br /> SQL_NULLABLE si la columna acepta valores NULL.<br /><br /> SQL_NULLABLE_UNKNOWN si no se sabe si la columna acepta valores NULL.<br /><br /> El valor devuelto para esta columna difiere del valor devuelto para la columna IS_NULLABLE. La columna que admite valores NULL indica con certeza que una columna puede aceptar valores NULL, pero no puede indicar con certeza que una columna no acepta valores NULL. La columna IS_NULLABLE indica con certeza que una columna no puede aceptar valores NULL, pero no puede indicar con certeza que una columna acepta valores NULL.|  
|COMENTARIOS (ODBC 1,0)|12|Varchar|Descripción de la columna.|  
|COLUMN_DEF (ODBC 3,0)|13|Varchar|Valor predeterminado de la columna. El valor de esta columna debe interpretarse como una cadena si está entre comillas.<br /><br /> Si se especificó NULL como valor predeterminado, esta columna es la palabra NULL, no entre comillas. Si el valor predeterminado no se puede representar sin truncamiento, esta columna contiene TRUNCAdo, sin incluir comillas simples. Si no se especificó ningún valor predeterminado, esta columna es NULL.<br /><br /> El valor de COLUMN_DEF se puede utilizar para generar una nueva definición de columna, excepto cuando contiene el valor TRUNCAdo.|  
|SQL_DATA_TYPE (ODBC 3,0)|14|Smallint no NULL|Tipo de datos SQL, tal y como aparece en el campo registro de SQL_DESC_TYPE de IRD. Puede ser un tipo de datos SQL de ODBC o un tipo de datos SQL específico del controlador. Esta columna es igual que la columna DATA_TYPE, excepto los tipos de datos datetime e Interval. Esta columna devuelve el tipo de datos no conciso (como SQL_DATETIME o SQL_INTERVAL), en lugar del tipo de datos conciso (como SQL_TYPE_DATE o SQL_INTERVAL_YEAR_TO_MONTH) para los tipos de datos datetime y Interval. Si esta columna devuelve SQL_DATETIME o SQL_INTERVAL, el tipo de datos específico se puede determinar a partir de la columna SQL_DATETIME_SUB. Para obtener una lista de tipos de datos ODBC SQL válidos, vea tipos de datos [SQL](../../../odbc/reference/appendixes/sql-data-types.md) en el Apéndice D: tipos de datos. Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador.<br /><br /> Los tipos de datos devueltos para ODBC 3. *x* y ODBC 2. las aplicaciones *x* pueden ser diferentes. Para obtener más información, consulte [compatibilidad con versiones anteriores y cumplimiento de estándares](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md).|  
|SQL_DATETIME_SUB (ODBC 3,0)|15|Smallint|Código de subtipo para los tipos de datos datetime y Interval. Para otros tipos de datos, esta columna devuelve un valor NULL. Para obtener más información sobre los subcódigos DateTime y Interval, vea "SQL_DESC_DATETIME_INTERVAL_CODE" en [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).|  
|CHAR_OCTET_LENGTH (ODBC 3,0)|16|Entero|La longitud máxima en bytes de una columna de tipo de datos de caracteres o binarios. Para todos los demás tipos de datos, esta columna devuelve NULL.|  
|ORDINAL_POSITION (ODBC 3,0)|17|Integer no NULL|Posición ordinal de la columna en la tabla. La primera columna de la tabla es el número 1.|  
|IS_NULLABLE (ODBC 3,0)|18|Varchar|"NO" si la columna no incluye valores NULL.<br /><br /> "Sí" si la columna puede incluir valores NULL.<br /><br /> Esta columna devuelve una cadena de longitud cero si no se conoce la nulabilidad.<br /><br /> Se siguen las normas ISO para determinar la nulabilidad. Un DBMS que cumpla la norma ISO SQL no puede devolver una cadena vacía.<br /><br /> El valor devuelto para esta columna difiere del valor devuelto para la columna que acepta valores NULL. (Vea la descripción de la columna que admite valores NULL).|  
  
## <a name="code-example"></a>Ejemplo de código  
 En el ejemplo siguiente, una aplicación declara los búferes para el conjunto de resultados devuelto por **SQLColumns**. Llama a **SQLColumns** para devolver un conjunto de resultados que describe cada columna de la tabla Employee. A continuación, llama a **SQLBindCol** para enlazar las columnas del conjunto de resultados a los búferes. Por último, la aplicación captura cada fila de datos con **SQLFetch** y los procesa.  
  
```cpp  
// SQLColumns_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqlext.h>  
#define STR_LEN 128 + 1  
#define REM_LEN 254 + 1  
  
// Declare buffers for result set data  
SQLCHAR szSchema[STR_LEN];  
SQLCHAR szCatalog[STR_LEN];  
SQLCHAR szColumnName[STR_LEN];  
SQLCHAR szTableName[STR_LEN];  
SQLCHAR szTypeName[STR_LEN];  
SQLCHAR szRemarks[REM_LEN];  
SQLCHAR szColumnDefault[STR_LEN];  
SQLCHAR szIsNullable[STR_LEN];  
  
SQLINTEGER ColumnSize;  
SQLINTEGER BufferLength;  
SQLINTEGER CharOctetLength;  
SQLINTEGER OrdinalPosition;  
  
SQLSMALLINT DataType;  
SQLSMALLINT DecimalDigits;  
SQLSMALLINT NumPrecRadix;  
SQLSMALLINT Nullable;  
SQLSMALLINT SQLDataType;  
SQLSMALLINT DatetimeSubtypeCode;  
  
SQLHSTMT hstmt = NULL;  
  
// Declare buffers for bytes available to return  
SQLINTEGER cbCatalog;  
SQLINTEGER cbSchema;  
SQLINTEGER cbTableName;  
SQLINTEGER cbColumnName;  
SQLINTEGER cbDataType;  
SQLINTEGER cbTypeName;  
SQLINTEGER cbColumnSize;  
SQLLEN cbBufferLength;  
SQLINTEGER cbDecimalDigits;  
SQLINTEGER cbNumPrecRadix;  
SQLINTEGER cbNullable;  
SQLINTEGER cbRemarks;  
SQLINTEGER cbColumnDefault;  
SQLINTEGER cbSQLDataType;  
SQLINTEGER cbDatetimeSubtypeCode;  
SQLINTEGER cbCharOctetLength;  
SQLINTEGER cbOrdinalPosition;  
SQLINTEGER cbIsNullable;  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
   retcode = SQLColumns(hstmt, NULL, 0, NULL, 0, (SQLCHAR*)"CUSTOMERS", SQL_NTS, NULL, 0);  
  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      // Bind columns in result set to buffers  
      SQLBindCol(hstmt, 1, SQL_C_CHAR, szCatalog, STR_LEN,&cbCatalog);  
      SQLBindCol(hstmt, 2, SQL_C_CHAR, szSchema, STR_LEN, &cbSchema);  
      SQLBindCol(hstmt, 3, SQL_C_CHAR, szTableName, STR_LEN,&cbTableName);  
      SQLBindCol(hstmt, 4, SQL_C_CHAR, szColumnName, STR_LEN, &cbColumnName);  
      SQLBindCol(hstmt, 5, SQL_C_SSHORT, &DataType, 0, &cbDataType);  
      SQLBindCol(hstmt, 6, SQL_C_CHAR, szTypeName, STR_LEN, &cbTypeName);  
      SQLBindCol(hstmt, 7, SQL_C_SLONG, &ColumnSize, 0, &cbColumnSize);  
      SQLBindCol(hstmt, 8, SQL_C_SLONG, &BufferLength, 0, &cbBufferLength);  
      SQLBindCol(hstmt, 9, SQL_C_SSHORT, &DecimalDigits, 0, &cbDecimalDigits);  
      SQLBindCol(hstmt, 10, SQL_C_SSHORT, &NumPrecRadix, 0, &cbNumPrecRadix);  
      SQLBindCol(hstmt, 11, SQL_C_SSHORT, &Nullable, 0, &cbNullable);  
      SQLBindCol(hstmt, 12, SQL_C_CHAR, szRemarks, REM_LEN, &cbRemarks);  
      SQLBindCol(hstmt, 13, SQL_C_CHAR, szColumnDefault, STR_LEN, &cbColumnDefault);  
      SQLBindCol(hstmt, 14, SQL_C_SSHORT, &SQLDataType, 0, &cbSQLDataType);  
      SQLBindCol(hstmt, 15, SQL_C_SSHORT, &DatetimeSubtypeCode, 0, &cbDatetimeSubtypeCode);  
      SQLBindCol(hstmt, 16, SQL_C_SLONG, &CharOctetLength, 0, &cbCharOctetLength);  
      SQLBindCol(hstmt, 17, SQL_C_SLONG, &OrdinalPosition, 0, &cbOrdinalPosition);  
      SQLBindCol(hstmt, 18, SQL_C_CHAR, szIsNullable, STR_LEN, &cbIsNullable);  
  
      while (SQL_SUCCESS == retcode) {  
         retcode = SQLFetch(hstmt);  
         /*  
         if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // show_error();  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // Process fetched data  
         else  
            break;  
        */  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver privilegios para una o varias columnas|[Función SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Obtener varias filas de datos|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Devolver columnas que identifican de forma única una fila o columnas actualizadas automáticamente por una transacción|[Función SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|  
|Devolver estadísticas y índices de tabla|[Función SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
|Devolver una lista de tablas de un origen de datos|[Función SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|  
|Devolver privilegios para una tabla o tablas|[Función SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
