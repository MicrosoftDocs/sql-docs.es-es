---
description: Método getWarnings (SQLServerConnection)
title: Método getWarnings (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getWarnings
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 15af39bf-6285-44cc-a021-7341e7a055c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e39a18e93ffc9b0b9d4f0c5dbb5a517f2680c4f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154508"
---
# <a name="getwarnings-method-sqlserverconnection"></a>Método getWarnings (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera la primera advertencia que notifican las llamadas en este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.SQLWarning getWarnings()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto SQLWarning.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getWarnings especifica este método getWarnings en la interfaz java.sql.Connection.  
  
 Las advertencias posteriores se encadenan al primer método SQLWarning y se llaman con el método getNextWarning. Si se llama en una conexión cerrada, se producirá una excepción.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
