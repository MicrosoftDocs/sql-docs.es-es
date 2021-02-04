---
description: Método getNClob (java.lang.String)
title: Método getNClob (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: be01ce56-8f13-437b-8de6-246cda5f7830
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41dc7c3a59b7221e62b241d917a6839234122b79
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175379"
---
# <a name="getnclob-method-javalangstring"></a>Método getNClob (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el valor del parámetro JDBC **NCLOB** como un objeto NClob en el lenguaje de programación Java.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.NClob getNClob(java.lang.String parameterName)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *parameterName*  
  
 Objeto **String** que contiene el nombre del parámetro.  
  
## <a name="return-value"></a>Valor devuelto  
 ANClobobject.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getNClob especifica este método getNClob en la interfaz java.sql.CallableStatement.  
  
 Este método solo admite la recuperación de los parámetros **NCHAR**, **NVARCHAR**, **NTEXT** y **XML**. Si estos métodos se llaman en otros parámetros del tipo de datos, se producirá una excepción.  
  
## <a name="see-also"></a>Consulte también  
 [Método getNClob &#40;SQLServerCallableStatement&#41;](../../../connect/jdbc/reference/getnclob-method-sqlservercallablestatement.md)   
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)  
  
  
