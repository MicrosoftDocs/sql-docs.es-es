---
description: Método setString (SQLServerCallableStatement)
title: Método setString (SQLServerCallableStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerCallableStatement.setString
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f38b97b5-d4f0-4f74-a33d-740241a85842
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 24334ffd36b51017b5b46e3aebebe52a7d913b0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178575"
---
# <a name="setstring-method-sqlservercallablestatement"></a>Método setString (SQLServerCallableStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el parámetro designado en el valor **String** de Java indicado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public void setString(java.lang.String sCol,  
                      java.lang.String s)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *sCol*  
  
 Valor **string** que contiene el nombre del parámetro.  
  
 *s*  
  
 Un valor **String**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setString especifica este método setString en la interfaz java.sql.CallableStatement.  
  
 Las conversiones de tipo string a binary solo se realizan cuando el [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] sabe que el tipo de destino es binary. Si el controlador JDBC no sabe cuál es el tipo subyacente, pasará el literal **String** y devolverá un error de servidor si este no puede realizar la conversión.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-members.md)   
 [Clase SQLServerCallableStatement](../../../connect/jdbc/reference/sqlservercallablestatement-class.md)  
  
  
