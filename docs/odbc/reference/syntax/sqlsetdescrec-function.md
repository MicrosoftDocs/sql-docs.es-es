---
description: Función SQLSetDescRec
title: Función SQLSetDescRec | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetDescRec
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescRec
helpviewer_keywords:
- SQLSetDescRec function [ODBC]
ms.assetid: bf55256c-7eb7-4e3f-97ef-b0fee09ba829
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 65e0a789f38ad9f0d1d4b624caca896d01356ce2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160184"
---
# <a name="sqlsetdescrec-function"></a>Función SQLSetDescRec
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 La función **SQLSetDescRec** establece varios campos de descriptor que afectan al tipo de datos y al búfer enlazados a los datos de una columna o parámetro.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLSetDescRec(  
      SQLHDESC      DescriptorHandle,  
      SQLSMALLINT   RecNumber,  
      SQLSMALLINT   Type,  
      SQLSMALLINT   SubType,  
      SQLLEN        Length,  
      SQLSMALLINT   Precision,  
      SQLSMALLINT   Scale,  
      SQLPOINTER    DataPtr,  
      SQLLEN *      StringLengthPtr,  
      SQLLEN *      IndicatorPtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *DescriptorHandle*  
 Entradas Identificador del descriptor. No debe ser un identificador IRD.  
  
 *RecNumber*  
 Entradas Indica el registro del descriptor que contiene los campos que se van a establecer. Los registros de descriptor se numeran a partir de 0, con el número de registro 0 como registro del marcador. Este argumento debe ser igual o mayor que 0. Si *RecNumber* es mayor que el valor de SQL_DESC_COUNT, SQL_DESC_COUNTis cambiar al valor de *RecNumber*.  
  
 *Type*  
 Entradas Valor en el que se va a establecer el SQL_DESC_TYPE campo del registro del descriptor.  
  
 *Subtipo*  
 Entradas En el caso de los registros cuyo tipo es SQL_DATETIME o SQL_INTERVAL, es el valor en el que se va a establecer el campo de SQL_DESC_DATETIME_INTERVAL_CODE.  
  
 *Duración*  
 Entradas Valor en el que se va a establecer el SQL_DESC_OCTET_LENGTH campo del registro del descriptor.  
  
 *Precisión*  
 Entradas Valor en el que se va a establecer el SQL_DESC_PRECISION campo del registro del descriptor.  
  
 *Escalar*  
 Entradas Valor en el que se va a establecer el SQL_DESC_SCALE campo del registro del descriptor.  
  
 *DataPtr*  
 [Entrada o salida diferida] Valor en el que se va a establecer el SQL_DESC_DATA_PTR campo del registro del descriptor. *DataPtr* se puede establecer en un puntero nulo.  
  
 El argumento *DataPtr* se puede establecer en un puntero nulo para establecer el SQL_DESC_DATA_PTR campo en un puntero nulo. Si el identificador del argumento *DescriptorHandle* está asociado a un ARD, se desenlaza la columna.  
  
 *StringLengthPtr*  
 [Entrada o salida diferida] Valor en el que se va a establecer el SQL_DESC_OCTET_LENGTH_PTR campo del registro del descriptor. *StringLengthPtr* se puede establecer en un puntero nulo para establecer el SQL_DESC_OCTET_LENGTH_PTR campo en un puntero nulo.  
  
 *IndicatorPtr*  
 [Entrada o salida diferida] Valor en el que se va a establecer el SQL_DESC_INDICATOR_PTR campo del registro del descriptor. *IndicatorPtr* se puede establecer en un puntero nulo para establecer el SQL_DESC_INDICATOR_PTR campo en un puntero nulo.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLSetDescRec** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DESC y un *identificador* de *DescriptorHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLSetDescRec** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07009|Índice de descriptor no válido|El argumento *RecNumber* se estableció en 0 y *DescriptorHandle* hacía referencia a un identificador IPD.<br /><br /> El argumento *RecNumber* era menor que 0.<br /><br /> El argumento *RecNumber* era mayor que el número máximo de columnas o parámetros que el origen de datos puede admitir y el argumento *DescriptorHandle* era APD, IPD o ARD.<br /><br /> El argumento *RecNumber* era igual a 0 y el argumento *DescriptorHandle* hacía referencia a un APD asignado implícitamente. (Este error no se produce con un descriptor de aplicación asignado explícitamente porque no se sabe si un descriptor de aplicación asignado explícitamente es un APD o ARD hasta el tiempo de ejecución).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|(DM) el *DescriptorHandle* estaba asociado a un *StatementHandle* para el que se llamó a una función que se ejecuta de forma asincrónica (no a este) y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para el *StatementHandle* con el que se asoció *DescriptorHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *DescriptorHandle*. Esta función aynchronous todavía se estaba ejecutando cuando se llamó a la función **SQLSetDescRec** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para uno de los identificadores de instrucciones asociados a *DescriptorHandle* y se devolvieron SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY016|No se puede modificar un descriptor de fila de implementación|El argumento *DescriptorHandle* estaba asociado a IRD.|  
|HY021|Información de descriptor incoherente|El campo de *tipo* , o cualquier otro campo asociado al campo SQL_DESC_TYPE del descriptor, no era válido o era coherente.<br /><br /> La información del descriptor comprobada durante una comprobación de coherencia no era coherente. (Vea "comprobaciones de coherencia" más adelante en esta sección).|  
|HY090|Longitud de búfer o cadena no válida|(DM) el controlador era un controlador ODBC *2. x* , el descriptor era ARD, el argumento *ColumnNumber* se estableció en 0 y el valor especificado para el argumento *BufferLength* no era igual a 4.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *DescriptorHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Una aplicación puede llamar a **SQLSetDescRec** para establecer los campos siguientes para una sola columna o parámetro:  
  
-   SQL_DESC_TYPE  
  
-   SQL_DESC_DATETIME_INTERVAL_CODE (para registros cuyo tipo es SQL_DATETIME o SQL_INTERVAL)  
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_DATA_PTR  
  
-   SQL_DESC_OCTET_LENGTH_PTR  
  
-   SQL_DESC_INDICATOR_PTR  
  
> [!NOTE]  
>  Si se produce un error en una llamada a **SQLSetDescRec** , el contenido del registro del descriptor identificado por el argumento *RecNumber* es indefinido.  
  
 Al enlazar una columna o un parámetro, **SQLSetDescRec** permite cambiar varios campos que afectan al enlace sin llamar a **SQLBindCol** o **SQLBindParameter** , o realizar varias llamadas a **SQLSetDescField**. **SQLSetDescRec** puede establecer campos en un descriptor no asociado actualmente a una instrucción. Tenga en cuenta que **SQLBindParameter** establece más campos que **SQLSetDescRec**, puede establecer campos en APD y en IPD en una llamada, y no requiere un identificador de descriptor.  
  
> [!NOTE]  
>  El atributo Statement SQL_ATTR_USE_BOOKMARKS debe establecerse siempre antes de llamar a **SQLSetDescRec** con un argumento *RecNumber* de 0 para establecer los campos de marcador. Aunque esto no es obligatorio, se recomienda encarecidamente.  
  
## <a name="consistency-checks"></a>Comprobaciones de coherencia  
 El controlador realiza automáticamente una comprobación de coherencia cada vez que una aplicación establece el campo de SQL_DESC_DATA_PTR de una APD, ARD o IPD. Si alguno de los campos no es coherente con otros campos, **SQLSetDescRec** devolverá SQLSTATE HY021 (información del descriptor incoherente).  
  
 Cada vez que una aplicación establece el campo de SQL_DESC_DATA_PTR de una APD, ARD o IPD, el controlador comprueba que el valor del campo SQL_DESC_TYPE y los valores aplicables a ese campo SQL_DESC_TYPE sean válidos y coherentes. Esta comprobación siempre se realiza cuando se llama a **SQLBindParameter** o **SQLBindCol** o cuando se llama a **SQLSetDescRec** para APD, ARD o IPD. Esta comprobación de coherencia incluye las siguientes comprobaciones en los campos de descriptor:  
  
-   El campo de SQL_DESC_TYPE debe ser uno de los tipos de ODBC C o SQL válidos o un tipo SQL específico del controlador. El campo de SQL_DESC_CONCISE_TYPE debe ser uno de los tipos de ODBC C o SQL válidos o un tipo de C o SQL específico del controlador, incluidos los tipos de intervalo y de fecha y hora concisos.  
  
-   Si el SQL_DESC_TYPE campo registro es SQL_DATETIME o SQL_INTERVAL, el campo SQL_DESC_DATETIME_INTERVAL_CODE debe ser uno de los códigos de fecha y hora válidos. (Vea la descripción del campo SQL_DESC_DATETIME_INTERVAL_CODE en [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)).  
  
-   Si el campo SQL_DESC_TYPE indica un tipo numérico, los campos SQL_DESC_PRECISION y SQL_DESC_SCALE se comprueban como válidos.  
  
-   Si el SQL_DESC_CONCISE_TYPE campo es un tipo de datos de hora o marca de tiempo, un tipo de intervalo con un componente de segundos o uno de los tipos de datos de intervalo con un componente de hora, se comprueba que el campo de SQL_DESC_PRECISION sea una precisión de segundos válida.  
  
-   Si el SQL_DESC_CONCISE_TYPE es un tipo de datos de intervalo, se comprueba que el campo de SQL_DESC_DATETIME_INTERVAL_PRECISION sea un valor de precisión inicial de intervalo válido.  
  
 Normalmente no se establece el campo de SQL_DESC_DATA_PTR de un IPD; sin embargo, una aplicación puede hacerlo para forzar una comprobación de coherencia de los campos IPD. No se puede realizar una comprobación de coherencia en un IRD. El valor en el que se establece el campo de SQL_DESC_DATA_PTR de IPD no se almacena realmente y no se puede recuperar mediante una llamada a **SQLGetDescField** o **SQLGetDescRec**. la configuración solo se realiza para forzar la comprobación de coherencia.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar una columna|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Enlazar un parámetro|[Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Obtener un único campo descriptor|[Función SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Obtener varios campos de descriptor|[Función SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Establecimiento de campos de descriptor único|[Función SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
