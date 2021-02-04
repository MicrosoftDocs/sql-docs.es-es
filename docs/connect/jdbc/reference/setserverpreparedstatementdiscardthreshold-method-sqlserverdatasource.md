---
description: Método setServerPreparedStatementDiscardThreshold (SQLServerDataSource)
title: Método setServerPreparedStatementDiscardThreshold (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 570b857b9e589fb71d791e47af90acca1927cfc5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173102"
---
# <a name="setserverpreparedstatementdiscardthreshold-method-sqlserverdatasource"></a>Método setServerPreparedStatementDiscardThreshold (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el valor de la propiedad de conexión serverPreparedStatementDiscardThreshold. Este valor controla cuántas acciones de descarte de instrucción preparada pendientes (sp_unprepare) pueden estar pendientes por conexión antes de que se ejecute una llamada para limpiar los identificadores pendientes del servidor. Cuando el valor es <= 1, las acciones de cancelación de preparación se ejecutan inmediatamente en la instrucción preparada CLOSE. Si el valor se establece en > 1, estas llamadas se procesan por lotes para evitar la sobrecarga resultante de llamar a sp_unprepare con demasiada frecuencia.
 
## <a name="syntax"></a>Sintaxis  
  
```
public void setServerPreparedStatementDiscardThreshold(int enablePrepareOnFirstPreparedStatementCall);  
```  
  
#### <a name="parameters"></a>Parámetros  
 *serverPreparedStatementDiscardThreshold*  
  
 El nuevo valor de la propiedad de conexión **serverPreparedStatementDiscardThreshold**.  

## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
 
## <a name="remarks"></a>Observaciones  
 Este método está disponible en la versión 6.4 y posteriores del controlador JDBC.
 
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
