---
description: Método getServerPreparedStatementDiscardThreshold (SQLServerConnection)
title: Método getServerPreparedStatementDiscardThreshold (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.getServerPreparedStatementDiscardThreshold
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ce985283a033c7942b2bca1cb7475a5b1813bcd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175031"
---
# <a name="getserverpreparedstatementdiscardthreshold-method-sqlserverconnection"></a>Método getServerPreparedStatementDiscardThreshold (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

 Devuelve el valor de la propiedad de conexión **serverPreparedStatementDiscardThreshold**. Este valor controla cuántas acciones de descarte de instrucción preparada pendientes (sp_unprepare) pueden estar pendientes por conexión antes de que se ejecute una llamada para limpiar los identificadores pendientes del servidor. Si el valor es <= 1, las acciones de cancelación de preparación se ejecutan inmediatamente en la instrucción preparada CLOSE. Si se establece en > 1, estas llamadas se procesan por lotes para evitar la sobrecarga resultante de llamar a sp_unprepare con demasiada frecuencia. El valor predeterminado de esta opción se puede cambiar llamando a getDefaultServerPreparedStatementDiscardThreshold().

## <a name="syntax"></a>Sintaxis  
  
```  
  
public int getServerPreparedStatementDiscardThreshold()  
```  

## <a name="return-value"></a>Valor devuelto
 Un valor **int** que contiene el valor de la propiedad de conexión **serverPreparedStatementDiscardThreshold**.

## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Observaciones  
 Este método está disponible en la versión 6.4 y posteriores del controlador JDBC.
 
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
