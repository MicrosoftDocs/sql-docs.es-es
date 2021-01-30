---
description: Función SQLGetDescRec
title: Función SQLGetDescRec | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDescRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDescRec
helpviewer_keywords:
- SQLGetDescRec function [ODBC]
ms.assetid: 325e0907-8e87-44e8-a111-f39e636a9cbc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a340ecc75e7ae4f03c44051f594fda7305a2a00f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205636"
---
# <a name="sqlgetdescrec-function"></a>Función SQLGetDescRec
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLGetDescRec** devuelve la configuración actual o los valores de varios campos de un registro de descriptor. Los campos devueltos describen el nombre, el tipo de datos y el almacenamiento de datos de columna o parámetro.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLGetDescRec(  
      SQLHDESC        DescriptorHandle,  
      SQLSMALLINT     RecNumber,  
      SQLCHAR *       Name,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLSMALLINT *   TypePtr,  
      SQLSMALLINT *   SubTypePtr,  
      SQLLEN *        LengthPtr,  
      SQLSMALLINT *   PrecisionPtr,  
      SQLSMALLINT *   ScalePtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *DescriptorHandle*  
 Entradas Identificador del descriptor.  
  
 *RecNumber*  
 Entradas Indica el registro del descriptor del que la aplicación busca información. Los registros de descriptor se numeran a partir de 1, donde el número de registro 0 es el registro del marcador. El argumento *RecNumber* debe ser menor o igual que el valor de SQL_DESC_COUNT. Si *RecNumber* es menor o igual que SQL_DESC_COUNT pero la fila no contiene datos para una columna o parámetro, una llamada a **SQLGetDescRec** devolverá los valores predeterminados de los campos. (Para obtener más información, vea "inicialización de campos de descriptor" en [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)).  
  
 *Nombre*  
 Genere Puntero a un búfer en el que se va a devolver el SQL_DESC_NAME campo del registro del descriptor.  
  
 Si *Name* es null, *StringLengthPtr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *el nombre*.  
  
 *BufferLength*  
 Entradas Longitud del búfer de *nombres* *, en caracteres.  
  
 *StringLengthPtr*  
 Genere Un puntero a un búfer en el que se va a devolver el número de caracteres de datos disponibles que se van a devolver en el búfer de \* *nombres* , excluido el carácter de terminación null. Si el número de caracteres es mayor o igual que *BufferLength*, los datos de \* *Name* se truncan en *BufferLength* menos la longitud de un carácter de terminación NULL y el controlador termina en NULL.  
  
 *TypePtr*  
 Genere Puntero a un búfer en el que se va a devolver el valor del campo SQL_DESC_TYPE del registro del descriptor.  
  
 *SubTypePtr*  
 Genere En el caso de los registros cuyo tipo es SQL_DATETIME o SQL_INTERVAL, es un puntero a un búfer en el que se devuelve el valor del campo SQL_DESC_DATETIME_INTERVAL_CODE.  
  
 *LengthPtr*  
 Genere Puntero a un búfer en el que se va a devolver el valor del campo SQL_DESC_OCTET_LENGTH del registro del descriptor.  
  
 *PrecisionPtr*  
 Genere Puntero a un búfer en el que se va a devolver el valor del campo SQL_DESC_PRECISION del registro del descriptor.  
  
 *ScalePtr*  
 Genere Puntero a un búfer en el que se va a devolver el valor del campo SQL_DESC_SCALE del registro del descriptor.  
  
 *NullablePtr*  
 Genere Puntero a un búfer en el que se va a devolver el valor del campo SQL_DESC_NULLABLE del registro del descriptor.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_NO_DATA o SQL_INVALID_HANDLE.  
  
 SQL_NO_DATA se devuelve si *RecNumber* es mayor que el número actual de registros de descriptor.  
  
 SQL_NO_DATA se devuelve si *DescriptorHandle* es un identificador de IRD y la instrucción está en estado preparado o ejecutado, pero no hay ningún cursor abierto asociado a él.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLGetDescRec** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DESC y un *identificador* de *DescriptorHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLGetDescRec** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|El \* *nombre* de búfer no era lo suficientemente grande como para devolver el campo de descriptor completo. Por lo tanto, el campo se truncó. La longitud del campo descriptor untruncado se devuelve en **StringLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07009|Índice de descriptor no válido|El argumento *FieldIdentifier* era un campo registro, el argumento *RecNumber* se estableció en 0 y el argumento *DescriptorHandle* era un identificador IPD.<br /><br /> (DM) el argumento *RecNumber* se estableció en 0 y el atributo instrucción SQL_ATTR_USE_BOOKMARKS se estableció en SQL_UB_OFF, y el argumento *DescriptorHandle* era un identificador IRD.<br /><br /> El argumento *RecNumber* era menor que 0.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY007|La instrucción asociada no está preparada|*DescriptorHandle* se asoció con un IRD y el identificador de instrucción asociado no estaba en el estado preparado o ejecutado.|  
|HY010|Error de secuencia de función|(DM) *DescriptorHandle* se asoció con un *StatementHandle* para el que se llamó a una función que se ejecuta de forma asincrónica (no a este) y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> (DM) *DescriptorHandle* se asoció con un *StatementHandle* para el que se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** y se devolvía SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *DescriptorHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a **SQLGetDescRec** .|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *DescriptorHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Una aplicación puede llamar a **SQLGetDescRec** para recuperar los valores de los siguientes campos de descriptor para una sola columna o parámetro:  
  
-   SQL_DESC_NAME  
  
-   SQL_DESC_TYPE  
  
-   SQL_DESC_DATETIME_INTERVAL_CODE (para registros cuyo tipo es SQL_DATETIME o SQL_INTERVAL)  
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_NULLABLE  
  
 **SQLGetDescRec** no recupera los valores de los campos de encabezado.  
  
 Una aplicación puede impedir que se devuelva el valor de un campo estableciendo el argumento que corresponde al campo en un puntero nulo.  
  
 Cuando una aplicación llama a **SQLGetDescRec** para recuperar el valor de un campo que no está definido para un tipo de descriptor determinado, la función devuelve SQL_SUCCESS pero el valor devuelto para el campo es indefinido. Por ejemplo, si se llama a **SQLGetDescRec** para el campo SQL_DESC_NAME o SQL_DESC_NULLABLE de un APD o ARD, se devolverá SQL_SUCCESS pero un valor no definido para el campo.  
  
 Cuando una aplicación llama a **SQLGetDescRec** para recuperar el valor de un campo definido para un tipo de descriptor determinado pero que no tiene ningún valor predeterminado y aún no se ha establecido, la función devuelve SQL_SUCCESS pero el valor devuelto para el campo es indefinido. Para obtener más información, vea "inicialización de campos de descriptor" en [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Los valores de los campos también se pueden recuperar individualmente mediante una llamada a **SQLGetDescField**. Para obtener una descripción de los campos de un registro o encabezado de descriptor, vea [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Para obtener más información acerca de los descriptores, consulte [descriptores](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar una columna|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Enlazar un parámetro|[Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Obtención de un campo de descriptor|[Función SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Establecer varios campos de descriptor|[Función SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
