---
description: Función SQLSetPos
title: Función SQLSetPos | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetPos
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetPos
helpviewer_keywords:
- SQLSetPos function [ODBC]
ms.assetid: 80190ee7-ae3b-45e5-92a9-693eb558f322
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4793fe06d25b090dfa6306eb8c4e72d513abc8d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188038"
---
# <a name="sqlsetpos-function"></a>Función SQLSetPos
**Conformidad**  
 Versión introducida: ODBC 1,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLSetPos** establece la posición del cursor en un conjunto de filas y permite que una aplicación actualice los datos del conjunto de filas o actualice o elimine los datos del conjunto de resultados.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLSetPos(  
      SQLHSTMT        StatementHandle,  
      SQLSETPOSIROW   RowNumber,  
      SQLUSMALLINT    Operation,  
      SQLUSMALLINT    LockType);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *RowNumber*  
 Entradas Posición de la fila del conjunto de filas en la que se va a realizar la operación especificada con el argumento *Operation* . Si *RowNumber* es 0, la operación se aplica a todas las filas del conjunto de filas.  
  
 Para obtener más información, vea "Comentarios".  
  
 *operación*  
 Entradas Operación que se va a realizar:  
  
 SQL_POSITION SQL_REFRESH SQL_UPDATE SQL_DELETE  
  
> [!NOTE]
>  El valor de SQL_ADD para el argumento *Operation* está en desuso para ODBC *3. x*. Los controladores ODBC *3. x* deberán admitir SQL_ADD para la compatibilidad con versiones anteriores. Esta funcionalidad se ha reemplazado por una llamada a **SQLBulkOperations** con una *operación* de SQL_ADD. Cuando una aplicación ODBC *3. x* funciona con un controlador *ODBC 2. x* , el administrador de controladores asigna una llamada a **SQLBulkOperations** con una *operación* de SQL_ADD a **SQLSetPos** con una *operación* de SQL_ADD.  
  
 Para obtener más información, vea "Comentarios".  
  
 *LockType*  
 Entradas Especifica cómo bloquear la fila después de realizar la operación especificada en el argumento de *operación* .  
  
 SQL_LOCK_NO_CHANGE SQL_LOCK_EXCLUSIVE SQL_LOCK_UNLOCK  
  
 Para obtener más información, vea "Comentarios".  
  
## <a name="returns"></a>Devoluciones  
  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLSetPos** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLSetPos** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
 En el caso de todos los SQLSTATEs que pueden devolver SQL_SUCCESS_WITH_INFO o SQL_ERROR (excepto 01xxx SQLSTATEs), se devuelve SQL_SUCCESS_WITH_INFO si se produce un error en una o varias filas de una operación de MultiRow y se devuelve SQL_ERROR si se produce un error en una operación de una sola fila.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01001|Conflicto de operación de cursor|El argumento de *operación* se SQL_DELETE o SQL_UPDATE y no se eliminó o actualizó ninguna fila o más de una fila. (Para obtener más información acerca de las actualizaciones de más de una fila, vea la descripción del *atributo* SQL_ATTR_SIMULATE_CURSOR en **SQLSetStmtAttr**). (La función devuelve SQL_SUCCESS_WITH_INFO).<br /><br /> El argumento de *operación* se SQL_DELETE o SQL_UPDATE y se produjo un error en la operación debido a la simultaneidad optimista. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Truncamiento de derecha de datos de cadena|El argumento de *operación* se SQL_REFRESH, y los datos binarios o de cadena devueltos para una columna o columnas con un tipo de datos de SQL_C_CHAR o SQL_C_BINARY dieron como resultado el truncamiento de datos binarios de caracteres no vacíos o no nulos.|  
