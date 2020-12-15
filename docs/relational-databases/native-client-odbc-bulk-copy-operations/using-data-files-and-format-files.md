---
description: Utilizar archivos de datos y archivos de formato
title: Usar archivos de datos y archivos de formato | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], file formats
- ODBC, functions
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [SQL Server Native Client]
- ODBC, bulk copy operations
- bulk copy [ODBC], data files
ms.assetid: c01b7155-3f0a-473d-90b7-87a97bc56ca5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5e307020d2ec76457d8f813a076f3f41b0207907
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465016"
---
# <a name="using-data-files-and-format-files"></a>Utilizar archivos de datos y archivos de formato
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  El programa de copia masiva más simple hace lo siguiente:  
  
1.  Llama a [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) para especificar la copia masiva de salida (establecer BCP_OUT) de una tabla o vista en un archivo de datos.  
  
2.  Llama a [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md) para ejecutar la operación de copia masiva.  
  
 El archivo de datos se crea en modo nativo; por tanto, los datos de todas las columnas de la tabla o la vista se almacenan en el archivo de datos con el mismo formato que en la base de datos. A continuación, se puede realizar una copia masiva del archivo en un servidor mediante estos mismos pasos y estableciendo DB_IN en lugar de DB_OUT. Esto solamente funciona si las tablas de destino y origen tienen exactamente la misma estructura. El archivo de datos resultante también puede ser una entrada a la utilidad **BCP** mediante el modificador **/n** (modo nativo).  
  
 Para realizar la copia masiva del conjunto de resultados de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] en lugar de directamente de una tabla o vista:  
  
1.  Llame a **bcp_init** para especificar las copias masivas, pero especifique NULL para el nombre de la tabla.  
  
2.  Llame a [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) con *EOPTION* establecido en BCPHINTS y *iValue* establecido en un puntero a una cadena SQLTCHAR que contiene la instrucción Transact-SQL.  
  
3.  Llame a **bcp_exec** para ejecutar la operación de copia masiva.  

 La instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] puede ser cualquier instrucción que genere un conjunto de resultados. Se crea el archivo de datos que contiene el primer conjunto de resultados de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)]. La copia masiva omite cualquier conjunto de resultados tras la primera si la instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] genera varios conjuntos de resultados.  
  
 Para crear un archivo de datos en el que los datos de columna se almacenen en un formato diferente que en la tabla, llame a [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) para especificar el número de columnas que se van a cambiar y, a continuación, llame a [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md) para cada columna cuyo formato desee cambiar. Esto se hace después de llamar a **bcp_init** pero antes de llamar a **bcp_exec**. **bcp_colfmt** especifica el formato en el que se almacenan los datos de la columna en el archivo de datos. Se puede usar cuando se realiza una copia masiva de dentro o fuera. También puede usar **bcp_colfmt** para establecer los terminadores de fila y de columna. Por ejemplo, si los datos no contienen caracteres de tabulación, puede crear un archivo delimitado por tabuladores mediante **bcp_colfmt** para establecer el carácter de tabulación como el terminador de cada columna.  
  
 Cuando se realiza una copia masiva y se usa **bcp_colfmt**, puede crear fácilmente un archivo de formato que describa el archivo de datos que ha creado llamando a [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md) después de la última llamada a **bcp_colfmt**.  
  
 Cuando se realiza una copia masiva desde un archivo de datos descrito por un archivo de formato, se lee el archivo de formato mediante una llamada a [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md) después de **bcp_init** pero antes de **bcp_exec**.  
  
 La función **bcp_control** controla varias opciones cuando se realiza una copia masiva [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde un archivo de datos. **bcp_control** establece opciones, como el número máximo de errores antes de la finalización, la fila del archivo en el que se va a iniciar la copia masiva, la fila en la que se va a detener y el tamaño del lote.  
  
## <a name="see-also"></a>Consulte también  
 [Realizar operaciones de copia masiva &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  
