---
description: Función SQLSetDescField
title: Función SQLSetDescField | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetDescField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescField
helpviewer_keywords:
- SQLSetDescField function [ODBC]
ms.assetid: 8c544388-fe9d-4f94-a0ac-fa0b9c9c88a5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bd48da2caec1acba5cfe775a2baa1efaaf886aab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200972"
---
# <a name="sqlsetdescfield-function"></a>Función SQLSetDescField

**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLSetDescField** establece el valor de un solo campo de un registro de descriptor.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
SQLRETURN SQLSetDescField(  
     SQLHDESC      DescriptorHandle,  
     SQLSMALLINT   RecNumber,  
     SQLSMALLINT   FieldIdentifier,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    BufferLength);  
```  
  
## <a name="arguments"></a>Argumentos  
 *DescriptorHandle*  
 Entradas Identificador del descriptor.  
  
 *RecNumber*  
 Entradas Indica el registro del descriptor que contiene el campo que la aplicación busca establecer. Los registros de descriptor se numeran a partir de 0, con el número de registro 0 como registro del marcador. El argumento *RecNumber* se omite para los campos de encabezado.  
  
 *FieldIdentifier*  
 Entradas Indica el campo del descriptor cuyo valor se va a establecer. Para obtener más información, vea el tema sobre el argumento *FieldIdentifier* en la sección "Comentarios".  
  
 *ValuePtr*  
 Entradas Puntero a un búfer que contiene la información del descriptor o un valor entero. El tipo de datos depende del valor de *FieldIdentifier*. Si *ValuePtr* es un valor entero, puede considerarse como 8 bytes (SQLLEN), 4 bytes (sqlinteger donde) o 2 bytes (SQLSMALLINT), dependiendo del valor del argumento *FieldIdentifier* .  
  
 *BufferLength*  
 Entradas Si *FieldIdentifier* es un campo definido por ODBC y *ValuePtr* apunta a una cadena de caracteres o a un búfer binario, este argumento debe ser la longitud de **ValuePtr*. En el caso de los datos de cadenas de caracteres, este argumento debe contener el número de bytes de la cadena.  
  
 Si *FieldIdentifier* es un campo definido por ODBC y *ValuePtr* es un entero, se omite *BufferLength* .  
  
 Si *FieldIdentifier* es un campo definido por el controlador, la aplicación indica la naturaleza del campo en el administrador de controladores estableciendo el argumento *BufferLength* . *BufferLength* puede tener los siguientes valores:  
  
-   Si *ValuePtr* es un puntero a una cadena de caracteres, *BufferLength* es la longitud de la cadena o SQL_NTS.  
  
-   Si *ValuePtr* es un puntero a un búfer binario, la aplicación coloca el resultado de la macro SQL_LEN_BINARY_ATTR (*length*) en *BufferLength*. Esto coloca un valor negativo en *BufferLength*.  
  
-   Si *ValuePtr* es un puntero a un valor distinto de una cadena de caracteres o una cadena binaria, *BufferLength* debe tener el valor SQL_IS_POINTER.  
  
-   Si *ValuePtr* contiene un valor de longitud fija, *BufferLength* es SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT o SQL_IS_USMALLINT, según corresponda.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLSetDescField** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DESC y un *identificador* de *DescriptorHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLSetDescField** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01S02|Valor de opción cambiado|El controlador no admitía el valor especificado en *\* ValuePtr* (si *ValuePtr* era un puntero) o el valor de *ValuePtr* (si *ValuePtr* era un valor entero) o *\* ValuePtr* no era válido debido a las condiciones de trabajo de implementación, por lo que el controlador sustituyó un valor similar. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07009|Índice de descriptor no válido|El argumento *FieldIdentifier* era un campo registro, el argumento *RecNumber* era 0 y el argumento *DescriptorHandle* hacía referencia a un identificador IPD.<br /><br /> El argumento *RecNumber* era menor que 0 y el argumento *DescriptorHandle* hacía referencia a ARD o APD.<br /><br /> El argumento *RecNumber* era mayor que el número máximo de columnas o parámetros que el origen de datos puede admitir, y el argumento *DescriptorHandle* hace referencia a APD o ARD.<br /><br /> (DM) el argumento *FieldIdentifier* se SQL_DESC_COUNT y el argumento *\* ValuePtr* era menor que 0.<br /><br /> El argumento *RecNumber* era igual a 0 y el argumento *DescriptorHandle* hacía referencia a un APD asignado implícitamente. (Este error no se produce con un descriptor de aplicación asignado explícitamente, porque no se sabe si un descriptor de aplicación asignado explícitamente es un APD o ARD hasta el tiempo de ejecución).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|22001|Datos de cadena, truncados a la derecha|Se SQL_DESC_NAME el argumento *FieldIdentifier* y el argumento *BufferLength* era un valor mayor que SQL_MAX_IDENTIFIER_LEN.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY010|Error de secuencia de función|(DM) el *DescriptorHandle* estaba asociado a un *StatementHandle* para el que se llamó a una función que se ejecuta de forma asincrónica (no a este) y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para el *StatementHandle* con el que se asoció *DescriptorHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *DescriptorHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLSetDescField** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para uno de los identificadores de instrucciones asociados a *DescriptorHandle* y se devolvieron SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY016|No se puede modificar un descriptor de fila de implementación|El argumento *DescriptorHandle* estaba asociado a IRD y el argumento *FieldIdentifier* no era SQL_DESC_ARRAY_STATUS_PTR ni SQL_DESC_ROWS_PROCESSED_PTR.|  
|HY021|Información de descriptor incoherente|Los campos SQL_DESC_TYPE y SQL_DESC_DATETIME_INTERVAL_CODE no forman un tipo SQL de ODBC válido o un tipo SQL específico del controlador válido (para IPD) o un tipo C de ODBC válido (para APD o ARDs).<br /><br /> La información del descriptor comprobada durante una comprobación de coherencia no era coherente. (Vea "comprobación de coherencia" en **SQLSetDescRec**).|  
|HY090|Longitud de búfer o cadena no válida|(DM) *\* ValuePtr* es una cadena de caracteres y *BufferLength* era menor que cero pero no era igual que SQL_NTS.<br /><br /> (DM) el controlador era un controlador ODBC 2 *. x* , el descriptor era ARD, el argumento *ColumnNumber* se estableció en 0 y el valor especificado para el argumento *BufferLength* no era igual a 4.|  
|HY091|Identificador de campo descriptor no válido|El valor especificado para el argumento *FieldIdentifier* no era un campo definido por ODBC y no era un valor definido por la implementación.<br /><br /> El argumento *FieldIdentifier* no era válido para el argumento *DescriptorHandle* .<br /><br /> El argumento *FieldIdentifier* era un campo definido por ODBC de solo lectura.|  
|HY092|Identificador de opción/atributo no válido|El valor de *\* ValuePtr* no era válido para el argumento *FieldIdentifier* .<br /><br /> Se SQL_DESC_UNNAMED el argumento *FieldIdentifier* y *ValuePtr* se SQL_NAMED.|  
|HY105|Tipo de parámetro no válido|(DM) el valor especificado para el campo SQL_DESC_PARAMETER_TYPE no era válido. (Para obtener más información, vea la sección "argumento *InputOutputType* " en **SQLBindParameter**).|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, vea [novedades de ODBC 3,8](../../../odbc/reference/what-s-new-in-odbc-3-8.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *DescriptorHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Una aplicación puede llamar a **SQLSetDescField** para establecer cualquier campo de descriptor de uno en uno. Una llamada a **SQLSetDescField** establece un único campo en un único descriptor. Se puede llamar a esta función para establecer cualquier campo en cualquier tipo de descriptor, siempre que se pueda establecer el campo. (Vea la tabla más adelante en esta sección).  
  
> [!NOTE]  
>  Si se produce un error en una llamada a **SQLSetDescField** , el contenido del registro del descriptor identificado por el argumento *RecNumber* es indefinido.  
  
 Se puede llamar a otras funciones para establecer varios campos de descriptor con una única llamada de la función. La función **SQLSetDescRec** establece una variedad de campos que afectan al tipo de datos y al búfer enlazado a una columna o parámetro (los campos SQL_DESC_TYPE, SQL_DESC_DATETIME_INTERVAL_CODE, SQL_DESC_OCTET_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_DATA_PTR, SQL_DESC_OCTET_LENGTH_PTR y SQL_DESC_INDICATOR_PTR). **SQLBindCol** o **SQLBindParameter** se pueden usar para realizar una especificación completa para el enlace de una columna o un parámetro. Estas funciones establecen un grupo específico de campos de descriptor con una llamada de función.  
  
 Se puede llamar a **SQLSetDescField** para cambiar los búferes de enlace agregando un desplazamiento a los punteros de enlace (SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR o SQL_DESC_OCTET_LENGTH_PTR). Esto cambia los búferes de enlace sin llamar a **SQLBindCol** o **SQLBindParameter**, lo que permite a una aplicación cambiar SQL_DESC_DATA_PTR sin cambiar otros campos, como SQL_DESC_DATA_TYPE.  
  
 Si una aplicación llama a **SQLSetDescField** para establecer cualquier campo que no sea SQL_DESC_COUNT o los campos diferidos SQL_DESC_DATA_PTR, SQL_DESC_OCTET_LENGTH_PTR o SQL_DESC_INDICATOR_PTR, el registro se vuelve sin enlazar.  
  
 Los campos de encabezado del descriptor se establecen mediante una llamada a **SQLSetDescField** con el *FieldIdentifier* adecuado. Muchos campos de encabezado también son atributos de instrucción, por lo que también se pueden establecer mediante una llamada a **SQLSetStmtAttr**. Esto permite que las aplicaciones establezcan un campo de descriptor sin obtener primero un identificador de descriptor. Cuando se llama a **SQLSetDescField** para establecer un campo de encabezado, se omite el argumento *RecNumber* .  
  
 Se usa un *RecNumber* de 0 para establecer los campos de marcador.  
  
> [!NOTE]  
>  El atributo Statement SQL_ATTR_USE_BOOKMARKS siempre debe establecerse antes de llamar a **SQLSetDescField** para establecer los campos de marcador. Aunque esto no es obligatorio, se recomienda encarecidamente.  
  
## <a name="sequence-of-setting-descriptor-fields"></a>Secuencia de campos de descriptor de configuración  
 Al establecer los campos de descriptor mediante una llamada a **SQLSetDescField**, la aplicación debe seguir una secuencia específica:  
  
1.  En primer lugar, la aplicación debe establecer el campo SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE o SQL_DESC_DATETIME_INTERVAL_CODE.  
  
2.  Una vez establecido uno de estos campos, la aplicación puede establecer un atributo de un tipo de datos y el controlador establece los campos de atributo de tipo de datos en los valores predeterminados adecuados para el tipo de datos. El valor predeterminado automático de los campos de atributo de tipo garantiza que el descriptor siempre esté listo para usarse una vez que la aplicación haya especificado un tipo de datos. Si la aplicación establece explícitamente un atributo de tipo de datos, se invalidará el atributo predeterminado.  
  
3.  Una vez que se ha establecido uno de los campos enumerados en el paso 1, y se han establecido los atributos de tipo de datos, la aplicación puede establecer SQL_DESC_DATA_PTR. Esto solicita una comprobación de coherencia de los campos del descriptor. Si la aplicación cambia el tipo de datos o los atributos después de establecer el campo de SQL_DESC_DATA_PTR, el controlador establece SQL_DESC_DATA_PTR en un puntero nulo, desenlazando el registro. Esto obliga a la aplicación a completar los pasos correctos en secuencia, antes de que se pueda usar el registro del descriptor.  
  
## <a name="initialization-of-descriptor-fields"></a>Inicialización de campos de Descriptor  
 Cuando se asigna un descriptor, los campos del descriptor se pueden inicializar en un valor predeterminado, inicializarse sin un valor predeterminado o no estar definidos para el tipo de descriptor. En las tablas siguientes se indica la inicialización de cada campo de cada tipo de descriptor, con "D", que indica que el campo se inicializa con un valor predeterminado y "ND" que indica que el campo se inicializa sin un valor predeterminado. Si se muestra un número, el valor predeterminado del campo es ese número. Las tablas también indican si un campo es de lectura/escritura (R/W) o de solo lectura (R).  
  
 Los campos de un IRD tienen un valor predeterminado solo después de que la instrucción se haya preparado o ejecutado y se haya rellenado IRD, no cuando se haya asignado el identificador o el descriptor de la instrucción. Hasta que se haya rellenado el IRD, cualquier intento de obtener acceso a un campo de un IRD devolverá un error.  
  
 Algunos campos de descriptor se definen para uno o varios de los tipos de descriptor (ARDs y IRDs y APD y IPD), pero no todos. Cuando un campo no está definido para un tipo de descriptor, no es necesario para ninguna de las funciones que utilizan ese descriptor.  
  
 Los campos a los que se puede tener acceso mediante **SQLGetDescField** no se pueden establecer con **SQLSetDescField**. En las tablas siguientes se enumeran los campos que se pueden establecer mediante **SQLSetDescField** .  
  
 La inicialización de los campos de encabezado se describe en la tabla siguiente.  
  
|Nombre del campo de encabezado|Tipo|L/E|Valor predeterminado|  
|-----------------------|----------|----------|-------------|  
|SQL_DESC_ALLOC_TYPE|SQLSMALLINT|ARD: R APD: R IRD: R IPD: R|ARD: SQL_DESC_ALLOC_AUTO para Implicit o SQL_DESC_ALLOC_USER para Explicit<br /><br /> APD: SQL_DESC_ALLOC_AUTO para Implicit o SQL_DESC_ALLOC_USER para Explicit<br /><br /> IRD: SQL_DESC_ALLOC_AUTO<br /><br /> IPD: SQL_DESC_ALLOC_AUTO|  
|SQL_DESC_ARRAY_SIZE|SQLULEN|ARD: l/e APD: R/W IRD: no se usa IPD: sin usar|ARD: [1] APD: [1] IRD: no se usa IPD: sin usar|  
|SQL_DESC_ARRAY_STATUS_PTR|SQLUSMALLINT*|ARD: L/E APD: R/W IRD: R/W IPD: R/W|ARD: null PTR APD: null PTR IRD: null PTR IPD: null PTR|  
|SQL_DESC_BIND_OFFSET_PTR|SQLLEN|ARD: l/e APD: R/W IRD: no se usa IPD: sin usar|ARD: null PTR APD: null PTR IRD: no usado IPD: sin usar|  
|SQL_DESC_BIND_TYPE|SQLINTEGER|ARD: l/e APD: R/W IRD: no se usa IPD: sin usar|ARD: SQL_BIND_BY_COLUMN<br /><br /> APD: SQL_BIND_BY_COLUMN<br /><br /> IRD: sin usar<br /><br /> IPD: sin usar|  
|SQL_DESC_COUNT|SQLSMALLINT|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: 0 APD: 0 IRD: D IPD: 0|  
|SQL_DESC_ROWS_PROCESSED_PTR|SQLULEN|ARD: APD no usado: IRD sin usar: R/W IPD: R/W|ARD: APD no usado: IRD sin usar: null PTR IPD: null PTR|  
  
 [1] estos campos se definen solo cuando el controlador rellena automáticamente IPD. Si no es así, son indefinidos. Si una aplicación intenta establecer estos campos, se devolverá SQLSTATE HY091 (identificador de campo de descriptor no válido).  
  
 La inicialización de los campos de registro es como se muestra en la tabla siguiente.  
  
|Nombre del campo de registro|Tipo|L/E|Valor predeterminado|  
|-----------------------|----------|----------|-------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQLINTEGER|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_BASE_COLUMN_NAME|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_BASE_TABLE_NAME|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_CASE_SENSITIVE|SQLINTEGER|ARD: APD no usado: IRD sin usar: R IPD: R|ARD: APD sin usar: IRD no usado: D IPD: D [1]|  
|SQL_DESC_CATALOG_NAME|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_CONCISE_TYPE|SQLSMALLINT|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: SQL_C_ APD PREDETERMINADO: SQL_C_ VALOR PREDETERMINADO IRD: D IPD: ND|  
|SQL_DESC_DATA_PTR|SQLPOINTER|ARD: l/e APD: R/W IRD: no se usa IPD: sin usar|ARD: null PTR APD: null PTR IRD: no se usa IPD: sin usar [2]|  
|SQL_DESC_DATETIME_INTERVAL_CODE|SQLSMALLINT|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|SQLINTEGER|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_DISPLAY_SIZE|SQLLEN|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_FIXED_PREC_SCALE|SQLSMALLINT|ARD: APD no usado: IRD sin usar: R IPD: R|ARD: APD sin usar: IRD no usado: D IPD: D [1]|  
|SQL_DESC_INDICATOR_PTR|SQLLEN|ARD: l/e APD: R/W IRD: no se usa IPD: sin usar|ARD: null PTR APD: null PTR IRD: no usado IPD: sin usar|  
|SQL_DESC_LABEL|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_LENGTH|SQLULEN|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_LITERAL_PREFIX|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_LITERAL_SUFFIX|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_LOCAL_TYPE_NAME|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: R|ARD: APD sin usar: IRD no usado: D IPD: D [1]|  
|SQL_DESC_NAME|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_NULLABLE|SQLSMALLINT|ARD: APD no usado: IRD sin usar: R IPD: R|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_NUM_PREC_RADIX|SQLINTEGER|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_OCTET_LENGTH|SQLLEN|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_OCTET_LENGTH_PTR|SQLLEN|ARD: l/e APD: R/W IRD: no se usa IPD: sin usar|ARD: null PTR APD: null PTR IRD: no usado IPD: sin usar|  
|SQL_DESC_PARAMETER_TYPE|SQLSMALLINT|ARD: APD no usado: IRD sin usar: IPD sin usar: R/W|ARD: APD no usado: IRD sin usar: IPD: D = SQL_PARAM_INPUT|  
|SQL_DESC_PRECISION|SQLSMALLINT|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_ROWVER|SQLSMALLINT|ARD: sin usar<br /><br /> APD: sin usar<br /><br /> IRD: R<br /><br /> IPD: R|ARD: sin usar<br /><br /> APD: sin usar<br /><br /> IRD: ND<br /><br /> IPD: ND|  
|SQL_DESC_SCALE|SQLSMALLINT|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_SCHEMA_NAME|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_SEARCHABLE|SQLSMALLINT|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_TABLE_NAME|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
|SQL_DESC_TYPE|SQLSMALLINT|ARD: L/E APD: R/W IRD: R IPD: R/W|ARD: SQL_C_DEFAULT APD: SQL_C_DEFAULT IRD: D IPD: ND|  
|SQL_DESC_TYPE_NAME|SQLCHAR|ARD: APD no usado: IRD sin usar: R IPD: R|ARD: APD sin usar: IRD no usado: D IPD: D [1]|  
|SQL_DESC_UNNAMED|SQLSMALLINT|ARD: APD no usado: IRD sin usar: R IPD: R/W|ARD: ND APD: ND IRD: D IPD: ND|  
|SQL_DESC_UNSIGNED|SQLSMALLINT|ARD: APD no usado: IRD sin usar: R IPD: R|ARD: APD sin usar: IRD no usado: D IPD: D [1]|  
|SQL_DESC_UPDATABLE|SQLSMALLINT|ARD: APD no usado: IRD sin usar: R IPD: sin usar|ARD: APD sin usar: IRD no usado: D IPD: sin usar|  
  
 [1] estos campos se definen solo cuando el controlador rellena automáticamente IPD. Si no es así, son indefinidos. Si una aplicación intenta establecer estos campos, se devolverá SQLSTATE HY091 (identificador de campo de descriptor no válido).  
  
 [2] se puede establecer el campo SQL_DESC_DATA_PTR en IPD para forzar una comprobación de coherencia. En una llamada subsiguiente a **SQLGetDescField** o **SQLGetDescRec**, no es necesario que el controlador devuelva el valor en el que SQL_DESC_DATA_PTR se estableció en.  
  
## <a name="fieldidentifier-argument"></a>Argumento FieldIdentifier  
 El argumento *FieldIdentifier* indica el campo de descriptor que se va a establecer. Un descriptor contiene el *encabezado del descriptor,* que consta de los campos de encabezado que se describen en la sección siguiente, "campos de encabezado" y cero o más *registros de descriptor,* que constan de los campos de registro que se describen en la sección siguiente a la sección "campos de encabezado".  
  
## <a name="header-fields"></a>Campos de encabezado  
 Cada descriptor tiene un encabezado que consta de los siguientes campos:  
  
 **SQL_DESC_ALLOC_TYPE [All]**  
 Este campo de encabezado SQLSMALLINT de solo lectura especifica si el controlador asignó automáticamente el descriptor o la aplicación de forma explícita. La aplicación puede obtener, pero no modificar, este campo. El campo se establece en SQL_DESC_ALLOC_AUTO por el controlador si el controlador lo asignó automáticamente. El controlador lo SQL_DESC_ALLOC_USER si el descriptor fue asignado explícitamente por la aplicación.  
  
 **SQL_DESC_ARRAY_SIZE [descriptores de aplicación]**  
 En ARDs, este campo de encabezado SQLULEN especifica el número de filas del conjunto de filas. Es el número de filas que se va a devolver mediante una llamada a **SQLFetch** o **SQLFetchScroll** , o que se va a utilizar en una llamada a **SQLBulkOperations** o **SQLSetPos**.  
  
 En APD, este campo de encabezado SQLULEN especifica el número de valores para cada parámetro.  
  
 El valor predeterminado de este campo es 1. Si SQL_DESC_ARRAY_SIZE es mayor que 1, SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR de APD o ARD apuntan a matrices. La cardinalidad de cada matriz es igual al valor de este campo.  
  
 Este campo de ARD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_ROW_ARRAY_SIZE. Este campo de APD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_PARAMSET_SIZE.  
  
 **SQL_DESC_ARRAY_STATUS_PTR [All]**  
 Para cada tipo de descriptor, este campo de encabezado SQLUSMALLINT * apunta a una matriz de valores de SQLUSMALLINT. Estas matrices tienen el nombre siguiente: matriz de estado de fila (IRD), matriz de estado de parámetro (IPD), matriz de operación de fila (ARD) y matriz de operaciones de parámetros (APD).  
  
 En IRD, este campo de encabezado apunta a una matriz de estado de fila que contiene los valores de estado después de una llamada a **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** o **SQLSetPos**. La matriz tiene tantos elementos como filas hay en el conjunto de filas. La aplicación debe asignar una matriz de SQLUSMALLINTs y establecer este campo para que apunte a la matriz. De forma predeterminada, el campo se establece en un puntero nulo. El controlador rellenará la matriz, a menos que el campo SQL_DESC_ARRAY_STATUS_PTR esté establecido en un puntero nulo, en cuyo caso no se generan valores de estado y la matriz no se rellena.  
  
> [!CAUTION]  
>  El comportamiento del controlador es indefinido si la aplicación establece los elementos de la matriz de estado de fila a la que señala el campo de SQL_DESC_ARRAY_STATUS_PTR de IRD.  
  
 La matriz se rellena inicialmente mediante una llamada a **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** o **SQLSetPos**. Si la llamada no devolvió SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, el contenido de la matriz a la que señala este campo es indefinido. Los elementos de la matriz pueden contener los valores siguientes:  
  
-   SQL_ROW_SUCCESS: la fila se ha recuperado correctamente y no ha cambiado desde que se capturó por última vez.  
  
-   SQL_ROW_SUCCESS_WITH_INFO: la fila se ha recuperado correctamente y no ha cambiado desde que se capturó por última vez. Sin embargo, se devolvió una advertencia sobre la fila.  
  
-   SQL_ROW_ERROR: se produjo un error al capturar la fila.  
  
-   SQL_ROW_UPDATED: la fila se ha recuperado correctamente y se ha actualizado desde que se capturó por última vez. Si se vuelve a capturar la fila, su estado es SQL_ROW_SUCCESS.  
  
-   SQL_ROW_DELETED: la fila se ha eliminado desde la última vez que se recuperó.  
  
-   SQL_ROW_ADDED: **SQLBulkOperations** insertó la fila. Si se vuelve a capturar la fila, su estado es SQL_ROW_SUCCESS.  
  
-   SQL_ROW_NOROW: el conjunto de filas se superpone al final del conjunto de resultados y no se devolvió ninguna fila que corresponda a este elemento de la matriz de estado de fila.  
  
 Este campo de IRD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_ROW_STATUS_PTR.  
  
 El campo SQL_DESC_ARRAY_STATUS_PTR de IRD es válido solo después de que se devuelvan SQL_SUCCESS o SQL_SUCCESS_WITH_INFO. Si el código de retorno no es uno de estos, la ubicación a la que apunta SQL_DESC_ROWS_PROCESSED_PTR es indefinida.  
  
 En IPD, este campo de encabezado apunta a una matriz de estado de parámetro que contiene información de estado para cada conjunto de valores de parámetro después de una llamada a **SQLExecute** o **SQLExecDirect**. Si la llamada a **SQLExecute** o **SQLExecDirect** no devolvió SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, el contenido de la matriz a la que señala este campo es indefinido. La aplicación debe asignar una matriz de SQLUSMALLINTs y establecer este campo para que apunte a la matriz. El controlador rellenará la matriz, a menos que el campo SQL_DESC_ARRAY_STATUS_PTR esté establecido en un puntero nulo, en cuyo caso no se generan valores de estado y la matriz no se rellena. Los elementos de la matriz pueden contener los valores siguientes:  
  
-   SQL_PARAM_SUCCESS: la instrucción SQL se ejecutó correctamente para este conjunto de parámetros.  
  
-   SQL_PARAM_SUCCESS_WITH_INFO: la instrucción SQL se ejecutó correctamente para este conjunto de parámetros; sin embargo, la información de advertencia está disponible en la estructura de datos de diagnóstico.  
  
-   SQL_PARAM_ERROR: error al procesar este conjunto de parámetros. Hay información adicional sobre el error en la estructura de datos de diagnóstico.  
  
-   SQL_PARAM_UNUSED: este conjunto de parámetros no se usó, posiblemente debido a que algún conjunto de parámetros anterior produjo un error que anuló el procesamiento más o porque se estableció SQL_PARAM_IGNORE para ese conjunto de parámetros en la matriz especificada por el campo SQL_DESC_ARRAY_STATUS_PTR de APD.  
  
-   SQL_PARAM_DIAG_UNAVAILABLE: la información de diagnóstico no está disponible. Un ejemplo de esto es cuando el controlador trata las matrices de parámetros como una unidad monolítica y, por tanto, no genera este nivel de información de error.  
  
 Este campo de IPD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_PARAM_STATUS_PTR.  
  
 En ARD, este campo de encabezado apunta a una matriz de operaciones de filas de valores que la aplicación puede establecer para indicar si esta fila se va a omitir para las operaciones **SQLSetPos** . Los elementos de la matriz pueden contener los valores siguientes:  
  
-   SQL_ROW_PROCEED: la fila se incluye en la operación masiva mediante **SQLSetPos**. (Este valor no garantiza que la operación se realizará en la fila. Si la fila tiene el estado SQL_ROW_ERROR en la matriz de estado de fila IRD, es posible que el controlador no pueda realizar la operación en la fila).  
  
-   SQL_ROW_IGNORE: la fila se excluye de la operación masiva mediante **SQLSetPos**.  
  
 Si no se establece ningún elemento de la matriz, todas las filas se incluyen en la operación masiva. Si el valor del campo SQL_DESC_ARRAY_STATUS_PTR de ARD es un puntero nulo, todas las filas se incluyen en la operación masiva; la interpretación es igual que si el puntero apuntase a una matriz válida y se SQL_ROW_PROCEED todos los elementos de la matriz. Si un elemento de la matriz se establece en SQL_ROW_IGNORE, no se cambia el valor de la matriz de estado de fila de la fila omitida.  
  
 Este campo de ARD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_ROW_OPERATION_PTR.  
  
 En APD, este campo de encabezado apunta a una matriz de operaciones de parámetros que la aplicación puede establecer para indicar si se debe omitir este conjunto de parámetros cuando se llama a **SQLExecute** o **SQLExecDirect** . Los elementos de la matriz pueden contener los valores siguientes:  
  
-   SQL_PARAM_PROCEED: el conjunto de parámetros se incluye en la llamada a **SQLExecute** o **SQLExecDirect** .  
  
-   SQL_PARAM_IGNORE: el conjunto de parámetros se excluye de la llamada **SQLExecute** o **SQLExecDirect** .  
  
 Si no se establece ningún elemento de la matriz, se usan todos los conjuntos de parámetros de la matriz en las llamadas **SQLExecute** o **SQLExecDirect** . Si el valor del campo SQL_DESC_ARRAY_STATUS_PTR de APD es un puntero nulo, se usan todos los conjuntos de parámetros. la interpretación es igual que si el puntero apuntase a una matriz válida y se SQL_PARAM_PROCEED todos los elementos de la matriz.  
  
 Este campo de APD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_PARAM_OPERATION_PTR.  
  
 **SQL_DESC_BIND_OFFSET_PTR [descriptores de aplicación]**  
 Este campo de encabezado SQLLEN * apunta al desplazamiento de enlace. De forma predeterminada, se establece en un puntero nulo. Si este campo no es un puntero nulo, el controlador desreferencia el puntero y agrega el valor desreferenciado a cada uno de los campos diferidos que tienen un valor distinto de NULL en el registro del descriptor (SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR) en el momento de la captura y usa los nuevos valores de puntero al enlazar.  
  
 El desplazamiento de enlace siempre se agrega directamente a los valores de los campos SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR. Si el desplazamiento se cambia a un valor diferente, el nuevo valor todavía se agrega directamente al valor en cada campo de descriptor. El nuevo desplazamiento no se agrega al valor del campo más cualquier desplazamiento anterior.  
  
 Este campo es un *campo diferido*: no se utiliza en el momento en que se establece, pero el controlador lo usa más adelante cuando necesita determinar las direcciones de los búferes de datos.  
  
 Este campo de ARD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_ROW_BIND_OFFSET_PTR. Este campo de ARD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_PARAM_BIND_OFFSET_PTR.  
  
 Para obtener más información, vea la descripción del enlace de modo de fila en [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md) y [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 **SQL_DESC_BIND_TYPE [descriptores de aplicación]**  
 Este campo de encabezado SQLUINTEGER que incluya establece la orientación de enlace que se va a usar para enlazar columnas o parámetros.  
  
 En ARDs, este campo especifica la orientación de enlace cuando se llama a **SQLFetchScroll** o **SQLFetch** en el identificador de instrucción asociado.  
  
 Para seleccionar el enlace de modo de columna para las columnas, este campo se establece en SQL_BIND_BY_COLUMN (valor predeterminado).  
  
 Este campo de ARD también se puede establecer llamando a **SQLSetStmtAttr** con el *atributo* SQL_ATTR_ROW_BIND_TYPE.  
  
 En APD, este campo especifica la orientación de enlace que se va a usar para los parámetros dinámicos.  
  
 Para seleccionar el enlace de modo de columna para los parámetros, este campo se establece en SQL_BIND_BY_COLUMN (valor predeterminado).  
  
 Este campo de APD también se puede establecer llamando a **SQLSetStmtAttr** con el *atributo* SQL_ATTR_PARAM_BIND_TYPE.  
  
 **SQL_DESC_COUNT [All]**  
 Este campo de encabezado SQLSMALLINT especifica el índice basado en 1 del registro con el número más alto que contiene los datos. Cuando el controlador establece la estructura de datos para el descriptor, también debe establecer el campo SQL_DESC_COUNT para mostrar cuántos registros son significativos. Cuando una aplicación asigna una instancia de esta estructura de datos, no tiene que especificar el número de registros para los que se va a reservar espacio. A medida que la aplicación especifica el contenido de los registros, el controlador toma las medidas necesarias para asegurarse de que el identificador del descriptor hace referencia a una estructura de datos del tamaño adecuado.  
  
 SQL_DESC_COUNT no es un recuento de todas las columnas de datos que están enlazadas (si el campo está en un ARD) o de todos los parámetros enlazados (si el campo está en un APD), pero el número del registro con el número más alto. Si la columna o el parámetro con el número más alto está desenlazado, SQL_DESC_COUNT se cambia al número de la siguiente columna o parámetro con el número superior. Si una columna o un parámetro con un número menor que el número de la columna con el número más alto es sin enlazar (llamando a **SQLBindCol** con el argumento *TargetValuePtr* establecido en un puntero nulo, o **SQLBindParameter** con el argumento *ParameterValuePtr* establecido en un puntero nulo), no se cambia SQL_DESC_COUNT. Si se enlazan columnas o parámetros adicionales con números mayores que el registro con el número más alto que contiene datos, el controlador aumenta automáticamente el valor en el campo SQL_DESC_COUNT. Si todas las columnas están desenlazadas llamando a **SQLFreeStmt** con la opción SQL_UNBIND, los campos de SQL_DESC_COUNT de ARD y IRD se establecen en 0. Si se llama a **SQLFreeStmt** con la opción SQL_RESET_PARAMS, los campos de SQL_DESC_COUNT de APD y IPD se establecen en 0.  
  
 Una aplicación puede establecer explícitamente el valor de SQL_DESC_COUNT mediante una llamada a **SQLSetDescField**. Si el valor de SQL_DESC_COUNT se reduce explícitamente, se quitan de manera eficaz todos los registros con números mayores que el nuevo valor de SQL_DESC_COUNT. Si el valor de SQL_DESC_COUNT se establece explícitamente en 0 y el campo está en un ARD, se liberarán todos los búferes de datos excepto una columna de marcador enlazado.  
  
 El número de registros de este campo de un ARD no incluye una columna de marcador enlazada. La única manera de desenlazar una columna de marcador es establecer el SQL_DESC_DATA_PTR campo en un puntero nulo.  
  
 **SQL_DESC_ROWS_PROCESSED_PTR [descriptores de implementación]**  
 En un IRD, este \* campo de encabezado SQLULEN apunta a un búfer que contiene el número de filas recuperadas después de una llamada a **SQLFetch** o **SQLFetchScroll**, o el número de filas afectadas en una operación masiva realizada por una llamada a **SQLBulkOperations** o **SQLSetPos**, incluidas las filas de error.  
  
 En un IPD, este campo de encabezado SQLUINTEGER que incluya * apunta a un búfer que contiene el número de conjuntos de parámetros que se han procesado, incluidos los conjuntos de errores. No se devolverá ningún número si se trata de un puntero nulo.  
  
 SQL_DESC_ROWS_PROCESSED_PTR es válido solo después de que se devuelvan SQL_SUCCESS o SQL_SUCCESS_WITH_INFO después de una llamada a **SQLFetch** o **SQLFetchScroll** (para un campo IRD) o **SQLExecute**, **SQLEXECDIRECT** o **SQLParamData** (para un campo IPD). Si la llamada que rellena el búfer señalado por este campo no devuelve SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, el contenido del búfer es indefinido, a menos que devuelva SQL_NO_DATA, en cuyo caso el valor del búfer se establece en 0.  
  
 Este campo de ARD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_ROWS_FETCHED_PTR. Este campo de APD también se puede establecer llamando a **SQLSetStmtAttr** con el atributo SQL_ATTR_PARAMS_PROCESSED_PTR.  
  
 La aplicación asigna el búfer señalado por este campo. Es un búfer de salida diferido establecido por el controlador. De forma predeterminada, se establece en un puntero nulo.  
  
## <a name="record-fields"></a>Campos de registro  
 Cada descriptor contiene uno o varios registros que se componen de campos que definen los datos de columna o los parámetros dinámicos, según el tipo de descriptor. Cada registro es una definición completa de una sola columna o parámetro.  
  
 **SQL_DESC_AUTO_UNIQUE_VALUE [IRDs]**  
 Este campo de registro SQLINTEGER donde de solo lectura contiene SQL_TRUE si la columna es una columna de incremento automático o SQL_FALSE si la columna no es una columna de incremento automático. Este campo es de solo lectura, pero la columna de incremento automático subyacente no es necesariamente de solo lectura.  
  
 **SQL_DESC_BASE_COLUMN_NAME [IRDs]**  
 Este campo de registro SQLCHAR * de solo lectura contiene el nombre de la columna base para la columna del conjunto de resultados. Si no existe un nombre de columna base (como en el caso de las columnas que son expresiones), esta variable contiene una cadena vacía.  
  
 **SQL_DESC_BASE_TABLE_NAME [IRDs]**  
 Este campo de registro SQLCHAR * de solo lectura contiene el nombre de la tabla base de la columna del conjunto de resultados. Si un nombre de tabla base no se puede definir o no es aplicable, esta variable contiene una cadena vacía.  
  
 **SQL_DESC_CASE_SENSITIVE [descriptores de implementación]**  
 Este campo de registro SQLINTEGER donde de solo lectura contiene SQL_TRUE si la columna o el parámetro se tratan con distinción de mayúsculas y minúsculas en las intercalaciones y comparaciones, o SQL_FALSE si la columna no se trata como si fuesen mayúsculas de minúsculas en las intercalaciones y comparaciones, o si es una columna que no es un carácter.  
  
 **SQL_DESC_CATALOG_NAME [IRDs]**  
 Este campo de registro SQLCHAR * de solo lectura contiene el catálogo de la tabla base que contiene la columna. El valor devuelto depende del controlador si la columna es una expresión o si la columna forma parte de una vista. Si el origen de datos no admite catálogos o no se puede determinar el catálogo, esta variable contiene una cadena vacía.  
  
 **SQL_DESC_CONCISE_TYPE [All]**  
 Este campo de encabezado SQLSMALLINT especifica el tipo de datos conciso para todos los tipos de datos, incluidos los tipos de datos datetime e Interval.  
  
 Los valores de los campos SQL_DESC_CONCISE_TYPE, SQL_DESC_TYPE y SQL_DESC_DATETIME_INTERVAL_CODE son interdependientes. Cada vez que se establece uno de los campos, también se debe establecer el otro. SQL_DESC_CONCISE_TYPE se puede establecer mediante una llamada a **SQLBindCol** o **SQLBindParameter**, o **SQLSetDescField**. SQL_DESC_TYPE se puede establecer mediante una llamada a **SQLSetDescField** o **SQLSetDescRec**.  
  
 Si SQL_DESC_CONCISE_TYPE se establece en un tipo de datos conciso que no sea un tipo de datos Interval o DateTime, el campo SQL_DESC_TYPE se establece en el mismo valor y el campo SQL_DESC_DATETIME_INTERVAL_CODE se establece en 0.  
  
 Si SQL_DESC_CONCISE_TYPE está establecido en el tipo de datos conciso de fecha y hora, el campo de SQL_DESC_TYPE se establece en el tipo detallado correspondiente (SQL_DATETIME o SQL_INTERVAL) y el campo SQL_DESC_DATETIME_INTERVAL_CODE se establece en el subcódigo correspondiente.  
  
 **SQL_DESC_DATA_PTR [descriptores de aplicación y IPD]**  
 Este campo de registro SQLPOINTER apunta a una variable que contendrá el valor del parámetro (para APD) o el valor de la columna (para ARDs). Este campo es un *campo aplazado*. No se utiliza en el momento en que se establece, sino que el controlador utiliza en un momento posterior para recuperar los datos.  
  
 La columna especificada por el campo SQL_DESC_DATA_PTR de ARD se desenlaza si el argumento *TargetValuePtr* de una llamada a **SQLBindCol** es un puntero nulo o si el campo SQL_DESC_DATA_PTR de ARD se establece mediante una llamada a **SQLSetDescField** o **SQLSetDescRec** en un puntero nulo. Otros campos no se ven afectados si el campo SQL_DESC_DATA_PTR está establecido en un puntero nulo.  
  
 Si la llamada a **SQLFetch** o **SQLFetchScroll** que rellena el búfer señalado por este campo no devolvió SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, el contenido del búfer es indefinido.  
  
 Siempre que se establece el campo de SQL_DESC_DATA_PTR de una APD, ARD o IPD, el controlador comprueba que el valor del campo SQL_DESC_TYPE contiene uno de los tipos de datos de ODBC C válidos o un tipo de datos específico del controlador, y que todos los demás campos que afectan a los tipos de datos son coherentes. Preguntar una comprobación de coherencia es el único uso del campo de SQL_DESC_DATA_PTR de un IPD. En concreto, si una aplicación establece el campo de SQL_DESC_DATA_PTR de un IPD y después llama a **SQLGetDescField** en este campo, no se devuelve necesariamente el valor que tenía establecido. Para obtener más información, vea "comprobaciones de coherencia" en [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 **SQL_DESC_DATETIME_INTERVAL_CODE [All]**  
 Este campo de registro SQLSMALLINT contiene el subcódigo para el tipo de datos datetime o Interval específico cuando el campo SQL_DESC_TYPE es SQL_DATETIME o SQL_INTERVAL. Esto se aplica a los tipos de datos SQL y C. El código está formado por el nombre del tipo de datos "CODE" sustituido por "TYPE" o "C_TYPE" (para los tipos DateTime), o por "CODE" sustituido por "INTERVAL" o "C_INTERVAL" (para los tipos de intervalo).  
  
 Si SQL_DESC_TYPE y SQL_DESC_CONCISE_TYPE en un descriptor de la aplicación se establecen en SQL_C_DEFAULT y el descriptor no está asociado a un identificador de instrucción, el contenido de SQL_DESC_DATETIME_INTERVAL_CODE no está definido.  
  
 Este campo se puede establecer para los tipos de datos de fecha y hora que se enumeran en la tabla siguiente.  
  
|Tipos DateTime|DATETIME_INTERVAL_CODE|  
|--------------------|------------------------------|  
|SQL_TYPE_DATE/SQL_C_TYPE_DATE|SQL_CODE_DATE|  
|SQL_TYPE_TIME/SQL_C_TYPE_TIME|SQL_CODE_TIME|  
|SQL_TYPE_TIMESTAMP/SQL_C_TYPE_TIMESTAMP|SQL_CODE_TIMESTAMP|  
  
 Este campo se puede establecer para los tipos de datos de intervalo que se enumeran en la tabla siguiente.  
  
|Tipo de intervalo|DATETIME_INTERVAL_CODE|  
|-------------------|------------------------------|  
|SQL_INTERVAL_DAY/SQL_C_INTERVAL_DAY|SQL_CODE_DAY|  
|SQL_INTERVAL_DAY_TO_HOUR/SQL_C_INTERVAL_DAY_TO_HOUR|SQL_CODE_DAY_TO_HOUR|  
|SQL_INTERVAL_DAY_TO_MINUTE/SQL_C_INTERVAL_DAY_TO_MINUTE|SQL_CODE_DAY_TO_MINUTE|  
|SQL_INTERVAL_DAY_TO_SECOND/SQL_C_INTERVAL_DAY_TO_SECOND|SQL_CODE_DAY_TO_SECOND|  
|SQL_INTERVAL_HOUR/SQL_C_INTERVAL_HOUR|SQL_CODE_HOUR|  
|SQL_INTERVAL_HOUR_TO_MINUTE/SQL_C_INTERVAL_HOUR_TO_MINUTE|SQL_CODE_HOUR_TO_MINUTE|  
|SQL_INTERVAL_HOUR_TO_SECOND/SQL_C_INTERVAL_HOUR_TO_SECOND|SQL_CODE_HOUR_TO_SECOND|  
|SQL_INTERVAL_MINUTE/SQL_C_INTERVAL_MINUTE|SQL_CODE_MINUTE|  
|SQL_INTERVAL_MINUTE_TO_SECOND/SQL_C_INTERVAL_MINUTE_TO_SECOND|SQL_CODE_MINUTE_TO_SECOND|  
|SQL_INTERVAL_MONTH/SQL_C_INTERVAL_MONTH|SQL_CODE_MONTH|  
|SQL_INTERVAL_SECOND/SQL_C_INTERVAL_SECOND|SQL_CODE_SECOND|  
|SQL_INTERVAL_YEAR/SQL_C_INTERVAL_YEAR|SQL_CODE_YEAR|  
|SQL_INTERVAL_YEAR_TO_MONTH/SQL_C_INTERVAL_YEAR_TO_MONTH|SQL_CODE_YEAR_TO_MONTH|  
  
 Para obtener más información sobre los intervalos de datos y este campo, vea [identificadores y descriptores de tipos de datos](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md).  
  
 **SQL_DESC_DATETIME_INTERVAL_PRECISION [All]**  
 Este campo de registro SQLINTEGER donde contiene la precisión inicial del intervalo si el campo SQL_DESC_TYPE es SQL_INTERVAL. Cuando el campo SQL_DESC_DATETIME_INTERVAL_CODE está establecido en un tipo de datos de intervalo, este campo se establece en la precisión inicial del intervalo predeterminado.  
  
 **SQL_DESC_DISPLAY_SIZE [IRDs]**  
 Este campo de registro SQLINTEGER donde de solo lectura contiene el número máximo de caracteres necesarios para mostrar los datos de la columna.  
  
 **SQL_DESC_FIXED_PREC_SCALE [descriptores de implementación]**  
 Este campo de registro SQLSMALLINT de solo lectura se establece en SQL_TRUE si la columna es una columna numérica exacta y tiene una escala de precisión fija y distinto de cero, o en SQL_FALSE si la columna no es una columna numérica exacta con una precisión y escala fijas.  
  
 **SQL_DESC_INDICATOR_PTR [descriptores de aplicación]**  
 En ARDs, este campo de registro SQLLEN * apunta a la variable de indicador. Esta variable contiene SQL_NULL_DATA si el valor de la columna es NULL. En el caso de APD, la variable de indicador se establece en SQL_NULL_DATA para especificar los argumentos dinámicos NULOs. De lo contrario, la variable es cero (a menos que los valores de SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR sean el mismo puntero).  
  
 Si el SQL_DESC_INDICATOR_PTR campo de un ARD es un puntero nulo, se impide que el controlador devuelva información sobre si la columna es NULL o no. Si la columna es NULL y SQL_DESC_INDICATOR_PTR es un puntero nulo, se devuelve SQLSTATE 22002 (variable de indicador necesaria pero no proporcionada) cuando el controlador intenta rellenar el búfer después de una llamada a **SQLFetch** o **SQLFetchScroll**. Si la llamada a **SQLFetch** o **SQLFetchScroll** no devolvió SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, el contenido del búfer es indefinido.  
  
 El campo SQL_DESC_INDICATOR_PTR determina si se establece el campo al que apunta SQL_DESC_OCTET_LENGTH_PTR. Si el valor de los datos de una columna es NULL, el controlador establece la variable de indicador en SQL_NULL_DATA. El campo al que apunta SQL_DESC_OCTET_LENGTH_PTR no se establece. Si no se encuentra un valor NULL durante la captura, el búfer señalado por SQL_DESC_INDICATOR_PTR se establece en cero y el búfer señalado por SQL_DESC_OCTET_LENGTH_PTR se establece en la longitud de los datos.  
  
 Si el SQL_DESC_INDICATOR_PTR campo de un APD es un puntero nulo, la aplicación no puede usar este registro del descriptor para especificar argumentos NULOs.  
  
 Este campo es un *campo diferido*: no se utiliza en el momento en que se establece, sino que se utiliza en un momento posterior por el controlador para indicar que la nulabilidad (para ARDs) o para determinar la nulabilidad (para APD).  
  
 **SQL_DESC_LABEL [IRDs]**  
 Este campo de registro SQLCHAR * de solo lectura contiene la etiqueta o el título de la columna. Si la columna no tiene una etiqueta, esta variable contiene el nombre de la columna. Si la columna no tiene nombre y no tiene etiqueta, esta variable contiene una cadena vacía.  
  
 **SQL_DESC_LENGTH [All]**  
 Este campo de registro SQLULEN es la longitud máxima o real de una cadena de caracteres en caracteres o un tipo de datos binario en bytes. Es la longitud máxima para un tipo de datos de longitud fija o la longitud real para un tipo de datos de longitud variable. Su valor siempre excluye el carácter de terminación null que finaliza la cadena de caracteres. Para los valores cuyo tipo es SQL_TYPE_DATE, SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP o uno de los tipos de datos de intervalo SQL, este campo tiene la longitud en caracteres de la representación de cadena de caracteres del valor de fecha y hora.  
  
 El valor de este campo puede ser diferente del valor de "length", tal y como se define en ODBC 2 *. x*. Para obtener más información, vea [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_LITERAL_PREFIX [IRDs]**  
 Este campo de registro SQLCHAR * de solo lectura contiene el carácter o los caracteres que el controlador reconoce como prefijo para un literal de este tipo de datos. Esta variable contiene una cadena vacía para un tipo de datos para el que no se puede aplicar un prefijo literal.  
  
 **SQL_DESC_LITERAL_SUFFIX [IRDs]**  
 Este campo de registro SQLCHAR * de solo lectura contiene el carácter o los caracteres que el controlador reconoce como sufijo de un literal de este tipo de datos. Esta variable contiene una cadena vacía para un tipo de datos para el que no se puede aplicar un sufijo literal.  
  
 **SQL_DESC_LOCAL_TYPE_NAME [descriptores de implementación]**  
 Este campo de registro SQLCHAR * de solo lectura contiene cualquier nombre localizado (lenguaje nativo) para el tipo de datos que puede ser diferente del nombre normal del tipo de datos. Si no hay ningún nombre localizado, se devuelve una cadena vacía. Este campo solo es para fines de presentación.  
  
 **SQL_DESC_NAME [descriptores de implementación]**  
 Este campo de registro SQLCHAR * de un descriptor de fila contiene el alias de columna, si se aplica. Si no se aplica el alias de columna, se devuelve el nombre de la columna. En cualquier caso, el controlador establece el campo SQL_DESC_UNNAMED en SQL_NAMED cuando establece el campo SQL_DESC_NAME. Si no hay un nombre de columna o un alias de columna, el controlador devuelve una cadena vacía en el campo SQL_DESC_NAME y establece el campo SQL_DESC_UNNAMED en SQL_UNNAMED.  
  
 Una aplicación puede establecer el campo de SQL_DESC_NAME de una IPD en un nombre de parámetro o alias para especificar los parámetros del procedimiento almacenado por nombre. (Para obtener más información, vea [enlazar parámetros por nombre (parámetros con nombre)](../../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md)). El SQL_DESC_NAME campo de un IRD es un campo de solo lectura; Se devolverá el valor de SQLSTATE HY091 (identificador de campo de descriptor no válido) si una aplicación intenta establecerlo.  
  
 En IPD, este campo es indefinido si el controlador no admite parámetros con nombre. Si el controlador admite parámetros con nombre y es capaz de describir parámetros, se devuelve el nombre del parámetro en este campo.  
  
 **SQL_DESC_NULLABLE [descriptores de implementación]**  
 En IRDs, este campo de registro SQLSMALLINT de solo lectura es SQL_NULLABLE si la columna puede tener valores NULL, SQL_NO_NULLS si la columna no tiene valores NULL o SQL_NULLABLE_UNKNOWN si no se sabe si la columna acepta valores NULL. Este campo pertenece a la columna del conjunto de resultados, no a la columna base.  
  
 En IPD, este campo siempre se establece en SQL_NULLABLE porque los parámetros dinámicos siempre aceptan valores NULL y no se pueden establecer en una aplicación.  
  
 **SQL_DESC_NUM_PREC_RADIX [All]**  
 Este campo SQLINTEGER donde contiene un valor de 2 Si el tipo de datos del campo SQL_DESC_TYPE es un tipo de datos numérico aproximado, porque el campo SQL_DESC_PRECISION contiene el número de bits. Este campo contiene un valor de 10 si el tipo de datos del campo SQL_DESC_TYPE es un tipo de datos numérico exacto, porque el campo SQL_DESC_PRECISION contiene el número de dígitos decimales. Este campo se establece en 0 para todos los tipos de datos no numéricos.  
  
 **SQL_DESC_OCTET_LENGTH [All]**  
 Este campo de registro SQLLEN contiene la longitud, en bytes, de una cadena de caracteres o un tipo de datos binario. En el caso de los tipos de caracteres de longitud fija o binarios, se trata de la longitud real en bytes. En el caso de los tipos de caracteres de longitud variable o binarios, es la longitud máxima en bytes. Este valor siempre excluye el espacio para el carácter de terminación null para los descriptores de implementación y siempre incluye espacio para el carácter de terminación null para los descriptores de la aplicación. En el caso de los datos de aplicación, este campo contiene el tamaño del búfer. En el caso de APD, este campo solo se define para los parámetros de salida o de entrada y salida.  
  
 **SQL_DESC_OCTET_LENGTH_PTR [descriptores de aplicación]**  
 Este campo de registro SQLLEN * apunta a una variable que contendrá la longitud total en bytes de un argumento dinámico (para los descriptores de parámetros) o de un valor de columna enlazada (para los descriptores de fila).  
  
 Para un APD, este valor se omite para todos los argumentos excepto la cadena de caracteres y el binario; Si este campo apunta a SQL_NTS, el argumento dinámico debe terminar en NULL. Para indicar que un parámetro enlazado será un parámetro de datos en ejecución, una aplicación establece este campo en el registro correspondiente de APD en una variable que, en tiempo de ejecución, contendrá el valor SQL_DATA_AT_EXEC o el resultado de la macro SQL_LEN_DATA_AT_EXEC. Si hay más de un campo de este tipo, SQL_DESC_DATA_PTR se puede establecer en un valor que identifica de forma única el parámetro para ayudar a la aplicación a determinar qué parámetro se solicita.  
  
 Si el OCTET_LENGTH_PTR campo de un ARD es un puntero nulo, el controlador no devuelve la información de longitud de la columna. Si el SQL_DESC_OCTET_LENGTH_PTR campo de un APD es un puntero nulo, el controlador supone que las cadenas de caracteres y los valores binarios terminan en NULL. (Los valores binarios no deben terminar en null, pero se les debe proporcionar una longitud para evitar el truncamiento).  
  
 Si la llamada a **SQLFetch** o **SQLFetchScroll** que rellena el búfer señalado por este campo no devolvió SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, el contenido del búfer es indefinido. Este campo es un *campo aplazado*. No se utiliza en el momento en que se establece, sino que el controlador usa más adelante el controlador para determinar o indicar la longitud del octeto de los datos.  
  
 **SQL_DESC_PARAMETER_TYPE [IPD]**  
 Este campo de registro SQLSMALLINT se establece en SQL_PARAM_INPUT para un parámetro de entrada, SQL_PARAM_INPUT_OUTPUT para un parámetro de entrada/salida, SQL_PARAM_OUTPUT para un parámetro de salida, SQL_PARAM_INPUT_OUTPUT_STREAM para un parámetro de entrada/salida transmitido, o SQL_PARAM_OUTPUT_STREAM para un parámetro de salida transmitido. Está establecido en SQL_PARAM_INPUT de forma predeterminada.  
  
 Para un IPD, el campo se establece en SQL_PARAM_INPUT de forma predeterminada si el controlador no rellena automáticamente IPD (el atributo de la instrucción SQL_ATTR_ENABLE_AUTO_IPD es SQL_FALSE). Una aplicación debe establecer este campo en IPD para los parámetros que no son parámetros de entrada.  
  
 **SQL_DESC_PRECISION [All]**  
 Este campo de registro SQLSMALLINT contiene el número de dígitos para un tipo numérico exacto, el número de bits de la mantisa (precisión binaria) para un tipo numérico aproximado, o el número de dígitos del componente de fracciones de segundo para el tipo de datos SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP o SQL_INTERVAL_SECOND. Este campo no está definido para el resto de tipos de datos.  
  
 El valor de este campo puede ser diferente del valor de "precisión", tal y como se define en ODBC 2 *. x*. Para obtener más información, vea [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_ROWVER [descriptores de implementación]**  
 Este campo SQLSMALLINTrecord indica si el DBMS modifica automáticamente una columna cuando se actualiza una fila (por ejemplo, una columna del tipo "timestamp" en SQL Server). El valor de este campo de registro se establece en SQL_TRUE si la columna es una columna de versiones de fila y en SQL_FALSE de lo contrario. Este atributo de columna es similar a llamar a **SQLSpecialColumns** con IdentifierType de SQL_ROWVER para determinar si una columna se actualiza automáticamente.  
  
 **SQL_DESC_SCALE [All]**  
 Este campo de registro SQLSMALLINT contiene la escala definida para los tipos de datos decimal y Numeric. El campo no está definido para el resto de tipos de datos.  
  
 El valor de este campo puede ser diferente del valor de "Scale", tal y como se define en ODBC 2 *. x*. Para obtener más información, vea [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_SCHEMA_NAME [IRDs]**  
 Este campo de registro SQLCHAR * de solo lectura contiene el nombre de esquema de la tabla base que contiene la columna. El valor devuelto depende del controlador si la columna es una expresión o si la columna forma parte de una vista. Si el origen de datos no admite esquemas o no se puede determinar el nombre del esquema, esta variable contiene una cadena vacía.  
  
 **SQL_DESC_SEARCHABLE [IRDs]**  
 Este campo de registro SQLSMALLINT de solo lectura se establece en uno de los siguientes valores:  
  
-   SQL_PRED_NONE si la columna no se puede usar en una cláusula **Where** . (Es igual que el valor de SQL_UNSEARCHABLE en ODBC 2 *. x*).  
  
-   SQL_PRED_CHAR si la columna se puede utilizar en una cláusula **Where** , pero solo con el predicado **like** . (Es igual que el valor de SQL_LIKE_ONLY en ODBC 2 *. x*).  
  
-   SQL_PRED_BASIC si la columna se puede utilizar en una cláusula **Where** con todos los operadores de comparación excepto **like**. (Es igual que el valor de SQL_EXCEPT_LIKE en ODBC 2 *. x*).  
  
-   SQL_PRED_SEARCHABLE si la columna se puede utilizar en una cláusula **Where** con cualquier operador de comparación.  
  
 **SQL_DESC_TABLE_NAME [IRDs]**  
 Este campo de registro SQLCHAR * de solo lectura contiene el nombre de la tabla base que contiene esta columna. El valor devuelto depende del controlador si la columna es una expresión o si la columna forma parte de una vista.  
  
 **SQL_DESC_TYPE [All]**  
 Este campo de registro SQLSMALLINT especifica el tipo de datos conciso de SQL o C para todos los tipos de datos excepto los tipos de datos datetime y Interval. En el caso de los tipos de datos datetime e Interval, este campo especifica el tipo de datos detallado, que es SQL_DATETIME o SQL_INTERVAL.  
  
 Siempre que este campo contenga SQL_DATETIME o SQL_INTERVAL, el campo de SQL_DESC_DATETIME_INTERVAL_CODE debe contener el subcódigo correspondiente para el tipo conciso. En el caso de los tipos de datos DateTime, SQL_DESC_TYPE contiene SQL_DATETIME y el campo SQL_DESC_DATETIME_INTERVAL_CODE contiene un subcódigo para el tipo de datos datetime específico. En los tipos de datos de intervalo, SQL_DESC_TYPE contiene SQL_INTERVAL y el campo SQL_DESC_DATETIME_INTERVAL_CODE contiene un subcódigo para el tipo de datos de intervalo específico.  
  
 Los valores de los campos SQL_DESC_TYPE y SQL_DESC_CONCISE_TYPE son interdependientes. Cada vez que se establece uno de los campos, también se debe establecer el otro. SQL_DESC_TYPE se puede establecer mediante una llamada a **SQLSetDescField** o **SQLSetDescRec**. SQL_DESC_CONCISE_TYPE se puede establecer mediante una llamada a **SQLBindCol** o **SQLBindParameter**, o **SQLSetDescField**.  
  
 Si SQL_DESC_TYPE se establece en un tipo de datos conciso que no sea un tipo de datos Interval o DateTime, el campo SQL_DESC_CONCISE_TYPE se establece en el mismo valor y el campo SQL_DESC_DATETIME_INTERVAL_CODE se establece en 0.  
  
 Si SQL_DESC_TYPE se establece en el tipo de datos verbose DateTime o Interval (SQL_DATETIME o SQL_INTERVAL) y el campo SQL_DESC_DATETIME_INTERVAL_CODE se establece en el subcódigo correspondiente, el campo tipo de SQL_DESC_CONCISE se establece en el tipo conciso correspondiente. Al intentar establecer SQL_DESC_TYPE en uno de los tipos conciso de DateTime o Interval, se devolverá SQLSTATE HY021 (información de descriptor incoherente).  
  
 Cuando el campo de SQL_DESC_TYPE se establece mediante una llamada a **SQLBindCol**, **SQLBindParameter** o **SQLSetDescField**, los campos siguientes se establecen en los valores predeterminados siguientes, tal como se muestra en la tabla siguiente. Los valores de los campos restantes del mismo registro no están definidos.  
  
|Valor de SQL_DESC_TYPE|Otros campos establecidos implícitamente|  
|------------------------------|---------------------------------|  
|SQL_CHAR, SQL_VARCHAR, SQL_C_CHAR, SQL_C_VARCHAR|SQL_DESC_LENGTH está establecido en 1. SQL_DESC_PRECISION se establece en 0.|  
|SQL_DATETIME|Cuando SQL_DESC_DATETIME_INTERVAL_CODE se establece en SQL_CODE_DATE o SQL_CODE_TIME, SQL_DESC_PRECISION se establece en 0. Cuando se establece en SQL_DESC_TIMESTAMP, SQL_DESC_PRECISION se establece en 6.|  
|SQL_DECIMAL, SQL_NUMERIC, SQL_C_NUMERIC|SQL_DESC_SCALE se establece en 0. SQL_DESC_PRECISION se establece en la precisión definida por la implementación para el tipo de datos respectivo.<br /><br /> Vea [SQL to C: Numeric](../../../odbc/reference/appendixes/sql-to-c-numeric.md) para obtener información sobre cómo enlazar manualmente un valor de SQL_C_NUMERIC.|  
|SQL_FLOAT, SQL_C_FLOAT|SQL_DESC_PRECISION se establece en la precisión predeterminada definida por la implementación para SQL_FLOAT.|  
|SQL_INTERVAL|Cuando SQL_DESC_DATETIME_INTERVAL_CODE se establece en un tipo de datos de intervalo, SQL_DESC_DATETIME_INTERVAL_PRECISION se establece en 2 (la precisión inicial del intervalo predeterminado). Cuando el intervalo tiene un componente de segundos, SQL_DESC_PRECISION se establece en 6 (la precisión de segundos del intervalo predeterminado).|  
  
 Cuando una aplicación llama a **SQLSetDescField** para establecer los campos de un descriptor en lugar de llamar a **SQLSetDescRec**, la aplicación debe declarar primero el tipo de datos. Cuando lo hace, se establecen implícitamente los demás campos indicados en la tabla anterior. Si alguno de los valores establecidos implícitamente es inaceptable, la aplicación puede llamar a **SQLSetDescField** o **SQLSetDescRec** para establecer el valor inaceptable explícitamente.  
  
 **SQL_DESC_TYPE_NAME [descriptores de implementación]**  
 Este campo de registro SQLCHAR * de solo lectura contiene el nombre de tipo dependiente del origen de datos (por ejemplo, "CHAR", "VARCHAR", etc.). Si no se conoce el nombre del tipo de datos, esta variable contiene una cadena vacía.  
  
 **SQL_DESC_UNNAMED [descriptores de implementación]**  
 Este campo de registro SQLSMALLINT de un descriptor de fila lo establece el controlador en SQL_NAMED o SQL_UNNAMED cuando establece el campo SQL_DESC_NAME. Si el campo SQL_DESC_NAME contiene un alias de columna o si no se aplica el alias de columna, el controlador establece el campo SQL_DESC_UNNAMED en SQL_NAMED. Si una aplicación establece el campo de SQL_DESC_NAME de una IPD en un nombre o alias de parámetro, el controlador establece el campo SQL_DESC_UNNAMED de IPD en SQL_NAMED. Si no hay un nombre de columna o un alias de columna, el controlador establece el campo de SQL_DESC_UNNAMED en SQL_UNNAMED.  
  
 Una aplicación puede establecer el campo de SQL_DESC_UNNAMED de una IPD en SQL_UNNAMED. Un controlador devuelve SQLSTATE HY091 (identificador de campo de descriptor no válido) si una aplicación intenta establecer el campo de SQL_DESC_UNNAMED de una IPD en SQL_NAMED. El campo de SQL_DESC_UNNAMED de una IRD es de solo lectura; Se devolverá el valor de SQLSTATE HY091 (identificador de campo de descriptor no válido) si una aplicación intenta establecerlo.  
  
 **SQL_DESC_UNSIGNED [descriptores de implementación]**  
 Este campo de registro SQLSMALLINT de solo lectura se establece en SQL_TRUE si el tipo de columna es sin signo o no numérico, o SQL_FALSE si el tipo de columna está firmado.  
  
 **SQL_DESC_UPDATABLE [IRDs]**  
 Este campo de registro SQLSMALLINT de solo lectura se establece en uno de los siguientes valores:  
  
-   SQL_ATTR_READ_ONLY si la columna del conjunto de resultados es de solo lectura.  
  
-   SQL_ATTR_WRITE si la columna del conjunto de resultados es de lectura y escritura.  
  
-   SQL_ATTR_READWRITE_UNKNOWN si no se sabe si la columna del conjunto de resultados es actualizable o no.  
  
 SQL_DESC_UPDATABLE describe la actualización de la columna en el conjunto de resultados, no la columna de la tabla base. La actualización de la columna de la tabla base en la que se basa esta columna de conjunto de resultados puede ser diferente del valor de este campo. El hecho de que una columna sea actualizable puede basarse en el tipo de datos, los privilegios de usuario y la definición del conjunto de resultados. Si no está claro si una columna es actualizable, se debe devolver SQL_ATTR_READWRITE_UNKNOWN.  
  
## <a name="consistency-checks"></a>Comprobaciones de coherencia  
 El controlador realiza automáticamente una comprobación de coherencia cada vez que una aplicación pasa un valor para el campo SQL_DESC_DATA_PTR de ARD, APD o IPD. Si alguno de los campos no es coherente con otros campos, **SQLSetDescField** devolverá SQLSTATE HY021 (información del descriptor incoherente). Para obtener más información, vea "comprobación de coherencia" en [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar una columna|[SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Enlazar un parámetro|[Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Obtención de un campo de descriptor|[Función SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Obtener varios campos de descriptor|[Función SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Establecer varios campos de descriptor|[Función SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Referencia de API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)
