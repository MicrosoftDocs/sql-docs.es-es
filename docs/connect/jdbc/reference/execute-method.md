---
description: Método execute ()
title: Método execute () | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.execute ()
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: fa96d0f8-101b-422f-a767-405be9a5f74f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a5339925a0325590594f5e62003c1dba935440d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165937"
---
# <a name="execute-method-"></a>Método execute ()
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ejecuta la instrucción SQL en este objeto [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md), que puede ser cualquier tipo de instrucción SQL.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean execute()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si la instrucción devuelve un conjunto de resultados. **false** si devuelve un recuento de actualizaciones o no devuelve ningún resultado.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método execute especifica este método execute en la interfaz java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Consulte también  
 [Método execute &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/execute-method-sqlserverpreparedstatement.md)   
 [Miembros de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Clase SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
