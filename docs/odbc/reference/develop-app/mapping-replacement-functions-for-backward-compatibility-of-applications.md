---
description: Asignación de funciones de reemplazo para mantener la compatibilidad de aplicaciones
title: Asignación de funciones de reemplazo para la compatibilidad de aplicaciones-ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- mapping replacement functions [ODBC]
- upgrading applications [ODBC], mapping replacement functions
- compatibility [ODBC], mapping replacement functions
- ODBC drivers [ODBC], backward compatibility
- functions [ODBC], mapping replacement functions
- application upgrades [ODBC], mapping replacement functions
- backward compatibility [ODBC], mapping replacement functions
ms.assetid: f5e6d9da-76ef-42cb-b3f5-f640857df732
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e4c31d523a9fb50dd45bf8ab56c551d3830b0861
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082166"
---
# <a name="mapping-replacement-functions-for-backward-compatibility-of-applications"></a>Asignación de funciones de reemplazo para mantener la compatibilidad de aplicaciones
Una aplicación ODBC *3. x* que trabaje con el administrador de controladores ODBC *3. x* funcionará con un controlador ODBC *2. x* siempre y cuando no se utilicen nuevas características. Sin embargo, tanto la funcionalidad duplicada como los cambios de comportamiento afectan a la manera en que funciona la aplicación ODBC *3. x* en un controlador ODBC *2. x* . Cuando se trabaja con un controlador ODBC *2. x* , el administrador de controladores asigna las siguientes funciones de ODBC *3. x* , que han reemplazado una o varias funciones ODBC *2. x* , a las funciones ODBC *2. x* correspondientes.  
  
|ODBC *3. x* (función)|ODBC *2. x* (función)|  
|-------------------------|-------------------------|  
|**SQLAllocHandle**|**SQLAllocEnv**, **SQLAllocConnect** o **SQLAllocStmt**|  
|**SQLBulkOperations**|**SQLSetPos**|  
|**SQLColAttribute**|**SQLColAttributes**|  
|**SQLEndTran**|**SQLTransact**|  
|**SQLFetch**|**SQLExtendedFetch**|  
|**SQLFetchScroll**|**SQLExtendedFetch**|  
|**SQLFreeHandle**|**SQLFreeEnv**, **SQLFreeConnect** o **SQLFreeStmt**|  
|**SQLGetConnectAttr**|**SQLGetConnectOption**|  
|**SQLGetDiagRec**|**SQLError**|  
|**SQLGetStmtAttr**|**SQLGetStmtOption**[1]|  
|**SQLSetConnectAttr**|**SQLSetConnectOption**|  
|**SQLSetStmtAttr**|**SQLSetStmtOption**[1]|  
  
 [1] también se pueden realizar otras acciones, dependiendo del atributo que se solicita.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
 El administrador de controladores asigna este a **SQLAllocEnv**, **SQLAllocConnect** o **SQLAllocStmt**, según corresponda. La siguiente llamada a **SQLAllocHandle**:  
  
```  
SQLAllocHandle(HandleType, InputHandle, OutputHandlePtr);  
```  
  
 dará lugar a que el administrador de controladores realice la asignación siguiente (conceptual, sin comprobación de errores):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLAllocEnv(OutputHandlePtr));  
   case SQL_HANDLE_DBC: return (SQLAllocConnect (InputHandle, OutputHandlePtr));  
   case SQL_HANDLE_STMT: return (SQLAllocStmt (InputHandle, OutputHandlePtr));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
 El administrador de controladores asigna esto a **SQLSetPos**. La siguiente llamada a **SQLBulkOperations**:  
  
```  
SQLBulkOperations(hstmt, Operation);  
```  
  
 dará como resultado la siguiente secuencia de pasos:  
  
1.  Si el argumento de operación es SQL_ADD, el administrador de controladores llama a **SQLSetPos** de la siguiente manera:  
  
    ```  
    SQLSetPos (hstmt, 0, SQL_ADD, SQL_LOCK_NO_CHANGE);  
    ```  
  
2.  Si el argumento de la operación no se SQL_ADD, el controlador devuelve SQLSTATE HY092 (identificador de opción o atributo no válido).  
  
