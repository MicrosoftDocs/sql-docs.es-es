---
description: Función SQLParamData
title: Función SQLParamData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLParamData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLParamData
helpviewer_keywords:
- SQLParamData function [ODBC]
ms.assetid: 68fe010d-9539-4e5b-a260-c8d32423b1db
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e050102612c7e5d370e730909522fa23e801a8f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195851"
---
# <a name="sqlparamdata-function"></a>Función SQLParamData
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLParamData** se usa junto con **SQLPutData** para proporcionar datos de parámetros en el tiempo de ejecución de la instrucción y con **SQLGetData** para recuperar los datos de parámetros de salida transmitidos.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLParamData(  
     SQLHSTMT       StatementHandle,  
     SQLPOINTER *   ValuePtrPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *ValuePtrPtr*  
 Genere Puntero a un búfer en el que se va a devolver la dirección del búfer *ParameterValuePtr* especificado en **SQLBindParameter** (para los datos de parámetros) o la dirección del búfer *TargetValuePtr* especificado en **SQLBindCol** (para los datos de columna), como se incluye en el campo registro del descriptor de SQL_DESC_DATA_PTR.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR, SQL_INVALID_HANDLE o SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLParamData** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLParamData** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|El valor de datos identificado por el argumento *ValueType* en **SQLBindParameter** para el parámetro enlazado no se pudo convertir al tipo de datos identificado por el argumento *ParameterType* en **SQLBindParameter**.<br /><br /> El valor de datos devuelto para un parámetro enlazado como SQL_PARAM_INPUT_OUTPUT o SQL_PARAM_OUTPUT no se pudo convertir al tipo de datos identificado por el argumento *ValueType* en **SQLBindParameter**.<br /><br /> (Si no se pudieron convertir los valores de datos de una o varias filas, pero se devolvieron una o varias filas correctamente, esta función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|22026|Datos de cadena, desigualdad de longitud|El tipo de información SQL_NEED_LONG_DATA_LEN en **SQLGetInfo** era "y" y se enviaron menos datos para un parámetro Long (el tipo de datos era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo de datos específico del origen de datos largo) que se especificó con el argumento *StrLen_or_IndPtr* en **SQLBindParameter**.<br /><br /> El tipo de información SQL_NEED_LONG_DATA_LEN en **SQLGetInfo** era "y" y se enviaron menos datos para una columna larga (el tipo de datos era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo de datos específico del origen de datos) que se especificó en el búfer de longitud correspondiente a una columna de una fila de datos que se agregó o actualizó con **SQLBulkOperations** o se actualizó con **SQLSetPos**.|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40003|Finalización de instrucciones desconocida|No se pudo establecer la conexión asociada durante la ejecución de esta función y no se puede determinar el estado de la transacción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*; a continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) la llamada de función anterior no era una llamada a **SQLExecDirect**, **SQLExecute**, **SQLBulkOperations** o **SQLSetPos** donde el código de retorno se SQL_NEED_DATA, o la llamada de función anterior era una llamada a **SQLPutData**.<br /><br /> La llamada de función anterior era una llamada a **SQLParamData**.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLParamData** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvieron SQL_NEED_DATA. Se llamó a **SQLCancel** antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador que corresponde a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
 Si se llama a **SQLParamData** mientras se envían datos para un parámetro en una instrucción SQL, puede devolver cualquier SQLSTATE que pueda ser devuelto por la función a la que se llama para ejecutar la instrucción (**SQLExecute** o **SQLExecDirect**). Si se llama al envío de datos para una columna que se está actualizando o agregando con **SQLBulkOperations** o se está actualizando con **SQLSetPos**, puede devolver cualquier SQLSTATE que pueda ser devuelto por **SQLBulkOperations** o **SQLSetPos**.  
  
## <a name="comments"></a>Comentarios  
 Se puede llamar a **SQLParamData** para proporcionar datos de datos en ejecución para dos usos: datos de parámetro que se usarán en una llamada a **SQLExecute** o **SQLExecDirect**, o datos de columna que se usarán cuando se actualice o agregue una fila mediante una llamada a **SQLBulkOperations** o se actualice mediante una llamada a **SQLSetPos**. En tiempo de ejecución, **SQLParamData** devuelve a la aplicación un indicador de los datos que requiere el controlador.  
  
 Cuando una aplicación llama a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos**, el controlador devuelve SQL_NEED_DATA si necesita datos de datos en ejecución. Después, una aplicación llama a **SQLParamData** para determinar qué datos se van a enviar. Si el controlador requiere datos de parámetros, el controlador devuelve en el búfer de salida *\* ValuePtrPtr* el valor que la aplicación coloca en el búfer del conjunto de filas. La aplicación puede utilizar este valor para determinar qué datos de parámetro solicita el controlador. Si el controlador requiere datos de columna, el controlador devuelve en el búfer de *\* ValuePtrPtr* la dirección a la que la columna estaba enlazada originalmente, como se indica a continuación:  
  
 *Dirección*  +  enlazada *Desplazamiento de enlace* + ((*número de fila* -1) x tamaño de *elemento*)  
  
 donde las variables se definen como se indica en la tabla siguiente.  
  
|Variable|Descripción|  
|--------------|-----------------|  
|*Dirección enlazada*|Dirección especificada con el argumento *TargetValuePtr* en **SQLBindCol**.|  
|*Desplazamiento de enlace*|El valor almacenado en la dirección especificada con el atributo de instrucción SQL_ATTR_ROW_BIND_OFFSET_PTR.|  
|*Número de fila*|Número basado en 1 de la fila del conjunto de filas. En el caso de las capturas de una sola fila, que son el valor predeterminado, es 1.|  
|*Tamaño del elemento*|El valor del atributo de la instrucción SQL_ATTR_ROW_BIND_TYPE para los búferes de datos y longitud/indicador.|  
  
 También devuelve SQL_NEED_DATA, que es un indicador de la aplicación que debe llamar a **SQLPutData** para enviar los datos.  
  
 La aplicación llama a **SQLPutData** tantas veces como sea necesario para enviar los datos de datos en ejecución para la columna o el parámetro. Una vez que se han enviado todos los datos para la columna o el parámetro, la aplicación llama a **SQLParamData** de nuevo. Si **SQLParamData** devuelve de nuevo SQL_NEED_DATA, se deben enviar los datos para otro parámetro o columna. Por lo tanto, la aplicación llama de nuevo a **SQLPutData**. Si se han enviado todos los datos de datos en ejecución para todos los parámetros o columnas, **SQLParamData** devuelve SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, el valor de *\* ValuePtrPtr* es INdefinido y la instrucción SQL se puede ejecutar o se puede procesar la llamada **SQLBulkOperations** o **SQLSetPos** .  
  
 Si **SQLParamData** proporciona datos de parámetros para una instrucción UPDATE o DELETE buscada que no afecta a ninguna fila del origen de datos, la llamada a **SQLParamData** devuelve SQL_NO_DATA.  
  
 Para obtener más información sobre cómo se pasan los datos de parámetros de datos en ejecución en el tiempo de ejecución de la instrucción, vea "pasar valores de parámetro" en [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) y [enviar datos largos](../../../odbc/reference/develop-app/sending-long-data.md). Para obtener más información sobre cómo se actualizan o agregan los datos de las columnas de datos en ejecución, vea la sección "usar SQLSetPos" en [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md), "realizar actualizaciones masivas mediante marcadores" en [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)y [Long Data y SQLSetPos y SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md).  
  
 Se puede llamar a **SQLParamData** para recuperar los parámetros de salida transmitidos por secuencias. Cuando **SQLMoreResults**, **SQLExecute**, **SQLGetData** o **SQLExecDirect** devuelve SQL_PARAM_DATA_AVAILABLE, llame a **SQLParamData** para determinar qué parámetro tiene un valor disponible. Para obtener más información sobre SQL_PARAM_DATA_AVAILABLE y los parámetros de salida transmitidos, consulte [recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a un parámetro|[Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver información sobre un parámetro en una instrucción|[Función SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Enviar datos de parámetros en tiempo de ejecución|[Función SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
