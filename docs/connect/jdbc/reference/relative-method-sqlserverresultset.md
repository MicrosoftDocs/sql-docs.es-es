---
description: Método relative (SQLServerResultSet)
title: Método relative (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.relative
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 2bcdbb69-95fd-4ae8-8488-1a75a91fe2e0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d61c075e123fc4ae77d852fbe2c3e221bf6ca231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176667"
---
# <a name="relative-method-sqlserverresultset"></a>Método relative (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Mueve el cursor tantas filas como se hayan indicado, con respecto a la fila actual, tanto en una posición positiva como en una negativa.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean relative(int nRows)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *nRows*  
  
 Un valor **int** que indica el número de filas que se van a mover.  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si el cursor está en una fila. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método relative especifica este método relative en la interfaz java.sql.ResultSet.  
  
 Al intentar desplazarse más allá de la primera o última fila del conjunto de resultados, el cursor se posiciona antes o después de esta primera o última fila. Llamar a `relative(0)` es válido, pero no cambia la posición del cursor.  
  
 Llamar al método `relative(1)` es igual que llamar al método [next](../../../connect/jdbc/reference/next-method-sqlserverresultset.md). Llamar al método `relative(-1)` es igual que llamar al método [previous](../../../connect/jdbc/reference/previous-method-sqlserverresultset.md).  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
