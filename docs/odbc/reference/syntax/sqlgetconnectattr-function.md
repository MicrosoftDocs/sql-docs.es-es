---
description: Función SQLGetConnectAttr
title: Función SQLGetConnectAttr | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConnectAttr
helpviewer_keywords:
- SQLGetConnectAttr function [ODBC]
ms.assetid: 2cb4ffa8-19d3-4664-8c2f-6682cdcc3f33
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 649e2dbf37770a94aae89ee43736f30e31a17573
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203458"
---
# <a name="sqlgetconnectattr-function"></a>Función SQLGetConnectAttr
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLGetConnectAttr** devuelve la configuración actual de un atributo de conexión.  
  
> [!NOTE]
>  Para obtener más información sobre lo que el administrador de controladores asigna a esta función cuando una aplicación ODBC 3 *. x* está trabajando con un controlador ODBC 2 *. x* , consulte [asignación de funciones de reemplazo para mantener la compatibilidad con versiones anteriores de las aplicaciones](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetConnectAttr(  
     SQLHDBC        ConnectionHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *ConnectionHandle*  
 [Entrada] Identificador de conexión.  
  
 *Atributo*  
 Entradas Atributo que se va a recuperar.  
  
 *ValuePtr*  
 Genere Puntero a la memoria en la que se va a devolver el valor actual del atributo especificado por el *atributo*. En el caso de los atributos de tipo entero, algunos controladores solo pueden escribir el menor 32 bits o 16 bits de un búfer y dejar el bit de orden superior sin cambios. Por lo tanto, las aplicaciones deben utilizar un búfer de SQLULEN e inicializar el valor en 0 antes de llamar a esta función.  
  
 Si *ValuePtr* es null, *StringLengthPtr* devolverá el número total de bytes (sin incluir el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *ValuePtr*.  
  
 *BufferLength*  
 Entradas Si el *atributo* es un atributo definido por ODBC y *ValuePtr* apunta a una cadena de caracteres o a un búfer binario, este argumento debe ser la longitud de \* *ValuePtr*. Si el *atributo* es un atributo definido por ODBC y *\* ValuePtr* es un entero, *BufferLength* se omite. Si el valor de *\* ValuePtr* es una cadena Unicode (al llamar a **SQLGetConnectAttrW**), el argumento *BufferLength* debe ser un número par.  
  
 Si el *atributo* es un atributo definido por el controlador, la aplicación indica la naturaleza del atributo para el administrador de controladores mediante el establecimiento del argumento *BufferLength* . *BufferLength* puede tener los siguientes valores:  
  
-   Si *\* ValuePtr* es un puntero a una cadena de caracteres, *BufferLength* es la longitud de la cadena.  
  
-   Si *\* ValuePtr* es un puntero a un búfer binario, la aplicación coloca el resultado de la macro SQL_LEN_BINARY_ATTR (*length*) en *BufferLength*. Esto coloca un valor negativo en *BufferLength*.  
  
-   Si *\* ValuePtr* es un puntero a un valor distinto de una cadena de caracteres o una cadena binaria, *BufferLength* debe tener el valor SQL_IS_POINTER.  
  
-   Si *\* ValuePtr* contiene un tipo de datos de longitud fija, *BufferLength* es SQL_IS_INTEGER o SQL_IS_UINTEGER, según corresponda.  
  
 *StringLengthPtr*  
 Genere Un puntero a un búfer en el que se va a devolver el número total de bytes (sin incluir el carácter de terminación de NULL) disponible para devolver en \* *ValuePtr*. Si el valor del atributo es una cadena de caracteres y el número de bytes disponibles para devolver es mayor que *BufferLength* menos la longitud del carácter de terminación null, los datos de *\* ValuePtr* se truncan en *BufferLength* menos la longitud del carácter de terminación NULL y el controlador termina en NULL.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetConnectAttr** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado de la estructura de datos de diagnóstico mediante una llamada a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DBC y un *identificador* de *ConnectionHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLGetConnectAttr** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|Los datos devueltos en \* *ValuePtr* se truncaron para *BufferLength* menos la longitud de un carácter de terminación null. La longitud del valor de cadena no truncado se devuelve en *\* StringLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08003|Conexión no abierta|(DM) se especificó un valor de *atributo* que requería una conexión abierta.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por el argumento *MessageText* en **SQLGetDiagField** de la estructura de datos de diagnóstico describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|Se llamó a (DM) **SQLBrowseConnect** para *ConnectionHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de que **SQLBrowseConnect** devolviera SQL_SUCCESS_WITH_INFO o SQL_SUCCESS.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *ConnectionHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) *\* ValuePtr* es una cadena de caracteres y BufferLength era menor que cero, pero no es igual a SQL_NTS.|  
|HY092|Identificador de opción/atributo no válido|El valor especificado para el *atributo* argument no era válido para la versión de ODBC admitida por el controlador.|  
|HY114|El controlador no admite la ejecución de funciones asincrónicas en el nivel de conexión|(DM) una aplicación intentó habilitar la ejecución de función asincrónica con SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE para un controlador que no admite operaciones de conexión asincrónicas.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El valor especificado para el *atributo* argument era un atributo de conexión ODBC válido para la versión de ODBC admitida por el controlador, pero no era compatible con el controlador.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador que corresponde a *ConnectionHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Para obtener información general sobre los atributos de conexión, consulte [atributos de conexión](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Para obtener una lista de los atributos que se pueden establecer, consulte [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md). Observe que si el *atributo* especifica un atributo que devuelve una cadena, *ValuePtr* debe ser un puntero a un búfer para la cadena. La longitud máxima de la cadena devuelta, incluido el carácter de terminación de NULL, será *BufferLength* bytes.  
  
 En función del atributo, una aplicación no tiene que establecer una conexión antes de llamar a **SQLGetConnectAttr**. Sin embargo, si se llama a **SQLGetConnectAttr** y el atributo especificado no tiene un valor predeterminado y no se ha establecido mediante una llamada anterior a **SQLSetConnectAttr**, **SQLGetConnectAttr** devolverá SQL_NO_DATA.  
  
 Si el *atributo* es SQL_ATTR_ TRACE o SQL_ATTR_ de seguimiento, *ConnectionHandle* no tiene que ser válido y **SQLGetConnectAttr** no devolverá SQL_ERROR o SQL_INVALID_HANDLE si *ConnectionHandle* no es válido. Estos atributos se aplican a todas las conexiones. **SQLGetConnectAttr** devolverá SQL_ERROR o SQL_INVALID_HANDLE si otro argumento no es válido.  
  
 Aunque una aplicación puede establecer atributos de instrucción mediante **SQLSetConnectAttr**, una aplicación no puede usar **SQLGetConnectAttr** para recuperar los valores de atributo de instrucción; debe llamar a **SQLGetStmtAttr** para recuperar la configuración de los atributos de instrucción.  
  
 Los atributos de conexión SQL_ATTR_AUTO_IPD y SQL_ATTR_CONNECTION_DEAD pueden ser devueltos por una llamada a **SQLGetConnectAttr** pero no se pueden establecer mediante una llamada a **SQLSetConnectAttr**.  
  
> [!NOTE]  
>  No hay compatibilidad asincrónica con **SQLGetConnectAttr**. Al implementar **SQLGetConnectAttr**, un controlador puede mejorar el rendimiento al minimizar el número de veces que la información se envía o solicita desde el servidor.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Devolver el valor de un atributo de instrucción|[Función SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Establecer un atributo de conexión|[Función SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Establecer un atributo de entorno|[Función SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Establecer un atributo de instrucción|[Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
