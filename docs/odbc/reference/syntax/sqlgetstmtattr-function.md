---
description: Función SQLGetStmtAttr
title: Función SQLGetStmtAttr | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetStmtAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetStmtAttr
helpviewer_keywords:
- SQLGetStmtAttr function [ODBC]
ms.assetid: e321d460-e997-4527-aee6-207cf5a498e9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b527d4a828ab1684977ced21a0509a7c75bf2041
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186185"
---
# <a name="sqlgetstmtattr-function"></a>Función SQLGetStmtAttr
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLGetStmtAttr** devuelve la configuración actual de un atributo de instrucción.  
  
> [!NOTE]  
>  Para obtener más información sobre lo que el administrador de controladores asigna a esta función cuando se trata de un ODBC 3. la aplicación *x* está trabajando con ODBC 2. controlador *x* , consulte [asignación de funciones de reemplazo para la compatibilidad con versiones anteriores de las aplicaciones](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetStmtAttr(  
     SQLHSTMT        StatementHandle,  
     SQLINTEGER      Attribute,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *Atributo*  
 Entradas Atributo que se va a recuperar.  
  
 *ValuePtr*  
 Genere Puntero a un búfer en el que se va a devolver el valor del atributo especificado en el *atributo*.  
  
 Si *ValuePtr* es null, *StringLengthPtr* devolverá el número total de bytes (sin incluir el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *ValuePtr*.  
  
 *BufferLength*  
 Entradas Si el *atributo* es un atributo definido por ODBC y *ValuePtr* apunta a una cadena de caracteres o a un búfer binario, este argumento debe ser la longitud de \* *ValuePtr*. Si el *atributo* es un atributo definido por ODBC y \* *ValuePtr* es un entero, *BufferLength* se omite. Si el valor devuelto en *\* ValuePtr* es una cadena Unicode (al llamar a **SQLGetStmtAttrW**), el argumento *BufferLength* debe ser un número par.  
  
 Si el *atributo* es un atributo definido por el controlador, la aplicación indica la naturaleza del atributo para el administrador de controladores mediante el establecimiento del argumento *BufferLength* . *BufferLength* puede tener los siguientes valores:  
  
-   Si *\* ValuePtr* es un puntero a una cadena de caracteres, *BufferLength* es la longitud de la cadena o SQL_NTS.  
  
-   Si *\* ValuePtr* es un puntero a un búfer binario, la aplicación coloca el resultado de la macro SQL_LEN_BINARY_ATTR (*length*) en *BufferLength*. Esto coloca un valor negativo en *BufferLength*.  
  
-   Si *\* ValuePtr* es un puntero a un valor distinto de una cadena de caracteres o una cadena binaria, *BufferLength* debe tener el valor SQL_IS_POINTER.  
  
-   Si *\* ValuePtr* contiene un tipo de datos de longitud fija, *BufferLength* es SQL_IS_INTEGER o SQL_IS_UINTEGER, según corresponda.  
  
 *StringLengthPtr*  
 Genere Un puntero a un búfer en el que se va a devolver el número total de bytes (sin incluir el carácter de terminación de NULL) disponible para devolver en *\* ValuePtr*. Si el valor del atributo es una cadena de caracteres y el número de bytes disponibles para devolver es mayor o igual que *BufferLength*, los datos de *\* ValuePtr* se truncan en *BufferLength* menos la longitud de un carácter de terminación NULL y el controlador termina en NULL.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetStmtAttr** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLGetStmtAttr** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|Los datos devueltos en *\* ValuePtr* se truncaron para *BufferLength* menos la longitud de un carácter de terminación null. La longitud del valor de cadena no truncado se devuelve en **StringLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|24000|Estado de cursor no válido|El *atributo* argument se SQL_ATTR_ROW_NUMBER y el cursor no estaba abierto o el cursor se colocó antes del inicio del conjunto de resultados o después del final del conjunto de resultados.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el argumento *MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLGetStmtAttr** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|*(DM) \* ValuePtr* es una cadena de caracteres y BufferLength era menor que cero, pero no es igual a SQL_NTS.|  
|HY092|Identificador de opción/atributo no válido|El valor especificado para el *atributo* argument no era válido para la versión de ODBC admitida por el controlador.|  
|HY109|Posición del cursor no válida|Se SQL_ATTR_ROW_NUMBER el argumento de *atributo* y se eliminó o no se pudo capturar la fila.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El valor especificado para el *atributo* argument era un atributo de instrucción ODBC válido para la versión de ODBC admitida por el controlador, pero no era compatible con el controlador.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador correspondiente a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Para obtener información general sobre los atributos de instrucción, vea [atributos de instrucción](../../../odbc/reference/develop-app/statement-attributes.md).  
  
 Una llamada a **SQLGetStmtAttr** devuelve en *\* ValuePtr* el valor del atributo de instrucción especificado en el *atributo*. Ese valor puede ser un valor SQLULEN o una cadena de caracteres terminada en NULL. Si el valor es un valor de SQLULEN, algunos controladores solo pueden escribir el menor 32 bits o 16 bits de un búfer y dejar el bit de orden superior sin cambios. Por lo tanto, las aplicaciones deben utilizar un búfer de SQLULEN e inicializar el valor en 0 antes de llamar a esta función. Además, no se usan los argumentos *BufferLength* y *StringLengthPtr* . Si el valor es una cadena terminada en null, la aplicación especifica la longitud máxima de esa cadena en el argumento *BufferLength* y el controlador devuelve la longitud de esa cadena en el búfer *\* StringLengthPtr* .  
  
 Para permitir que las aplicaciones que llaman a **SQLGetStmtAttr** funcionen con ODBC 2. Controladores *x* , una llamada a **SQLGetStmtAttr** se asigna en el administrador de controladores a **SQLGetStmtOption**.  
  
 Los siguientes atributos de instrucción son de solo lectura, por lo que se puede recuperar mediante **SQLGetStmtAttr**, pero no se establece mediante **SQLSetStmtAttr**:  
  
-   SQL_ATTR_IMP_PARAM_DESC  
  
-   SQL_ATTR_IMP_ROW_DESC  
  
-   SQL_ATTR_ROW_NUMBER  
  
 Para obtener una lista de los atributos que se pueden establecer y recuperar, vea [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Devolver el valor de un atributo de conexión|[Función SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Establecer un atributo de conexión|[Función SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Establecer un atributo de instrucción|[Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
