---
description: Función SQLFetchScroll
title: Función SQLFetchScroll | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFetchScroll
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFetchScroll
helpviewer_keywords:
- SQLFetchScroll function [ODBC]
ms.assetid: c0243667-428c-4dda-ae91-3c307616a1ac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cfa442ab1ada0b17e06137257b4d17bf37f854c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198542"
---
# <a name="sqlfetchscroll-function"></a>Función SQLFetchScroll
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLFetchScroll** recupera el conjunto de filas especificado de los datos del conjunto de resultados y devuelve los datos de todas las columnas enlazadas. Los conjuntos de filas se pueden especificar en una posición absoluta o relativa o por marcador.  
  
 Cuando se trabaja con un controlador ODBC 2. x, el administrador de controladores asigna esta función a **SQLExtendedFetch**. Para obtener más información, consulte [asignación de funciones de reemplazo para la compatibilidad con versiones anteriores de las aplicaciones](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLFetchScroll(  
      SQLHSTMT      StatementHandle,  
      SQLSMALLINT   FetchOrientation,  
      SQLLEN        FetchOffset);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *FetchOrientation*  
 Entradas  
  
 Tipo de captura:  
  
 SQL_FETCH_NEXT  
  
 SQL_FETCH_PRIOR  
  
 SQL_FETCH_FIRST  
  
 SQL_FETCH_LAST  
  
 SQL_FETCH_ABSOLUTE  
  
 SQL_FETCH_RELATIVE  
  
 SQL_FETCH_BOOKMARK  
  
 Para obtener más información, vea "colocar el cursor" en la sección "Comentarios".  
  
 *FetchOffset*  
 Entradas  
  
 Número de la fila que se va a capturar. La interpretación de este argumento depende del valor del argumento *FetchOrientation* . Para obtener más información, vea "colocar el cursor" en la sección "Comentarios".  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLFetchScroll** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un HandleType de SQL_HANDLE_STMT y un identificador de StatementHandle. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLFetchScroll** y se explica cada uno en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario. Si se produce un error en una sola columna, se puede llamar a **SQLGetDiagField** con un DiagIdentifier de SQL_DIAG_COLUMN_NUMBER para determinar la columna en la que se produjo el error; y se puede llamar a **SQLGetDiagField** con un DiagIdentifier de SQL_DIAG_ROW_NUMBER para determinar la fila que contiene esa columna.  
  
 En el caso de todos los SQLSTATEs que pueden devolver SQL_SUCCESS_WITH_INFO o SQL_ERROR (excepto 01xxx SQLSTATEs), se devuelve SQL_SUCCESS_WITH_INFO si se produce un error en una o varias filas de una operación de MultiRow y se devuelve SQL_ERROR si se produce un error en una operación de una sola fila.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|Los datos binarios o de cadena devueltos para una columna dieron como resultado el truncamiento de datos binarios de caracteres que no están en blanco o no NULOs. Si fuera un valor de cadena, se truncó a la derecha.|  
|01S01|Error en la fila|Se produjo un error al capturar una o más filas.<br /><br /> (Si se devuelve este SQLSTATE cuando una aplicación ODBC 3 *. x* está trabajando con un controlador ODBC 2 *. x* , se puede pasar por alto).|  
|01S06|Intento de captura antes de que el conjunto de resultados devolviera el primer conjunto de filas|El conjunto de filas solicitado se superpone al inicio del conjunto de resultados cuando se SQL_FETCH_PRIOR FetchOrientation, la posición actual estaba más allá de la primera fila y el número de la fila actual es menor o igual que el tamaño del conjunto de filas.<br /><br /> El conjunto de filas solicitado se superpone al inicio del conjunto de resultados cuando se SQL_FETCH_PRIOR FetchOrientation, la posición actual estaba más allá del final del conjunto de resultados y el tamaño del conjunto de filas era mayor que el tamaño del conjunto de resultados.<br /><br /> El conjunto de filas solicitado se superpone al inicio del conjunto de resultados cuando se SQL_FETCH_RELATIVE FetchOrientation, FetchOffset era negativo y el valor absoluto de FetchOffset era menor o igual que el tamaño del conjunto de filas.<br /><br /> El conjunto de filas solicitado se superpone al inicio del conjunto de resultados cuando se SQL_FETCH_ABSOLUTE FetchOrientation, FetchOffset era negativo y el valor absoluto de FetchOffset era mayor que el tamaño del conjunto de resultados, pero menor o igual que el tamaño del conjunto de filas.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S07|Truncamiento fraccionario|Los datos devueltos para una columna se han truncado. En el caso de los tipos de datos numéricos, se truncó la parte fraccionaria del número. En el caso de los tipos de datos Time, TIMESTAMP y Interval que contienen un componente de hora, se truncó la parte fraccionaria de la hora.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|No se pudo convertir el valor de datos de una columna del conjunto de resultados al tipo de datos especificado por *TargetType* en **SQLBindCol**.<br /><br /> La columna 0 se ha enlazado con un tipo de datos de SQL_C_BOOKMARK y el atributo SQL_ATTR_USE_BOOKMARKS instrucción se ha establecido en SQL_UB_VARIABLE.<br /><br /> La columna 0 se ha enlazado con un tipo de datos de SQL_C_VARBOOKMARK y el atributo SQL_ATTR_USE_BOOKMARKS instrucción no se ha establecido en SQL_UB_VARIABLE.|  
|07009|Índice de descriptor no válido|El controlador era un controlador ODBC 2 *. x* que no admite **SQLExtendedFetch**, y un número de columna especificado en el enlace para una columna era 0.<br /><br /> La columna 0 estaba enlazada y el atributo SQL_ATTR_USE_BOOKMARKS instrucción se estableció en SQL_UB_OFF.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|22001|Datos de cadena, truncados a la derecha|Un marcador de longitud variable devuelto para una columna se truncó.|  
|22002|Se requiere una variable de indicador pero no se ha proporcionado|Se han capturado datos NULL en una columna cuya *StrLen_or_IndPtr* establecida por **SQLBindCol** (o SQL_DESC_INDICATOR_PTR establecida por **SQLSetDescField** o **SQLSetDescRec**) era un puntero nulo.|  
|22003|Valor numérico fuera del intervalo|Si se devuelve el valor numérico (como Numeric o String) para una o varias columnas enlazadas, la parte entera (en oposición a fraccionaria) del número se trunca.<br /><br /> Para obtener más información, vea [convertir datos de SQL a tipos de datos de C](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) en el [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md).|  
|22007|Formato de fecha y hora no válido|Una columna de caracteres del conjunto de resultados estaba enlazada a una estructura de fecha, hora o marca de tiempo de C, y un valor de la columna era, respectivamente, una fecha, una hora o una marca de tiempo no válidas.|  
|22012|División por cero|Se devolvió un valor de una expresión aritmética, lo que da como resultado una división por cero.|  
|22015|Desbordamiento de campo de intervalo|La asignación de un tipo numérico exacto o de intervalo SQL a un tipo de intervalo C provoca una pérdida de dígitos significativos en el campo inicial.<br /><br /> Al capturar datos en un tipo de intervalo C, no había ninguna representación del valor del tipo SQL en el tipo de intervalo C.|  
|22018|Valor de carácter no válido para la especificación de conversión|Una columna de caracteres del conjunto de resultados estaba enlazada a un búfer de caracteres C y la columna contenía un carácter para el que no había ninguna representación en el juego de caracteres del búfer.<br /><br /> El tipo C era un tipo de datos numérico exacto o aproximado, un valor de fecha y hora o un intervalo. el tipo SQL de la columna era un tipo de datos de caracteres. y el valor de la columna no era un literal válido del tipo C enlazado.|  
|24000|Estado de cursor no válido|El *StatementHandle* estaba en un estado ejecutado pero no se asoció ningún conjunto de resultados con el *StatementHandle*.|  
|40001|Error de serialización|La transacción en la que se ejecutó la captura ha finalizado para evitar el interbloqueo.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLFetchScroll** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) el *StatementHandle* especificado no se encontraba en un estado ejecutado. Se llamó a la función sin llamar primero a **SQLExecDirect**, **SQLExecute** o a una función de catálogo.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> Se llamó a **SQLFetch** (DM) para *StatementHandle* después de llamar a **SQLExtendedFetch** y antes de que se llamara a **SQLFreeStmt** con la opción SQL_CLOSE.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|El atributo de instrucción SQL_ATTR_USE_BOOKMARK se estableció en SQL_UB_VARIABLE y la columna 0 estaba enlazada a un búfer cuya longitud no es igual a la longitud máxima para el marcador de este conjunto de resultados. (Esta longitud está disponible en el campo SQL_DESC_OCTET_LENGTH de IRD y se puede obtener llamando a **SQLDescribeCol**, **SQLColAttribute** o **SQLGetDescField**).|  
|HY106|Tipo de captura fuera del intervalo|DM) el valor especificado para el argumento FetchOrientation no era válido.<br /><br /> (DM) el argumento FetchOrientation se SQL_FETCH_BOOKMARK y el atributo SQL_ATTR_USE_BOOKMARKS instrucción se estableció en SQL_UB_OFF.<br /><br /> Se SQL_CURSOR_FORWARD_ONLY el valor del atributo de instrucción SQL_ATTR_CURSOR_TYPE y no se SQL_FETCH_NEXT el valor del argumento FetchOrientation.<br /><br /> Se SQL_NONSCROLLABLE el valor del atributo de instrucción SQL_ATTR_CURSOR_SCROLLABLE y no se SQL_FETCH_NEXT el valor del argumento FetchOrientation.|  
|HY107|Valor de fila fuera del intervalo|Se SQL_CURSOR_KEYSET_DRIVEN el valor especificado con el atributo de instrucción SQL_ATTR_CURSOR_TYPE, pero el valor especificado con el atributo de instrucción SQL_ATTR_KEYSET_SIZE era mayor que 0 y menor que el valor especificado con el atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE.|  
|HY111|Valor de marcador no válido|Se SQL_FETCH_BOOKMARK el argumento FetchOrientation y el marcador al que apunta el valor en el atributo de la instrucción SQL_ATTR_FETCH_BOOKMARK_PTR no era válido o era un puntero nulo.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El controlador o el origen de datos no admite la conversión especificada por la combinación de *TargetType* en **SQLBindCol** y el tipo de datos SQL de la columna correspondiente.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados solicitado. El período de tiempo de espera se establece a través de SQLSetStmtAttr, SQL_ATTR_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 **SQLFetchScroll** devuelve un conjunto de filas especificado del conjunto de resultados. Los conjuntos de filas se pueden especificar por posición absoluta o relativa o por marcador. Se puede llamar a **SQLFetchScroll** solo mientras exista un conjunto de resultados, es decir, después de una llamada que crea un conjunto de resultados y antes de que se cierre el cursor sobre dicho conjunto de resultados. Si alguna columna está enlazada, devuelve los datos de esas columnas. Si la aplicación ha especificado un puntero a una matriz de estado de fila o a un búfer en el que se va a devolver el número de filas recuperadas, **SQLFetchScroll** también devuelve esta información. Las llamadas a **SQLFetchScroll** se pueden mezclar con llamadas a **SQLFetch** pero no se pueden mezclar con llamadas a **SQLExtendedFetch**.  
  
 Para obtener más información, vea [uso de cursores de bloque](../../../odbc/reference/develop-app/using-block-cursors.md) y uso de [cursores desplazables](../../../odbc/reference/develop-app/using-scrollable-cursors.md).  
  
