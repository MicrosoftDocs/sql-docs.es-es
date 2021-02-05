---
description: Método updateNClob (java.lang.String, java.sql.NClob)
title: Método updateNClob (java.lang.String, java.sql.NClob) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a025d124-3634-49fa-8bb5-e9b98f2d5de3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1dbcf32c3736cf3656924533906b385ec360a647
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194955"
---
# <a name="updatenclob-method-javalangstring-javasqlnclob"></a>Método updateNClob (java.lang.String, java.sql.NClob)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Actualiza la columna designada con un valor **NClob**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void updateNClob(java.lang.String columnLabel,  
                        java.sql.NClob x)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *columnLabel*  
  
 Valor **string** que indica la etiqueta de columna.  
  
 *x*  
  
 Un objeto NClob.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método updateNClob especifica este método updateNClob en la interfaz java.sql.ResultSet.  
  
 Este método solo se admite en columnas **nvarchar (max)**, **ntext** y **xml**. Si se utiliza este método en cualquier otro tipo de datos, provocará una excepción.  
  
## <a name="see-also"></a>Consulte también  
 [Método updateNClob &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updatenclob-method-sqlserverresultset.md)   
 [Miembros SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [Clase SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
