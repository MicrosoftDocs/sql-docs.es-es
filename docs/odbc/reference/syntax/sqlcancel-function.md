---
description: Función SQLCancel
title: Función SQLCancel | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCancel
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCancel
helpviewer_keywords:
- SQLCancel function [ODBC]
ms.assetid: ac0b5972-627f-4440-8c5a-0e8da728726d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c1d630af340bf676deda8c18f415b85aba15403c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185802"
---
# <a name="sqlcancel-function"></a>Función SQLCancel
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLCancel** cancela el procesamiento en una instrucción.  
  
 Para cancelar el procesamiento en una conexión o instrucción, utilice la [función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLCancel(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLCancel** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLCancel** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) en el búfer del argumento *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLCancel** .<br /><br /> Error en la operación de cancelación de (DM) porque hay una operación asincrónica en curso en un identificador de conexión que está asociado a *StatementHandle*.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY018|El servidor rechazó la solicitud de cancelación|El servidor rechazó la solicitud de cancelación.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 **SQLCancel** puede cancelar los siguientes tipos de procesamiento en una instrucción:  
  
-   Función que se ejecuta de forma asincrónica en la instrucción.  
  
-   Función en una instrucción que necesita datos.  
  
-   Función que se ejecuta en la instrucción en otro subproceso.  
  
 En ODBC 2. *x*, si una aplicación llama a **SQLCancel** cuando no se realiza ningún procesamiento en la instrucción, **SQLCancel** tiene el mismo efecto que **SQLFreeStmt** con la opción SQL_CLOSE; Este comportamiento solo se define para la integridad y las aplicaciones deben llamar a **SQLFreeStmt** o **SQLCloseCursor** para cerrar los cursores.  
  
 Cuando se llama a **SQLCancel** para cancelar una función que se ejecuta de forma asincrónica en una instrucción o una función en una instrucción que necesita datos, se borran los registros de diagnóstico publicados por la función que se está cancelando y **SQLCancel** envía sus propios registros de diagnóstico; sin embargo, cuando se llama a **SQLCancel** para cancelar una función que se ejecuta en una instrucción en otro subproceso, no se borran los registros de diagnóstico de la función que se está cancelando y no se publican sus propios registros de diagnóstico.  
  
## <a name="canceling-asynchronous-processing"></a>Cancelar el procesamiento asincrónico  
 Una vez que una aplicación llama a una función de forma asincrónica, llama a la función repetidamente para determinar si ha finalizado el procesamiento. Si todavía se está procesando la función, devuelve SQL_STILL_EXECUTING. Si la función ha finalizado el procesamiento, devuelve un código diferente.  
  
 Después de cualquier llamada a la función que devuelve SQL_STILL_EXECUTING, una aplicación puede llamar a **SQLCancel** para cancelar la función. Si la solicitud de cancelación se realiza correctamente, el controlador devuelve SQL_SUCCESS. Este mensaje no indica que la función se canceló realmente; indica que se ha procesado la solicitud de cancelación. Cuando o si la función se cancela realmente, depende del controlador y dependiente del origen de datos. La aplicación debe seguir llamando a la función original hasta que no se SQL_STILL_EXECUTING el código de retorno. Si la función se canceló correctamente, el código de retorno es SQL_ERROR y SQLSTATE HY008 (operación cancelada). Si la función completa su procesamiento normal, el código de retorno es SQL_SUCCESS o SQL_SUCCESS_WITH_INFO si la función se realizó correctamente o SQL_ERROR y un SQLSTATE distinto de HY008 (operación cancelada) si se produjo un error en la función.  
  
> [!NOTE]  
>  En ODBC 3,5, una llamada a **SQLCancel** cuando no se realiza ningún procesamiento en la instrucción no se trata como **SQLFreeStmt** con la opción SQL_CLOSE, pero no tiene ningún efecto en absoluto. Para cerrar un cursor, una aplicación debe llamar a **SQLCloseCursor**, no a **SQLCancel**.  
  
 Para obtener más información sobre el procesamiento asincrónico, vea [ejecución asincrónica](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md).  
  
## <a name="canceling-functions-that-need-data"></a>Cancelar funciones que necesitan datos  
 Después de que **SQLExecute** o **SQLExecDirect** devuelva SQL_NEED_DATA y antes de que se hayan enviado los datos para todos los parámetros de datos en ejecución, una aplicación puede llamar a **SQLCancel** para cancelar la ejecución de la instrucción. Una vez cancelada la instrucción, la aplicación puede volver a llamar a **SQLExecute** o **SQLExecDirect** . Para obtener más información, consulte [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 Después de que **SQLBulkOperations** o **SQLSetPos** devuelvan SQL_NEED_DATA y antes de que se hayan enviado los datos para todas las columnas de datos en ejecución, una aplicación puede llamar a **SQLCancel** para cancelar la operación. Una vez cancelada la operación, la aplicación puede volver a llamar a **SQLBulkOperations** o **SQLSetPos** ; la cancelación no afecta al estado del cursor ni a la posición actual del cursor. Para obtener más información, vea [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) o [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
## <a name="canceling-functions-executing-on-another-thread"></a>Cancelar funciones que se ejecutan en otro subproceso  
 En una aplicación multiproceso, la aplicación puede cancelar una función que se está ejecutando en otro subproceso. Para cancelar la función, la aplicación llama a **SQLCancel** con el mismo identificador de instrucción que utiliza la función de destino, pero en un subproceso diferente. La forma en que se cancela la función depende del controlador y del sistema operativo. Como en la cancelación de una función que se ejecuta de forma asincrónica, el código de retorno de **SQLCancel** indica únicamente si el controlador procesó la solicitud correctamente. Solo se pueden devolver SQL_SUCCESS o SQL_ERROR; no se devuelve información de diagnóstico. Si se cancela la función original, devuelve SQL_ERROR y SQLSTATE HY008 (operación cancelada).  
  
 Si se ejecuta una instrucción SQL cuando se llama a **SQLCancel** en otro subproceso para cancelar la ejecución de la instrucción, es posible que la ejecución se realice correctamente y que se devuelva SQL_SUCCESS mientras la cancelación también se realiza correctamente. En este caso, el administrador de controladores supone que el cursor abierto por la ejecución de la instrucción se cierra con la cancelación, por lo que la aplicación no podrá usar el cursor.  
  
 Para obtener más información sobre los subprocesos, vea [multithreading](../../../odbc/reference/develop-app/multithreading.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a un parámetro|[Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Realización de operaciones Bulk Insert o Update|[Función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|Cancela una función que se ejecuta de forma asincrónica en un identificador de conexión, además de la funcionalidad de **SQLCancel**.|[Función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Liberar un identificador de instrucción|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Obtención de un campo de un registro de diagnóstico o un campo del encabezado de diagnóstico|[Función SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
|Obtener varios campos de una estructura de datos de diagnóstico|[Función SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|  
|Devolver el siguiente parámetro para el que se van a enviar datos|[Función SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Enviar datos de parámetros en tiempo de ejecución|[Función SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|Colocar el cursor en un conjunto de filas, actualizar los datos del conjunto de filas o actualizar o eliminar los datos del conjunto de resultados|[Función SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
