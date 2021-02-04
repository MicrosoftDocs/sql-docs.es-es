---
description: Método getTrustManagerConstructorArg (SQLServerDataSource)
title: Método getTrustManagerConstructorArg (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getTrustManagerConstructorArg
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41e3a4c86a5b5ad43a425614fab09d59a9d0340e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165481"
---
# <a name="gettrustmanagerconstructorarg-method-sqlserverdatasource"></a>Método getTrustManagerConstructorArg (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve el valor de cadena de la propiedad de conexión TrustManagerConstructorArg.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getTrustManagerConstructorArg()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto **String** que contiene el valor de la propiedad de conexión TrustManagerConstructorArg, o null si no se establece ningún valor.  
  
## <a name="remarks"></a>Observaciones  
 Si no se establece la propiedad TrustManagerClass, el método [getTrustManagerConstructorArg](../../../connect/jdbc/reference/gettrustmanagerconstructorarg-method-sqlserverdatasource.md) devuelve NULL.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
