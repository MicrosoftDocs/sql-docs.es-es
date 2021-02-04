---
description: Método setServerName (SQLServerDataSource)
title: Método setServerName (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setServerName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 70920828-eda0-4064-be9f-c1e460db8f00
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 086b100bb084fa2c5926f0b7cdade412b19a9bdd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173159"
---
# <a name="setservername-method-sqlserverdatasource"></a>Método setServerName (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el nombre del equipo que ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setServerName(java.lang.String serverName)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *serverName*  
  
 Objeto **String** que contiene el nombre del servidor.  
  
## <a name="remarks"></a>Observaciones  
 El nombre del servidor es el nombre de host del equipo de destino que ejecuta [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si no se establece la propiedad serverName, [getServerName](../../../connect/jdbc/reference/getservername-method-sqlserverdatasource.md) devuelve el valor predeterminado, que es NULL.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
