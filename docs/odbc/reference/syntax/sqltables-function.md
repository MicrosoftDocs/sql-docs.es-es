---
description: Función SQLTables
title: SQLTables (función) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLTables
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLTables
helpviewer_keywords:
- SQLTables function [ODBC]
ms.assetid: 60d5068a-7d7c-447c-acc6-f3f2cf73440c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 537323651f0759a435c574fe6234c3050fb57536
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191724"
---
# <a name="sqltables-function"></a>Función SQLTables
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: Open Group  
  
 **Resumen**  
 **SQLTables** devuelve la lista de nombres de tablas, catálogos o esquemas, así como los tipos de tabla, almacenados en un origen de datos concreto. El controlador devuelve la información como un conjunto de resultados.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLTables(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      TableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      TableType,  
     SQLSMALLINT    NameLength4);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción para los resultados recuperados.  
  
 *Nombrecatálogo*  
 Entradas Nombre del catálogo. El argumento *nombrecatálogo* acepta patrones de búsqueda si el atributo de entorno SQL_ODBC_VERSION es SQL_OV_ODBC3; no acepta patrones de búsqueda si se establece SQL_OV_ODBC2. Si un controlador admite catálogos para algunas tablas, pero no para otros, por ejemplo, cuando un controlador recupera datos de distintos DBMS, una cadena vacía ("") indica las tablas que no tienen catálogos.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *nombrecatálogo* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *nombrecatálogo* es un argumento de valor de patrón; se trata literalmente y su caso es significativo. Para obtener más información, vea [argumentos en funciones de catálogo](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).  
  
 *NameLength1*  
 Entradas Longitud en caracteres de **nombrecatálogo*.  
  
 *Equivale*  
 Entradas Patrón de búsqueda de cadenas para los nombres de esquema. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, una cadena vacía ("") indica las tablas que no tienen esquemas.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *SchemaName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *SchemaName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength2*  
 Entradas Longitud en caracteres de **SchemaName*.  
  
 *TableName*  
 Entradas Patrón de búsqueda de cadenas para los nombres de tabla.  
  
 Si el atributo de instrucción SQL_ATTR_METADATA_ID está establecido en SQL_TRUE, *TableName* se trata como un identificador y su caso no es significativo. Si se SQL_FALSE, *TableName* es un argumento de valor de patrón; se trata literalmente y su caso es significativo.  
  
 *NameLength3*  
 Entradas Longitud en caracteres de **TableName*.  
  
 *TableType*  
 Entradas Lista de tipos de tabla que deben coincidir.  
  
 Observe que el atributo de instrucción SQL_ATTR_METADATA_ID no tiene ningún efecto sobre el argumento *TableType* . *TableType* es un argumento de la lista de valores, independientemente del valor de SQL_ATTR_METADATA_ID.  
  
 *NameLength4*  
 Entradas Longitud en caracteres de **TableType*.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLTables** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLTables** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|24000|Estado de cursor no válido|Un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** . Este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.<br /><br /> Un cursor estaba abierto en el *StatementHandle*, pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|El atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE, el argumento *nombrecatálogo* era un puntero nulo y el SQL_CATALOG_NAME *InfoType* devuelve los nombres de catálogo que se admiten.<br /><br /> (DM) el atributo de instrucción SQL_ATTR_METADATA_ID se estableció en SQL_TRUE y el argumento *SchemaName* o *TableName* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a SQLTables.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor de uno de los argumentos de longitud era menor que 0 pero no es igual a SQL_NTS.<br /><br /> El valor de uno de los argumentos de longitud de nombre ha superado el valor de longitud máxima para el nombre correspondiente.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|Se ha especificado un catálogo y el controlador o el origen de datos no admiten catálogos.<br /><br /> Se ha especificado un esquema y el controlador o el origen de datos no admiten esquemas.<br /><br /> Se ha especificado un patrón de búsqueda de cadenas para el nombre del catálogo, el esquema de tabla o el nombre de tabla, y el origen de datos no admite patrones de búsqueda para uno o varios de esos argumentos.<br /><br /> La combinación de los valores actuales de los atributos de instrucción SQL_ATTR_CONCURRENCY y SQL_ATTR_CURSOR_TYPE no era compatible con el controlador o el origen de datos.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados solicitado. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 **SQLTables** enumera todas las tablas del intervalo solicitado. Un usuario puede tener o no privilegios SELECT en cualquiera de estas tablas. Para comprobar la accesibilidad, una aplicación puede:  
  
