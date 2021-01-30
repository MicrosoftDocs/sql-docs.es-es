---
description: Función SQLDescribeParam
title: Función SQLDescribeParam | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDescribeParam
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeParam
helpviewer_keywords:
- SQLDescribeParam function [ODBC]
ms.assetid: 1f5b63c4-2f3e-44da-b155-876405302281
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8459ca7dd493ba8ab316f832951170f87a77664
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192038"
---
# <a name="sqldescribeparam-function"></a>Función SQLDescribeParam
**Conformidad**  
 Versión introducida: ODBC 1,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLDescribeParam** devuelve la descripción de un marcador de parámetro asociado a una instrucción SQL preparada. Esta información también está disponible en los campos de IPD.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLDescribeParam(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT *   DataTypePtr,  
      SQLULEN *       ParameterSizePtr,  
      SQLSMALLINT *   DecimalDigitsPtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *ParameterNumber*  
 Entradas Número de marcador de parámetro ordenado secuencialmente en aumento del orden de los parámetros, empezando por 1.  
  
 *DataTypePtr*  
 Genere Puntero a un búfer en el que se va a devolver el tipo de datos SQL del parámetro. Este valor se lee desde el campo registro de SQL_DESC_CONCISE_TYPE de IPD. Este será uno de los valores de la sección [tipos de datos de SQL](../../../odbc/reference/appendixes/sql-data-types.md) de los tipos de datos de, o un tipo de datos SQL específico del controlador.  
  
 En ODBC 3. *x*, SQL_TYPE_DATE, SQL_TYPE_TIME o SQL_TYPE_TIMESTAMP se devolverán en *\* DataTypePtr* para los datos de fecha, hora o marca de tiempo, respectivamente, en ODBC 2.** se devolverá x, SQL_DATE, SQL_TIME o SQL_TIMESTAMP. El administrador de controladores realiza las asignaciones necesarias cuando se trata de un ODBC 2. la aplicación *x* está trabajando con un ODBC 3. controlador *x* o cuando se trata de un ODBC 3. la aplicación *x* está trabajando con ODBC 2. controlador *x* .  
  
 Cuando *ColumnNumber* es igual a 0 (para una columna de marcador), se devuelve SQL_BINARY en *\* DataTypePtr* para los marcadores de longitud variable. (Se devuelve SQL_INTEGER si los marcadores se usan en ODBC 3. aplicación *x* que trabaja con ODBC 2. controlador *x* o ODBC 2. aplicación *x* que trabaja con ODBC 3. controlador *x* ).  
  
 Para obtener más información, vea [tipos de datos SQL](../../../odbc/reference/appendixes/sql-data-types.md) en el Apéndice D: tipos de datos. Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador.  
  
 *ParameterSizePtr*  
 Genere Puntero a un búfer en el que se va a devolver el tamaño, en caracteres, de la columna o expresión del marcador de parámetro correspondiente tal y como se define en el origen de datos. Para obtener más información sobre el tamaño de las columnas, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *DecimalDigitsPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número de dígitos decimales de la columna o expresión del parámetro correspondiente tal y como se define en el origen de datos. Para obtener más información acerca de los dígitos decimales, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *NullablePtr*  
 Genere Puntero a un búfer en el que se va a devolver un valor que indica si el parámetro permite valores NULL. Este valor se lee desde el campo SQL_DESC_NULLABLE de IPD. Uno de los siguientes:  
  
-   SQL_NO_NULLS: el parámetro no permite valores NULL (este es el valor predeterminado).  
  
-   SQL_NULLABLE: el parámetro permite valores NULL.  
  
-   SQL_NULLABLE_UNKNOWN: el controlador no puede determinar si el parámetro permite valores NULL.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLDescribeParam** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores SQLSTATE que devuelve normalmente **SQLDescribeParam** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07009|Índice de descriptor no válido|(DM) el valor especificado para el argumento *ParameterNumber* es menor que 1.<br /><br /> El valor especificado para el argumento *ParameterNumber* era mayor que el número de parámetros de la instrucción SQL asociada.<br /><br /> El marcador de parámetro formaba parte de una instrucción no DML.<br /><br /> El marcador de parámetro formaba parte de una lista de **selección** .|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|21S01|La lista de valores de inserción no coincide con la lista de columnas|El número de parámetros de la instrucción **Insert** no coincidía con el número de columnas de la tabla mencionada en la instrucción.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a la función antes de llamar a **SQLPrepare** o **SQLExecDirect** para *StatementHandle*.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLDescribeParam** .<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
## <a name="comments"></a>Comentarios  
 Los marcadores de parámetros se numeran al aumentar el orden de los parámetros, comenzando por 1, en el orden en que aparecen en la instrucción SQL.  
  
 **SQLDescribeParam** no devuelve el tipo (entrada, entrada/salida ni salida) de un parámetro en una instrucción SQL. Excepto en las llamadas a procedimientos, todos los parámetros de las instrucciones SQL son parámetros de entrada. Para determinar el tipo de cada parámetro en una llamada a un procedimiento, una aplicación llama a **SQLProcedureColumns**.  
  
 Para obtener más información, vea [Descripción de los parámetros](../../../odbc/reference/develop-app/describing-parameters.md).  
  
## <a name="code-example"></a>Ejemplo de código  
 En el ejemplo siguiente se solicita al usuario una instrucción SQL y, a continuación, se prepara esa instrucción. A continuación, llama a **SQLNumParams** para determinar si la instrucción contiene algún parámetro. Si la instrucción contiene parámetros, llama a **SQLDescribeParam** para describir esos parámetros y **SQLBindParameter** para enlazarlos. Por último, solicita al usuario los valores de los parámetros y, a continuación, ejecuta la instrucción.  
  
```cpp  
SQLCHAR       Statement[100];  
SQLSMALLINT   NumParams, i, DataType, DecimalDigits, Nullable;  
SQLUINTEGER   ParamSize;  
SQLHSTMT      hstmt;  
  
// Prompt the user for an SQL statement and prepare it.  
GetSQLStatement(Statement);  
SQLPrepare(hstmt, Statement, SQL_NTS);  
  
// Check to see if there are any parameters. If so, process them.  
SQLNumParams(hstmt, &NumParams);  
if (NumParams) {  
   // Allocate memory for three arrays. The first holds pointers to buffers in which  
   // each parameter value will be stored in character form. The second contains the  
   // length of each buffer. The third contains the length/indicator value for each  
   // parameter.  
   SQLPOINTER * PtrArray = (SQLPOINTER *) malloc(NumParams * sizeof(SQLPOINTER));  
   SQLINTEGER * BufferLenArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
   SQLINTEGER * LenOrIndArray = (SQLINTEGER *) malloc(NumParams * sizeof(SQLINTEGER));  
  
   for (i = 0; i < NumParams; i++) {  
   // Describe the parameter.  
   SQLDescribeParam(hstmt, i + 1, &DataType, &ParamSize, &DecimalDigits, &Nullable);  
  
   // Call a helper function to allocate a buffer in which to store the parameter  
   // value in character form. The function determines the size of the buffer from  
   // the SQL data type and parameter size returned by SQLDescribeParam and returns  
   // a pointer to the buffer and the length of the buffer.  
   AllocParamBuffer(DataType, ParamSize, &PtrArray[i], &BufferLenArray[i]);  
  
   // Bind the memory to the parameter. Assume that we only have input parameters.  
   SQLBindParameter(hstmt, i + 1, SQL_PARAM_INPUT, SQL_C_CHAR, DataType, ParamSize,  
         DecimalDigits, PtrArray[i], BufferLenArray[i],  
         &LenOrIndArray[i]);  
  
   // Prompt the user for the value of the parameter and store it in the memory  
   // allocated earlier. For simplicity, this function does not check the value  
   // against the information returned by SQLDescribeParam. Instead, the driver does  
   // this when the statement is executed.  
   GetParamValue(PtrArray[i], BufferLenArray[i], &LenOrIndArray[i]);  
   }  
}  
  
// Execute the statement.  
SQLExecute(hstmt);  
  
// Process the statement further, such as retrieving results (if any) and closing the  
// cursor (if any). Code not shown.  
  
// Free the memory allocated for each parameter and the memory allocated for the arrays  
// of pointers, buffer lengths, and length/indicator values.  
for (i = 0; i < NumParams; i++) free(PtrArray[i]);  
free(PtrArray);  
free(BufferLenArray);  
free(LenOrIndArray);  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a un parámetro|[Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Preparar una instrucción para su ejecución|[Función SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
