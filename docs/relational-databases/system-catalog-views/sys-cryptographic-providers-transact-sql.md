---
description: sys.cryptographic_providers (Transact-SQL)
title: sys.cryptographic_providers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- cryptographic_providers
- sys.cryptographic_providers
- sys.cryptographic_providers_TSQL
- cryptographic_providers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.cryptographic_providers catalog view
ms.assetid: 9da0da95-792e-48b4-9f60-47f0729c279c
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 305ea4a72855dd5ba136740dcc9b4321826384ad
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384816"
---
# <a name="syscryptographic_providers-transact-sql"></a>sys.cryptographic_providers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila para cada proveedor criptográfico registrado.  
    
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**provider_id**|**int**|Número de identificación del proveedor de servicios criptográficos.|  
|**name**|**sysname**|Nombre del proveedor criptográfico.|  
|**guid**|**uniqueidentifier**|Proveedor único GUID.|  
|**version**|**nvarchar(50)**|Versión del proveedor con el formato ' *AA.BB.CCCC.DD* '.|  
|**dll_path**|**nvarchar(512)**|Ruta de acceso a DLL que implementa la interfaz de programación de aplicaciones (API) de la Administración extensible de claves (EKM).|  
|**is_enabled**|**bit**|Si el proveedor está habilitado en el servidor o no.<br /><br /> 0 = no habilitado (predeterminado)<br /><br /> 1 = habilitado|  
  
## <a name="permissions"></a>Permisos  
 La vista **Sys.cryptographic_providers** es visible para el público.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Jerarquía de cifrado](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Administración extensible de claves &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
  