## <a name="positioning-the-cursor"></a>Colocar el cursor  
 Cuando se crea el conjunto de resultados, el cursor se coloca antes del inicio del conjunto de resultados. **SQLFetchScroll** coloca el cursor de bloque en función de los valores de los argumentos *FetchOrientation* y *FetchOffset* como se muestra en la tabla siguiente. En la sección siguiente se muestran las reglas exactas para determinar el inicio del nuevo conjunto de filas.  
  
|FetchOrientation|Significado|  
|----------------------|-------------|  
|SQL_FETCH_NEXT|Devuelve el conjunto de filas siguiente. Esto es equivalente a llamar a **SQLFetch**.<br /><br /> **SQLFetchScroll** omite el valor de *FetchOffset*.|  
|SQL_FETCH_PRIOR|Devuelve el conjunto de filas anterior.<br /><br /> **SQLFetchScroll** omite el valor de *FetchOffset*.|  
|SQL_FETCH_RELATIVE|Devuelve el conjunto de filas *FetchOffset* desde el principio del conjunto de filas actual.|  
|SQL_FETCH_ABSOLUTE|Devuelve el conjunto de filas a partir de la fila *FetchOffset*.|  
|SQL_FETCH_FIRST|Devuelve el primer conjunto de filas del conjunto de resultados.<br /><br /> **SQLFetchScroll** omite el valor de *FetchOffset*.|  
|SQL_FETCH_LAST|Devuelve el último conjunto de filas completo del conjunto de resultados.<br /><br /> **SQLFetchScroll** omite el valor de *FetchOffset*.|  
|SQL_FETCH_BOOKMARK|Devuelve las filas del conjunto de filas FetchOffset del marcador especificado por el atributo de instrucción SQL_ATTR_FETCH_BOOKMARK_PTR.|  
  
 No es necesario que los controladores admitan todas las orientaciones de Fetch; una aplicación llama a **SQLGetInfo** con un tipo de información de SQL_DYNAMIC_CURSOR_ATTRIBUTES1, SQL_KEYSET_CURSOR_ATTRIBUTES1 o SQL_STATIC_CURSOR_ATTRIBUTES1 (según el tipo de cursor) para determinar qué orientaciones de captura son compatibles con el controlador. La aplicación debe examinar las máscaras de SQL_CA1_NEXT, SQL_CA1_RELATIVE, SQL_CA1_ABSOLUTE y WQL_CA1_BOOKMARK en estos tipos de información. Además, si el cursor es de solo avance y FetchOrientation no está SQL_FETCH_NEXT, **SQLFetchScroll** devuelve SQLSTATE HY106 (tipo de captura fuera del intervalo).  
  
 El atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE especifica el número de filas del conjunto de filas. Si **sqlfetchscroll** se superpone al final del conjunto de resultados, **sqlfetchscroll** devuelve un conjunto de filas parcial. Es decir, si S + R-1 es mayor que L, donde S es la fila inicial del conjunto de filas que se va a capturar, R es el tamaño del conjunto de filas y L es la última fila del conjunto de resultados, solo las primeras L-S + 1 filas del conjunto de filas son válidas. Las filas restantes están vacías y tienen el estado SQL_ROW_NOROW.  
  
 Después de que **SQLFetchScroll** vuelva, la fila actual es la primera fila del conjunto de filas.  
  
