---
description: Desconectarse de un origen de datos
title: Desconectar de un origen de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC data sources, connections
- data sources [SQL Server Native Client]
- disconnecting [ODBC]
- ODBC applications, disconnecting
- SQLDisconnect function
- ODBC applications, data sources
- connections [SQL Server Native Client]
- SQLFreeHandle function
- ODBC data sources, disconnecting
- SQL Server Native Client ODBC driver, data sources
- ODBC functions
- SQL Server Native Client ODBC driver, connections
ms.assetid: 65b0267d-b2ab-4a59-83f2-436d90cfbf79
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd4277c773c07e56a8669d1bb5c6f9d8e024d769
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464976"
---
# <a name="disconnecting-from-a-data-source"></a>Desconectarse de un origen de datos
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cuando una aplicación ha terminado de usar un origen de datos, llama a **SQLDisconnect**. **SQLDisconnect** libera todas las instrucciones que se asignan en la conexión y desconecta el controlador del origen de datos. Después de desconectarse, la aplicación puede llamar a [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) para liberar el identificador de conexión. Antes de salir, una aplicación también llama a **SQLFreeHandle** para liberar el identificador del entorno.  
  
 Después de desconectarse, una aplicación puede reutilizar el identificador de conexión asignado, conectarse a un origen de datos diferente o volver a conectarse al mismo origen de datos. La decisión de seguir conectado en lugar de desconectarse y volver a conectarse después requiere que el sistema de escritura de la aplicación considere los costes relativos de cada opción. Conectarse a un origen de datos y seguir conectado puede ser relativamente costoso, dependiendo del medio de conexión. Para realizar un intercambio, la aplicación debe hacer también suposiciones sobre la probabilidad y el control de tiempo de operaciones adicionales en el mismo origen de datos. Puede que una aplicación tenga que utilizar además más de una conexión.  
  
## <a name="see-also"></a>Consulte también  
 [Comunicarse con SQL Server &#40;ODBC&#41;](../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
  
  
