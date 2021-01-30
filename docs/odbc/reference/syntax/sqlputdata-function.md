---
description: Función SQLPutData
title: Función SQLPutData | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPutData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPutData
helpviewer_keywords:
- SQLPutData function [ODBC]
ms.assetid: 9a60f004-1477-4c54-a20c-7378e1116713
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f76a3624222968aabf91f9aff012e9370aa8ac8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192561"
---
# <a name="sqlputdata-function"></a>Función SQLPutData
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLPutData** permite a una aplicación enviar datos de un parámetro o una columna al controlador en el momento de la ejecución de la instrucción. Esta función se puede usar para enviar valores de datos binarios o de caracteres en partes a una columna con un tipo de datos de caracteres, binarios o de orígenes de datos (por ejemplo, parámetros de los tipos SQL_LONGVARBINARY o SQL_LONGVARCHAR). **SQLPutData** admite el enlace a un tipo de datos de C de Unicode, aunque el controlador subyacente no admita datos Unicode.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLPutData(  
      SQLHSTMT     StatementHandle,  
      SQLPOINTER   DataPtr,  
      SQLLEN       StrLen_or_Ind);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *DataPtr*  
 Entradas Puntero a un búfer que contiene los datos reales del parámetro o de la columna. Los datos deben estar en el tipo de datos C especificado en el argumento *ValueType* de **SQLBindParameter** (para los datos de parámetros) o en el argumento *TargetType* de **SQLBindCol** (para los datos de columna).  
  
 *StrLen_or_Ind*  
 Entradas Longitud de \* *DataPtr*. Especifica la cantidad de datos enviados en una llamada a **SQLPutData**. La cantidad de datos puede variar con cada llamada para un parámetro o columna determinados. *StrLen_or_Ind* se omite a menos que cumpla una de las condiciones siguientes:  
  
-   *StrLen_or_Ind* es SQL_NTS, SQL_NULL_DATA o SQL_DEFAULT_PARAM.  
  
-   El tipo de datos C especificado en **SQLBindParameter** o **SQLBindCol** es SQL_C_CHAR o SQL_C_BINARY.  
  
-   El tipo de datos C es SQL_C_DEFAULT y el tipo de datos C predeterminado para el tipo de datos SQL especificado es SQL_C_CHAR o SQL_C_BINARY.  
  
 En el caso de todos los demás tipos de datos de C, si *StrLen_or_Ind* no es SQL_NULL_DATA o SQL_DEFAULT_PARAM, el controlador supone que el tamaño del \* *búfer DataPtr* es el tamaño del tipo de datos de C especificado con *ValueType* o *TargetType* y envía el valor de datos completo. Para obtener más información, vea [convertir datos de C a tipos de datos SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) en el Apéndice D: tipos de datos.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLPutData** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLPutData** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|Los datos binarios o de cadena devueltos para un parámetro de salida dieron como resultado el truncamiento de datos binarios de caracteres no vacíos o no NULOs. Si fuera un valor de cadena, se truncó a la derecha. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07006|Infracción de atributo de tipo de datos restringido|El valor de datos identificado por el argumento *ValueType* en **SQLBindParameter** para el parámetro enlazado no se pudo convertir al tipo de datos identificado por el argumento *ParameterType* en **SQLBindParameter**.|  