|01S01|Error en la fila|El argumento *RowNumber* era 0 y se produjo un error en una o varias filas mientras se realizaba la operación especificada con el argumento *Operation* .<br /><br /> (SQL_SUCCESS_WITH_INFO se devuelve si se produce un error en una o más filas de una operación de varias filas y se devuelve SQL_ERROR si se produce un error en una operación de una sola fila).<br /><br /> (Este SQLSTATE solo se devuelve cuando se llama a **SQLSetPos** después de **SQLExtendedFetch**, si el controlador es ODBC *2. x* y no se utiliza la biblioteca de cursores).|  
|01S07|Truncamiento fraccionario|El argumento de *operación* se SQL_REFRESH, el tipo de datos del búfer de aplicación no era SQL_C_CHAR o SQL_C_BINARY, y los datos devueltos a los búferes de la aplicación para una o más columnas se truncaron. En el caso de los tipos de datos numéricos, se truncó la parte fraccionaria del número. En el caso de los tipos de datos Time, TIMESTAMP y Interval que contienen un componente de hora, se truncó la parte fraccionaria de la hora.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|No se pudo convertir el valor de datos de una columna del conjunto de resultados al tipo de datos especificado por *TargetType* en la llamada a **SQLBindCol**.|  
|07009|Índice de descriptor no válido|La *operación* de argumento se SQL_REFRESH o SQL_UPDATE y una columna estaba enlazada con un número de columna mayor que el número de columnas del conjunto de resultados.|  
|21S02|El grado de la tabla derivada no coincide con la lista de columnas|La *operación* de argumento se SQL_UPDATE, y no se puede actualizar ninguna columna porque todas las columnas eran independientes, de solo lectura, o bien se SQL_COLUMN_IGNORE el valor del búfer de indicador/longitud enlazado.|  
|22001|Datos de cadena, truncamiento derecho|El argumento de *operación* se SQL_UPDATE y la asignación de un valor de carácter o binario a una columna dio como resultado el truncamiento de caracteres que no están en blanco (para caracteres) o que no son NULL (para binario) o bytes.|  
|22003|Valor numérico fuera del intervalo|Se SQL_UPDATE la *operación* de argumento y la asignación de un valor numérico a una columna del conjunto de resultados provocó la parte entera (en oposición a fraccionaria) del número que se va a truncar.<br /><br /> La *operación* de argumento se SQL_REFRESH y devolver el valor numérico de una o varias columnas enlazadas habría causado una pérdida de dígitos significativos.|  
|22007|Formato de fecha y hora no válido|La *operación* de argumento se SQL_UPDATE, y la asignación de un valor de fecha o marca de tiempo a una columna del conjunto de resultados ha provocado que el campo de año, mes o día esté fuera del intervalo.<br /><br /> La *operación* de argumento se SQL_REFRESH y devolver el valor de fecha o marca de tiempo de una o varias columnas enlazadas habría provocado que el campo Year, month o Day esté fuera del intervalo.|  
|22008|Desbordamiento del campo de fecha y hora|El argumento de *operación* se SQL_UPDATE, y el rendimiento de la aritmética de DateTime en los datos que se envían a una columna del conjunto de resultados provocó que un campo DateTime (el año, mes, día, hora, minuto o segundo) del resultado fuera del intervalo de valores permitido para el campo o no es válido en función de las reglas naturales del calendario gregoriano para fechas y horas.<br /><br /> Se SQL_REFRESH el argumento *Operation* y el rendimiento de las operaciones aritméticas de fecha y hora en los datos que se recuperan del conjunto de resultados resultó en un campo DateTime (el año, mes, día, hora, minuto o segundo campo) del resultado que está fuera del intervalo de valores permitido para el campo o que no es válido según las reglas naturales del calendario gregoriano para los valores DateTime.|  
|22015|Desbordamiento de campo de intervalo|El argumento de *operación* se SQL_UPDATE y la asignación de un tipo numérico exacto o de intervalo C a un tipo de datos SQL de intervalo produce una pérdida de dígitos significativos.<br /><br /> Se SQL_UPDATE el argumento de la *operación* ; al asignar a un tipo SQL de intervalo, no había ninguna representación del valor del tipo C en el tipo SQL de intervalo.<br /><br /> El argumento de *operación* se SQL_REFRESH y la asignación de un tipo numérico exacto o de intervalo SQL a un tipo de intervalo C provocó una pérdida de dígitos significativos en el campo inicial.<br /><br /> El argumento de *operación* se SQL_ actualizar; al asignar a un tipo de intervalo C, no había ninguna representación del valor del tipo SQL en el tipo de intervalo C.|  
|22018|Valor de carácter no válido para la especificación de conversión|Se SQL_REFRESH el argumento de la *operación* ; el tipo C era un tipo de datos numérico exacto o aproximado, un valor de fecha y hora o un intervalo. el tipo SQL de la columna era un tipo de datos de caracteres. y el valor de la columna no era un literal válido del tipo C enlazado.<br /><br /> Se SQL_UPDATE la *operación* de argumento; el tipo SQL era un tipo numérico exacto o aproximado, un valor de fecha y hora o un tipo de datos de intervalo; se SQL_C_CHAR el tipo C; y el valor de la columna no era un literal válido del tipo SQL enlazado.|  
|23000|Infracción de la restricción de integridad|La *operación* de argumento se SQL_DELETE o SQL_UPDATE y se infringió una restricción de integridad.|  
|24000|Estado de cursor no válido|El *StatementHandle* estaba en un estado ejecutado, pero no hay ningún conjunto de resultados asociado a *StatementHandle*.<br /><br /> (DM) un cursor estaba abierto en el *StatementHandle*, pero no se ha llamado a **SQLFetch** o **SQLFetchScroll** .<br /><br /> Un cursor estaba abierto en el *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** , pero el cursor se colocó antes del inicio del conjunto de resultados o después del final del conjunto de resultados.<br /><br /> La *operación* de argumento se SQL_DELETE, SQL_REFRESH o SQL_UPDATE y el cursor se colocó antes del inicio del conjunto de resultados o después del final del conjunto de resultados.|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|42000|Error de sintaxis o infracción de acceso|El controlador no pudo bloquear la fila según sea necesario para realizar la operación solicitada en la *operación* de argumento.<br /><br /> El controlador no pudo bloquear la fila tal como se solicitó en el argumento *LockType*.|  
|44000|Infracción de WITH CHECK OPTION|Se SQL_UPDATE el argumento *Operation* y la actualización se realizó en una tabla vista o en una tabla derivada de la tabla vista que se creó especificando **with check Option**, de modo que una o varias filas afectadas por la actualización ya no estarán presentes en la tabla vista.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle* y, a continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función SQLSetPos.<br /><br /> (DM) el *StatementHandle* especificado no se encontraba en un estado ejecutado. Se llamó a la función sin llamar primero a **SQLExecDirect**, **SQLExecute** o a una función de catálogo.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) el controlador era un controlador ODBC *2. x* y se llamó a **SQLSetPos** para un *StatementHandle* después de llamar a **SQLFetch** .|  
|HY011|El atributo no se puede establecer ahora|(DM) el controlador era un controlador ODBC *2. x.* se estableció el atributo de instrucción SQL_ATTR_ROW_STATUS_PTR. después se llamó a **SQLSetPos** antes de llamar a **SQLFetch**, **SQLFetchScroll** o **SQLExtendedFetch** .|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|El argumento de *operación* se SQL_UPDATE, un valor de datos era un puntero nulo y el valor de la longitud de columna no era 0, SQL_DATA_AT_EXEC, SQL_COLUMN_IGNORE, SQL_NULL_DATA, o menor o igual que SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Se SQL_UPDATE el argumento de la *operación* ; un valor de datos no era un puntero nulo; el tipo de datos C se SQL_C_BINARY o SQL_C_CHAR; y el valor de la longitud de la columna era menor que 0 pero no es igual a SQL_DATA_AT_EXEC, SQL_COLUMN_IGNORE, SQL_NTS o SQL_NULL_DATA, o menor o igual que SQL_LEN_DATA_AT_EXEC_OFFSET.<br /><br /> Se SQL_DATA_AT_EXEC el valor de un búfer de longitud/indicador; el tipo SQL era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo de datos específico del origen de datos largo; y el tipo de información SQL_NEED_LONG_DATA_LEN en **SQLGetInfo** era "Y".|  
|HY092|Identificador de atributo no válido|(DM) el valor especificado para el argumento de la *operación* no era válido.<br /><br /> (DM) el valor especificado para el argumento *LockType* no era válido.<br /><br /> El argumento de *operación* se SQL_UPDATE o SQL_DELETE y se SQL_ATTR_CONCUR_READ_ONLY el atributo de instrucción SQL_ATTR_CONCURRENCY.|  
|HY107|Valor de fila fuera del intervalo|El valor especificado para el argumento *RowNumber* era mayor que el número de filas del conjunto de filas.|  
|HY109|Posición del cursor no válida|El cursor asociado con *StatementHandle* se definió como de solo avance, por lo que el cursor no se pudo colocar en el conjunto de filas. Vea la descripción del atributo SQL_ATTR_CURSOR_TYPE en **SQLSetStmtAttr**.<br /><br /> El argumento de *operación* se SQL_UPDATE, SQL_DELETE o SQL_REFRESH, y la fila identificada por el argumento *RowNumber* se ha eliminado o no se ha capturado.<br /><br /> (DM) el argumento *RowNumber* era 0 y el argumento *Operation* se SQL_POSITION.<br /><br /> Se llamó a **SQLSetPos** después de llamar a **SQLBulkOperations** y antes de llamar a **SQLFetchScroll** o **SQLFetch** .|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El controlador o el origen de datos no admite la operación solicitada en el argumento *Operation* o el argumento *LockType* .|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados. El período de tiempo de espera se establece a través de **SQLSetStmtAttr** con un *atributo* de SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
  
