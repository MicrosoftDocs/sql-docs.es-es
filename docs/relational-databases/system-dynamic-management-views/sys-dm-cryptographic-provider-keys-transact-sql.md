---
description: sys.dm_cryptographic_provider_keys (Transact-SQL)
title: sys.dm_cryptographic_provider_keys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_cryptographic_provider_keys_TSQL
- dm_cryptographic_provider_keys_TSQL
- dm_cryptographic_provider_keys
- sys.dm_cryptographic_provider_keys
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cryptographic_provider_keys dynamic management function
ms.assetid: 5a8c1421-c56b-44b5-96e5-4f01782a0c7c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9fcfeed749aea4fa18ac74ba4e78cc9dfa368828
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196297"
---
# <a name="sysdm_cryptographic_provider_keys-transact-sql"></a>sys.dm_cryptographic_provider_keys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de las claves proporcionadas por un proveedor de Administración extensible de claves (EKM).  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
dm_cryptographic_provider_keys ( provider_id )  
```  
  
## <a name="arguments"></a>Argumentos  
 *provider_id*  
 Número de identificación del proveedor de EKM, sin valor predeterminado.  
  
## <a name="tables-returned"></a>Tablas devueltas  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**key_id**|**int**|Número de identificación de la clave del proveedor.|  
|**key_name**|**nvarchar(512)**|Nombre de la clave del proveedor.|  
|**key_thumbprint**|**varbinary(32)**|Huella digital del proveedor de la clave.|  
|**algorithm_id**|**int**|Número de identificación del algoritmo del proveedor.|  
|**algorithm_tag**|**int**|Etiqueta del algoritmo del proveedor.|  
|**key_type**|**NCHAR (256)**|Tipo de clave del proveedor.|  
|**key_length**|**int**|Longitud de la clave del proveedor.|  
  
## <a name="permissions"></a>Permisos  
 Cuando se consulte esta vista, ésta autenticará el contexto de usuario con el proveedor y enumerará todas las claves visibles al usuario.  
  
 Si el usuario no puede autenticar con el proveedor de EKM, no se devolverá ninguna información acerca de la clave.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente muestra las propiedades de la clave para un proveedor con el número de identificación `1234567`.  
  
```  
SELECT * FROM sys.dm_cryptographic_provider_keys(1234567);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
  
