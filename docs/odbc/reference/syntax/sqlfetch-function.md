---
description: Función SQLFetch
title: Función SQLFetch | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFetch
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFetch
helpviewer_keywords:
- SQLFetch function [ODBC]
ms.assetid: 6c6611d2-bc6a-4390-87c9-1c5dd9cfe07c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 095a1ca4a67122344926d9ca363dd9f05205c48b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209800"
---
# <a name="sqlfetch-function"></a>Función SQLFetch
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLFetch** captura el siguiente conjunto de filas de datos del conjunto de resultados y devuelve los datos de todas las columnas enlazadas.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLFetch(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLFetch** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado mediante una llamada a la [función SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLFetch** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario. Si se produce un error en una sola columna, se puede llamar a [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) con un *DiagIdentifier* de SQL_DIAG_COLUMN_NUMBER para determinar la columna en la que se produjo el error; y se puede llamar a **SQLGetDiagField** con un *DiagIdentifier* de SQL_DIAG_ROW_NUMBER para determinar la fila que contiene esa columna.  
  
 En el caso de todos los SQLSTATEs que pueden devolver SQL_SUCCESS_WITH_INFO o SQL_ERROR (excepto 01xxx SQLSTATEs), se devuelve SQL_SUCCESS_WITH_INFO si se produce un error en una o varias filas de una operación de MultiRow y se devuelve SQL_ERROR si se produce un error en una operación de una sola fila.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|Los datos binarios o de cadena devueltos para una columna dieron como resultado el truncamiento de datos binarios de caracteres que no están en blanco o no NULOs. Si fuera un valor de cadena, se truncó a la derecha.|  
|01S01|Error en la fila|Se produjo un error al capturar una o más filas.<br /><br /> (Si se devuelve este SQLSTATE cuando una aplicación ODBC 3 *. x* está trabajando con un controlador ODBC 2 *. x* , se puede pasar por alto).|  
|01S07|Truncamiento fraccionario|Los datos devueltos para una columna se han truncado. En el caso de los tipos de datos numéricos, se truncó la parte fraccionaria del número. En el caso de los tipos de datos Time, TIMESTAMP y Interval que contienen un componente de hora, se truncó la parte fraccionaria de la hora.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|No se pudo convertir el valor de datos de una columna del conjunto de resultados al tipo de datos especificado por *TargetType* en **SQLBindCol**.<br /><br /> La columna 0 se ha enlazado con un tipo de datos de SQL_C_BOOKMARK y el atributo SQL_ATTR_USE_BOOKMARKS instrucción se ha establecido en SQL_UB_VARIABLE.<br /><br /> La columna 0 se ha enlazado con un tipo de datos de SQL_C_VARBOOKMARK y el atributo SQL_ATTR_USE_BOOKMARKS instrucción no se ha establecido en SQL_UB_VARIABLE.|  
|07009|Índice de descriptor no válido|El controlador era un controlador ODBC 2 *. x* que no admite **SQLExtendedFetch**, y un número de columna especificado en el enlace para una columna era 0.<br /><br /> La columna 0 estaba enlazada y el atributo SQL_ATTR_USE_BOOKMARKS instrucción se estableció en SQL_UB_OFF.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|22001|Datos de cadena, truncados a la derecha|Un marcador de longitud variable devuelto para una columna se truncó.|  
|22002|Se requiere una variable de indicador pero no se ha proporcionado|Se han capturado datos NULL en una columna cuya *StrLen_or_IndPtr* establecida por **SQLBindCol** (o SQL_DESC_INDICATOR_PTR establecida por **SQLSetDescField** o **SQLSetDescRec**) era un puntero nulo.|  
|22003|Valor numérico fuera del intervalo|Si se devuelve el valor numérico como numérico o de cadena para una o varias columnas enlazadas, la parte entera (en oposición a fraccionaria) del número se trunca.<br /><br /> Para obtener más información, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) en el Apéndice D: tipos de datos.|  
|22007|Formato de fecha y hora no válido|Una columna de caracteres del conjunto de resultados estaba enlazada a una estructura de fecha, hora o marca de tiempo de C, y un valor de la columna era, respectivamente, una fecha, una hora o una marca de tiempo no válidas.|  
|22012|División por cero|Se devolvió un valor de una expresión aritmética, lo que da como resultado una división por cero.|  
|22015|Desbordamiento de campo de intervalo|La asignación de un tipo numérico exacto o de intervalo SQL a un tipo de intervalo C provoca una pérdida de dígitos significativos en el campo inicial.<br /><br /> Al capturar datos en un tipo de intervalo C, no había ninguna representación del valor del tipo SQL en el tipo de intervalo C.|  
|22018|Valor de carácter no válido para la especificación de conversión|Una columna de caracteres del conjunto de resultados estaba enlazada a un búfer de caracteres C y la columna contenía un carácter para el que no había ninguna representación en el juego de caracteres del búfer.<br /><br /> El tipo C era un tipo de datos numérico exacto o aproximado, un valor de fecha y hora o un intervalo. el tipo SQL de la columna era un tipo de datos de caracteres. y el valor de la columna no era un literal válido del tipo C enlazado.|  
|24000|Estado de cursor no válido|El *StatementHandle* estaba en un estado ejecutado pero no se asoció ningún conjunto de resultados con el *StatementHandle*.|  
|40001|Error de serialización|La transacción en la que se ejecutó la captura ha finalizado para evitar el interbloqueo.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función **SQLFetch** y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función **SQLFetch** en el *StatementHandle*.<br /><br /> O bien, se llamó a la función  **SQLFetch** y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLFetch** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) el *StatementHandle* especificado no se encontraba en un estado ejecutado. Se llamó a la función sin llamar primero a **SQLExecDirect**, **SQLExecute** o a una función de catálogo.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> Se llamó a **SQLFetch** (DM) para *StatementHandle* después de llamar a **SQLExtendedFetch** y antes de que se llamara a **SQLFreeStmt** con la opción SQL_CLOSE.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|El atributo de instrucción SQL_ATTR_USE_BOOKMARK se estableció en SQL_UB_VARIABLE y la columna 0 estaba enlazada a un búfer cuya longitud no es igual a la longitud máxima para el marcador de este conjunto de resultados. (Esta longitud está disponible en el campo SQL_DESC_OCTET_LENGTH de IRD y se puede obtener llamando a **SQLDescribeCol**, **SQLColAttribute** o **SQLGetDescField**).|  
|HY107|Valor de fila fuera del intervalo|Se SQL_CURSOR_KEYSET_DRIVEN el valor especificado con el atributo de instrucción SQL_ATTR_CURSOR_TYPE, pero el valor especificado con el atributo de instrucción SQL_ATTR_KEYSET_SIZE era mayor que 0 y menor que el valor especificado con el atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El controlador o el origen de datos no admite la conversión especificada por la combinación de *TargetType* en **SQLBindCol** y el tipo de datos SQL de la columna correspondiente.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados solicitado. El período de tiempo de espera se establece a través de SQLSetStmtAttr, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 **SQLFetch** devuelve el siguiente conjunto de filas del conjunto de resultados. Solo se puede llamar mientras exista un conjunto de resultados: es decir, después de una llamada que crea un conjunto de resultados y antes de que se cierre el cursor sobre dicho conjunto de resultados. Si alguna columna está enlazada, devuelve los datos de esas columnas. Si la aplicación ha especificado un puntero a una matriz de estado de fila o a un búfer en el que se va a devolver el número de filas recuperadas, **SQLFetch** también devuelve esta información. Las llamadas a **SQLFetch** se pueden mezclar con llamadas a **SQLFetchScroll** pero no se pueden mezclar con llamadas a **SQLExtendedFetch**. Para obtener más información, vea obtener [una fila de datos](../../../odbc/reference/develop-app/fetching-a-row-of-data.md).  
  
 Si una aplicación ODBC *3. x* funciona con un controlador ODBC *2. x* , el administrador de controladores asigna llamadas **SQLFetch** a **SQLExtendedFetch** para un controlador ODBC 2 *. x* que admite **SQLExtendedFetch**. Si el controlador ODBC 2 *. x* no es compatible con **SQLExtendedFetch**, el administrador de controladores asigna llamadas **SQLFetch** a **SQLFetch** en el controlador ODBC 2 *. x* , que solo puede capturar una sola fila.  
  
 Para obtener más información, vea [cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) en el Apéndice G: instrucciones de controlador para la compatibilidad con versiones anteriores.  
  