3.  Si la aplicación intenta cambiar el SQL_ATTR_ROW_STATUS_PTR entre las llamadas a **SQLFetch** o **SQLFetchScroll** y **SQLBulkOperations**, el administrador de controladores devolverá SQLSTATE HY011 (el atributo no se puede establecer ahora).  
  
4.  Si el argumento de operación es SQL_ADD, la aplicación debe llamar a **SQLBindCol** para enlazar los datos que se van a insertar. No puede llamar a **SQLSetDescField** o **SQLSetDescRec** para enlazar los datos que se van a insertar.  
  
5.  Si el argumento de operación es SQL_ADD y el número de filas que se van a insertar no es el mismo que el tamaño actual del conjunto de filas, se debe llamar a **SQLSetStmtAttr** para establecer el atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE en el número de filas que se van a insertar antes de llamar a **SQLBulkOperations**. Para volver al tamaño del conjunto de filas anterior, la aplicación debe establecer el atributo de instrucción SQL_ATTR_ROW_ARRAY_SIZE antes de que se llame a **SQLFetch**, **SQLFetchScroll** o **SQLSetPos** .  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
 El administrador de controladores lo asigna a **SQLColAttributes**. La siguiente llamada a **SQLColAttribute**:  
  
```  
SQLColAttribute(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
```  
  
 dará como resultado la siguiente secuencia de pasos:  
  
1.  Si *FieldIdentifier* es uno de los siguientes:  
  
     SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_LENGTH, SQL_DESC_OCTET_LENGTH, SQL_DESC_UNNAMED, SQL_DESC_BASE_COLUMN_NAME, SQL_DESC_LITERAL_PREFIX, SQL_DESC_LITERAL_SUFFIX o SQL_DESC_LOCAL_TYPE_NAME  
  
     el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY091 (identificador de campo de descriptor no válido). No se aplica ninguna otra regla de esta sección.  
  
2.  El administrador de controladores asigna SQL_COLUMN_COUNT, SQL_COLUMN_NAME o SQL_COLUMN_NULLABLE a SQL_DESC_COUNT, SQL_DESC_NAME o SQL_DESC_NULLABLE, respectivamente. (Un controlador ODBC *2. x* solo necesita admitir SQL_COLUMN_COUNT, SQL_COLUMN_NAME y SQL_COLUMN_NULLABLE, no SQL_DESC_COUNT, SQL_DESC_NAME y SQL_DESC_NULLABLE). La llamada a SQLColAttribute se asigna a:  
  
    ```  
    SQLColAttributes(StatementHandle, ColumnNumber, FieldIdentifier, CharacterAttributePtr, BufferLength, StringLengthPtr, NumericAttributePtr);  
    ```  
  
3.  Todos los demás valores de *FieldIdentifier* se pasan al controlador, con **SQLColAttribute** asignado a **SQLColAttributes** , como se mostró anteriormente.  
  
4.  Si *BufferLength* es menor que 0, el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY090 (longitud de búfer o cadena no válida). No se aplica ninguna otra regla de esta sección.  
  
5.  Si *FieldIdentifier* es SQL_DESC_CONCISE_TYPE y el tipo devuelto es un tipo de datos de fecha y hora conciso, el administrador de controladores asigna los valores devueltos para los códigos de fecha, hora y marca de tiempo.  
  
6.  El administrador de controladores realiza las comprobaciones necesarias para ver si es necesario generar SQLSTATE HY010 (error de secuencia de función). Si es así, el administrador de controladores devuelve SQL_ERROR y SQLSTATE HY010 (error de secuencia de función). No se aplica ninguna otra regla de esta sección.  
  
## <a name="sqlendtran"></a>SQLEndTran  
 El administrador de controladores lo asigna a **SQLTransact**. La siguiente llamada a **SQLEndTran**:  
  
