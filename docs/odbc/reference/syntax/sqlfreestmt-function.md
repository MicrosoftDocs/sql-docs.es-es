---
description: Función SQLFreeStmt
title: Función SQLFreeStmt | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFreeStmt
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFreeStmt
helpviewer_keywords:
- SQLFreeStmt function [ODBC]
ms.assetid: 03408162-8b63-4470-90c4-e6c7d8d33892
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0dcab0413f353b4c600964dd38818df37f86bfe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201562"
---
# <a name="sqlfreestmt-function"></a>Función SQLFreeStmt
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLFreeStmt** detiene el procesamiento asociado con una instrucción específica, cierra cualquier cursor abierto asociado a la instrucción, descarta los resultados pendientes o, opcionalmente, libera todos los recursos asociados al identificador de instrucción.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLFreeStmt(  
     SQLHSTMT       StatementHandle,  
     SQLUSMALLINT   Option);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción  
  
 *Opción*  
 Entradas Una de las siguientes opciones:  
  
 SQL_ CLOSE: cierra el cursor asociado a *StatementHandle* (si se definió uno) y descarta todos los resultados pendientes. La aplicación puede volver a abrir este cursor más adelante mediante la ejecución de una instrucción **Select** de nuevo con los mismos valores de parámetro o distintos. Si no hay ningún cursor abierto, esta opción no tiene ningún efecto en la aplicación. También se puede llamar a **SQLCloseCursor** para cerrar un cursor. Para obtener más información, vea [cerrar el cursor](../../../odbc/reference/develop-app/closing-the-cursor.md).  
  
 SQL_DROP: esta opción está en desuso. Una llamada a **SQLFreeStmt** con una *opción* de SQL_DROP está asignada en el administrador de controladores a [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
 SQL_UNBIND: establece el campo de SQL_DESC_COUNT del ARD en 0, liberando todos los búferes de columna enlazados por **SQLBindCol** para el *StatementHandle* determinado. Esto no desenlaza la columna de marcador; para ello, el campo de SQL_DESC_DATA_PTR de ARD para la columna de marcador se establece en NULL. Tenga en cuenta que si esta operación se realiza en un descriptor asignado explícitamente compartido por más de una instrucción, la operación afectará a los enlaces de todas las instrucciones que compartan el descriptor. Para obtener más información, vea [información general sobre la recuperación de resultados (Basic)](../../../odbc/reference/develop-app/retrieving-results-basic.md).  
  
 SQL_RESET_PARAMS: establece el campo de SQL_DESC_COUNT del APD en 0, liberando todos los búferes de parámetros establecidos por **SQLBindParameter** para el *StatementHandle* determinado. Si esta operación se realiza en un descriptor asignado explícitamente compartido por más de una instrucción, esta operación afectará a los enlaces de todas las instrucciones que compartan el descriptor. Para obtener más información, vea [Binding Parameters](../../../odbc/reference/develop-app/binding-parameters-odbc.md).  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLFreeStmt** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLFreeStmt** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a **SQLFreeStmt** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función con la *opción* establecida en SQL_RESET_PARAMS antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY092|Tipo de opción fuera del intervalo|(DM) el valor especificado para la *opción* argument no era:<br /><br /> SQL_CLOSE SQL_DROP SQL_UNBIND SQL_RESET_PARAMS|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Llamar a **SQLFreeStmt** con la opción SQL_CLOSE es equivalente a llamar a **SQLCloseCursor**, salvo que **SQLFreeStmt** con SQL_CLOSE no afecta a la aplicación si no hay ningún cursor abierto en la instrucción. Si no hay ningún cursor abierto, una llamada a **SQLCloseCursor** devuelve SQLSTATE 24000 (estado de cursor no válido).  
  
 Una aplicación no debe utilizar un identificador de instrucción una vez que se ha liberado; el administrador de controladores no comprueba la validez de un identificador en una llamada de función.  
  
## <a name="example"></a>Ejemplo  
 Es una buena práctica de programación para liberar identificadores. Sin embargo, para simplificar, el ejemplo siguiente no incluye el código que libera los identificadores asignados. Para obtener un ejemplo de cómo liberar identificadores, vea [función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
```cpp  
// SQLFreeStmt.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   // declare and initialize the environment, connection, statement handles  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1024 + 1, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
   retCode = SQLFreeStmt(hstmt, SQL_UNBIND);  
   retCode = SQLFreeStmt(hstmt, SQL_RESET_PARAMS);  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Asignación de un identificador|[Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Cerrar un cursor|[Función SQLCloseCursor](../../../odbc/reference/syntax/sqlclosecursor-function.md)|  
|Liberar un identificador|[Función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Establecer un nombre de cursor|[Función SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