> [!CAUTION]
>  Para obtener información sobre la instrucción que indica que se puede llamar a **SQLSetPos** en y lo que necesita para la compatibilidad con aplicaciones ODBC *2. x* , consulte [cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md).  
  
## <a name="rownumber-argument"></a>RowNumber (argumento)  
 El argumento *RowNumber* especifica el número de la fila del conjunto de filas en la que se va a realizar la operación especificada por el argumento de la *operación* . Si *RowNumber* es 0, la operación se aplica a todas las filas del conjunto de filas. *RowNumber* debe ser un valor comprendido entre 0 y el número de filas del conjunto de filas.  
  
> [!NOTE]  
>  En el lenguaje C, las matrices se basan en 0 y el argumento *RowNumber* se basa en 1. Por ejemplo, para actualizar la quinta fila del conjunto de filas, una aplicación modifica los búferes del conjunto de filas en el índice de matriz 4, pero especifica un *RowNumber* de 5.  
  
 Todas las operaciones sitúan el cursor en la fila especificada por *RowNumber*. Las siguientes operaciones requieren una posición del cursor:  
  
-   Instrucciones Update y DELETE posicionadas.  
  
-   Llamadas a **SQLGetData**.  
  
-   Llama a **SQLSetPos** con las opciones SQL_DELETE, SQL_REFRESH y SQL_UPDATE.  
  
 Por ejemplo, si *RowNumber* es 2 para una llamada a **SQLSetPos** con una *operación* de SQL_DELETE, el cursor se coloca en la segunda fila del conjunto de filas y se elimina esa fila. La entrada de la matriz de estado de fila de implementación (indicada por el atributo de instrucción SQL_ATTR_ROW_STATUS_PTR) de la segunda fila se cambia a SQL_ROW_DELETED.  
  
 Una aplicación puede especificar una posición del cursor cuando llama a **SQLSetPos**. Generalmente, llama a **SQLSetPos** con la operación SQL_POSITION o SQL_REFRESH para colocar el cursor antes de ejecutar una instrucción UPDATE o DELETE posicionada o llamando a **SQLGetData**.  
  
## <a name="operation-argument"></a>Argumento de operación  
 El argumento *Operation* admite las siguientes operaciones. Para determinar qué opciones admite un origen de datos, una aplicación llama a **SQLGetInfo** con el tipo de información SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 o SQL_STATIC_CURSOR_ATTRIBUTES1 (dependiendo del tipo de cursor).  
  
