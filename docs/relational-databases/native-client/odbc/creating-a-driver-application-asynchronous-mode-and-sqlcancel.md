---
description: 'Crear una aplicación de controlador: modo asincrónico y SQLCancel'
title: Modo asincrónico y SQLCancel | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- asynchronous operations [SQL Server Native Client]
- SQLCancel function
- SQLSetConnectAttr function
- SQL Server Native Client, asynchronous operations
- ODBC functions
- ODBC applications, asynchronous operations
- SQL Server Native Client ODBC driver, asynchronous mode
ms.assetid: f31702a2-df76-4589-ac3b-da5412c03dc2
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4b2e0d637dcd3e956afcbe3867c02464453ece2a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463336"
---
# <a name="creating-a-driver-application---asynchronous-mode-and-sqlcancel"></a>Crear una aplicación de controlador: modo asincrónico y SQLCancel
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Algunas funciones ODBC pueden operar de forma sincrónica o asincrónica. La aplicación puede habilitar las operaciones asincrónicas para un identificador de instrucción o un identificador de conexión. Si la opción está establecida para un identificador de conexión, afectará a todos los identificadores de instrucción del identificador de conexión. La aplicación utiliza las instrucciones siguientes para habilitar o deshabilitar las operaciones asincrónicas:  
  
```  
SQLSetConnectAttr(hdbc, SQL_ATTR_ASYNC_ENABLE,  
                        SQL_ASYNC_ENABLE_ON, SQL_IS_INTEGER);  
SQLSetConnectAttr(hdbc, SQL_ATTR_ASYNC_ENABLE,  
                        SQL_ASYNC_ENABLE_OFF, SQL_IS_INTEGER);  
SQLSetStmtAttr(hstmt, SQL_ATTR_ASYNC_ENABLE,  
                        SQL_ASYNC_ENABLE_ON, SQL_IS_INTEGER);  
SQLSetStmtAttr(hstmt, SQL_ATTR_ASYNC_ENABLE,  
                        SQL_ASYNC_ENABLE_OFF, SQL_IS_INTEGER);  
```  
  
 Cuando una aplicación llama a una función ODBC en modo sincrónico, el controlador no devuelve el control a la aplicación hasta que se notifique que el servidor ha completado el comando.  
  
 Cuando funciona en modo asincrónico, el controlador devuelve inmediatamente el control a la aplicación, incluso antes de enviar el comando al servidor. El controlador establece el código de retorno en SQL_STILL_EXECUTING. A continuación, la aplicación realiza las demás tareas.  
  
 Cuando la aplicación comprueba que el comando ha finalizado, realiza la misma llamada de función con los mismos parámetros al controlador. Si el controlador aún no ha recibido una respuesta del servidor, devolverá de nuevo SQL_STILL_EXECUTING. La aplicación debe comprobar periódicamente el comando hasta que el código de retorno sea distinto de SQL_STILL_EXECUTING. Cuando la aplicación obtiene otro código de retorno, incluso SQL_ERROR, puede determinar que el comando ha finalizado.  
  
 A veces un comando tarda mucho tiempo en completarse. Si la aplicación necesita cancelar el comando sin esperar una respuesta, puede hacerlo mediante una llamada a **SQLCancel** con el mismo identificador de instrucción que el comando pendiente. Esta es la única vez que se debe usar **SQLCancel** . Algunos programadores usan **SQLCancel** cuando se han procesado de forma parcial a través de un conjunto de resultados y desean cancelar el resto del conjunto de resultados. Se debe usar [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md) o [SQLCloseCursor](../../../relational-databases/native-client-odbc-api/sqlclosecursor.md) para cancelar el resto de un conjunto de resultados pendiente, no **SQLCancel**.  
  
## <a name="see-also"></a>Consulte también  
 [Crear una aplicación de controlador ODBC de SQL Server Native Client](../../../relational-databases/native-client/odbc/creating-a-driver-application.md)  
  
  
