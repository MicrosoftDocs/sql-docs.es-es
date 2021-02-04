---
description: Clase SQLServerConnectionPoolDataSource
title: Clase SQLServerConnectionPoolDataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b00e5a90-2af7-4d04-8ef8-256183777dcf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 152dcc1f6aa2d8b463b3e859f79ade54b5620334
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172824"
---
# <a name="sqlserverconnectionpooldatasource-class"></a>Clase SQLServerConnectionPoolDataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Representa las conexiones a bases de datos físicas para los administradores de grupos de conexiones.  
  
 **Paquete:** com.microsoft.sqlserver.jdbc  
  
 **Extiende:** [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
 **Implementa:** javax.sql.ConnectionPoolDataSource  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public class SQLServerConnectionPoolDataSource  
```  
  
## <a name="remarks"></a>Observaciones  
 SQLServerConnectionPoolDataSource se utiliza normalmente en entornos de servidores de aplicación Java que admiten la agrupación de conexiones integradas y requieren ConnectionPoolDataSource para proporcionar conexiones físicas, como Java Platform, servidores de aplicación de Enterprise Edition (Java EE) que proporcionen agrupaciones de conexiones de las especificaciones de la API de JDBC 3.0.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-members.md)   
 [Referencia de API del controlador JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
