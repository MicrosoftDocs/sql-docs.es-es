---
description: Método setBytes (long, byte)
title: Método setBytes (long, byte) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.setBytes (long.byte[])
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ffb8f107-0f9d-4410-957f-62b718e1e872
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2329bb9598c56ecabb7282faaf6b7b7f5565b068
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173765"
---
# <a name="setbytes-method-long-byte"></a>Método setBytes (long, byte)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Escribe la matriz de bytes determinada en el BLOB comenzando en la posición determinada y posteriormente devuelve el número de bytes escritos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public int setBytes(long pos,  
                    byte[] bytes)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *pos*  
  
 La posición (de base 1) en el objeto BLOB en la que se comienzan a escribir los datos.  
  
 *bytes*  
  
 La matriz de bytes que se va a escribir en el BLOB.  
  
## <a name="return-value"></a>Valor devuelto  
 Un valor **long** que especifica el número de bytes que se han escrito.  
  
## <a name="exceptions"></a>Excepciones  
 java.sql.SQLException  
  
## <a name="remarks"></a>Observaciones  
 El método setBytes especifica este método setBytes en la interfaz java.sql.Blob.  
  
 Los datos se sobrescriben tomando como punto de inicio la posición especificada y pueden saturar la longitud inicial del BLOB. Si se especifican valores position+1, se anexarán bytes. Si se pasan valores position+2 o superiores (o cero o menos), se producirá un error de la posición. Si se pasa una matriz **byte** de longitud cero, se devolverá cero porque no se escribieron bytes.  
  
## <a name="see-also"></a>Consulte también  
 [Método setBytes &#40;SQLServerBlob&#41;](../../../connect/jdbc/reference/setbytes-method-sqlserverblob.md)   
 [Métodos SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Miembros SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Clase SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
