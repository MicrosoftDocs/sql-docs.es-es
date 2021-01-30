---
description: Transiciones de instrucción
title: Transiciones de instrucciones | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transitioning states [ODBC], statement
- state transitions [ODBC], statement
- statement transitions [ODBC]
ms.assetid: 3d70e0e3-fe83-4b4d-beac-42c82495a05b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7fb43bb8aa7ad9bd71906036a081dac4ec91c794
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202550"
---
# <a name="statement-transitions"></a>Transiciones de instrucción
Las instrucciones ODBC tienen los Estados siguientes.  
  
|Estado|Descripción|  
|-----------|-----------------|  
|S0|Instrucción sin asignar. (El estado de conexión debe ser C4, C5 o C6. Para obtener más información, vea [transiciones de conexión](../../../odbc/reference/appendixes/connection-transitions.md)).|  
|S1|Instrucción asignada.|  
|S2|Instrucción preparada. No se creará ningún conjunto de resultados.|  
|S3|Instrucción preparada. Se creará un conjunto de resultados (posiblemente vacío).|  
|S4|Instrucción ejecutada y no se ha creado ningún conjunto de resultados.|  
|S5|Instrucción ejecutada y se ha creado un conjunto de resultados (posiblemente vacío). El cursor está abierto y colocado antes de la primera fila del conjunto de resultados.|  
|S6|Cursor colocado con **SQLFetch** o **SQLFetchScroll**.|  
|S7|Cursor colocado con **SQLExtendedFetch**.|  
|S8|La función necesita datos. No se ha llamado a **SQLParamData** .|  
|S9|La función necesita datos. No se ha llamado a **SQLPutData** .|  
|S10|La función necesita datos. Se ha llamado a **SQLPutData** .|  
|S11|Todavía en ejecución. Una instrucción se deja en este estado después de que una función que se ejecuta de forma asincrónica devuelva SQL_STILL_EXECUTING. Una instrucción está temporalmente en este estado mientras se está ejecutando cualquier función que acepte un identificador de instrucción. La residencia temporal en el estado S11 no se muestra en ninguna de las tablas de estado excepto en la tabla de estado de **SQLCancel**. Aunque una instrucción está temporalmente en el estado S11, la función se puede cancelar llamando a **SQLCancel** desde otro subproceso.|  
|S12|Ejecución asincrónica cancelada. En S12, una aplicación debe llamar a la función cancelada hasta que devuelva un valor distinto de SQL_STILL_EXECUTING. La función se canceló correctamente solo si la función devuelve SQL_ERROR y SQLSTATE HY008 (operación cancelada). Si devuelve cualquier otro valor, como SQL_SUCCESS, se produce un error en la operación de cancelación y la función se ejecuta con normalidad.|  
  
 Los Estados S2 y S3 se conocen como Estados preparados, Estados S5 a través de S7 como Estados de cursor, Estados S8 a través de S10 como Estados de datos necesarios y Estados S11 y S12 como Estados asincrónicos. En cada uno de estos grupos, las transiciones se muestran por separado solo cuando son diferentes para cada estado del grupo. en la mayoría de los casos, las transiciones para cada estado de cada uno de los grupos son las mismas.  
  
 En las tablas siguientes se muestra cómo afecta cada función ODBC al estado de la instrucción.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1], [5], [6]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|--[2], [5]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|S1 [3]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|--[4], [5]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
  
 [1] esta fila muestra las transiciones cuando se SQL_HANDLE_ENV *HandleType* .  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_DBC *HandleType* .  
  
 [3] esta fila muestra las transiciones cuando se SQL_HANDLE_STMT *HandleType* .  
  
 [4] esta fila muestra las transiciones cuando se SQL_HANDLE_DESC *HandleType* .  
  
 [5] si se llama a **SQLAllocHandle** con *OutputHandlePtr* que apuntan a un identificador válido, se sobrescribe ese identificador sin tener en cuenta el contenido anterior para ese identificador y podrían producirse problemas con los controladores ODBC. La programación de aplicaciones ODBC es incorrecta para llamar a **SQLAllocHandle** dos veces con la misma variable de aplicación definida para *\* OutputHandlePtr* sin llamar a **SQLFreeHandle** para liberar el identificador antes de reasignarlo. Si se sobrescriben los identificadores ODBC de tal manera, se podría producir un comportamiento incoherente o errores en la parte de los controladores ODBC.  
  
