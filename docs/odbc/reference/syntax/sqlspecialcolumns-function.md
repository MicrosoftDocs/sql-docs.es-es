---
description: Función SQLSpecialColumns
title: Función SQLSpecialColumns | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSpecialColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSpecialColumns
helpviewer_keywords:
- SQLSpecialColumns function [ODBC]
ms.assetid: bb2d9f21-bda0-4e50-a8be-f710db660034
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cf4f810499c4a3409d134806ead739096c5b1b89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191754"
---
# <a name="sqlspecialcolumns-function"></a>Función SQLSpecialColumns
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: Open Group  
  
 **Resumen**  
 **SQLSpecialColumns** recupera la siguiente información sobre las columnas de una tabla especificada:  
  
-   Conjunto óptimo de columnas que identifica de forma única una fila de la tabla.  
  
-   Columnas que se actualizan automáticamente cuando una transacción actualiza cualquier valor de la fila.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLSpecialColumns(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   IdentifierType,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     TableName,  
     SQLSMALLINT   NameLength3,  
     SQLSMALLINT   Scope,  
     SQLSMALLINT   Nullable);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *IdentifierType*  
 Entradas Tipo de columna que se va a devolver. Debe ser uno de los siguientes valores:   
  
 SQL_BEST_ROWID: devuelve la columna óptima o el conjunto de columnas que, al recuperar valores de la columna o columnas, permite que cualquier fila de la tabla especificada se identifique de forma única. Una columna puede ser una pseudo columna diseñada específicamente para este propósito (como en el TID de Oracle ROWID o Ingres) o la columna o columnas de cualquier índice único de la tabla.  
  
 SQL_ROWVER: devuelve la columna o columnas de la tabla especificada, si existen, que el origen de datos actualiza automáticamente cuando una transacción actualiza cualquier valor de la fila (como en SQLBase ROWID o Sybase TIMESTAMP).  
  
 *Nombrecatálogo*  
 Entradas Nombre del catálogo de la tabla. Si un controlador admite catálogos para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") denota las tablas que no tienen catálogos. *Nombrecatálogo* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *nombrecatálogo* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *nombrecatálogo* es un argumento normal; se trata literalmente y su caso es significativo. Para obtener más información, vea [argumentos en funciones de catálogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entradas Longitud en caracteres de **nombrecatálogo*.  
  
 *Equivale*  
 Entradas Nombre del esquema de la tabla. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") denota las tablas que no tienen esquemas. *SchemaName* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *SchemaName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *SchemaName* es un argumento normal; se trata literalmente y su caso es significativo.  
  
 *NameLength2*  
 Entradas Longitud en caracteres de **SchemaName*.  
  
 *TableName*  
 Entradas Nombre de la tabla. Este argumento no puede ser un puntero nulo. *TableName* no puede contener un patrón de búsqueda de cadenas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *TableName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *TableName* es un argumento normal; se trata literalmente y su caso es significativo.  
  
 *NameLength3*  
 Entradas Longitud en caracteres de **TableName*.  
  
 *Scope*  
 Entradas Ámbito mínimo necesario del ROWID. El ROWID devuelto puede tener un ámbito mayor. Debe ser una de las siguientes:  
  
 SQL_SCOPE_CURROW: se garantiza que el ROWID es válido solo mientras se coloca en esa fila. Una reselección posterior mediante ROWID no puede devolver una fila si otra transacción actualizó o eliminó la fila.  
  
 SQL_SCOPE_TRANSACTION: se garantiza que el ROWID es válido mientras dure la transacción actual.  
  
 SQL_SCOPE_SESSION: se garantiza que el ROWID es válido mientras dure la sesión (a través de los límites de las transacciones).  
  
 *Admisión de valores NULL*  
 Entradas Determina si se van a devolver columnas especiales que pueden tener un valor NULL. Debe ser una de las siguientes:  
  
 SQL_NO_NULLS: excluya las columnas especiales que pueden tener valores NULL. Algunos controladores no admiten SQL_NO_NULLS, y estos controladores devolverán un conjunto de resultados vacío si se ha especificado SQL_NO_NULLS. Las aplicaciones se deben preparar para este caso y solicitar SQL_NO_NULLS solo si es absolutamente necesario.  
  
 SQL_NULLABLE: se devuelven columnas especiales aunque puedan tener valores NULL.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLSpecialColumns** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLSpecialColumns** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|24000|Estado de cursor no válido|Un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** . Este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.<br /><br /> Un cursor estaba abierto en el *StatementHandle*, pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|El argumento *TableName* era un puntero nulo.<br /><br /> El atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE, el argumento *nombrecatálogo* era un puntero nulo y el SQL_CATALOG_NAME *InfoType* devuelve los nombres de catálogo que se admiten.<br /><br /> (DM) el atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE y el argumento *SchemaName* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función todavía se estaba ejecutando cuando se llamó a **SQLSpecialColumns** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor de uno de los argumentos de longitud era menor que 0 pero no es igual a SQL_NTS.<br /><br /> El valor de uno de los argumentos de longitud superó el valor de longitud máxima para el nombre correspondiente. La longitud máxima de cada nombre se puede obtener llamando a **SQLGetInfo** con los valores *InfoType* : SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN o SQL_MAX_TABLE_NAME_LEN.|  
