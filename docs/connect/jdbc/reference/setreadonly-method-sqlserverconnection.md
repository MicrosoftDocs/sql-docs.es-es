---
description: Método setReadOnly (SQLServerConnection)
title: Método setReadOnly (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.setReadOnly
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bd11fd50-f092-43a0-a6bc-c63e70cff8da
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1150dff13ab8f7f94c8652a24cacb108aace640c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173177"
---
# <a name="setreadonly-method-sqlserverconnection"></a>Método setReadOnly (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece este objeto [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) en modo de solo lectura como sugerencia para que el controlador JDBC pueda habilitar las optimizaciones de la base de datos.  
  
> [!NOTE]  
>  El [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] no admite este método.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setReadOnly(boolean readOnly)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *readOnly*  
  
 **true** si la conexión va a ser de solo lectura. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setReadOnly especifica este método setReadOnly en la interfaz java.sql.Connection.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
