---
description: Método rowDeleted (SQLServerResultSet)
title: Método rowDeleted (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.rowDeleted
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9c6db315-e614-4604-b020-41af6a214cc1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1b3557c1df6b128b69671a3fa6846944e466a28e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174137"
---
# <a name="rowdeleted-method-sqlserverresultset"></a>Método rowDeleted (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si se ha eliminado una fila.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean rowDeleted()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Es **true** si se eliminó una fila y se detectaron eliminaciones. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método rowDeleted especifica este método rowDeleted en la interfaz java.sql.ResultSet.  
  
 Si se elimina una fila, podría quedar un hueco visible en un conjunto de resultados. Este método se puede utilizar para detectar los huecos en un conjunto de resultados. El valor que se devuelve depende de si este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) puede detectar eliminaciones.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] detecta las filas eliminadas para todos los tipos de cursor actualizables, aunque la detección sea transitoria para los cursores de avance y dinámicos.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
