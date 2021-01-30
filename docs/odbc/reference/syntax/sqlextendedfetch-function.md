---
description: Función SQLExtendedFetch
title: Función SQLExtendedFetch | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLExtendedFetch
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLExtendedFetch
helpviewer_keywords:
- SQLExtendedFetch function [ODBC]
ms.assetid: 940b5cf7-581c-4ede-8533-c67d5e9ef488
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 403b8d426049ba6a333920d472c2e69ed279d761
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179863"
---
# <a name="sqlextendedfetch-function"></a>Función SQLExtendedFetch
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: desusado  
  
 **Resumen**  
 **SQLExtendedFetch** recupera el conjunto de filas especificado de los datos del conjunto de resultados y devuelve los datos de todas las columnas enlazadas. Los conjuntos de filas se pueden especificar en una posición absoluta o relativa o por marcador.  
  
> [!NOTE]
>  En ODBC 3 *. x*, **SQLExtendedFetch** se ha reemplazado por **SQLFetchScroll**. Las aplicaciones ODBC 3 *. x* no deben llamar a **SQLExtendedFetch**; en su lugar, deben llamar a **SQLFetchScroll**. El administrador de controladores asigna **SQLFetchScroll** a **SQLExtendedFetch** cuando se trabaja con un controlador ODBC 2 *. x* . Los controladores ODBC 3 *. x* deben admitir **SQLExtendedFetch** si desean trabajar con aplicaciones ODBC 2 *. x* que lo llaman. Para obtener más información, vea "Comentarios" y [cursores de bloque, cursores desplazables y compatibilidad con versiones anteriores](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) en el Apéndice G: instrucciones de controlador para la compatibilidad con versiones anteriores.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLExtendedFetch(  
      SQLHSTMT         StatementHandle,  
      SQLUSMALLINT     FetchOrientation,  
      SQLLEN           FetchOffset,  
      SQLULEN *        RowCountPtr,  
      SQLUSMALLINT *   RowStatusArray);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *FetchOrientation*  
 Entradas Tipo de captura. Es lo mismo que *FetchOrientation* en **SQLFetchScroll**.  
  
 *FetchOffset*  
 Entradas Número de la fila que se va a capturar. Es lo mismo que *FetchOffset* en **SQLFetchScroll**, con una excepción. Cuando *FetchOrientation* es SQL_FETCH_BOOKMARK, *FetchOffset* es un marcador de longitud fija, no un desplazamiento de un marcador. En otras palabras, **SQLExtendedFetch** recupera el marcador de este argumento, no el atributo de instrucción SQL_ATTR_FETCH_BOOKMARK_PTR. No admite marcadores de longitud variable y no admite la captura de un conjunto de filas en un desplazamiento (distinto de 0) de un marcador.  
  
 *RowCountPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número de filas que se capturan realmente. Este búfer se utiliza de la misma manera que el búfer especificado por el atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR. Este búfer solo se usa en **SQLExtendedFetch**. No lo usa **SQLFetch** o **SQLFetchScroll**.  
  
 *RowStatusArray*  
 Genere Puntero a una matriz en la que se va a devolver el estado de cada fila. Esta matriz se utiliza de la misma manera que la matriz especificada por el atributo de instrucción SQL_ATTR_ROW_STATUS_PTR.  
  
 Sin embargo, la dirección de esta matriz no se almacena en el campo SQL_DESC_STATUS_ARRAY_PTR de IRD. Además, la matriz solo se usa en **SQLExtendedFetch** y **SQLBulkOperations** con una *operación* de SQL_ADD o **SQLSetPos** cuando se llama después de **SQLExtendedFetch**. No lo usa **SQLFetch** ni **Sqlfetchscroll**, y **SQLBulkOperations** o **SQLSetPos** no lo usan cuando se llama después de **SQLFetch** o **SQLFetchScroll**. Tampoco se utiliza cuando se llama a **SQLBulkOperations** con una *operación* de SQL_ADD antes de llamar a cualquier función Fetch. En otras palabras, solo se usa en el estado de instrucción S7. No se utiliza en Estados de instrucción S5 o S6. Para obtener más información, vea [transiciones de instrucciones](../../../odbc/reference/appendixes/statement-transitions.md) en el Apéndice B: tablas de transición de estado de ODBC.  
  
 Las aplicaciones deben proporcionar un puntero válido en el argumento *RowStatusArray* ; en caso contrario, el comportamiento de **SQLExtendedFetch** y el comportamiento de las llamadas a **SQLBulkOperations** o **SQLSetPos** después de que un cursor se haya colocado por **SQLExtendedFetch** no están definidos.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLExtendedFetch** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLError**. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLExtendedFetch** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario. Si se produce un error en una sola columna, se puede llamar a **SQLGetDiagField** con un *DiagIdentifier* de SQL_DIAG_COLUMN_NUMBER para determinar la columna en la que se produjo el error; y se puede llamar a **SQLGetDiagField** con un *DiagIdentifier* de SQL_DIAG_ROW_NUMBER para determinar la fila que contiene esa columna.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|Los datos binarios o de cadena devueltos para una columna dieron como resultado el truncamiento de datos binarios de caracteres que no están en blanco o no NULOs. Si fuera un valor de cadena, se truncó a la derecha. Si se trata de un valor numérico, se truncó la parte fraccionaria del número.  (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S01|Error en la fila|Se produjo un error al capturar una o más filas. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S06|Intento de captura antes de que el conjunto de resultados devolviera el primer conjunto de filas|El conjunto de filas solicitado se superpone al inicio del conjunto de resultados cuando la posición actual estaba más allá de la primera fila, y se SQL_PRIOR *FetchOrientation* o SQL_RELATIVE *FetchOrientation* con una *FetchOffset* negativa cuyo valor absoluto era menor o igual que el SQL_ROWSET_SIZE actual. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S07|Truncamiento fraccionario|Los datos devueltos para una columna se han truncado. En el caso de los tipos de datos numéricos, se truncó la parte fraccionaria del número. En el caso de los tipos de datos Time, TIMESTAMP y Interval que contienen un componente de hora, se truncó la parte fraccionaria de la hora.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|No se pudo convertir un valor de datos al tipo de datos C especificado por *TargetType* en **SQLBindCol**.|  
|07009|Índice de descriptor no válido|La columna 0 se ha enlazado con **SQLBindCol** y el atributo SQL_ATTR_USE_BOOKMARKS instrucción se ha establecido en SQL_UB_OFF.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|22002|Se requiere una variable de indicador pero no se ha proporcionado|Se han capturado datos NULL en una columna cuyo *StrLen_or_IndPtr* establecida por **SQLBindCol** era un puntero nulo.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|22003|Valor numérico fuera del intervalo|Si se devuelve el valor numérico (como Numeric o String) para una o más columnas, la parte entera (en oposición a fraccionaria) del número se truncará.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).<br /><br /> Para obtener más información, vea [instrucciones para los tipos de datos Interval y Numeric](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md) en el Apéndice D: tipos de datos.|  
|22007|Formato de fecha y hora no válido|Una columna de caracteres del conjunto de resultados estaba enlazada a una estructura de fecha, hora o marca de tiempo de C, y un valor de la columna era, respectivamente, una fecha, una hora o una marca de tiempo no válidas.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|22012|División por cero|Se devolvió un valor de una expresión aritmética, lo que da como resultado una división por cero.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|22015|Desbordamiento de campo de intervalo|La asignación de un tipo numérico exacto o de intervalo SQL a un tipo de intervalo C provoca una pérdida de dígitos significativos en el campo inicial.<br /><br /> Al capturar datos en un tipo de intervalo C, no había ninguna representación del valor del tipo SQL en el tipo de intervalo C.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|22018|Valor de carácter no válido para la especificación de conversión|El tipo C era un tipo de datos numérico exacto o aproximado, un valor de fecha y hora o un intervalo. el tipo SQL de la columna era un tipo de datos de caracteres. y el valor de la columna no era un literal válido del tipo C enlazado.<br /><br /> (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|24000|Estado de cursor no válido|El *StatementHandle* estaba en un estado ejecutado, pero no hay ningún conjunto de resultados asociado a *StatementHandle*.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLError** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle* y, a continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLExtendedFetch** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) el *StatementHandle* especificado no se encontraba en un estado ejecutado. Se llamó a la función sin llamar primero a **SQLExecDirect**, **SQLExecute** o a una función de catálogo.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> Se llamó a **SQLExtendedFetch** (DM) para *StatementHandle* después de llamar a **SQLFetch** o **SQLFetchScroll** y antes de llamar a **SQLFreeStmt** con la opción SQL_CLOSE.<br /><br /> Se llamó a **SQLBulkOperations** (DM) para una instrucción antes de llamar a **SQLFetch**, **SQLFetchScroll** o **SQLExtendedFetch** y, a continuación, se llamó a **SQLExtendedFetch** antes de llamar a **SQLFreeStmt** con la opción SQL_CLOSE.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY106|Tipo de captura fuera del intervalo|(DM) el valor especificado para el argumento *FetchOrientation* no era válido. (Vea "Comentarios").<br /><br /> El argumento *FetchOrientation* se SQL_FETCH_BOOKMARK y el atributo SQL_ATTR_USE_BOOKMARKS instrucción se estableció en SQL_UB_OFF.<br /><br /> Se SQL_CURSOR_FORWARD_ONLY el valor de la opción de instrucción SQL_CURSOR_TYPE y no se SQL_FETCH_NEXT el valor del argumento *FetchOrientation* .<br /><br /> El argumento *FetchOrientation* se SQL_FETCH_RESUME.|  
|HY107|Valor de fila fuera del intervalo|El valor especificado con la opción de instrucción SQL_CURSOR_TYPE era SQL_CURSOR_KEYSET_DRIVEN, pero el valor especificado con el atributo de instrucción SQL_KEYSET_SIZE era mayor que 0 y menor que el valor especificado con el atributo de instrucción SQL_ROWSET_SIZE.|  
|HY111|Valor de marcador no válido|El argumento *FetchOrientation* se SQL_FETCH_BOOKMARK y el marcador especificado en el argumento *FetchOffset* no era válido.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El controlador o el origen de datos no admite el tipo de captura especificado.<br /><br /> El controlador o el origen de datos no admite la conversión especificada por la combinación de *TargetType* en **SQLBindCol** y el tipo de datos SQL de la columna correspondiente. Este error solo se aplica cuando el tipo de datos SQL de la columna se ha asignado a un tipo de datos SQL específico del controlador.|  
|HYT00|Tiempo de espera agotado|El período de tiempo de espera de consulta expiró antes de que el origen de datos devolviera el conjunto de resultados. El período de tiempo de espera se establece a través de **SQLSetStmtOption**, SQL_QUERY_TIMEOUT.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 El comportamiento de **SQLExtendedFetch** es idéntico al de **SQLFetchScroll**, con las siguientes excepciones:  
  