|HY097|Tipo de columna fuera del intervalo|(DM) se especificó un valor *IdentifierType* no válido.|  
|HY098|Tipo de ámbito fuera del intervalo|(DM) se especificó un valor de *ámbito* no válido.|  
|HY099|Tipo que acepta valores NULL fuera del intervalo|(DM) se especificó un valor que *acepta valores NULL* no válido.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|Se ha especificado un catálogo y el controlador o el origen de datos no admiten catálogos.<br /><br /> Se ha especificado un esquema y el controlador o el origen de datos no admiten esquemas.<br /><br /> La combinación de los valores actuales de los atributos de instrucción SQL_ATTR_CONCURRENCY y SQL_ATTR_CURSOR_TYPE no era compatible con el controlador o el origen de datos.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados solicitado. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 Cuando se SQL_BEST_ROWID el argumento *IdentifierType* , **SQLSpecialColumns** devuelve la columna o columnas que identifican de forma única cada fila de la tabla. Estas columnas siempre se pueden usar en una *lista de selección* o **en** una cláusula WHERE. **SQLColumns**, que se usa para devolver una gran cantidad de información sobre las columnas de una tabla, no devuelve necesariamente las columnas que identifican de forma única cada fila o las columnas que se actualizan automáticamente cuando una transacción actualiza cualquier valor de la fila. Por ejemplo, **SQLColumns** podría no devolver el ROWID de la Pseudocolumna de Oracle. Este es el motivo por el que se usa **SQLSpecialColumns** para devolver estas columnas. Para obtener más información, vea [usos de los datos del catálogo](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
> [!NOTE]  
>  Para obtener más información sobre el uso general, los argumentos y los datos devueltos de las funciones de catálogo de ODBC, vea [funciones de catálogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Si no hay ninguna columna que identifique de forma única cada fila de la tabla, **SQLSpecialColumns** devuelve un conjunto de filas sin filas; una llamada subsiguiente a **SQLFetch** o **SQLFetchScroll** en la instrucción devuelve SQL_NO_DATA.  
  
 Si los argumentos *IdentifierType*, *ámbito* o que *aceptan valores NULL* especifican características que no son compatibles con el origen de datos, **SQLSpecialColumns** devuelve un conjunto de resultados vacío.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, los argumentos *nombrecatálogo*, *SchemaName* y *TableName* se tratan como identificadores, por lo que no se pueden establecer en un puntero nulo en determinadas situaciones. (Para obtener más información, vea [argumentos en las funciones de catálogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)).  
  
 **SQLSpecialColumns** devuelve los resultados como un conjunto de resultados estándar, ordenados por ámbito.  
  
 Se ha cambiado el nombre de las siguientes columnas para ODBC *3. x*. Los cambios de nombre de columna no afectan a la compatibilidad con versiones anteriores porque las aplicaciones se enlazan por número de columna.  
  
|Columna ODBC 2,0|Columna ODBC *3. x*|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
  
 Para determinar la longitud real de la columna COLUMN_NAME, una aplicación puede llamar a **SQLGetInfo** con la opción SQL_MAX_COLUMN_NAME_LEN.  
  
 En la tabla siguiente se enumeran las columnas del conjunto de resultados. El controlador puede definir columnas adicionales más allá de la columna 8 (PSEUDO_COLUMN). Una aplicación debe obtener acceso a las columnas específicas del controlador contando desde el final del conjunto de resultados en lugar de especificar una posición ordinal explícita. Para obtener más información, vea [datos devueltos por las funciones de catálogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nombre de la columna|Número de columna|Tipo de datos|Comentarios|  
|-----------------|-------------------|---------------|--------------|  
|ÁMBITO (ODBC 1,0)|1|Smallint|Ámbito real del ROWID. Contiene uno de los valores siguientes:<br /><br /> SQL_SCOPE_CURROW SQL_SCOPE_TRANSACTION SQL_SCOPE_SESSION<br /><br /> Se devuelve NULL cuando se SQL_ROWVER *IdentifierType* . Para obtener una descripción de cada valor, vea la descripción del *ámbito* en "sintaxis", anteriormente en esta sección.|  
|COLUMN_NAME (ODBC 1,0)|2|VARCHAR NOT NULL|Nombre de la columna. El controlador devuelve una cadena vacía para una columna que no tiene un nombre.|  
|DATA_TYPE (ODBC 1,0)|3|Smallint no NULL|Tipo de datos SQL. Puede ser un tipo de datos SQL de ODBC o un tipo de datos SQL específico del controlador. Para obtener una lista de tipos de datos ODBC SQL válidos, vea [tipos de datos de SQL](../../../odbc/reference/appendixes/sql-data-types.md). Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador.|  
|TYPE_NAME (ODBC 1,0)|4|VARCHAR NOT NULL|Nombre del tipo de datos dependiente del origen de datos; por ejemplo, "CHAR", "VARCHAR", "MONEY", "LONG VARBINARY" o "CHAR () FOR BIT DATA".|  
|COLUMN_SIZE (ODBC 1,0)|5|Entero|Tamaño de la columna en el origen de datos. Para obtener más información sobre el tamaño de las columnas, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|BUFFER_LENGTH (ODBC 1,0)|6|Entero|La longitud en bytes de los datos transferidos en una operación **SQLGetData** o **SQLFetch** si se especifica SQL_C_DEFAULT. En el caso de los datos numéricos, este tamaño puede ser diferente del tamaño de los datos almacenados en el origen de datos. Este valor es el mismo que el de la columna COLUMN_SIZE para los datos binarios o de caracteres. Para obtener más información, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|DECIMAL_DIGITS (ODBC 1,0)|7|Smallint|Dígitos decimales de la columna en el origen de datos. Se devuelve NULL para los tipos de datos en los que los dígitos decimales no son aplicables. Para obtener más información acerca de los dígitos decimales, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).|  
|PSEUDO_COLUMN (ODBC 2,0)|8|Smallint|Indica si la columna es una pseudo columna, como el ROWID de Oracle:<br /><br /> SQL_PC_UNKNOWN SQL_PC_NOT_PSEUDO SQL_PC_PSEUDO **Nota:**  para obtener la máxima interoperabilidad, las pseudo columnas no deben ir entre comillas con el carácter de comilla de identificador devuelto por **SQLGetInfo**.|  
  
 Una vez que la aplicación recupera los valores de SQL_BEST_ROWID, la aplicación puede utilizar estos valores para reseleccionar esa fila dentro del ámbito definido. Se garantiza que la instrucción **Select** devuelve una fila o ninguna fila.  
  
 Si una aplicación reselecciona una fila basada en la columna o columnas ROWID y no se encuentra la fila, la aplicación puede suponer que se eliminó la fila o que se modificaron las columnas ROWID. Lo contrario no es cierto: aunque el ROWID no haya cambiado, puede que las demás columnas de la fila hayan cambiado.  
  
 Las columnas devueltas para el tipo de columna SQL_BEST_ROWID son útiles para las aplicaciones que necesitan desplazarse hacia delante y hacia atrás en un conjunto de resultados para recuperar los datos más recientes de un conjunto de filas. Se garantiza que la columna o columnas del ROWID no cambian mientras están colocadas en esa fila.  
  
 Es posible que la columna o columnas de ROWID sigan siendo válidas incluso cuando el cursor no esté situado en la fila; la aplicación puede determinar esto comprobando la columna ámbito en el conjunto de resultados.  
  
 Las columnas devueltas para el tipo de columna SQL_ROWVER son útiles para las aplicaciones que necesitan tener la capacidad de comprobar si se han actualizado las columnas de una fila determinada mientras se volvió a seleccionar la fila con el ROWID. Por ejemplo, después de volver a seleccionar una fila mediante ROWID, la aplicación puede comparar los valores anteriores de las columnas SQL_ROWVER con las que acaba de capturar. Si el valor de una columna de SQL_ROWVER difiere del valor anterior, la aplicación puede avisar al usuario de que ha cambiado los datos de la pantalla.  
  
## <a name="code-example"></a>Ejemplo de código  
 Para obtener un ejemplo de código de una función similar, vea [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver las columnas de una tabla o tablas|[Función SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|Capturar una sola fila o un bloque de datos en una dirección de solo avance|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Devolver las columnas de una clave principal|[Función SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
