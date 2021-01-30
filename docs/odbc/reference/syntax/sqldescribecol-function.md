---
description: SQLDescribeCol (función)
title: Función SQLDescribeCol | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDescribeCol
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDescribeCol
helpviewer_keywords:
- SQLDescribeCol function [ODBC]
ms.assetid: eddef353-83f3-4a3c-8f24-f9ed888890a4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ceffc2b0db32ccce75c566c89ac3d69b535abe29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192076"
---
# <a name="sqldescribecol-function"></a>SQLDescribeCol (función)
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 1,0: ISO 92  
  
 **Resumen**  
 **SQLDescribeCol** devuelve el descriptor de resultados, el nombre de columna, el tipo, el tamaño de la columna, los dígitos decimales y la nulabilidad de una columna del conjunto de resultados. Esta información también está disponible en los campos de IRD.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLDescribeCol(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   ColumnNumber,  
      SQLCHAR *      ColumnName,  
      SQLSMALLINT    BufferLength,  
      SQLSMALLINT *  NameLengthPtr,  
      SQLSMALLINT *  DataTypePtr,  
      SQLULEN *      ColumnSizePtr,  
      SQLSMALLINT *  DecimalDigitsPtr,  
      SQLSMALLINT *  NullablePtr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *StatementHandle*  
 Entradas Identificador de instrucción.  
  
 *ColumnNumber*  
 Entradas Número de columnas de datos de resultados, ordenados secuencialmente en el orden de columnas creciente, empezando por 1. El argumento *ColumnNumber* también se puede establecer en 0 para describir la columna de marcador.  
  
 *ColumnName*  
 Genere Puntero a un búfer terminado en null en el que se va a devolver el nombre de columna. Este valor se lee desde el campo SQL_DESC_NAME de IRD. Si la columna no tiene nombre o no se puede determinar el nombre de la columna, el controlador devuelve una cadena vacía.  
  
 Si *columnName* es null, *NameLengthPtr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *columnName*.  
  
 *BufferLength*  
 Entradas Longitud del búfer **columnName* , en caracteres.  
  
 *NameLengthPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de caracteres (excepto la terminación nula) disponible para devolver en \* *columnName*. Si el número de caracteres disponibles para devolver es mayor o igual que *BufferLength*, el nombre de la columna en \* *columnName* se trunca a *BufferLength* menos la longitud de un carácter de terminación null.  
  
 *DataTypePtr*  
 Genere Puntero a un búfer en el que se va a devolver el tipo de datos SQL de la columna. Este valor se lee desde el campo SQL_DESC_CONCISE_TYPE de IRD. Este será uno de los valores de los [tipos de datos de SQL](../../../odbc/reference/appendixes/sql-data-types.md)o un tipo de datos de SQL específico del controlador. Si no se puede determinar el tipo de datos, el controlador devuelve SQL_UNKNOWN_TYPE.  
  
 En ODBC 3. *x*, SQL_TYPE_DATE, SQL_TYPE_TIME o SQL_TYPE_TIMESTAMP se devuelve en *\* DataTypePtr* para los datos de fecha, hora o marca de tiempo, respectivamente, en ODBC 2.** se devuelve x, SQL_DATE, SQL_TIME o SQL_TIMESTAMP. El administrador de controladores realiza las asignaciones necesarias cuando se trata de un ODBC 2. la aplicación *x* está trabajando con un ODBC 3. controlador *x* o cuando se trata de un ODBC 3. la aplicación *x* está trabajando con ODBC 2. controlador *x* .  
  
 Cuando *ColumnNumber* es igual a 0 (para una columna de marcador), se devuelve SQL_BINARY en *\* DataTypePtr* para los marcadores de longitud variable. (Se devuelve SQL_INTEGER si los marcadores se usan en ODBC 3. aplicación *x* que trabaja con ODBC 2. controlador *x* o ODBC 2. aplicación *x* que trabaja con ODBC 3. controlador *x* ).  
  
 Para obtener más información sobre estos tipos de datos, vea tipos de datos [SQL](../../../odbc/reference/appendixes/sql-data-types.md) en el Apéndice D: tipos de datos. Para obtener información acerca de los tipos de datos de SQL específicos del controlador, consulte la documentación del controlador.  
  
 *ColumnSizePtr*  
 Genere Puntero a un búfer en el que se va a devolver el tamaño (en caracteres) de la columna en el origen de datos. Si no se puede determinar el tamaño de la columna, el controlador devuelve 0. Para obtener más información sobre el tamaño de las columnas, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.  
  
 *DecimalDigitsPtr*  
 Genere Puntero a un búfer en el que se va a devolver el número de dígitos decimales de la columna en el origen de datos. Si no se puede determinar el número de dígitos decimales o no es aplicable, el controlador devuelve 0. Para obtener más información sobre los dígitos decimales, vea [tamaño de columna, dígitos decimales, longitud de octetos de transferencia y tamaño de presentación en el](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) Apéndice D: tipos de datos.  
  
 *NullablePtr*  
 Genere Puntero a un búfer en el que se va a devolver un valor que indica si la columna permite valores NULL. Este valor se lee desde el campo SQL_DESC_NULLABLE de IRD. El valor puede ser:  
  
 SQL_NO_NULLS: la columna no admite valores NULL.  
  
 SQL_NULLABLE: la columna permite valores NULL.  
  
 SQL_NULLABLE_UNKNOWN: el controlador no puede determinar si la columna permite valores NULL.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLDescribeCol** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_STMT y un *identificador* de *StatementHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLDescribeCol** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|La \* *columnName* del búfer no era lo suficientemente grande como para devolver el nombre completo de la columna, por lo que se truncó el nombre de la columna. La longitud del nombre de columna sin truncar se devuelve en **NameLengthPtr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|07005|Instrucción preparada no es una *especificación de cursor*|La instrucción asociada a *StatementHandle* no devolvió un conjunto de resultados. No había columnas para describir.|  
|07009|Índice de descriptor no válido|(DM) el valor especificado para el argumento *ColumnNumber* era igual a 0 y la opción de instrucción SQL_ATTR_USE_BOOKMARKS era SQL_UB_OFF.<br /><br /> El valor especificado para el argumento *ColumnNumber* era mayor que el número de columnas del conjunto de resultados.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY008|Operación cancelada|El procesamiento asincrónico se ha habilitado para *StatementHandle*. Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en *StatementHandle*. A continuación, se llamó de nuevo a la función en *StatementHandle*.<br /><br /> Se llamó a la función y antes de completar la ejecución, se llamó a **SQLCancel** o **SQLCancelHandle** en el *StatementHandle* desde un subproceso diferente en una aplicación multiproceso.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *StatementHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a **SQLDescribeCol** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para *StatementHandle* y se devolvió SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica (no a esta) para *StatementHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> (DM) se llamó a la función antes de llamar a **SQLPrepare**, **SQLExecute** o a una función de catálogo en el identificador de instrucción.<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** para *StatementHandle* y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el valor especificado para el argumento *BufferLength* era menor que 0.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *StatementHandle* no admite la función.|  
|IM017|El sondeo está deshabilitado en el modo de notificación asincrónico|Cada vez que se usa el modelo de notificación, el sondeo se deshabilita.|  
|IM018|No se ha llamado a **SQLCompleteAsync** para completar la operación asincrónica anterior en este controlador.|Si la llamada de función anterior en el identificador devuelve SQL_STILL_EXECUTING y si el modo de notificación está habilitado, se debe llamar a **SQLCompleteAsync** en el identificador para realizar el procesamiento posterior y completar la operación.|  
  
 **SQLDescribeCol** puede devolver cualquier SQLSTATE que pueda ser devuelto por **SQLPrepare** o **SQLExecute** cuando se llama después de **SQLPrepare** y antes de **SQLExecute**, dependiendo de si el origen de datos evalúa la instrucción SQL asociada al identificador de instrucción.  
  
 Por motivos de rendimiento, una aplicación no debe llamar a **SQLDescribeCol** antes de ejecutar una instrucción.  
  
## <a name="comments"></a>Comentarios  
 Una aplicación normalmente llama a **SQLDescribeCol** después de una llamada a **SQLPrepare** y antes o después de la llamada asociada a **SQLExecute**. Una aplicación también puede llamar a **SQLDescribeCol** después de una llamada a **SQLExecDirect**. Para obtener más información, consulte [metadatos del conjunto de resultados](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLDescribeCol** recupera el nombre de columna, el tipo y la longitud generados por una instrucción **Select** . Si la columna es una expresión, **columnName* es una cadena vacía o un nombre definido por el controlador.  
  
> [!NOTE]  
>  ODBC admite SQL_NULLABLE_UNKNOWN como una extensión, aunque la especificación de interfaz de nivel de llamada de grupo abierto y grupo de acceso de SQL no especifica la opción de **SQLDescribeCol**.  
  
## <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Enlazar un búfer a una columna de un conjunto de resultados|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Cancelar el procesamiento de instrucciones|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Devolver información acerca de una columna de un conjunto de resultados|[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|Obtener varias filas de datos|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Devolver el número de columnas del conjunto de resultados|[SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|Preparar una instrucción para su ejecución|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