## <a name="positioning-the-cursor"></a>Colocar el cursor  
 Cuando se crea el conjunto de resultados, el cursor se coloca antes del inicio del conjunto de resultados. **SQLFetch** captura el siguiente conjunto de filas. Es equivalente a llamar a **SQLFetchScroll** con *FetchOrientation* establecido en SQL_FETCH_NEXT. Para obtener más información sobre los cursores [, vea](../../../odbc/reference/develop-app/cursors.md) cursores y [cursores de bloque](../../../odbc/reference/develop-app/block-cursors.md).  
  
 El atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE especifica el número de filas del conjunto de filas. Si el conjunto de filas que se captura por **SQLFetch** se superpone al final del conjunto de resultados, **SQLFetch** devuelve un conjunto de filas parcial. Es decir, si S + R-1 es mayor que L, donde S es la fila inicial del conjunto de filas que se va a capturar, R es el tamaño del conjunto de filas y L es la última fila del conjunto de resultados, solo las primeras L-S + 1 filas del conjunto de filas son válidas. Las filas restantes están vacías y tienen el estado SQL_ROW_NOROW.  
  
 Una vez que se devuelve **SQLFetch** , la fila actual es la primera fila del conjunto de filas.  
  
 Las reglas que se muestran en la siguiente tabla describen el posicionamiento del cursor después de una llamada a **SQLFetch**, en función de las condiciones enumeradas en la segunda tabla de esta sección.  
  
