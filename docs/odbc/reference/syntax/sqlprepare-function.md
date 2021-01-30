---
description: Función SQLPrepare
title: SQLPrepare (función) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPrepare
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPrepare
helpviewer_keywords:
- SQLPrepare function [ODBC]
ms.assetid: 332e1b4b-b0ed-4e7a-aa4d-4f35f4f4476b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce0348f2161def7dabc4941f088b883b286d2969
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192601"
---
# <a name="sqlprepare-function"></a>Función SQLPrepare
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLPrepare** prepara una cadena SQL para su ejecución.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLPrepare(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     StatementText,  
     SQLINTEGER    TextLength);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *StatementText*  
 Entradas Cadena de texto SQL.  
  
 *TextLength*  
 Entradas Longitud de **StatementText* en caracteres.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLPrepare** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que se suelen devolver mediante **SQLPrepare** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S02|Valor de opción cambiado|Un atributo de instrucción especificado no era válido debido a condiciones de trabajo de implementación, por lo que un valor similar se sustituyó temporalmente. (Se puede llamar a **SQLGetStmtAttr** para determinar cuál es el valor sustituido temporalmente). El valor sustituto es válido para *StatementHandle* hasta que se cierre el cursor. Los atributos de instrucción que se pueden cambiar son: SQL_ATTR_CONCURRENCY SQL_ATTR_CURSOR_TYPE SQL_ATTR_KEYSET_SIZE SQL_ATTR_MAX_LENGTH SQL_ATTR_MAX_ROWS SQL_ATTR_QUERY_TIMEOUT SQL_ATTR_SIMULATE_CURSOR<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|21S01|La lista de valores de inserción no coincide con la lista de columnas|\**StatementText* contenía una instrucción **Insert** y el número de valores que se van a insertar no coincidía con el grado de la tabla derivada.|  
|21S02|El grado de la tabla derivada no coincide con la lista de columnas|\**StatementText* contenía una instrucción **Create View** y el número de nombres especificado no es el mismo que el de la tabla derivada definida por la especificación de la consulta.|  
|22018|Valor de carácter no válido para la especificación de conversión|**StatementText* contenía una instrucción SQL que contenía un literal o un parámetro, y el valor no era compatible con el tipo de datos de la columna de la tabla asociada.|  
|22019|Carácter de escape no válido|El argumento *StatementText* contenía un predicado **like** con un **escape** en la cláusula **Where** y la longitud del carácter de escape siguiente a **escape** no era igual a 1.|  
|22025|Secuencia de escape no válida|El argumento *StatementText* contenía **"like** _Pattern Value_ escape _character_ **escape"** en la cláusula **Where** y el carácter que sigue al carácter de escape en el valor del patrón no era "%" ni "_".|  
|24000|Estado de cursor no válido|(DM) un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** .<br /><br /> Un cursor estaba abierto en el *StatementHandle*, pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .|  
|34000|Nombre de cursor no válido|\**StatementText* contenía una actualización posicionada o una **actualización** **posicionada** y el cursor al que hace referencia la instrucción que se está preparando no estaba abierto.|  
|3D000|Nombre de catálogo no válido|El nombre de catálogo especificado en *StatementText* no era válido.|  
|3F000|Nombre de esquema no válido|El nombre de esquema especificado en *StatementText* no era válido.|  
|42000|Error de sintaxis o infracción de acceso|\**StatementText* contenía una instrucción SQL que no era preparable. o contenía un error de sintaxis.<br /><br /> **StatementText* contenía una instrucción para la que el usuario no tenía los privilegios necesarios.|  
|42S01|La tabla o vista base ya existe|\**StatementText* contenía una instrucción **CREATE TABLE** o **Create View** , y el nombre de tabla o el nombre de vista especificado ya existe.|  
|42S02|No se encontró la tabla base o la vista|\**StatementText* contenía una instrucción **DROP TABLE** o **Drop View** , y el nombre de tabla o el nombre de vista especificados no existían.<br /><br /> \**StatementText* contenía una instrucción **ALTER TABLE** y el nombre de tabla especificado no existía.<br /><br /> \**StatementText* contenía una instrucción **Create View** y no existía un nombre de tabla o nombre de vista definido por la especificación de consulta.<br /><br /> \**StatementText* contenía una instrucción **Create index** y el nombre de tabla especificado no existía.<br /><br /> \**StatementText* contenía una instrucción **Grant** o **REVOKE** y el nombre de tabla o el nombre de vista especificados no existían.<br /><br /> \**StatementText* contenía una instrucción **Select** y no existía un nombre de tabla o nombre de vista especificado.<br /><br /> \**StatementText* contenía una instrucción **Delete**, **Insert** o **Update** , y el nombre de tabla especificado no existía.<br /><br /> \**StatementText* contenía una instrucción **CREATE TABLE** y una tabla especificada en una restricción (que hace referencia a una tabla distinta de la que se está creando) no existía.|  
|42S11|El índice ya existe|\**StatementText* contenía una instrucción **Create index** y el nombre de índice especificado ya existía.|  
|42S12|No se encontró el índice|\**StatementText* contenía una instrucción **Drop index** y el nombre de índice especificado no existía.|  
|42S21|La columna ya existe|\**StatementText* contenía una instrucción **ALTER TABLE** y la columna especificada en la cláusula **Add** no es única o identifica una columna existente en la tabla base.|  
|42S22|Columna no encontrada|\**StatementText* contenía una instrucción **Create index** y uno o varios de los nombres de columna especificados en la lista de columnas no existían.<br /><br /> \**StatementText* contenía una instrucción **Grant** o **REVOKE** y no existía un nombre de columna especificado.<br /><br /> \**StatementText* contenía una instrucción **Select**, **Delete**, **Insert** o **Update** , y no existía un nombre de columna especificado.<br /><br /> \**StatementText* contenía una instrucción **CREATE TABLE** y una columna especificada en una restricción (que hace referencia a una tabla distinta de la que se está creando) no existía.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle* y, a continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|(DM) *StatementText* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLPrepare** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el argumento *textLength* era menor o igual que 0 pero no es igual a SQL_NTS.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|La configuración de simultaneidad no era válida para el tipo de cursor definido.<br /><br /> El atributo de instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_VARIABLE y el atributo de instrucción SQL_ATTR_CURSOR_TYPE se estableció en un tipo de cursor para el que el controlador no admite marcadores.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera expira antes de que el origen de datos devolviera el conjunto de resultados. El período de tiempo de espera se establece a través de **SQLSetStmtAttr**, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 La aplicación llama a **SQLPrepare** para enviar una instrucción SQL al origen de datos para su preparación. Para obtener más información sobre la ejecución preparada, vea [ejecución preparada](../../../odbc/reference/develop-app/prepared-execution-odbc.md). La aplicación puede incluir uno o varios marcadores de parámetros en la instrucción SQL. Para incluir un marcador de parámetro, la aplicación inserta un signo de interrogación (?) en la cadena de SQL en la posición adecuada. Para obtener información sobre los parámetros, vea parámetros de la [instrucción](../../../odbc/reference/develop-app/statement-parameters.md).  
  
