---
description: getAsciiStream (int)
title: getAsciiStream (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getAsciiStream(int paramIndex)
apilocation:
- SQLServerCallableStatement.getAsciiStream(int paramIndex)
apitype: Assembly
ms.assetid: 9d8b235e-4208-40ee-b5a5-bc76f73b82f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e0df4dac18ff61ea230a3729cabf1f6884e943d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165807"
---
# <a name="getasciistream-int"></a>getAsciiStream (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del parámetro designado como un flujo de caracteres **ASCII** según el índice del parámetro.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final java.io.InputStream getAsciiStream(int paramIndex)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *paramIndex*  
  
 Un valor **int** que indica el índice del parámetro.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto InputStream.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="see-also"></a>Consulte también  
 [Método getAsciiStream &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getasciistream-method-sqlservercallablestatement.md)   
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