|*operación*<br /><br /> Argumento|Operación|  
|------------------------------|---------------|  
|SQL_POSITION|El controlador coloca el cursor en la fila especificada por *RowNumber*.<br /><br /> Se omite el contenido de la matriz de estado de fila a la que señala el atributo de instrucción SQL_ATTR_ROW_OPERATION_PTR para la *operación* de SQL_POSITION.|  
|SQL_REFRESH|El controlador coloca el cursor en la fila especificada por *RowNumber* y actualiza los datos en los búferes del conjunto de filas de esa fila. Para obtener más información sobre cómo el controlador devuelve datos en los búferes del conjunto de filas, vea las descripciones de los enlaces de modo de fila y de modo de columna en **SQLBindCol**.<br /><br /> **SQLSetPos** con una *operación* de SQL_REFRESH actualiza el estado y el contenido de las filas del conjunto de filas capturado actual. Esto incluye actualizar los marcadores. Dado que los datos de los búferes se actualizan pero no se recuperan, la pertenencia al conjunto de filas es fija. Esto es diferente de la actualización realizada por una llamada a **SQLFetchScroll** con un *FetchOrientation* de SQL_FETCH_RELATIVE y un *RowNumber* igual a 0, que recupera el conjunto de filas del conjunto de resultados para que pueda mostrar los datos agregados y quitar los datos eliminados si las operaciones son compatibles con el controlador y el cursor.<br /><br /> Una actualización correcta con **SQLSetPos** no cambiará el estado de fila de SQL_ROW_DELETED. Las filas eliminadas del conjunto de filas seguirán marcadas como eliminadas hasta la siguiente captura. Las filas desaparecerán en la siguiente captura si el cursor admite el empaquetado (en el que un siguiente **SQLFetch** o **SQLFetchScroll** no devuelve filas eliminadas).<br /><br /> Las filas agregadas no aparecen cuando se realiza una actualización con **SQLSetPos** . Este comportamiento es diferente de **SQLFetchScroll** con un *FetchType* de SQL_FETCH_RELATIVE y un *RowNumber* igual a 0, que también actualiza el conjunto de filas actual pero mostrará registros agregados o paquetes eliminados si el cursor admite estas operaciones.<br /><br /> Una actualización correcta con **SQLSetPos** cambiará el estado de fila SQL_ROW_ADDED a SQL_ROW_SUCCESS (si la matriz de estado de fila existe).<br /><br /> Una actualización correcta con **SQLSetPos** cambiará el estado de una fila de SQL_ROW_UPDATED al nuevo estado de la fila (si existe la matriz de estado de la fila).<br /><br /> Si se produce un error en una operación **SQLSetPos** en una fila, el estado de la fila se establece en SQL_ROW_ERROR (si existe la matriz de estado de la fila).<br /><br /> Para un cursor abierto con un atributo de instrucción SQL_ATTR_CONCURRENCY de SQL_CONCUR_ROWVER o SQL_CONCUR_VALUES, una actualización con **SQLSetPos** podría actualizar los valores de simultaneidad optimista utilizados por el origen de datos para detectar que la fila ha cambiado. Si esto ocurre, se actualizan las versiones de fila o los valores utilizados para garantizar la simultaneidad del cursor siempre que los búferes del conjunto de filas se actualizan desde el servidor. Esto sucede en cada fila que se actualiza.<br /><br /> Se omite el contenido de la matriz de estado de fila a la que señala el atributo de instrucción SQL_ATTR_ROW_OPERATION_PTR para la *operación* de SQL_REFRESH.|  
|SQL_UPDATE|El controlador coloca el cursor en la fila especificada por *RowNumber* y actualiza la fila subyacente de datos con los valores de los búferes del conjunto de filas (el argumento *TargetValuePtr* en **SQLBindCol**). Recupera las longitudes de los datos de los búferes de longitud/indicador (el argumento *StrLen_or_IndPtr* en **SQLBindCol**). Si la longitud de una columna es SQL_COLUMN_IGNORE, no se actualiza la columna. Después de actualizar la fila, el controlador cambia el elemento correspondiente de la matriz de estado de fila a SQL_ROW_UPDATED o SQL_ROW_SUCCESS_WITH_INFO (si la matriz de estado de fila existe).<br /><br /> Está definido por el controlador cuál es el comportamiento si se llama a **SQLSetPos** con un argumento de *operación* de SQL_UPDATE en un cursor que contiene columnas duplicadas. El controlador puede devolver un SQLSTATE definido por el controlador, puede actualizar la primera columna que aparece en el conjunto de resultados o realizar otro comportamiento definido por el controlador.<br /><br /> La matriz de operación de fila a la que apunta el atributo de instrucción SQL_ATTR_ROW_OPERATION_PTR se puede utilizar para indicar que una fila del conjunto de filas actual debe omitirse durante una actualización masiva. Para obtener más información, vea "matrices de estado y de operaciones" más adelante en esta referencia de función.|  
|SQL_DELETE|El controlador coloca el cursor en la fila especificada por *RowNumber* y elimina la fila de datos subyacente. Cambia el elemento correspondiente de la matriz de estado de fila a SQL_ROW_DELETED. Una vez eliminada la fila, las siguientes no son válidas para la fila: las instrucciones Update y DELETE posicionadas, las llamadas a **SQLGetData** y las llamadas a **SQLSetPos** con la *operación* establecida en cualquier cosa excepto SQL_POSITION. En el caso de los controladores que admiten el empaquetado, la fila se elimina del cursor cuando se recuperan nuevos datos desde el origen de datos.<br /><br /> El hecho de que la fila permanezca visible depende del tipo de cursor. Por ejemplo, las filas eliminadas son visibles para los cursores estáticos y los controlados por conjunto de claves, pero no son visibles para los cursores dinámicos.<br /><br /> La matriz de operación de fila a la que apunta el atributo de instrucción SQL_ATTR_ROW_OPERATION_PTR se puede utilizar para indicar que una fila del conjunto de filas actual debe omitirse durante una eliminación masiva. Para obtener más información, vea "matrices de estado y de operaciones" más adelante en esta referencia de función.|  
  
## <a name="locktype-argument"></a>LockType (argumento)  
 El argumento *LockType* proporciona una manera para que las aplicaciones controlen la simultaneidad. En la mayoría de los casos, los orígenes de datos que admiten niveles de simultaneidad y transacciones solo admitirán el valor SQL_LOCK_NO_CHANGE del argumento *LockType* . Normalmente, el argumento *LockType* solo se usa para la compatibilidad basada en archivos.  
  
 El argumento *LockType* especifica el estado de bloqueo de la fila después de que se haya ejecutado **SQLSetPos** . Si el controlador no puede bloquear la fila para realizar la operación solicitada o para satisfacer el argumento *LockType* , devuelve SQL_ERROR y SQLSTATE 42000 (error de sintaxis o infracción de acceso).  
  
 Aunque el argumento *LockType* se especifica para una única instrucción, el bloqueo concede los mismos privilegios a todas las instrucciones de la conexión. En concreto, un bloqueo adquirido por una instrucción en una conexión se puede desbloquear mediante una instrucción diferente en la misma conexión.  
  
 Una fila bloqueada a **SQLSetPos** permanece bloqueada hasta que la aplicación llama a **SQLSetPos** para la fila con *LockType* establecida en SQL_LOCK_UNLOCK, o hasta que la aplicación llama a **SQLFreeHandle** para la instrucción o **SQLFreeStmt** con la opción SQL_CLOSE. Para un controlador que admite transacciones, una fila bloqueada a **SQLSetPos** se desbloquea cuando la aplicación llama a **SQLEndTran** para confirmar o revertir una transacción en la conexión (si se cierra un cursor cuando se confirma o se revierte una transacción, como indica el SQL_CURSOR_COMMIT_BEHAVIOR y SQL_CURSOR_ROLLBACK_BEHAVIOR tipos de información devueltos por **SQLGetInfo**).  
  
 El argumento *LockType* admite los siguientes tipos de bloqueos. Para determinar qué bloqueos se admiten en un origen de datos, una aplicación llama a **SQLGetInfo** con el tipo de información SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 o SQL_STATIC_CURSOR_ATTRIBUTES1 (dependiendo del tipo del cursor).  
  
