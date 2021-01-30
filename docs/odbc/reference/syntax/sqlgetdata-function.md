---
description: Función SQLGetData
title: SQLGetData, función | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetData
helpviewer_keywords:
- SQLGetData function [ODBC]
ms.assetid: e3c1356a-5db7-4186-85fd-8b74633317e8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42b81d50a83fef0be7c13604902cfe67b2d2dd89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201970"
---
# <a name="sqlgetdata-function"></a>Función SQLGetData
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLGetData** recupera los datos de una sola columna en el conjunto de resultados o de un parámetro único después de que **SQLParamData** devuelva SQL_PARAM_DATA_AVAILABLE. Se puede llamar varias veces para recuperar datos de longitud variable en partes.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetData(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   Col_or_Param_Num,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *Col_or_Param_Num*  
 Entradas Para recuperar los datos de la columna, es el número de la columna para la que se van a devolver los datos. Las columnas del conjunto de resultados se numeran en el orden de las columnas creciente a partir de 1. La columna de marcador es el número de columna 0; solo se puede especificar si los marcadores están habilitados.  
  
 Para recuperar los datos de parámetro, es el ordinal del parámetro, que comienza en 1.  
  
 *TargetType*  
 Entradas El identificador de tipo del tipo de datos C del búfer **TargetValuePtr* . Para obtener una lista de tipos de datos e identificadores de tipo de C válidos, vea la sección tipos de datos de [c](../../../odbc/reference/appendixes/c-data-types.md) en el Apéndice D: tipos de datos.  
  
 Si *TargetType* es SQL_ARD_TYPE, el controlador usa el identificador de tipo especificado en el campo SQL_DESC_CONCISE_TYPE de ARD. Si *TargetType* es SQL_APD_TYPE, **SQLGetData** usará el mismo tipo de datos de C que se especificó en **SQLBindParameter**. De lo contrario, el tipo de datos C especificado en **SQLGetData** invalida el tipo de datos c especificado en **SQLBindParameter**. Si se SQL_C_DEFAULT, el controlador selecciona el tipo de datos C predeterminado según el tipo de datos SQL del origen.  
  
 También puede especificar un tipo de datos de C extendido. Para obtener más información, vea el tema sobre [tipos de datos C en ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 *TargetValuePtr*  
 Genere Puntero al búfer en el que se van a devolver los datos.  
  
 *TargetValuePtr* no puede ser null.  
  
 *BufferLength*  
 Entradas Longitud del búfer **TargetValuePtr* en bytes.  
  
 El controlador usa *BufferLength* para evitar escribir más allá del final del \* búfer *TargetValuePtr* al devolver datos de longitud variable, como datos de caracteres o binarios. Tenga en cuenta que el controlador cuenta el carácter de terminación null al devolver los datos de caracteres a \* *TargetValuePtr*. *Por lo tanto, *TargetValuePtr* debe contener espacio para el carácter de terminación null o el controlador truncará los datos.  
  
 Cuando el controlador devuelve datos de longitud fija, como un entero o una estructura de fecha, el controlador omite *BufferLength* y asume que el búfer es lo suficientemente grande como para contener los datos. Por lo tanto, es importante que la aplicación asigne un búfer suficientemente grande para los datos de longitud fija o que el controlador escriba más allá del final del búfer.  
  
 **SQLGetData** devuelve SQLSTATE HY090 (longitud de búfer o cadena no válida) cuando *BufferLength* es menor que 0 pero no cuando *BufferLength* es 0.  
  
 *StrLen_or_IndPtr*  
 Genere Puntero al búfer en el que se va a devolver el valor de longitud o indicador. Si se trata de un puntero nulo, no se devuelve ningún valor de longitud o indicador. Esto devuelve un error cuando los datos que se capturan son NULL.  
  
 **SQLGetData** puede devolver los siguientes valores en el búfer de longitud/indicador:  
  
-   La longitud de los datos disponibles para devolver  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 Para obtener más información, vea [usar valores de indicador/longitud](../../../odbc/reference/develop-app/using-length-and-indicator-values.md) y "Comentarios" en este tema.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetData** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLGetData** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|No todos los datos de la columna especificada, *Col_or_Param_Num*, se pueden recuperar en una única llamada a la función. SQL_NO_TOTAL o la longitud de los datos que quedan en la columna especificada antes de que se devuelva la llamada actual a **SQLGetData** en \* *StrLen_or_IndPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).<br /><br /> Para obtener más información sobre el uso de varias llamadas a **SQLGetData** para una sola columna, vea "Comentarios".|  
|01S07|Truncamiento fraccionario|Los datos devueltos para una o más columnas se han truncado. En el caso de los tipos de datos numéricos, se truncó la parte fraccionaria del número. En el caso de los tipos de datos Time, TIMESTAMP y Interval que contienen un componente de hora, se truncó la parte fraccionaria de la hora.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|El valor de datos de una columna en el conjunto de resultados no se puede convertir al tipo de datos C especificado por el argumento *TargetType*.|  
|07009|Índice de descriptor no válido|El valor especificado para el argumento *Col_or_Param_Num* era 0 y el atributo SQL_ATTR_USE_BOOKMARKS instrucción se estableció en SQL_UB_OFF.<br /><br /> El valor especificado para el argumento *Col_or_Param_Num* era mayor que el número de columnas del conjunto de resultados.<br /><br /> El valor *Col_or_Param_Num* no era igual al ordinal del parámetro que está disponible.<br /><br /> (DM) la columna especificada estaba enlazada. Esta descripción no se aplica a los controladores que devuelven la máscara de SQL_GD_BOUND para la opción SQL_GETDATA_EXTENSIONS de **SQLGetInfo**.<br /><br /> (DM) el número de la columna especificada era menor o igual que el número de la columna enlazada más alta. Esta descripción no se aplica a los controladores que devuelven la máscara de SQL_GD_ANY_COLUMN para la opción SQL_GETDATA_EXTENSIONS de **SQLGetInfo**.<br /><br /> (DM) la aplicación ya ha llamado **SQLGetData** para la fila actual. el número de la columna especificada en la llamada actual era menor que el número de la columna especificada en la llamada anterior; y el controlador no devuelve la máscara de SQL_GD_ANY_ORDER para la opción SQL_GETDATA_EXTENSIONS de **SQLGetInfo**.<br /><br /> (DM) el argumento *TargetType* se SQL_ARD_TYPE y el registro del descriptor *Col_or_Param_Num* de ARD no pudo realizar la comprobación de coherencia.<br /><br /> (DM) el argumento *TargetType* se SQL_ARD_TYPE y el valor del campo SQL_DESC_COUNT de ARD era menor que el argumento *Col_or_Param_Num* .|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|22002|Se requiere una variable de indicador pero no se ha proporcionado|*StrLen_or_IndPtr* era un puntero nulo y se recuperaron datos null.|  
|22003|Valor numérico fuera del intervalo|Si se devuelve el valor numérico (como numérico o cadena) de la columna, la parte entera (en oposición a fraccionaria) del número se trunca.<br /><br /> Para obtener más información, vea [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22007|Formato de fecha y hora no válido|La columna de caracteres del conjunto de resultados se enlazó a una estructura de fecha, hora o marca de tiempo de C, y el valor de la columna era una fecha, hora o marca de tiempo no válida, respectivamente. Para obtener más información, vea [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22012|División por cero|Se devolvió un valor de una expresión aritmética que dio como resultado una división por cero.|  
|22015|Desbordamiento de campo de intervalo|La asignación de un tipo numérico exacto o de intervalo SQL a un tipo de intervalo C provoca una pérdida de dígitos significativos en el campo inicial.<br /><br /> Al devolver datos a un tipo de intervalo C, no había ninguna representación del valor del tipo SQL en el tipo de intervalo C.|  
|22018|Valor de carácter no válido para la especificación de conversión|Se devolvió una columna de caracteres en el conjunto de resultados a un búfer de caracteres C y la columna contenía un carácter para el que no había ninguna representación en el juego de caracteres del búfer.<br /><br /> El tipo C era un tipo de datos numérico exacto o aproximado, un valor de fecha y hora o un intervalo. el tipo SQL de la columna era un tipo de datos de caracteres. y el valor de la columna no era un literal válido del tipo C enlazado.|  
|24000|Estado de cursor no válido|(DM) se llamó a la función sin llamar primero a **SQLFetch** o **SQLFetchScroll** para colocar el cursor en la fila de datos necesaria.<br /><br /> (DM) el *StatementHandle* estaba en un estado ejecutado, pero no hay ningún conjunto de resultados asociado a *StatementHandle*.<br /><br /> Un cursor estaba abierto en *StatementHandle* y se ha llamado a **SQLFetch** o **SQLFetchScroll** , pero el cursor se colocó antes del inicio del conjunto de resultados o después del final del conjunto de resultados.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY003|Tipo de programa fuera del intervalo|(DM) el argumento *TargetType* no era un tipo de datos válido, SQL_C_DEFAULT, SQL_ARD_TYPE (en el caso de la recuperación de datos de columna) o SQL_APD_TYPE (en el caso de la recuperación de datos de parámetros).<br /><br /> (DM) el argumento *Col_or_Param_Num* era 0 y el valor *TargetType* del argumento no se SQL_C_BOOKMARK para un marcador de longitud fija o SQL_C_VARBOOKMARK para un marcador de longitud variable.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle* y, a continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso y, a continuación, se llamó de nuevo a la función en *StatementHandle*.|  
|HY009|Uso no válido de puntero nulo|(DM) el argumento *TargetValuePtr* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) el *StatementHandle* especificado no se encontraba en un estado ejecutado. Se llamó a la función sin llamar primero a **SQLExecDirect**, **SQLExecute** o a una función de catálogo.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLGetData** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) el *StatementHandle* estaba en un estado ejecutado, pero no hay ningún conjunto de resultados asociado a *StatementHandle*.<br /><br /> Una llamada a **SQLExeceute**, **SQLExecDirect** o **SQLMoreResults** devolvió SQL_PARAM_DATA_AVAILABLE, pero se llamó a **SQLGetData** , en lugar de **SQLParamData**.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado para el argumento *BufferLength* era menor que 0.<br /><br /> El valor especificado para el argumento *BufferLength* era menor que 4, el argumento *Col_or_Param_Num* se estableció en 0 y el controlador era un controlador ODBC 2 *. x* .|  
|HY109|Posición del cursor no válida|El cursor estaba colocado (por **SQLSetPos**, **SQLFetch**, **SQLFetchScroll** o **SQLBulkOperations**) en una fila que se ha eliminado o no se ha podido recuperar.<br /><br /> El cursor era un cursor de solo avance y el tamaño del conjunto de filas era mayor que uno.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El controlador o el origen de datos no admite el uso de **SQLGetData** con varias filas en **SQLFetchScroll**. Esta descripción no se aplica a los controladores que devuelven la máscara de SQL_GD_BLOCK para la opción SQL_GETDATA_EXTENSIONS de **SQLGetInfo**.<br /><br /> El controlador o el origen de datos no admite la conversión especificada por la combinación del argumento *TargetType* y el tipo de datos SQL de la columna correspondiente. Este error solo se aplica cuando el tipo de datos SQL de la columna se ha asignado a un tipo de datos SQL específico del controlador.<br /><br /> El controlador solo admite ODBC 2 *. x* y el argumento *TargetType* era uno de los siguientes:<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> y cualquiera de los tipos de datos de Interval C enumerados en los [tipos de datos de c](../../../odbc/reference/appendixes/c-data-types.md) en los tipos de datos Apéndice D:.<br /><br /> El controlador solo es compatible con las versiones ODBC anteriores a 3,50 y el argumento *TargetType* se SQL_C_GUID.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador correspondiente a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 **SQLGetData** devuelve los datos de una columna especificada. Se puede llamar a **SQLGetData** solo después de que se hayan capturado una o varias filas del conjunto de resultados mediante **SQLFetch**, **SQLFetchScroll** o **SQLExtendedFetch**. Si los datos de longitud variable son demasiado grandes para que se devuelvan en una sola llamada a **SQLGetData** (debido a una limitación en la aplicación), **SQLGetData** puede recuperarlos en partes. Es posible enlazar algunas columnas de una fila y llamar a **SQLGetData** para otras, aunque esto está sujeto a algunas restricciones. Para obtener más información, vea [obtener datos de tipo Long](../../../odbc/reference/develop-app/getting-long-data.md).  
  
 Para obtener información sobre el uso de **SQLGetData** con parámetros de salida transmitidos, vea [recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="using-sqlgetdata"></a>Usar SQLGetData  
 Si el controlador no admite extensiones de **SQLGetData**, la función solo puede devolver datos de columnas sin enlazar con un número mayor que el de la última columna enlazada. Además, dentro de una fila de datos, el valor del argumento *Col_or_Param_Num* en cada llamada a **SQLGetData** debe ser mayor o igual que el valor de *Col_or_Param_Num* en la llamada anterior; es decir, los datos se deben recuperar en orden de número de columna creciente. Por último, si no se admiten extensiones, no se puede llamar a **SQLGetData** si el tamaño del conjunto de filas es mayor que 1.  
  
 Los controladores pueden relajar cualquiera de estas restricciones. Para determinar qué restricciones relaja un controlador, una aplicación llama a **SQLGetInfo** con cualquiera de las siguientes opciones de SQL_GETDATA_EXTENSIONS:  
  
-   SQL_GD_OUTPUT_PARAMS = **SQLGetData** se puede llamar para devolver valores de parámetros de salida. Para obtener más información, vea el tema que trata sobre [recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
-   SQL_GD_ANY_COLUMN. Si se devuelve esta opción, se puede llamar a **SQLGetData** para cualquier columna sin enlazar, incluidas las anteriores a la última columna enlazada.  
  
-   SQL_GD_ANY_ORDER. Si se devuelve esta opción, se puede llamar a **SQLGetData** para las columnas sin enlazar en cualquier orden.  
  
-   SQL_GD_BLOCK. Si **SQLGetInfo** devuelve esta opción para el SQL_GETDATA_EXTENSIONS InfoType, el controlador admite llamadas a **SQLGetData** cuando el tamaño del conjunto de filas es mayor que 1 y la aplicación puede llamar a **SQLSetPos** con la opción SQL_POSITION para colocar el cursor en la fila correcta antes de llamar a **SQLGetData.**  
  
-   SQL_GD_BOUND. Si se devuelve esta opción, se puede llamar a **SQLGetData** para las columnas enlazadas, así como para las columnas sin enlazar.  
  
 Existen dos excepciones a estas restricciones y la capacidad de un controlador de relajarlas. En primer lugar, no se debe llamar nunca a **SQLGetData** para un cursor de solo avance cuando el tamaño del conjunto de filas es mayor que 1. En segundo lugar, si un controlador admite marcadores, siempre debe admitir la capacidad de llamar a **SQLGetData** para la columna 0, incluso si no permite que las aplicaciones llamen a **SQLGetData** para otras columnas antes de la última columna enlazada. (Cuando una aplicación está trabajando con ODBC 2 *. controlador x* , **SQLGetData** devolverá correctamente un marcador cuando se le llame con *Col_or_Param_Num* igual a 0 después de una llamada a **SQLFETCH**, porque el administrador de controladores ODBC 3 *. x* asigna **SQLFetch** a **SQLExtendedFetch** con un *FetchOrientation* de SQL_FETCH_NEXT y **SQLGetData** con un *Col_or_Param_Num* de 0 se asigna mediante el administrador de controladores ODBC 3.*x* a **SQLGetStmtOption** con un *fOption* de SQL_GET_BOOKMARK).  
  
 No se puede utilizar **SQLGetData** para recuperar el marcador de una fila que se acaba de insertar llamando a **SQLBulkOperations** con la opción SQL_ADD, porque el cursor no está situado en la fila. Una aplicación puede recuperar el marcador para dicha fila enlazando la columna 0 antes de llamar a **SQLBulkOperations** con SQL_ADD, en cuyo caso **SQLBulkOperations** devuelve el marcador en el búfer enlazado. A continuación, se puede llamar a **SQLFetchScroll** con SQL_FETCH_BOOKMARK para cambiar la posición del cursor en esa fila.  
  
 Si el argumento *TargetType* es un tipo de datos de intervalo, se usan para los datos la precisión inicial del intervalo predeterminado (2) y la precisión de segundos del intervalo predeterminado (6), tal como se establece en los campos SQL_DESC_DATETIME_INTERVAL_PRECISION y SQL_DESC_PRECISION de ARD, respectivamente. Si el argumento *TargetType* es un tipo de datos SQL_C_NUMERIC, la precisión predeterminada (definida por el controlador) y la escala predeterminada (0), como se establece en los campos SQL_DESC_PRECISION y SQL_DESC_SCALE de ARD, se usan para los datos. Si una precisión o escala predeterminada no es adecuada, la aplicación debe establecer explícitamente el campo de descriptor adecuado mediante una llamada a **SQLSetDescField** o **SQLSetDescRec**. Puede establecer el campo SQL_DESC_CONCISE_TYPE en SQL_C_NUMERIC y llamar a **SQLGetData** con un argumento *TargetType* de SQL_ARD_TYPE, lo que hará que se usen los valores de precisión y escala de los campos de descriptor.  
  
> [!NOTE]
>  En ODBC 2 *. x*, las aplicaciones establecen *TargetType* en SQL_C_DATE, SQL_C_TIME o SQL_C_TIMESTAMP para indicar que \* *TargetValuePtr* es una estructura de fecha, hora o marca de tiempo. En ODBC 3 *. x*, las aplicaciones establecen *TargetType* en SQL_C_TYPE_DATE, SQL_C_TYPE_TIME o SQL_C_TYPE_TIMESTAMP. El administrador de controladores realiza las asignaciones adecuadas, si es necesario, en función de la versión de la aplicación y del controlador.  
  
## <a name="retrieving-variable-length-data-in-parts"></a>Recuperar datos de Variable-Length en partes  
 **SQLGetData** se puede utilizar para recuperar datos de una columna que contiene datos de longitud variable en partes; es decir, cuando el identificador del tipo de datos SQL de la columna es SQL_CHAR, SQL_VARCHAR, SQL_LONGVARCHAR, SQL_WCHAR, SQL_WVARCHAR, SQL_WLONGVARCHAR, SQL_BINARY, SQL_VARBINARY, SQL_LONGVARBINARY o un identificador específico del controlador para un tipo de longitud variable.  
  
 Para recuperar datos de una columna de elementos, la aplicación llama a **SQLGetData** varias veces consecutivas para la misma columna. En cada llamada, **SQLGetData** devuelve la siguiente parte de los datos. Depende de la aplicación volver a ensamblar los elementos, teniendo cuidado de quitar el carácter de terminación null de partes intermedias de datos de caracteres. Si hay más datos para devolver o no hay suficiente búfer asignado para el carácter de terminación, **SQLGetData** devuelve SQL_SUCCESS_WITH_INFO y SQLSTATE 01004 (datos truncados). Cuando devuelve la última parte de los datos, **SQLGetData** devuelve SQL_SUCCESS. No se puede devolver ni SQL_NO_TOTAL ni cero en la última llamada válida para recuperar datos de una columna, ya que la aplicación no tendría forma de saber cuántos de los datos del búfer de aplicación es válido. Si se llama a **SQLGetData** después de esto, devuelve SQL_NO_DATA. Para obtener más información, vea la sección siguiente, "recuperar datos con SQLGetData".  
  
 Los marcadores de longitud variable se pueden devolver en las partes de **SQLGetData**. Al igual que con otros datos, una llamada a **SQLGetData** para devolver marcadores de longitud variable en elementos devolverá SQLSTATE 01004 (datos de cadena, truncados a la derecha) y SQL_SUCCESS_WITH_INFO cuando haya más datos que se van a devolver. Esto es diferente de lo que ocurre cuando un marcador de longitud variable se trunca mediante una llamada a **SQLFetch** o **SQLFetchScroll**, que devuelve SQL_ERROR y SQLSTATE 22001 (datos de cadena, truncados a la derecha).  
  
 **SQLGetData** no se puede usar para devolver datos de longitud fija en partes. Si se llama a **SQLGetData** más de una vez en una fila para una columna que contiene datos de longitud fija, devuelve SQL_NO_DATA para todas las llamadas posteriores a la primera.  
  
## <a name="retrieving-streamed-output-parameters"></a>Recuperación de parámetros de salida transmitidos  
 Si un controlador admite parámetros de salida transmitidos por secuencias, una aplicación puede llamar a **SQLGetData** con un búfer pequeño muchas veces para recuperar un valor de parámetro grande. Para obtener más información sobre el parámetro de salida transmitido, vea [recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="retrieving-data-with-sqlgetdata"></a>Recuperación de datos con SQLGetData  
 Para devolver los datos de la columna especificada, **SQLGetData** realiza la siguiente secuencia de pasos:  
  
1.  Devuelve SQL_NO_DATA si ya ha devuelto todos los datos de la columna.  
  
2.  Establece \* *StrLen_or_IndPtr* en SQL_NULL_DATA si los datos son NULL. Si los datos son NULL y *StrLen_or_IndPtr* era un puntero nulo, **SQLGetData** devuelve SQLSTATE 22002 (la variable de indicador es necesaria pero no se ha proporcionado).  
  
     Si los datos de la columna no son NULL, **SQLGetData** continúa en el paso 3.  
  
3.  Si el atributo de instrucción SQL_ATTR_MAX_LENGTH está establecido en un valor distinto de cero, si la columna contiene datos binarios o de caracteres, y si no se ha llamado a **SQLGetData** previamente para la columna, los datos se truncan en SQL_ATTR_MAX_LENGTH bytes.  
  
    > [!NOTE]  
    >  El atributo de instrucción SQL_ATTR_MAX_LENGTH está diseñado para reducir el tráfico de red. Normalmente, se implementa mediante el origen de datos, que trunca los datos antes de devolverlos a través de la red. No es necesario que los controladores y los orígenes de datos sean compatibles. Por lo tanto, para garantizar que los datos se truncan a un tamaño determinado, una aplicación debe asignar un búfer de ese tamaño y especificar el tamaño en el argumento *BufferLength* .  
  
4.  Convierte los datos al tipo especificado en *TargetType*. A los datos se les asigna la precisión y la escala predeterminadas para ese tipo de datos. Si *TargetType* es SQL_ARD_TYPE, se usa el tipo de datos del campo SQL_DESC_CONCISE_TYPE de ARD. Si *TargetType* es SQL_ARD_TYPE, los datos reciben la precisión y la escala en los campos SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_PRECISION y SQL_DESC_SCALE de ARD, en función del tipo de datos del campo SQL_DESC_CONCISE_TYPE. Si una precisión o escala predeterminada no es adecuada, la aplicación debe establecer explícitamente el campo de descriptor adecuado mediante una llamada a **SQLSetDescField** o **SQLSetDescRec**.  
  
5.  Si los datos se han convertido en un tipo de datos de longitud variable, como carácter o binario, **SQLGetData** comprueba si la longitud de los datos supera *BufferLength*. Si la longitud de los datos de caracteres (incluido el carácter de terminación null) supera el valor de *BufferLength*, **SQLGetData** trunca los datos para *BufferLength* menos la longitud de un carácter de terminación null. Después, termina en NULL los datos. Si la longitud de los datos binarios supera la longitud del búfer de datos, **SQLGetData** lo trunca en *BufferLength* bytes.  
  
     Si el búfer de datos proporcionado es demasiado pequeño para contener el carácter de terminación null, **SQLGetData** devuelve SQL_SUCCESS_WITH_INFO y SQLSTATE 01004.  
  
     **SQLGetData** nunca trunca los datos convertidos en tipos de datos de longitud fija; siempre supone que la longitud de **TargetValuePtr* es el tamaño del tipo de datos.  
  
6.  Coloca los datos convertidos (y posiblemente truncados) en \* *TargetValuePtr*. Tenga en cuenta que **SQLGetData** no puede devolver datos fuera de línea.  
  
7.  Coloca la longitud de los datos en \* *StrLen_or_IndPtr*. Si *StrLen_or_IndPtr* era un puntero nulo, **SQLGetData** no devuelve la longitud.  
  
    -   En el caso de los datos de caracteres o binarios, es la longitud de los datos después de la conversión y antes del truncamiento debido a *BufferLength*. Si el controlador no puede determinar la longitud de los datos después de la conversión, como en ocasiones es el caso con datos largos, devuelve SQL_SUCCESS_WITH_INFO y establece la longitud en SQL_NO_TOTAL. (La última llamada a **SQLGetData** siempre debe devolver la longitud de los datos, no cero ni SQL_NO_TOTAL). Si los datos se truncaron debido al atributo de instrucción SQL_ATTR_MAX_LENGTH, el valor de este atributo, en contraposición a la longitud real, se coloca en \* *StrLen_or_IndPtr*. Esto se debe a que este atributo está diseñado para truncar los datos en el servidor antes de la conversión, por lo que el controlador no tiene forma de averiguar cuál es la longitud real. Cuando se llama a **SQLGetData** varias veces consecutivamente para la misma columna, se trata de la longitud de los datos disponibles al inicio de la llamada actual. es decir, la longitud disminuye con cada llamada subsiguiente.  
  
    -   Para el resto de tipos de datos, es la longitud de los datos después de la conversión. es decir, es el tamaño del tipo al que se han convertido los datos.  
  
8.  Si los datos se truncan sin pérdida de importancia durante la conversión (por ejemplo, el número real 1,234 se trunca cuando se convierte en el entero 1) o porque *BufferLength* es demasiado pequeño (por ejemplo, la cadena "abcdef" se coloca en un búfer de 4 bytes), **SQLGetData** devuelve SQLSTATE 01004 (datos truncados) y SQL_SUCCESS_WITH_INFO. Si los datos se truncan sin pérdida de importancia debido al atributo de instrucción SQL_ATTR_MAX_LENGTH, **SQLGetData** devuelve SQL_SUCCESS y no devuelve SQLSTATE 01004 (datos truncados).  
  
 El contenido del búfer de datos enlazado (si se llama a **SQLGetData** en una columna enlazada) y el búfer de longitud/indicador no se definen si **sqlgetdata** no devuelve SQL_SUCCESS o SQL_SUCCESS_WITH_INFO.  
  
 Las llamadas sucesivas a **SQLGetData** recuperarán datos de la última columna solicitada; los desplazamientos anteriores dejan de ser válidos. Por ejemplo, cuando se realiza la siguiente secuencia:  
  
```cpp  
SQLGetData(icol=n), SQLGetData(icol=m), SQLGetData(icol=n)  
```  
  
 la segunda llamada a SQLGetData (ICOL = n) recupera datos del inicio de la columna n. Cualquier desplazamiento en los datos debido a llamadas anteriores a **SQLGetData** para la columna ya no es válido.  
  
## <a name="descriptors-and-sqlgetdata"></a>Descriptores y SQLGetData  
 **SQLGetData** no interactúa directamente con ningún campo de descriptor.  
  
 Si *TargetType* es SQL_ARD_TYPE, se usa el tipo de datos del campo SQL_DESC_CONCISE_TYPE de ARD. Si *TargetType* es SQL_ARD_TYPE o SQL_C_DEFAULT, los datos reciben la precisión y la escala en los campos SQL_DESC_DATETIME_INTERVAL_PRECISION, SQL_DESC_PRECISION y SQL_DESC_SCALE de ARD, dependiendo del tipo de datos del campo SQL_DESC_CONCISE_TYPE.  
  
## <a name="code-example"></a>Ejemplo de código  
 En el ejemplo siguiente, una aplicación ejecuta una instrucción **Select** para devolver un conjunto de resultados de los identificadores de cliente, los nombres y los números de teléfono ordenados por nombre, identificador y número de teléfono. Para cada fila de datos, llama a **SQLFetch** para colocar el cursor en la fila siguiente. Llama a **SQLGetData** para recuperar los datos capturados; los búferes de los datos y el número de bytes devuelto se especifican en la llamada a **SQLGetData**. Por último, imprime el nombre, el identificador y el número de teléfono de cada empleado.  
  
```cpp  
#define NAME_LEN 50  
#define PHONE_LEN 50  
  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   sCustID, cbName, cbAge, cbBirthday;  
SQLRETURN    retcode;  
SQLHSTMT     hstmt;  
  
retcode = SQLExecDirect(hstmt,  
   "SELECT CUSTID, NAME, PHONE FROM CUSTOMERS ORDER BY 2, 1, 3",  
   SQL_NTS);  
  
if (retcode == SQL_SUCCESS) {  
   while (TRUE) {  
      retcode = SQLFetch(hstmt);  
      if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO) {  
         show_error();  
      }  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
         /* Get data for columns 1, 2, and 3 */  
  
         SQLGetData(hstmt, 1, SQL_C_ULONG, &sCustID, 0, &cbCustID);  
         SQLGetData(hstmt, 2, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
         SQLGetData(hstmt, 3, SQL_C_CHAR, szPhone, PHONE_LEN,  
            &cbPhone);  
  
         /* Print the row of data */  
  
         fprintf(out, "%-5d %-*s %*s", sCustID, NAME_LEN-1, szName,   
            PHONE_LEN-1, szPhone);  
      } else {  
         break;  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de almacenamiento para una columna en un conjunto de resultados|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Realizar operaciones masivas que no están relacionadas con la posición del cursor de bloque|[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Cancelar el procesamiento de instrucciones|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Ejecutar una instrucción SQL|[SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Capturar una sola fila de datos o un bloque de datos en una dirección de solo avance|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Enviar datos de parámetros en tiempo de ejecución|[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Colocar el cursor, actualizar los datos del conjunto de filas o actualizar o eliminar los datos del conjunto de filas|[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