```  
SQLEndTran(HandleType, Handle, CompletionType);  
```  
  
 dará lugar a que el administrador de controladores realice la asignación siguiente (conceptual, sin comprobación de errores):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV:return(SQLTransact(Handle, SQL_NULL_HDBC, CompletionType));  
   case SQL_HANDLE_DBC:return(SQLTransact(SQL_NULL_HENV, Handle, CompletionType);  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlfetch"></a>SQLFetch  
 El administrador de controladores asigna este a **SQLExtendedFetch** con un argumento *FetchOrientation* de SQL_FETCH_NEXT. La siguiente llamada a **SQLFetch**:  
  
```  
SQLFetch (StatementHandle);  
```  
  
 provocará que el administrador de controladores llame a **SQLExtendedFetch**, como se indica a continuación:  
  
```  
rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, &RowCount, RowStatusArray);  
```  
  
 En esta llamada, el argumento *pcRow* se establece en el valor en el que la aplicación establece el atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR a través de una llamada a **SQLSetStmtAttr**.  
  
> [!NOTE]  
>  Cuando la aplicación llama a **SQLSetStmtAttr** para establecer SQL_ATTR_ROW_STATUS_PTR para que apunte a una matriz de estado, el administrador de controladores almacena en caché el puntero. *RowStatusArray* puede ser igual a un puntero nulo.  
  
 Si el controlador no admite **SQLExtendedFetch** y se carga la biblioteca de cursores, el administrador de controladores usa el **SQLExtendedFetch** de la biblioteca de cursores para asignar **SQLFetch** a **SQLExtendedFetch**. Si el controlador no admite **SQLExtendedFetch** y la biblioteca de cursores no está cargada, el administrador de controladores pasa la llamada a **SQLFetch** a través del controlador. Si la aplicación llama a **SQLSetStmtAttr** para establecer SQL_ATTR_ROW_STATUS_PTR, el administrador de controladores garantiza que se rellena la matriz. Si la aplicación llama a **SQLSetStmtAttr** para establecer SQL_ATTR_ROWS_FETCHED_PTR, el administrador de controladores establece este campo en 1.  
  
## <a name="sqlfetchscroll"></a>SQLFetchScroll  
 El administrador de controladores lo asigna a **SQLExtendedFetch**. La siguiente llamada a **SQLFetchScroll**:  
  
```  
SQLFetchScroll(StatementHandle, FetchOrientation, FetchOffset);  
```  
  
 dará como resultado la siguiente secuencia de pasos:  
  
1.  Cuando la aplicación llama a **SQLSetStmtAttr** para establecer SQL_ATTR_ROW_STATUS_PTR (que establece el campo SQL_DESC_ARRAY_STATUS_PTR en IRD) para que señale a una matriz de estado, el administrador de controladores almacena en caché este puntero. Permite que este puntero sea *RowStatusArray*; de lo contrario, deje que *RowStatusArray* sea igual a un puntero nulo. Si el argumento *RowStatusArray* se establece en un puntero nulo, el administrador de controladores genera una matriz de estado de fila.  
  
2.  Si *FetchOrientation* no es uno de SQL_FETCH_NEXT, SQL_FETCH_PRIOR, SQL_FETCH_ABSOLUTE, SQL_FETCH_RELATIVE, SQL_FETCH_FIRST, SQL_FETCH_LAST o SQL_FETCH_BOOKMARK, el administrador de controladores devuelve con SQL_ERROR y SQLSTATE HY106 (tipo de captura fuera del intervalo). No se aplica ninguna otra regla de esta sección.  
  
3.  Caso:  
  
-   Si *FetchOrientation* es igual a SQL_FETCH_BOOKMARK, entonces:  
  
    -   Si se llamó a **SQLSetStmtAttr** antes para establecer el valor de SQL_ATTR_FETCH_BOOKMARK_PTR, deje que *BMK* sea el valor obtenido desreferenciando el puntero SQL_DESC_FETCH_BOOKMARK_PTR.  
  
    -   De lo contrario, devuelva SQL_ERROR con SQLSTATE HY111 (valor de marcador no válido). No se aplica ninguna otra regla de esta sección.  
  
     El administrador de controladores llama ahora a **SQLExtendedFetch**, como se indica a continuación:  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, Bmk, pcRow, RowStatusArray);  
    ```  
  
-   De lo contrario, el administrador de controladores llama a **SQLExtendedFetch**, como se indica a continuación:  
  
    ```  
    rc = SQLExtendedFetch(StatementHandle, FetchOrientation, FetchOffset, pcRow, RowStatusArray);  
    ```  
  
     En estas llamadas, el argumento *pcRow* se establece en el valor en el que la aplicación establece el atributo de instrucción SQL_ATTR_ROWS_FETCHED_PTR a través de una llamada a **SQLSetStmtAttr**.  
  
-   SQL_ATTR_ROW_ARRAY_SIZE está asignado a SQL_ROWSET_SIZE.  
  
-   Si *RC* es igual a SQL_SUCCESS o SQL_SUCCESS_WITH_INFO, y si *FetchOrientation* es igual a SQL_FETCH_BOOKMARK y *FetchOffset* no es igual a 0, el administrador de controladores publica una advertencia, SQLSTATE 01S10 (intento de captura por un desplazamiento de marcador, valor de desplazamiento omitido) y devuelve SQL_SUCCESS_WITH_INFO.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
 El administrador de controladores asigna este a **SQLFreeEnv**, **SQLFreeConnect** o **SQLFreeStmt** , según corresponda. La siguiente llamada a **SQLFreeHandle**:  
  
```  
SQLFreeHandle(HandleType, Handle);  
```  
  
 dará lugar a que el administrador de controladores realice la asignación siguiente (conceptual, sin comprobación de errores):  
  
```  
switch (HandleType) {  
   case SQL_HANDLE_ENV: return (SQLFreeEnv(Handle));  
   case SQL_HANDLE_DBC: return (SQLFreeConnect(Handle));  
   case SQL_HANDLE_STMT: return (SQLFreeStmt(Handle, SQL_DROP));  
   default: // return SQL_ERROR, SQLSTATE HY092 ("Invalid attribute/option identifier")  
}  
```  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
 El administrador de controladores lo asigna a **SQLGetConnectOption**. La siguiente llamada a **SQLGetConnectAttr**:  
  
```  
SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 dará como resultado la siguiente secuencia de pasos:  
  
