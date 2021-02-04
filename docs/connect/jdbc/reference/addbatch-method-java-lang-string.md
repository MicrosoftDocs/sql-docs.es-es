---
description: Método addBatch (java.lang.String)
title: Método addBatch (java.lang.String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerPreparedStatement.addBatch (java.lang.String)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 093f6c3b-49a6-4043-9993-bd0482de04dd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0771dc065700ee23454ff621a3f890857f9b9e06
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163585"
---
# <a name="addbatch-method-javalangstring"></a>Método addBatch (java.lang.String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Agrega el comando SQL determinado a la lista de comandos actual para este objeto [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void addBatch(java.lang.String sql)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *sql*  
  
 Un valor **String** que contiene una instrucción SQL.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método addBatch especifica este método addBatch en la interfaz java.sql.Statement.  
  
 Si se llama a este método se producirá una excepción, ya que la instrucción SQL para el objeto SQLServerPreparedStatement se especificó cuando se creó el objeto.  
  
## <a name="see-also"></a>Consulte también  
 [Método addBatch &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/addbatch-method-sqlserverpreparedstatement.md)   
 [Miembros de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)   
 [Clase SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md)  
  
  
