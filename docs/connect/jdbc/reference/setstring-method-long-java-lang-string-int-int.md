---
description: Método setString (long, java.lang.String, int, int)
title: Método setString (long, java.lang.String, int, int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.setString (long, java.lang.String, int, int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9fb59b09-e825-46a6-ba5d-85d4a8dc143a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1084ba6293ec6fa8056306558ec4d463848f312e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173047"
---
# <a name="setstring-method-long-javalangstring-int-int"></a>Método setString (long, java.lang.String, int, int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Escribe la cadena especificada en el objeto CLOB comenzando en la posición establecida y según el desplazamiento y longitud indicados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public int setString(long pos,  
                     java.lang.String str,  
                     int offset,  
                     int len)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *pos*  
  
 La posición donde se comienza a escribir en el CLOB.  
  
 *str*  
  
 La cadena que se va a escribir en CLOB.  
  
 *offset*  
  
 El desplazamiento en la cadena a partir del que se van a comenzar a leer los caracteres.  
  
 *len*  
  
 El número de caracteres que se va a escribir.  
  
## <a name="return-value"></a>Valor devuelto  
 El número de caracteres que se han escrito.  
  
## <a name="exceptions"></a>Excepciones  
 java.sql.SQLException  
  
## <a name="remarks"></a>Observaciones  
 El método setString especifica este método setString en la interfaz java.sql.Clob.  
  
 Los datos de caracteres se sobrescriben tomando como punto de inicio la posición especificada y pueden sobrescribir la longitud inicial del CLOB. Si se especifica un valor position+1, se anexará la cadena. Si se especifican valores position+2 o superiores (o cero o menos), se producirá un error de la posición.  
  
## <a name="see-also"></a>Consulte también  
 [Método setString &#40;SQLServerClob&#41;](../../../connect/jdbc/reference/setstring-method-sqlserverclob.md)   
 [Métodos SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [Miembros SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Clase SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
