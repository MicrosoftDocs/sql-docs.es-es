---
description: Función SQLCancelHandle
title: Función SQLCancelHandle | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SQLCancelHandle
helpviewer_keywords:
- SQLCancelHandle function [ODBC]
ms.assetid: 16049b5b-22a7-4640-9897-c25dd0f19d21
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 727f9c15809969560410cfa54bdf9b70ec765f53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181042"
---
# <a name="sqlcancelhandle-function"></a>Función SQLCancelHandle
**Conformidad**  
 Versión introducida: cumplimiento de estándares de ODBC 3,8: ninguno  
  
 Se espera que la mayoría de los controladores ODBC 3,8 (y posteriores) implementen esta función. Si un controlador no lo hace, una llamada a **SQLCancelHandle** con un identificador de conexión en el parámetro *Handle* devolverá SQL_ERROR con un SQLSTATE de IM001 y el mensaje ' driver ' no admite esta función ' ' una llamada a **SQLCancelHandle** con un identificador de instrucción, ya que el parámetro *Handle* se asignará a una llamada a **SQLCancel** por el administrador de controladores y se puede procesar si el controlador implementa **SQLCancel**. Una aplicación puede usar **SQLGetFunctions** para determinar si un controlador admite **SQLCancelHandle**.  
  
 **Resumen**  
 **SQLCancelHandle** cancela el procesamiento en una conexión o instrucción. El administrador de controladores asigna una llamada a **SQLCancelHandle** a una llamada a **SQLCancel** cuando *HandleType* está SQL_HANDLE_STMT.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLCancelHandle(  
      SQLSMALLINT  HandleType,  
      SQLHANDLE    Handle);  
