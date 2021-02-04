---
description: Método getClientInfoProperties (SQLServerDatabaseMetaData)
title: Método getClientInfoProperties (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 1568aef4-f4c4-40a0-a1ab-9c106905bd92
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fe2bb68b8567393ccaa8481e0903bd8b97c5664
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163341"
---
# <a name="getclientinfoproperties-method-sqlserverdatabasemetadata"></a>Método getClientInfoProperties (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera una lista de las propiedades de la información de cliente que admite el controlador.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.sql.ResultSet getClientInfoProperties()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un objeto ResultSet.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getClientInfoProperties especifica este método getClientInfoProperties en la interfaz java.sql.DatabaseMetaData.  
  
> [!NOTE]  
>  Este método devuelve un conjunto de resultados vacío. El controlador permite establecer solamente **applicationName** y establece **applicationName** únicamente en el momento de la conexión. SQL Server no proporciona soporte técnico para actualizar la información de la aplicación cliente una vez se haya establecido la conexión.  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
