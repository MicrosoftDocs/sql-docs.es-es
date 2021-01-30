---
description: Resumen de la interfaz del proveedor de servicios de ODBC
title: Resumen de la interfaz del proveedor de servicios ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ace6085b-355b-435b-8734-503fc3c12ec2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9583d75f98283b40d6e8c5d3e320d82533bf55d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174614"
---
# <a name="odbc-service-provider-interface-summary"></a>Resumen de la interfaz del proveedor de servicios de ODBC
En la tabla siguiente se describen las funciones de la interfaz del proveedor de servicios ODBC. Para obtener más información sobre la sintaxis y la semántica de cada función, vea referencia de la [interfaz del proveedor de servicios ODBC (SPI)](../../../odbc/reference/syntax/odbc-service-provider-interface-spi-reference.md).  
  
|Nombre de función|Fin|  
|-------------------|-------------|  
|[SQLSetConnectAttrForDbcInfo](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Igual que [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md), pero establece el atributo en el token de información de conexión en lugar de en el identificador de conexión.|  
|[SQLSetDriverConnectInfo](../../../odbc/reference/syntax/sqldrivertodatasource-function.md)|Establece la cadena de conexión en el token de información de conexión para la llamada [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) de una aplicación.|  
|[SQLSetConnectInfo](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Establece el origen de datos, el identificador de usuario y la contraseña en el token de información de conexión de la llamada de [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) de una aplicación.|  
|[SQLGetPoolID](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Recupera el identificador del grupo.|  
|[SQLRateConnection](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Determina si un controlador puede reutilizar una conexión existente en el grupo de conexiones.|  
|[SQLPoolConnect](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Cree una nueva conexión si no se puede volver a usar una conexión en el grupo.|  
|[SQLCleanupConnectionPoolID](../../../odbc/reference/syntax/sqldatasourcetodriver-function.md)|Informa a un controlador de que se ha agotado el tiempo de espera de un ID. de grupo.|