## <a name="sqlbindcol"></a>SQLBindCol  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--|--|--|--|HY010|HY010|  
  
## <a name="sqlbindparameter"></a>SQLBindParameter  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--|--|--|--|HY010|HY010|  
  
## <a name="sqlbrowseconnect-sqlconnect-and-sqldriverconnect"></a>SQLBrowseConnect, SQLConnect y SQLDriverConnect  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|08002|08002|08002|08002|08002|08002|08002|  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|HY010|24000|Vea la tabla siguiente|HY010|NS [c] HY010 o|  
  
## <a name="sqlbulkoperations-cursor-states"></a>SQLBulkOperations (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|--[s] S8 [d] S11 [x]|--[s] S8 [d] S11 [x]|HY010|  
  
## <a name="sqlcancel"></a>SQLCancel  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--|--|--|--|S1 [1] S2 [NR] y [2] S3 [r] y [2] S5 [3] y [5] S6 ([3] o [4]) y [6] S7 [4] y [7]|Vea la tabla siguiente|  
  
 [1]   **SQLExecDirect** devolvió SQL_NEED_DATA.  
  
 [2]   **SQLExecute** devolvió SQL_NEED_DATA.  
  
 [3]   **SQLBulkOperations** devolvió SQL_NEED_DATA.  
  
 [4]   **SQLSetPos** devolvió SQL_NEED_DATA.  
  
 [5] no se llamó a   **SQLFetch**, **SQLFetchScroll** o **SQLExtendedFetch** .  
  
 [6] se ha llamado a   **SQLFetch** o **SQLFetchScroll** .  
  
 [7] se ha llamado a   **SQLExtendedFetch** .  
  
## <a name="sqlcancel-asynchronous-states"></a>SQLCancel (Estados asincrónicos)  
  
|S11<br /><br /> Todavía en ejecución|S12<br /><br /> Asincrónico cancelado|  
|-----------------------------|-----------------------------|  
|NS [1] S12 [2]|S12|  
  
 [1] la instrucción estaba temporalmente en el estado S11 mientras se estaba ejecutando una función. Se llamó a **SQLCancel** desde un subproceso diferente.  
  
 [2] la instrucción estaba en el estado S11 porque una función llamada de forma asincrónica devolvió SQL_STILL_EXECUTING.  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|24000|24000|24000|S1 [NP] S3 [p]|HY010|HY010|  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|Vea la tabla siguiente|24000|--[s] S11 [x]|HY010|NS [c] HY010 o|  
  
## <a name="sqlcolattribute-prepared-states"></a>SQLColAttribute (Estados preparados)  
  
