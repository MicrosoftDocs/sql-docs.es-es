---
description: Referencia de API ODBC
title: Referencia de la API de ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apitype: dllExport
ms.assetid: b7a49774-f458-44ce-9a04-a0457501405b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60dab6c50856d9f4132b9ec3d066d15c99194814
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174659"
---
# <a name="odbc-api-reference"></a>Referencia de API ODBC
En los temas de esta sección se describe cada función ODBC en orden alfabético. Cada función se define como una función del lenguaje de programación C. Entre las descripciones se incluyen las siguientes:  
  
-   Propósito  
  
-   Versión de ODBC  
  
-   Nivel de conformidad de la CLI estándar  
  
-   Sintaxis  
  
-   Argumentos  
  
-   Valores devueltos  
  
-   Diagnóstico  
  
-   Comentarios sobre el uso y la implementación  
  
-   Ejemplo de código  
  
-   Referencias a funciones relacionadas  
  
 El nivel de conformidad de la CLI estándar puede ser uno de los siguientes: ISO 92, Open Group, ODBC o deprecated. Una función etiquetada como compatible con ISO 92 también aparece en abrir Grupo versión 1, porque abrir Grupo es un supraconjunto puro de ISO 92. Una función etiquetada como compatible con grupos abiertos también aparece en ODBC 3. *x*, porque ODBC 3. *x* es un supraconjunto puro de la versión 1 del grupo abierto. Una función etiquetada como compatible con ODBC aparece en ninguna de las normas. Una función etiquetada como en desuso ha quedado en desuso en ODBC 3. *x*.  
  
 El control de la información de diagnóstico se describe en la descripción de la función [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) . El texto asociado a los valores SQLSTATE se incluye para proporcionar una descripción de la condición, pero no está pensada para prescribir texto específico.  
  
> [!NOTE]  
>  Para obtener información específica del controlador sobre las funciones ODBC, consulte la sección del controlador.  
  
 Esta sección contiene temas para las siguientes funciones:  
  
-   [Función SQLAllocConnect](../../../odbc/reference/syntax/sqlallocconnect-function.md)  
  
-   [Función SQLAllocEnv](../../../odbc/reference/syntax/sqlallocenv-function.md)  
  
-   [Función SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)  
  
-   [Función SQLAllocStmt](../../../odbc/reference/syntax/sqlallocstmt-function.md)  
  
-   [SQLBindCol (función)](../../../odbc/reference/syntax/sqlbindcol-function.md)  
  
-   [Función SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)  
  
-   [Función SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)  
  
-   [Función SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)  
  
-   [Función SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)  
  
-   [Función SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)  
  
-   [Función SQLCloseCursor](../../../odbc/reference/syntax/sqlclosecursor-function.md)  
  
-   [Función SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)  
  
-   [Función SQLColAttributes](../../../odbc/reference/syntax/sqlcolattributes-function.md)  
  
-   [Función SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)  
  
-   [Función SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)  
  
-   [Función SQLCompleteAsync](../../../odbc/reference/syntax/sqlcompleteasync-function.md)  
  
-   [Función SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)  
  
-   [Función SQLCopyDesc](../../../odbc/reference/syntax/sqlcopydesc-function.md)  
  
-   [Función SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)  
  
-   [SQLDescribeCol (función)](../../../odbc/reference/syntax/sqldescribecol-function.md)  
  
-   [Función SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)  
  
-   [Función SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)  
  
-   [Función SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)  
  
-   [Función SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)  
  
-   [Función SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)  
  
-   [Función SQLError](../../../odbc/reference/syntax/sqlerror-function.md)  
  
-   [Función SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)  
  
-   [Función SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)  
  
-   [Función SQLExtendedFetch](../../../odbc/reference/syntax/sqlextendedfetch-function.md)  
  
-   [Función SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)  
  
-   [Función SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)  
  
-   [Función SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)  
  
-   [Función SQLFreeConnect](../../../odbc/reference/syntax/sqlfreeconnect-function.md)  
  
-   [Función SQLFreeEnv](../../../odbc/reference/syntax/sqlfreeenv-function.md)  
  
-   [Función SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)  
  
-   [Función SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)  
  
-   [Función SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)  
  
-   [Función SQLGetConnectOption](../../../odbc/reference/syntax/sqlgetconnectoption-function.md)  
  
-   [Función SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)  
  
-   [Función SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)  
  
-   [Función SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)  
  
-   [Función SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)  
  
-   [Función SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)  
  
-   [Función SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)  
  
-   [Función SQLGetEnvAttr](../../../odbc/reference/syntax/sqlgetenvattr-function.md)  
  
-   [Función SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md)  
  
-   [Función SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)  
  
-   [Función SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)  
  
-   [Función SQLGetStmtOption](../../../odbc/reference/syntax/sqlgetstmtoption-function.md)  
  
-   [Función SQLGetTypeInfo](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)  
  
-   [SQLMoreResults (función)](../../../odbc/reference/syntax/sqlmoreresults-function.md)  
  
-   [Función SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md)  
  
-   [Función SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)  
  
-   [SQLNumResultCols (función)](../../../odbc/reference/syntax/sqlnumresultcols-function.md)  
  
-   [Función SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)  
  
-   [Función SQLParamOptions](../../../odbc/reference/syntax/sqlparamoptions-function.md)  
  
-   [Función SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)  
  
-   [Función SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)  
  
-   [Función SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)  
  
-   [Función SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)  
  
-   [Función SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)  
  
-   [Función SQLRowCount](../../../odbc/reference/syntax/sqlrowcount-function.md)  
  
-   [Función SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)  
  
-   [Función SQLSetConnectOption](../../../odbc/reference/syntax/sqlsetconnectoption-function.md)  
  
-   [Función SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)  
  
-   [Función SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)  
  
-   [Función SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)  
  
-   [Función SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)  
  
-   [Función SQLSetParam](../../../odbc/reference/syntax/sqlsetparam-function.md)  
  
-   [Función SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)  
  
-   [Función SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)  
  
-   [Función SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)  
  
-   [Función SQLSetStmtOption](../../../odbc/reference/syntax/sqlsetstmtoption-function.md)  
  
-   [Función SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)  
  
-   [Función SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)  
  
-   [Función SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)  
  
-   [Función SQLTables](../../../odbc/reference/syntax/sqltables-function.md)  
  
-   [Función SQLTransact](../../../odbc/reference/syntax/sqltransact-function.md)
