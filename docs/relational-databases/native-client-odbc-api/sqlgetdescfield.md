---
description: SQLGetDescField
title: SQLGetDescField | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLGetDescField function
ms.assetid: 3e59a37a-28ee-4c91-8968-7fe3b966739d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 93d040ac1d32ad04eda08d86646202621b9254b6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465156"
---
# <a name="sqlgetdescfield"></a>SQLGetDescField
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  El controlador ODBC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client solamente expone campos de descriptor específicos del controlador para el descriptor de fila de implementación (IRD). En IRD, se hace referencia a los campos de descriptor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de atributos de columna específicos del controlador. Para obtener información sobre una lista completa de campos de descriptor específicos del controlador disponibles, vea [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md).  
  
 Los campos de descriptor que contienen cadenas de identificador de columna son a menudo cadenas de longitud cero. Todos los valores de campos de descriptor específicos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]son de solo lectura.  
  
 Al igual que los atributos recuperados con SQLColAttribute, los campos de descriptor que notifican atributos de nivel de fila (como SQL_CA_SS_COMPUTE_ID) se notifican para todas las columnas del conjunto de resultados.  
  
## <a name="sqlgetdescfield-and-table-valued-parameters"></a>SQLGetDescField y parámetros con valores de tabla  
 SQLGetDescField se puede usar para obtener valores para atributos extendidos de parámetros con valores de tabla y columnas de parámetros con valores de tabla. Para obtener más información sobre los parámetros con valores de tabla, vea [parámetros con valores de tabla &#40;ODBC&#41;](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md).  
  
## <a name="sqlgetdescfield-support-for-enhanced-date-and-time-features"></a>SQLGetDescField admite las características mejoradas de fecha y hora  
 Para obtener información sobre los campos de descriptor disponibles con los nuevos tipos de fecha y hora, vea [Parameter and Result Metadata](../../relational-databases/native-client-odbc-date-time/metadata-parameter-and-result.md).  
  
 Para obtener más información, vea [mejoras de fecha y hora &#40;ODBC&#41;](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  
  
 A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , SQLGetDescField puede devolver **SQL_C_SS_TIME2** (para tipos de **hora** ) o **SQL_C_SS_TIMESTAMPOFFSET** (para **DateTimeOffset**) en lugar de **SQL_C_BINARY**, si la aplicación usa ODBC 3,8.  
  
## <a name="sqlgetdescfield-support-for-large-clr-udts"></a>SQLGetDescField admite UDT CLR grandes  
 **SQLGetDescField** admite los tipos definidos por el usuario CLR grandes (UDT). Para obtener más información, vea [tipos de User-Defined CLR grandes &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md).  
  
## <a name="sqlgetdescfield-support-for-sparse-columns"></a>SQLGetDescField admite columnas dispersas  
 SQLGetDescField se puede usar para consultar el nuevo campo IRD SQL_CA_SS_IS_COLUMN_SET para determinar si una columna es una columna **COLUMN_SET** .  
  
 Para obtener más información, consulte [compatibilidad con columnas Dispersas &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md).  
  
## <a name="example"></a>Ejemplo  
  
```  
typedef struct tagCOMPUTEBYLIST  
    {  
    SQLSMALLINT nBys;  
    SQLSMALLINT aByList[1];  
    } COMPUTEBYLIST;  
typedef COMPUTEBYLIST* PCOMPUTEBYLIST;   
  
SQLHDESC    hIRD;   
SQLINTEGER  cbIRD;   
SQLINTEGER  nSet = 0;   
  
// . . .  
// Execute a statement that contains a COMPUTE clause,  
//  then get the descriptor handle of the IRD and  
//  get some IRD values.  
  
SQLGetStmtAttr(g_hStmt, SQL_ATTR_IMP_ROW_DESC,  
    (SQLPOINTER) &hIRD, sizeof(SQLHDESC), &cbIRD);  
  
// For statement-wide column attributes, any  
//  descriptor record will do. You know that 1 exists,  
//  so use it.  
SQLGetDescField(hIRD, 1, SQL_CA_SS_NUM_COMPUTES,  
    (SQLPOINTER) &nComputes, SQL_IS_INTEGER, &cbIRD);  
  
if (nSet == 0)  
    {  
    SQLINTEGER      nOrderID;  
  
    printf_s("Normal result set.\n");  
  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        SQLGetDescField(hIRD, nCol+1,  
            SQL_CA_SS_COLUMN_ORDER,  
            (SQLPOINTER) &nOrderID, SQL_IS_INTEGER,  
            &cbIRD);  
  
        if (nOrderID != 0)  
            {  
            printf_s("Col in ORDER BY, pos: %ld",  
                nOrderID);  
            }  
            printf_s("\n");  
        }  
  
    printf_s("\n");  
    }  
else  
    {  
    PCOMPUTEBYLIST  pByList;  
    SQLSMALLINT     nBy;  
    SQLINTEGER      nColID;  
  
    printf_s("Computed result set number: %lu\n",  
        nSet);  
  
    SQLGetDescField(hIRD, 1, SQL_CA_SS_COMPUTE_BYLIST,  
        (SQLPOINTER) &pByList, SQL_IS_INTEGER,  
        &cbIRD);  
  
    if (pByList != NULL)  
        {  
        printf_s("Clause ordered by columns: ");  
        for (nBy = 0; nBy < pByList->nBys; )  
            {  
            printf_s("%u", pByList->aByList[nBy]);  
            nBy++;  
  
            if (nBy == pByList->nBys)  
                {  
                printf_s("\n");  
                }  
            else  
                {  
                printf_s(", ");  
                }  
            }  
        }  
    else  
        {  
        printf_s("Compute clause set not ordered.\n");  
        }  
  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        SQLGetDescField(hIRD, nCol+1,  
            SQL_CA_SS_COLUMN_ID, (SQLPOINTER) &nColID,  
            SQL_IS_INTEGER, &cbIRD);  
        printf_s("ColumnID: %lu, nColID);  
        }  
    printf_s("\n");  
    }  
  
if (SQLMoreResults(g_hStmt) == SQL_SUCCESS)  
    {  
    // Determine the result set indicator.  
    SQLGetDescField(hIRD, 1, SQL_CA_SS_COMPUTE_ID,  
        (SQLPOINTER) &nSet, SQL_IS_INTEGER, &cbIRD);  
    }  
```  
  
## <a name="see-also"></a>Consulte también  
 [SQLGetDescField función)](../../odbc/reference/syntax/sqlgetdescfield-function.md)   
 [ODBC API Implementation Details](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