1.  Si el *atributo* no es un atributo de instrucción o una conexión definida por el controlador y no es un atributo definido en ODBC *2. x*, el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY092 (identificador de opción o atributo no válido). No se aplican más reglas en esta sección.  
  
2.  Si el *atributo* es igual a SQL_ATTR_AUTO_IPD o SQL_ATTR_METADATA_ID, el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY092 (identificador de opción o atributo no válido).  
  
3.  El administrador de controladores realiza las comprobaciones necesarias para ver si es necesario generar SQLSTATE 08003 (conexión no abierta) o SQLSTATE HY010 (error de secuencia de función). Si es así, el administrador de controladores devuelve SQL_ERROR y envía el mensaje de error correspondiente. No se aplica ninguna otra regla de esta sección.  
  
4.  El administrador de controladores llama a **SQLGetConnectOption** como se indica a continuación:  
  
    ```  
    SQLGetConnectOption (ConnectionHandle, Attribute, ValuePtr);  
    ```  
  
     Tenga en cuenta que *BufferLength* y *StringLengthPtr* se omiten.  
  
## <a name="sqlgetdata"></a>SQLGetData  
 Cuando una aplicación ODBC *3. x* que trabaja con un controlador ODBC *2. x* llama a **SQLGetData** con el argumento *ColumnNumber* igual a 0, el administrador de controladores ODBC *3. x* lo asigna a una llamada a **SQLGetStmtOption** con el atributo *Option* establecido en SQL_GET_BOOKMARK.  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
 El administrador de controladores lo asigna a **SQLGetStmtOption**. La siguiente llamada a **SQLGetStmtAttr**:  
  
```  
SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, StringLengthPtr);  
```  
  
 dará como resultado la siguiente secuencia de pasos:  
  
1.  Si el *atributo* no es un atributo de instrucción o una conexión definida por el controlador y no es un atributo definido en ODBC *2. x*, el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY092 (identificador de opción o atributo no válido). No se aplican más reglas en esta sección.  
  
2.  Si el *atributo* es uno de los siguientes:  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY092 (identificador de opción o atributo no válido). No se aplica ninguna otra regla de esta sección.  
  
3.  El administrador de controladores realiza las comprobaciones necesarias para ver si es necesario generar SQLSTATE HY010 (error de secuencia de función). Si es así, el administrador de controladores devuelve SQL_ERROR y SQLSTATE HY010 (error de secuencia de función). No se aplica ninguna otra regla de esta sección.  
  
