---
description: Función SQLGetDescField
title: Función SQLGetDescField | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDescField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDescField
helpviewer_keywords:
- SQLGetDescField function [ODBC]
ms.assetid: f09ff660-1e4a-4370-be85-90d4da0487d3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0692c769921d8e0ee326b19f0b5d501367237c28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201945"
---
# <a name="sqlgetdescfield-function"></a>Función SQLGetDescField
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLGetDescField** devuelve la configuración actual o el valor de un solo campo de un registro de descriptor.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *DescriptorHandle*  
 Entradas Identificador del descriptor.  
  
 *RecNumber*  
 Entradas Indica el registro del descriptor del que la aplicación busca información. Los registros de descriptor se numeran a partir de 0, con el número de registro 0 como registro del marcador. Si el argumento *FieldIdentifier* indica un campo de encabezado, *RecNumber* se omite. Si *RecNumber* es menor o igual que SQL_DESC_COUNT pero la fila no contiene datos para una columna o parámetro, una llamada a **SQLGetDescField** devolverá los valores predeterminados de los campos. (Para obtener más información, vea "inicialización de campos de descriptor" en [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)).  
  
 *FieldIdentifier*  
 Entradas Indica el campo del descriptor cuyo valor se va a devolver. Para obtener más información, vea la sección sobre el argumento *FieldIdentifier* en [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 *ValuePtr*  
 Genere Puntero a un búfer en el que se va a devolver la información del descriptor. El tipo de datos depende del valor de *FieldIdentifier*.  
  
 Si *ValuePtr* es un tipo entero, las aplicaciones deben usar un búfer de SQLULEN e inicializar el valor en 0 antes de llamar a esta función, ya que algunos controladores solo pueden escribir el menor 32 bits o 16 bits de un búfer y dejar el bit de orden superior sin cambios.  
  
 Si *ValuePtr* es null, *StringLengthPtr* devolverá el número total de bytes (sin incluir el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *ValuePtr*.  
  
 *BufferLength*  
 Entradas Si *FieldIdentifier* es un campo definido por ODBC y *ValuePtr* apunta a una cadena de caracteres o a un búfer binario, este argumento debe ser la longitud de \* *ValuePtr*. Si *FieldIdentifier* es un campo definido por ODBC y \* *ValuePtr* es un entero, se omite *BufferLength* . Si el valor de *\* ValuePtr* es de un tipo de datos Unicode (al llamar a **SQLGetDescFieldW**), el argumento *BufferLength* debe ser un número par.  
  
 Si *FieldIdentifier* es un campo definido por el controlador, la aplicación indica la naturaleza del campo en el administrador de controladores estableciendo el argumento *BufferLength* . *BufferLength* puede tener los siguientes valores:  
  
-   Si *\* ValuePtr* es un puntero a una cadena de caracteres, *BufferLength* es la longitud de la cadena o SQL_NTS.  
  
-   Si *\* ValuePtr* es un puntero a un búfer binario, la aplicación coloca el resultado de la macro SQL_LEN_BINARY_ATTR (*length*) en *BufferLength*. Esto coloca un valor negativo en *BufferLength*.  
  
-   Si *\* ValuePtr* es un puntero a un valor distinto de una cadena de caracteres o una cadena binaria, *BufferLength* debe tener el valor SQL_IS_POINTER.  
  
-   Si *\* ValuePtr* contiene un tipo de datos de longitud fija, *BufferLength* es SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT o SQL_IS_USMALLINT, según corresponda.  
  
 *StringLengthPtr*  
 Genere Puntero al búfer en el que se va a devolver el número total de bytes (sin incluir el número de bytes necesario para el carácter de terminación null) disponible para devolver en **ValuePtr*.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_NO_DATA o SQL_INVALID_HANDLE.  
  
 SQL_NO_DATA se devuelve si *RecNumber* es mayor que el número actual de registros de descriptor.  
  
 SQL_NO_DATA se devuelve si *DescriptorHandle* es un identificador de IRD y la instrucción está en estado preparado o ejecutado, pero no hay ningún cursor abierto asociado a él.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetDescField** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLGetDescField** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|El búfer \* *ValuePtr* no era lo suficientemente grande como para devolver el campo de descriptor completo, por lo que el campo se truncó. La longitud del campo descriptor untruncado se devuelve en **StringLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07009|Índice de descriptor no válido|(DM) el argumento *RecNumber* era igual a 0, se SQL_UB_OFF el atributo de instrucción SQL_ATTR_USE_BOOKMARK y el argumento *DescriptorHandle* era un identificador IRD. (Este error se puede devolver para un descriptor asignado explícitamente solo si el descriptor está asociado a un identificador de instrucción.)<br /><br /> El argumento *FieldIdentifier* era un campo registro, el argumento *RecNumber* era 0 y el argumento *DescriptorHandle* era un identificador IPD.<br /><br /> El argumento *RecNumber* era menor que 0.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY007|La instrucción asociada no está preparada|*DescriptorHandle* se asoció con un *StatementHandle* como IRD y el identificador de instrucción asociado no se ha preparado ni ejecutado.|  
|HY010|Error de secuencia de función|(DM) *DescriptorHandle* se asoció con un *StatementHandle* para el que se llamó a una función que se ejecuta de forma asincrónica (no a este) y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> (DM) *DescriptorHandle* se asoció con un *StatementHandle* para el que se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** y se devolvía SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *DescriptorHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLGetDescField** .|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY021|Información de descriptor incoherente|Los campos SQL_DESC_TYPE y SQL_DESC_DATETIME_INTERVAL_CODE no forman un tipo SQL de ODBC válido, un tipo SQL específico del controlador válido (para IPD) o un tipo C de ODBC válido (para APD o ARDs).|  
|HY090|Longitud de búfer o cadena no válida|(DM) *\* ValuePtr* era una cadena de caracteres y *BufferLength* era menor que cero.|  
|HY091|Identificador de campo descriptor no válido|*FieldIdentifier* no era un campo definido por ODBC y no era un valor definido por la implementación.<br /><br /> *FieldIdentifier* no está definido para *DescriptorHandle*.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *DescriptorHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Una aplicación puede llamar a **SQLGetDescField** para devolver el valor de un solo campo de un registro del descriptor. Una llamada a **SQLGetDescField** puede devolver el valor de cualquier campo de cualquier tipo de descriptor, incluidos los campos de encabezado, los campos de registro y los campos de marcador. Una aplicación puede obtener la configuración de varios campos en el mismo descriptor o en uno diferente, en orden arbitrario, realizando llamadas repetidas a **SQLGetDescField**. También se puede llamar a **SQLGetDescField** para devolver los campos de descriptor definidos por el controlador.  
  
 Por motivos de rendimiento, una aplicación no debe llamar a **SQLGetDescField** para IRD antes de ejecutar una instrucción.  
  
 La configuración de varios campos que describen el nombre, el tipo de datos y el almacenamiento de datos de columna o parámetro también se puede recuperar en una única llamada a **SQLGetDescRec**. Se puede llamar a **SQLGetStmtAttr** para devolver la configuración de un único campo en el encabezado del descriptor que también es un atributo de instrucción. **SQLColAttribute**, **SQLDescribeCol** y **SQLDescribeParam** devuelven los campos de registro o marcador.  
  
 Cuando una aplicación llama a **SQLGetDescField** para recuperar el valor de un campo que no está definido para un tipo de descriptor determinado, la función devuelve SQL_SUCCESS pero el valor devuelto para el campo es indefinido. Por ejemplo, si se llama a **SQLGetDescField** para el campo SQL_DESC_NAME o SQL_DESC_NULLABLE de un APD o ARD, se devolverá SQL_SUCCESS pero un valor no definido para el campo.  
  
 Cuando una aplicación llama a **SQLGetDescField** para recuperar el valor de un campo definido para un tipo de descriptor determinado pero que no tiene ningún valor predeterminado y aún no se ha establecido, la función devuelve SQL_SUCCESS pero el valor devuelto para el campo es indefinido. Para obtener más información sobre la inicialización de campos de descriptor y descripciones de los campos, vea "inicialización de campos de descriptor" en [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Para obtener más información acerca de los descriptores, consulte [descriptores](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Obtener varios campos de descriptor|[Función SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Establecer un solo campo de descriptor|[Función SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Establecer varios campos de descriptor|[Función SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
