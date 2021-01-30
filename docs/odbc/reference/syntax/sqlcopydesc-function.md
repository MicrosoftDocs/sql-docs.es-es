---
description: Función SQLCopyDesc
title: Función SQLCopyDesc | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCopyDesc
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLCopyDesc
helpviewer_keywords:
- SQLCopyDesc function [ODBC]
ms.assetid: d5450895-3824-44c4-8aa4-d4f9752a9602
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ab2ef18e777d433daf4ab93b73d249e62507ea3c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206308"
---
# <a name="sqlcopydesc-function"></a>Función SQLCopyDesc
**Conformidad**  
 Versión introducida: compatibilidad con estándares de ODBC 3,0: ISO 92  
  
 **Resumen**  
 **SQLCopyDesc** copia la información del descriptor de un identificador de descriptor a otro.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
  
SQLRETURN SQLCopyDesc(  
     SQLHDESC     SourceDescHandle,  
     SQLHDESC     TargetDescHandle);  
```  
  
## <a name="arguments"></a>Argumentos  
 *SourceDescHandle*  
 Entradas Identificador del descriptor de origen.  
  
 *TargetDescHandle*  
 Entradas Identificador del descriptor de destino. El argumento *TargetDescHandle* puede ser un identificador de un descriptor de aplicación o un IPD. *TargetDescHandle* no se puede establecer en un identificador de IRD o **SQLCopyDesc** devolverá SQLSTATE HY016 (no se puede modificar un descriptor de fila de implementación).  
  
## <a name="returns"></a>Devoluciones  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR o SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnóstico  
 Cuando **SQLCopyDesc** devuelve SQL_ERROR o SQL_SUCCESS_WITH_INFO, se puede obtener un valor SQLSTATE asociado llamando a **SQLGetDiagRec** con un *HandleType* de SQL_HANDLE_DESC y un *identificador* de *TargetDescHandle*. Si se pasó un *SourceDescHandle* no válido en la llamada, se devolverá SQL_INVALID_HANDLE pero no se devolverá ningún SQLSTATE. En la tabla siguiente se enumeran los valores de SQLSTATE que devuelve normalmente **SQLCopyDesc** y se explica cada uno de ellos en el contexto de esta función. la notación "(DM)" precede a las descripciones de SQLSTATEs devueltas por el administrador de controladores. El código de retorno asociado a cada valor SQLSTATE es SQL_ERROR, a menos que se indique lo contrario.  
  
 Cuando se devuelve un error, la llamada a **SQLCopyDesc** se anula inmediatamente y el contenido de los campos del descriptor *TargetDescHandle* es indefinido.  
  
 Dado **que SQLCopyDesc** se puede implementar llamando a **SQLGetDescField** y **SQLSetDescField**, **SQLCopyDesc** puede devolver SQLSTATEs devuelto por **SQLGetDescField** o **SQLSetDescField**.  
  
|SQLSTATE|Error|Descripción|  
|--------------|-----------|-----------------|  
|01000|ADVERTENCIA general|Mensaje informativo específico del controlador. (La función devuelve SQL_SUCCESS_WITH_INFO).|  
|08S01|Error de vínculo de comunicación|Se produjo un error en el vínculo de comunicación entre el controlador y el origen de datos al que se conectó el controlador antes de que la función finalizara el procesamiento.|  
|HY000|Error general|Se produjo un error para el que no había ningún SQLSTATE específico y para el que no se definió ningún SQLSTATE específico de la implementación. El mensaje de error devuelto por **SQLGetDiagRec** en el búfer *\* MessageText* describe el error y su causa.|  
|HY001|Error de asignación de memoria|El controlador no pudo asignar la memoria necesaria para admitir la ejecución o la finalización de la función.|  
|HY007|La instrucción asociada no está preparada|*SourceDescHandle* se asoció con un IRD y el identificador de instrucción asociado no estaba en el estado preparado o ejecutado.|  
|HY010|Error de secuencia de función|(DM) el identificador de descriptor de *SourceDescHandle* o *TargetDescHandle* estaba asociado a un *StatementHandle* para el que se llamó a una función que se ejecuta de forma asincrónica (no a este) y que todavía se estaba ejecutando cuando se llamó a esta función.<br /><br /> (DM) el identificador de descriptor de *SourceDescHandle* o *TargetDescHandle* se asoció con un *StatementHandle* para el que se llamó a **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** o **SQLSetPos** , y se devolvió SQL_NEED_DATA. Se llamó a esta función antes de enviar los datos para todos los parámetros o columnas de datos en ejecución.<br /><br /> (DM) se llamó a una función que se ejecuta de forma asincrónica para el identificador de conexión que está asociado a *SourceDescHandle* o *TargetDescHandle*. Esta función asincrónica todavía se estaba ejecutando cuando se llamó a la función **SQLCopyDesc** .<br /><br /> Se llamó a **SQLExecute**, **SQLExecDirect** o **SQLMoreResults** para uno de los identificadores de instrucciones asociados a *SourceDescHandle* o *TargetDescHandle* y se devolvieron SQL_PARAM_DATA_AVAILABLE. Se llamó a esta función antes de recuperar los datos de todos los parámetros transmitidos por secuencias.|  
|HY013|Error de administración de memoria|No se pudo procesar la llamada de función porque no se pudo tener acceso a los objetos de memoria subyacentes, posiblemente debido a condiciones de memoria insuficientes.|  
|HY016|No se puede modificar un descriptor de fila de implementación|*TargetDescHandle* se asoció con IRD.|  
|HY021|Información de descriptor incoherente|La información del descriptor comprobada durante una comprobación de coherencia no era coherente. Para obtener más información, vea "comprobaciones de coherencia" en **SQLSetDescField**.|  
|HY092|Identificador de opción/atributo no válido|La llamada a **SQLCopyDesc** solicitó una llamada a **SQLSetDescField**, pero *\* ValuePtr* no era válido para el argumento *FieldIdentifier* en *TargetDescHandle*.|  
|HY117|La conexión se suspendió debido a un estado de transacción desconocido. Solo se permiten las funciones de desconexión y de solo lectura.|(DM) para obtener más información sobre el estado suspendido, consulte [función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Tiempo de espera de conexión agotado|Expiró el tiempo de espera de conexión antes de que el origen de datos respondiera a la solicitud. El período de tiempo de espera de la conexión se establece mediante **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|El controlador no admite esta función|(DM) el controlador asociado a *SourceDescHandle* o *TargetDescHandle* no admite la función.|  
  
## <a name="comments"></a>Comentarios  
 Una llamada a **SQLCopyDesc** copia los campos del identificador del descriptor de origen en el identificador del descriptor de destino. Los campos solo se pueden copiar en un descriptor de aplicación o en IPD, pero no en IRD. Los campos se pueden copiar desde una aplicación o un descriptor de implementación.  
  
 Los campos se pueden copiar de un IRD solo si el identificador de instrucción está en el estado preparado o ejecutado; de lo contrario, la función devuelve SQLSTATE HY007 (la instrucción asociada no está preparada).  
  
 Los campos se pueden copiar desde un IPD independientemente de si se ha preparado o no una instrucción. Si se ha preparado una instrucción SQL con parámetros dinámicos y se admite el rellenado automático de IPD y está habilitado, el controlador rellenará el IPD. Cuando se llama a **SQLCopyDesc** con IPD como *SourceDescHandle*, se copian los campos rellenos. Si el controlador no rellena el IPD, se copia el contenido de los campos originalmente en IPD.  
  
 Todos los campos del descriptor, excepto SQL_DESC_ALLOC_TYPE (que especifica si el identificador del descriptor se asignó de forma automática o explícita), se copian, independientemente de si el campo está definido para el descriptor de destino. Los campos copiados sobrescriben los campos existentes.  
  
 El controlador copia todos los campos de descriptor si los argumentos *SourceDescHandle* y *TargetDescHandle* están asociados al mismo controlador, aunque los controladores estén en dos conexiones o entornos diferentes. Si los argumentos *SourceDescHandle* y *TargetDescHandle* están asociados a controladores diferentes, el administrador de controladores copia los campos definidos por ODBC, pero no copia los campos definidos por el controlador o los campos que no están definidos por ODBC para el tipo de descriptor.  
  
 La llamada a **SQLCopyDesc** se anula inmediatamente si se produce un error.  
  
 Cuando se copia el campo de SQL_DESC_DATA_PTR, se realiza una comprobación de coherencia en el descriptor de destino. Si se produce un error en la comprobación de coherencia, se devuelve SQLSTATE HY021 (información del descriptor incoherente) y la llamada a **SQLCopyDesc** se anula inmediatamente. Para obtener más información sobre las comprobaciones de coherencia, consulte "comprobaciones de coherencia" en la [función SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 Los identificadores de descriptor se pueden copiar entre conexiones incluso si las conexiones están en entornos diferentes. Si el administrador de controladores detecta que los identificadores de descriptor de origen y de destino no pertenecen a la misma conexión y que las dos conexiones pertenecen a controladores independientes, implementa **SQLCopyDesc** realizando una copia campo por campo mediante **SQLGetDescField** y **SQLSetDescField**.  
  
 Cuando se llama a **SQLCopyDesc** con *SourceDescHandle* en un controlador y un *TargetDescHandle* en otro controlador, se borra la cola de errores de *SourceDescHandle* . Esto se debe a que, en este caso, **SQLCopyDesc** se implementa mediante llamadas a **SQLGetDescField** y **SQLSetDescField**.  
  
> [!NOTE]  
>  Es posible que una aplicación pueda asociar un identificador de descriptor asignado explícitamente a un *StatementHandle*, en lugar de llamar a **SQLCopyDesc** para copiar los campos de un descriptor a otro. Un descriptor asignado explícitamente se puede asociar a otro *StatementHandle* en el mismo *ConnectionHandle* estableciendo el atributo de instrucción SQL_ATTR_APP_ROW_DESC o SQL_ATTR_APP_PARAM_DESC en el identificador del descriptor asignado explícitamente. Cuando se hace esto, no es necesario llamar a **SQLCopyDesc** para copiar los valores de campo de descriptor de un descriptor a otro. Sin embargo, un identificador de descriptor no se puede asociar a un *StatementHandle* en otro *ConnectionHandle*. para usar los mismos valores de campo de descriptor en *StatementHandles* en diferentes *ConnectionHandles*, se debe llamar a **SQLCopyDesc** .  
  
 Para obtener una descripción de los campos de un encabezado o un registro de descriptores, consulte [función SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Para obtener más información acerca de los descriptores, consulte [descriptores](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="copying-rows-between-tables"></a>Copiar filas entre tablas  
 Una aplicación puede copiar datos de una tabla a otra sin copiar los datos en el nivel de la aplicación. Para ello, la aplicación enlaza los mismos búferes de datos e información de descriptor a una instrucción que captura los datos y la instrucción que inserta los datos en una copia. Esto puede realizarse mediante el uso compartido de un descriptor de aplicación (enlazar un descriptor asignado explícitamente como ARD a una instrucción y el APD en otro) o mediante **SQLCopyDesc** para copiar los enlaces entre ARD y APD de las dos instrucciones. Si las instrucciones están en conexiones diferentes, se debe usar **SQLCopyDesc** . Además, se debe llamar a **SQLCopyDesc** para copiar los enlaces entre el IRD y el IPD de las dos instrucciones. Al copiar instrucciones en la misma conexión, el SQL_ACTIVE_STATEMENTS tipo de información devuelto por el controlador para una llamada a **SQLGetInfo** debe ser mayor que 1 para que esta operación se realice correctamente. (Este no es el caso cuando se copia entre conexiones).  
  
### <a name="code-example"></a>Ejemplo de código  
 En el ejemplo siguiente, se utilizan las operaciones de descriptor para copiar los campos de la tabla PartsSource en la tabla PartsCopy. El contenido de la tabla PartsSource se captura en los búferes del conjunto de filas de *hstmt0*. Estos valores se usan como parámetros de una instrucción INSERT en *hstmt1* para rellenar las columnas de la tabla PartsCopy. Para ello, los campos del IRD de *hstmt0* se copian en los campos de la IPD de *hstmt1* y los campos del ARD de *hstmt0* se copian en los campos del APD de *hstmt1*. Use **SQLSetDescField** para establecer el atributo SQL_DESC_PARAMETER_TYPE de IPD en SQL_PARAM_INPUT Cuando copie campos IRD desde una instrucción con parámetros de salida a campos de IPD que deben ser parámetros de entrada.  
  
```cpp  
#define ROWS 100  
#define DESC_LEN 50  
#define SQL_SUCCEEDED(rc) (rc == SQL_SUCCESS || rc == SQL_SUCCESS_WITH_INFO)  
  
