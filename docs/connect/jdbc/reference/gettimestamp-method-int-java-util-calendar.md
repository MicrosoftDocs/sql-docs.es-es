---
description: Método getTimestamp (int, java.util.Calendar)
title: Método getTimestamp (int, java.util.Calendar) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getTimestamp (int, java.util.Calendar)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 161c559a-8651-44ba-a914-15eb6a612417
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fee2b35c221d32bd1c99cc172e8c6629c8ef61e8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174827"
---
# <a name="gettimestamp-method-int-javautilcalendar"></a>Método getTimestamp (int, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del parámetro designado como un objeto java.sql.Timestamp en el lenguaje de programación Java según el índice de parámetro y usando un objeto Calendar.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.Timestamp getTimestamp(int index,  
                                       java.util.Calendar cal)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *índice*  
  
 Un valor **int** que indica el índice del parámetro.  
  
 *cal*  
  
 Un objeto Calendar.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto Timestamp.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getTimestamp especifica este método getTimestamp en la interfaz java.sql.CallableStatement.  
  
 Este método solamente devuelve valores de columnas [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **datetime** y **smalldatetime**.  
  
## <a name="see-also"></a>Consulte también  
 [Método getTimestamp &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/gettimestamp-method-sqlservercallablestatement.md)   
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
