---
description: Método createStatement (int, int, int)
title: Método createStatement (int, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.createStatement (int, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2e4fa385-8f61-4394-8f75-3e839930a57d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62505633804ceea3e8f5e4e65143a8ff6ee61990
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165996"
---
# <a name="createstatement-method-int-int-int"></a>Método createStatement (int, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Crea un objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) que genera objetos [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) con el tipo, simultaneidad y capacidad de alojamiento especificados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.Statement createStatement(int nType,  
                                          int nConcur,  
                                          int nHold)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *resultSetType*  
  
 El valor **int** que representa el tipo del conjunto de resultados.  
  
 *nConcur*  
  
 El valor **int** que representa el tipo de simultaneidad del conjunto de resultados.  
  
 *nHold*  
  
 El valor **int** que representa la capacidad de alojamiento.  
  
## <a name="return-value"></a>Valor devuelto  
 El objeto Statement.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método createStatement method especifica este método createStatement en la interfaz java.sql.Connection.  
  
## <a name="see-also"></a>Consulte también  
 [Método createStatement &#40;SQLServerConnection&#41;](../../../connect/jdbc/reference/createstatement-method-sqlserverconnection.md)   
 [Miembros SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
