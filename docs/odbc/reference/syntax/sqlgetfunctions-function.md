---
description: Función SQLGetFunctions
title: Función SQLGetFunctions | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetFunctions
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetFunctions
helpviewer_keywords:
- SQLGetFunctions function [ODBC]
ms.assetid: 0451d2f9-0f4f-46ba-b252-670956a52183
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a5ae807638c25b0e01129d1dfbdc6238fd3001f8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201012"
---
# <a name="sqlgetfunctions-function"></a>Función SQLGetFunctions
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLGetFunctions** devuelve información acerca de si un controlador admite una función de ODBC concreta. Esta función se implementa en el administrador de controladores. también se puede implementar en los controladores. Si un controlador implementa **SQLGetFunctions**, el administrador de controladores llama a la función en el controlador. De lo contrario, ejecuta la propia función.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetFunctions(  
     SQLHDBC           ConnectionHandle,  
     SQLUSMALLINT      FunctionId,  
     SQLUSMALLINT *    SupportedPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *ConnectionHandle*  
 [Entrada] Identificador de conexión.  
  
 *FunctionId*  
 Entradas Un valor **#define** que identifica la función ODBC de interés; **SQL_API_ODBC3_ALL_FUNCTIONS orSQL_API_ALL_FUNCTIONS**. Una aplicación ODBC 3 *. x* usa **SQL_API_ODBC3_ALL_FUNCTIONS** para determinar la compatibilidad de ODBC 3 *. x* y las funciones anteriores. Una aplicación ODBC 2 *. x* usa **SQL_API_ALL_FUNCTIONS** para determinar la compatibilidad con las funciones ODBC 2 *. x* y anteriores.  
  
 Para obtener una lista de los valores de **#define** que identifican funciones ODBC, vea las tablas de "Comentarios".  
  
 *SupportedPtr*  
 Genere  Si el valor de *FunctionId* identifica una sola función ODBC, *SupportedPtr* señala a un único valor SQLUSMALLINT que se SQL_TRUE si el controlador admite la función especificada y SQL_FALSE si no se admite.  
  
 Si el valor de *FunctionId* es SQL_API_ODBC3_ALL_FUNCTIONS, *SupportedPtr* apunta a una matriz de SQLSMALLINT con un número de elementos igual a SQL_API_ODBC3_ALL_FUNCTIONS_SIZE. El administrador de controladores trata esta matriz como un mapa de bits de 4.000 bits que se puede usar para determinar si se admite una función ODBC 3 *. x* o anterior. Se llama a la macro SQL_FUNC_EXISTS para determinar la compatibilidad de la función. (Vea "Comentarios"). Una aplicación ODBC 3 *. x* puede llamar a **SQLGetFunctions** con SQL_API_ODBC3_ALL_FUNCTIONS en un controlador ODBC 3 *. x* u ODBC 2 *. x* .  
  
 Si *FunctionId* es SQL_API_ALL_FUNCTIONS, *SupportedPtr* apunta a una matriz SQLUSMALLINT de 100 elementos. La matriz se indiza mediante **#define** valores utilizados por *FunctionId* para identificar cada función ODBC. algunos elementos de la matriz no se usan y se reservan para uso futuro. Un elemento se SQL_TRUE si identifica una función ODBC 2 *. x* o anterior admitida por el controlador. Es SQL_FALSE si identifica una función ODBC no admitida por el controlador o no identifica una función ODBC.  
  
 Las matrices devueltas en **SupportedPtr* usan una indización de base cero.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetFunctions** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DBC y un *identificador* de *ConnectionHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLGetFunctions** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------|-----|-----------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|Se llamó a (DM) **SQLGetFunctions** antes de **SQLConnect**, **SQLBrowseConnect** o **SQLDriverConnect**.<br /><br /> Se llamó a (DM) **SQLBrowseConnect** para *ConnectionHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de que **SQLBrowseConnect** devolviera SQL_SUCCESS_WITH_INFO o SQL_SUCCESS.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *ConnectionHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY095|Tipo de función fuera del intervalo|(DM) se especificó un valor de *FunctionId* no válido.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
  
## <a name="comments"></a>Comentarios  
 **SQLGetFunctions** siempre devuelve que se admiten **SQLGetFunctions**, **SQLDataSources** y **SQLDrivers** . Esto se debe a que estas funciones se implementan en el administrador de controladores. El administrador de controladores asignará una función ANSI a la función Unicode correspondiente si la función Unicode existe y asignará una función Unicode a la función ANSI correspondiente si existe la función ANSI. Para obtener información sobre cómo usan las aplicaciones **SQLGetFunctions**, consulte [niveles de cumplimiento](../../../odbc/reference/develop-app/interface-conformance-levels.md)de la interfaz.  
  
 A continuación se muestra una lista de valores válidos para el valor de *FunctionId* para las funciones que cumplen con el nivel de cumplimiento normativo ISO 92:  
  
|Valor de FunctionId|Valor de FunctionId|  
|----------|----------|  
|SQL_API_SQLALLOCHANDLE|SQL_API_SQLGETDESCFIELD|  
|SQL_API_SQLBINDCOL|SQL_API_SQLGETDESCREC|  
|SQL_API_SQLCANCEL|SQL_API_SQLGETDIAGFIELD|  
|SQL_API_SQLCLOSECURSOR|SQL_API_SQLGETDIAGREC|  
|SQL_API_SQLCOLATTRIBUTE|SQL_API_SQLGETENVATTR|  
|SQL_API_SQLCONNECT|SQL_API_SQLGETFUNCTIONS|  
|SQL_API_SQLCOPYDESC|SQL_API_SQLGETINFO|  
|SQL_API_SQLDATASOURCES|SQL_API_SQLGETSTMTATTR|  
|SQL_API_SQLDESCRIBECOL|SQL_API_SQLGETTYPEINFO|  
|SQL_API_SQLDISCONNECT|SQL_API_SQLNUMRESULTCOLS|  
|SQL_API_SQLDRIVERS|SQL_API_SQLPARAMDATA|  
|SQL_API_SQLENDTRAN|SQL_API_SQLPREPARE|  
|SQL_API_SQLEXECDIRECT|SQL_API_SQLPUTDATA|  
|SQL_API_SQLEXECUTE|SQL_API_SQLROWCOUNT|  
|SQL_API_SQLFETCH|SQL_API_SQLSETCONNECTATTR|  
|SQL_API_SQLFETCHSCROLL|SQL_API_SQLSETCURSORNAME|  
|SQL_API_SQLFREEHANDLE|SQL_API_SQLSETDESCFIELD|  
|SQL_API_SQLFREESTMT|SQL_API_SQLSETDESCREC|  
|SQL_API_SQLGETCONNECTATTR|SQL_API_SQLSETENVATTR|  
|SQL_API_SQLGETCURSORNAME|SQL_API_SQLSETSTMTATTR|  
|SQL_API_SQLGETDATA| |  
  
 A continuación se muestra una lista de los valores válidos de *FunctionId* para las funciones que se ajustan al nivel de cumplimiento de estándares de grupo abierto:  
  
|Valor de FunctionId|Valor de FunctionId|  
|-|-|  
|SQL_API_SQLCOLUMNS|SQL_API_SQLSTATISTICS|  
|SQL_API_SQLSPECIALCOLUMNS|SQL_API_SQLTABLES|  
  
 A continuación se muestra una lista de valores válidos de *FunctionId* para las funciones que se ajustan al nivel de cumplimiento de los estándares ODBC.  
  
|Valor de FunctionId|Valor de FunctionId|  
|-|-|  
|SQL_API_SQLBINDPARAMETER|SQL_API_SQLNATIVESQL|  
|SQL_API_SQLBROWSECONNECT|SQL_API_SQLNUMPARAMS|  
|SQL_API_SQLBULKOPERATIONS [1]|SQL_API_SQLPRIMARYKEYS|  
|SQL_API_SQLCOLUMNPRIVILEGES|SQL_API_SQLPROCEDURECOLUMNS|  
|SQL_API_SQLDESCRIBEPARAM|SQL_API_SQLPROCEDURES|  
|SQL_API_SQLDRIVERCONNECT|SQL_API_SQLSETPOS|  
|SQL_API_SQLFOREIGNKEYS|SQL_API_SQLTABLEPRIVILEGES|  
|SQL_API_SQLMORERESULTS| |  
  
 [1] al trabajar con un controlador ODBC 2 *. x* , **SQLBulkOperations** se devolverá como compatible solo si se cumplen las dos condiciones siguientes: el controlador ODBC 2 *. x* admite **SQLSetPos** y el tipo de información SQL_POS_OPERATIONS devuelve el bit de SQL_POS_ADD como establecido.  
  
 A continuación se muestra una lista de valores válidos para *FunctionId* para las funciones introducidas en ODBC 3,8 o posterior:  
  
|Valor de FunctionId|  
|-|  
|SQL_API_SQLCANCELHANDLE [2]|  
  
 [2]   **SQLCancelHandle** se devolverá como compatible solo si el controlador admite **SQLCancel** y **SQLCancelHandle**. Si se admite **SQLCancel** pero **SQLCancelHandle** no, la aplicación puede seguir llamando a **SQLCancelHandle** en un identificador de instrucción, porque se asignará a **SQLCancel**.  
  
## <a name="sql_func_exists-macro"></a>SQL_FUNC_EXISTS macro  
 La macro SQL_FUNC_EXISTS (*SupportedPtr*, *FunctionID*) se usa para determinar la compatibilidad con las funciones ODBC 3 *. x* o anteriores después de llamar a **SQLGetFunctions** con un argumento *FunctionID* de SQL_API_ODBC3_ALL_FUNCTIONS. La aplicación llama a SQL_FUNC_EXISTS con el argumento *SupportedPtr* establecido en *SupportedPtr* pasado en *SQLGetFunctions* y con el argumento *FunctionID* establecido en el **#define** de la función. SQL_FUNC_EXISTS devuelve SQL_TRUE si se admite la función y SQL_FALSE en caso contrario.  
  
> [!NOTE]
>  Cuando se trabaja con un controlador ODBC 2 *. x* , el administrador de controladores ODBC 3 *. x* devolverá SQL_TRUE para **SQLAllocHandle** y **SQLFreeHandle** porque **SQLAllocHandle** está asignado a **SQLAllocEnv**, **SQLAllocConnect** o **SQLAllocStmt**, y porque **SQLFreeHandle** está asignado a **SQLFreeEnv**, **SQLFreeConnect** o **SQLFreeStmt**. No obstante, no se admite **SQLAllocHandle** o **SQLFreeHandle** con un argumento *HandleType* de SQL_HANDLE_DESC, aunque se devuelvan SQL_TRUE para las funciones, ya que no hay ninguna función ODBC 2 *. x* a la que asignar en este caso.  
  
## <a name="code-example"></a>Ejemplo de código  
 Los tres ejemplos siguientes muestran cómo una aplicación usa **SQLGetFunctions** para determinar si un controlador admite **SQLTables**, **SQLColumns** y **SQLStatistics**. Si el controlador no admite estas funciones, la aplicación se desconecta del controlador. En el primer ejemplo se llama a **SQLGetFunctions** una vez para cada función.  
  
```cpp  
SQLUSMALLINT TablesExists, ColumnsExists, StatisticsExists;  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
  
retcodeTables = SQLGetFunctions(hdbc, SQL_API_SQLTABLES, &TablesExists);  
retcodeColumns = SQLGetFunctions(hdbc, SQL_API_SQLCOLUMNS, &ColumnsExists);  
retcodeStatistics = SQLGetFunctions(hdbc, SQL_API_SQLSTATISTICS, &StatisticsExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (retcodeTables == SQL_SUCCESS && TablesExists == SQL_TRUE &&   
retcodeColumns == SQL_SUCCESS && ColumnsExists == SQL_TRUE &&   
retcodeStatistics == SQL_SUCCESS && StatisticsExists == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 En el segundo ejemplo, una aplicación ODBC 3. x llama a **SQLGetFunctions** y le pasa una matriz en la que **SQLGetFunctions** devuelve información acerca de todas las funciones de ODBC 3. x y anteriores.  
  
```cpp  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[SQL_API_ODBC3_ALL_FUNCTIONS_SIZE];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ODBC3_ALL_FUNCTIONS, fExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (reccode == SQL_SUCCESS &&   
SQL_FUNC_EXISTS(fExists, SQL_API_SQLTABLES) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLCOLUMNS) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLSTATISTICS) == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 El tercer ejemplo es una aplicación ODBC 2. x que llama a **SQLGetFunctions** y le pasa una matriz de 100 elementos en los que **SQLGetFunctions** devuelve información acerca de todas las funciones ODBC 2. x y anteriores.  
  
```cpp  
#define FUNCTIONS 100  
  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[FUNCTIONS];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ALL_FUNCTIONS, fExists);  
  
/* SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver. */  
if (retcode == SQL_SUCCESS &&   
fExists[SQL_API_SQLTABLES] == SQL_TRUE &&  
   fExists[SQL_API_SQLCOLUMNS] == SQL_TRUE &&  
   fExists[SQL_API_SQLSTATISTICS] == SQL_TRUE)   
{  
  
   /* Continue with application */  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Devolver el valor de un atributo de conexión|[Función SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Devolver información acerca de un controlador o un origen de datos|[Función SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Devolver el valor de un atributo de instrucción|[Función SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
