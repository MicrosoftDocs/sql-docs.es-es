---
description: Método getLoginTimeout (SQLServerDataSource)
title: Método getLoginTimeout (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getLoginTimeout
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 316f067c-9e08-456a-af19-b80b0bbd4a5c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0748b46e99113e80266379361ed403815621feec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162892"
---
# <a name="getlogintimeout-method-sqlserverdatasource"></a>Método getLoginTimeout (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve el número de segundos que este objeto [SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md) esperará mientras intenta realizar una conexión.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public int getLoginTimeout()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **int** que representa el número de segundos durante los que se esperará.  
  
## <a name="remarks"></a>Observaciones  
 Si la aplicación no especifica un valor de tiempo de forma explícita, este método devuelve un valor predeterminado de 15 segundos.  
  
 El método getLoginTimeout especifica este método getLoginTimeout en la interfaz javax.sql.DataSource.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
