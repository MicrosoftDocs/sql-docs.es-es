---
description: Clase SQLServerXAConnection
title: Clase SQLServerXAConnection | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5ecb4bf1-b8d1-47cf-9cb1-7a18acc11ce2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 13d2ad39b4259d514c099e221961132336d152aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179980"
---
# <a name="sqlserverxaconnection-class"></a>Clase SQLServerXAConnection
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Representa las conexiones JDBC que pueden participar en transacciones distribuidas (XA).  
  
 **Paquete:** com.microsoft.sqlserver.jdbc  
  
 **Extiende:** [SQLServerPooledConnection](../../../connect/jdbc/reference/sqlserverpooledconnection-class.md)  
  
 **Implementa**: javax.sql.XAConnection  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public class SQLServerXAConnection  
```  
  
## <a name="remarks"></a>Observaciones  
 Un objeto SQLServerXAConnection se puede dar de alta en una transacción distribuida por medio de un objeto [SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md). Un administrador de transacciones, que normalmente forma parte de un servidor de nivel intermedio, administra un objeto SQLServerXAConnection a través del objeto SQLServerXAResource.  
  
> [!NOTE]  
>  Los programadores de la aplicación, por lo general, no utilizan directamente esta interfaz. Normalmente lo utiliza un administrador de transacciones que trabaja en el servidor del nivel intermedio.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerXAConnection](../../../connect/jdbc/reference/sqlserverxaconnection-members.md)   
 [Referencia de API del controlador JDBC](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
