---
description: Función SQLNativeSql
title: Función SQLNativeSql | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLNativeSql
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLNativeSql
helpviewer_keywords:
- SQLNativeSql function [ODBC]
ms.assetid: b8efc247-27ab-4a00-92b6-1400785783fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7cc73e184b6b092e1de4eef40de8eaadd11d738b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160786"
---
# <a name="sqlnativesql-function"></a>Función SQLNativeSql
**Conformidad**  
 Versión introducida: ODBC 1,0 Standards Compliance: ODBC  
  
 **Resumen**  
 **SQLNativeSql** devuelve la cadena SQL modificada por el controlador. **SQLNativeSql** no ejecuta la instrucción SQL.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLNativeSql(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      InStatementText,  
     SQLINTEGER     TextLength1,  
     SQLCHAR *      OutStatementText,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   TextLength2Ptr);  
```  
  
## <a name="arguments"></a>Argumentos  
 *ConnectionHandle*  
 [Entrada] Identificador de conexión.  
  
 *InStatementText*  
 Entradas Cadena de texto SQL que se va a traducir.  
  
 *TextLength1*  
 Entradas Longitud en caracteres de **InStatementText* cadena de texto.  
  
 *OutStatementText*  
 Genere Puntero a un búfer en el que se va a devolver la cadena SQL traducida.  
  
 Si *OutStatementText* es null, *TextLength2Ptr* seguirá devolviendo el número total de caracteres (excepto el carácter de terminación null para los datos de caracteres) disponible para devolver en el búfer señalado por *OutStatementText*.  
  
 *BufferLength*  
 Entradas Número de caracteres en el \* búfer de *OutStatementText* . Si el valor devuelto en *\* InStatementText* es una cadena Unicode (al llamar a **SQLNativeSqlW**), el argumento *BufferLength* debe ser un número par.  
  
 *TextLength2Ptr*  
 Genere Puntero a un búfer en el que se va a devolver el número total de caracteres (excluyendo la terminación nula) disponible para devolver en \* *OutStatementText*. Si el número de caracteres disponibles para devolver es mayor o igual que *BufferLength*, la cadena SQL traducida en \* *OutStatementText* se trunca a *BufferLength* menos la longitud de un carácter de terminación null.  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLNativeSql** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DBC y un *identificador* de *ConnectionHandle*. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLNativeSql** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|01004|Datos de cadena, truncados a la derecha|El búfer \* *OutStatementText* no era lo suficientemente grande como para devolver la cadena SQL completa, por lo que se TRUNCÓ la cadena SQL. La longitud de la cadena de SQL no truncada se devuelve en **TextLength2Ptr*. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08003|Conexión no abierta|El estado de *ConnectionHandle* no estaba conectado.|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|22007|Formato de fecha y hora no válido|**InStatementText* contenía una cláusula escape con un valor de fecha, hora o marca de tiempo no válido.|  
|24000|Estado de cursor no válido|El cursor al que se hace referencia en la instrucción se colocó antes del inicio del conjunto de resultados o después del final del conjunto de resultados. Es posible que un controlador que tenga una implementación de cursor DBMS nativa no devuelva este error.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY009|Uso no válido de puntero nulo|(DM) **InStatementText* era un puntero nulo.|  
|HY010|Error de secuencia de función|(DM) se llamó a una función que se ejecuta de forma asincrónica para *ConnectionHandle* y que todavía se estaba ejecutando cuando se llamó a esta función.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY090|Longitud de búfer o cadena no válida|(DM) el argumento *TextLength1* era menor que 0, pero no es igual a SQL_NTS.|  
|||(DM) el argumento *BufferLength* era menor que 0 y el argumento *OutStatementText* no era un puntero nulo.|  
|HY109|Posición del cursor no válida|La fila actual del cursor se ha eliminado o no se ha capturado. Es posible que un controlador que tenga una implementación de cursor DBMS nativa no devuelva este error.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *ConnectionHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 A continuación se muestran ejemplos de lo que **SQLNativeSql** podría devolver para la siguiente cadena de entrada de SQL que contiene la función escalar Convert. Supongamos que la columna empid es de tipo entero en el origen de datos:  
  
```sql  
SELECT { fn CONVERT (empid, SQL_SMALLINT) } FROM employee  
```  
  
 Un controlador para Microsoft SQL Server podría devolver la siguiente cadena SQL traducida:  
  
```sql  
SELECT convert (smallint, empid) FROM employee  
```  
  
 Un controlador para el servidor de ORACLE podría devolver la siguiente cadena SQL traducida:  
  
```sql  
SELECT to_number (empid) FROM employee  
```  
  
 Un controlador para Ingres puede devolver la siguiente cadena SQL traducida:  
  
```sql  
SELECT int2 (empid) FROM employee  
```  
  
 Para obtener más información, vea [ejecución directa](../../../odbc/reference/develop-app/direct-execution-odbc.md) y [ejecución preparada](../../../odbc/reference/develop-app/prepared-execution-odbc.md).  
  
## <a name="related-functions"></a>Funciones relacionadas  
 Ninguno.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
