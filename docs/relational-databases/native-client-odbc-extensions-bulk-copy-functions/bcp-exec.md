---
description: bcp_exec
title: bcp_exec | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_exec
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_exec function
ms.assetid: b23ea2cc-8545-4873-b0c1-57e76b0a3a7b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06e9cdecd37e5fc1f78de6726384715ad7b8c9bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97406893"
---
# <a name="bcp_exec"></a>bcp_exec
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ejecuta una copia masiva completa de los datos entre una tabla de base de datos y un archivo de usuario.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
RETCODE bcp_exec (  
        HDBC hdbc,  
        LPDBINT pnRowsProcessed);  
```  
  
## <a name="arguments"></a>Argumentos  
 *hdbc*  
 Es el identificador de la conexión ODBC habilitada para la copia masiva.  
  
 *pnRowsProcessed*  
 Es un puntero a un DBINT. La función **bcp_exec** llena este DBINT con el número de filas copiadas correctamente. Si *pnRowsProcessed* es NULL, **bcp_exec** lo omite.  
  
## <a name="returns"></a>Devoluciones  
 SUCCEED, SUCCEED_ASYNC o FAIL. La función **bcp_exce** devuleve SUCCEED si se se copian todas las filas. **bcp_exec** devuelve SUCCEED_ASYNC si la copia masiva asincrónica no se ha completado todavía. **bcp_exce** devuelve FAIL si se produce un error total o si el número de filas que generan errores alcanza el valor que se ha especificado para BCPMAXERRS con [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md). BCPMAXERRS toma como valor predeterminado 10. La opción BCPMAXERRS afecta solo a los errores de sintaxis detectados por el proveedor al leer las filas del archivo de datos (y no las filas enviadas al servidor). El servidor anula el lote cuando detecta un error con una fila. Compruebe en el parámetro *pnRowsProcessed* el número de filas copiadas correctamente.  
  
## <a name="remarks"></a>Comentarios  
 Esta función copia los datos de un archivo de usuario en una tabla de base de datos o viceversa, dependiendo del valor del parámetro *eDirection* de [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md).  
  
 Antes de llamar a **bcp_exec**, llame a **bcp_init** con un nombre de archivo de usuario válido. Si no lo hace, se producirá un error.  
  
 **bcp_exec** es la única función de copia masiva que es probable que quede pendiente durante un período de tiempo indeterminado. Por lo tanto, es la única función de copia masiva que admite el modo asincrónico. Para establecer el modo asincrónico, utilice [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) para establecer SQL_ATTR_ASYNC_ENABLE en SQL_ASYNC_ENABLE_ON antes de llamar a **bcp_exec**. Para comprobar si se ha completado, llame a **bcp_exec** con los mismos parámetros. Si la copia masiva no se ha completado todavía, **bcp_exec** devuelve SUCCEED_ASYNC. También devuelve en *pnRowsProcessed* un recuento del estado del número de filas enviadas al servidor. Las filas enviadas al servidor no se confirman hasta que se alcanza el final de un lote.  
  
 Para obtener información sobre un cambio importante en la copia masiva a partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , vea [realizar operaciones de copia masiva &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md).  
  
## <a name="example"></a>Ejemplo  
 En el siguiente ejemplo, se muestra cómo utilizar **bcp_exec**:  
  
```  
// Variables like henv not specified.  
HDBC      hdbc;  
DBINT      nRowsProcessed;  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...   
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source, return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bulk copy.   
if (bcp_init(hdbc, _T("pubs..authors"), _T("authors.sav"), NULL, DB_OUT)  
   == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Now, execute the bulk copy.   
if (bcp_exec(hdbc, &nRowsProcessed) == FAIL)  
   {  
   if (nRowsProcessed == -1)  
      {  
      printf_s("No rows processed on bulk copy execution.\n");  
      }  
   else  
      {  
      printf_s("Incomplete bulk copy.   Only %ld row%s copied.\n",  
         nRowsProcessed, (nRowsProcessed == 1) ? "": "s");  
      }  
   return;  
   }  
  
printf_s("%ld rows processed.\n", nRowsProcessed);  
  
// Carry on.  
```  
  
## <a name="see-also"></a>Consulte también  
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
