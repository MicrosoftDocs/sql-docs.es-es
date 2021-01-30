---
description: Función SQLBindParameter
title: SQLBindParameter (función) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBindParameter
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindParameter
helpviewer_keywords:
- SQLBindParameter function [ODBC]
ms.assetid: 38349d4b-be03-46f9-9d6a-e50dd144e225
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d173d3cc38a0f07532f6b9d8a225d9bbf44a6a28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181079"
---
# <a name="sqlbindparameter-function"></a>Función SQLBindParameter

**Conformidad**  
 Versión introducida: ODBC 2,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLBindParameter** enlaza un búfer a un marcador de parámetro en una instrucción SQL. **SQLBindParameter** admite el enlace a un tipo de datos de C de Unicode, aunque el controlador subyacente no admita datos Unicode.  
  
> [!NOTE]  
>  Esta función reemplaza a la función **SQLSetParam** de ODBC 1,0. Para obtener más información, vea "Comentarios".  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp
  
SQLRETURN SQLBindParameter(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT     InputOutputType,  
      SQLSMALLINT     ValueType,  
      SQLSMALLINT     ParameterType,  
      SQLULEN         ColumnSize,  
      SQLSMALLINT     DecimalDigits,  
      SQLPOINTER      ParameterValuePtr,  
      SQLLEN          BufferLength,  
      SQLLEN *        StrLen_or_IndPtr);  
