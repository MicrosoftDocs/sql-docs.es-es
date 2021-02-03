---
description: '&#x40;&#x40;MAX_CONNECTIONS (Transact-SQL)'
title: '@@MAX_CONNECTIONS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@MAX_CONNECTIONS'
- '@@MAX_CONNECTIONS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- simultaneous connections [SQL Server]
- maximum number of simultaneous user connections
- '@@MAX_CONNECTIONS function'
- connections [SQL Server], simultaneous
- number of simultaneous user connections
ms.assetid: 57eb9f4b-548f-4212-9684-a11d831c4732
author: cawrites
ms.author: chadam
ms.openlocfilehash: 67e4a3868e852a7bd454347e8e1e4b9c5a7fade4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160244"
---
# <a name="x40x40max_connections-transact-sql"></a>&#x40;&#x40;MAX_CONNECTIONS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el número máximo de conexiones de usuario simultáneas que se permiten en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El número devuelto no es necesariamente el número configurado actualmente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
@@MAX_CONNECTIONS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **integer**  
  
## <a name="remarks"></a>Observaciones  
 El número real de conexiones de usuario permitidas depende también de la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que está instalada y de los límites de las aplicaciones y del hardware.  
  
 Para volver a configurar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de modo que admita menos conexiones, use **sp_configure**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra cómo devolver el número máximo de conexiones de usuario simultáneas en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En el ejemplo se considera que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se ha configurado de nuevo para utilizar menos conexiones de usuario.  
  
```sql 
SELECT @@MAX_CONNECTIONS AS 'Max Connections';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Max Connections  
---------------  
32767            
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Funciones de configuración](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [Establecer la opción de configuración del servidor Conexiones de usuario](../../database-engine/configure-windows/configure-the-user-connections-server-configuration-option.md)  
  
  
