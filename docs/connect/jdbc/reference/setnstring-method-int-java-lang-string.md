---
description: Método setNString (int, java.lang.String)
title: Método setNString (int, java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: b7da6d44-f5b1-44f8-95f5-40179968b1b0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c9a0e8ddb0bb108fbb6772ad12e98256efa291d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178784"
---
# <a name="setnstring-method-int-javalangstring"></a>Método setNString (int, java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el parámetro designado en el objeto **String** especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final void setNString(int parameterIndex,  
                                                  java.lang.String value)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *parameterIndex*  
  
 Un valor **int** que indica el índice del parámetro.  
  
 *value*  
  
 Un objeto **String** que contiene el valor del parámetro.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 Este método se debe usar para los tipos de datos **NCHAR**, **NVARCHAR**, **NTEXT** y **XML**.  
  
 El método setNString especifica este método setNString en la interfaz java.sql.PreparedStatement.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