|07S01|Uso no válido del parámetro predeterminado|Se SQL_DEFAULT_PARAM un valor de parámetro, establecido con **SQLBindParameter**, y el parámetro correspondiente no tenía un valor predeterminado.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|22001|Datos de cadena, truncamiento derecho|La asignación de un valor de carácter o binario a una columna dio como resultado el truncamiento de caracteres no vacíos (caracteres) o no nulos (binarios) o bytes.<br /><br /> El tipo de información SQL_NEED_LONG_DATA_LEN en **SQLGetInfo** era "y" y se enviaron más datos para un parámetro Long (el tipo de datos era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo de datos específico del origen de datos) que se especificó con el argumento *StrLen_or_IndPtr* en **SQLBindParameter**.<br /><br /> El tipo de información SQL_NEED_LONG_DATA_LEN en **SQLGetInfo** era "y" y se enviaron más datos para una columna larga (el tipo de datos era SQL_LONGVARCHAR, SQL_LONGVARBINARY o un tipo de datos específico de origen de datos) que se especificó en el búfer de longitud correspondiente a una columna de una fila de datos que se agregó o actualizó con **SQLBulkOperations** o se actualizó con **SQLSetPos**.|  
|22003|Valor numérico fuera del intervalo|Los datos que se envían para una columna o parámetro numérico enlazado produjeron la parte entera (en oposición a fraccionaria) del número que se va a truncar cuando se asigna a la columna de la tabla asociada.<br /><br /> La devolución de un valor numérico (como Numeric o String) para uno o varios parámetros de entrada/salida o de salida habría causado la parte entera (en oposición a fraccionario) del número que se va a truncar.|  
|22007|Formato de fecha y hora no válido|Los datos enviados para un parámetro o columna enlazados a una estructura de fecha, hora o marca de tiempo era, respectivamente, una fecha, una hora o una marca de tiempo no válidas.<br /><br /> Un parámetro de entrada/salida o de salida se ha enlazado a una estructura de fecha, hora o marca de tiempo de C, y un valor en el parámetro devuelto era, respectivamente, una fecha, una hora o una marca de tiempo no válidas. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|22008|Desbordamiento de campo de fecha y hora|Una expresión DateTime calculada para un parámetro de entrada/salida o de salida dio como resultado una estructura de fecha, hora o marca de tiempo de C que no era válida.|  
|22012|División por cero|Una expresión aritmética calculada para un parámetro de entrada/salida o de salida dio como resultado una división por cero.|  
|22015|Desbordamiento de campo de intervalo|Los datos enviados para un tipo de datos numérico exacto o de intervalo o un parámetro a un tipo de datos SQL de intervalo producen una pérdida de dígitos significativos.<br /><br /> Los datos se enviaron para una columna de intervalo o un parámetro con más de un campo, se convirtió a un tipo de datos numérico y no tenían representación en el tipo de datos numérico.<br /><br /> Los datos enviados para los datos de columna o parámetro se asignaron a un tipo SQL de intervalo y no había ninguna representación del valor del tipo C en el tipo SQL de intervalo.<br /><br /> Los datos enviados para un parámetro o una columna de intervalo C exacto o de intervalo para un tipo C de intervalo producen una pérdida de dígitos significativos.<br /><br /> Los datos enviados para los datos de columna o parámetro se asignaron a una estructura de intervalo C y no había ninguna representación de los datos en la estructura de datos de intervalo.|  
|22018|Valor de carácter no válido para la especificación de conversión|El tipo C era un tipo de datos numérico exacto o aproximado, un valor de fecha y hora o un intervalo. el tipo SQL de la columna era un tipo de datos de caracteres. y el valor de la columna o del parámetro no era un literal válido del tipo C enlazado.<br /><br /> El tipo SQL era un tipo numérico exacto o aproximado, un valor de fecha y hora o un tipo de datos de intervalo; se SQL_C_CHAR el tipo C; y el valor de la columna o del parámetro no era un literal válido del tipo SQL enlazado.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY009|Uso no válido de puntero nulo|(DM) el argumento *DataPtr* era un puntero nulo y el argumento *StrLen_or_Ind* no era 0, SQL_DEFAULT_PARAM o SQL_NULL_DATA.|  
|HY010|Error de secuencia de función|(DM) la llamada de función anterior no era una llamada a **SQLPutData** o **SQLParamData**.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función SQLPutData.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY019|Datos no binarios y no de caracteres enviados en fragmentos|Se llamó a **SQLPutData** más de una vez para un parámetro o columna, y no se usaba para enviar datos de caracteres c a una columna con un tipo de datos de caracteres, binarios o de orígenes de datos, o para enviar datos binarios de c a una columna con un tipo de datos de caracteres, binarios o de orígenes de datos.|  
|HY020|Intento de concatenar un valor null|Se llamó a **SQLPutData** más de una vez desde la llamada que devolvió SQL_NEED_DATA, y en una de esas llamadas, el *StrLen_or_Ind* argumento contiene SQL_NULL_DATA o SQL_DEFAULT_PARAM.|  
|HY090|Longitud de búfer o cadena no válida|El argumento *DataPtr* no era un puntero nulo y el argumento *StrLen_or_Ind* era menor que 0 pero no es igual a SQL_NTS o SQL_NULL_DATA.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
 Si se llama a **SQLPutData** mientras se envían datos para un parámetro en una instrucción SQL, puede devolver cualquier SQLSTATE que pueda ser devuelto por la función a la que se llama para ejecutar la instrucción (**SQLExecute** o **SQLExecDirect**). Si se llama al envío de datos para una columna que se está actualizando o agregando con **SQLBulkOperations** o se está actualizando con **SQLSetPos**, puede devolver cualquier SQLSTATE que pueda ser devuelto por **SQLBulkOperations** o **SQLSetPos**.  
  
