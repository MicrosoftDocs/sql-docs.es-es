---
description: Método rowUpdated (SQLServerResultSet)
title: Método rowUpdated (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.rowUpdated
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 29303550-294e-4d43-b892-312b42e21271
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c09a12aa4a9ceabf1f4b5d26fe017b709368acec
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174155"
---
# <a name="rowupdated-method-sqlserverresultset"></a>Método rowUpdated (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si se ha actualizado la fila actual.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean rowUpdated()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si el propietario u otro usuario han actualizado la fila visiblemente y si se detectan actualizaciones. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método rowUpdated especifica este método rowUpdated en la interfaz java.sql.ResultSet.  
  
 El valor que se devuelve depende de si el conjunto de resultados puede detectar las actualizaciones o no.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no detecta filas actualizadas para ningún tipo de cursor.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
