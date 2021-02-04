---
description: Método getBigDecimal (java.lang.String)
title: Método getBigDecimal (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getBigDecimal (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d0b29b48-9663-4de4-9fc2-82bc30e44aed
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b8a5b274d9d4c5248908fbd25d29d61ff4167cc2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165738"
---
# <a name="getbigdecimal-method-javalangstring"></a>Método getBigDecimal (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del parámetro designado como java.math.BigDecimal con precisión completa según el nombre de parámetro.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.math.BigDecimal getBigDecimal(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *sCol*  
  
 Objeto **String** que contiene el nombre del parámetro.  
  
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
  
  
