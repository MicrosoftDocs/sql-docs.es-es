---
description: Método setEscapeProcessing (SQLServerStatement)
title: Método setEscapeProcessing (SQLServerStatement) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.setEscapeProcessing
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6ac0682e-e04c-4fdb-893b-92408d42051e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c60d547ed0a72a573bda8f16b4051ff796261dd5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173461"
---
# <a name="setescapeprocessing-method-sqlserverstatement"></a>Método setEscapeProcessing (SQLServerStatement)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el modo de procesamiento de escape.  
  
> [!NOTE]  
>  El proceso de escape del [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] está siempre habilitado. Si este método se establece en "false", no tendrá efecto alguno.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final void setEscapeProcessing(boolean enable)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *enable*  
  
 **true** para habilitar el procesamiento de escape. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setEscapeProcessing especifica este método setEscapeProcessing en la interfaz java.sql.Statement.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [Clase SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
