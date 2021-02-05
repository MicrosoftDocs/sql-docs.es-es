---
description: Método updateBytes (SQLServerResultSet)
title: Método updateBytes (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.updateBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3050c836-fbb3-4475-99e5-05637a48a932
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 28414c4d58a5a1b2ef0cada57457de960722e8d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188300"
---
# <a name="updatebytes-method-sqlserverresultset"></a>Método updateBytes (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Actualiza la columna designada con una matriz de valores **byte**.  
  
## <a name="overload-list"></a>Lista de sobrecargas  
  
|NOMBRE|Descripción|  
|----------|-----------------|  
|[updateBytes (int, byte&#91;&#93;)](../../../connect/jdbc/reference/updatebytes-method-int-byte.md)|Actualiza la columna designada con una matriz de valores **byte** según el índice de columna.|  
|[updateBytes (java.lang.String, byte&#91;&#93;)](../../../connect/jdbc/reference/updatebytes-method-java-lang-string-byte.md)|Actualiza la columna designada con una matriz de valores **byte** según el nombre de columna.|  
  
## <a name="remarks"></a>Observaciones  
 En una versión anterior de [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)], se podía usar SQLServerResultSet.updateBytes para convertir valores entre matrices de bytes y tipos de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]**date**, **time**, **datetime2** o **datetimeoffset**. Ahora, al usar este método con esos tipos de datos, se producirá una excepción que indica que no se admite la conversión.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
