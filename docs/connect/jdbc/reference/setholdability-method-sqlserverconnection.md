---
description: Método setHoldability (SQLServerConnection)
title: Método setHoldability (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setHoldability
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 552eebd0-4c38-43f0-961f-35244f99109b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3f5b35e755449c61aa8e9b23bc05b1b71ab16318
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173414"
---
# <a name="setholdability-method-sqlserverconnection"></a>Método setHoldability (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Cambia la capacidad de alojamiento de los objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) que se han creado mediante el uso de este objeto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) según la capacidad de alojamiento determinada.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setHoldability(int nNewHold)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *nNewHold*  
  
 Un valor **int** que contiene uno de los siguientes niveles capacidad de alojamiento:  
  
 HOLD_CURSORS_OVER_COMMIT  
  
 CLOSE_CURSORS_AT_COMMIT  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setHoldability especifica este método setHoldability en la interfaz java.sql.Connection.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
