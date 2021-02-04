---
description: Método setBytes (SQLServerCallableStatement)
title: Método setBytes (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setBytes
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f264f1a6-ee35-4eaf-81d8-ecf99f03b35d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 00b6584d3e15fdfeb606d0718eed85ff0fec9ca1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173795"
---
# <a name="setbytes-method-sqlservercallablestatement"></a>Método setBytes (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el parámetro designado en la matriz determinada de valores de tipo **byte**.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setBytes(java.lang.String sCol,  
                     byte[] b)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *sCol*  
  
 Objeto **String** que contiene el nombre del parámetro.  
  
 *b*  
  
 Una matriz de valores de **byte**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 En una versión anterior del controlador, se podía usar SQLServerCallableStatement.setBytes para convertir valores entre las matrices de bytes y los tipos de datos **date**, **time**, **datetime2** o **datetimeoffset** de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Ahora, al usar este método con esos tipos de datos, se producirá una excepción que indica que no se admite la conversión.  
  
 El método setBytes especifica este método setBytes en la interfaz java.sql.CallableStatement.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