## <a name="cursor-positioning-rules"></a>Reglas de posición del cursor  
 En las secciones siguientes se describen las reglas exactas para cada valor de FetchOrientation. Estas reglas usan la notación siguiente.  
  
|Notación|Significado|  
|--------------|-------------|  
|*Antes del inicio*|El cursor de bloque se coloca antes del inicio del conjunto de resultados. Si la primera fila del conjunto de filas nuevo es anterior al inicio del conjunto de resultados, **SQLFetchScroll** devuelve SQL_NO_DATA.|  
|*Después del final*|El cursor de bloque se coloca después del final del conjunto de resultados. Si la primera fila del conjunto de filas nuevo es posterior al final del conjunto de resultados, **SQLFetchScroll** devuelve SQL_NO_DATA.|  
|*CurrRowsetStart*|Número de la primera fila del conjunto de filas actual.|  
|*LastResultRow*|Número de la última fila del conjunto de resultados.|  
|*RowsetSize*|Tamaño del conjunto de filas.|  
|*FetchOffset*|Valor del argumento *FetchOffset* .|  
|*BookmarkRow*|Fila correspondiente al marcador especificado por el atributo de instrucción SQL_ATTR_FETCH_BOOKMARK_PTR.|  
  
## <a name="sql_fetch_next"></a>SQL_FETCH_NEXT  
 Se aplican las siguientes reglas.  
  
