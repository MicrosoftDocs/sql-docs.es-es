---
description: Preparar y ejecutar una instrucción (ODBC)
title: Preparar y ejecutar una instrucción (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statement execution
- statement preparation
ms.assetid: 0adecc63-4da5-486c-bc48-09a004a2fae6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e26cb2ffe39eeafc510d7f17bc324e12c5a44502
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476226"
---
# <a name="prepare-and-execute-a-statement-odbc"></a>Preparar y ejecutar una instrucción (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-prepare-a-statement-once-and-then-execute-it-multiple-times"></a>Para preparar una instrucción una sola vez y ejecutarla varias veces  
  
1.  Llame a [SQLPrepare Function](../../../odbc/reference/syntax/sqlprepare-function.md) para preparar la instrucción.  
  
2.  Puede llamar a [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) para determinar el número de parámetros de la instrucción preparada.  
  
3.  Para cada uno de los parámetros de la instrucción preparada, puede hacer lo siguiente:  
  
    -   Llamar a [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) para obtener información sobre los parámetros.  
  
    -   Enlazar cada parámetro a una variable de programa utilizando [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md). Configurar cualquier parámetro de datos en ejecución.  
  
4.  Para cada ejecución de una instrucción preparada:  
  
    -   Si la instrucción incluye marcadores de parámetros, coloque los valores de datos en el búfer de parámetros enlazados.  
  
    -   Llame a [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) para ejecutar la instrucción preparada.  
  
    -   Si se usan parámetros de entrada de datos en ejecución, [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) devuelve SQL_NEED_DATA. Envíe los datos en fragmentos utilizyo [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md) y [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md).  
  
### <a name="to-prepare-a-statement-with-column-wise-parameter-binding"></a>Para preparar una instrucción con enlace de parámetros de modo de columna  
  
1.  Llame a [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) para establecer los atributos siguientes:  
  
    -   Establezca SQL_ATTR_PARAMSET_SIZE en el número de conjuntos (S) de parámetros.  
  
    -   Establezca SQL_ATTR_PARAM_BIND_TYPE en SQL_PARAMETER_BIND_BY_COLUMN.  
  
    -   Establezca el atributo SQL_ATTR_PARAMS_PROCESSED_PTR para que señale a una variable SQLUINTEGER que incluya el número de parámetros procesados.  
  
    -   Establezca SQL_ATTR_PARAMS_STATUS_PTR para que señale a una matriz[S] de variables SQLUSSMALLINT que incluya indicadores de estado de parámetros.  
  
2.  Llame a SQLPrepare para preparar la instrucción.  
  
3.  Puede llamar a [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) para determinar el número de parámetros de la instrucción preparada.  
  
4.  Opcionalmente, para cada parámetro de la instrucción preparada, llame a SQLDescribeParam para obtener información sobre los parámetros.  
  
5.  Para cada marcador de parámetro:  
  
    -   Asigne una matriz de S búferes de parámetros donde almacenar los valores de datos.  
  
    -   Asigne una matriz de S búferes de parámetros donde almacenar las longitudes de datos.  
  
    -   Llame a SQLBindParameter para enlazar el valor de datos de parámetro y las matrices de longitud de datos al parámetro de instrucción.  
  
    -   Si el parámetro es un parámetro de imagen o texto de datos en ejecución, configúrelo.  
  
    -   Si se utiliza algún parámetro de datos en ejecución, configúrelo.  
  
6.  Para cada ejecución de una instrucción preparada:  
  
    -   Coloque los S valores de datos y S longitudes de datos en las matrices de parámetros enlazadas.  
  
    -   Llame a SQLExecute para ejecutar la instrucción preparada.  
  
    -   Si se usan parámetros de entrada de datos en ejecución, SQLExecute devuelve SQL_NEED_DATA. Envíe los datos en fragmentos utilizyo SQLParamData y SQLPutData.  
  
### <a name="to-prepare-a-statement-with-row-wise-bound-parameters"></a>Para preparar una instrucción con parámetros enlazados de modo de fila  
  
1.  Asigne una matriz[S] de estructuras, donde S es el número de conjuntos de parámetros. La estructura tiene un elemento para cada parámetro y cada elemento tiene dos partes:  
  
    -   La primera parte es una variable del tipo de datos adecuado donde almacenar los datos de parámetros.  
  
    -   La segunda parte es una variable SQLINTEGER donde almacenar el indicador de estado.  
  
2.  Llame a [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) para establecer los atributos siguientes:  
  
    -   Establezca SQL_ATTR_PARAMSET_SIZE en el número de conjuntos (S) de parámetros.  
  
    -   Establezca SQL_ATTR_PARAM_BIND_TYPE en el tamaño de la estructura asignada en el paso 1.  
  
    -   Establezca el atributo SQL_ATTR_PARAMS_PROCESSED_PTR para que señale a una variable SQLUINTEGER que incluya el número de parámetros procesados.  
  
    -   Establezca SQL_ATTR_PARAMS_STATUS_PTR para que señale a una matriz[S] de variables SQLUSSMALLINT que incluya indicadores de estado de parámetros.  
  
3.  Llame a SQLPrepare para preparar la instrucción.  
  
4.  Para cada marcador de parámetro, llame a SQLBindParameter para que el valor de datos de parámetro y el puntero de longitud de datos apunten a sus variables en el primer elemento de la matriz de estructuras asignada en el paso 1. Si el parámetro es un parámetro de datos en ejecución, configúrelo.  
  
5.  Para cada ejecución de una instrucción preparada:  
  
    -   Rellene la matriz de búferes de parámetros enlazados con valores de datos.  
  
    -   Llame a SQLExecute para ejecutar la instrucción preparada. El controlador ejecuta eficazmente la instrucción SQL S veces, una vez para cada conjunto de parámetros.  
  
    -   Si se usan parámetros de entrada de datos en ejecución, SQLExecute devuelve SQL_NEED_DATA. Envíe los datos en fragmentos utilizyo SQLParamData y SQLPutData.  
  
## <a name="see-also"></a>Consulte también  
 [Temas de procedimientos de ejecución de consultas &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
