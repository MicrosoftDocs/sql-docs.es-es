---
description: Método getSubString (SQLServerClob)
title: Método getSubString (SQLServerClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerClob.getSubString
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bf915590-a883-4403-befa-5b5bb42f34d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f08b8515dd827b2202d3e373242bb292c5060f72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162188"
---
# <a name="getsubstring-method-sqlserverclob"></a>Método getSubString (SQLServerClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve una copia de la subcadena especificada en el objeto CLOB según la posición de inicio indicada y el número de caracteres que se van a copiar.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getSubString(long pos,  
                                     int length)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *pos*  
  
 El primer carácter de la subcadena que se va a extraer. El primer carácter está en la posición 1.  
  
 *length*  
  
 El número de caracteres consecutivos que se van a copiar.  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **String** que es la subcadena especificada en el objeto CLOB.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getSubString especifica este método getSubString en la interfaz java.sql.Clob.  
  
 Si se intenta obtener un número de caracteres igual a cero a partir de un CLOB que sea NULL o de longitud cero, se obtiene una cadena vacía. Si se intentar obtener una longitud de caracteres cualquiera en alguna posición que no sea la posición 1 en un CLOB de longitud cero, se producirá una excepción relativa a la posición.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-methods.md)   
 [Miembros SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-members.md)   
 [Clase SQLServerClob](../../../connect/jdbc/reference/sqlserverclob-class.md)  
  
  
