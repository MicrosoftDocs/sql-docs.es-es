---
description: Método setBinaryStream (SQLServerBlob)
title: Método setBinaryStream (SQLServerBlob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerBlob.setBinaryStream
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: abcec31f-1a60-4765-9725-8cf7e9f1f8ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0f72c246530b4f2298af9bf846bad15fd8380d5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173858"
---
# <a name="setbinarystream-method-sqlserverblob"></a>Método setBinaryStream (SQLServerBlob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera un flujo que se puede utilizar para escribir en el valor BLOB.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.io.OutputStream setBinaryStream(long pos)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Pos*  
  
 La posición en el valor BLOB donde se comienza a escribir.  
  
## <a name="return-value"></a>Valor devuelto  
 Un flujo de salida.  
  
## <a name="exceptions"></a>Excepciones  
 java.sql.SQLException  
  
## <a name="remarks"></a>Observaciones  
 El método setBinaryStream especifica este método setBinaryStream en la interfaz java.sql.Blob.  
  
 El flujo de salida sobrescribe los datos en el objeto BLOB a partir de la posición especificada y puede exceder la longitud inicial del BLOB. Si se especifica un valor position+1, se anexarán bytes. Si se pasan valores position+2 o superiores (o cero o menos), se producirá un error de la posición.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Miembros SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Clase SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
