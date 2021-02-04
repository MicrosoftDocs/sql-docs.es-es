---
description: Método autoCommitFailureClosesAllResultSets (SQLServerDatabaseMetaData)
title: El controlador JDBC cierra los conjuntos de resultados abiertos | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 1739ecb5-e5cb-4807-b5a8-97c0299929d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6482a22b6a39db54d69638b0a91c2c84f2d21477
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163546"
---
# <a name="autocommitfailureclosesallresultsets-method-sqlserverdatabasemetadata"></a>Método autoCommitFailureClosesAllResultSets (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Indica si el controlador JDBC cierra todos los conjuntos de resultados abiertos, incluso los que se pueden retener, cuando se habilita la confirmación automática y se produce una excepción.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean autoCommitFailureClosesAllResultSets()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si todos los conjuntos de resultados abiertos, incluidos los que se pueden retener, se cierran cuando se habilita la confirmación automática y se produce una excepción. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método autoCommitFailureClosesAllResultSets especifica este método autoCommitFailureClosesAllResultSets en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
