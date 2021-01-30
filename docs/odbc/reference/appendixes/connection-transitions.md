---
description: Transiciones de conexión
title: Transiciones de conexión | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transitioning states [ODBC], connection
- connection transitions [ODBC]
- state transitions [ODBC], connection
ms.assetid: 6b6e1a47-4a52-41c8-bb9e-7ddeae09913e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60a26505af6d53265111bd24b08990e61b992902
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165422"
---
# <a name="connection-transitions"></a>Transiciones de conexión
Las conexiones ODBC tienen los Estados siguientes.  
  
|Estado|Descripción|  
|-----------|-----------------|  
|C0|Entorno sin asignar, conexión sin asignar|  
|C1|Entorno asignado, conexión sin asignar|  
|C2|Entorno asignado, conexión asignada|  
|C3|La función de conexión necesita datos|  
|C4|Conexión conectada|  
|C5|Conexión conectada, instrucción asignada|  
|C6|Conexión conectada, transacción en curso. Es posible que una conexión esté en el estado C6 sin instrucciones asignadas en la conexión. Por ejemplo, supongamos que la conexión está en modo de confirmación manual y está en el estado C4. Si se asigna una instrucción, se ejecuta (iniciando una transacción) y, a continuación, se libera, la transacción permanece activa, pero no hay ninguna instrucción en la conexión.|  
  
 En las tablas siguientes se muestra cómo afecta cada función ODBC al estado de la conexión.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|C0<br /><br /> No env.|C1 sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|--------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|C1 [1]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|ADMITIR dos|C2|--[5]|--[5]|--[5]|--[5]|--[5]|  
|ADMITIR 3|ADMITIR|(08003)|(08003)|C5|--[5]|--[5]|  
|ADMITIR 4|ADMITIR|(08003)|(08003)|--[5]|--[5]|--[5]|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DBC *HandleType* .  
  
 [3] esta fila muestra las transiciones cuando se SQL_HANDLE_STMT *HandleType* .  
  
 [4] esta fila muestra las transiciones cuando se SQL_HANDLE_DESC *HandleType* .  
  
 [5] si se llama a **SQLAllocHandle** con *OutputHandlePtr* que apuntan a un identificador válido, se sobrescribe ese identificador sin tener en cuenta el contenido anterior de ese control y podrían producirse problemas con los controladores ODBC. La programación de aplicaciones ODBC es incorrecta para llamar a **SQLAllocHandle** dos veces con la misma variable de aplicación definida para *\* OutputHandlePtr* sin llamar a **SQLFreeHandle** para liberar el identificador antes de reasignarlo. Si se sobrescriben los identificadores ODBC de este modo, se puede producir un comportamiento incoherente o errores en la parte de los controladores ODBC.  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|C3 [d] C4 [s]|--[d] C2 [e] C4 [s]|(08002)|(08002)|(08002)|  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--|--[1] C5 [2]|  
  
 [1] la conexión estaba en modo de confirmación manual.  
  
 [2] la conexión estaba en modo de confirmación automática.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges y SQLTables  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--[1] C6 [2]|--|  
  
 [1] la conexión estaba en modo de confirmación automática o el origen de datos no ha comenzado una transacción.  
  
 [2] la conexión estaba en modo de confirmación manual y el origen de datos comenzó una transacción.  
  
## <a name="sqlconnect"></a>SQLConnect  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|C4|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlcopydesc-sqlgetdescfield-sqlgetdescrec-sqlsetdescfield-and-sqlsetdescrec"></a>SQLCopyDesc, SQLGetDescField, SQLGetDescRec, SQLSetDescField y SQLSetDescRec  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--[1]|--|--|  
  
 [1] en este estado, los únicos descriptores disponibles para la aplicación son descriptores asignados explícitamente.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources y SQLDrivers  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|--|--|--|--|--|--|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|(08003)|C2|C2|C2|25000|  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|C4 s--n [f]|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR dimensional|--[3]|--[3]|--[3]|--|--|--[4] o ([5], [6] y [8]) C4 [5] y [7] C5 [5], [6] y [9]|  
