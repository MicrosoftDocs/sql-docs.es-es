---
description: Método supportsSchemasInDataManipulation (SQLServerDatabaseMetaData)
title: Método supportsSchemasInDataManipulation | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsSchemasInDataManipulation
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 812dc551-c718-494e-80d9-75732464c8ba
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7f6c79603f54b1e594d25fb145ada5af2ca6bf4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187671"
---
# <a name="supportsschemasindatamanipulation-method-sqlserverdatabasemetadata"></a>Método supportsSchemasInDataManipulation (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si un nombre de esquema se puede utilizar en una instrucción de manipulación de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean supportsSchemasInDataManipulation()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si es compatible. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método supportsSchemasInDataManipulation especifica este método supportsSchemasInDataManipulation en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