## <a name="comments"></a>Comentarios  
 Se puede llamar a **SQLPutData** para proporcionar datos de datos en ejecución para dos usos: datos de parámetro que se van a usar en una llamada a **SQLExecute** o **SQLExecDirect**, o datos de columna que se van a usar cuando se actualice una fila o se agregue mediante una llamada a **SQLBulkOperations** o se actualice mediante una llamada a **SQLSetPos**.  
  
 Cuando una aplicación llama a **SQLParamData** para determinar qué datos debe enviar, el controlador devuelve un indicador que la aplicación puede usar para determinar qué datos de parámetro se van a enviar o dónde se pueden encontrar los datos de la columna. También devuelve SQL_NEED_DATA, que es un indicador de la aplicación que debe llamar a **SQLPutData** para enviar los datos. En el argumento *DataPtr* de **SQLPutData**, la aplicación pasa un puntero al búfer que contiene los datos reales del parámetro o de la columna.  
  
 Cuando el controlador devuelve SQL_SUCCESS para **SQLPutData**, la aplicación llama a **SQLParamData** de nuevo. **SQLParamData** devuelve SQL_NEED_DATA si es necesario enviar más datos, en cuyo caso la aplicación llama de nuevo a **SQLPutData** . Devuelve SQL_SUCCESS si se han enviado todos los datos de datos en ejecución. A continuación, la aplicación llama a **SQLParamData** de nuevo. Si el controlador devuelve SQL_NEED_DATA y otro indicador en *\* ValuePtrPtr*, requiere datos para otro parámetro o columna y se llama de nuevo a **SQLPutData** . Si el controlador devuelve SQL_SUCCESS, se envían todos los datos de datos en ejecución y se puede ejecutar la instrucción SQL, o bien se puede procesar la llamada a **SQLBulkOperations** o **SQLSetPos** .  
  
 Para obtener más información sobre cómo se pasan los datos de parámetros de datos en ejecución en el tiempo de ejecución de la instrucción, vea "pasar valores de parámetro" en [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) y [enviar datos largos](../../../odbc/reference/develop-app/sending-long-data.md). Para obtener más información sobre cómo se actualizan o agregan los datos de las columnas de datos en ejecución, vea la sección "usar SQLSetPos" en [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md), "realizar actualizaciones masivas mediante marcadores" en [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)y [Long Data y SQLSetPos y SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md).  
  