4.  Si el *atributo* es igual a SQL_ATTR_ROWS_FETCHED_PTR, el administrador de controladores devuelve un puntero a la variable interna del administrador de controladores *cRow*, que ha usado o usará en una llamada a **SQLExtendedFetch**. No se aplica ninguna otra regla de esta sección.  
  
5.  Si el *atributo* es igual a SQL_DESC_FETCH_BOOKMARK_PTR, el administrador de controladores devuelve el puntero adecuado que se ha almacenado en la memoria caché durante una llamada a **SQLSetStmtAttr**.  
  
6.  Si el *atributo* es igual a SQL_ATTR_ROW_STATUS_PTR, el administrador de controladores devuelve el puntero adecuado que se ha almacenado en la memoria caché durante una llamada a **SQLSetStmtAttr**.  
  
7.  El administrador de controladores llama a **SQLGetStmtOption** como se indica a continuación:  
  
    ```  
    SQLGetStmtOption (hstmt, fOption, pvParam);  
    ```  
  
     donde *hstmt*, *fOption* y *pvParam* se establecerán en los valores de *StatementHandle*, *Attribute* y *ValuePtr*, respectivamente. Se omiten *BufferLength* y *StringLengthPtr* .  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
 El administrador de controladores lo asigna a **SQLSetConnectOption**. La siguiente llamada a **SQLSetConnectAttr**:  
  
```  
SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, StringLength);  
```  
  
 dará como resultado la siguiente secuencia de pasos:  
  
1.  Si el *atributo* no es un atributo de instrucción o una conexión definida por el controlador y no es un atributo definido en ODBC *2. x*, el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY092 (identificador de opción o atributo no válido). No se aplican más reglas en esta sección.  
  
2.  Si el *atributo* es igual a SQL_ATTR_AUTO_IPD, el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY092 (identificador de opción o atributo no válido).  
  
3.  El administrador de controladores realiza las comprobaciones necesarias para ver si es necesario generar SQLSTATE 08003 (conexión no abierta) o SQLSTATE HY010 (error de secuencia de función). Si es necesario producir uno de estos errores, el administrador de controladores devuelve SQL_ERROR y envía el mensaje de error correspondiente. No se aplica ninguna otra regla de esta sección.  
  
4.  El administrador de controladores llama a **SQLSetConnectOption** como se indica a continuación:  
  
    ```  
    SQLSetConnectOption (hdbc, fOption, vParam);  
    ```  
  
     donde *hdbc*, *fOption* y *vParam* se establecerán en los valores de *ConnectionHandle*, *Attribute* y *ValuePtr*, respectivamente. *StringLengthPtr* se omite.  
  
> [!NOTE]  
>  La capacidad de establecer los atributos de instrucción en el nivel de conexión está en desuso. Los atributos de instrucción nunca deben establecerse en el nivel de conexión mediante una aplicación ODBC *3. x* .  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 El administrador de controladores lo asigna a **SQLSetStmtOption**. La siguiente llamada a **SQLSetStmtAttr**:  
  
```  
SQLSetStmtAttr(StatementHandle, Attribute, ValuePtr, StringLength);  
```  
  
 dará como resultado la siguiente secuencia de pasos:  
  
1.  Si el *atributo* no es un atributo de instrucción o una conexión definida por el controlador y no es un atributo definido en ODBC *2. x*, el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY092 (identificador de opción o atributo no válido). No se aplican más reglas en esta sección.  
  
2.  Si el *atributo* es uno de los siguientes:  
  
     SQL_ATTR_APP_ROW_DESC  
  
     SQL_ATTR_APP_PARAM_DESC  
  
     SQL_ATTR_AUTO_IPD  
  
     SQL_ATTR_ROW_BIND_TYPE  
  
     SQL_ATTR_IMP_ROW_DESC  
  
     SQL_ATTR_IMP_PARAM_DESC  
  
     SQL_ATTR_METADATA_ID  
  
     SQL_ATTR_PARAM_BIND_TYPE  
  
     SQL_ATTR_PREDICATE_PTR  
  
     SQL_ATTR_PREDICATE_OCTET_LENGTH_PTR  
  
     SQL_ATTR_PARAM_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_BIND_OFFSET_PTR  
  
     SQL_ATTR_ROW_OPERATION_PTR  
  
     SQL_ATTR_PARAM_OPERATION_PTR  
  
     el administrador de controladores devuelve SQL_ERROR con SQLSTATE HY092 (identificador de opción o atributo no válido). No se aplica ninguna otra regla de esta sección.  
  
