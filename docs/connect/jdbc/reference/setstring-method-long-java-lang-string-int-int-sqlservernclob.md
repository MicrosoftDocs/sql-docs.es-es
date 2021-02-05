---
description: Método setString (long, java.lang.String, int, int) (SQLServerNClob)
title: Método setString (long, java.lang.String, int, int) - NClob | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 2d5e9f50-15b2-4c76-8bfc-3b5be49c2781
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0ac85bfb0742c298c215d95d0092dfe032a26139
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178602"
---
# <a name="setstring-method-long-javalangstring-int-int-sqlservernclob"></a>Método setString (long, java.lang.String, int, int) (SQLServerNClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Escribe la cadena especificada en NClob comenzando en la posición establecida según el desplazamiento y longitud indicados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
int setString(long pos,  
              java.lang.String str,  
              int offset,  
              int len)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *pos*  
  
 La posición en la que se comienza a escribir en **NCLOB**; la primera posición es 1.  
  
 *str*  
  
 La cadena que se va a escribir en **NCLOB**.  
  
 *offset*  
  
 El desplazamiento en *str* para empezar a leer los caracteres que se van a escribir.  
  
 *len*  
  
 El número de caracteres que se va a escribir.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setString especifica este método setString en la interfaz java.sql.NClob.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-methods.md)   
 [Miembros SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-members.md)   
 [Clase SQLServerNClob](../../../connect/jdbc/reference/sqlservernclob-class.md)  
  
  
