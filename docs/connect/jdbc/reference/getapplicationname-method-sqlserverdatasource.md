---
description: Método getApplicationName (SQLServerDataSource)
title: Método getApplicationName (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getApplicationName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f71e501c-ccd7-4a1e-b6ea-4d47a81c18c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 45e4ce6a37e24ee4d3e8692fe4b145e8e8f019b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168421"
---
# <a name="getapplicationname-method-sqlserverdatasource"></a>Método getApplicationName (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Devuelve el nombre de la aplicación.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getApplicationName()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Un elemento **String** que contiene el nombre de aplicación o "[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]" si no se establece ningún valor.  
  
## <a name="remarks"></a>Observaciones  
 El nombre de la aplicación se usa para identificar la aplicación específica en diversas herramientas de creación de perfiles y registros de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si no se establece el nombre de aplicación, el método getApplicationName devuelve la cadena no localizada "[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]".  
  
## <a name="see-also"></a>Consulte también  
 [Miembros SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [Clase SQLServerDataSource](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