|Condición|Primera fila del conjunto de filas nuevo|  
|---------------|-----------------------------|  
|*Antes del inicio*|1|  
|*CurrRowsetStart + RowsetSize*[1] *\< = LastResultRow*|*CurrRowsetStart + RowsetSize*[1]|  
|*CurrRowsetStart + RowsetSize*[1]*> LastResultRow*|*Después del final*|  
|*Después del final*|*Después del final*|  
  
 [1] si se ha cambiado el tamaño del conjunto de filas desde la llamada anterior para capturar filas, este es el tamaño del conjunto de filas que se usó con la llamada anterior.  
  
## <a name="sql_fetch_prior"></a>SQL_FETCH_PRIOR  
 Se aplican las siguientes reglas.  
  
|Condición|Primera fila del conjunto de filas nuevo|  
|---------------|-----------------------------|  
|*Antes del inicio*|*Antes del inicio*|  
|*CurrRowsetStart = 1*|*Antes del inicio*|  
|*1 < CurrRowsetStart <= RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*CurrRowsetStart > RowsetSize* <sup>[2]</sup>|*CurrRowsetStart-RowsetSize* <sup>[2]</sup>|  
|*Después de end y LastResultRow < RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*Después de end y LastResultRow >= RowsetSize* <sup>[2]</sup>|*LastResultRow-RowsetSize + 1* <sup>[2]</sup>|  
  
 [1]   **SQLFetchScroll** devuelve SQLSTATE 01S06 (intento de captura antes de que el conjunto de resultados devolviera el primer conjunto de filas) y SQL_SUCCESS_WITH_INFO.  
  
 [2] si se ha cambiado el tamaño del conjunto de filas desde la llamada anterior para capturar filas, este es el nuevo tamaño del conjunto de filas.  
  
## <a name="sql_fetch_relative"></a>SQL_FETCH_RELATIVE  
 Se aplican las siguientes reglas.  
  
