---
description: Referencia de la interfaz del proveedor de servicios (SPI) de ODBC
title: Referencia de la interfaz del proveedor de servicios ODBC (SPI) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: cdeffb4a-f344-4abe-97f3-be2ede1c8e59
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e4cbf20ed3d158fca4e7ae58854bf53778dc9db6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174628"
---
# <a name="odbc-service-provider-interface-spi-reference"></a>Referencia de la interfaz del proveedor de servicios (SPI) de ODBC
Tradicionalmente, ODBC definía una interfaz de programación de aplicaciones (API). Las aplicaciones pueden llamar a las funciones de la API y deben implementarse dentro del administrador de controladores y del controlador.  
  
 Con la adición de la característica de agrupación de conexiones compatible con controladores, ODBC introduce la interfaz del proveedor de servicios (SPI). Las funciones del SPI se usan para la comunicación entre el administrador de controladores y el controlador. El controlador implementa las funciones SPI; el administrador de controladores no expone las funciones SPI a las aplicaciones. Las aplicaciones no deben llamar a estas funciones directamente.  
  
 Incluya sqlspi. h para el desarrollo del controlador ODBC.  
  
 Esta sección contiene los temas siguientes  
  
-   [SQLCleanupConnectionPoolID](../../../odbc/reference/syntax/sqlcleanupconnectionpoolid-function.md)  
  
-   [SQLGetPoolID](../../../odbc/reference/syntax/sqlgetpoolid-function.md)  
  
-   [SQLPoolConnect](../../../odbc/reference/syntax/sqlpoolconnect-function.md)  
  
-   [SQLRateConnection](../../../odbc/reference/syntax/sqlrateconnection-function.md)  
  
-   [SQLSetConnectAttrForDbcInfo](../../../odbc/reference/syntax/sqlsetconnectattrfordbcinfo-function.md)  
  
-   [SQLSetConnectInfo](../../../odbc/reference/syntax/sqlsetconnectinfo-function.md)  
  
-   [SQLSetDriverConnectInfo](../../../odbc/reference/syntax/installation-and-configuration-wwi-oltp.md)  
  
## <a name="see-also"></a>Consulte también  
 [Desarrollar un controlador ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Desarrollar el reconocimiento de Connection-Pool en un controlador ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)   
 [Agrupación de conexiones de administrador de controladores](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md)
