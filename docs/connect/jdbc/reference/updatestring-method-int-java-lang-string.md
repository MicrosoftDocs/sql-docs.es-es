---
description: Método updateString (int, java.lang.String)
title: Método updateString (int, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateString (int, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f8d2f620-0cdf-4a3b-8af4-5e8c4462a42d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a0e34e8627f9af14a25536b6bbedaf1b6276ab78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171941"
---
# <a name="updatestring-method-int-javalangstring"></a>Método updateString (int, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Actualiza la columna designada con un valor **string** según el índice de columna.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void updateString(int index,  
                         java.lang.String x)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *índice*  
  
 Valor **int** que indica el índice de la columna.  
  
 *x*  
  
 Un objeto **String**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método updateString especifica este método updateString en la interfaz java.sql.ResultSet.  
  
## <a name="see-also"></a>Consulte también  
 [updateString (método) &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatestring-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
