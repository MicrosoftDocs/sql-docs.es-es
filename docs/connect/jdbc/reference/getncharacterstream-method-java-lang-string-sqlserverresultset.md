---
description: Método getNCharacterStream (java.lang.String) (SQLServerResultSet)
title: Método getNCharacterStream (java.lang.String) (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a117f3a3-9c25-41e1-9adb-a40e90620dd6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 717c7fc3435dd7e8df34a7df246f44c567ea092d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162700"
---
# <a name="getncharacterstream-method-javalangstring-sqlserverresultset"></a>Método getNCharacterStream (java.lang.String) (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor de la columna designada en la fila actual del objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) como un objeto Reader.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.io.Reader getNCharacterStream(java.lang.String columnLabel)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnLabel*  
  
 Un valor String que contiene la etiqueta de columna.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto Reader.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getNCharacterStream especifica este método getNCharacterStream en la interfaz java.sql.ResultSet.  
  
 Este método se puede usar para recuperar el valor de una columna **nvarchar**, **nchar**, **nvarchar(max)**, **ntext** o **xml** en la fila actual de este objeto [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md). Si intenta utilizar este método para recuperar valores de otros tipos de datos, se producirá una excepción.  
  
## <a name="see-also"></a>Consulte también  
 [Método getNCharacterStream &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/getncharacterstream-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)  
  
  
