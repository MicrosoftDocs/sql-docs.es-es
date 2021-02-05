---
description: Clase SQLServerXADataSource
title: Clase SQLServerXADataSource | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 95fc7b07-2498-4a7e-8f7f-ee0d86b598b4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 61d86781a9f730c78a6e2d78da0d5152d3f84e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180353"
---
# <a name="sqlserverxadatasource-class"></a>Clase SQLServerXADataSource
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Representa un generador para objetos [SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-class.md) que se usa internamente.  
  
 **Paquete:** com.microsoft.sqlserver.jdbc  
  
 **Extiende:** [SQLServerConnectionPoolDataSource](../../../connect/jdbc/reference/sqlserverconnectionpooldatasource-class.md)  
  
 **Implementa:** javax.sql.XADataSource  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public class SQLServerXADataSource  
```  
  
## <a name="remarks"></a>Observaciones  
 Un objeto que implementa la interfaz SQLServerXADataSource se registra normalmente con un servicio de asignación de nombres que utiliza la interfaz Java Naming and Directory Interface (JNDI).  
  
 La clase SQLServerXADataSource proporciona las conexiones a bases de datos para su utilización en transacciones distribuidas (XA). La clase SQLServerXADataSource también admite la agrupación de conexiones físicas. Las interfaces SQLServerXADataSource y SQLServerXAConnection, que se definen en el paquete javax.sql, se implementan mediante [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Un objeto SQLServerXAConnection es una conexión agrupada que puede participar en una transacción distribuida. Más concretamente, SQLServerXAConnection extiende la interfaz SQLServerPooledConnection agregando el método [getXAResource](../../../connect/jdbc/reference/getxaresource-method-sqlserverxaconnection.md). Este método genera un objeto [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md) que puede utilizar un administrador de transacciones para coordinar el trabajo realizado en esta conexión con los otros participantes en la transacción distribuida. Dado que extienden la interfaz SQLServerPooledConnection, los objetos SQLServerXAConnection admiten todos los métodos de objetos SQLServerPooledConnection. Se trata de conexiones físicas reutilizables a un origen de datos subyacente y generan controladores de conexión lógicos que se pueden devolver a una aplicación JDBC.  
  
 Los objetos SQLServerXAConnection se generan mediante un objeto SQLServerXADataSource. Los objetos SQLServerConnectionPoolDataSource y SQLServerXADataSource son similares porque ambos se implementan bajo un nivel del origen de datos que está visible para la aplicación JDBC. Esta arquitectura permite que [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admita transacciones distribuidas de manera transparente a la aplicación. SQLServerXADataSource se puede configurar para que se integre con el Coordinador de transacciones distribuidas (DTC) de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] para proporcionar un procesamiento verdadero de las transacciones distribuidas.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros de clase SQLServerXADataSource](../../../connect/jdbc/reference/sqlserverxadatasource-members.md)   
 [Referencia de API del controlador JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
