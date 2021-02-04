---
description: Método setFetchSize (SQLServerResultSet)
title: Método setFetchSize (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.setFetchSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 233bf4f8-4758-42d0-a80b-33e34fa78027
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 830949a046b70e11930e58d12882814c208c41ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173433"
---
# <a name="setfetchsize-method-sqlserverresultset"></a>Método setFetchSize (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Ofrece al controlador JDBC una sugerencia sobre el número de filas que se deberían capturar en la base de datos cuando se necesitan más filas para este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setFetchSize(int rows)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *rows*  
  
 Un valor **int** que indica el número de filas que se van a capturar.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setFetchSize especifica este método setFetchSize en la interfaz java.sql.ResultSet.  
  
 Si el tamaño de la captura es cero, el controlador JDBC omite el valor y calcula el tamaño de captura apropiado. El objeto [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) que creó el conjunto de resultados establece el valor predeterminado. El tamaño de la captura puede cambiar en cualquier momento.  
  
 Este método cambia el tamaño de la captura del bloque para los cursores de servidor y surte efecto la próxima vez que el controlador JDBC necesite llamar a sp_cursorfetch. Al establecer el tamaño de la captura en cero, se restaura el tamaño predeterminado de la misma para el tipo de cursor que se esté utilizando en esos momentos.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
