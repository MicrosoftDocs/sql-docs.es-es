---
description: Método getBinaryStream (long, long)
title: Método getBinaryStream (long, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 30bc8882-04b4-4efd-95e4-7d3a2a8c1d47
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: da3fcf0ab4e97c19458981feea8f7e2af4c5a133
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168227"
---
# <a name="getbinarystream-method-long-long"></a>Método getBinaryStream (long, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve un objeto del flujo de entrada que contiene un valor BLOB parcial, para ello utiliza la posición de inicio y la longitud especificadas.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.io.InputStream getBinaryStream(long pos, long length)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *pos*  
  
 El desplazamiento para el primer byte del valor parcial que se va a recuperar.  
  
 *length*  
  
 La longitud en bytes del valor parcial que se va a recuperar.  
  
## <a name="return-value"></a>Valor devuelto  
 Un flujo de entrada que contiene los datos BLOB.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getBinaryStream especifica este método getBinaryStream en la interfaz java.sql.Blob.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-methods.md)   
 [Miembros SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-members.md)   
 [Clase SQLServerBlob](../../../connect/jdbc/reference/sqlserverblob-class.md)  
  
  
