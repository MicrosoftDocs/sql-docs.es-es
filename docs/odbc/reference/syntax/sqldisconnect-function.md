---
description: Función SQLDisconnect
title: Función SQLDisconnect | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDisconnect
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLDisconnect
helpviewer_keywords:
- SQLDisconnect function [ODBC]
ms.assetid: 9e84a58e-db48-4821-a0cd-5c711fcbe36b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a5f246c3d5aafbc02a138e809a0a07805f8ae30f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165315"
---
# <a name="sqldisconnect-function"></a>Función SQLDisconnect
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLDisconnect** cierra la conexión asociada a un identificador de conexión específico.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLDisconnect(  
     SQLHDBC     ConnectionHandle);  
```  
  
## <a name="arguments"></a>Argumentos  
 *ConnectionHandle*  
 [Entrada] Identificador de conexión.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLDisconnect** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DBC y un *identificador* de *ConnectionHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLDisconnect** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01002|Error de desconexión|Se produjo un error durante la desconexión. Sin embargo, la desconexión se ha realizado correctamente. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08003|Conexión no abierta|(DM) la conexión especificada en el argumento *ConnectionHandle* no estaba abierta.|  
|25000|Estado de transacción no válido|Había una transacción en proceso en la conexión especificada por el argumento *ConnectionHandle*. La transacción permanece activa.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *ConnectionHandle*. Se llamó a la función y antes de finalizado la ejecución de la [función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) en *ConnectionHandle*. A continuación, se llamó de nuevo a la función en *ConnectionHandle*.<br /><br /> Se llamó a la función y antes de que finalizara la ejecución de **SQLCancelHandle** se llamó a en *ConnectionHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para un *StatementHandle* asociado a *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a **SQLDisconnect** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para un *StatementHandle* asociado con *ConnectionHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|El período de tiempo de espera de la conexión expiró antes de que el origen de datos respondiera a la solicitud y la conexión sigue activa. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *ConnectionHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 Si una aplicación llama a **SQLDisconnect** después de que **SQLBrowseConnect** devuelva SQL_NEED_DATA y antes de que devuelva un código de retorno diferente, el controlador cancela el proceso de exploración de conexiones y devuelve la conexión a un estado no conectado.  
  
 Si una aplicación llama a **SQLDisconnect** mientras haya una transacción incompleta asociada al identificador de conexión, el controlador devuelve SQLSTATE 25000 (estado de transacción no válido), lo que indica que la transacción no ha cambiado y la conexión está abierta. Una transacción incompleta es aquella que no se ha confirmado ni revertido con **SQLEndTran**.  
  
 Si una aplicación llama a **SQLDisconnect** antes de liberar todas las instrucciones asociadas a la conexión, el controlador, después de que se haya desconectado correctamente del origen de datos, libera esas instrucciones y todos los descriptores que se han asignado explícitamente en la conexión. Sin embargo, si una o varias de las instrucciones asociadas a la conexión todavía se están ejecutando de forma asincrónica, **SQLDisconnect** devuelve SQL_ERROR con un valor de SQLSTATE de HY010 (error de secuencia de función). Además, **SQLDisconnect** liberará todas las instrucciones asociadas y todos los descriptores que se hayan asignado explícitamente en la conexión, si la conexión está en estado suspendido o si **SQLDisconnect** ha cancelado correctamente **SQLCancelHandle**.  
  
 Para obtener información sobre cómo una aplicación usa **SQLDisconnect**, consulte [desconexión de un origen de datos o un controlador](../../../odbc/reference/develop-app/disconnecting-from-a-data-source-or-driver.md).  
  
## <a name="disconnecting-from-a-pooled-connection"></a>Desconectar de una conexión agrupada  
 Si la agrupación de conexiones está habilitada para un entorno compartido y una aplicación llama a **SQLDisconnect** en una conexión de ese entorno, la conexión se devuelve al grupo de conexiones y sigue estando disponible para otros componentes que usen el mismo entorno compartido.  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea [ejemplo de programa ODBC](../../../odbc/reference/sample-odbc-program.md), [función SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)y [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de un identificador|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Conectarse a un origen de datos|[Función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Conectar con un origen de datos mediante una cadena de conexión o un cuadro de diálogo|[Función SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Ejecutar una operación de confirmación o reversión|[Función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|Liberar un identificador de conexión|[Función SQLFreeConnect](../../../odbc/reference/syntax/sqlfreeconnect-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