-   **SQLExtendedFetch** y **SQLFetchScroll** usan métodos diferentes para devolver el número de filas recuperadas. **SQLExtendedFetch** devuelve el número de filas recuperadas en *\* RowCountPtr*; **SQLFetchScroll** devuelve el número de filas que se capturan directamente en el búfer al que apunta SQL_ATTR_ROWS_FETCHED_PTR. Para obtener más información, vea el argumento *RowCountPtr* .  
  
-   **SQLExtendedFetch** y **SQLFetchScroll** devuelven el estado de cada fila en diferentes matrices. Para obtener más información, vea el argumento *RowStatusArray* .  
  
-   **SQLExtendedFetch** y **SQLFetchScroll** usan métodos diferentes para recuperar el marcador cuando *FetchOrientation* es SQL_FETCH_BOOKMARK. **SQLExtendedFetch** no admite marcadores de longitud variable ni recupera conjuntos de filas en un desplazamiento distinto de 0 de un marcador. Para obtener más información, vea el argumento *FetchOffset* .  
  
-   **SQLExtendedFetch** y **SQLFetchScroll** usan diferentes tamaños de conjunto de filas. **SQLExtendedFetch** usa el valor del atributo de instrucción SQL_ROWSET_SIZE y **SQLFetchScroll** usa el valor del atributo de la instrucción SQL_ATTR_ROW_ARRAY_SIZE.  
  