|Condición|Primera fila del conjunto de filas nuevo|  
|---------------|-----------------------------|  
|Antes del inicio|1|  
| \< CurrRowsetStart =  *LastResultRow-RowsetSize*[1]|*CurrRowsetStart*  +  *RowsetSize*[2]|  
|*CurrRowsetStart*  >  *LastResultRow-RowsetSize*[1]|Después del final|  
|Después del final|Después del final|  
  
 [1] si se cambia el tamaño del conjunto de filas entre las capturas, este es el tamaño del conjunto de filas que se usó con la captura anterior.  
  
 [2] si se cambia el tamaño del conjunto de filas entre las capturas, este es el tamaño del conjunto de filas que se usó con la nueva captura.  
  
|Notación|Significado|  
|--------------|-------------|  
|Antes del inicio|El cursor de bloque se coloca antes del inicio del conjunto de resultados. Si la primera fila del conjunto de filas nuevo es anterior al inicio del conjunto de resultados, **SQLFetch** devuelve SQL_NO_DATA.|  
|Después del final|El cursor de bloque se coloca después del final del conjunto de resultados. Si la primera fila del conjunto de filas nuevo es posterior al final del conjunto de resultados, **SQLFetch** devuelve SQL_NO_DATA.|  
|*CurrRowsetStart*|Número de la primera fila del conjunto de filas actual.|  
|*LastResultRow*|Número de la última fila del conjunto de resultados.|  
|*RowsetSize*|Tamaño del conjunto de filas.|  
  
 Por ejemplo, suponga que un conjunto de resultados tiene 100 filas y el tamaño del conjunto de filas es 5. En la tabla siguiente se muestra el conjunto de filas y el código de retorno devuelto por **SQLFetch** para diferentes posiciones de inicio.  
  
|Conjunto de filas actual|Código de retorno|Nuevo conjunto de filas|número de filas recuperadas|  
|--------------------|-----------------|----------------|------------------------|  
|Antes del inicio|SQL_SUCCESS|de 1 a 5|5|  
|de 1 a 5|SQL_SUCCESS|de 6 a 10|5|  
|de 52 a 56|SQL_SUCCESS|de 57 a 61|5|  
|de 91 a 95|SQL_SUCCESS|de 96 a 100|5|  
|de 93 a 97|SQL_SUCCESS|de 98 a 100. Las filas 4 y 5 de la matriz de estado de fila se establecen en SQL_ROW_NOROW.|3|  
|de 96 a 100|SQL_NO_DATA|Ninguno.|0|  
|de 99 a 100|SQL_NO_DATA|Ninguno.|0|  
|Después del final|SQL_NO_DATA|Ninguno.|0|  
  
## <a name="returning-data-in-bound-columns"></a>Devolver datos en columnas enlazadas  
 Cuando **SQLFetch** devuelve cada fila, coloca los datos de cada columna enlazada en el búfer enlazado a esa columna. Si no hay columnas enlazadas, **SQLFetch** no devuelve ningún dato, pero mueve el cursor de bloque hacia delante. Los datos se pueden recuperar mediante **SQLGetData**. Si el cursor es un cursor de varias filas (es decir, el SQL_ATTR_ROW_ARRAY_SIZE es mayor que 1), solo se puede llamar a **SQLGetData** si se devuelve SQL_GD_BLOCK cuando se llama a **SQLGetInfo** con un *InfoType* de SQL_GETDATA_EXTENSIONS. (Para obtener más información, vea [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)).  
  
 Para cada columna enlazada de una fila, **SQLFetch** hace lo siguiente:  
  