// Template for a row  
typedef struct {  
   SQLINTEGER   sPartID;  
   SQLINTEGER   cbPartID;  
   SQLUCHAR     szDescription[DESC_LENGTH];  
   SQLINTEGER   cbDescription;  
   REAL         sPrice;  
   SQLINTEGER   cbPrice;  
} PartsSource;  
  
PartsSource    rget[ROWS];          // rowset buffer  
SQLUSMALLINT   sts_ptr[ROWS];       // status pointer  
SQLHSTMT       hstmt0, hstmt1;  
SQLHDESC       hArd0, hIrd0, hApd1, hIpd1;  
  
// ARD and IRD of hstmt0  
SQLGetStmtAttr(hstmt0, SQL_ATTR_APP_ROW_DESC, &hArd0, 0, NULL);  
SQLGetStmtAttr(hstmt0, SQL_ATTR_IMP_ROW_DESC, &hIrd0, 0, NULL);  
  
// APD and IPD of hstmt1  
SQLGetStmtAttr(hstmt1, SQL_ATTR_APP_PARAM_DESC, &hApd1, 0, NULL);  
SQLGetStmtAttr(hstmt1, SQL_ATTR_IMP_PARAM_DESC, &hIpd1, 0, NULL);  
  
// Use row-wise binding on hstmt0 to fetch rows  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_BIND_TYPE, (SQLPOINTER) sizeof(PartsSource), 0);  
  