3.  El administrador de controladores realiza las comprobaciones necesarias para ver si es necesario generar SQLSTATE HY010 (error de secuencia de función). Si es así, el administrador de controladores devuelve SQL_ERROR y SQLSTATE HY010 (error de secuencia de función). No se aplica ninguna otra regla de esta sección.  
  
4.  Si el *atributo* es igual a SQL_ATTR_PARAMSET_SIZE o SQL_ATTR_PARAMS_PROCESSED_PTR, vea la sección "asignaciones para controlar matrices de parámetros", más adelante en este tema. No se aplica ninguna otra regla de esta sección.  
  
5.  Si el *atributo* es igual a SQL_ATTR_ROWS_FETCHED_PTR, el administrador de controladores almacena en caché el valor del puntero para su uso posterior con **SQLFetchScroll**.  
  
6.  Si el *atributo* es igual a SQL_ATTR_ROW_STATUS_PTR, el administrador de controladores almacena en caché el valor del puntero para su uso posterior con **SQLFetchScroll** o **SQLSetPos**. No se aplica ninguna otra regla de esta sección.  
  
7.  Si el *atributo* es igual a SQL_ATTR_FETCH_BOOKMARK_PTR, el administrador de controladores almacena en caché *ValuePtr* y usará el valor almacenado en caché más adelante en una llamada a **SQLFetchScroll**. No se aplica ninguna otra regla de esta sección.  
  
8.  El administrador de controladores llama a **SQLSetStmtOption** como se indica a continuación:  
  
    ```  
    SQLSetStmtOption (hstmt, fOption, vParam);  
    ```  
  
     donde *hstmt*, *fOption* y *vParam* se establecerán en los valores de *StatementHandle*, *Attribute* y *ValuePtr*, respectivamente. Se omite el argumento *StringLength* .  
  
     Si un controlador ODBC *2. x* admite las opciones de instrucción de cadena de caracteres específicas del controlador, una aplicación ODBC *3. x* debe llamar a **SQLSetStmtOption** para establecer esas opciones.  
  
## <a name="mappings-for-handling-parameter-arrays"></a>Asignaciones para controlar matrices de parámetros  
 Cuando la aplicación llama a:  
  
```  
SQLSetStmtAttr (StatementHandle, SQL_ATTR_PARAMSET_SIZE, Size, StringLength);  
```  
  
 el administrador de controladores llama a:  
  
```  
SQLParamOptions (StatementHandle, Size, &RowCount);  
```  
  
 Más adelante, el administrador de controladores devuelve un puntero a esta variable cuando la aplicación llama a **SQLGetStmtAttr** para recuperar SQL_ATTR_PARAMS_PROCESSED_PTR. El administrador de controladores no puede cambiar esta variable interna hasta que el identificador de instrucción se devuelva al estado preparado o asignado.  
  
 Una aplicación ODBC *3. x* puede llamar a **SQLGetStmtAttr** para obtener el valor de SQL_ATTR_PARAMS_PROCESSED_PTR incluso aunque no haya establecido explícitamente el campo SQL_DESC_ARRAY_SIZE en el APD. Esta situación podría surgir, por ejemplo, si la aplicación tiene una rutina genérica que comprueba la "fila" actual de los parámetros que se están procesando cuando **SQLExecute** devuelve SQL_NEED_DATA. Se invoca esta rutina tanto si el SQL_DESC_ARRAY_SIZE es 1 como si es mayor que 1. Para tener esto en cuenta, el administrador de controladores deberá definir esta variable interna tanto si la aplicación ha llamado a **SQLSetStmtAttr** como si no, para establecer el campo de SQL_DESC_ARRAY_SIZE en APD. Si no se ha establecido SQL_DESC_ARRAY_SIZE, el administrador de controladores tiene que asegurarse de que esta variable contiene el valor 1 antes de devolver **SQLExecDirect** o **SQLExecute**.  
  
