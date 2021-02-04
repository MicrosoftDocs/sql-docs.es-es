---
description: Método getArray (int) (SQLServerResultSet)
title: Método getArray (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getArray (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 377746c7-8c9c-41f5-8490-ca0dd56fd57a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 512cd23bf0422449b62f39c5baa20505057cfcd6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165852"
---
# <a name="getarray-method-int-sqlserverresultset"></a>Método getArray (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del índice de la columna que se ha designado en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto Array.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.Array getArray(int i)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *i*  
  
 Valor **int** que indica el índice de la columna.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto Array.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getArray especifica este método getArray en la interfaz java.sql.ResultSet.  
  
## <a name="see-also"></a>Consulte también  
 [Método getArray &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getarray-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
