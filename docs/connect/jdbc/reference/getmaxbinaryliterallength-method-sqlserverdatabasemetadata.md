---
description: Método getMaxBinaryLiteralLength (SQLServerDatabaseMetaData)
title: Método getMaxBinaryLiteralLength (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getMaxBinaryLiteralLength
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 42e49ff9-8072-44e4-ad75-c864c3a4ad8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9ccb65d40257cc8960706895ce1cf4beda208a7e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175602"
---
# <a name="getmaxbinaryliterallength-method-sqlserverdatabasemetadata"></a>Método getMaxBinaryLiteralLength (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el número máximo de caracteres hexadecimales que esta base de datos permite en un literal binario insertado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public int getMaxBinaryLiteralLength()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **int** que indica la número máximo de caracteres hexadecimales.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 Este método getMaxBinaryLiteralLength se especifica mediante el método getMaxBinaryLiteralLength en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