-   Llame a **SQLGetInfo** y compruebe el tipo de información SQL_ACCESSIBLE_TABLES.  
  
-   Llame a **SQLTablePrivileges** para comprobar los privilegios de cada tabla.  
  
 De lo contrario, la aplicación debe ser capaz de controlar una situación en la que el usuario selecciona una tabla para la que no se conceden privilegios **Select** .  
  
 Los argumentos *SchemaName* y *TableName* aceptan patrones de búsqueda. El argumento *nombrecatálogo* acepta patrones de búsqueda si el atributo de entorno SQL_ODBC_VERSION es SQL_OV_ODBC3; no acepta patrones de búsqueda si se establece SQL_OV_ODBC2. Si se establece SQL_OV_ODBC3, un controlador ODBC 3 *. x* necesitará que los caracteres comodín en el argumento *nombrecatálogo* se traten literalmente. Para obtener más información sobre los patrones de búsqueda válidos, vea [argumentos de valor de patrón](../../../odbc/reference/develop-app/pattern-value-arguments.md).  
  
> [!NOTE]  
>  Para obtener más información sobre el uso general, los argumentos y los datos devueltos de las funciones de catálogo de ODBC, vea [funciones de catálogo](../../../odbc/reference/develop-app/catalog-functions.md).  
  
 Para admitir la enumeración de catálogos, esquemas y tipos de tabla, se definen las siguientes semánticas especiales para los argumentos *nombrecatálogo*, *SchemaName*, *TableName* y *TableType* de **SQLTables**:  
  
-   Si *nombrecatálogo* es SQL_ALL_CATALOGS y *SchemaName* y *TableName* son cadenas vacías, el conjunto de resultados contiene una lista de catálogos válidos para el origen de datos. (Todas las columnas excepto la columna TABLE_CAT contienen valores NULL).  
  
-   Si *SchemaName* es SQL_ALL_SCHEMAS y *nombrecatálogo* y *TableName* son cadenas vacías, el conjunto de resultados contiene una lista de esquemas válidos para el origen de datos. (Todas las columnas excepto la columna TABLE_SCHEM contienen valores NULL).  
  
