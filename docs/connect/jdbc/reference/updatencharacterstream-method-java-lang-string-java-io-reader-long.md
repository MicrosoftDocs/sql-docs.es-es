---
description: Método updateNCharacterStream (java.lang.String, java.io.Reader, long)
title: Método updateNCharacterStream String - Reader - long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: db0a96a8-248f-4664-9c13-f480f309ab91
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d72c1356f075170913179ed8c2e06ea2d060e967
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193324"
---
# <a name="updatencharacterstream-method-javalangstring-javaioreader-long"></a>Método updateNCharacterStream (java.lang.String, java.io.Reader, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Actualiza la columna designada con un valor de flujo de caracteres, que tendrá el número de bytes especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void updateNCharacterStream(java.lang.String columnLabel,  
                                    java.io.Reader reader,  
                                    long length)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnLabel*  
  
 Valor **string** que contiene la etiqueta de columna.  
  
 *reader*  
  
 Un objeto Reader.  
  
 *length*  
  
 Longitud del flujo.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 Este método updateNCharacterStream se especifica mediante el método updateNCharacterStream de la interfaz java.sql.ResultSet.  
  
 Este método pasa los caracteres Unicode de un objeto Reader a columnas **nchar**, **nvarchar(max)**, **ntext** y **xml** seleccionadas. Si se utiliza este método en otras columnas de tipo de datos, se producirá una excepción.  
  
 Si la longitud del flujo es distinta a la especificada en el parámetro *length*, el controlador JDBC produce una excepción cuando la fila se actualiza o se inserta.  
  
 Si se desconoce la longitud del flujo, el parámetro *length* puede establecerse en -1 para indicar que el controlador debe aceptar el flujo independientemente de su longitud. Con sqljdbc4.jar, se recomienda usar el [Método updateNCharacterStream &#40;java.lang.String, java.io.Reader&#41;](../../../connect/jdbc/reference/updatencharacterstream-method-java-lang-string-java-io-reader.md) de JDBC 40 si la aplicación quiere actualizar la columna a partir de un flujo cuya longitud se desconoce.  
  
## <a name="see-also"></a>Consulte también  
 [Método updateNCharacterStream &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatencharacterstream-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