|S2<br /><br /> Sin resultados|S3<br /><br /> Resultados|  
|-----------------------|--------------------|  
|--[1] 07005 [2]|--[s] S11 x|  
  
 [1]   *FieldIdentifier* se SQL_DESC_COUNT.  
  
 [2]   *FieldIdentifier* no se SQL_DESC_COUNT.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges y SQLTables  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|S5 [s] S11 [x]|S1 [e] S5 [s] S11 [x]|S1 [e] y [1] S5 [s] y [1] S11 [x] y [1] 24000 [2]|Vea la tabla siguiente|HY010|NS [c] HY010 o|  
  
 [1] el resultado actual es el último o único resultado, o bien no hay ningún resultado actual. Para obtener más información sobre varios resultados, vea [varios resultados](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [2] el resultado actual no es el último resultado.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables-cursor-states"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges y SQLTables (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000 [1]|24000|  
  
 [1] este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.  
  
## <a name="sqlcopydesc"></a>SQLCopyDesc  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR [1]|--|--|--|--|HY010|NS [c] y [3] HY010 [o] o [4]|  
|ADMITIR [2]|HY010|Vea la tabla siguiente|24000|--[s] S11 x|HY010|NS [c] y [3] HY010 [o] o [4]|  
  
 [1] esta fila muestra las transiciones cuando el argumento *SourceDescHandle* era ARD, APD o IPD.  
  
 [2] esta fila muestra las transiciones cuando el argumento *SourceDescHandle* era un IRD.  
  
 [3] los argumentos *SourceDescHandle* y *TargetDescHandle* eran los mismos que en la función **SQLCopyDesc** que se ejecuta de forma asincrónica.  
  
 [4] el argumento *SourceDescHandle* o el argumento *TargetDescHandle* (o ambos) eran diferentes de los de la función **SQLCopyDesc** que se ejecuta de forma asincrónica.  
  
## <a name="sqlcopydesc-prepared-states"></a>SQLCopyDesc (Estados preparados)  
  
|S2<br /><br /> Sin resultados|S3<br /><br /> Resultados|  
|-----------------------|--------------------|  
|24000 [1]|--[s] S11 [x]|  
  
 dimensional Esta fila muestra las transiciones cuando el argumento *SourceDescHandle* era un IRD.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources y SQLDrivers  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqldescribecol"></a>SQLDescribeCol  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|Vea la tabla siguiente|24000|--[s] S11 [x]|HY010|NS [c] HY010 o|  
  
## <a name="sqldescribecol-prepared-states"></a>SQLDescribeCol (Estados preparados)  
  
|S2<br /><br /> Sin resultados|S3<br /><br /> Resultados|  
|-----------------------|--------------------|  
|07005|--[s] S11 [x]|  
  
## <a name="sqldescribeparam"></a>SQLDescribeParam  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|--[s] S11 [x]|HY010|HY010|HY010|NS [c] HY010 [o]|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|S0 [1]|S0 [1]|S0 [1]|S0 [1]|HY010|HY010|  
  
 [1] al llamar a **SQLDisconnect** se liberan todas las instrucciones asociadas a la conexión. Además, esto devuelve el estado de la conexión a C2; el estado de la conexión debe ser C4 antes de que el estado de la instrucción sea S0.  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--[2] o [3] S1 [1]|--[3] S1 [NP] y ([1] o [2]) S1 [p] y [1] S2 [p] y [2]|--[3] S1 [NP] y ([1] o [2]) S1 [p] y [1] S3 [p] y [2]|HY010|HY010|  
  
 [1] el argumento *CompletionType* es SQL_COMMIT y **SQLGetInfo** devuelve SQL_CB_DELETE para el tipo de información SQL_CURSOR_COMMIT_BEHAVIOR, o el argumento *CompletionType* es SQL_ROLLBACK y **SQLGetInfo** devuelve SQL_CB_DELETE para el tipo de información SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
 [2] el argumento *CompletionType* es SQL_COMMIT y **SQLGetInfo** devuelve SQL_CB_CLOSE para el tipo de información SQL_CURSOR_COMMIT_BEHAVIOR, o el argumento *CompletionType* es SQL_ROLLBACK y **SQLGetInfo** devuelve SQL_CB_CLOSE para el tipo de información SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
 [3] el argumento *CompletionType* es SQL_COMMIT y **SQLGetInfo** devuelve SQL_CB_PRESERVE para el tipo de información SQL_CURSOR_COMMIT_BEHAVIOR, o el argumento *CompletionType* es SQL_ROLLBACK y **SQLGetInfo** devuelve SQL_CB_PRESERVE para el tipo de información SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
## <a name="sqlexecdirect"></a>SQLExecDirect  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|S4 [s] y [NR] S5 [s] y [r] S8 [d] S11 [x]|--[e] y [1] S1 [e] y [2] S4 [s] y [NR] S5 [s] y [r] S8 [d] S11 [x]|--[e], [1] y [3] S1 [e], [2] y [3] S4 [s], [NR] y [3] S5 [s], [r] y [3] S8 [d] y [3] S11 [x] y [3] 24000 [4]|Vea la tabla siguiente|HY010|NS [c] HY010 [o]|  
  
 [1] el administrador de controladores ha devuelto el error.  
  
 [2] el administrador de controladores no ha devuelto el error.  
  
 [3] el resultado actual es el último o único resultado, o bien no hay ningún resultado actual. Para obtener más información sobre varios resultados, vea [varios resultados](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [4] el resultado actual no es el último resultado.  
  
## <a name="sqlexecdirect-cursor-states"></a>SQLExecDirect (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000 [1]|24000|  
  
 [1] este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.  
  
## <a name="sqlexecute"></a>SQLExecute  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|Vea la tabla siguiente|S2 [e], p, y [1] S4 [s], [p], [NR] y [1] S5 [s], [p], [r] y [1] S8 [d], [p] y [1] S11 [x], [p] y [1] 24000 [p] y [2] HY010 [NP]|Vea tabla de Estados de cursores|HY010|NS [c] HY010 [o]|  
  
 [1] el resultado actual es el último o único resultado, o bien no hay ningún resultado actual. Para obtener más información sobre varios resultados, vea [varios resultados](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [2] el resultado actual no es el último resultado.  
  
## <a name="sqlexecute-prepared-states"></a>SQLExecute (Estados preparados)  
  
|S2<br /><br /> Sin resultados|S3<br /><br /> Resultados|  
|-----------------------|--------------------|  
|S4 [s] S8 [d] S11 [x]|S5 [s] S8 [d] S11 [x]|  
  
## <a name="sqlexecute-cursor-states"></a>SQLExecute (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000 [p] HY010 [NP]|24000 [p], [1] HY010 [NP]|24000 [p] HY010 [NP]|  
  
 [1] este error lo devuelve el administrador de controladores si **SQLFetch** o **sqlfetchscroll** no ha devuelto SQL_NO_DATA y lo devuelve el controlador si **SQLFetch** o **sqlfetchscroll** ha devuelto SQL_NO_DATA.  
  
## <a name="sqlextendedfetch"></a>SQLExtendedFetch  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|S1010|S1010|24000|Vea la tabla siguiente|S1010|NS [c] S1010 [o]|  
  
## <a name="sqlextendedfetch-cursor-states"></a>SQLExtendedFetch (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|S7 [s] o [NF] S11 [x]|S1010|--[s] o [NF] S11 [x]|  
  
## <a name="sqlfetch-and-sqlfetchscroll"></a>SQLFetch y SQLFetchScroll  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|HY010|24000|Vea la tabla siguiente|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlfetch-and-sqlfetchscroll-cursor-states"></a>SQLFetch y SQLFetchScroll (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|S6 [s] o [NF] S11 [x]|--[s] o [NF] S11 [x]|HY010|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|HY010|HY010|HY010|HY010|HY010|HY010|  
|ADMITIR [2]|S0|S0|S0|S0|HY010|HY010|  
|--[3]|--|--|--|--|--|--|  
  
 [1] esta fila muestra las transiciones cuando *HandleType* se SQL_HANDLE_ENV o SQL_HANDLE_DBC.  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_STMT *HandleType* .  
  
 [3] esta fila muestra las transiciones cuando se SQL_HANDLE_DESC *HandleType* y el descriptor se ha asignado explícitamente.  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR [1]|--|--|S1 [NP] S2 [p]|S1 [NP] S3 [p]|HY010|HY010|  
|ADMITIR [2]|--|--|--|--|HY010|HY010|  
  
 [1] esta fila muestra las transiciones cuando se SQL_CLOSE la *opción* .  
  
 [2] esta fila muestra las transiciones cuando la *opción* se SQL_UNBIND o SQL_RESET_PARAMS. Si el argumento de *opción* se SQL_DROP y el controlador subyacente es un controlador ODBC 3 *. x* , el administrador de controladores lo asigna a una llamada a **SQLFreeHandle** con *HandleType* establecido en SQL_HANDLE_STMT. Para obtener más información, vea la tabla de transición de [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetcursorname"></a>SQLGetCursorName  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--|--|--|--|HY010|HY010|  
  
## <a name="sqlgetdata"></a>SQLGetData  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|HY010|24000|Vea la tabla siguiente|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlgetdata-cursor-states"></a>SQLGetData (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|--[s] o [NF] S11 [x] 24000 [b] HY109 [i]|--[s] o [NF] S11 [x] 24000 [b] HY109 [i]|  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec"></a>SQLGetDescField y SQLGetDescRec  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--[1] o [2] HY010 [3]|Vea la tabla siguiente|--[1] o [2] 24000 [3]|--[1], [2] o [3] S11 [3] y [x]|HY010|NS [c] o [4] HY010 [o] y [5]|  
  
 [1] el argumento *DescriptorHandle* era APD o ARD.  
  
 [2] el argumento *DescriptorHandle* era un IPD.  
  
 [3] el argumento *DescriptorHandle* era un IRD.  
  
 [4] el argumento *DescriptorHandle* era el mismo que el argumento *DescriptorHandle* en la función **SQLGetDescField** o **SQLGetDescRec** que se ejecuta de forma asincrónica.  
  
 [5] el argumento *DescriptorHandle* era diferente del argumento *DescriptorHandle* en la función **SQLGetDescField** o **SQLGetDescRec** que se ejecuta de forma asincrónica.  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec-prepared-states"></a>SQLGetDescField y SQLGetDescRec (Estados preparados)  
  
|S2<br /><br /> Sin resultados|S3<br /><br /> Resultados|  
|-----------------------|--------------------|  
|--[1], [2] o [3] S11 [2] y [x]|--[1], [2] o [3] S11 [x]|  
  
 [1] el argumento *DescriptorHandle* era APD o ARD.  
  
 [2] el argumento *DescriptorHandle* era un IPD.  
  
 [3] el argumento *DescriptorHandle* era un IRD. Tenga en cuenta que estas funciones siempre devuelven SQL_NO_DATA en el estado S2 cuando *DescriptorHandle* era un IRD.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField y SQLGetDiagRec  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|--|--|--|--|--|--|  
|ADMITIR [2]|--[3]|--[3]|--|--|--[3]|--[3]|  
  
 [1] esta fila muestra las transiciones cuando *HandleType* se SQL_HANDLE_ENV, SQL_HANDLE_DBC o SQL_HANDLE_DESC.  
  
 [2] esta fila muestra las transiciones cuando se SQL_HANDLE_STMT *HandleType* .  
  
 [3]   **SQLGetDiagField** siempre devuelve un error en este estado cuando *DiagIdentifier* es SQL_DIAG_ROW_COUNT.  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--[1] 24000 [2]|--[1] 24000 [2]|--[1] 24000 [2]|Vea la tabla siguiente|HY010|HY010|  
  
 [1] el atributo de instrucción no se SQL_ATTR_ROW_NUMBER.  
  
 [2] el atributo de instrucción se SQL_ATTR_ROW_NUMBER.  
  
## <a name="sqlgetstmtattr-cursor-states"></a>SQLGetStmtAttr (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|--[1] 24000 [2]|--[1] o ([v] y [2]) 24000 [b] y [2] HY109 [i] y [2]|--[i] o ([v] y [2]) 24000 [b] y [2] HY109 [1] y [2]|  
  
 [1] el argumento de *atributo* no se SQL_ATTR_ROW_NUMBER.  
  
 [2] el argumento de *atributo* se SQL_ATTR_ROW_NUMBER.  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--[1]|--[1]|--[s] y [2] S1 [NF], [NP] y [4] S2 [NF], [p] y [4] S5 [s] y [3] S11 [x]|S1 [NF], [NP] y [4] S3 [NF], [p] y [4] S4 [s] y [2] S5 [s] y [3] S11 [x]|HY010|NS [c] HY010 [o]|  
  
 [1] la función siempre devuelve SQL_NO_DATA en este estado.  
  
 [2] el siguiente resultado es un recuento de filas.  
  
 [3] el siguiente resultado es un conjunto de resultados.  
  
 [4] el resultado actual es el último resultado.  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlnumparams"></a>SQLNumParams  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|--[s] S11 [x]|--[s] S11 [x]|--[s] S11 [x]|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlnumresultcols"></a>SQLNumResultCols  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|--[s] S11 [x]|--[s] S11 [x]|--[s] S11 [x]|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlparamdata"></a>SQLParamData  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|HY010|HY010|HY010|Vea la tabla siguiente|NS [c] HY010 [o]|  
  
## <a name="sqlparamdata-need-data-states"></a>SQLParamData (requerir Estados de datos)  
  
|S8<br /><br /> Necesidad de datos|S9<br /><br /> Debe colocar|S10<br /><br /> Puede colocar|  
|----------------------|---------------------|---------------------|  
|S1 [e] y [1] S2 [e], [NR] y [2] S3 [e], [r] y [2] S5 [e] y [4] S6 [e] y [5] S7 [e] y [3] S9 [d] S11 [x]|HY010|S1 [e] y [1] S2 [e], [NR] y [2] S3 [e], [r] y [2] S4 [s], [NR] y ([1] o [2]) S5 [s], [r] y ([1] o [2]) S5 ([s] o [e]) y [4] S6 ([s] o [e]) y [5] S7 ([s] o [e]) y [3] S9 [d] S11 [x]|  
  
 [1]   **SQLExecDirect** devolvió SQL_NEED_DATA.  
  
 [2]   **SQLExecute** devolvió SQL_NEED_DATA.  
  
 [3] se ha llamado a   **SQLSetPos** desde el estado S7 y se ha devuelto SQL_NEED_DATA.  
  
 [4] se ha llamado a   **SQLBulkOperations** desde el estado S5 y se ha devuelto SQL_NEED_DATA.  
  
 [5] se ha llamado a   **SQLSetPos** o a **SQLBulkOperations** desde el estado S6 y se ha devuelto SQL_NEED_DATA.  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|S2 [s] y [NR] S3 [s] y [r] S11 [x]|--[s] o ([e] y [1]) S1 [e] y [2] S11 [x]|S1 [e] y [3] S2 [s], [NR] y [3] S3 [s], [r] y [3] S11 [x] y [3] 24000 [4]|Vea la tabla siguiente|HY010|NS [c] HY010 [o]|  
  
 [1] se produce un error en la preparación por un motivo distinto al de la validación de la instrucción (el SQLSTATE era HY009 [valor de argumento no válido] o HY090 [longitud de búfer o cadena no válida]).  
  
 [2] se produce un error en la preparación al validar la instrucción (no se HY009 el SQLSTATE [valor de argumento no válido] o HY090 [longitud de búfer o cadena no válida]).  
  
 [3] el resultado actual es el último o único resultado, o bien no hay ningún resultado actual. Para obtener más información sobre varios resultados, vea [varios resultados](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [4] el resultado actual no es el último resultado.  
  
## <a name="sqlprepare-cursor-states"></a>SQLPrepare (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000|24000|  
  
## <a name="sqlputdata"></a>SQLPutData  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|HY010|HY010|HY010|Vea la tabla siguiente|NS [c] HY010 [o]|  
  
## <a name="sqlputdata-need-data-states"></a>SQLPutData (requerir Estados de datos)  
  
|S8<br /><br /> Necesidad de datos|S9<br /><br /> Debe colocar|S10<br /><br /> Puede colocar|  
|----------------------|---------------------|---------------------|  
|HY010|S1 [e] y [1] S2 [e], [NR] y [2] S3 [e], [r] y [2] S5 [e] y [4] S6 [e] y [5] S7 [e] y [3] S10 [s] S11 [x]|--[s] S1 [e] y [1] S2 [e], [NR] y [2] S3 [e], [r] y [2] S5 [e] y [4] S6 [e] y [5] S7 [e] y [3] S11 [x] HY011 [6]|  
  
 [1]   **SQLExecDirect** devolvió SQL_NEED_DATA.  
  
 [2]   **SQLExecute** devolvió SQL_NEED_DATA.  
  
 [3] se ha llamado a   **SQLSetPos** desde el estado S7 y se ha devuelto SQL_NEED_DATA.  
  
 [4] se ha llamado a   **SQLBulkOperations** desde el estado S5 y se ha devuelto SQL_NEED_DATA.  
  
 [5] se ha llamado a   **SQLSetPos** o a **SQLBulkOperations** desde el estado S6 y se ha devuelto SQL_NEED_DATA.  
  
 [6] una o más llamadas a **SQLPutData** para un único parámetro devuelto SQL_SUCCESS y, a continuación, se realizó una llamada a **SQLPutData** para el mismo parámetro con *StrLen_or_Ind* establecido en SQL_NULL_DATA.  
  
## <a name="sqlrowcount"></a>SQLRowCount  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|HY010|--|--|HY010|HY010|  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|--|--|--|--[2] 24000 [3]|HY010|HY010|  
  
 [1] esta fila muestra las transiciones cuando el *atributo* era un atributo de conexión. Para las transiciones cuando el *atributo* era un atributo de instrucción, vea la tabla de transición de instrucciones para **SQLSetStmtAttr**.  
  
 [2] el argumento de *atributo* no se SQL_ATTR_CURRENT_CATALOG.  
  
 [3] se SQL_ATTR_CURRENT_CATALOG el argumento de *atributo* .  
  
## <a name="sqlsetcursorname"></a>SQLSetCursorName  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--|--|24000|24000|HY010|HY010|  
  
## <a name="sqlsetdescfield-and-sqlsetdescrec"></a>SQLSetDescField y SQLSetDescRec  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR [1]|--|--|--|--|HY010|HY010|  
  
 [1] esta fila muestra las transiciones en las que el argumento *DescriptorHandle* es ARD, APD, IPD o (para **SQLSETDESCFIELD**) un IRD cuando el argumento *FieldIdentifier* es SQL_DESC_ARRAY_STATUS_PTR o SQL_DESC_ROWS_PROCESSED_PTR. Es un error llamar a **SQLSetDescField** para un IRD cuando *FieldIdentifier* es cualquier otro valor.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|HY011|HY011|HY011|HY011|Y011|HY01|HY011|  
  
## <a name="sqlsetpos"></a>SQLSetPos  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|HY010|HY010|24000|Vea la tabla siguiente|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlsetpos-cursor-states"></a>SQLSetPos (Estados de cursor)  
  
|S5<br /><br /> Abierto|S6<br /><br /> SQLFetch o SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|--[s] S8 [d] S11 [x] 24000 [b] HY109 [i]|--[s] S8 [d] S11 [x] 24000 [b] HY109 [i]|  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
  
|S0<br /><br /> Sin asignar|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Realiza|S5-S7<br /><br /> Cursor|S8-S10<br /><br /> Necesidad de datos|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|ADMITIR|--|--[1] HY011 [2]|--[1] 24000 [2]|--[1] 24000 [2]|HY010 [NP] o [1] HY011 [p] y [2]|HY010 [NP] o [1] HY011 [p] y [2]|  
  
 [1] el argumento de *atributo* no se SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR, SQL_ATTR_USE_BOOKMARKS, SQL_ATTR_CURSOR_SCROLLABLE o SQL_ATTR_CURSOR_SENSITIVITY.  
  
 [2] el argumento de *atributo* era SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR, SQL_ATTR_USE_BOOKMARKS, SQL_ATTR_CURSOR_SCROLLABLE o SQL_ATTR_CURSOR_SENSITIVITY.