|Condición|Primera fila del conjunto de filas nuevo|  
|---------------|-----------------------------|  
|*(Antes de Start y FetchOffset > 0) O (después de end y FetchOffset < 0)*|*--*<sup>[1]</sup>|  
|*BeforeStart y FetchOffset <= 0*|*Antes del inicio*|  
|*CurrRowsetStart = 1 y FetchOffset < 0*|*Antes del inicio*|  
|*CurrRowsetStart > 1 y CurrRowsetStart + FetchOffset < 1 y &#124; FetchOffset &#124; > RowsetSize* <sup>[3]</sup>|*Antes del inicio*|  
|*CurrRowsetStart > 1 y CurrRowsetStart + FetchOffset < 1 y &#124; FetchOffset &#124; <= RowsetSize* <sup>[3]</sup>|*1* <sup>[2]</sup>|  
|*1 <= CurrRowsetStart + FetchOffset \< = LastResultRow*|*CurrRowsetStart + FetchOffset*|  
|*CurrRowsetStart + FetchOffset > LastResultRow*|*Después del final*|  
|*Después de end y FetchOffset >= 0*|*Después del final*|  
  
 [1] ***SQLFetchScroll** _ devuelve el mismo conjunto de filas que si se llamara con FetchOrientation establecido en SQL_FETCH_ABSOLUTE. Para obtener más información, vea la sección "SQL_FETCH_ABSOLUTE".  
  
 [2] _ *SQLFetchScroll** devuelve SQLSTATE 01S06 (intento de captura antes de que el conjunto de resultados devolviera el primer conjunto de filas) y SQL_SUCCESS_WITH_INFO.  
  
 [3] si se ha cambiado el tamaño del conjunto de filas desde la llamada anterior para capturar filas, este es el nuevo tamaño del conjunto de filas.  
  
## <a name="sql_fetch_absolute"></a>SQL_FETCH_ABSOLUTE  
 Se aplican las siguientes reglas.  
  
|Condición|Primera fila del conjunto de filas nuevo|  
|---------------|-----------------------------|  
|*FetchOffset < 0 y &#124; FetchOffset &#124; <= LastResultRow*|*LastResultRow + FetchOffset + 1*|  
|*FetchOffset < 0 y &#124; FetchOffset &#124; > LASTRESULTROW y &#124; FetchOffset &#124; > RowsetSize* <sup>[2]</sup>|*Antes del inicio*|  
|*FetchOffset < 0 y &#124; FetchOffset &#124; > LASTRESULTROW y &#124; FetchOffset &#124; <= RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*FetchOffset = 0*|*Antes del inicio*|  
|*1 <= FetchOffset \< = LastResultRow*|*FetchOffset*|  
|*FetchOffset > LastResultRow*|*Después del final*|  
  
 [1]   **SQLFetchScroll** devuelve SQLSTATE 01S06 (intento de captura antes de que el conjunto de resultados devolviera el primer conjunto de filas) y SQL_SUCCESS_WITH_INFO.  
  
 [2] si se ha cambiado el tamaño del conjunto de filas desde la llamada anterior para capturar filas, este es el nuevo tamaño del conjunto de filas.  
  
 Una captura absoluta realizada en un cursor dinámico no puede proporcionar el resultado necesario porque no se determinan las posiciones de fila de un cursor dinámico. Este tipo de operación es equivalente a una captura primero seguida de una captura relativa; no es una operación atómica, como es una captura absoluta en un cursor estático.  
  
## <a name="sql_fetch_first"></a>SQL_FETCH_FIRST  
 Se aplican las siguientes reglas.  
  
|Condición|Primera fila del conjunto de filas nuevo|  
|---------------|-----------------------------|  
|*Cualquiera*|*1*|  
  
## <a name="sql_fetch_last"></a>SQL_FETCH_LAST  
 Se aplican las siguientes reglas.  
  
|Condición|Primera fila del conjunto de filas nuevo|  
|---------------|-----------------------------|  
|*RowsetSize* <sup>[1]</sup> <= LastResultRow|*LastResultRow-RowsetSize + 1* <sup>[1]</sup>|  
|*RowsetSize* <sup>[1]</sup> > LastResultRow|*1*|  
  
 [1] si se ha cambiado el tamaño del conjunto de filas desde la llamada anterior para capturar filas, este es el nuevo tamaño del conjunto de filas.  
  
## <a name="sql_fetch_bookmark"></a>SQL_FETCH_BOOKMARK  
 Se aplican las siguientes reglas.  
  
|Condición|Primera fila del conjunto de filas nuevo|  
|---------------|-----------------------------|  
|*BookmarkRow + FetchOffset < 1*|*Antes del inicio*|  
|*1 <= BookmarkRow + FetchOffset \< = LastResultRow*|*BookmarkRow + FetchOffset*|  
|*BookmarkRow + FetchOffset > LastResultRow*|*Después del final*|  
  
 Para obtener información sobre los marcadores, vea [marcadores (ODBC)](../../../odbc/reference/develop-app/bookmarks-odbc.md).  
  