-   Si *TableType* es SQL_ALL_TABLE_TYPES y *nombrecatálogo*, *SchemaName* y *TableName* son cadenas vacías, el conjunto de resultados contiene una lista de tipos de tabla válidos para el origen de datos. (Todas las columnas excepto la columna TABLE_TYPE contienen valores NULL).  
  
 Si *TableType* no es una cadena vacía, debe contener una lista de valores separados por comas para los tipos de interés; cada valor se puede incluir entre comillas simples (') o sin comillas; por ejemplo, ' tabla ', ' vista ' o tabla, vista. Una aplicación siempre debe especificar el tipo de tabla en mayúsculas; el controlador debe convertir el tipo de tabla en cualquier caso que sea necesario para el origen de datos. Si el origen de datos no admite un tipo de tabla especificado, **SQLTables** no devuelve ningún resultado para ese tipo.  
  
 **SQLTables** devuelve los resultados como un conjunto de resultados estándar, ordenados por TABLE_TYPE, TABLE_CAT, TABLE_SCHEM y TABLE_NAME. Para obtener información sobre cómo se puede usar esta información, vea [usos de los datos del catálogo](../../../odbc/reference/develop-app/uses-of-catalog-data.md).  
  
 Para determinar las longitudes reales de las columnas TABLE_CAT, TABLE_SCHEM y TABLE_NAME, una aplicación puede llamar a **SQLGetInfo** con los tipos de información SQL_MAX_CATALOG_NAME_LEN, SQL_MAX_SCHEMA_NAME_LEN y SQL_MAX_TABLE_NAME_LEN.  
  
 Se ha cambiado el nombre de las siguientes columnas para ODBC 3 *. x*. Los cambios de nombre de columna no afectan a la compatibilidad con versiones anteriores porque las aplicaciones se enlazan por número de columna.  
  
|Columna ODBC 2,0|Columna ODBC 3 *. x*|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
  
 En la tabla siguiente se enumeran las columnas del conjunto de resultados. El controlador puede definir columnas adicionales más allá de la columna 5 (comentarios). Una aplicación debe obtener acceso a las columnas específicas del controlador contando desde el final del conjunto de resultados en lugar de especificar una posición ordinal explícita. Para obtener más información, vea [datos devueltos por las funciones de catálogo](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md).  
  
|Nombre de la columna|Número de columna|Tipo de datos|Comentarios|  
|-----------------|-------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1,0)|1|Varchar|Nombre del catálogo; ES NULL si no es aplicable al origen de datos. Si un controlador admite catálogos para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para las tablas que no tienen catálogos.|  
|TABLE_SCHEM (ODBC 1,0)|2|Varchar|Nombre del esquema; ES NULL si no es aplicable al origen de datos. Si un controlador admite esquemas para algunas tablas, pero no para otros, como cuando el controlador recupera datos de distintos DBMS, devuelve una cadena vacía ("") para las tablas que no tienen esquemas.|  
|TABLE_NAME (ODBC 1,0)|3|Varchar|Nombre de la tabla.|  
|TABLE_TYPE (ODBC 1,0)|4|Varchar|Nombre del tipo de tabla; uno de los siguientes: "tabla", "ver", "tabla del sistema", "temporal GLOBAL", "temporal LOCAL", "ALIAS", "sinónimo" o un nombre de tipo específico del origen de datos.<br /><br /> El significado de "ALIAS" y "sinónimo" es específico del controlador.|  
|COMENTARIOS (ODBC 1,0)|5|Varchar|Descripción de la tabla.|  
  
## <a name="example"></a>Ejemplo  
 El código de ejemplo siguiente no libera identificadores ni conexiones. Vea la función [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md) y la [función SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md) para ver ejemplos de código para liberar identificadores e instrucciones.  
  
```cpp  
// SQLTables.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
// simple helper functions  
int MySQLSuccess(SQLRETURN rc) {  
   return (rc == SQL_SUCCESS || rc == SQL_SUCCESS_WITH_INFO);  
}  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printCatalog(const struct DataBinding* catalogResult) {  
   if (catalogResult[0].StrLen_or_Ind != SQL_NULL_DATA)   
      printf("Catalog Name = %s\n", (char *)catalogResult[0].TargetValuePtr);  
}  
  
// remember to disconnect and free memory, and free statements and handles  
int main() {  
   int bufferSize = 1024, i, numCols = 5;  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   wchar_t* dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   wchar_t* userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
  
   // declare and initialize the environment, connection, statement handles  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR*)"Driver={SQL Server}", SQL_NTS, (SQLCHAR*)connStrbuffer, 1024 + 1, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferSize);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferSize);  
  
   bufferSize = 1024;  
  
   // allocate memory for the binding  
   // free this memory when done  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // setup the binding (can be used even if the statement is closed by closeStatementHandle)  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   // all catalogs query  
   printf( "A list of names of all catalogs\n" );  
   retCode = SQLTables( hstmt, (SQLCHAR*)SQL_ALL_CATALOGS, SQL_NTS, (SQLCHAR*)"", SQL_NTS, (SQLCHAR*)"", SQL_NTS, (SQLCHAR*)"", SQL_NTS );  
   for ( retCode = SQLFetch(hstmt) ;  MySQLSuccess(retCode) ; retCode = SQLFetch(hstmt) )  
      printCatalog( catalogResult );  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver privilegios para una o varias columnas|[Función SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)|  
|Devolver las columnas de una tabla o tablas|[Función SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|  
|Capturar una sola fila o un bloque de datos en una dirección de solo avance|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Devolver estadísticas y índices de tabla|[Función SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
|Devolver privilegios para una tabla o tablas|[Función SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