> [!NOTE]  
>  Si una aplicación usa **SQLPrepare** para preparar y **SQLExecute** para enviar una instrucción **commit** o **Rollback** , no será interoperable entre productos DBMS. Para confirmar o revertir una transacción, llame a **SQLEndTran**.  
  
 El controlador puede modificar la instrucción para usar la forma de SQL utilizada por el origen de datos y, a continuación, enviarla al origen de datos para su preparación. En concreto, el controlador modifica las secuencias de escape que se usan para definir la sintaxis de SQL para determinadas características. (Para obtener una descripción de la gramática de instrucciones SQL, vea [secuencias de escape en ODBC](../../../odbc/reference/develop-app/escape-sequences-in-odbc.md) y [Apéndice C: gramática de SQL](../../../odbc/reference/appendixes/appendix-c-sql-grammar.md)). En el caso del controlador, un identificador de instrucción es similar a un identificador de instrucción en el código SQL incrustado. Si el origen de datos admite identificadores de instrucción, el controlador puede enviar un identificador de instrucción y valores de parámetro al origen de datos.  
  
 Después de preparar una instrucción, la aplicación usa el identificador de instrucción para hacer referencia a la instrucción en llamadas de función posteriores. La instrucción preparada asociada al identificador de instrucción se puede volver a ejecutar mediante una llamada a **SQLExecute** hasta que la aplicación libere la instrucción con una llamada a **SQLFreeStmt** con la opción SQL_DROP o hasta que el identificador de instrucción se use en una llamada a **SQLPrepare**, **SQLExecDirect** o a una de las funciones de catálogo (**SQLColumns**, **SQLTables**, etc.). Una vez que la aplicación prepara una instrucción, puede solicitar información sobre el formato del conjunto de resultados. En algunas implementaciones, llamar a **SQLDescribeCol** o **SQLDescribeParam** después de **SQLPrepare** podría no ser tan eficaz como llamar a la función después de **SQLExecute** o **SQLExecDirect**.  
  
 Algunos controladores no pueden devolver errores de sintaxis ni infracciones de acceso cuando la aplicación llama a **SQLPrepare**. Un controlador puede controlar errores de sintaxis e infracciones de acceso, solo errores de sintaxis o errores de sintaxis o infracciones de acceso. Por lo tanto, una aplicación debe ser capaz de controlar estas condiciones al llamar a funciones relacionadas posteriores, como **SQLNumResultCols**, **SQLDescribeCol**, **SQLColAttribute** y **SQLExecute**.  
  
 En función de las capacidades del controlador y el origen de datos, se puede comprobar la información de parámetros (por ejemplo, los tipos de datos) cuando se prepara la instrucción (si todos los parámetros se han enlazado) o cuando se ejecuta (si no se han enlazado todos los parámetros). Para obtener la máxima interoperabilidad, una aplicación debe desenlazar todos los parámetros que se aplican a una instrucción SQL anterior antes de preparar una nueva instrucción SQL en la misma instrucción. Esto evita los errores debidos a la aplicación de la información de parámetros anterior a la nueva instrucción.  
  
> [!IMPORTANT]  
>  La confirmación de una transacción, ya sea mediante una llamada explícita a **SQLEndTran** o al trabajar en el modo de confirmación automática, puede hacer que el origen de datos elimine los planes de acceso de todas las instrucciones de una conexión. Para obtener más información, vea los tipos de información SQL_CURSOR_COMMIT_BEHAVIOR y SQL_CURSOR_ROLLBACK_BEHAVIOR en [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) y el [efecto de las transacciones en cursores y instrucciones preparadas](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md).  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md), [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)y [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignar un identificador de instrucción|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Enlazar un búfer a un parámetro|[Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Ejecutar una operación de confirmación o reversión|[Función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Devolver el número de filas afectadas por una instrucción|[Función SQLRowCount](../../../odbc/reference/syntax/sqlrowcount-function.md)|  
|Establecer un nombre de cursor|[Función SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