## <a name="effect-of-deleted-added-and-error-rows-on-cursor-movement"></a>Efecto de las filas eliminadas, agregadas y de error en el movimiento del cursor  
 A veces, los cursores estáticos y los controlados por conjunto de claves detectan las filas agregadas al conjunto de resultados y quitan las filas eliminadas del conjunto de resultados. Si se llama a **SQLGetInfo** con las opciones SQL_STATIC_CURSOR_ATTRIBUTES2 y SQL_KEYSET_CURSOR_ATTRIBUTES2 y se examinan las máscaras de SQL_CA2_SENSITIVITY_ADDITIONS, SQL_CA2_SENSITIVITY_DELETIONS y SQL_CA2_SENSITIVITY_UPDATES, una aplicación determina si los cursores implementados por un controlador determinado lo hacen. En el caso de los controladores que pueden detectar filas eliminadas y quitarlas, los siguientes párrafos describen los efectos de este comportamiento. En el caso de los controladores que pueden detectar filas eliminadas pero no se pueden quitar, las eliminaciones no tienen ningún efecto en los movimientos de cursor y los párrafos siguientes no se aplican.  
  
 Si el cursor detecta filas agregadas al conjunto de resultados o quita las filas eliminadas del conjunto de resultados, aparece como si detectase estos cambios solo cuando captura datos. Esto incluye el caso en el que se llama a **SQLFetchScroll** con FetchOrientation establecido en SQL_FETCH_RELATIVE y FetchOffset establecido en 0 para volver a capturar el mismo conjunto de filas, pero no incluye el caso cuando se llama a SQLSetPos con fOption establecido en SQL_REFRESH. En el último caso, los datos de los búferes del conjunto de filas se actualizan, pero no se recuperan, y las filas eliminadas no se quitan del conjunto de resultados. Por lo tanto, cuando una fila se elimina o inserta en el conjunto de filas actual, el cursor no modifica los búferes del conjunto de filas. En su lugar, detecta el cambio cuando captura cualquier conjunto de filas que antes incluía la fila eliminada o que ahora incluye la fila insertada.  
  
 Por ejemplo:  
  
```cpp  
// Fetch the next rowset.  
SQLFetchScroll(hstmt, SQL_FETCH_NEXT, 0);  
// Delete third row of the rowset. Does not modify the rowset buffers.  
SQLSetPos(hstmt, 3, SQL_DELETE, SQL_LOCK_NO_CHANGE);  
// The third row has a status of SQL_ROW_DELETED after this call.  
SQLSetPos(hstmt, 3, SQL_REFRESH, SQL_LOCK_NO_CHANGE);  
// Refetch the same rowset. The third row is removed, replaced by what  
// was previously the fourth row.  
SQLFetchScroll(hstmt, SQL_FETCH_RELATIVE, 0);  
```  
  
 Cuando **SQLFetchScroll** devuelve un conjunto de filas nuevo que tiene una posición relativa al conjunto de filas actual; es decir, FetchOrientation es SQL_FETCH_NEXT, SQL_FETCH_PRIOR o SQL_FETCH_RELATIVE-no incluye los cambios en el conjunto de filas actual al calcular la posición inicial del nuevo conjunto de filas. Sin embargo, incluye cambios fuera del conjunto de filas actual si es capaz de detectarlos. Además, cuando **SQLFetchScroll** devuelve un conjunto de filas nuevo que tiene una posición independiente del conjunto de filas actual; es decir, FetchOrientation es SQL_FETCH_FIRST, SQL_FETCH_LAST, SQL_FETCH_ABSOLUTE o SQL_FETCH_BOOKMARK incluye todos los cambios que es capaz de detectar, incluso si están en el conjunto de filas actual.  
  
 Al determinar si las filas recién agregadas están dentro o fuera del conjunto de filas actual, se considera que un conjunto de filas parcial termina en la última fila válida. es decir, la última fila para la que no se SQL_ROW_NOROW el estado de la fila. Por ejemplo, supongamos que el cursor es capaz de detectar filas recién agregadas, el conjunto de filas actual es un conjunto de filas parcial, la aplicación agrega nuevas filas y el cursor agrega estas filas al final del conjunto de resultados. Si la aplicación llama a **SQLFetchScroll** con FetchOrientation establecido en SQL_FETCH_NEXT, **SQLFetchScroll** devuelve el conjunto de filas a partir de la primera fila recién agregada.  
  
 Por ejemplo, supongamos que el conjunto de filas actual incluye las filas 21 a 30, el tamaño del conjunto de filas es 10, el cursor quita las filas eliminadas del conjunto de resultados y el cursor detecta las filas agregadas al conjunto de resultados. En la tabla siguiente se muestran las filas que devuelve **SQLFetchScroll** en varias situaciones.  
  
