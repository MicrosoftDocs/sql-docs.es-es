---
description: Método getTime (java.lang.String)
title: Método getTime (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.getTime (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ca0a3b29-30d1-4d20-bc8d-d3d9ed19ff50
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fbaf986b61066809edb89647eb0ca7ba43c1e981
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162124"
---
# <a name="gettime-method-javalangstring"></a>Método getTime (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del parámetro designado como un objeto java.sql.Time en el lenguaje de programación Java según el nombre de parámetro especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.Time getTime(java.lang.String sCol)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *sCol*  
  
 Objeto **String** que contiene el nombre del parámetro.  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto Time.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getTime especifica este método getTime en la interfaz java.sql.CallableStatement.  
  
 Consulte el gráfico titulado "Conversiones de métodos de captador" en [Descripción de las conversiones de tipos de datos](../../../connect/jdbc/understanding-data-type-conversions.md) para ver los tipos datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se pueden recuperar con este método.  
  
## <a name="see-also"></a>Consulte también  
 [Método getTime &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/gettime-method-sqlservercallablestatement.md)   
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
