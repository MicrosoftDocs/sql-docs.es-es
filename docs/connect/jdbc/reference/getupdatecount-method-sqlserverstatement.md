---
description: Método getUpdateCount (SQLServerStatement)
title: Método getUpdateCount (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getUpdateCount
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: e9570228-4500-44b6-b2f1-84ac050b5112
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fbb56b829c3069318ec8f6df3c4353e80cb31860
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154859"
---
# <a name="getupdatecount-method-sqlserverstatement"></a>Método getUpdateCount (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el resultado actual como un recuento de actualizaciones.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final int getUpdateCount()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **int** que contiene el recuento de la actualización. Si el resultado que se devuelve es un objeto de conjunto de resultados o no hay más resultados, se devolverá -1.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getUpdateCount especifica este método getUpdateCount en la interfaz java.sql.Statement.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
