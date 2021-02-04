---
description: Método getShort (int) (SQLServerResultSet)
title: Método getShort (int) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.getShort (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 0b543c92-feb8-46a4-8477-9b5f94f1cdc7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: abb4ff328a15541349a094553c507b0290a6ee76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175054"
---
# <a name="getshort-method-int-sqlserverresultset"></a>Método getShort (int) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del índice de columna designado en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un valor **short** en el lenguaje de programación Java.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public short getShort(int columnIndex)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnIndex*  
  
 Valor **int** que indica el índice de la columna.  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **short**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getShort especifica este método getShort en la interfaz java.sql.ResultSet.  
  
 Este método solamente se admite en los tipos de datos [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que pueden devolver de forma segura un valor entero como smallint, tinyint y bit. Si se utiliza este método en cualquier otro tipo de datos, provocará una excepción.  
  
## <a name="see-also"></a>Consulte también  
 [Método getShort &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getshort-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
