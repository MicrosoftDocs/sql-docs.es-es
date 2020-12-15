---
title: Administrar tamaños de lote de copia masiva | Microsoft Docs
description: Obtenga información sobre cómo el tamaño de lote para una copia masiva define el ámbito de una transacción, lo que afecta al comportamiento de errores y a la sobrecarga de bloqueo en SQL Server Native Client ODBC.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- ODBC, bulk copy operations
- batches [ODBC]
- bulk copy [ODBC], batch sizes
ms.assetid: 4b24139f-788b-45a6-86dc-ae835435d737
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f9be8ff6adc10b74ae176011dd1ae0f3a2b9cae1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465026"
---
# <a name="managing-bulk-copy-batch-sizes"></a>Administrar tamaños de lote de copia masiva
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  El propósito principal de un lote en las operaciones de copia masiva consiste en definir el ámbito de una transacción. Si no se establece un tamaño de lote, las funciones de copia masiva consideran una copia masiva completa como una transacción. Si se establece un tamaño de lote, cada lote constituye una transacción que confirma cuando finaliza el lote.  
  
 Si una copia masiva se realiza sin especificar ningún tamaño de lote y se produce un error, se revierte la copia masiva completa. La recuperación de una copia masiva de ejecución prolongada puede tardar mucho tiempo. Cuando se establece un tamaño de lote, la copia masiva considera cada lote como una transacción y confirma cada lote. Si se produce un error, solo es necesario revertir el último lote pendiente.  
  
 El tamaño de lote también puede afectar a la sobrecarga de bloqueo. Al realizar una copia masiva en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se puede especificar la sugerencia TABLOCK mediante [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) para adquirir un bloqueo de tabla en lugar de bloqueos de fila. El bloqueo de una única tabla se puede mantener con una sobrecarga mínima en una operación de copia masiva completa. Si no se especifica TABLOCK, los bloqueos se mantienen en las filas individuales y la sobrecarga de mantener todos los bloqueos durante la copia masiva puede reducir el rendimiento. Dado que los bloqueos solo se mantienen mientras dura una transacción, la especificación de un tamaño del lote resuelve este problema ya que se genera periódicamente una confirmación que libera los bloqueos actuales.  
  
 El número de filas que conforman un lote puede tener efectos significativos en el rendimiento cuando se realiza la copia masiva de un gran número de filas. Las recomendaciones para el tamaño del lote dependen del tipo de copia masiva que se realiza.  
  
-   Cuando realice una copia masiva en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], especifique la sugerencia TABLOCK de copia masiva y establezca un tamaño de lote grande.  
  
-   Si no especifica TABLOCK, limite los tamaños de lote a un número menor que 1.000 filas.  
  
 Cuando se realiza una copia masiva desde un archivo de datos, el tamaño del lote se especifica mediante una llamada a **bcp_control** con la opción BCPBATCH antes de llamar a [bcp_exec](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md). Cuando se realiza una copia masiva desde variables de programa mediante [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) y [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md), el tamaño del lote se controla mediante una llamada a [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md) después de llamar a [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) *x* veces, donde *x* es el número de filas de un lote.  
  
 Además de especificar el tamaño de una transacción, los lotes también afectan al envío de las filas al servidor a través de la red. Las funciones de copia masiva suelen almacenar en caché las filas de **bcp_sendrow** hasta que se rellene un paquete de red y, a continuación, enviar el paquete completo al servidor. Sin embargo, cuando una aplicación llama a **bcp_batch**, el paquete actual se envía al servidor independientemente de si se ha rellenado. La utilización de un tamaño de lote muy bajo puede reducir el rendimiento si da lugar al envío de numerosos paquetes parcialmente rellenados al servidor. Por ejemplo, la llamada a **bcp_batch** después de cada **bcp_sendrow** hace que cada fila se envíe en un paquete independiente y, a menos que las filas sean muy grandes, desperdicia espacio en cada paquete. El tamaño predeterminado de los paquetes de red para SQL Server es 4 KB, aunque una aplicación puede cambiar el tamaño mediante una llamada a [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) que especifique el atributo de SQL_ATTR_PACKET_SIZE.  
  
 Otro efecto secundario de los lotes es que cada lote se considera un conjunto de resultados pendiente hasta que se completa con **bcp_batch**. Si se intenta realizar cualquier otra operación en un identificador de conexión mientras un lote está pendiente, el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controlador ODBC de Native Client emite un error con SQLSTATE = "HY000" y una cadena de mensaje de error de:  
  
```  
"[Microsoft][SQL Server Native Client] Connection is busy with  
results for another hstmt."  
```  
  
## <a name="see-also"></a>Consulte también  
 [Realizar operaciones de copia masiva &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)   
 [Importar y exportar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  
