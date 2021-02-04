---
description: Método execute (java.lang.String) (SQLServerStatement)
title: Método execute (java.lang.String) (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.execute (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 64ac78b8-d5b3-4134-9b72-d2b0c52168a2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7f01611e334b44cecbbf8fa93dcf24ab281c96ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163424"
---
# <a name="execute-method-javalangstring-sqlserverstatement"></a>Método execute (java.lang.String) (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ejecuta la instrucción SQL determinada, que puede devolver varios resultados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean execute(java.lang.String sql)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *sql*  
  
 Un valor **String** que contiene una instrucción SQL.  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si el primer resultado es un conjunto de resultados. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método execute especifica este método execute en la interfaz java.sql.Statement.  
  
## <a name="see-also"></a>Consulte también  
 [Método execute &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md)   
 [Miembros SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
