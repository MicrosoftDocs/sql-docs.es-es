---
description: Método getServerName (SQLServerDataSource)
title: Método getServerName (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getServerName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3004ed22-5d69-4dd0-8761-d39f0b7dde13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0e3669bfacc98989a1e21852ad53cabfee95ce0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162345"
---
# <a name="getservername-method-sqlserverdatasource"></a>Método getServerName (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve el nombre de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getServerName()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Objeto **String** que contiene el nombre del servidor o el valor NULL si no se establece ningún valor.  
  
## <a name="remarks"></a>Observaciones  
 El nombre del servidor es el nombre de host del equipo de destino que ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si no se establece la propiedad getServerName, getServerName devuelve el valor predeterminado, que es NULL.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
