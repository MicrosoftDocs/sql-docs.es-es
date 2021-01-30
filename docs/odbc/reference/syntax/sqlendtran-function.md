---
description: Función SQLEndTran
title: Función SQLEndTran | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLEndTran
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLEndTran
helpviewer_keywords:
- SQLEndTran function [ODBC]
ms.assetid: ff375ce1-eb50-4693-b1e6-70181a6dbf9f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2961cab7c061583f68387e29ed564efc16916870
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195601"
---
# <a name="sqlendtran-function"></a>Función SQLEndTran
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLEndTran** solicita una operación de confirmación o reversión para todas las operaciones activas en todas las instrucciones asociadas a una conexión. **SQLEndTran** también puede solicitar que se realice una operación de confirmación o reversión para todas las conexiones asociadas a un entorno.  
  
> [!NOTE]  
>  Para obtener más información sobre lo que el administrador de controladores asigna a esta función cuando se trata de un ODBC 3. la aplicación *x* está trabajando con ODBC 2. controlador *x* , consulte [asignación de funciones de reemplazo para la compatibilidad con versiones anteriores de las aplicaciones](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLEndTran(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle,  
     SQLSMALLINT   CompletionType);  
```  
  
## <a name="arguments"></a>Argumentos  
 *HandleType*  
 Entradas Identificador de tipo de identificador. Contiene SQL_HANDLE_ENV (si el *identificador* es un identificador de entorno) o SQL_HANDLE_DBC (si el *identificador* es un identificador de conexión).  
  
 *Handle*  
 Entradas Identificador del tipo indicado por *HandleType*, que indica el ámbito de la transacción. Consulte "Comentarios" para obtener más información.  
  
 *CompletionType*  
 Entradas Uno de los dos valores siguientes:  
  
 SQL_COMMIT SQL_ROLLBACK  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE o SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLEndTran** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con el *HandleType* y el *identificador* adecuados. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLEndTran** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08003|Conexión no abierta|(DM) *HandleType* se SQL_HANDLE_DBC y el *identificador* no estaba conectado.|  
|08007|Error de conexión durante la transacción|Se SQL_HANDLE_DBC *HandleType* y la conexión asociada con el *identificador* no se ha realizado durante la ejecución de la función y no se puede determinar si la **confirmación** o **reversión** solicitadas se produjo antes del error.|  
|25S01|Estado de transacción desconocido|Una o varias conexiones del *identificador* no pudieron completar la transacción con el resultado especificado y el resultado es desconocido.|  
|25S02|La transacción sigue activa|El controlador no pudo garantizar que todo el trabajo de la transacción global se pudiera completar de forma atómica y la transacción todavía está activa.|  
|25S03|La transacción se revierte|El controlador no pudo garantizar que todo el trabajo de la transacción global se pudiera completar de forma atómica y todo el trabajo en la transacción activa en el *identificador* se revirtió.|  
|40001|Error de serialización|La transacción se revirtió debido a un interbloqueo de recursos con otra transacción.|  
|40002|Infracción de la restricción de integridad|Se SQL_COMMIT *CompletionType* y el compromiso de los cambios causó una infracción de la restricción de integridad. Como resultado, la transacción se revirtió.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* szMessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *ConnectionHandle*. Se llamó a la función y antes de que finalizara la ejecución de la [función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) en *ConnectionHandle*. A continuación, se llamó de nuevo a la función en *ConnectionHandle*.<br /><br /> Se llamó a la función y antes de que finalizara la ejecución de **SQLCancelHandle** se llamó a en *ConnectionHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para un identificador de instrucción asociado a *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a **SQLEndTran** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para un identificador de instrucción asociado a *ConnectionHandle* y devuelto SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para el *identificador* con *HandleType* establecido en SQL_HANDLE_DBC y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para uno de los identificadores de instrucciones asociados a *Handle* y devueltos SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY012|Código de operación de transacción no válido|(DM) el valor especificado para el argumento *CompletionType* no se SQL_COMMIT ni SQL_ROLLBACK.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY092|Identificador de opción/atributo no válido|(DM) el valor especificado para el argumento *HandleType* no se SQL_HANDLE_ENV ni SQL_HANDLE_DBC.|  
|HY115|No se permite SQLEndTran para un entorno que contenga una conexión con la ejecución de función asincrónica habilitada|No se puede establecer (DM) *HandleType* en SQL_HANDLE_ENV si la ejecución asincrónica de las funciones de conexión está habilitada para una conexión en el entorno.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte la sección Comentarios de este tema.|  
|HYC00|Característica opcional no implementada|El controlador o el origen de datos no admite la operación de **reversión** .|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *ConnectionHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 Para un ODBC 3. controlador *x* , si *HandleType* es SQL_HANDLE_ENV y *Handle* es un identificador de entorno válido, el administrador de controladores llamará a **SQLEndTran** en cada controlador asociado con el entorno. El argumento de *identificador* de la llamada a un controlador será el identificador del entorno del controlador. Para un ODBC 2. controlador *x* , si *HandleType* es SQL_HANDLE_ENV y *Handle* es un identificador de entorno válido, y hay varias conexiones en un estado conectado en ese entorno, el administrador de controladores llamará a **SQLTransact** en el controlador una vez para cada conexión en un estado conectado en ese entorno. El argumento *Handle* en cada llamada será el identificador de la conexión. En cualquier caso, el controlador intentará confirmar o revertir las transacciones, según el valor de *CompletionType*, en todas las conexiones que estén en un estado conectado en ese entorno. Las conexiones que no están activas no afectan a la transacción.  
  
> [!NOTE]  
>  **SQLEndTran** no se puede usar para confirmar o revertir transacciones en un entorno compartido. Se devolverá el valor de SQLSTATE HY092 (identificador de atributo u opción no válido) si se llama a **SQLEndTran** con *el identificador establecido* en el identificador de un entorno compartido o en el identificador de una conexión en un entorno compartido.  
  
 El administrador de controladores devolverá SQL_SUCCESS solo si recibe SQL_SUCCESS para cada conexión. Si el administrador de controladores recibe SQL_ERROR en una o varias conexiones, devuelve SQL_ERROR a la aplicación y la información de diagnóstico se coloca en la estructura de datos de diagnóstico del entorno. Para determinar qué conexión o conexiones no se pudieron realizar durante la operación de confirmación o reversión, la aplicación puede llamar a **SQLGetDiagRec** para cada conexión.  
  
> [!NOTE]  
>  El administrador de controladores no simula una transacción global en todas las conexiones y, por tanto, no utiliza protocolos de confirmación en dos fases.  
  
 Si *CompletionType* es SQL_COMMIT, **SQLEndTran** emite una solicitud de confirmación para todas las operaciones activas en cualquier instrucción asociada a una conexión afectada. Si *CompletionType* es SQL_ROLLBACK, **SQLEndTran** emite una solicitud de reversión para todas las operaciones activas en cualquier instrucción asociada a una conexión afectada. Si no hay ninguna transacción activa, **SQLEndTran** devuelve SQL_SUCCESS sin ningún efecto en los orígenes de datos. Para obtener más información, consulte [confirmar y revertir transacciones](../../../odbc/reference/develop-app/committing-and-rolling-back-transactions.md).  
  
 Si el controlador está en modo de confirmación manual (llamando a **SQLSetConnectAttr** con el atributo SQL_ATTR_AUTOCOMMIT establecido en SQL_AUTOCOMMIT_OFF), se inicia implícitamente una nueva transacción cuando una instrucción SQL que se puede incluir en una transacción se ejecuta en el origen de datos actual. Para obtener más información, vea [modo de confirmación](../../../odbc/reference/develop-app/commit-mode.md).  
  
 Para determinar cómo las operaciones de transacción afectan a los cursores, una aplicación llama a **SQLGetInfo** con las opciones SQL_CURSOR_ROLLBACK_BEHAVIOR y SQL_CURSOR_COMMIT_BEHAVIOR. Para obtener más información, vea los párrafos siguientes y vea el [efecto de las transacciones en los cursores y las instrucciones preparadas](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md).  
  
 Si el valor SQL_CURSOR_ROLLBACK_BEHAVIOR o SQL_CURSOR_COMMIT_BEHAVIOR es igual a SQL_CB_DELETE, **SQLEndTran** cierra y elimina todos los cursores abiertos en todas las instrucciones asociadas a la conexión y descarta todos los resultados pendientes. **SQLEndTran** deja cualquier instrucción presente en un estado asignado (no preparado); la aplicación puede volver a usarlos para solicitudes SQL posteriores o puede llamar a **SQLFreeStmt** o **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_STMT para desasignarlos.  
  
 Si el valor SQL_CURSOR_ROLLBACK_BEHAVIOR o SQL_CURSOR_COMMIT_BEHAVIOR es igual a SQL_CB_CLOSE, **SQLEndTran** cierra todos los cursores abiertos en todas las instrucciones asociadas a la conexión. **SQLEndTran** deja cualquier instrucción presente en un estado preparado; la aplicación puede llamar a **SQLExecute** para una instrucción asociada a la conexión sin llamar primero a **SQLPrepare**.  
  
 Si el valor SQL_CURSOR_ROLLBACK_BEHAVIOR o SQL_CURSOR_COMMIT_BEHAVIOR es igual a SQL_CB_PRESERVE, **SQLEndTran** no afecta a los cursores abiertos asociados a la conexión. Los cursores permanecen en la fila a la que apuntaban antes de la llamada a **SQLEndTran**.  
  
 En el caso de los controladores y orígenes de datos que admiten transacciones, al llamar a **SQLEndTran** con SQL_COMMIT o SQL_ROLLBACK cuando no hay ninguna transacción activa, se devuelve SQL_SUCCESS (lo que indica que no hay ningún trabajo para confirmar o revertir) y no tiene ningún efecto en el origen de datos.  
  
 Cuando un controlador está en modo de confirmación automática, el administrador de controladores no llama a **SQLEndTran** en el controlador. **SQLEndTran** siempre devuelve SQL_SUCCESS independientemente de si se llama con un *CompletionType* de SQL_COMMIT o SQL_ROLLBACK.  
  
 Los controladores o los orígenes de datos que no admiten transacciones (la *opción* **SQLGetInfo** SQL_TXN_CAPABLE es SQL_TC_NONE) siempre están en modo de confirmación automática y, por tanto, siempre devuelven SQL_SUCCESS para **SQLEndTran** tanto si se llaman con un *CompletionType* de SQL_COMMIT o SQL_ROLLBACK. Estos controladores y orígenes de datos no revierten realmente las transacciones cuando se solicita.  
  
## <a name="suspended-state"></a>Estado suspendido  
 En los administradores de controladores que se lanzaron antes de Windows 7, una transacción estaba activa si **SQLEndTran** devolvía SQL_ERROR del controlador. Sin embargo, es posible que la transacción se haya confirmado correctamente en el servidor, pero no se ha notificado al controlador en el cliente (por ejemplo, porque se ha producido un error de red). Esto dejaría la conexión en un estado incorrecto. A partir de Windows 7, cuando **SQLEndTran** devuelve SQL_ERROR, la conexión puede estar en estado suspendido. En un estado suspendido, es posible llamar a funciones de solo lectura. Finalmente, la aplicación debe llamar a **SQLDisconnect** en una conexión suspendida para liberar recursos.  
  
 Si se cumplen todas las condiciones siguientes, la conexión se pone en estado suspendido:  
  
-   El controlador devuelve SQL_ERROR de **SQLEndTran**.  
  
-   El controlador es ODBC versión 3,8 o posterior.  
  
-   La versión de la aplicación es 3,8 o posterior; o bien, la aplicación ODBC 2. x o 3. x recompilada cancela correctamente la función **SQLEndTran** a través de **SQLCancelHandle**.  
  
-   El controlador no devolvió uno de los mensajes siguientes, que confirman que la transacción no se ha completado:  
  
    -   25S03: la transacción se revierte  
  
    -   40001: error de serialización  
  
    -   40002: restricción de integridad  
  
    -   HYC00: característica opcional no implementada  
  
 Si se llamó a **SQLEndTran** en un identificador de entorno y una de sus conexiones cumple las condiciones anteriores, todas las conexiones que se conectan al mismo controlador se pondrán en estado suspendido.  
  
 Una vez que una aplicación llama a **SQLDisconnect** en una conexión suspendida, la conexión se puede usar para volver a conectar con otro origen de datos o con el mismo origen de datos.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Cancelar una función que se ejecuta de forma asincrónica en un identificador de conexión.|[Función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|Devolver información acerca de un controlador o un origen de datos|[Función SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Liberar un identificador|[Función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Liberar un identificador de instrucción|[Función SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Ejecución asincrónica (método de sondeo)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