// Set rowset size for hstmt0  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
  
// Execute a select statement  
SQLExecDirect(hstmt0, "SELECT PARTID, DESCRIPTION, PRICE FROM PARTS ORDER BY 3, 1, 2"",  
               SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt0, 1, SQL_C_SLONG, rget[0].sPartID, 0,   
   &rget[0].cbPartID);  
SQLBindCol(hstmt0, 2, SQL_C_CHAR, &rget[0].szDescription, DESC_LEN,   
   &rget[0].cbDescription);  
SQLBindCol(hstmt0, 3, SQL_C_FLOAT, rget[0].sPrice,   
   0, &rget[0].cbPrice);  
  
// Perform parameter bindings on hstmt1.   
SQLCopyDesc(hArd0, hApd1);  
SQLCopyDesc(hIrd0, hIpd1);  
  
// Set the array status pointer of IRD  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_STATUS_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the ARRAY_STATUS_PTR field of APD to be the same  
// as that in IRD.  
SQLSetStmtAttr(hstmt1, SQL_ATTR_PARAM_OPERATION_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the hIpd1 records as input parameters  
rc = SQLSetDescField(hIpd1, 1, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 2, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 3, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
  
// Prepare an insert statement on hstmt1. PartsCopy is a copy of  
// PartsSource  
SQLPrepare(hstmt1, "INSERT INTO PARTS_COPY VALUES (?, ?, ?)", SQL_NTS);  
  
// In a loop, fetch a rowset, and copy the fetched rowset to PARTS_COPY  
  
rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
while (SQL_SUCCEEDED(rc)) {  
  
   // After the call to SQLFetchScroll, the status array has row   
   // statuses. This array is used as input status in the APD  
   // and hence determines which elements of the rowset buffer  
   // are inserted.  
   SQLExecute(hstmt1);  
  
   rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
} // while  
```  
  
### <a name="related-functions"></a>Funciones relacionadas  
  
|Para información acerca de|Vea|  
|---------------------------|---------|  
|Obtener varios campos de descriptor|[Función SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Establecer un solo campo de descriptor|[Función SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Establecer varios campos de descriptor|[Función SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de la API de ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Archivos de encabezado de ODBC](../../../odbc/reference/install/odbc-header-files.md)
