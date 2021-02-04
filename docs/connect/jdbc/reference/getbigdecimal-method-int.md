---
description: Método getBigDecimal (int)
title: Método getBigDecimal (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getBigDecimal Method (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f74030d8-3789-463b-b414-2eb01cef8a30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f357b75524a76fc67817ea8681960311ad56bd2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165757"
---
# <a name="getbigdecimal-method-int"></a>Método getBigDecimal (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del parámetro designado como java.math.BigDecimal con precisión completa según el índice de parámetro.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.math.BigDecimal getBigDecimal(int index)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *índice*  
  
 Un valor **int** que indica el índice del parámetro.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto BigDecimal.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getBigDecimal especifica este método getBigDecimal en la interfaz java.sql.CallableStatement.  
  
## <a name="see-also"></a>Consulte también  
 [Método getBigDecimal &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getbigdecimal-method-sqlservercallablestatement.md)   
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
