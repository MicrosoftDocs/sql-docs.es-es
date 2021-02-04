---
description: Método releaseSavepoint (SQLServerConnection)
title: Método releaseSavepoint (SQLServerConnection) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerConnection.releaseSavepoint
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: b6b625ea-c7ce-4a32-a9e0-6d2b4321bfd8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43a159feee134937fa14889fde196b2115e59494
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174171"
---
# <a name="releasesavepoint-method-sqlserverconnection"></a>Método releaseSavepoint (SQLServerConnection)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Quita el objeto [SQLServerSavepoint](../../../connect/jdbc/reference/sqlserversavepoint-class.md) determinado de la transacción actual.  
  
> [!NOTE]  
>  El [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] no admite este método actualmente.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void releaseSavepoint(java.sql.Savepoint savepoint)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *savepoint*  
  
 El objeto SavePoint que se va a quitar.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método releaseSavepoint especifica este método releaseSavepoint en la interfaz java.sql.Connection.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-members.md)   
 [Clase SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md)  
  
  