|*LockType* (argumento)|Tipo de bloqueo|  
|-------------------------|---------------|  
|SQL_LOCK_NO_CHANGE|El controlador o el origen de datos garantiza que la fila se encuentra en el mismo estado bloqueado o desbloqueado que tenía antes de que se llamase a **SQLSetPos** . Este valor de *LockType* permite que los orígenes de datos que no admiten el bloqueo explícito de nivel de fila utilicen el bloqueo que requieran los niveles de aislamiento de transacción y simultaneidad actuales.|  
|SQL_LOCK_EXCLUSIVE|El controlador o el origen de datos bloquea la fila exclusivamente. No se puede usar una instrucción en una conexión diferente o en una aplicación diferente para adquirir bloqueos en la fila.|  
|SQL_LOCK_UNLOCK|El controlador o el origen de datos desbloquea la fila.|  
  
 Si un controlador admite SQL_LOCK_EXCLUSIVE pero no admite SQL_LOCK_UNLOCK, una fila bloqueada permanecerá bloqueada hasta que se produzca una de las llamadas de función descritas en el párrafo anterior.  
  
 Si un controlador admite SQL_LOCK_EXCLUSIVE pero no admite SQL_LOCK_UNLOCK, una fila bloqueada permanecerá bloqueada hasta que la aplicación llame a **SQLFreeHandle** para la instrucción o **SQLFreeStmt** con la opción SQL_CLOSE. Si el controlador admite transacciones y cierra el cursor al confirmar o revertir la transacción, la aplicación llama a **SQLEndTran**.  
  
 En el caso de las operaciones de actualización y eliminación en **SQLSetPos**, la aplicación usa el argumento *LockType* como se indica a continuación:  
  
-   Para garantizar que una fila no cambie una vez recuperada, una aplicación llama a **SQLSetPos** con la *operación* establecida en SQL_REFRESH y *LockType* establecida en SQL_LOCK_EXCLUSIVE.  
  
-   Si la aplicación establece *LockType* en SQL_LOCK_NO_CHANGE, el controlador garantiza que una operación de actualización o eliminación se realizará correctamente solo si la aplicación especificada SQL_CONCUR_LOCK para el atributo de instrucción SQL_ATTR_CONCURRENCY.  
  
-   Si la aplicación especifica SQL_CONCUR_ROWVER o SQL_CONCUR_VALUES para el atributo de instrucción SQL_ATTR_CONCURRENCY, el controlador compara las versiones de fila o los valores y rechaza la operación si la fila ha cambiado desde que la aplicación ha capturado la fila.  
  
