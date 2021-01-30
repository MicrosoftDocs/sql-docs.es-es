---
description: Función SQLGetCursorName
title: Función SQLGetCursorName | Microsoft Docs
ms.custom: ''
ms.date: 06/12/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetCursorName
helpviewer_keywords:
- SQLGetCursorName function [ODBC]
ms.assetid: e6e92199-7bb6-447c-8987-049a4c6ce05d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f413bb9fb5a11a9e09b932725219e47177aa0c2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199210"
---
# <a name="sqlgetcursorname-function"></a>Función SQLGetCursorName
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLGetCursorName** devuelve el nombre del cursor asociado a una instrucción especificada.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetCursorName(  
     SQLHSTMT        StatementHandle,  
     SQLCHAR *       CursorName,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   NameLengthPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *CursorName*  
 Genere Puntero a un búfer en el que se va a devolver el nombre del cursor.  
  
 Si *CursorName* es null, *NameLengthPtr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *CursorName*.  
  
 *BufferLength*  
 Entradas Longitud de \* *CursorName*, en caracteres. 
  
 *NameLengthPtr*  
 Genere Puntero a la memoria en la que se va a devolver el número total de caracteres (excepto el carácter de terminación null) disponible para devolver en \* *CursorName*. Si el número de caracteres disponibles para devolver es mayor o igual que *BufferLength*, el nombre del cursor en \* *CursorName* se trunca a *BufferLength* menos la longitud de un carácter de terminación null.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetCursorName** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLGetCursorName** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|El búfer \* *CursorName* no era lo suficientemente grande como para devolver el nombre completo del cursor, por lo que se truncó el nombre del cursor. La longitud del nombre de cursor sin truncar se devuelve en **NameLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLGetCursorName** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY015|No hay ningún nombre de cursor disponible|(DM) el controlador era un controlador ODBC 2 *. x* , no había ningún cursor abierto en la instrucción y no se ha establecido ningún nombre de cursor con **SQLSetCursorName**.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado en el argumento *BufferLength* era menor que 0.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Los nombres de cursor solo se usan en las instrucciones Update y DELETE posicionadas (por ejemplo, **Update** _TABLE-Name_ ... **Donde Current of** _cursor-Name_). Para obtener más información, vea [instrucciones Update y DELETE posicionadas](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md). Si la aplicación no llama a **SQLSetCursorName** para definir un nombre de cursor, el controlador genera un nombre. Este nombre comienza con las letras SQL_CUR.  
  
> [!NOTE]
>  En ODBC 2 *. x*, si no había ningún cursor abierto y no se ha establecido ningún nombre mediante una llamada a **SQLSetCursorName**, una llamada a **SQLGetCursorName** devolvió SQLSTATE HY015 (no hay ningún nombre de cursor disponible). En ODBC 3 *. x*, esto ya no es así. independientemente de Cuándo se llame a **SQLGetCursorName** , el controlador devuelve el nombre del cursor.  
  
 **SQLGetCursorName** devuelve el nombre de un cursor independientemente de si el nombre se creó o no de forma explícita o implícita. Un nombre de cursor se genera implícitamente si no se llama a **SQLSetCursorName** . Se puede llamar a **SQLSetCursorName** para cambiar el nombre de un cursor en una instrucción siempre que el cursor esté en un estado asignado o preparado.  
  
 Un nombre de cursor que se establece explícita o implícitamente permanece establecido hasta que se quita el *StatementHandle* con el que está asociado, mediante **SQLFreeHandle** con *HandleType* de SQL_HANDLE_STMT.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Preparar una instrucción para su ejecución|[Función SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Establecer un nombre de cursor|[Función SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
