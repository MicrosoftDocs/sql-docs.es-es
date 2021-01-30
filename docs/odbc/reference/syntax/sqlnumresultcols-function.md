---
description: SQLNumResultCols (función)
title: Función SQLNumResultCols | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLNumResultCols
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLNumResultCols
helpviewer_keywords:
- SQLNumResultCols function [ODBC]
ms.assetid: d863179f-12a9-4b55-ac6b-7d84202d3da3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8c4f20d6ff0f3c61c8055678226fffa0d15c9a2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182901"
---
# <a name="sqlnumresultcols-function"></a>SQLNumResultCols (función)
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLNumResultCols** devuelve el número de columnas de un conjunto de resultados.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLNumResultCols(  
     SQLHSTMT        StatementHandle,  
     SQLSMALLINT *   ColumnCountPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *ColumnCountPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número de columnas del conjunto de resultados. Este recuento no incluye una columna de marcador enlazado.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLNumResultCols** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLNumResultCols** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*; a continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLNumResultsCols** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a la función antes de llamar a **SQLPrepare** o **SQLExecDirect** para *StatementHandle*.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> Consulte [función de SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md) para obtener más información sobre cuándo se puede liberar un identificador de instrucción.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
 **SQLNumResultCols** puede devolver cualquier SQLSTATE que pueda ser devuelto por **SQLPrepare** o **SQLExecute** cuando se llama después de **SQLPrepare** y antes de **SQLExecute**, dependiendo de si el origen de datos evalúa la instrucción SQL asociada a la instrucción.  
  
## <a name="comments"></a>Comentarios  
 Solo se puede llamar a **SQLNumResultCols** correctamente cuando la instrucción está en el estado preparado, ejecutado o posicionado.  
  
 Si la instrucción asociada a *StatementHandle* no devuelve columnas, **SQLNumResultCols** establece **ColumnCountPtr* en 0.  
  
 El número de columnas devueltas por **SQLNumResultCols** es el mismo valor que el campo de SQL_DESC_COUNT de IRD.  
  
 Para obtener más información, vea ¿ [se ha creado un conjunto de resultados?](../../../odbc/reference/develop-app/was-a-result-set-created.md) y [cómo se usan los metadatos](../../../odbc/reference/develop-app/how-is-metadata-used.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver información acerca de una columna de un conjunto de resultados|[Función SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Devolver información acerca de una columna de un conjunto de resultados|[SQLDescribeCol (función)](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Capturar una sola fila o un bloque de datos en una dirección de solo avance|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Capturar parte o toda una columna de datos|[Función SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|Preparar una instrucción SQL para su ejecución|[Función SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Establecer opciones de desplazamiento del cursor|[Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
