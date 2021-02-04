---
description: Método start (SQLServerXAResource)
title: Método start (SQLServerXAResource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerXAResource.start
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 33c90213-92f7-416b-b2fa-67a1afe64e97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 041e4a8db2bbd30b5f7226c531d13a6ddb455f6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158272"
---
# <a name="start-method-sqlserverxaresource"></a>Método start (SQLServerXAResource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Comienza a funcionar en nombre de una bifurcación de transacción que se especifica en el objeto Xid.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void start(javax.transaction.xa.Xid xid,  
                  int flags)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *xid*  
  
 Un objeto Xid.  
  
 *flags*  
  
 Un valor **int**.  
  
## <a name="exceptions"></a>Excepciones  
 javax.transaction.xa.XAException  
  
## <a name="remarks"></a>Observaciones  
 El método start especifica este método start en la interfaz javax.transaction.xa.XAResource.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-methods.md)   
 [Miembros SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-members.md)   
 [Clase SQLServerXAResource](../../../connect/jdbc/reference/sqlserverxaresource-class.md)  
  
  
