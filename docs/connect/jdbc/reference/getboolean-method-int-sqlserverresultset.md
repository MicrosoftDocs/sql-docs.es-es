---
description: Método getBoolean (int) (SQLServerResultSet)
title: Método getBoolean (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getBoolean (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 50fcc0c3-36a1-47b2-b18c-7aa2ac9b27d3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7eb830a3f5bcbca11f0e0dfb2493b9f6ea3d53a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168148"
---
# <a name="getboolean-method-int-sqlserverresultset"></a>Método getBoolean (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del índice de la columna designada en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor de tipo **boolean** en el lenguaje de programación Java.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean getBoolean(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnIndex*  
  
 Valor **int** que indica el índice de la columna.  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **boolean**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getBoolean especifica este método getBoolean en la interfaz java.sql.ResultSet.  
  
 Este método se admite únicamente en los tipos de datos numéricos y de caracteres. Convierte los valores "1", 1 y "**true**" en **true**, y los valores "0", 0 y "**false**" en **false**. Para el resto de los valores no se ha definido el comportamiento.  
  
## <a name="see-also"></a>Consulte también  
 [Método getBoolean &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getboolean-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
