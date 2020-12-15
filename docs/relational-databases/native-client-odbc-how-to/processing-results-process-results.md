---
description: 'Procesar resultados: procesar resultados'
title: Procesar resultados (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- processing results [ODBC]
ms.assetid: 4810fe3f-78ee-4f0d-8bcc-a4659fbcf46f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 12c2e1a04ce4ed79d90d7f8e10852e68e0e86352
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440031"
---
# <a name="processing-results---process-results"></a>Procesar resultados: procesar resultados
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

El procesamiento de los resultados en una aplicación ODBC implica determinar primero las características del conjunto de resultados y, a continuación, recuperar los datos en variables de programa mediante [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) o [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md).  
  
### <a name="to-process-results"></a>Para procesar resultados  
  
1.  Recupere información del conjunto de resultados.  
  
2.  Si se usan columnas enlazadas, por cada columna a la que quiera enlazar, llame a [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) para enlazar un búfer de programa a la columna.  
  
3.  Por cada fila del conjunto de resultados:  
  
    -   Llame a [SQLFetch](../../odbc/reference/syntax/sqlfetch-function.md) para obtener la siguiente fila.  
  
    -   Si se utilizan columnas enlazadas, utilice los datos que están ahora disponibles en los búferes de columna enlazada.  
  
    -   Si se usan columnas sin enlazar, llame a [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) una o más veces para obtener los datos de las columnas sin enlazar después de la última columna enlazada. Las llamadas a **SQLGetData** deber estar en orden de número de columna creciente.  
  
    -   Llame a **SQLGetData** varias veces para obtener datos de una columna de texto o de imagen.  
  
4.  Cuando [SQLFetch](../../odbc/reference/syntax/sqlfetch-function.md) señale el fin del conjunto de resultados devolviendo SQL_NO_DATA, llame a [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md) para determinar si está disponible otro conjunto de resultados.  
  
    -   Si devuelve SQL_SUCCESS, está disponible otro conjunto de resultados.  
  
    -   Si devuelve SQL_NO_DATA, no hay ningún otro conjunto de resultados disponible.  
  
    -   Si devuelve SQL_SUCCESS_WITH_INFO o SQL_ERROR, llame a [SQLGetDiagRec](../../odbc/reference/syntax/sqlgetdiagrec-function.md) para determinar si está disponible la salida de una instrucción PRINT o RAISERROR.  
  
         Si se utilizan parámetros de instrucción enlazados como parámetros de salida o como valor devuelto de un procedimiento almacenado, utilice los datos ahora disponibles en los búferes de parámetros enlazados. Asimismo, si se usan parámetros enlazados, cada llamada a [SQLExecute](../../odbc/reference/syntax/sqlexecute-function.md) o [SQLExecDirect](../../odbc/reference/syntax/sqlexecdirect-function.md) ejecutará la instrucción SQL *S* veces, donde *S* es el número de elementos en la matriz de parámetros enlazados. Esto significa que habrá *S* conjuntos de resultados para procesar, donde cada conjunto de resultados comprende todos los conjuntos de resultados, parámetros de salida y códigos de retorno devueltos normalmente por una ejecución única de la instrucción SQL.  
  
    > [!NOTE]  
    >  Cuando un conjunto de resultados contiene filas del cálculo, cada fila del cálculo está disponible como un conjunto de resultados independiente. Estos conjuntos de resultados de cálculo se intercalan entre las filas normales e interrumpen las filas normales en varios conjuntos de resultados.  
  
5.  Opcionalmente, llame a [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) con SQL_UNBIND para liberar los búferes de columna enlazada.  
  
6.  Si está disponible otro conjunto de resultados, vaya al paso 1.  

> [!NOTE]  
>  Para cancelar el procesamiento de un conjunto de resultados antes de que [SQLFetch](../../odbc/reference/syntax/sqlfetch-function.md) devuelva SQL_NO_DATA, llame a [SQLCloseCursor](../../relational-databases/native-client-odbc-api/sqlclosecursor.md).  
  
## <a name="see-also"></a>Consulte también  
[Recuperar información del conjunto de resultados &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/processing-results-retrieve-result-set-information.md)   
  
