---
description: Método isBeforeFirst (SQLServerResultSet)
title: Método isBeforeFirst (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.isBeforeFirst
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e0e2bd28-6949-47dc-b9dd-145ffb337069
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7613a8f5d33f869057840f4b1f4fcfc0ea0ec767
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177491"
---
# <a name="isbeforefirst-method-sqlserverresultset"></a>Método isBeforeFirst (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si el cursor está antes de la primera fila en este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean isBeforeFirst()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si el cursor está antes de la primera fila. **false** si el cursor está en cualquier otra posición o si el conjunto de resultados no contiene filas.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método isBeforeFirst especifica este método isBeforeFirst en la interfaz java.sql.ResultSet.  
  
 Si este método se utiliza con cursores dinámicos, incluso los de solo avance y solo lectura, y la propiedad de conexión selectMethod se ha establecido para el "cursor", se producirá una excepción.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
