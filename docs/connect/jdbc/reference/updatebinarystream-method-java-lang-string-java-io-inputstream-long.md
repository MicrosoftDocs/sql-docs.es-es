---
description: Método updateBinaryStream (java.lang.String, java.io.InputStream, long)
title: Método updateBinaryStream (java.io.InputStream, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: d3c0fb5d-ca05-43f7-9f38-fba6cf3705c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a7206829368b547af9d68945681cc2b7be5bfce7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158774"
---
# <a name="updatebinarystream-method-javalangstring-javaioinputstream-long"></a>Método updateBinaryStream (java.lang.String, java.io.InputStream, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Actualiza la columna designada con un valor de flujo binario, que tendrá el número especificado de bytes.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void updateBinaryStream(java.lang.String columnLabel,  
                               java.io.InputStream x,  
                               long length)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnLabel*  
  
 Valor string que contiene la etiqueta de columna.  
  
 *x*  
  
 Un objeto InputStream.  
  
 *length*  
  
 Valor **long** que indica la longitud del flujo.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 Este método updateBinaryStream se especifica mediante el método updateBinaryStream de la interfaz java.sql.ResultSet.  
  
 Este método pasa bytes desde un objeto InputStream a las columnas binarias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] seleccionadas, como binary, varbinary, varbinary(max), image, xml y udt. Este método no admite la actualización de columnas de caracteres. Para actualizar las columnas de caracteres con un elemento InputStream, use el método [updateAsciiStream](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md).  
  
 Si la longitud del flujo es distinta a la especificada en el parámetro *length*, el controlador JDBC produce una excepción cuando la fila se actualiza o se inserta.  
  
 Si se desconoce la longitud del flujo, el parámetro *length* puede establecerse en -1 para indicar que el controlador debe aceptar el flujo independientemente de su longitud. Con sqljdbc4.jar, se recomienda usar el [Método updateBinaryStream &#40;java.lang.String, java.io.InputStream&#41;](../../../connect/jdbc/reference/updatebinarystream-method-java-lang-string-java-io-inputstream.md) de JDBC 4.0 si la aplicación quiere actualizar la columna a partir de un flujo cuya longitud se desconoce.  
  
## <a name="see-also"></a>Consulte también  
 [Método updateBinaryStream &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatebinarystream-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