|Change|Tipo de captura|FetchOffset|Nuevo conjunto de filas [1]|  
|------------|----------------|-----------------|---------------------|  
|Eliminar fila 21|NEXT|0|de 31 a 40|  
|Eliminar fila 31|NEXT|0|de 32 a 41|  
|Insertar fila entre las filas 21 y 22|NEXT|0|de 31 a 40|  
|Insertar fila entre las filas 30 y 31|NEXT|0|Fila insertada, de 31 a 39|  
|Eliminar fila 21|PRIOR|0|de 11 a 20|  
|Eliminar fila 20|PRIOR|0|de 10 a 19|  
|Insertar fila entre las filas 21 y 22|PRIOR|0|de 11 a 20|  
|Insertar fila entre las filas 20 y 21|PRIOR|0|de 12 a 20, fila insertada|  
|Eliminar fila 21|RELATIVE|0|de 22 a 31<sup>[2]</sup>|  
|Eliminar fila 21|RELATIVE|1|de 22 a 31|  
|Insertar fila entre las filas 21 y 22|RELATIVE|0|21, fila insertada, de 22 a 29|  
|Insertar fila entre las filas 21 y 22|RELATIVE|1|de 22 a 31|  
|Eliminar fila 21|ABSOLUTE|21|de 22 a 31<sup>[2]</sup>|  
|Eliminar fila 22|ABSOLUTE|21|de 21 a 31|  
|Insertar fila entre las filas 21 y 22|ABSOLUTE|22|Fila insertada, de 22 a 29|  
  
 [1] esta columna utiliza los números de fila antes de insertar o eliminar filas.  
  
 [2] en este caso, el cursor intenta devolver filas a partir de la fila 21. Dado que se ha eliminado la fila 21, la primera fila que devuelve es la fila 22.  
  
 Las filas de error (es decir, las filas con el estado SQL_ROW_ERROR) no afectan al movimiento del cursor. Por ejemplo, si el conjunto de filas actual comienza con la fila 11 y el estado de la fila 11 es SQL_ROW_ERROR, al llamar a **SQLFetchScroll** con FetchOrientation establecido en SQL_FETCH_RELATIVE y FetchOffset establecido en 5 se devuelve el conjunto de filas a partir de la fila 16, tal como lo haría si se SQL_SUCCESS el estado de la fila 11.  
  
