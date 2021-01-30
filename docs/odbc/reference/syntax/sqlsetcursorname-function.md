---
description: Función SQLSetCursorName
title: Función SQLSetCursorName | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetCursorName
helpviewer_keywords:
- SQLSetCursorName function [ODBC]
ms.assetid: 4e055946-12d4-4589-9891-41617a50f34e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2141a6ee259e51080d018af32bd0c433a1725e1e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208102"
---
# <a name="sqlsetcursorname-function"></a>Función SQLSetCursorName
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLSetCursorName** asocia un nombre de cursor a una instrucción activa. Si una aplicación no llama a **SQLSetCursorName**, el controlador genera nombres de cursor según sea necesario para el procesamiento de instrucciones SQL.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLSetCursorName(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CursorName,  
     SQLSMALLINT   NameLength);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *CursorName*  
 Entradas Nombre del cursor. Para un procesamiento eficaz, el nombre del cursor no debe incluir ningún espacio inicial o final en el nombre del cursor, y si el nombre del cursor incluye un identificador delimitado, el delimitador debe colocarse como el primer carácter del nombre del cursor.  
  
 *NameLength*  
 Entradas Longitud en caracteres de **CursorName*.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLSetCursorName** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLSetCursorName** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|El nombre del cursor superó el límite máximo, por lo que solo se usó el número máximo de caracteres permitido.|  
|24000|Estado de cursor no válido|La instrucción correspondiente a *StatementHandle* ya estaba en un estado ejecutado o situado en el cursor.|  
|34000|Nombre de cursor no válido|El nombre de cursor especificado en **CursorName* no era válido porque superó la longitud máxima tal como la define el controlador o comenzó con "SQLCUR" o "SQL_CUR".|  
|3C000|Nombre de cursor duplicado|El nombre de cursor especificado en **CursorName* ya existe.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY009|Uso no válido de puntero nulo|(DM) el argumento *CursorName* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función aynchronous todavía se estaba ejecutando cuando se llamó a la función **SQLSetCursorName** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el argumento *NameLength* era menor que 0 pero no es igual a SQL_NTS.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Los nombres de cursor solo se usan en las instrucciones Update y DELETE posicionadas (por ejemplo, **Update** _TABLE-Name_ ... **Donde Current of** _cursor-Name_). Para obtener más información, vea [instrucciones Update y DELETE posicionadas](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md). Si la aplicación no llama a **SQLSetCursorName** para definir un nombre de cursor, en la ejecución de una instrucción de consulta el controlador genera un nombre que comienza con las letras SQL_CUR y no supera los 18 caracteres de longitud.  
  
 Todos los nombres de cursor dentro de la conexión deben ser únicos. El controlador define la longitud máxima de un nombre de cursor. Para obtener la máxima interoperabilidad, se recomienda que las aplicaciones limiten los nombres de cursor a un máximo de 18 caracteres. En ODBC 3 *. x*, si un nombre de cursor es un identificador entre comillas, se trata de manera que distingue entre mayúsculas y minúsculas y puede contener caracteres que la sintaxis de SQL no permitiría o trataría especialmente, como espacios en blanco o palabras clave reservadas. Si un nombre de cursor se debe tratar de manera que distinga entre mayúsculas y minúsculas, se debe pasar como un identificador entre comillas.  
  
 Un nombre de cursor que se establece explícita o implícitamente permanece establecido hasta que se quita la instrucción a la que está asociada, mediante **SQLFreeHandle**. Se puede llamar a **SQLSetCursorName** para cambiar el nombre de un cursor en una instrucción siempre que el cursor esté en un estado asignado o preparado.  
  
## <a name="code-example"></a>Ejemplo de código  
 En el ejemplo siguiente, una aplicación usa **SQLSetCursorName** para establecer un nombre de cursor para una instrucción. A continuación, utiliza esa instrucción para recuperar los resultados de la tabla CUSTOMers. Por último, realiza una actualización posicionada para cambiar el número de teléfono de John Smith. Tenga en cuenta que la aplicación utiliza identificadores de instrucciones diferentes para las instrucciones **Select** y **Update** .  
  
 Para obtener otro ejemplo de código, vea [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp  
#define NAME_LEN 50  
#define PHONE_LEN 10  
  
SQLHSTMT     hstmtSelect,  
SQLHSTMT     hstmtUpdate;  
SQLRETURN    retcode;  
SQLHDBC      hdbc;  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   cbName, cbPhone;  
  
/* Allocate the statements and set the cursor name. */  
  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtSelect);  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtUpdate);  
SQLSetCursorName(hstmtSelect, "C1", SQL_NTS);  
  
/* SELECT the result set and bind its columns to local buffers. */  
  
SQLExecDirect(hstmtSelect,  
      "SELECT NAME, PHONE FROM CUSTOMERS",  
      SQL_NTS);  
SQLBindCol(hstmtSelect, 1, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
SQLBindCol(hstmtSelect, 2, SQL_C_CHAR, szPhone, PHONE_LEN, &cbPhone);  
  
/* Read through the result set until the cursor is */  
/* positioned on the row for John Smith. */  
  
do  
 retcode = SQLFetch(hstmtSelect);  
while ((retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) &&  
   (strcmp(szName, "Smith, John") != 0));  
  
/* Perform a positioned update of John Smith's name. */  
  
if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
   SQLExecDirect(hstmtUpdate,  
   "UPDATE EMPLOYEE SET PHONE=\"2064890154\" WHERE CURRENT OF C1",  
   SQL_NTS);  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Devolver un nombre de cursor|[Función SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|Establecer opciones de desplazamiento del cursor|[Función SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