|ADMITIR dos|ADMITIR|(08003)|(08003)|--|--|C5|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DBC *HandleType* .  
  
 [3] debido a que la conexión no está en estado conectado, no se ve afectada por la transacción.  
  
 [4] error en la confirmación o reversión de la conexión. La función devuelve SQL_ERROR en este caso.  
  
 [5] la confirmación o reversión se realizó correctamente en la conexión. La función devuelve SQL_ERROR si no se pudo realizar la operación commit o ROLLBACK en otra conexión, o si la función devuelve SQL_SUCCESS si la instrucción COMMIT o Rollback se realizó correctamente en todas las conexiones.  
  
 [6] había al menos una instrucción asignada en la conexión.  
  
 [7] no se asignaron instrucciones en la conexión.  
  
 [8] la conexión tenía al menos una instrucción para la que había un cursor abierto y el origen de datos conserva los cursores cuando las transacciones se confirman o se revierten, lo que se aplique (dependiendo de si *CompletionType* se SQL_COMMIT o SQL_ROLLBACK). Para obtener más información, vea los atributos SQL_CURSOR_COMMIT_BEHAVIOR y SQL_CURSOR_ROLLBACK_BEHAVIOR en [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 [9] si la conexión tuviera instrucciones para las que había cursores abiertos, no se conservarían los cursores cuando se confirmara o revirtió la transacción.  
  
## <a name="sqlexecdirect-and-sqlexecute"></a>SQLExecDirect y SQLExecute  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--[1] C6 [2] C6 [3]|--|  
  
 [1] la conexión estaba en modo de confirmación automática y la instrucción ejecutada no era una  *especificación* de cursor (como una instrucción SELECT). o la conexión estaba en modo de confirmación manual y la instrucción ejecutada no inició una transacción.  
  
 [2] la conexión estaba en modo de confirmación automática y la instrucción ejecutada era una  *especificación* de cursor (como una instrucción SELECT).  
  
 [3] la conexión estaba en modo de confirmación manual y el origen de datos comenzó una transacción.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR dimensional|C0|HY010|HY010|HY010|HY010|HY010|  
|ADMITIR dos|ADMITIR|C1|HY010|HY010|HY010|HY010|  
|ADMITIR 3|ADMITIR|ADMITIR|ADMITIR|ADMITIR|C4 [5]--[6]|--[7] C4 [5] y [8] C5 [6] y [8]|  
|ADMITIR 4|ADMITIR|ADMITIR|ADMITIR|--|--|--|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DBC *HandleType* .  
  
 [3] esta fila muestra las transiciones cuando se SQL_HANDLE_STMT *HandleType* .  
  
 [4] esta fila muestra las transiciones cuando se SQL_HANDLE_DESC *HandleType* .  
  
 [5] solo había una instrucción asignada en la conexión.  
  
 [6] se asignaron varias instrucciones en la conexión.  
  
 [7] la conexión estaba en modo de confirmación manual.  
  
 [8] la conexión estaba en modo de confirmación automática.  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR dimensional|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--|C5 [3]--[4]|  
|ADMITIR dos|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--|--|  
  
 [1] esta fila muestra las transacciones cuando el argumento de *opción* es SQL_CLOSE.  
  
 [2] esta fila muestra las transacciones cuando el argumento de *opción* es SQL_UNBIND o SQL_RESET_PARAMS.  
  
 [3] la conexión estaba en modo de confirmación automática y no hay ningún cursor abierto en ninguna instrucción excepto en esta.  
  
 [4] la conexión estaba en modo de confirmación manual o estaba en modo de confirmación automática y un cursor estaba abierto al menos en otra instrucción.  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|--[1] 08003 [2]|HY010|--|--|--|  
  
 [1] el argumento de *atributo* era SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, SQL_ATTR_LOGIN_TIMEOUT, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE o SQL_ATTR_TRACEFILE, o se ha establecido un valor para el atributo de conexión.  
  
 [2] el argumento de *atributo* no se SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, SQL_ATTR_LOGIN_TIMEOUT, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE o SQL_ATTR_TRACEFILE, y no se ha establecido un valor para el atributo de conexión.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField y SQLGetDiagRec  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR dimensional|--|--|--|--|--|--|  
|ADMITIR dos|ADMITIR|--|--|--|--|--|  
|ADMITIR 3|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--|--|  
|ADMITIR 4|ADMITIR|ADMITIR|ADMITIR|--|--|--|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DBC *HandleType* .  
  
 [3] esta fila muestra las transiciones cuando se SQL_HANDLE_STMT *HandleType* .  
  
 [4] esta fila muestra las transiciones cuando se SQL_HANDLE_DESC *HandleType* .  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|HY010|HY010|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|--[1] 08003 [2]|08003|--|--|--|  
  
 [1] el argumento *InfoType* se SQL_ODBC_VER.  
  
 [2] el argumento *InfoType* no se SQL_ODBC_VER.  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--[1] C6 [2]|--[3] C5 [1]|  
  
 [1] la conexión estaba en modo de confirmación automática y la llamada a **SQLMoreResults** no ha inicializado el procesamiento de un conjunto de resultados de una especificación de cursor.  
  
 [2] la conexión estaba en modo de confirmación automática y la llamada a **SQLMoreResults** ha inicializado el procesamiento de un conjunto de resultados de una especificación de cursor.  
  
 [3] la conexión estaba en modo de confirmación manual.  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|(08003)|(08003)|--|--|--|  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--[1] C6 [2]|--|  
  
 [1] la conexión estaba en modo de confirmación automática o el origen de datos no ha comenzado una transacción.  
  
 [2] la conexión estaba en modo de confirmación manual y el origen de datos comenzó una transacción.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|--[1] 08003 [2]|HY010|--[3] 08002 [4] HY011 [5]|--[3] 08002 [4] HY011 [5]|--[3] y [6] C5 [8] 08002 [4] HY011 [5] o [7]|  
  
 [1] el argumento de *atributo* no se SQL_ATTR_TRANSLATE_LIB o SQL_ATTR_TRANSLATE_OPTION.  
  
 [2] el argumento de *atributo* se SQL_ATTR_TRANSLATE_LIB o SQL_ATTR_TRANSLATE_OPTION.  
  
 [3] el argumento de *atributo* no se SQL_ATTR_ODBC_CURSORS ni SQL_ATTR_PACKET_SIZE.  
  
 [4] el argumento de *atributo* se SQL_ATTR_ODBC_CURSORS.  
  
 [5] el argumento de *atributo* se SQL_ATTR_PACKET_SIZE.  
  
 [6] el argumento de *atributo* no se SQL_ATTR_AUTOCOMMIT o el argumento de *atributo* se SQL_ATTR_AUTOCOMMIT y el establecimiento de este atributo no confirmó la transacción.  
  
 [7] el argumento de *atributo* se SQL_ATTR_TXN_ISOLATION.  
  
 [8] el argumento de *atributo* se SQL_ATTR_AUTOCOMMIT y el establecimiento de este atributo confirmó la transacción.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|--|--|HY010|--|--|--|  
  
## <a name="all-other-odbc-functions"></a>Todas las demás funciones ODBC  
  
|C0<br /><br /> No env.|C1<br /><br /> Sin asignar|C2<br /><br /> Allocated|C3<br /><br /> Necesidad de datos|C4<br /><br /> Conectado|C5<br /><br /> .|C6<br /><br /> Transacción|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|ADMITIR|ADMITIR|ADMITIR|ADMITIR|ADMITIR|--|--|