## <a name="error-handling"></a>Tratamiento de errores  
 En ODBC *3. x*, si se llama a **SQLFetch** o **SQLFetchScroll** , se rellena el SQL_DESC_ARRAY_STATUS_PTR en IRD y el campo SQL_DIAG_ROW_NUMBER de un registro de diagnóstico determinado contiene el número de la fila del conjunto de filas al que pertenece este registro. Con esto, la aplicación puede correlacionar un mensaje de error con una posición de fila determinada.  
  
 Un controlador ODBC *2. x* no podrá proporcionar esta funcionalidad. Sin embargo, proporcionará la demarcación de errores con SQLSTATE 01S01 (error en la fila). Una aplicación ODBC *3. x* que usa **SQLFetch** o **SQLFetchScroll** al pasar por un controlador ODBC *2. x* debe tener en cuenta este hecho. Tenga en cuenta también que este tipo de aplicación no podrá llamar a **SQLGetDiagField** para obtener realmente el SQL_DIAG_ROW_NUMBER campo de todos modos. Una aplicación ODBC *3. x* que funcione con un controlador ODBC *2. x* podrá llamar a **SQLGetDiagField** solo con un argumento *DiagIdentifier* de SQL_DIAG_MESSAGE_TEXT, SQL_DIAG_NATIVE, SQL_DIAG_RETURNCODE o SQL_DIAG_SQLSTATE. El administrador de controladores ODBC *3. x* mantiene la estructura de datos de diagnóstico cuando se trabaja con un controlador ODBC *2. x* , pero el controlador ODBC *2. x* solo devuelve estos cuatro campos.  
  
 Cuando una aplicación ODBC *2. x* trabaja con un controlador ODBC *2. x* , si una operación puede provocar que el administrador de controladores devuelva varios errores, es posible que el administrador de controladores ODBC *3. x* devuelva distintos errores que el administrador de controladores ODBC 2. *x* .  
  
## <a name="mappings-for-bookmark-operations"></a>Asignaciones de operaciones de marcador  
 El administrador de controladores ODBC *3. x* realiza las siguientes asignaciones cuando una aplicación ODBC *3. x* que trabaja con un controlador ODBC *2. x* realiza operaciones de marcador.  
  
### <a name="sqlbindcol"></a>SQLBindCol  
 Cuando una aplicación ODBC *3. x* que trabaja con un controlador ODBC *2. x* llama a **SQLBindCol** para enlazar con la columna 0 con *fCType* igual a SQL_C_VARBOOKMARK, el administrador de controladores ODBC *3. x* comprueba si el argumento *BufferLength* es menor que 4 o mayor que 4 y, en ese caso, devuelve SQLSTATE HY090 (longitud de búfer o cadena no válida). Si el argumento *BufferLength* es igual a 4, el administrador de controladores llama a **SQLBindCol** en el controlador, después de reemplazar *fCType* por SQL_C_BOOKMARK.  
  
### <a name="sqlcolattribute"></a>SQLColAttribute  
 Cuando una aplicación ODBC *3. x* que trabaja con un controlador ODBC *2. x* llama a **SQLColAttribute** con el argumento *ColumnNumber* establecido en 0, el administrador de controladores devuelve los valores de *FieldIdentifier* que se muestran en la tabla siguiente.  
  
