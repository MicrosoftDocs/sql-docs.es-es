---
description: Método updateAsciiStream (java.lang.String, java.io.InputStream)
title: Método updateAsciiStream (java.lang.String, java.io.InputStream)
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 747b0308-1ce6-4eba-bdfc-af29c21c18cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bd49d28586b52cf5f8e5b5b120e30f8a8b4e10a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182080"
---
# <a name="updateasciistream-method-javalangstring-javaioinputstream"></a>Método updateAsciiStream (java.lang.String, java.io.InputStream)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Actualiza la columna designada con un valor de flujo ASCII.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void updateAsciiStream(java.lang.String columnLabel,  
                              java.io.InputStream x)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnLabel*  
  
 Valor **string** que contiene la etiqueta de columna.  
  
 *x*  
  
 Un objeto InputStream.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método updateAsciiStream especifica este método updateAsciiStream en la interfaz java.sql.ResultSet.  
  
 Este método pasa caracteres ASCII (bytes) desde un objeto InputStream a las columnas de caracteres convertibles, que son el rango ASCII [0x00 - 0x7F] de Unicode y las páginas de códigos 874, 932, 936, 949, 950 y desde la 1250 a la 1258. Este método realiza una conversión en la página de intercalación de destino. Si se intenta actualizar una columna de destino no convertible se producirá una excepción. Para las columnas binarias, se pasan bytes sin formato.  
  
 El uso de este método para los tipos de datos **image**, **text** y **ntext**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] podría afectar al rendimiento.  
  
## <a name="see-also"></a>Consulte también  
 [Método updateAsciiStream &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
