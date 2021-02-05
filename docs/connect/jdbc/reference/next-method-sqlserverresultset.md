---
description: Método next (SQLServerResultSet)
title: Método next (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.next
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 60248447-6908-4036-a779-a501453cd553
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 17e4ba604834b486ae6e4d38cdea81ac9bd86606
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177003"
---
# <a name="next-method-sqlserverresultset"></a>Método next (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Mueve el cursor una fila hacia abajo desde su posición actual.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean next()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si la nueva fila actual es válida. **false** si no hay más filas para procesar.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método next especifica este método next en la interfaz java.sql.ResultSet.  
  
 Un cursor del conjunto de resultados se coloca inicialmente antes de la primera fila. La primera llamada al método next convierte a la primera fila en la fila actual, la segunda llamada convierte a la segunda fila en la fila actual y así sucesivamente.  
  
 Si está abierto un flujo de entrada para la fila actual, al llamar al método next se cerrará implícitamente. Cuando se lee una fila nueva, se borra una cadena de la advertencia para el objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