|*FieldIdentifier*|Value|  
|-----------------------|-----------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQL_FALSE|  
|SQL_DESC_CASE_SENSITIVE|SQL_FALSE|  
|SQL_DESC_CATALOG_NAME|"" (cadena vacía)|  
|SQL_DESC_CONCISE_TYPE|SQL_BINARY|  
|SQL_DESC_COUNT|El mismo valor devuelto por **SQLNumResultCols**|  
|SQL_DESC_DATETIME_INTERVAL_CODE|0|  
|SQL_DESC_DISPLAY_SIZE|8|  
|SQL_DESC_FIXED_PREC_SCALE|SQL_FALSE|  
|SQL_DESC_LABEL|"" (cadena vacía)|  
|SQL_DESC_LENGTH|0|  
|SQL_DESC_LITERAL_PREFIX|"" (cadena vacía)|  
|SQL_DESC_LITERAL_SUFFIX|"" (cadena vacía)|  
|SQL_DESC_LOCAL_TYPE_NAME|"" (cadena vacía)|  
|SQL_DESC_NAME|"" (cadena vacía)|  
|SQL_DESC_NULLABLE|SQL_NO_NULLS|  
|SQL_DESC_OCTET_LENGTH|4|  
|SQL_DESC_PRECISION|4|  
|SQL_DESC_SCALE|0|  
|SQL_DESC_SCHEMA_NAME|"" (cadena vacía)|  
|SQL_DESC_SEARCHABLE|SQL_PRED_NONE|  
|SQL_DESC_TABLE_NAME|"" (cadena vacía)|  
|SQL_DESC_TYPE|SQL_BINARY|  
|SQL_DESC_TYPE_NAME|"" (cadena vacía)|  
|SQL_DESC_UNNAMED|SQL_UNNAMED|  
|SQL_DESC_UNSIGNED|SQL_FALSE|  
|SQL_DESC_UPDATEABLE|SQL_ATTR_READ_ONLY|  
  
### <a name="sqldescribecol"></a>SQLDescribeCol  
 Cuando una aplicación ODBC *3. x* que trabaja con un controlador ODBC *2. x* llama a **SQLDescribeCol** con el argumento *ColumnNumber* establecido en 0, el administrador de controladores devuelve los valores que se muestran en la tabla siguiente.  
  
|Buffer|Value|  
|------------|-----------|  
|ColumnName|"" (cadena vacía)|  
|*NameLengthPtr|0|  
|*DataTypePtr|SQL_BINARY|  
|*ColumnSizePtr|4|  
|*DecimalDigitsPtr|0|  
|*NullablePtr|SQL_NO_NULLS|  
  
### <a name="sqlgetdata"></a>SQLGetData  
 Cuando una aplicación ODBC *3. x* que trabaja con un controlador ODBC *2. x* realiza la siguiente llamada a **SQLGetData** para recuperar un marcador:  
  
```  
SQLGetData(StatementHandle, 0, SQL_C_VARBOOKMARK, TargetValuePtr, BufferLength, StrLen_or_IndPtr)  
```  
  
 la llamada se asigna a **SQLGetStmtOption** con un *fOption* de SQL_GET_BOOKMARK, como se indica a continuación:  
  
```  
SQLGetStmtOption(hstmt, SQL_GET_BOOKMARK, TargetValuePtr)  
```  
  
 donde *hstmt* y *pvParam* se establecen en los valores de *StatementHandle* y *TargetValuePtr*, respectivamente. El marcador se devuelve en el búfer señalado por el argumento *pvParam* (*TargetValuePtr*). El valor del búfer al que apunta el argumento *StrLen_or_IndPtr* en la llamada a **SQLGetData** se establece en 4.  
  
 Esta asignación es necesaria para tener en cuenta el caso en el que se llamó a **SQLFetch** antes de la llamada a **SQLGetData** y el controlador ODBC *2. x* no admitía **SQLExtendedFetch**. En este caso, el **SQLFetch** se pasaría a través del controlador ODBC *2. x* , en cuyo caso no se admite la recuperación de marcadores.  
  
 No se puede llamar a **SQLGetData** varias veces en un controlador ODBC *2. x* para recuperar un marcador en elementos, por lo que llamar a **SQLGetData** con el argumento *BufferLength* establecido en un valor inferior a 4 y el argumento *ColumnNumber* establecido en 0 devolverá SQLSTATE HY090 (cadena o longitud de búfer no válida). Sin embargo, se puede llamar a **SQLGetData** varias veces para recuperar el mismo marcador.  
  
### <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
 Cuando una aplicación ODBC *3. x* que trabaja con un controlador ODBC *2. x* llama a **SQLSetStmtAttr** para establecer el atributo de SQL_ATTR_USE_BOOKMARKS en SQL_UB_VARIABLE, el administrador de controladores establece el atributo en SQL_UB_ON en el controlador ODBC *2. x* subyacente.
