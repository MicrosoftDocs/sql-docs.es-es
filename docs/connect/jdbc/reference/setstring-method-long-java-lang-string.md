---
description: Método setString (long, java.lang.String)
title: Método setString (long, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.setString (long, java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 1b2190e9-5ace-497a-8554-0e913ea9b0cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e2580de02d1cc0ba91bb36428d3309192f465c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173042"
---
# <a name="setstring-method-long-javalangstring"></a>Método setString (long, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Escribe el objeto **String** especificado en el CLOB a partir de la posición determinada.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public int setString(long pos,  
                     java.lang.String s)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *pos*  
  
 La posición donde se comienza a escribir en el CLOB.  
  
 *s*  
  
 El objeto **String** que se va a escribir en CLOB.  
  
## <a name="return-value"></a>Valor devuelto  
 El número de caracteres que se han escrito.  
  
## <a name="exceptions"></a>Excepciones  
 java.sql.SQLException  
  
## <a name="remarks"></a>Observaciones  
 El método setString especifica este método setString en la interfaz java.sql.Clob.  
  
 Los datos de caracteres se sobrescriben tomando como punto de inicio la posición especificada y pueden saturar la longitud inicial del CLOB. Si se especifica un valor position+1, se anexará la cadena. Si especifica valores de position+2 o superiores (o cero o menos), producirán un error de la posición.  
  
## <a name="see-also"></a>Consulte también  
 [Método setString &#40;SQLServerClob&#41;](../../../connect/jdbc/reference/setstring-method-sqlserverclob.md)   
 [Métodos SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [Miembros SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Clase SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