1.  Establece el búfer de longitud/indicador en SQL_NULL_DATA y continúa en la columna siguiente si los datos son NULL. Si los datos son NULL y no se ha enlazado ningún búfer de longitud/indicador, **SQLFetch** devuelve SQLSTATE 22002 (se requiere una variable de indicador pero no se proporciona) para la fila y continúa con la siguiente fila. Para obtener información sobre cómo determinar la dirección del búfer de longitud/indicador, vea el tema sobre direcciones de búfer en [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
     Si los datos de la columna no son NULL, **SQLFetch** continúa en el paso 2.  
  
2.  Si el atributo de instrucción SQL_ATTR_MAX_LENGTH está establecido en un valor distinto de cero y la columna contiene datos binarios o de caracteres, los datos se truncan en SQL_ATTR_MAX_LENGTH bytes.  
  
    > [!NOTE]  
    >  El atributo de instrucción SQL_ATTR_MAX_LENGTH está diseñado para reducir el tráfico de red. Normalmente, se implementa mediante el origen de datos, que trunca los datos antes de devolverlos a través de la red. No es necesario que los controladores y los orígenes de datos sean compatibles. Por lo tanto, para garantizar que los datos se truncan a un tamaño determinado, una aplicación debe asignar un búfer de ese tamaño y especificar el tamaño en el argumento *cbValueMax* en **SQLBindCol**.  
  
3.  Convierte los datos al tipo especificado por *TargetType* en **SQLBindCol**.  
  
4.  Si los datos se han convertido en un tipo de datos de longitud variable, como carácter o binario, **SQLFetch** comprueba si la longitud de los datos supera la longitud del búfer de datos. Si la longitud de los datos de caracteres (incluido el carácter de terminación null) supera la longitud del búfer de datos, **SQLFetch** trunca los datos a la longitud del búfer de datos menos la longitud de un carácter de terminación null. Después, termina en NULL los datos. Si la longitud de los datos binarios supera la longitud del búfer de datos, **SQLFetch** lo trunca en la longitud del búfer de datos. La longitud del búfer de datos se especifica con *BufferLength* en **SQLBindCol**.  
  
     **SQLFetch** nunca trunca los datos convertidos en tipos de datos de longitud fija; siempre supone que la longitud del búfer de datos es el tamaño del tipo de datos.  
  
5.  Coloca los datos convertidos (y posiblemente truncados) en el búfer de datos. Para obtener información acerca de cómo determinar la dirección del búfer de datos, vea el tema sobre las direcciones de búfer en [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
6.  Coloca la longitud de los datos en el búfer de longitud/indicador. Si el puntero del indicador y el puntero de longitud se establecieron en el mismo búfer (como una llamada a **SQLBindCol** sí), la longitud se escribe en el búfer para los datos válidos y SQL_NULL_DATA se escribe en el búfer para los datos null. Si no se ha enlazado ningún búfer de longitud/indicador, **SQLFetch** no devuelve la longitud.  
  
    -   En el caso de los datos de caracteres o binarios, es la longitud de los datos después de la conversión y antes del truncamiento debido a que el búfer de datos es demasiado pequeño. Si el controlador no puede determinar la longitud de los datos después de la conversión, como en ocasiones es el caso con datos largos, establece la longitud en SQL_NO_TOTAL. Si los datos se truncaron debido al atributo de instrucción SQL_ATTR_MAX_LENGTH, el valor de este atributo se coloca en el búfer de longitud/indicador en lugar de en la longitud real. Esto se debe a que este atributo está diseñado para truncar los datos en el servidor antes de la conversión, de modo que el controlador no tenga forma de averiguar cuál es la longitud real.  
  
    -   Para el resto de tipos de datos, es la longitud de los datos después de la conversión. es decir, es el tamaño del tipo al que se han convertido los datos.  
  
     Para obtener información sobre cómo determinar la dirección del búfer de longitud/indicador, vea el tema sobre direcciones de búfer en [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
7.  Si los datos se truncan durante la conversión sin una pérdida de dígitos significativos (por ejemplo, el número real 1,234 se trunca en el entero 1 cuando se convierte), **SQLFetch** devuelve SQLSTATE 01S07 (truncamiento fraccionario) y SQL_SUCCESS_WITH_INFO. Si se truncan los datos porque la longitud del búfer de datos es demasiado pequeña (por ejemplo, la cadena "abcdef" se coloca en un búfer de 4 bytes), **SQLFetch** devuelve SQLSTATE 01004 (datos truncados) y SQL_SUCCESS_WITH_INFO. Si se truncan los datos debido al atributo de instrucción SQL_ATTR_MAX_LENGTH, **SQLFetch** devuelve SQL_SUCCESS y no devuelve SQLSTATE 01S07 (truncamiento fraccionario) o SQLSTATE 01004 (datos truncados). Si se truncan los datos durante la conversión con una pérdida de dígitos significativos (por ejemplo, si un valor SQL_INTEGER mayor que 100.000 se convirtió en una SQL_C_TINYINT), **SQLFetch** devuelve SQLSTATE 22003 (valor numérico fuera del intervalo) y SQL_ERROR (si el tamaño del conjunto de filas es 1) o SQL_SUCCESS_WITH_INFO (si el tamaño del conjunto de filas es mayor que 1).  
  
 El contenido del búfer de datos enlazado y el búfer de indicador o longitud no están definidos si **SQLFetch** o **SQLFetchScroll** no devuelve SQL_SUCCESS ni SQL_SUCCESS_WITH_INFO.  
  
## <a name="row-status-array"></a>Matriz de Estados de fila  
 La matriz de estado de fila se utiliza para devolver el estado de cada fila del conjunto de filas. La dirección de esta matriz se especifica con el atributo de instrucción SQL_ATTR_ROW_STATUS_PTR. La aplicación asigna la matriz y debe tener tantos elementos como especifique el atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE. Sus valores se establecen mediante **SQLFetch**, **SQLFetchScroll** y **SQLBulkOperations** o **SQLSetPos** (excepto cuando se les ha llamado después de haber colocado el cursor por **SQLExtendedFetch**). Si el valor del atributo de instrucción SQL_ATTR_ROW_STATUS_PTR es un puntero nulo, estas funciones no devuelven el estado de fila.  
  
 El contenido del búfer de matriz de estado de fila no está definido si **SQLFetch** o **SQLFetchScroll** no devuelve SQL_SUCCESS ni SQL_SUCCESS_WITH_INFO.  
  
 Los valores siguientes se devuelven en la matriz de estado de fila.  
  
|Valor de la matriz de estado de fila|Descripción|  
|----------------------------|-----------------|  
|SQL_ROW_SUCCESS|La fila se ha recuperado correctamente y no ha cambiado desde que se capturó por última vez desde este conjunto de resultados.|  
|SQL_ROW_SUCCESS_WITH_INFO|La fila se ha recuperado correctamente y no ha cambiado desde que se capturó por última vez desde este conjunto de resultados. Sin embargo, se devolvió una advertencia sobre la fila.|  
|SQL_ROW_ERROR|Se produjo un error al capturar la fila.|  
|SQL_ROW_UPDATED [1], [2] y [3]|La fila se ha recuperado correctamente y ha cambiado desde que se capturó por última vez desde este conjunto de resultados. Si la fila se recupera de nuevo desde este conjunto de resultados o se actualiza mediante **SQLSetPos**, el estado cambia al nuevo estado de la fila.|  
|SQL_ROW_DELETED [3]|La fila se ha eliminado desde la última vez que se recuperó de este conjunto de resultados.|  
|SQL_ROW_ADDED [4]|**SQLBulkOperations** insertó la fila. Si la fila se recupera de nuevo desde este conjunto de resultados o se actualiza mediante **SQLSetPos**, su estado es SQL_ROW_SUCCESS.|  
|SQL_ROW_NOROW|El conjunto de filas se superpone al final del conjunto de resultados y no se devolvió ninguna fila que corresponda a este elemento de la matriz de estado de fila.|  
  
 [1] para los cursores KEYSET, mixto y dinámico, si se actualiza un valor de clave, se considera que la fila de datos se ha eliminado y se ha agregado una nueva fila.  
  
 [2] algunos controladores no pueden detectar las actualizaciones de los datos y, por lo tanto, no pueden devolver este valor. Para determinar si un controlador puede detectar actualizaciones de filas recuperadas, una aplicación llama a **SQLGetInfo** con la opción SQL_ROW_UPDATES.  
  
 [3]   **SQLFetch** solo puede devolver este valor cuando se combina con llamadas a **SQLFetchScroll**. Esto se debe a que el **SQLFetch** avanza por el conjunto de resultados y, cuando se utiliza exclusivamente, no vuelve a capturar ninguna fila. Dado que no se recupera ninguna fila, **SQLFetch** no detecta los cambios realizados en las filas capturadas previamente. Sin embargo, si **SQLFetchScroll** coloca el cursor antes de cualquier fila capturada anteriormente y se usa **SQLFetch** para capturar esas filas, **SQLFetch** puede detectar cualquier cambio en esas filas.  
  
 [4] solo devuelto por SQLBulkOperations. No establecido por **SQLFetch** o **SQLFetchScroll**.  
  
### <a name="rows-fetched-buffer"></a>Búfer de filas recuperadas  
 El búfer de filas recuperadas se usa para devolver el número de filas recuperadas, incluidas las filas para las que no se devolvieron datos porque se produjo un error mientras se capturaban. En otras palabras, es el número de filas para las que no se SQL_ROW_NOROW el valor de la matriz de estado de fila. La dirección de este búfer se especifica con el atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR. La aplicación asigna el búfer. Se establece mediante **SQLFetch** y **SQLFetchScroll**. Si el valor del atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR es un puntero nulo, estas funciones no devuelven el número de filas recuperadas. Para determinar el número de la fila actual del conjunto de resultados, una aplicación puede llamar a **SQLGetStmtAttr** con el atributo SQL_ATTR_ROW_NUMBER.  
  
 El contenido del búfer de filas recuperadas no está definido si **SQLFetch** o **SQLFetchScroll** no devuelve SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, excepto cuando se devuelve SQL_NO_DATA, en cuyo caso el valor del búfer de filas capturadas se establece en 0.  
  
### <a name="error-handling"></a>Tratamiento de errores  
 Los errores y las advertencias se pueden aplicar a filas individuales o a toda la función. Para obtener más información sobre los registros de diagnóstico, consulte [diagnósticos](../../../odbc/reference/develop-app/diagnostics.md) y [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
#### <a name="errors-and-warnings-on-the-entire-function"></a>Errores y advertencias en toda la función  
 Si se aplica un error a toda la función, como SQLSTATE HYT00 (tiempo de espera expirado) o SQLSTATE 24000 (estado de cursor no válido), **SQLFetch** devuelve SQL_ERROR y el SQLSTATE aplicable. El contenido de los búferes del conjunto de filas no está definido y la posición del cursor no se modifica.  
  
 Si se aplica una advertencia a la función completa, **SQLFetch** devuelve SQL_SUCCESS_WITH_INFO y el SQLSTATE aplicable. Se devuelven los registros de estado de las advertencias que se aplican a toda la función antes de los registros de estado que se aplican a las filas individuales.  
  
#### <a name="errors-and-warnings-in-individual-rows"></a>Errores y advertencias en filas individuales  
 Si un error (por ejemplo, SQLSTATE 22012 (división por cero)) o una advertencia (como SQLSTATE 01004 (datos truncados)) se aplican a una sola fila, **SQLFetch** realiza lo siguiente:  
  
-   Establece el elemento correspondiente de la matriz de estado de fila en SQL_ROW_ERROR para errores o SQL_ROW_SUCCESS_WITH_INFO para advertencias.  
  
-   Agrega cero o más registros de estado que contienen SQLSTATEs para el error o la advertencia.  
  
-   Establece los campos de número de fila y columna en los registros de estado. Si **SQLFetch** no puede determinar un número de fila o columna, establece ese número en SQL_ROW_NUMBER_UNKNOWN o SQL_COLUMN_NUMBER_UNKNOWN, respectivamente. Si el registro de estado no se aplica a una columna determinada, **SQLFetch** establece el número de columna en SQL_NO_COLUMN_NUMBER.  
  
 **SQLFetch** continúa recuperando filas hasta que haya capturado todas las filas del conjunto de filas. Devuelve SQL_SUCCESS_WITH_INFO a menos que se produzca un error en cada fila del conjunto de filas (sin incluir las filas con el estado SQL_ROW_NOROW), en cuyo caso devuelve SQL_ERROR. En concreto, si el tamaño del conjunto de filas es 1 y se produce un error en esa fila, **SQLFetch** devuelve SQL_ERROR.  
  
 **SQLFetch** devuelve los registros de estado en orden de número de fila. Es decir, devuelve todos los registros de estado de las filas desconocidas (si existen); a continuación, devuelve todos los registros de estado de la primera fila (si existe) y, a continuación, devuelve todos los registros de estado para la segunda fila (si existe), y así sucesivamente. Los registros de estado de cada fila se ordenan de acuerdo con las reglas normales para el orden de los registros de estado. para obtener más información, vea "secuencia de registros de estado" en [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
### <a name="descriptors-and-sqlfetch"></a>Descriptores y SQLFetch  
 En las secciones siguientes se **describe cómo interactúa** con los descriptores.  
  
#### <a name="argument-mappings"></a>Asignaciones de argumentos  
 El controlador no establece ningún campo de descriptor en función de los argumentos de **SQLFetch**.  
  
#### <a name="other-descriptor-fields"></a>Otros campos de descriptor  
 El uso de los siguientes campos de descriptor es **SQLFetch**.  
  
|Campo descriptor|Multilínea.|Campo de|Establecer mediante|  
|----------------------|-----------|--------------|-----------------|  
|SQL_DESC_ARRAY_SIZE|ARD|header|SQL_ATTR_ROW_ARRAY_SIZE (atributo de instrucción)|  
|SQL_DESC_ARRAY_STATUS_PTR|IRD|header|SQL_ATTR_ROW_STATUS_PTR (atributo de instrucción)|  
|SQL_DESC_BIND_OFFSET_PTR|ARD|header|SQL_ATTR_ROW_BIND_OFFSET_PTR (atributo de instrucción)|  
|SQL_DESC_BIND_TYPE|ARD|header|SQL_ATTR_ROW_BIND_TYPE (atributo de instrucción)|  
|SQL_DESC_COUNT|ARD|header|Argumento *ColumnNumber* de **SQLBindCol**|  
|SQL_DESC_DATA_PTR|ARD|records|Argumento *TargetValuePtr* de **SQLBindCol**|  
|SQL_DESC_INDICATOR_PTR|ARD|records|*StrLen_or_IndPtr* argumento en **SQLBindCol**|  
|SQL_DESC_OCTET_LENGTH|ARD|records|Argumento *BufferLength* en **SQLBindCol**|  
|SQL_DESC_OCTET_LENGTH_PTR|ARD|records|*StrLen_or_IndPtr* argumento en **SQLBindCol**|  
|SQL_DESC_ROWS_PROCESSED_PTR|IRD|header|SQL_ATTR_ROWS_FETCHED_PTR (atributo de instrucción)|  
|SQL_DESC_TYPE|ARD|records|Argumento *TargetType* en **SQLBindCol**|  
  
 Todos los campos de descriptor también se pueden establecer a través de **SQLSetDescField**.  
  
#### <a name="separate-length-and-indicator-buffers"></a>Búferes de longitud y indicador independientes  
 Las aplicaciones pueden enlazar un único búfer o dos búferes independientes que se pueden usar para contener valores de longitud y indicador. Cuando una aplicación llama a **SQLBindCol**, el controlador establece los campos SQL_DESC_OCTET_LENGTH_PTR y SQL_DESC_INDICATOR_PTR de ARD en la misma dirección, que se pasa en el argumento *StrLen_or_IndPtr* . Cuando una aplicación llama a **SQLSetDescField** o **SQLSetDescRec**, puede establecer estos dos campos en direcciones diferentes.  
  
 **SQLFetch** determina si la aplicación ha especificado la longitud y los búferes de indicador independientes. En este caso, cuando los datos no son NULL, **SQLFetch** establece el búfer de indicador en 0 y devuelve la longitud en el búfer de longitud. Cuando los datos son NULL, **SQLFetch** establece el búfer de indicador en SQL_NULL_DATA y no modifica el búfer de longitud.  
  
### <a name="code-example"></a>Ejemplo de código  
 Vea [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md), [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md), [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)y [SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md).  
  
### <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver información acerca de una columna de un conjunto de resultados|[SQLDescribeCol (función)](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Cerrar el cursor en la instrucción|[Función SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Capturar parte o toda una columna de datos|[Función SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Devolver el número de columnas del conjunto de resultados|[SQLNumResultCols (función)](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Preparar una instrucción para su ejecución|[Función SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
