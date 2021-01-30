---
description: SQLMoreResults (función)
title: Función SQLMoreResults | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLMoreResults
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLMoreResults
helpviewer_keywords:
- SQLMoreResults function [ODBC]
ms.assetid: bf169ed5-4d55-412c-b184-12065a726e89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d894cc8cf87a3db794bed763c9bffc596910299a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160797"
---
# <a name="sqlmoreresults-function"></a>SQLMoreResults (función)
**Conformidad**  
 Versión introducida: ODBC 1,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLMoreResults** determina si hay más resultados disponibles en una instrucción que contiene instrucciones **Select**, **Update**, **Insert** o **Delete** y, en ese caso, inicializa el procesamiento de esos resultados.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLMoreResults(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_NO_DATA, SQL_ERROR, SQL_INVALID_HANDLE O SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLMoreResults** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que suele devolver **SQLMoreResults** y se explica cada uno en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S02|El valor de la opción ha cambiado|El valor de un atributo de instrucción ha cambiado a medida que se estaba procesando el lote. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función **SQLMoreResults** y, antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función **SQLMoreResults** en *StatementHandle*.<br /><br /> Se llamó a la función **SQLMoreResults** y, antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLMoreResults** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 Las instrucciones **Select** devuelven conjuntos de resultados. Las instrucciones **Update**, **Insert** y **Delete** devuelven un recuento de las filas afectadas. Si alguna de estas instrucciones se procesa por lotes, se envía con matrices de parámetros (numeradas en aumento del orden de los parámetros, en el orden en que aparecen en el lote) o en los procedimientos, pueden devolver varios conjuntos de resultados o recuentos de filas. Para obtener información sobre los lotes de instrucciones y matrices de parámetros, vea [lotes de instrucciones SQL](../../../odbc/reference/develop-app/batches-of-sql-statements.md) y [matrices de valores de parámetro](../../../odbc/reference/develop-app/arrays-of-parameter-values.md).  
  
 Después de ejecutar el lote, la aplicación se coloca en el primer conjunto de resultados. La aplicación puede llamar a **SQLBindCol**, **SQLBulkOperations**, **SQLFetch**, **SQLGetData**, **SQLFetchScroll**, **SQLSetPos** y a todas las funciones de metadatos, en el primero o en todos los conjuntos de resultados posteriores, de la misma forma que si hubiera un único conjunto de resultados. Una vez que se realiza con el primer conjunto de resultados, la aplicación llama a **SQLMoreResults** para pasar al siguiente conjunto de resultados. Si hay otro conjunto de resultados o un recuento disponible, **SQLMoreResults** devuelve SQL_SUCCESS e inicializa el conjunto de resultados o el recuento para el procesamiento adicional. Si las instrucciones de generación de recuento de filas aparecen entre las instrucciones que generan conjuntos de resultados, se pueden pasar por una llamada a **SQLMoreResults**. Después de llamar a **SQLMoreResults** para las instrucciones **Update**, **Insert** o **Delete** , una aplicación puede llamar a **SQLRowCount**.  
  
 Si hay un conjunto de resultados actual con filas sin capturar, **SQLMoreResults** descarta dicho conjunto de resultados y hace que esté disponible el siguiente conjunto de resultados o recuento. Si se han procesado todos los resultados, **SQLMoreResults** devuelve SQL_NO_DATA. En algunos controladores, los parámetros de salida y los valores devueltos no están disponibles hasta que se hayan procesado todos los conjuntos de resultados y recuentos de filas. Para estos controladores, los parámetros de salida y los valores devueltos están disponibles cuando **SQLMoreResults** devuelve SQL_NO_DATA.  
  
 Los enlaces que se establecieron para el conjunto de resultados anterior siguen siendo válidos. Si las estructuras de columna son diferentes para este conjunto de resultados, la llamada a **SQLFetch** o **SQLFetchScroll** puede producir un error o un truncamiento. Para evitar esto, la aplicación tiene que llamar a **SQLBindCol** para volver a enlazar explícitamente según corresponda (o hacerlo estableciendo los campos de descriptor). Como alternativa, la aplicación puede llamar a **SQLFreeStmt** con una *opción* de SQL_UNBIND para desenlazar todos los búferes de columna.  
  
 Los valores de los atributos de instrucción, como el tipo de cursor, la simultaneidad del cursor, el tamaño del conjunto de claves o la longitud máxima, pueden cambiar a medida que la aplicación navega por el lote mediante llamadas a **SQLMoreResults**. Si esto ocurre, **SQLMoreResults** devolverá SQL_SUCCESS_WITH_INFO y SQLSTATE 01S02 (el valor de opción ha cambiado).  
  
 Al llamar a **SQLCloseCursor** o **SQLFreeStmt** con una *opción* de SQL_CLOSE, se descartan todos los conjuntos de resultados y recuentos de filas que estaban disponibles como resultado de la ejecución del lote. El identificador de instrucción vuelve al estado asignado o preparado. Llamar a **SQLCancel** para cancelar una función que se ejecuta de forma asincrónica cuando se ha ejecutado un lote y el identificador de instrucción se encuentra en los resultados de estado de ejecución, posición de cursor o asincrónico en todos los conjuntos de resultados y recuentos de filas generados por el lote que se está descartando si la llamada de cancelación se realizó correctamente. A continuación, la instrucción vuelve al estado preparado o asignado.  
  
 Si un lote de instrucciones o un procedimiento combina otras instrucciones SQL con instrucciones **Select**, **Update**, **Insert** y **Delete** , estas otras instrucciones no afectan a **SQLMoreResults**.  
  
 Para obtener más información, vea [varios resultados](../../../odbc/reference/develop-app/multiple-results.md).  
  
 Si una instrucción UPDATE, INSERT o DELETE buscada en un lote de instrucciones no afecta a ninguna fila del origen de datos, **SQLMoreResults** devuelve SQL_SUCCESS. Esto es diferente del caso de una instrucción UPDATE, INSERT o DELETE de búsqueda que se ejecuta a través de **SQLExecDirect**, **SQLExecute** o **SQLParamData**, que devuelve SQL_NO_DATA si no afecta a ninguna fila del origen de datos. Si una aplicación llama a **SQLRowCount** para recuperar el recuento de filas después de que una llamada a **SQLMoreResults** no afecte a ninguna fila, **SQLRowCount** devolverá SQL_NO_DATA.  
  
 Para obtener más información acerca de la secuenciación válida de las funciones de procesamiento de resultados, vea [Apéndice B: tablas de transición de estado de ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).  
  
 Para obtener más información sobre SQL_PARAM_DATA_AVAILABLE y los parámetros de salida transmitidos, consulte [recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="availability-of-row-counts"></a>Disponibilidad de recuentos de filas  
 Cuando un lote contiene varias instrucciones de generación de recuento de filas consecutivas, es posible que estos recuentos de filas se acumulen en un solo recuento de filas. Por ejemplo, si un lote tiene cinco instrucciones INSERT, algunos orígenes de datos pueden devolver cinco recuentos de filas individuales. Otros orígenes de datos devuelven solo un recuento de filas que representa la suma de los cinco recuentos de filas individuales.  
  
 Cuando un lote contiene una combinación de instrucciones de generación de conjuntos de resultados y recuentos de filas, los recuentos de filas pueden o no estar disponibles en absoluto. El comportamiento del controlador con respecto a la disponibilidad de los recuentos de filas se enumera en el tipo de información SQL_BATCH_ROW_COUNT disponible a través de una llamada a **SQLGetInfo**. Por ejemplo, supongamos que el lote contiene una instrucción **Select**, seguida de dos **Insert** s y otro **Select**. A continuación, se pueden realizar los siguientes casos:  
  
-   Los recuentos de filas correspondientes a las dos instrucciones **Insert** no están disponibles en absoluto. La primera llamada a **SQLMoreResults** lo colocará en el conjunto de resultados de la segunda instrucción **Select** .  
  
-   Los recuentos de filas correspondientes a las dos instrucciones **Insert** están disponibles de forma individual. (Una llamada a **SQLGetInfo** no devuelve el SQL_BRC_ROLLED_UP bit para el tipo de información de SQL_BATCH_ROW_COUNT). La primera llamada a **SQLMoreResults** lo colocará en el recuento de filas de la primera **inserción** y la segunda llamará al recuento de filas de la segunda **inserción**. La tercera llamada a **SQLMoreResults** lo colocará en el conjunto de resultados de la segunda instrucción **Select** .  
  
-   Los recuentos de filas correspondientes a las dos **inserciones** se acumulan en un solo recuento de filas que está disponible. (Una llamada a **SQLGetInfo** devuelve el bit de SQL_BRC_ROLLED_UP para el tipo de información de SQL_BATCH_ROW_COUNT). La primera llamada a **sqlmoreresults** lo colocará en el recuento de filas acumuladas y la segunda llamada a **sqlmoreresults** lo colocará en el conjunto de resultados de la segunda **selección**.  
  
 Algunos controladores hacen que los recuentos de filas estén disponibles solo para los lotes explícitos y no para los procedimientos almacenados.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Obtener un bloque de datos o desplazarse por un conjunto de resultados|[Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Capturar una sola fila o un bloque de datos en una dirección de solo avance|[Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Capturar parte o toda una columna de datos|[Función SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