## <a name="returning-data-in-bound-columns"></a>Devolver datos en columnas enlazadas  
 **SQLFetchScroll** devuelve datos en columnas enlazadas de la misma manera que **SQLFetch**. Para obtener más información, vea "devolver datos en columnas enlazadas" en la [función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
 Si no hay columnas enlazadas, **SQLFetchScroll** no devuelve datos, pero mueve el cursor de bloque a la posición especificada. El hecho de que los datos se puedan recuperar de las columnas sin enlazar de un cursor de bloque con **SQLGetData** depende del controlador. Esta funcionalidad se admite si una llamada a **SQLGetInfo** devuelve el bit de SQL_GD_BLOCK para el tipo de información de SQL_GETDATA_EXTENSIONS.  
  
## <a name="buffer-addresses"></a>Direcciones de búfer  
 **SQLFetchScroll** usa la misma fórmula para determinar la dirección de los datos y los búferes de longitud/indicador como **SQLFetch**. Para obtener más información, vea el tema sobre las direcciones de búfer en la [función SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md).  
  
## <a name="row-status-array"></a>Matriz de Estados de fila  
 **SQLFetchScroll** establece los valores de la matriz de estado de fila de la misma manera que SQLFetch. Para obtener más información, vea "matriz de estado de fila" en la [función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="rows-fetched-buffer"></a>Búfer de filas recuperadas  
 **SQLFetchScroll** devuelve el número de filas capturadas en el búfer de filas capturadas de la misma manera que **SQLFetch**. Para obtener más información, vea el tema sobre el búfer capturado de filas en la [función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="error-handling"></a>Tratamiento de errores  
 Cuando una aplicación llama a **sqlfetchscroll** en un controlador ODBC 3. x, el administrador de controladores llama a **sqlfetchscroll** en el controlador. Cuando una aplicación llama a **SQLFetchScroll** en un controlador ODBC 2. x, el administrador de controladores llama a SQLExtendedFetch en el controlador. Dado que **SQLFetchScroll** y SQLExtendedFetch controlan los errores de una manera ligeramente diferente, la aplicación observa un comportamiento de error ligeramente diferente cuando llama a **SQLFetchScroll** en los controladores ODBC 2. x y ODBC 3. x.  
  
 **SQLFetchScroll** devuelve errores y advertencias de la misma manera que **SQLFetch**; para obtener más información, vea el tema sobre el control de errores en **SQLFetch**. **SQLExtendedFetch** devuelve errores de la misma manera que **SQLFetch**, con las siguientes excepciones:  
  
 Cuando se produce una advertencia que se aplica a una fila determinada del conjunto de filas, SQLExtendedFetch establece la entrada correspondiente en la matriz de estado de fila en SQL_ROW_SUCCESS, no SQL_ROW_SUCCESS_WITH_INFO.  
  
 Si se producen errores en cada fila del conjunto de filas, SQLExtendedFetch devuelve SQL_SUCCESS_WITH_INFO, no SQL_ERROR.  
  
 En cada grupo de registros de estado que se aplica a una fila individual, el primer registro de estado devuelto por SQLExtendedFetch debe contener SQLSTATE 01S01 (error en la fila); **SQLFetchScroll** no devuelve este SQLSTATE. Si SQLExtendedFetch no puede devolver SQLSTATEs adicionales, todavía debe devolver este SQLSTATE.  
  
## <a name="sqlfetchscroll-and-optimistic-concurrency"></a>SQLFetchScroll y simultaneidad optimista  
 Si un cursor usa simultaneidad optimista, es decir, el atributo de instrucción SQL_ATTR_CONCURRENCY tiene un valor de SQL_CONCUR_VALUES o SQL_CONCUR_ROWVER- **SQLFetchScroll** actualiza los valores de simultaneidad optimista utilizados por el origen de datos para detectar si una fila ha cambiado. Esto sucede siempre que **SQLFetchScroll** captura un nuevo conjunto de filas, incluso cuando recupera el conjunto de filas actual. (Se llama con FetchOrientation establecido en SQL_FETCH_RELATIVE y FetchOffset establecido en 0).  
  
## <a name="sqlfetchscroll-and-odbc-2x-drivers"></a>Controladores de SQLFetchScroll y ODBC 2. x  
 Cuando una aplicación llama a **SQLFetchScroll** en un controlador ODBC 2. x, el administrador de controladores asigna esta llamada a **SQLExtendedFetch**. Pasa los valores siguientes para los argumentos de **SQLExtendedFetch**.  
  
|Argumento SQLExtendedFetch|Value|  
|-------------------------------|-----------|  
|StatementHandle|StatementHandle en **SQLFetchScroll**.|  
|FetchOrientation|FetchOrientation en **SQLFetchScroll**.|  
|FetchOffset|Si FetchOrientation no se SQL_FETCH_BOOKMARK, se usa el valor del argumento FetchOffset en **SQLFetchScroll** .<br /><br /> Si FetchOrientation es SQL_FETCH_BOOKMARK, se utiliza el valor almacenado en la dirección especificada por el atributo de la instrucción SQL_ATTR_FETCH_BOOKMARK_PTR.|  
|RowCountPtr|La dirección especificada por el atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR.|  
|RowStatusArray|La dirección especificada por el atributo de instrucción SQL_ATTR_ROW_STATUS_PTR.|  
  
 Para obtener más información, vea [cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) en el Apéndice G: instrucciones de controlador para la compatibilidad con versiones anteriores.  
  
## <a name="descriptors-and-sqlfetchscroll"></a>Descriptores y SQLFetchScroll  
 **SQLFetchScroll** interactúa con descriptores de la misma manera que **SQLFetch**. Para obtener más información, vea la sección sobre descriptores y SQLFetchScroll en la [función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md).  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea [enlace de modo de columna](../../../odbc/reference/develop-app/column-wise-binding.md), enlace de modo de [fila](../../../odbc/reference/develop-app/row-wise-binding.md), [actualización posicionada y instrucciones de eliminación](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md)y [actualización de filas en el conjunto de filas con SQLSetPos](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Realización de operaciones Bulk Insert, Update o DELETE|[Función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver información acerca de una columna de un conjunto de resultados|[SQLDescribeCol (función)](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Capturar una sola fila o un bloque de datos en una dirección de solo avance|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Cerrar el cursor en la instrucción|[Función SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Devolver el número de columnas del conjunto de resultados|[SQLNumResultCols (función)](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Colocar el cursor, actualizar los datos del conjunto de filas o actualizar o eliminar los datos del conjunto de resultados|[Función SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Establecer un atributo de instrucción|[Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
