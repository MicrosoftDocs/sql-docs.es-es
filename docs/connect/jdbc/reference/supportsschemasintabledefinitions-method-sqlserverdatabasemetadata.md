---
description: Método supportsSchemasInTableDefinitions (SQLServerDatabaseMetaData)
title: Método supportsSchemasInTableDefinitions | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.supportsSchemasInTableDefinitions
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 3326b1b0-53e2-42ae-9ff7-98e8c7017ffa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c5dbaf0711011bc3d5e9ade3e19931a4564c1b33
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188408"
---
# <a name="supportsschemasintabledefinitions-method-sqlserverdatabasemetadata"></a>Método supportsSchemasInTableDefinitions (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera si un nombre de esquema se puede utilizar en una instrucción de definición de tablas.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public boolean supportsSchemasInTableDefinitions()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 **true** si es compatible. De lo contrario, se devuelve el valor **False**.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método supportsSchemasInTableDefinitions especifica este método supportsSchemasInTableDefinitions en la interfaz java.sql.DatabaseMetaData.  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