> [!NOTE]  
>  Una aplicación puede utilizar **SQLPutData** para enviar datos en partes solo cuando se envían datos de caracteres c a una columna con un tipo de datos de caracteres, binarios o de orígenes de datos, o cuando se envían datos binarios de c a una columna con un tipo de datos de caracteres, binarios o de orígenes de datos. Si se llama a **SQLPutData** más de una vez en cualquier otra condición, devuelve SQL_ERROR y SQLSTATE HY019 (datos no binarios y de caracteres no binarios enviados en fragmentos).  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se supone que se trata de un nombre de origen de datos denominado test. La base de datos asociada debe tener una tabla que se puede crear, como se indica a continuación:  
  
```sql  
CREATE TABLE emp4 (NAME char(30), AGE int, BIRTHDAY datetime, Memo1 text)  
```  
  
```cpp  
// SQLPutData.cpp  
// compile with: odbc32.lib user32.lib  
#include <stdio.h>  
#include <windows.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define TEXTSIZE  12000  
#define MAXBUFLEN 256  
  
SQLHENV henv = SQL_NULL_HENV;  
SQLHDBC hdbc1 = SQL_NULL_HDBC;       
SQLHSTMT hstmt1 = SQL_NULL_HSTMT;  
  
void Cleanup() {  
   if (hstmt1 != SQL_NULL_HSTMT)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
  
   if (hdbc1 != SQL_NULL_HDBC) {  
      SQLDisconnect(hdbc1);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   }  
  
   if (henv != SQL_NULL_HENV)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
int main() {  
   RETCODE retcode;  
  
   // SQLBindParameter variables.  
   SQLLEN cbTextSize, lbytes;  
  
   // SQLParamData variable.  
   PTR pParmID;  
  
   // SQLPutData variables.  
   UCHAR  Data[] =   
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyz";  
  
   SDWORD cbBatch = (SDWORD)sizeof(Data) - 1;  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(Env) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Notify ODBC that this is an ODBC 3.0 app.  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetEnvAttr(ODBC version) Failed\n\n");  
      Cleanup();  
      return(9);      
   }  
  
   // Allocate ODBC connection handle and connect.  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Sample uses Integrated Security, create SQL Server DSN using Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"Test", SQL_NTS, (UCHAR*)"",SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Allocate statement handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLAllocHandle(hstmt1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Set parameters based on total data to send.  
   lbytes = (SDWORD)TEXTSIZE;  
   cbTextSize = SQL_LEN_DATA_AT_EXEC(lbytes);  
  
   // Bind the parameter marker.  
   retcode = SQLBindParameter (hstmt1,           // hstmt  
                               1,                // ipar  
                               SQL_PARAM_INPUT,  // fParamType  
                               SQL_C_CHAR,       // fCType  
                               SQL_LONGVARCHAR,  // FSqlType  
                               lbytes,           // cbColDef  
                               0,                // ibScale  
                               (VOID *)1,        // rgbValue  
                               0,                // cbValueMax  
                               &cbTextSize);     // pcbValue  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLBindParameter Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Execute the command.  
   retcode =   
      SQLExecDirect(hstmt1, (UCHAR*)"INSERT INTO emp4 VALUES('Paul Borm', 46,'1950-11-12 00:00:00', ?)", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_NEED_DATA) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLExecDirect Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Check to see if NEED_DATA; if yes, use SQLPutData.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if (retcode == SQL_NEED_DATA) {  
      while (lbytes > cbBatch) {  
         SQLPutData(hstmt1, Data, cbBatch);  
         lbytes -= cbBatch;  
      }  
      // Put final batch.  
      retcode = SQLPutData(hstmt1, Data, lbytes);   
   }  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Make final SQLParamData call.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("Final SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Clean up.  
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a un parámetro|[Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Cancelar el procesamiento de instrucciones|[Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Ejecutar una instrucción SQL|[Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Ejecutar una instrucción SQL preparada|[Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Devolver el siguiente parámetro para el que se van a enviar datos|[Función SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
