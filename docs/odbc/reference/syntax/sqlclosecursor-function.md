---
description: Función SQLCloseCursor
title: Función SQLCloseCursor | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCloseCursor
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLCloseCursor
helpviewer_keywords:
- SQLCloseCursor function [ODBC]
ms.assetid: 05b0a054-e28d-4e16-b5b0-07418486b372
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9ca47ce82c8daaf47f02f9cfb44e578285e837da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180993"
---
# <a name="sqlclosecursor-function"></a>Función SQLCloseCursor
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLCloseCursor** cierra un cursor que se ha abierto en una instrucción y descarta los resultados pendientes.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLCloseCursor(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLCloseCursor** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLCloseCursor** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|24000|Estado de cursor no válido|No hay ningún cursor abierto en el *StatementHandle*. (Solo lo devuelve un ODBC 3. controlador *x* ).|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión asociado a *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 **SQLCloseCursor** devuelve SQLSTATE 24000 (estado de cursor no válido) si no hay ningún cursor abierto. Llamar a **SQLCloseCursor** es equivalente a llamar a **SQLFreeStmt** con la opción SQL_CLOSE, con la excepción de que **SQLFreeStmt** con SQL_CLOSE no tiene ningún efecto en la aplicación si no hay ningún cursor abierto en la instrucción, mientras que **SQLCloseCursor** devuelve SQLSTATE 24000 (estado de cursor no válido).  
  
> [!NOTE]  
>  Si es ODBC 3. aplicación *x* que trabaja con ODBC 2. el controlador *x* llama a **SQLCloseCursor** cuando no hay ningún cursor abierto; SQLSTATE 24000 (estado de cursor no válido) no se devuelve, porque el administrador de controladores asigna **SQLCloseCursor** a **SQLFreeStmt** con SQL_CLOSE.  
  
 Para obtener más información, vea [cerrar el cursor](../../../odbc/reference/develop-app/closing-the-cursor.md).  
  
## <a name="code-example"></a>Ejemplo de código  
 Vea función [SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md) y [función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Liberar un identificador|[Función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Procesar varios conjuntos de resultados|[SQLMoreResults (función)](../../../odbc/reference/syntax/sqlmoreresults-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
