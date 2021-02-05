---
description: Método setNClob (int, java.io.Reader, long)
title: Método setNClob (int, java.io.Reader, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 11071f8f-0e9b-45f0-b600-aaef7e2815d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6923ec8ef943b65acb69e703a784d25de7a7b597
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178841"
---
# <a name="setnclob-method-int-javaioreader-long"></a>Método setNClob (int, java.io.Reader, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el parámetro designado en el objeto Reader especificado, que es el número de caracteres indicado para la longitud.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final void setNClob(int parameterIndex,  
                    java.io.Reader reader,  
                    long length)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *parameterIndex*  
  
 Un valor **int** que indica el índice del parámetro.  
  
 *reader*  
  
 Un objeto Reader que indica el valor del parámetro.  
  
 *length*  
  
 Un valor **long** que indica el número de caracteres en el valor del parámetro.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setNClob especifica este método setNClob en la interfaz java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Consulte también  
 [Método setNClob &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setnclob-method-sqlserverpreparedstatement.md)   
 [Miembros de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
