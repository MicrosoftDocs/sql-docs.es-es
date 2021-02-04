---
description: Método setWorkstationID (SQLServerDataSource)
title: Método setWorkstationID (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setWorkstationID
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: c1093615-90bf-4918-9f05-8abd765ffb03
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8b0047a866a34f68fe1f4f2be9d51714f04fba73
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172904"
---
# <a name="setworkstationid-method-sqlserverdatasource"></a>Método setWorkstationID (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve el nombre del equipo cliente que se utiliza para la conexión al origen de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setWorkstationID(java.lang.String workstationID)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *workstationID*  
  
 Un objeto **String** que contiene el nombre del equipo cliente.  
  
## <a name="remarks"></a>Observaciones  
 WorkstationID es el nombre del equipo cliente o de la estación de trabajo. Si no se establece la propiedad workstationID, el valor predeterminado se genera mediante una llamada al método InetAddress.getLocalHost().getHostName(). Si getHostName devuelve un valor en blanco, se llama al método getHostAddress().toString().  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