```
  
## <a name="arguments"></a>Argumentos

 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *ParameterNumber*  
 Entradas Número de parámetro, ordenado secuencialmente en aumento del orden de los parámetros, empezando por 1.  
  
 *InputOutputType*  
 Entradas Tipo del parámetro. Para obtener más información, vea "argumento *InputOutputType* " en "Comentarios".  
  
 *ValueType*  
 Entradas El tipo de datos C del parámetro. Para obtener más información, vea "argumento *ValueType* " en "Comentarios".  
  
 *ParameterType*  
 Entradas El tipo de datos SQL del parámetro. Para obtener más información, vea "argumento *ParameterType* " en "Comentarios".  
  
 *ColumnSize*  
 Entradas Tamaño de la columna o expresión del marcador de parámetro correspondiente. Para obtener más información, vea "argumento de *columnas* " en "Comentarios".  
  
 Si la aplicación se ejecutará en un sistema operativo Windows de 64 bits, consulte [la información de ODBC 64](../../../odbc/reference/odbc-64-bit-information.md)bits.  
  
 *DecimalDigits*  
 Entradas Los dígitos decimales de la columna o expresión del marcador de parámetro correspondiente. Para obtener más información sobre el tamaño de las columnas, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *ParameterValuePtr*  
 [Entrada diferida] Un puntero a un búfer para los datos del parámetro. Para obtener más información, vea "argumento *ParameterValuePtr* " en "Comentarios".  
  
 *BufferLength*  
 [Entrada/salida] Longitud del búfer de *ParameterValuePtr* en bytes. Para obtener más información, vea "argumento *BufferLength* " en "Comentarios".  
  
 Consulte la [información de ODBC 64](../../../odbc/reference/odbc-64-bit-information.md)bits, si la aplicación se ejecutará en un sistema operativo de 64 bits.  
  
 *StrLen_or_IndPtr*  
 [Entrada diferida] Un puntero a un búfer para la longitud del parámetro. Para obtener más información, vea "*StrLen_or_IndPtr* argumento" en "Comentarios".  
  
## <a name="returns"></a>Devoluciones

 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico

 Cuando **SQLBindParameter** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que normalmente devuelve **SQLBindParameter** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  

|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|El tipo de datos identificado por el argumento *ValueType* no se puede convertir al tipo de datos identificado por el argumento *ParameterType* . Tenga en cuenta que este error puede ser devuelto por **SQLExecDirect**, **SQLExecute** o **SQLPutData** en tiempo de ejecución, en lugar de por **SQLBindParameter**.|  
|07009|Índice de descriptor no válido|(DM) el valor especificado para el argumento *ParameterNumber* era menor que 1.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer **MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY003|Tipo de búfer de aplicación no válido|El valor especificado por el argumento *ValueType* no era un tipo de datos de C válido o SQL_C_DEFAULT.|  
|HY004|Tipo de datos SQL no válido|El valor especificado para el argumento *ParameterType* no era un identificador de tipo de datos SQL de ODBC válido ni un identificador de tipo de datos SQL específico del controlador compatible con el controlador.|  
|HY009|Valor de argumento no válido|(DM) el argumento *ParameterValuePtr* era un puntero nulo, el argumento *StrLen_or_IndPtr* era un puntero nulo y el argumento *InputOutputType* no se SQL_PARAM_OUTPUT.<br /><br /> (DM) SQL_PARAM_OUTPUT, donde el argumento *ParameterValuePtr* era un puntero nulo, el tipo C era char o binary y BufferLength (*cbValueMax*) era mayor que 0.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a **SQLBindParameter** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY021|Información de descriptor incoherente|La información del descriptor comprobada durante una comprobación de coherencia no era coherente. (Consulte la sección "comprobaciones de coherencia" en **SQLSetDescField**).<br /><br /> El valor especificado para el argumento *ColumnSize* estaba fuera del intervalo de valores admitidos por el origen de datos para una columna del tipo de datos SQL especificado por el argumento *ParameterType* .|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor de *BufferLength* era menor que 0. (Vea la descripción del campo SQL_DESC_DATA_PTR en **SQLSetDescField**).|  
|HY104|Valor de precisión o escala no válido|El valor especificado para el argumento *columnas* o *ColumnSize* se encontraba fuera del intervalo de valores admitidos por el origen de datos para una columna del tipo de datos SQL especificado por el argumento *ParameterType* .|  
|HY105|Tipo de parámetro no válido|(DM) el valor especificado para el argumento *InputOutputType* no era válido. (Vea "Comentarios").|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Característica opcional no implementada|El controlador o el origen de datos no admite la conversión especificada por la combinación del valor especificado para el argumento *ValueType* y el valor específico del controlador especificado para el argumento *ParameterType*.<br /><br /> El valor especificado para el argumento *ParameterType* era un identificador de tipo de datos SQL de ODBC válido para la versión de ODBC admitida por el controlador, pero no era compatible con el controlador o el origen de datos.<br /><br /> El controlador solo admite ODBC 2. *x* y el argumento *ValueType* era uno de los siguientes:<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> y todos los tipos de datos de Interval C enumerados en [tipos de datos de c](../../../odbc/reference/appendixes/c-data-types.md) en los tipos de datos Apéndice D:.<br /><br /> El controlador solo es compatible con las versiones ODBC anteriores a 3,50 y el argumento *ValueType* se SQL_C_GUID.|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios

 Una aplicación llama a **SQLBindParameter** para enlazar cada marcador de parámetro en una instrucción SQL. Los enlaces permanecen en vigor hasta que la aplicación llama de nuevo a **SQLBindParameter** , llama a **SQLFreeStmt** con la opción SQL_RESET_PARAMS, o llama a **SQLSetDescField** para establecer el campo de encabezado SQL_DESC_COUNT de APD en 0.  
  
 Para obtener más información sobre los parámetros, vea parámetros de la [instrucción](../../../odbc/reference/develop-app/statement-parameters.md). Para obtener más información sobre los tipos de datos de parámetros y marcadores de parámetros, vea [tipos de datos de parámetros](../../../odbc/reference/appendixes/parameter-data-types.md) y [marcadores](../../../odbc/reference/appendixes/parameter-markers.md) de parámetros en el Apéndice C: gramática de SQL.  
  
## <a name="parameternumber-argument"></a>Argumento ParameterNumber  
 Si *ParameterNumber* en la llamada a **SQLBindParameter** es mayor que el valor de SQL_DESC_COUNT, se llama a **SQLSetDescField** para aumentar el valor de SQL_DESC_COUNT a *ParameterNumber*.  
  
## <a name="inputoutputtype-argument"></a>Argumento InputOutputType  
 El argumento *InputOutputType* especifica el tipo del parámetro. Este argumento establece el campo de SQL_DESC_PARAMETER_TYPE de IPD. Todos los parámetros de las instrucciones SQL que no llaman a procedimientos, como las instrucciones **Insert** , son *parámetros* de *entrada* . Los parámetros de las llamadas a procedimientos pueden ser de entrada, de entrada/salida o de salida. (Una aplicación llama a **SQLProcedureColumns** para determinar el tipo de un parámetro en una llamada a procedimiento; se supone que los parámetros cuyo tipo no se puede determinar son parámetros de entrada).  
  
 El argumento *InputOutputType* es uno de los valores siguientes:  
  
-   SQL_PARAM_INPUT. El parámetro marca un parámetro en una instrucción SQL que no llama a un procedimiento, como una instrucción **Insert** , o marca un parámetro de entrada en un procedimiento. Por ejemplo, los parámetros de **Insert en valores de empleados (?,?,?)** son parámetros de entrada, mientras que los parámetros de **{call AddEmp (?,?,?)}** pueden ser, pero no necesariamente, parámetros de entrada.  
  
     Cuando se ejecuta la instrucción, el controlador envía datos para el parámetro al origen de datos; el \* búfer *ParameterValuePtr* debe contener un valor de entrada válido o el búfer **StrLen_or_IndPtr* debe contener SQL_NULL_DATA, SQL_DATA_AT_EXEC o el resultado de la macro SQL_LEN_DATA_AT_EXEC.  
  
     Si una aplicación no puede determinar el tipo de un parámetro en una llamada a procedimiento, establece *InputOutputType* en SQL_PARAM_INPUT; Si el origen de datos devuelve un valor para el parámetro, el controlador lo descarta.  
  
-   SQL_PARAM_INPUT_OUTPUT. El parámetro marca un parámetro de entrada/salida en un procedimiento. Por ejemplo, el parámetro de **{Call GetEmpDept (?)}** es un parámetro de entrada y salida que acepta el nombre de un empleado y devuelve el nombre del Departamento del empleado.  
  
     Cuando se ejecuta la instrucción, el controlador envía datos para el parámetro al origen de datos; el \* búfer de *ParameterValuePtr* debe contener un valor de entrada válido o el \* búfer de *StrLen_or_IndPtr* debe contener SQL_NULL_DATA, SQL_DATA_AT_EXEC o el resultado de la macro de SQL_LEN_DATA_AT_EXEC. Una vez ejecutada la instrucción, el controlador devuelve datos para el parámetro a la aplicación; Si el origen de datos no devuelve un valor para un parámetro de entrada/salida, el controlador establece el búfer **StrLen_or_IndPtr* en SQL_NULL_DATA.  
  
    > [!NOTE]  
    >  Cuando una aplicación ODBC 1,0 llama a **SQLSetParam** en un controlador ODBC 2,0, el administrador de controladores lo convierte en una llamada a **SQLBindParameter** en la que el argumento *InputOutputType* se establece en SQL_PARAM_INPUT_OUTPUT.  
  
-   SQL_PARAM_OUTPUT. El parámetro marca el valor devuelto de un procedimiento o un parámetro de salida en un procedimiento. en cualquier caso, se conocen como *parámetros de salida*. Por ejemplo, el parámetro de **{? = Call GetNextEmpID}** es un parámetro de salida que devuelve el siguiente identificador de empleado.  
  
     Una vez ejecutada la instrucción, el controlador devuelve datos para el parámetro a la aplicación, a menos que los argumentos *ParameterValuePtr* y *StrLen_or_IndPtr* sean punteros NULL, en cuyo caso el controlador descarta el valor de salida. Si el origen de datos no devuelve un valor para un parámetro de salida, el controlador establece el búfer **StrLen_or_IndPtr* en SQL_NULL_DATA.  
  
-   SQL_PARAM_INPUT_OUTPUT_STREAM. Indica que se debe transmitir por secuencias un parámetro de entrada/salida. **SQLGetData** puede leer los valores de los parámetros en partes. *BufferLength* se omite porque la longitud del búfer se determinará en la llamada de **SQLGetData**. El valor del búfer *StrLen_or_IndPtr* debe contener SQL_NULL_DATA, SQL_DEFAULT_PARAM, SQL_DATA_AT_EXEC o el resultado de la macro SQL_LEN_DATA_AT_EXEC. Un parámetro se debe enlazar como un parámetro de datos en ejecución (DAE) en la entrada si se transmitirá en la salida. *ParameterValuePtr* puede ser cualquier valor de puntero no nulo que se devolverá por **SQLParamData** como el token definido por el usuario cuyo valor se pasó con *ParameterValuePtr* tanto para la entrada como para la salida. Para obtener más información, vea el tema que trata sobre [recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
-   SQL_PARAM_OUTPUT_STREAM. Igual que SQL_PARAM_INPUT_OUTPUT_STREAM, para un parámetro de salida. **StrLen_or_IndPtr* se omite en la entrada.  
  
 En la tabla siguiente se enumeran las distintas combinaciones de *InputOutputType* y **StrLen_or_IndPtr*:  
  
|*InputOutputType*|**StrLen_or_IndPtr*|Resultado|Comentario en ParameterValuePtr|  
|-----------------------|----------------------------|-------------|---------------------------------|  
|SQL_PARAM_INPUT|SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Entrada en elementos|*ParameterValuePtr* puede ser cualquier valor de puntero que **SQLParamData** devolverá como el token definido por el usuario cuyo valor se pasó con *ParameterValuePtr*.|  
|SQL_PARAM_INPUT|Not SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Búfer enlazado de entrada|*ParameterValuePtr* es la dirección del búfer de entrada.|  
|SQL_PARAM_OUTPUT|Se omite en la entrada.|Búfer enlazado de salida|*ParameterValuePtr* es la dirección del búfer de salida.|  
|SQL_PARAM_OUTPUT_STREAM|Se omite en la entrada.|Salida transmitida|*ParameterValuePtr* puede ser cualquier valor de puntero, que será devuelto por **SQLParamData** como el token definido por el usuario cuyo valor se pasó con *ParameterValuePtr*.|  
|SQL_PARAM_INPUT_OUTPUT|SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Entrada en los elementos y búfer enlazado de salida|*ParameterValuePtr* es la dirección del búfer de salida, que también devolverá **SQLParamData** como el token definido por el usuario cuyo valor se pasó con *ParameterValuePtr*.|  
|SQL_PARAM_INPUT_OUTPUT|Not SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Búfer enlazado de entrada y búfer enlazado de salida|*ParameterValuePtr* es la dirección del búfer de entrada/salida compartido.|
|SQL_PARAM_INPUT_OUTPUT_STREAM|SQL_LEN_DATA_AT_EXEC (*Len*) o SQL_DATA_AT_EXEC|Entrada en elementos y salida transmitida|*ParameterValuePtr* puede ser cualquier valor de puntero que no sea nulo, que será devuelto por **SQLParamData** como el token definido por el usuario cuyo valor se pasó con *ParameterValuePtr* tanto para la entrada como para la salida.|  
  
> [!NOTE]  
>  El controlador debe decidir qué tipos de SQL se permiten cuando una aplicación enlaza un parámetro de salida o de entrada-salida como streaming. El administrador de controladores no generará un error para un tipo SQL no válido.  
  
## <a name="valuetype-argument"></a>Argumento ValueType

 El argumento *ValueType* especifica el tipo de datos C del parámetro. Este argumento establece los campos SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE y SQL_DESC_DATETIME_INTERVAL_CODE de APD. Debe ser uno de los valores de la sección [tipos de datos de C](../../../odbc/reference/appendixes/c-data-types.md) del Apéndice D: tipos de datos.  
  
 Si el argumento *ValueType* es uno de los tipos de datos Interval, el campo SQL_DESC_TYPE del registro *ParameterNumber* de APD se establece en SQL_INTERVAL, el campo SQL_DESC_CONCISE_TYPE de APD se establece en el tipo de datos Interval conciso y el campo SQL_DESC_DATETIME_INTERVAL_CODE del registro *ParameterNumber* se establece en un subcódigo para el tipo de datos Interval específico. (Consulte [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md)). La precisión inicial del intervalo predeterminado (2) y la precisión de los segundos del intervalo predeterminado (6), como se establece en los campos SQL_DESC_DATETIME_INTERVAL_PRECISION y SQL_DESC_PRECISION de APD, respectivamente, se usan para los datos. Si una precisión predeterminada no es adecuada, la aplicación debe establecer explícitamente el campo descriptor mediante una llamada a **SQLSetDescField** o **SQLSetDescRec**.  
  
 Si el argumento *ValueType* es uno de los tipos de datos DateTime, el campo SQL_DESC_TYPE del registro *ParameterNumber* de APD se establece en SQL_DATETIME, el campo SQL_DESC_CONCISE_TYPE del registro *ParameterNumber* del APD se establece en el tipo de datos de DateTime de C conciso y el campo SQL_DESC_DATETIME_INTERVAL_CODE del registro *ParameterNumber* se establece en un subcódigo para el tipo de datos datetime específico. (Consulte [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md)).  
  
 Si el argumento *ValueType* es un tipo de datos SQL_C_NUMERIC, la precisión predeterminada (definida por el controlador) y la escala predeterminada (0), como se establece en los campos SQL_DESC_PRECISION y SQL_DESC_SCALE de APD, se usan para los datos. Si la precisión o la escala predeterminadas no son adecuadas, la aplicación debe establecer explícitamente el campo descriptor mediante una llamada a **SQLSetDescField** o **SQLSetDescRec**.  
  
 SQL_C_DEFAULT especifica que el valor del parámetro se transfiera desde el tipo de datos C predeterminado para el tipo de datos SQL especificado con *ParameterType*.  
  
 También puede especificar un tipo de datos de C extendido. Para obtener más información, vea el tema sobre [tipos de datos C en ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 Para obtener más información, vea [tipos de datos de c predeterminados](../../../odbc/reference/appendixes/default-c-data-types.md), [convertir datos de c a tipos de datos de SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)y [convertir datos de tipos de datos de SQL a c](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) en el Apéndice D: tipos de datos.  
  
## <a name="parametertype-argument"></a>Argumento ParameterType

 Debe ser uno de los valores enumerados en la sección [tipos de datos de SQL](../../../odbc/reference/appendixes/sql-data-types.md) del Apéndice D: tipos de datos o debe ser un valor específico del controlador. Este argumento establece los campos SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE y SQL_DESC_DATETIME_INTERVAL_CODE de IPD.  
  
 Si el argumento *ParameterType* es uno de los identificadores de fecha y hora, el campo SQL_DESC_TYPE de IPD se establece en SQL_DATETIME, el campo SQL_DESC_CONCISE_TYPE de IPD se establece en el tipo de datos SQL conciso y el campo SQL_DESC_DATETIME_INTERVAL_CODE se establece en el valor de subcódigo DateTime adecuado.  
  
 Si *ParameterType* es uno de los identificadores de intervalo, el campo de SQL_DESC_TYPE de IPD se establece en SQL_INTERVAL, el campo SQL_DESC_CONCISE_TYPE de IPD se establece en el tipo de datos de intervalo SQL conciso y el campo SQL_DESC_DATETIME_INTERVAL_CODE de IPD se establece en el subcódigo de intervalo adecuado. El campo de SQL_DESC_DATETIME_INTERVAL_PRECISION de IPD se establece en la precisión inicial del intervalo y el campo SQL_DESC_PRECISION se establece en la precisión de segundos del intervalo, si procede. Si el valor predeterminado de SQL_DESC_DATETIME_INTERVAL_PRECISION o SQL_DESC_PRECISION no es adecuado, la aplicación debe establecerlo explícitamente mediante una llamada a **SQLSetDescField**. Para obtener más información acerca de cualquiera de estos campos, vea [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Si el argumento *ValueType* es un tipo de datos SQL_NUMERIC, la precisión predeterminada (definida por el controlador) y la escala predeterminada (0), como se establece en los campos SQL_DESC_PRECISION y SQL_DESC_SCALE de IPD, se usan para los datos. Si la precisión o la escala predeterminadas no son adecuadas, la aplicación debe establecer explícitamente el campo descriptor mediante una llamada a **SQLSetDescField** o **SQLSetDescRec**.  
  
 Para obtener información sobre cómo se convierten los datos, vea [convertir datos de c a tipos de datos SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) y [convertir datos de SQL a tipos de datos de c](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) en el Apéndice D: tipos de datos.  
  
## <a name="columnsize-argument"></a>Argumento de columnas

 El argumento *columnas* especifica el tamaño de la columna o expresión que corresponde al marcador de parámetro, la longitud de los datos o ambos. Este argumento establece distintos campos de IPD, dependiendo del tipo de datos SQL (el argumento *ParameterType* ). Las siguientes reglas se aplican a esta asignación:  
  
-   Si *ParameterType* es SQL_CHAR, SQL_VARCHAR, SQL_LONGVARCHAR, SQL_BINARY, SQL_VARBINARY, SQL_LONGVARBINARY o uno de los tipos de datos de intervalo o fecha y hora de SQL concisos, el campo SQL_DESC_LENGTH de IPD se establece en el valor de *columnas*. (Para obtener más información, vea la sección [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño para mostrar](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) del Apéndice D: tipos de datos).  
  
-   Si *ParameterType* es SQL_DECIMAL, SQL_NUMERIC, SQL_FLOAT, SQL_REAL o SQL_DOUBLE, el campo SQL_DESC_PRECISION de IPD se establece en el valor de *columnas*.  
  
-   Para otros tipos de datos, se omite el argumento *columnas* .  
  
 Para obtener más información, vea "pasar valores de parámetro" y SQL_DATA_AT_EXEC en "*StrLen_or_IndPtr* argumento".  
  
## <a name="decimaldigits-argument"></a>Argumento ColumnSize

 Si *ParameterType* es SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP, SQL_INTERVAL_SECOND, SQL_INTERVAL_DAY_TO_SECOND, SQL_INTERVAL_HOUR_TO_SECOND o SQL_INTERVAL_MINUTE_TO_SECOND, el campo SQL_DESC_PRECISION de IPD se establece en *ColumnSize*. Si *ParameterType* es SQL_NUMERIC o SQL_DECIMAL, el campo SQL_DESC_SCALE de IPD se establece en *ColumnSize*. Para el resto de tipos de datos, se omite el argumento *ColumnSize* .  
  
## <a name="parametervalueptr-argument"></a>Argumento ParameterValuePtr

 El argumento *ParameterValuePtr* apunta a un búfer que, cuando se llama a **SQLExecute** o **SQLExecDirect** , contiene los datos reales del parámetro. Los datos deben estar en el formato especificado por el argumento *ValueType* . Este argumento establece el campo de SQL_DESC_DATA_PTR de APD. Una aplicación puede establecer el argumento *ParameterValuePtr* en un puntero nulo, siempre que *\* StrLen_or_IndPtr* sea SQL_NULL_DATA o SQL_DATA_AT_EXEC. (Esto solo se aplica a los parámetros de entrada o de entrada y salida).  
  
 Si \* *StrLen_or_IndPtr* es el resultado de la macro de SQL_LEN_DATA_AT_EXEC (*longitud*) o SQL_DATA_AT_EXEC, *ParameterValuePtr* es un valor de puntero definido por la aplicación que está asociado al parámetro. Se devuelve a la aplicación a través de **SQLParamData**. Por ejemplo, *ParameterValuePtr* puede ser un token distinto de cero, como un número de parámetro, un puntero a datos o un puntero a una estructura que la aplicación usó para enlazar parámetros de entrada. Sin embargo, tenga en cuenta que si el parámetro es un parámetro de entrada/salida, *ParameterValuePtr* debe ser un puntero a un búfer en el que se almacenará el valor de salida. Si el valor del atributo de instrucción SQL_ATTR_PARAMSET_SIZE es mayor que 1, la aplicación puede usar el valor al que apunta el atributo de instrucción SQL_ATTR_PARAMS_PROCESSED_PTR junto con el argumento *ParameterValuePtr* . Por ejemplo, *ParameterValuePtr* podría apuntar a una matriz de valores y la aplicación podría usar el valor al que apunta SQL_ATTR_PARAMS_PROCESSED_PTR para recuperar el valor correcto de la matriz. Para obtener más información, vea "pasar valores de parámetro" más adelante en esta sección.  
  
 Si el argumento *InputOutputType* es SQL_PARAM_INPUT_OUTPUT o SQL_PARAM_OUTPUT, *ParameterValuePtr* apunta a un búfer en el que el controlador devuelve el valor de salida. Si el procedimiento devuelve uno o más conjuntos de resultados, \* no se garantiza que el búfer *ParameterValuePtr* se establezca hasta que se hayan procesado todos los conjuntos de resultados o filas. Si el búfer no se establece hasta que se complete el procesamiento, los parámetros de salida y los valores devueltos no estarán disponibles hasta que **SQLMoreResults** devuelva SQL_NO_DATA. Llamar a **SQLCloseCursor** o **SQLFreeStmt** con una opción de SQL_CLOSE hará que se descarten estos valores.  
  
 Si el valor del atributo de instrucción SQL_ATTR_PARAMSET_SIZE es mayor que 1, *ParameterValuePtr* apunta a una matriz. Una única instrucción SQL procesa la matriz completa de valores de entrada para un parámetro de entrada o de entrada/salida y devuelve una matriz de valores de salida para un parámetro de entrada/salida o de salida.  
  
## <a name="bufferlength-argument"></a>Argumento BufferLength

 En el caso de los datos de caracteres y binarios de C, el argumento *BufferLength* especifica la longitud del \* búfer *ParameterValuePtr* (si es un elemento único) o la longitud de un elemento en la \* matriz *ParameterValuePtr* (si el valor del atributo de instrucción SQL_ATTR_PARAMSET_SIZE es mayor que 1). Este argumento establece el campo de registro de SQL_DESC_OCTET_LENGTH de APD. Si la aplicación especifica varios valores, *BufferLength* se usa para determinar la ubicación de los valores de la matriz **ParameterValuePtr* , tanto en la entrada como en la salida. Para los parámetros de entrada/salida y de salida, se usa para determinar si se truncarán los datos de C binarios y de caracteres en la salida:  
  
-   En el caso de los datos de caracteres C, si el número de bytes disponibles para devolver es mayor o igual que *BufferLength*, los datos de \* *ParameterValuePtr* se truncan a *BufferLength* menos la longitud de un carácter de terminación NULL y el controlador termina en NULL.  
  
-   En el caso de los datos binarios de C, si el número de bytes disponibles para devolver es mayor que *BufferLength*, los datos de \* *ParameterValuePtr* se truncan en *BufferLength* bytes.  
  
 Para todos los demás tipos de datos de C, se omite el argumento *BufferLength* . La longitud del búfer \* *ParameterValuePtr* (si es un elemento único) o la longitud de un elemento en la \* matriz *ParameterValuePtr* (si la aplicación llama a **SQLSetStmtAttr** con un argumento de *atributo* de SQL_ATTR_PARAMSET_SIZE para especificar varios valores para cada parámetro) se supone que es la longitud del tipo de datos C.  
  
 En el caso de los parámetros de entrada y salida transmitidos por secuencias, el argumento *BufferLength* se omite porque la longitud del búfer se especifica en **SQLGetData**.  
  
> [!NOTE]  
>  Cuando una aplicación ODBC 1,0 llama a **SQLSetParam** en un ODBC 3. *x* , el administrador de controladores lo convierte en una llamada a **SQLBindParameter** en la que el argumento *BufferLength* siempre se SQL_SETPARAM_VALUE_MAX. Dado que el administrador de controladores devuelve un error si se trata de un ODBC 3. la aplicación *x* establece *BufferLength* en SQL_SETPARAM_VALUE_MAX, un ODBC 3. el controlador *x* puede utilizarlo para determinar cuándo lo llama una aplicación ODBC 1,0.  
  
> [!NOTE]  
>  En **SQLSetParam**, la forma en que una aplicación especifica la longitud del búfer **ParameterValuePtr* para que el controlador pueda devolver datos de caracteres o binarios, y el modo en que una aplicación envía una matriz de valores de parámetro de caracteres o binarios al controlador, está definido por el controlador.  
  
## <a name="strlen_or_indptr-argument"></a>StrLen_or_IndPtr argumento

 El argumento *StrLen_or_IndPtr* apunta a un búfer que, cuando se llama a **SQLExecute** o **SQLExecDirect** , contiene uno de los siguientes. (Este argumento establece los campos registro de SQL_DESC_OCTET_LENGTH_PTR y SQL_DESC_INDICATOR_PTR de los punteros de parámetros de la aplicación).  
  
-   La longitud del valor del parámetro almacenado en **ParameterValuePtr*. Esto se omite excepto en el caso de los datos de C de caracteres o binarios.  
  
-   SQL_NTS. El valor del parámetro es una cadena terminada en NULL.  
  
-   SQL_NULL_DATA. El valor del parámetro es NULL.  
  
-   SQL_DEFAULT_PARAM. Un procedimiento es usar el valor predeterminado de un parámetro, en lugar de un valor recuperado de la aplicación. Este valor solo es válido en un procedimiento llamado en la sintaxis canónica de ODBC y, a continuación, solo si el argumento *InputOutputType* es SQL_PARAM_INPUT, SQL_PARAM_INPUT_OUTPUT o SQL_PARAM_INPUT_OUTPUT_STREAM. Cuando \* *StrLen_or_IndPtr* se SQL_DEFAULT_PARAM, los argumentos *ValueType*, *ParameterType*, *ColumnSize*, *BufferLength* y *ParameterValuePtr* se omiten para los parámetros de entrada y solo se usan para definir el valor del parámetro de salida para los parámetros de entrada y salida.   
  
-   Resultado de la macro de SQL_LEN_DATA_AT_EXEC (*longitud*). Los datos para el parámetro se enviarán con **SQLPutData**. Si el argumento *ParameterType* es SQL_LONGVARBINARY, SQL_LONGVARCHAR o un tipo de datos Long, específico del origen de datos, y el controlador devuelve "Y" para el tipo de información SQL_NEED_LONG_DATA_LEN en **SQLGetInfo**, *length* es el número de bytes de datos que se van a enviar para el parámetro; de lo contrario, la *longitud* debe ser un valor no negativo y se omite. Para obtener más información, vea "pasar valores de parámetro", más adelante en esta sección.  
  
     Por ejemplo, para especificar que 10.000 bytes de datos se enviarán con **SQLPutData** en una o varias llamadas, para un parámetro SQL_LONGVARCHAR, una aplicación establece **StrLen_or_IndPtr* en SQL_LEN_DATA_AT_EXEC (10000).  
  
-   SQL_DATA_AT_EXEC. Los datos para el parámetro se enviarán con **SQLPutData**. Las aplicaciones ODBC 1,0 usan este valor cuando llaman a ODBC 3. Controladores *x* . Para obtener más información, vea "pasar valores de parámetro", más adelante en esta sección.  
  
 Si *StrLen_or_IndPtr* es un puntero nulo, el controlador supone que todos los valores de parámetro de entrada no son nulos y que los datos de caracteres y binarios están terminados en NULL. Si *InputOutputType* es SQL_PARAM_OUTPUT o SQL_PARAM_OUTPUT_STREAM y *ParameterValuePtr* y *StrLen_or_IndPtr* son punteros nulos, el controlador descarta el valor de salida.  
  
> [!NOTE]  
>  No se recomienda a los desarrolladores de aplicaciones especificar un puntero nulo para *StrLen_or_IndPtr* cuando el tipo de datos del parámetro es SQL_C_BINARY. Para asegurarse de que un controlador no trunca inesperadamente SQL_C_BINARY datos, *StrLen_or_IndPtr* debe contener un puntero a un valor de longitud válido.  
  
 Si el argumento *InputOutputType* es SQL_PARAM_INPUT_OUTPUT, SQL_PARAM_OUTPUT, SQL_PARAM_INPUT_OUTPUT_STREAM o SQL_PARAM_OUTPUT_STREAM, *StrLen_or_IndPtr* apunta a un búfer en el que el controlador devuelve SQL_NULL_DATA, el número de bytes disponibles para devolver en \* *ParameterValuePtr* (sin incluir el byte de terminación null de los datos de caracteres) o SQL_NO_TOTAL (si no se puede determinar el número de bytes disponibles para devolver). Si el procedimiento devuelve uno o más conjuntos de resultados, no se garantiza que el búfer **StrLen_or_IndPtr* se establezca hasta que se hayan capturado todos los resultados.  
  
 Si el valor del atributo de instrucción SQL_ATTR_PARAMSET_SIZE es mayor que 1, *StrLen_or_IndPtr* apunta a una matriz de valores SQLLEN. Pueden ser cualquiera de los valores enumerados anteriormente en esta sección y se procesan con una única instrucción SQL.  
  
## <a name="passing-parameter-values"></a>Pasar valores de parámetro

 Una aplicación puede pasar el valor de un parámetro en el búfer de \* *ParameterValuePtr* o con una o más llamadas a **SQLPutData**. Los parámetros cuyos datos se pasan con **SQLPutData** se conocen como parámetros de *datos en ejecución* . Normalmente se usan para enviar datos de SQL_LONGVARBINARY y SQL_LONGVARCHAR parámetros, y se pueden combinar con otros parámetros.  
  
 Para pasar valores de parámetro, una aplicación realiza la siguiente secuencia de pasos:  
  
1.  Llama a **SQLBindParameter** para cada parámetro para enlazar los búferes del valor del parámetro (argumento *ParameterValuePtr* ) y la longitud/indicador (argumento *StrLen_or_IndPtr* ). En el caso de los parámetros de datos en ejecución, *ParameterValuePtr* es un valor de puntero definido por la aplicación, como un número de parámetro o un puntero a datos. El valor se devolverá más adelante y se puede usar para identificar el parámetro.  
  
2.  Coloca valores para los parámetros de entrada y de entrada/salida en los \* búferes *ParameterValuePtr* y **StrLen_or_IndPtr* :  
  
    -   En el caso de los parámetros normales, la aplicación coloca el valor del parámetro en el búfer de \* *ParameterValuePtr* y la longitud de ese valor en el búfer **StrLen_or_IndPtr* . Para obtener más información, vea [establecer valores de parámetro](../../../odbc/reference/develop-app/setting-parameter-values.md).  
  
    -   En el caso de los parámetros de datos en ejecución, la aplicación coloca el resultado de la macro SQL_LEN_DATA_AT_EXEC (*longitud*) (cuando se llama a un controlador ODBC 2,0) en el búfer **StrLen_or_IndPtr* .  
  
3.  Llama a **SQLExecute** o **SQLExecDirect** para ejecutar la instrucción SQL.  
  
    -   Si no hay parámetros de datos en ejecución, el proceso se completa.  
  
    -   Si hay parámetros de datos en ejecución, la función devuelve SQL_NEED_DATA.  
  
4.  Llama a **SQLParamData** para recuperar el valor definido por la aplicación especificado en el argumento *ParameterValuePtr* de **SQLBindParameter** para el primer parámetro de datos en ejecución que se va a procesar. **SQLParamData** devuelve SQL_NEED_DATA.  
  
    > [!NOTE]  
    >  Aunque los parámetros de datos en ejecución se parecen a las columnas de datos en ejecución, el valor devuelto por **SQLParamData** es diferente para cada uno. Los parámetros de datos en ejecución son parámetros de una instrucción SQL para los que se enviarán datos con **SQLPutData** cuando se ejecute la instrucción con **SQLExecDirect** o **SQLExecute**. Se enlazan con **SQLBindParameter**. El valor devuelto por **SQLParamData** es un valor de puntero que se pasa a **SQLBindParameter** en el argumento *ParameterValuePtr* . Las columnas de datos en ejecución son columnas de un conjunto de filas para el que se enviarán datos con **SQLPutData** cuando una fila se actualice o se agregue con **SQLBulkOperations** o se actualice con **SQLSetPos**. Están enlazadas con **SQLBindCol**. El valor devuelto por **SQLParamData** es la dirección de la fila en el búfer **TargetValuePtr* (establecido por una llamada a **SQLBindCol**) que se está procesando.  
  
5.  Llama a **SQLPutData** una o más veces para enviar datos para el parámetro. Se necesita más de una llamada si el valor de los datos es mayor que el \* búfer de *ParameterValuePtr* especificado en **SQLPutData**; solo se permiten varias llamadas a **SQLPutData** para el mismo parámetro cuando se envían datos de caracteres c a una columna con un tipo de datos de carácter, binario o de origen de datos o cuando se envían datos binarios de c a una columna con un tipo de datos de carácter, Binari  
  
6.  Llama de nuevo a **SQLParamData** para indicar que se han enviado todos los datos para el parámetro.  
  
    -   Si hay más parámetros de datos en ejecución, **SQLParamData** devuelve SQL_NEED_DATA y el valor definido por la aplicación para el siguiente parámetro de datos en ejecución que se va a procesar. La aplicación repite los pasos 4 y 5.  
  
    -   Si no hay más parámetros de datos en ejecución, el proceso se completa. Si la instrucción se ejecutó correctamente, **SQLParamData** devuelve SQL_SUCCESS o SQL_SUCCESS_WITH_INFO; Si se produce un error en la ejecución, devuelve SQL_ERROR. En este punto, **SQLParamData** puede devolver cualquier SQLSTATE que pueda ser devuelto por la función que se usa para ejecutar la instrucción (**SQLExecDirect** o **SQLExecute**).  
  
         Los valores de salida de los parámetros de entrada/salida o de salida están disponibles en los \* búferes *ParameterValuePtr* y **StrLen_or_IndPtr* después de que la aplicación recupere todos los conjuntos de resultados generados por la instrucción.  
  
 Al llamar a **SQLExecute** o **SQLExecDirect** , la instrucción se coloca en un estado SQL_NEED_DATA. En este punto, la aplicación solo puede llamar a **SQLCancel**, **SQLGetDiagField**, **SQLGetDiagRec**, **SQLGetFunctions**, **SQLParamData** o **SQLPutData** con la instrucción o el *identificador de conexión* asociado a la instrucción. Si llama a cualquier otra función con la instrucción o la conexión asociada a la instrucción, la función devuelve SQLSTATE HY010 (error de secuencia de función). La instrucción deja el estado SQL_NEED_DATA cuando **SQLParamData** o **SQLPutData** devuelve un error, **SQLParamData** devuelve SQL_SUCCESS o SQL_SUCCESS_WITH_INFO o se cancela la instrucción.  
  
 Si la aplicación llama a **SQLCancel** mientras que el controlador todavía necesita datos para los parámetros de datos en ejecución, el controlador cancela la ejecución de la instrucción. a continuación, la aplicación puede llamar a **SQLExecute** o **SQLExecDirect** de nuevo.  
  
## <a name="retrieving-streamed-output-parameters"></a>Recuperación de parámetros de salida transmitidos

 Cuando una aplicación establece *InputOutputType* en SQL_PARAM_INPUT_OUTPUT_STREAM o SQL_PARAM_OUTPUT_STREAM, el valor del parámetro de salida debe ser recuperado por una o varias llamadas a **SQLGetData**. Cuando el controlador tiene un valor de parámetro de salida transmitido por secuencias para volver a la aplicación, devolverá SQL_PARAM_DATA_AVAILABLE en respuesta a una llamada a las siguientes funciones: **SQLMoreResults**, **SQLExecute** y **SQLExecDirect**. Una aplicación llama a **SQLParamData** para determinar qué valor de parámetro está disponible.  
  
 Para obtener más información sobre SQL_PARAM_DATA_AVAILABLE y los parámetros de salida transmitidos, consulte [recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="using-arrays-of-parameters"></a>Utilizar matrices de parámetros

 Cuando una aplicación prepara una instrucción con marcadores de parámetros y pasa en una matriz de parámetros, hay dos formas diferentes de ejecutarse. Una manera es que el controlador se base en las capacidades de procesamiento de matrices del back-end, en cuyo caso toda la instrucción con la matriz de parámetros se trata como una unidad atómica. Oracle es un ejemplo de un origen de datos que admite capacidades de procesamiento de matrices. Otra manera de implementar esta característica es que el controlador genere un lote de instrucciones SQL, una instrucción SQL para cada conjunto de parámetros en la matriz de parámetros y ejecute el lote. Las matrices de parámetros no se pueden usar con una instrucción **Update WHERE CURRENT of** .  
  
 Cuando se procesa una matriz de parámetros, los conjuntos de resultados individuales o los recuentos de filas (uno para cada conjunto de parámetros) pueden estar disponibles o los conjuntos de resultados o filas se pueden acumular en uno. La opción SQL_PARAM_ARRAY_ROW_COUNTS de **SQLGetInfo** indica si los recuentos de filas están disponibles para cada conjunto de parámetros (SQL_PARC_BATCH) o si solo hay disponible un recuento de filas (SQL_PARC_NO_BATCH).  
  
 La opción SQL_PARAM_ARRAY_SELECTS de **SQLGetInfo** indica si un conjunto de resultados está disponible para cada conjunto de parámetros (SQL_PAS_BATCH) o si solo hay un conjunto de resultados disponible (SQL_PAS_NO_BATCH). Si el controlador no permite que se ejecute una instrucción de generación de conjuntos de resultados con una matriz de parámetros, SQL_PARAM_ARRAY_SELECTS devuelve SQL_PAS_NO_SELECT.  
  
 Para obtener más información, vea [función SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 Para admitir matrices de parámetros, el atributo de instrucción SQL_ATTR_PARAMSET_SIZE se establece para especificar el número de valores de cada parámetro. Si el campo es mayor que 1, los campos SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR de APD deben apuntar a matrices. La cardinalidad de cada matriz es igual al valor de SQL_ATTR_PARAMSET_SIZE.  
  
 El campo SQL_DESC_ROWS_PROCESSED_PTR de APD señala a un búfer que contiene el número de conjuntos de parámetros que se han procesado, incluidos los conjuntos de errores. A medida que se procesa cada conjunto de parámetros, el controlador almacena un nuevo valor en el búfer. No se devolverá ningún número si se trata de un puntero nulo. Cuando se usan matrices de parámetros, el valor al que apunta el campo de SQL_DESC_ROWS_PROCESSED_PTR de APD se rellena aunque la función de configuración devuelva SQL_ERROR. Si se devuelve SQL_NEED_DATA, el valor al que apunta el campo SQL_DESC_ROWS_PROCESSED_PTR de APD se establece en el conjunto de parámetros que se está procesando.  
  
 Lo que ocurre cuando se enlaza una matriz de parámetros y una instrucción **Update donde Current of** se ejecuta está definida por el controlador.  
  
## <a name="column-wise-parameter-binding"></a>Column-Wise enlace de parámetros

 En el enlace de modo de columna, la aplicación enlaza matrices de parámetros y de longitud/indicador independientes a cada parámetro.  
  
 Para usar el enlace de modo de columna, la aplicación establece primero el atributo de instrucción SQL_ATTR_PARAM_BIND_TYPE en SQL_PARAM_BIND_BY_COLUMN. (Este es el valor predeterminado). Para cada columna que se va a enlazar, la aplicación realiza los siguientes pasos:  
  
1.  Asigna una matriz de búferes de parámetros.  
  
2.  Asigna una matriz de búferes de longitud/indicador.  
  
    > [!NOTE]  
    >  Si la aplicación escribe directamente en los descriptores cuando se usa el enlace de modo de columna, se pueden usar matrices independientes para los datos de longitud y indicador.  
  
3.  Llama a **SQLBindParameter** con los argumentos siguientes:  
  
    -   *ValueType* es el tipo C de un único elemento en la matriz de búferes de parámetros.  
  
    -   *ParameterType* es el tipo SQL del parámetro.  
  
    -   *ParameterValuePtr* es la dirección de la matriz de búferes de parámetros.  
  
    -   *BufferLength* es el tamaño de un único elemento en la matriz de búferes de parámetros. El argumento *BufferLength* se omite cuando los datos son datos de longitud fija.  
  
    -   *StrLen_or_IndPtr* es la dirección de la matriz de longitud/indicador.  
  
 Para obtener más información sobre cómo se usa esta información, vea "argumento ParameterValuePtr" en "Comentarios", más adelante en esta sección. Para obtener más información sobre el enlace de modo de columna de parámetros, vea [enlazar matrices de parámetros](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md).  
  
## <a name="row-wise-parameter-binding"></a>Row-Wise enlace de parámetros

 En el enlace de modo de fila, la aplicación define una estructura que contiene el parámetro y los búferes de longitud/indicador de cada parámetro que se va a enlazar.  
  
 Para usar el enlace de modo de fila, la aplicación realiza los pasos siguientes:  
  
1.  Define una estructura que contiene un único conjunto de parámetros (incluidos los búferes de parámetros y de longitud/indicador) y asigna una matriz de estas estructuras.  
  
    > [!NOTE]  
    >  Si la aplicación escribe directamente en los descriptores cuando se usa el enlace de modo de fila, se pueden usar campos independientes para los datos de longitud y indicador.  
  
2.  Establece el atributo de instrucción SQL_ATTR_PARAM_BIND_TYPE en el tamaño de la estructura que contiene un único conjunto de parámetros o en el tamaño de una instancia de un búfer al que se enlazarán los parámetros. La longitud debe incluir espacio para todos los parámetros enlazados y cualquier relleno de la estructura o búfer, para asegurarse de que cuando la dirección de un parámetro enlazado se incrementa con la longitud especificada, el resultado apunta al principio del mismo parámetro en la fila siguiente. Cuando se usa el operador *sizeof* en ANSI C, se garantiza este comportamiento.  
  
3.  Llama a **SQLBindParameter** con los siguientes argumentos para cada parámetro que se va a enlazar:  
  
    -   *ValueType* es el tipo del miembro de búfer de parámetros que se va a enlazar a la columna.  
  
    -   *ParameterType* es el tipo SQL del parámetro.  
  
    -   *ParameterValuePtr* es la dirección del miembro del búfer de parámetros en el primer elemento de la matriz.  
  
    -   *BufferLength* es el tamaño del miembro del búfer de parámetros.  
  
    -   *StrLen_or_IndPtr* es la dirección del miembro de longitud/indicador que se va a enlazar.  
  
 Para obtener más información sobre cómo se usa esta información, vea "argumento *ParameterValuePtr* ", más adelante en esta sección. Para obtener más información sobre el enlace de modo de fila de los parámetros, vea las [matrices de enlaces de parámetros](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md).  
  
## <a name="error-information"></a>Información del error

 Si un controlador no implementa matrices de parámetros como lotes (la opción SQL_PARAM_ARRAY_ROW_COUNTS es igual a SQL_PARC_NO_BATCH), las situaciones de error se administran como si se ejecutara una instrucción. Si el controlador implementa matrices de parámetros como lotes, una aplicación puede utilizar el campo de encabezado SQL_DESC_ARRAY_STATUS_PTR de IPD para determinar qué parámetro de una instrucción SQL o qué parámetro de una matriz de parámetros ha provocado que **SQLExecDirect** o **SQLExecute** devuelvan un error. Este campo contiene información de estado para cada fila de valores de parámetro. Si el campo indica que se ha producido un error, los campos de la estructura de datos de diagnóstico indicarán la fila y el número de parámetro del parámetro en el que se produjo el error. El número de elementos de la matriz se definirá mediante el SQL_DESC_ARRAY_SIZE campo de encabezado de APD, que se puede establecer mediante el atributo de instrucción SQL_ATTR_PARAMSET_SIZE.  
  
> [!NOTE]  
>  El campo de encabezado SQL_DESC_ARRAY_STATUS_PTR de APD se usa para omitir los parámetros. Para obtener más información acerca de cómo omitir los parámetros, vea la sección siguiente, "omitir un conjunto de parámetros".  
  
 Cuando **SQLExecute** o **SQLExecDirect** devuelve SQL_ERROR, los elementos de la matriz a los que apunta el campo SQL_DESC_ARRAY_STATUS_PTR de IPD contendrán SQL_PARAM_ERROR, SQL_PARAM_SUCCESS, SQL_PARAM_SUCCESS_WITH_INFO, SQL_PARAM_UNUSED o SQL_PARAM_DIAG_UNAVAILABLE.  
  
 Para cada elemento de esta matriz, la estructura de datos de diagnóstico contiene uno o más registros de estado. El campo SQL_DIAG_ROW_NUMBER de la estructura indica el número de fila de los valores de parámetro que produjeron el error. Si es posible determinar el parámetro determinado en una fila de parámetros que causó el error, el número de parámetro se especificará en el campo SQL_DIAG_COLUMN_NUMBER.  
  
 SQL_PARAM_UNUSED se escribe cuando no se ha usado un parámetro porque se ha producido un error en un parámetro anterior que forzó **SQLExecute** o **SQLExecDirect** se anulan. Por ejemplo, si hay 50 parámetros y se produjo un error al ejecutar el conjunto de parámetros fortieth que provocó la anulación de **SQLExecute** o **SQLExecDirect** , SQL_PARAM_UNUSED se escribe en la matriz de estado para los parámetros de 41 a 50.  
  
 SQL_PARAM_DIAG_UNAVAILABLE se especifica cuando el controlador trata las matrices de parámetros como una unidad monolítica, por lo que no genera este nivel de parámetro individual de información de error.  
  
 Algunos errores en el procesamiento de un conjunto único de parámetros provocan que se detenga el procesamiento de los conjuntos de parámetros posteriores en la matriz. Otros errores no afectan al procesamiento de los parámetros subsiguientes. Los errores detendrán el procesamiento definido por el controlador. Si no se detiene el procesamiento, se procesan todos los parámetros de la matriz, se devuelve SQL_SUCCESS_WITH_INFO como resultado del error y el búfer definido por SQL_ATTR_PARAMS_PROCESSED_PTR se establece en el número total de conjuntos de parámetros procesados (tal y como se define en el atributo de instrucción SQL_ATTR_PARAMSET_SIZE), que incluye los conjuntos de errores.  
  
> [!CAUTION]  
>  El comportamiento de ODBC cuando se produce un error en el procesamiento de una matriz de parámetros es diferente en ODBC 3. *x* que estaba en ODBC 2. *x*. En ODBC 2. *x*, la función devolvió SQL_ERROR y se ha dejado de procesar. El búfer señalado por el argumento *Pirow* de **SQLParamOptions** contenía el número de la fila de error. En ODBC 3. *x*, la función devuelve SQL_SUCCESS_WITH_INFO y el procesamiento puede detenerse o continuar. Si continúa, el búfer especificado por SQL_ATTR_PARAMS_PROCESSED_PTR se establecerá en el valor de todos los parámetros procesados, incluidos los que dieron como resultado un error. Este cambio de comportamiento puede causar problemas en las aplicaciones existentes.  
  
 Cuando **SQLExecute** o **SQLExecDirect** devuelve antes de completar el procesamiento de todos los conjuntos de parámetros en una matriz de parámetros, por ejemplo, cuando se devuelve SQL_ERROR o SQL_NEED_DATA, la matriz de estado contiene los Estados de los parámetros que ya se han procesado. La ubicación a la que señala el campo SQL_DESC_ROWS_PROCESSED_PTR en IPD contiene el número de fila en la matriz de parámetros que causó el código de error de SQL_ERROR o SQL_NEED_DATA. Cuando se envía una matriz de parámetros a una instrucción SELECT, la disponibilidad de los valores de la matriz de estado está definida por el controlador. pueden estar disponibles después de que se haya ejecutado la instrucción o cuando se capturen los conjuntos de resultados.  
  
## <a name="ignoring-a-set-of-parameters"></a>Omitir un conjunto de parámetros

 El campo SQL_DESC_ARRAY_STATUS_PTR de APD (establecido por el atributo de instrucción SQL_ATTR_PARAM_STATUS_PTR) se puede usar para indicar que se debe omitir un conjunto de parámetros enlazados en una instrucción SQL. Para indicar al controlador que omita uno o más conjuntos de parámetros durante la ejecución, una aplicación debe seguir estos pasos:  
  
1.  Llame a **SQLSetDescField** para establecer el campo de encabezado SQL_DESC_ARRAY_STATUS_PTR de APD de forma que señale a una matriz de valores SQLUSMALLINT para que contenga información de estado. Este campo también se puede establecer mediante una llamada a **SQLSetStmtAttr** con un *atributo* de SQL_ATTR_PARAM_OPERATION_PTR, lo que permite que una aplicación establezca el campo sin obtener un identificador de descriptor.  
  
2.  Establezca cada elemento de la matriz definido por el campo SQL_DESC_ARRAY_STATUS_PTR del APD en uno de dos valores:  
  
    -   SQL_PARAM_IGNORE, para indicar que la fila se excluye de la ejecución de la instrucción.  
  
    -   SQL_PARAM_PROCEED, para indicar que la fila se incluye en la ejecución de la instrucción.  
  
3.  Llame a **SQLExecDirect** o **SQLExecute** para ejecutar la instrucción preparada.  
  
 Las siguientes reglas se aplican a la matriz definida por el campo SQL_DESC_ARRAY_STATUS_PTR de APD:  
  
-   De forma predeterminada, el puntero se establece en NULL.  
  
-   Si el puntero es null, se usan todos los conjuntos de parámetros, como si todos los elementos estuvieran establecidos en SQL_ROW_PROCEED.  
  
-   Si se establece un elemento en SQL_PARAM_PROCEED, no se garantiza que la operación usará ese conjunto determinado de parámetros.  
  
-   SQL_PARAM_PROCEED se define como 0 en el archivo de encabezado.  
  
 Una aplicación puede establecer el campo de SQL_DESC_ARRAY_STATUS_PTR de APD para que señale a la misma matriz que apunta por el campo SQL_DESC_ARRAY_STATUS_PTR de IRD. Esto resulta útil cuando se enlazan parámetros a los datos de fila. Los parámetros se pueden pasar por alto según el estado de los datos de la fila. Además de SQL_PARAM_IGNORE, los códigos siguientes hacen que se omita un parámetro en una instrucción SQL: SQL_ROW_DELETED, SQL_ROW_UPDATED y SQL_ROW_ERROR. Además de SQL_PARAM_PROCEED, los códigos siguientes hacen que una instrucción SQL continúe: SQL_ROW_SUCCESS, SQL_ROW_SUCCESS_WITH_INFO y SQL_ROW_ADDED.  
  
## <a name="rebinding-parameters"></a>Reenlazar parámetros

 Una aplicación puede realizar cualquiera de las dos operaciones para cambiar un enlace:  
  
-   Llame a **SQLBindParameter** para especificar un nuevo enlace para una columna que ya está enlazada. El controlador sobrescribe el enlace anterior con el nuevo.  
  
-   Especifique el desplazamiento que se va a agregar a la dirección del búfer especificado por la llamada de enlace a **SQLBindParameter**. Para obtener más información, vea la sección siguiente, "reenlazar con desplazamientos".  
  
## <a name="rebinding-with-offsets"></a>Reenlazar con desplazamientos

 El reenlace de parámetros es especialmente útil cuando una aplicación tiene una configuración de área de búfer que puede contener muchos parámetros pero una llamada a **SQLExecDirect** o **SQLExecute** solo usa algunos de los parámetros. El espacio restante en el área de búfer se puede usar para el siguiente conjunto de parámetros modificando el enlace existente por un desplazamiento.  
  
 El campo de encabezado SQL_DESC_BIND_OFFSET_PTR de APD apunta al desplazamiento de enlace. Si el campo no es null, el controlador desreferencia el puntero y, si ninguno de los valores de los campos SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR es un puntero nulo, agrega el valor desreferenciado a esos campos en los registros descriptores en tiempo de ejecución. Los nuevos valores de puntero se usan cuando se ejecutan las instrucciones SQL. El desplazamiento sigue siendo válido después del reenlace. Dado que SQL_DESC_BIND_OFFSET_PTR es un puntero al desplazamiento en lugar del desplazamiento en sí, una aplicación puede cambiar el desplazamiento directamente, sin tener que llamar a [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) o [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md) para cambiar el campo descriptor. De forma predeterminada, el puntero se establece en NULL. El campo SQL_DESC_BIND_OFFSET_PTR del ARD se puede establecer mediante una llamada a [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) o mediante una llamada a [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)con un *fAttribute* de SQL_ATTR_PARAM_BIND_OFFSET_PTR.  
  
 El desplazamiento de enlace siempre se agrega directamente a los valores de los campos SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR y SQL_DESC_OCTET_LENGTH_PTR. Si el desplazamiento se cambia a un valor diferente, el nuevo valor todavía se agrega directamente al valor en cada campo de descriptor. El nuevo desplazamiento no se agrega a la suma del valor del campo y de los desplazamientos anteriores.  
  
## <a name="descriptors"></a>Descriptores de

 La forma en que se enlaza un parámetro viene determinada por los campos de APD y IPD. Los argumentos de **SQLBindParameter** se usan para establecer los campos de descriptor. Los campos también se pueden establecer mediante las funciones **SQLSetDescField** , aunque **SQLBindParameter** es más eficaz, ya que la aplicación no tiene que obtener un identificador de descriptor para llamar a **SQLBindParameter**.  
  
> [!CAUTION]  
>  Llamar a **SQLBindParameter** para una instrucción puede afectar a otras instrucciones. Esto sucede cuando el ARD asociado a la instrucción se asigna explícitamente y también está asociado a otras instrucciones. Dado que **SQLBindParameter** modifica los campos de APD, las modificaciones se aplican a todas las instrucciones a las que está asociado este descriptor. Si este no es el comportamiento necesario, la aplicación debe desasociar Este descriptor de las demás instrucciones antes de llamar a **SQLBindParameter**.  
  
 Conceptualmente, **SQLBindParameter** realiza los pasos siguientes en la secuencia:  
  
1.  Llama a [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) para obtener el identificador de APD.  
  
2.  Llama a [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) para obtener el campo de SQL_DESC_COUNT de APD y si el valor del argumento *ColumnNumber* supera el valor de SQL_DESC_COUNT, llama a **SQLSetDescField** para aumentar el valor de SQL_DESC_COUNT a *ColumnNumber*.  
  
3.  Llama a [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) varias veces para asignar valores a los campos siguientes de APD:  
  
    -   Establece SQL_DESC_TYPE y SQL_DESC_CONCISE_TYPE en el valor de *ValueType*, salvo que si *ValueType* es uno de los identificadores concisos de un subtipo DateTime o Interval, establece SQL_DESC_TYPE en SQL_DATETIME o SQL_INTERVAL, respectivamente, establece SQL_DESC_CONCISE_TYPE en el identificador conciso y establece SQL_DESC_DATETIME_INTERVAL_CODE en el correspondiente subcódigo DateTime o Interval.  
  
    -   Establece el campo de SQL_DESC_OCTET_LENGTH en el valor de *BufferLength*.  
  
    -   Establece el campo de SQL_DESC_DATA_PTR en el valor de *ParameterValue*.  
  
    -   Establece el SQL_DESC_OCTET_LENGTH_PTR campo en el valor de *StrLen_or_Ind*.  
  
    -   Establece el campo de SQL_DESC_INDICATOR_PTR también en el valor de *StrLen_or_Ind*.  
  
     El parámetro *StrLen_or_Ind* especifica tanto la información del indicador como la longitud del valor del parámetro.  
  
4.  Llama a [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) para obtener el identificador de IPD.  
  
5.  Llama a [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) para obtener el campo de SQL_DESC_COUNT de IPD y si el valor del argumento *ColumnNumber* supera el valor de SQL_DESC_COUNT, llama a **SQLSetDescField** para aumentar el valor de SQL_DESC_COUNT a *ColumnNumber*.  
  
6.  Llama a [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) varias veces para asignar valores a los siguientes campos de IPD:  
  
    -   Establece SQL_DESC_TYPE y SQL_DESC_CONCISE_TYPE en el valor de *ParameterType*, con la excepción de que si *ParameterType* es uno de los identificadores concisos de un subtipo DateTime o Interval, establece SQL_DESC_TYPE en SQL_DATETIME o SQL_INTERVAL, respectivamente, establece SQL_DESC_CONCISE_TYPE en el identificador conciso y establece SQL_DESC_DATETIME_INTERVAL_CODE en el correspondiente subcódigo DateTime o Interval.  
  
    -   Establece uno o varios SQL_DESC_LENGTH, SQL_DESC_PRECISION y SQL_DESC_DATETIME_INTERVAL_PRECISION, según corresponda para *ParameterType*.  
  
    -   Establece SQL_DESC_SCALE en el valor de *ColumnSize*.  
  
 Si se produce un error en la llamada a **SQLBindParameter** , el contenido de los campos de descriptor que habría establecido en APD no está definido y el campo de SQL_DESC_COUNT de APD no cambia. Además, los campos SQL_DESC_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE y SQL_DESC_TYPE del registro adecuado en IPD son indefinidos y el campo SQL_DESC_COUNT de IPD no se modifica.  
  
## <a name="conversion-of-calls-to-and-from-sqlsetparam"></a>Conversión de llamadas a y desde SQLSetParam

 Cuando una aplicación ODBC 1,0 llama a **SQLSetParam** en un ODBC 3. controlador *x* , ODBC 3. el administrador de controladores *x* asigna la llamada como se muestra en la tabla siguiente.  
  
|Llamada por la aplicación ODBC 1,0|Llame a ODBC 3. controlador *x*|  
|----------------------------------|-------------------------------|  
|SQLSetParam (StatementHandle, ParameterNumber, ValueType, ParameterType, LengthPrecision, ParameterScale, ParameterValuePtr, StrLen_or_IndPtr);|SQLBindParameter (StatementHandle, ParameterNumber, SQL_PARAM_INPUT_OUTPUT, ValueType, ParameterType,      *columnas*,      *ColumnSize*, ParameterValuePtr, SQL_SETPARAM_VALUE_MAX, StrLen_or_IndPtr);|  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-use-sqlbindparameter-function"></a>A. Usar la función SQLBindParameter

 En el ejemplo siguiente, una aplicación prepara una instrucción SQL para insertar datos en la tabla ORDERs. Para cada parámetro de la instrucción, la aplicación llama a **SQLBindParameter** para especificar el tipo de datos C de ODBC y el tipo de datos SQL del parámetro, y para enlazar un búfer a cada parámetro. Para cada fila de datos, la aplicación asigna valores de datos a cada parámetro y llama a **SQLExecute** para ejecutar la instrucción.  
  
 En el ejemplo siguiente se supone que tiene un origen de datos ODBC en el equipo denominado Northwind que está asociado a la base de datos Northwind.  
  
 Para obtener más ejemplos de código, vea [función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), función [SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md), [función SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)y [función SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp
// SQLBindParameter_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define EMPLOYEE_ID_LEN 10  
  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLSMALLINT sCustID;  
  
SQLCHAR szEmployeeID[EMPLOYEE_ID_LEN];  
SQL_DATE_STRUCT dsOrderDate;  
SQLINTEGER cbCustID = 0, cbOrderDate = 0, cbEmployeeID = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, EMPLOYEE_ID_LEN, 0, szEmployeeID, 0, &cbEmployeeID);  
   retcode = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_SSHORT, SQL_INTEGER, 0, 0, &sCustID, 0, &cbCustID);  
   retcode = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TIMESTAMP, sizeof(dsOrderDate), 0, &dsOrderDate, 0, &cbOrderDate);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"INSERT INTO Orders(CustomerID, EmployeeID, OrderDate) VALUES (?, ?, ?)", SQL_NTS);  
  
   strcpy_s((char*)szEmployeeID, _countof(szEmployeeID), "BERGS");  
   sCustID = 5;  
   dsOrderDate.year = 2006;  
   dsOrderDate.month = 3;  
   dsOrderDate.day = 17;  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
### <a name="b-execute-a-stored-procedure-using-a-named-parameter"></a>B. Ejecutar un procedimiento almacenado mediante un parámetro con nombre

 En el ejemplo siguiente, una aplicación ejecuta un SQL Server procedimiento almacenado mediante un parámetro con nombre.  
  
```cpp
// SQLBindParameter_Function_2.cpp  
// compile with: ODBC32.lib  
// sample assumes the following stored procedure:  
// use northwind  
// DROP PROCEDURE SQLBindParameter  
// GO  
//   
// CREATE PROCEDURE SQLBindParameter @quote int  
// AS  
// delete from orders where OrderID >= @quote  
// GO  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
SQLHDESC hIpd = NULL;  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLCHAR szQuote[50] = "100084";  
SQLINTEGER cbValue = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"{call SQLBindParameter(?)}", SQL_NTS);  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 50, 0, szQuote, 0, &cbValue);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_IMP_PARAM_DESC, &hIpd, 0, 0);  
   retcode = SQLSetDescField(hIpd, 1, SQL_DESC_NAME, "@quote", SQL_NTS);  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Devolver información sobre un parámetro en una instrucción|[Función SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Liberar búferes de parámetros en la instrucción|[Función SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Devolver el número de parámetros de instrucción|[Función SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)|  
|Devolver el siguiente parámetro para el que se van a enviar datos|[Función SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Especificar varios valores de parámetro|[Función SQLParamOptions](../../../odbc/reference/syntax/sqlparamoptions-function.md)|  
|Enviar datos de parámetros en tiempo de ejecución|[Función SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>Consulte también

 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Recuperar parámetros de salida mediante SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