-   Si la aplicación especifica SQL_CONCUR_READ_ONLY para el atributo de instrucción SQL_ATTR_CONCURRENCY, el controlador rechaza cualquier operación de actualización o eliminación.  
  
 Para obtener más información sobre el atributo de instrucción SQL_ATTR_CONCURRENCY, vea [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
## <a name="status-and-operation-arrays"></a>Matrices de estado y de operación  
 Cuando se llama a **SQLSetPos**, se usan las matrices de estado y de operación siguientes:  
  
-   La matriz de estado de fila (indicada por el campo SQL_DESC_ARRAY_STATUS_PTR en el atributo IRD y la instrucción SQL_ATTR_ROW_STATUS_ARRAY) contiene los valores de estado de cada fila de datos del conjunto de filas. El controlador establece los valores de estado de esta matriz después de una llamada a **SQLFetch**, **SQLFetchScroll**, **SQLBulkOperations** o **SQLSetPos**. El atributo de instrucción SQL_ATTR_ROW_STATUS_PTR apunta a esta matriz.  
  
-   La matriz de operación de fila (indicada por el campo SQL_DESC_ARRAY_STATUS_PTR de los atributos ARD y SQL_ATTR_ROW_OPERATION_ARRAY Statement) contiene un valor para cada fila del conjunto de filas que indica si se omite o se realiza una llamada a **SQLSetPos** para una operación masiva. Cada elemento de la matriz se establece en SQL_ROW_PROCEED (valor predeterminado) o SQL_ROW_IGNORE. El atributo de instrucción SQL_ATTR_ROW_OPERATION_PTR apunta a esta matriz.  
  
 El número de elementos de las matrices de estado y de operación debe ser igual al número de filas del conjunto de filas (tal y como se define en el atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE).  
  
 Para obtener información sobre la matriz de estado de fila, vea [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md). Para obtener información sobre la matriz de operación de fila, vea "omitir una fila en una operación masiva", más adelante en esta sección.  
  
## <a name="using-sqlsetpos"></a>Usar SQLSetPos  
 Antes de que una aplicación llame a **SQLSetPos**, debe realizar la siguiente secuencia de pasos:  
  
1.  Si la aplicación va a llamar a **SQLSetPos** con la *operación* establecida en SQL_UPDATE, llame a **SQLBindCol** (o **SQLSetDescRec**) para cada columna para especificar su tipo de datos y búferes de enlace para los datos y la longitud de la columna.  
  
2.  Si la aplicación va a llamar a **SQLSetPos** con la *operación* establecida en SQL_DELETE o SQL_UPDATE, llame a **SQLColAttribute** para asegurarse de que las columnas que se van a eliminar o actualizar son actualizables.  
  
3.  Llame a **SQLExecDirect**, **SQLExecute** o una función de catálogo para crear un conjunto de resultados.  
  
4.  Llame a **SQLFetch** o **SQLFetchScroll** para recuperar los datos.  
  
 Para obtener más información sobre el uso de **SQLSetPos**, vea [actualizar datos con SQLSetPos](../../../odbc/reference/develop-app/updating-data-with-sqlsetpos.md).  
  
## <a name="deleting-data-using-sqlsetpos"></a>Eliminar datos mediante SQLSetPos  
 Para eliminar datos con **SQLSetPos**, una aplicación llama a **SQLSetPos** con *RowNumber* establecido en el número de la fila que se va a eliminar y en la *operación* establecida en SQL_DELETE.  
  
 Una vez eliminados los datos, el controlador cambia el valor de la matriz de estado de la fila de implementación de la fila adecuada a SQL_ROW_DELETED (o SQL_ROW_ERROR).  
  
## <a name="updating-data-using-sqlsetpos"></a>Actualizar datos mediante SQLSetPos  
 Una aplicación puede pasar el valor de una columna en el búfer de datos enlazados o con una o más llamadas a **SQLPutData**. Las columnas cuyos datos se pasan con **SQLPutData** se conocen como *columnas* de *datos en ejecución* . Normalmente se usan para enviar datos de SQL_LONGVARBINARY y SQL_LONGVARCHAR columnas y se pueden mezclar con otras columnas.  
  
#### <a name="to-update-data-with-sqlsetpos-an-application"></a>Para actualizar datos con SQLSetPos, una aplicación:  
  
1.  Coloca los valores en los datos y en los búferes de indicador y longitud enlazados con **SQLBindCol**:  
  
    -   En el caso de las columnas normales, la aplicación coloca el nuevo valor de columna en el búfer de *\* TargetValuePtr* y la longitud de ese valor en el búfer de *\* StrLen_or_IndPtr* . Si no se debe actualizar la fila, la aplicación coloca SQL_ROW_IGNORE en el elemento de esa fila de la matriz de operación de fila.  
  
    -   En el caso de las columnas de datos en ejecución, la aplicación coloca un valor definido por la aplicación, como el número de columna, en el búfer de *\* TargetValuePtr* . El valor se puede usar más adelante para identificar la columna.  
  
         La aplicación coloca el resultado de la macro SQL_LEN_DATA_AT_EXEC (*length*) en el búfer **StrLen_or_IndPtr* . Si el tipo de datos SQL de la columna es SQL_LONGVARBINARY, SQL_LONGVARCHAR o un tipo de datos específico de origen de datos largo y el controlador devuelve "Y" para el tipo de información SQL_NEED_LONG_DATA_LEN en **SQLGetInfo**, *length* es el número de bytes de datos que se van a enviar para el parámetro; de lo contrario, debe ser un valor no negativo y se omite.  
  
2.  Llama a **SQLSetPos** con el argumento de *operación* establecido en SQL_UPDATE para actualizar la fila de datos.  
  
    -   Si no hay columnas de datos en ejecución, el proceso se completa.  
  
    -   Si hay columnas de datos en ejecución, la función devuelve SQL_NEED_DATA y continúa en el paso 3.  
  
3.  Llama a **SQLParamData** para recuperar la dirección del búfer *\* TargetValuePtr* de la primera columna de datos en ejecución que se va a procesar. **SQLParamData** devuelve SQL_NEED_DATA. La aplicación recupera el valor definido por la aplicación del búfer *\* TargetValuePtr* .  
  
    > [!NOTE]  
    >  Aunque los parámetros de datos en ejecución son similares a las columnas de datos en ejecución, el valor devuelto por **SQLParamData** es diferente para cada uno.  
  
    > [!NOTE]  
    >  Los parámetros de datos en ejecución son parámetros de una instrucción SQL para los que se enviarán datos con **SQLPutData** cuando se ejecute la instrucción con **SQLExecDirect** o **SQLExecute**. Se enlazan con **SQLBindParameter** o estableciendo descriptores con **SQLSetDescRec**. El valor devuelto por **SQLParamData** es un valor de 32 bits que se pasa a **SQLBindParameter** en el argumento *ParameterValuePtr* .  
  
    > [!NOTE]  
    >  Las columnas de datos en ejecución son columnas de un conjunto de filas para el que se enviarán datos con **SQLPutData** cuando una fila se actualice con **SQLSetPos**. Están enlazadas con **SQLBindCol**. El valor devuelto por **SQLParamData** es la dirección de la fila del búfer **TargetValuePtr* que se está procesando.  
  
4.  Llama a **SQLPutData** una o más veces para enviar datos para la columna. Se necesita más de una llamada si no se pueden devolver todos los valores de datos en el búfer de *\* TargetValuePtr* especificado en **SQLPutData**; se permiten varias llamadas a **SQLPutData** para la misma columna solo cuando se envían datos de caracteres c a una columna con un tipo de datos de caracteres, binarios o de origen de datos, o cuando se envían datos binarios de c a una columna con un tipo de datos de  
  
5.  Llama de nuevo a **SQLParamData** para indicar que se han enviado todos los datos para la columna.  
  
    -   Si hay más columnas de datos en ejecución, **SQLParamData** devuelve SQL_NEED_DATA y la dirección del búfer *TargetValuePtr* para la siguiente columna de datos en ejecución que se va a procesar. La aplicación repite los pasos 4 y 5.  
  
    -   Si no hay más columnas de datos en ejecución, el proceso se completa. Si la instrucción se ha ejecutado correctamente, **SQLParamData** devuelve SQL_SUCCESS o SQL_SUCCESS_WITH_INFO; Si se produce un error en la ejecución, devuelve SQL_ERROR. En este punto, **SQLParamData** puede devolver cualquier SQLSTATE que pueda ser devuelto por **SQLSetPos**.  
  
 Si se han actualizado los datos, el controlador cambia el valor de la matriz de estado de la fila de implementación para que se SQL_ROW_UPDATED la fila correspondiente.  
  
 Si se cancela la operación o se produce un error en **SQLParamData** o **SQLPutData**, después **que SQLSetPos** devuelve SQL_NEED_DATA y antes de que se envíen los datos para todas las columnas de datos en ejecución, la aplicación solo puede llamar a **SQLCancel**, **SQLGetDiagField**, **SQLGetDiagRec**, **SQLGetFunctions**, **SQLParamData** o **SQLPutData** para la instrucción o la conexión asociada con la instrucción. Si llama a cualquier otra función para la instrucción o la conexión asociada a la instrucción, la función devuelve SQL_ERROR y SQLSTATE HY010 (error de secuencia de función).  
  
 Si la aplicación llama a **SQLCancel** mientras el controlador todavía necesita datos para las columnas de datos en ejecución, el controlador cancela la operación. A continuación, la aplicación puede llamar a **SQLSetPos** de nuevo; la cancelación no afecta al estado del cursor ni a la posición actual del cursor.  
  
 Cuando la lista de selección de la especificación de consulta asociada al cursor contiene más de una referencia a la misma columna, si se genera un error o si el controlador omite las referencias duplicadas y realiza las operaciones solicitadas definidas por el controlador.  
  
## <a name="performing-bulk-operations"></a>Realización de operaciones masivas  
 Si el argumento *RowNumber* es 0, el controlador realiza la operación especificada en el argumento *Operation* para cada fila del conjunto de filas que tenga un valor de SQL_ROW_PROCEED en el campo de la matriz de operación Row a la que apunta SQL_ATTR_ROW_OPERATION_PTR atributo Statement. Es un valor válido del argumento *RowNumber* para un argumento de *operación* de SQL_DELETE, SQL_REFRESH o SQL_UPDATE, pero no SQL_POSITION. **SQLSetPos** con una *operación* de SQL_POSITION y un *RowNumber* igual a 0 devolverá SQLSTATE HY109 (posición del cursor no válida).  
  
 Si se produce un error que pertenece al conjunto de filas completo, como SQLSTATE HYT00 (tiempo de espera agotado), el controlador devuelve SQL_ERROR y el SQLSTATE adecuado. El contenido de los búferes del conjunto de filas no está definido y la posición del cursor no se modifica.  
  
 Si se produce un error que pertenece a una única fila, el controlador:  
  
-   Establece el elemento de la fila de la matriz de estado de fila a la que apunta el atributo de instrucción SQL_ATTR_ROW_STATUS_PTR a SQL_ROW_ERROR.  
  
-   Envía una o más SQLSTATEs adicionales para el error en la cola de errores y establece el campo de SQL_DIAG_ROW_NUMBER en la estructura de datos de diagnóstico.  
  
 Después de haber procesado el error o la advertencia, si el controlador completa la operación para las filas restantes del conjunto de filas, devuelve SQL_SUCCESS_WITH_INFO. Por lo tanto, para cada fila que devolvió un error, la cola de errores contiene cero o más SQLSTATEs adicionales. Si el controlador detiene la operación después de haber procesado el error o la advertencia, devuelve SQL_ERROR.  
  
 Si el controlador devuelve advertencias, como SQLSTATE 01004 (datos truncados), devuelve las advertencias que se aplican a todo el conjunto de filas o a las filas desconocidas del conjunto de filas antes de devolver la información de error que se aplica a filas específicas. Devuelve advertencias para filas específicas junto con cualquier otra información de error sobre esas filas.  
  
 Si *RowNumber* es igual a 0 y la *operación* es SQL_UPDATE, SQL_REFRESH o SQL_DELETE, el número de filas en las que funciona **SQLSetPos** se señala mediante el atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR.  
  
 Si *RowNumber* es igual a 0 y la *operación* es SQL_DELETE, SQL_REFRESH o SQL_UPDATE, la fila actual después de la operación es la misma que la fila actual antes de la operación.  
  
## <a name="ignoring-a-row-in-a-bulk-operation"></a>Omitir una fila en una operación masiva  
 La matriz de operaciones de fila se puede usar para indicar que una fila del conjunto de filas actual debe omitirse durante una operación masiva mediante **SQLSetPos**. Para indicar al controlador que omita una o más filas durante una operación masiva, una aplicación debe realizar los siguientes pasos:  
  
1.  Llame a **SQLSetStmtAttr** para establecer el atributo de instrucción SQL_ATTR_ROW_OPERATION_PTR para que apunte a una matriz de SQLUSMALLINTs. Este campo también se puede establecer mediante una llamada a **SQLSetDescField** para establecer el campo de encabezado SQL_DESC_ARRAY_STATUS_PTR de ARD, que requiere que una aplicación obtenga el identificador de descriptor.  
  
2.  Establezca cada elemento de la matriz de operación de fila en uno de dos valores:  
  
    -   SQL_ROW_IGNORE, para indicar que la fila se excluye para la operación masiva.  
  
    -   SQL_ROW_PROCEED, para indicar que la fila se incluye en la operación masiva. Este es el valor predeterminado.  
  
3.  Llame a **SQLSetPos** para realizar la operación masiva.  
  
 Las siguientes reglas se aplican a la matriz de operaciones de fila:  
  
-   SQL_ROW_IGNORE y SQL_ROW_PROCEED afectan únicamente a las operaciones masivas que usan **SQLSetPos** con una *operación* de SQL_DELETE o SQL_UPDATE. No afectan a las llamadas a **SQLSetPos** con una *operación* de SQL_REFRESH o SQL_POSITION.  
  
-   De forma predeterminada, el puntero se establece en NULL.  
  
-   Si el puntero es null, todas las filas se actualizan como si todos los elementos estuvieran establecidos en SQL_ROW_PROCEED.  
  
-   Al establecer un elemento en SQL_ROW_PROCEED no se garantiza que la operación se produzca en esa fila concreta. Por ejemplo, si una determinada fila del conjunto de filas tiene el estado SQL_ROW_ERROR, es posible que el controlador no pueda actualizar esa fila sin tener en cuenta si la aplicación especificada SQL_ROW_PROCEED. Una aplicación siempre debe comprobar la matriz de estado de fila para ver si la operación se realizó correctamente.  
  
-   SQL_ROW_PROCEED se define como 0 en el archivo de encabezado. Una aplicación puede inicializar la matriz de operación de fila en 0 para procesar todas las filas.  
  
-   Si el número de elemento "n" en la matriz de operaciones de fila se establece en SQL_ROW_IGNORE y se llama a **SQLSetPos** para realizar una operación de actualización o eliminación masiva, la fila nth del conjunto de filas permanece sin cambios después de la llamada a **SQLSetPos**.  
  
-   Una aplicación debe establecer automáticamente una columna de solo lectura en SQL_ROW_IGNORE.  
  
## <a name="ignoring-a-column-in-a-bulk-operation"></a>Omitir una columna en una operación masiva  
 Para evitar los diagnósticos innecesarios de procesamiento generados por intentos de actualización en una o varias columnas de solo lectura, una aplicación puede establecer el valor del búfer de indicador/longitud enlazado en SQL_COLUMN_IGNORE. Para obtener más información, vea [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
## <a name="code-example"></a>Ejemplo de código  
 En el ejemplo siguiente, una aplicación permite al usuario examinar la tabla ORDERs y actualizar el estado de los pedidos. El cursor está controlado por conjunto de claves con un tamaño de conjunto de filas de 20 y utiliza el control de simultaneidad optimista para comparar las versiones de fila. Una vez que se captura cada conjunto de filas, la aplicación lo imprime y permite al usuario seleccionar y actualizar el estado de un pedido. La aplicación usa **SQLSetPos** para colocar el cursor en la fila seleccionada y realiza una actualización posicionada de la fila. (El control de errores se omite para mayor claridad).  
  
```cpp  
#define ROWS 20  
#define STATUS_LEN 6  
  
SQLCHAR        szStatus[ROWS][STATUS_LEN], szReply[3];  
SQLINTEGER     cbStatus[ROWS], cbOrderID;  
SQLUSMALLINT   rgfRowStatus[ROWS];  
SQLUINTEGER    sOrderID, crow = ROWS, irow;  
SQLHSTMT       hstmtS, hstmtU;  
  
SQLSetStmtAttr(hstmtS, SQL_ATTR_CONCURRENCY, (SQLPOINTER) SQL_CONCUR_ROWVER, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_CURSOR_TYPE, (SQLPOINTER) SQL_CURSOR_KEYSET_DRIVEN, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
SQLSetStmtAttr(hstmtS, SQL_ATTR_ROW_STATUS_PTR, (SQLPOINTER) rgfRowStatus, 0);  
SQLSetCursorName(hstmtS, "C1", SQL_NTS);  
SQLExecDirect(hstmtS, "SELECT ORDERID, STATUS FROM ORDERS ", SQL_NTS);  
  
SQLBindCol(hstmtS, 1, SQL_C_ULONG, &sOrderID, 0, &cbOrderID);  
SQLBindCol(hstmtS, 2, SQL_C_CHAR, szStatus, STATUS_LEN, &cbStatus);  
  
while ((retcode == SQLFetchScroll(hstmtS, SQL_FETCH_NEXT, 0)) != SQL_ERROR) {  
   if (retcode == SQL_NO_DATA_FOUND)  
      break;  
   for (irow = 0; irow < crow; irow++) {  
      if (rgfRowStatus[irow] != SQL_ROW_DELETED)  
         printf("%2d %5d %*s\n", irow+1, sOrderID, NAME_LEN-1, szStatus[irow]);  
   }  
   while (TRUE) {  
      printf("\nRow number to update?");  
      gets_s(szReply, 3);  
      irow = atoi(szReply);  
      if (irow > 0 && irow <= crow) {  
         printf("\nNew status?");  
         gets_s(szStatus[irow-1], (ROWS * STATUS_LEN));  
         SQLSetPos(hstmtS, irow, SQL_POSITION, SQL_LOCK_NO_CHANGE);  
         SQLPrepare(hstmtU,  
          "UPDATE ORDERS SET STATUS=? WHERE CURRENT OF C1", SQL_NTS);  
         SQLBindParameter(hstmtU, 1, SQL_PARAM_INPUT,  
            SQL_C_CHAR, SQL_CHAR,  
            STATUS_LEN, 0, szStatus[irow], 0, NULL);  
         SQLExecute(hstmtU);  
      } else if (irow == 0) {  
         break;  
      }  
   }  
}  
```  
  
 Para obtener más ejemplos, vea [instrucciones Update y DELETE posicionadas](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md) y [Actualizar filas en el conjunto de filas con SQLSetPos](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Realizar operaciones masivas que no están relacionadas con la posición del cursor de bloque|[Función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Obtener un solo campo de un descriptor|[Función SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Obtener varios campos de un descriptor|[Función SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Establecer un solo campo de un descriptor|[Función SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Establecer varios campos de un descriptor|[Función SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
|Establecer un atributo de instrucción|[Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