```  
  
## <a name="arguments"></a>Argumentos  
 *HandleType*  
 Entradas Tipo del identificador en el que se va a cacel el procesamiento. Los valores válidos son SQL_HANDLE_DBC o SQL_HANDLE_STMT.  
  
 *Handle*  
 Entradas Identificador en el que se va a cancelar el procesamiento.  
  
 Si el *identificador* no es un identificador válido del tipo especificado por *HandleType*, **SQLCancelHandle** devuelve SQL_INVALID_HANDLE.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLCancelHandle** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de identificador de instrucción o un *HandleType* de SQL_HANDLE_DBC y un *identificador* de identificador de conexión.  
  
 En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLCancelHandle** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md) en el búfer del argumento *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|Se llamó a una función relacionada con instrucciones que se ejecuta de forma asincrónica para uno de los identificadores de instrucciones asociados al *identificador* y *HandleType* se estableció en SQL_HANDLE_DBC. La función asincrónica todavía se estaba ejecutando cuando se llamó a **SQLCancelHandle** .<br /><br /> (DM) el argumento *HandleType* se SQL_HANDLE_STMT; se llamó a una función que se ejecuta de forma asincrónica en el identificador de conexión asociado; y la función todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para uno de los identificadores de instrucciones asociados al *identificador* y *HandleType* se estableció en SQL_HANDLE_DBC y devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> Se llamó a **SQLBrowseConnect** para *ConnectionHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de que se completara el proceso de exploración.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY092|Identificador de opción/atributo no válido|*HandleType* se estableció en SQL_HANDLE_ENV o SQL_HANDLE_DESC.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado al *identificador* no admite la función.|  
  
 Si se llama a **SQLCancelHandle** con *HandleType* establecido en SQL_HANDLE_STMT, puede devolver cualquier SQLSTATE que pueda ser devuelto por la función **SQLCancel**.  
  
## <a name="comments"></a>Comentarios  
 Esta función es similar a **SQLCancel** , pero puede tomar una conexión o un identificador de instrucción como parámetro en lugar de solo un identificador de instrucción. El administrador de controladores asigna una llamada a **SQLCancelHandle** a una llamada a **SQLCancel** cuando *HandleType* está SQL_HANDLE_STMT. Esto permite a las aplicaciones usar **SQLCancelHandle** para cancelar las operaciones de instrucción incluso si el controlador no implementa **SQLCancelHandle**.  
  
 Para obtener más información sobre la cancelación de una operación de instrucción, vea [SQLCancel (función](../../../odbc/reference/syntax/sqlcancel-function.md)).  
  
 Si no hay ninguna operación en curso en el *control* , la llamada a **SQLCancelHandle** no tiene ningún efecto.  
  
 **SQLCancelHandle** en un identificador de conexión puede cancelar los siguientes tipos de procesamiento:  
  
-   Función que se ejecuta de forma asincrónica en la conexión.  
  
-   Función que se ejecuta en el identificador de conexión en otro subproceso.  
  
 Cuando se llama a **SQLCancelHandle** para cancelar una función que se ejecuta de forma asincrónica en una conexión, los registros de diagnóstico publicados por **SQLCancelHandle** se anexan a los devueltos por la operación que se está cancelando; No obstante, **SQLCancelHandle** no devuelve registros de diagnóstico cuando se cancela una función que se ejecuta en una conexión en otro subproceso.  
  
 El uso de **SQLCancelHandle** para cancelar **SQLEndTran** puede poner la conexión en estado suspendido. Para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).  
  
> [!NOTE]  
>  Para obtener información sobre cómo usar **SQLCancelHandle** en una aplicación que se implementará en un sistema operativo Windows anterior a Windows 7, consulte [matriz de compatibilidad](../../../odbc/reference/develop-app/compatibility-matrix.md).  
  
#### <a name="canceling-connection-related-asynchronous-processing"></a>Cancelar Connection-Related procesamiento asincrónico  
 Si una función devuelve SQL_STILL_EXECUTING, una aplicación puede llamar a **SQLCancelHandle** para cancelar la operación. Si la solicitud de cancelación se realiza correctamente, **SQLCancelHandle** devuelve SQL_SUCCESS. Esto no significa que se haya cancelado la función original. indica que se ha procesado la solicitud de cancelación. El controlador y el origen de datos determinan cuándo o si se cancela la operación. La aplicación debe seguir llamando a la función original hasta que no se SQL_STILL_EXECUTING el código de retorno. Si se canceló la función original, el código de retorno es SQL_ERROR y SQLSTATE HY008 (operación cancelada). Si la función original completó su procesamiento normal (no se canceló), el código de retorno es SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, o SQL_ERROR y un SQLSTATE distinto de HY008 (operación cancelada), si se produjo un error en la función original.  
  
#### <a name="canceling-functions-executing-on-another-thread"></a>Cancelar funciones que se ejecutan en otro subproceso  
 En una aplicación multiproceso, la aplicación puede cancelar una operación que se está ejecutando en otro subproceso. Para cancelar la operación, la aplicación llama a **SQLCancelHandle** con el identificador usado por la función, pero en un subproceso diferente. El controlador y el sistema operativo determinan cómo se cancela la operación. El código de retorno **SQLCancelHandle** indica si el controlador procesó la solicitud, devolviendo SQL_SUCCESS o SQL_ERROR (no se devuelve información de diagnóstico). Si se cancela el procesamiento de la función original, la función original devuelve SQL_ERROR y SQLSTATE HY008 (operación cancelada).  
  
 Si se ejecuta una función cuando se llama a **SQLCancelHandle** en otro subproceso para cancelar la función, es posible que la función se ejecute correctamente y devuelva SQL_SUCCESS antes de que la cancelación surta efecto. Una llamada a **SQLCancelHandle** no tiene ningún efecto si la operación se completó antes de que **SQLCancelHandle** pudiera cancelar la operación.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Cancelar una función que se ejecuta de forma asincrónica en un identificador de instrucción, cancelar una función en una instrucción que necesita datos o cancelar una función que se ejecuta en una instrucción en otro subproceso.|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Ejecución asincrónica (método de sondeo)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