-   **SQLExtendedFetch** tiene una semántica de control de errores ligeramente diferente que **SQLFetchScroll**. Para obtener más información, vea el tema sobre el control de errores en la sección "Comentarios" de [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
-   **SQLExtendedFetch** no admite desplazamientos de enlace (el atributo de instrucción SQL_ATTR_ROW_BIND_OFFSET_PTR).  
  
-   Las llamadas a **SQLExtendedFetch** no se pueden mezclar con llamadas a **SQLFetch** o **SQLFetchScroll**, y si se llama a **SQLBulkOperations** antes de llamar a una función fetch, no se puede llamar a **SQLExtendedFetch** hasta que el cursor se cierre y se vuelva a abrir. Es decir, solo se puede llamar a **SQLExtendedFetch** en el estado de instrucción S7. Para obtener más información, vea [transiciones de instrucciones](../../../odbc/reference/appendixes/statement-transitions.md) en el Apéndice B: tablas de transición de estado de ODBC.  
  
 Cuando una aplicación llama a **SQLFetchScroll** mientras usa un controlador ODBC 2 *. x* , el administrador de controladores asigna esta llamada a **SQLExtendedFetch**. Para obtener más información, consulte los controladores "SQLFetchScroll y ODBC 2 *. x* " en [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
 En ODBC 2 *. x*, se llamó a **SQLExtendedFetch** para capturar varias filas y se llamó a **SQLFetch** para capturar una sola fila. En ODBC 3 *. x*, por otro lado, se puede llamar a **SQLFetch** para capturar varias filas.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Realización de operaciones Bulk Insert, Update o DELETE|[Función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver información acerca de una columna de un conjunto de resultados|[SQLDescribeCol (función)](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Devolver el número de columnas del conjunto de resultados|[SQLNumResultCols (función)](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Colocar el cursor, actualizar los datos del conjunto de filas o actualizar o eliminar los datos del conjunto de resultados|[Función SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|Establecer un atributo de instrucción|[Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
