---
description: Función SQLFreeHandle
title: Función SQLFreeHandle | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFreeHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFreeHandle
helpviewer_keywords:
- SQLFreeHandle function [ODBC]
ms.assetid: 17a6fcdc-b05a-4de7-be93-a316f39696a1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 14c649c894250abdcac54010c0d18444d20b3baa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201566"
---
# <a name="sqlfreehandle-function"></a>Función SQLFreeHandle
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLFreeHandle** libera los recursos asociados a un entorno específico, una conexión, una instrucción o un identificador de descriptor.  
  
> [!NOTE]
>  Esta función es una función genérica para liberar identificadores. Reemplaza las funciones de ODBC 2,0 **SQLFreeConnect** (para liberar un identificador de conexión) y **SQLFreeEnv** (para liberar un identificador de entorno). **SQLFreeConnect** y **SQLFreeEnv** están en desuso en ODBC 3 *. x*. **SQLFreeHandle** también reemplaza la función **SQLFreeStmt** de ODBC 2,0 (con la *opción* SQL_DROP) para liberar un identificador de instrucción. Para obtener más información, vea "Comentarios". Para obtener más información sobre lo que el administrador de controladores asigna a esta función cuando una aplicación ODBC 3 *. x* está trabajando con un controlador ODBC 2 *. x* , consulte [asignación de funciones de reemplazo para mantener la compatibilidad con versiones anteriores de las aplicaciones](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLFreeHandle(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle);  
```  
  
## <a name="arguments"></a>Argumentos  
 *HandleType*  
 Entradas Tipo de identificador que va a ser liberado por **SQLFreeHandle**. Debe ser uno de los siguientes valores:   
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 El identificador de SQL_HANDLE_DBC_INFO_TOKEN solo lo utiliza el administrador de controladores y el controlador. Las aplicaciones no deben usar este tipo de identificador. Para obtener más información acerca de SQL_HANDLE_DBC_INFO_TOKEN, vea [desarrollar el reconocimiento de Connection-Pool en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 Si *HandleType* no es uno de estos valores, **SQLFreeHandle** devuelve SQL_INVALID_HANDLE.  
  
 *Handle*  
 Entradas Identificador que se va a liberar.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_ERROR o SQL_INVALID_HANDLE.  
  
 Si **SQLFreeHandle** devuelve SQL_ERROR, el identificador sigue siendo válido.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLFreeHandle** devuelve SQL_ERROR, se puede obtener un valor SQLSTATE asociado de la estructura de datos de diagnóstico para el identificador que **SQLFreeHandle** intentó liberar pero no se pudo. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLFreeHandle** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|(DM) el argumento *HandleType* se SQL_HANDLE_ENV y al menos una conexión se encontraba en un estado asignado o conectado. Se debe llamar a **SQLDisconnect** y **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_DBC para cada conexión antes de llamar a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_ENV.<br /><br /> (DM) el argumento *HandleType* se SQL_HANDLE_DBC y se llamó a la función antes de llamar a **SQLDisconnect** para la conexión.<br /><br /> (DM) el argumento *HandleType* se SQL_HANDLE_DBC. Se llamó a una función que se ejecuta de forma asincrónica con el *identificador* y la función todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> (DM) el argumento *HandleType* se SQL_HANDLE_STMT. Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** con el identificador de instrucción y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) el argumento *HandleType* se SQL_HANDLE_STMT. Se llamó a una función que se ejecuta de forma asincrónica en el identificador de instrucción o en el identificador de conexión asociado y la función todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> (DM) el argumento *HandleType* se SQL_HANDLE_DESC. Se llamó a una función que se ejecuta de forma asincrónica en el identificador de conexión asociado; y la función todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> (DM) no se liberaron todos los identificadores subsidiarios y otros recursos antes de que se llamara a **SQLFreeHandle** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para uno de los identificadores de instrucciones asociados al *identificador* y *HandleType* se estableció en SQL_HANDLE_STMT o SQL_HANDLE_DESC devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY013|Error de administración de memoria|El argumento *HandleType* se SQL_HANDLE_STMT o SQL_HANDLE_DESC y no se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY017|Uso no válido de un identificador de descriptor asignado automáticamente.|(DM) el argumento de *identificador* se estableció en el identificador de un descriptor asignado automáticamente.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el argumento *HandleType* se SQL_HANDLE_DESC y el controlador era un controlador ODBC 2 *. x* .<br /><br /> (DM) el argumento *HandleType* se SQL_HANDLE_STMT y el controlador no era un controlador ODBC válido.|  
  
## <a name="comments"></a>Comentarios  
 **SQLFreeHandle** se usa para liberar los identificadores de los entornos, las conexiones, las instrucciones y los descriptores, tal como se describe en las secciones siguientes. Para obtener información general acerca de los identificadores, vea [identificadores](../../../odbc/reference/develop-app/handles.md).  
  
 Una aplicación no debe utilizar un identificador una vez que se ha liberado; el administrador de controladores no comprueba la validez de un identificador en una llamada de función.  
  
## <a name="freeing-an-environment-handle"></a>Liberar un identificador de entorno  
 Antes de llamar a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_ENV, una aplicación debe llamar a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_DBC para todas las conexiones asignadas en el entorno. De lo contrario, la llamada a **SQLFreeHandle** devuelve SQL_ERROR y el entorno y todas las conexiones activas siguen siendo válidas. Para obtener más información, consulte [controladores de entorno](../../../odbc/reference/develop-app/environment-handles.md) y [asignación del identificador de entorno](../../../odbc/reference/develop-app/allocating-the-environment-handle.md).  
  
 Si el entorno es un entorno compartido, la aplicación que llama a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_ENV ya no tiene acceso al entorno después de la llamada, pero los recursos del entorno no se liberan necesariamente. La llamada a **SQLFreeHandle** disminuye el recuento de referencias del entorno. El administrador de controladores mantiene el recuento de referencias. Si no llega a cero, el entorno compartido no se libera, porque todavía lo está utilizando otro componente. Si el recuento de referencias llega a cero, se liberan los recursos del entorno compartido.  
  
## <a name="freeing-a-connection-handle"></a>Liberar un identificador de conexión  
 Antes de llamar a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_DBC, una aplicación debe llamar a **SQLDisconnect** para la conexión si hay una conexión en este controlador *.* De lo contrario, la llamada a **SQLFreeHandle** devuelve SQL_ERROR y la conexión sigue siendo válida.  
  
 Para obtener más información, vea [identificadores de conexión](../../../odbc/reference/develop-app/connection-handles.md) y [desconexión de un origen de datos o un controlador](../../../odbc/reference/develop-app/disconnecting-from-a-data-source-or-driver.md).  
  
## <a name="freeing-a-statement-handle"></a>Liberar un identificador de instrucción  
 Una llamada a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_STMT libera todos los recursos asignados mediante una llamada a **SQLAllocHandle** con un *HandleType* de SQL_HANDLE_STMT. Cuando una aplicación llama a **SQLFreeHandle** para liberar una instrucción que tiene resultados pendientes, se eliminan los resultados pendientes. Cuando una aplicación libera un identificador de instrucción, el controlador libera los cuatro descriptores asignados automáticamente asociados a ese identificador. Para obtener más información, vea [identificadores de instrucciones](../../../odbc/reference/develop-app/statement-handles.md) y [liberar un identificador de instrucción](../../../odbc/reference/develop-app/freeing-a-statement-handle-odbc.md).  
  
 Tenga en cuenta que **SQLDisconnect** quita automáticamente las instrucciones y los descriptores abiertos en la conexión.  
  
## <a name="freeing-a-descriptor-handle"></a>Liberar un identificador de descriptor  
 Una llamada a **SQLFreeHandle** con un *HandleType* de SQL_HANDLE_DESC libera el identificador del descriptor en el *identificador*. La llamada a **SQLFreeHandle** no libera ninguna memoria asignada por la aplicación a la que pueda hacer referencia un campo de puntero (incluidos SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR) de cualquier registro de descriptores de *identificador*. La memoria asignada por el controlador para los campos que no son campos de puntero se libera cuando se libera el identificador. Cuando se libera un identificador de descriptor asignado por el usuario, todas las instrucciones a las que se ha asociado el identificador liberado se revierten a sus correspondientes identificadores de descriptor asignados automáticamente.  
  
> [!NOTE]
>  Los controladores ODBC 2 *. x* no permiten liberar identificadores de descriptor, del mismo modo que no admiten la asignación de identificadores de descriptor.  
  
 Tenga en cuenta que **SQLDisconnect** quita automáticamente las instrucciones y los descriptores abiertos en la conexión. Cuando una aplicación libera un identificador de instrucción, el controlador libera todos los descriptores generados automáticamente asociados a ese identificador.  
  
 Para obtener más información acerca de los descriptores, consulte [descriptores](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="code-example"></a>Ejemplo de código  
 Para obtener ejemplos de código adicionales, vea [SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md) y [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
### <a name="code"></a>Código  
  
```cpp  
// SQLFreeHandle.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include <stdio.h>  
  
int main() {  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
  
   // Initialize the environment, connection, statement handles.  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   // Allocate the environment.  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set environment attributes.  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
  
   // Allocate the connection.  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
   // Set the login timeout.  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
  
   // Let the user select the data source and connect to the database.  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // Free handles, and disconnect.     
   if (hstmt) {   
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
      hstmt = NULL;   
   }  
   if (hdbc) {   
      SQLDisconnect(hdbc);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      hdbc = NULL;   
   }  
   if (henv) {   
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
      henv = NULL;   
   }  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de un identificador|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Cancelar el procesamiento de instrucciones|[SQLCance función)](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Establecer un nombre de cursor|[Función SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Programa de ejemplo de ODBC](../../../odbc/reference/sample-odbc-program.md)
