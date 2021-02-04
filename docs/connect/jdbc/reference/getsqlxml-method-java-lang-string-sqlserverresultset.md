---
description: Método getSQLXML (java.lang.String) (SQLServerResultSet)
title: Método getSQLXML (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ab9c7b10-026f-4a51-8d60-e6871d1abd02
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07eea350ed9eb598705037f8b28a36e6a08f97b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174992"
---
# <a name="getsqlxml-method-javalangstring-sqlserverresultset"></a>Método getSQLXML (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor de la columna designada en la fila actual del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto SQLXML.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final java.sql.SQLXML getSQLXML(java.lang.String columnLabel)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnName*  
  
 Valor **string** que indica la etiqueta de columna.  
  
## <a name="return-value"></a>Valor devuelto  
 ASQLXMLobject.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getSQLXML especifica este método getSQLXML en la interfaz java.sql.ResultSet.  
  
## <a name="see-also"></a>Consulte también  
 [Método getSQLXML &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getsqlxml-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)  
  
  
