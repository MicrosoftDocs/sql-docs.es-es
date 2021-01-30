---
description: sys.trusted_assemblies (Transact-SQL)
title: sys.trusted_assemblies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- trusted_assemblies_TSQL
- trusted_assemblies
- sys.trusted_assemblies_TSQL
- sys.trusted_assemblies
dev_langs:
- TSQL
helpviewer_keywords:
- sys.trusted_assemblies
ms.assetid: ''
author: VanMSFT
ms.author: vanto
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f2b26f5e646d1997959ba4856a88463ae8b1b3ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186154"
---
# <a name="systrusted_assemblies-transact-sql"></a>sys.trusted_assemblies (Transact-SQL)  
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

Contiene una fila para cada ensamblado de confianza del servidor.

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


|Nombre de la columna |Tipo de datos |Descripción |
|--- |--- |--- |
|hash |varbinary(8000) |SHA2_512 hash del contenido del ensamblado. |
|description |nvarchar(4000) |Descripción opcional definida por el usuario del ensamblado. Microsoft recomienda usar el nombre canónico que codifica el nombre simple, el número de versión, la referencia cultural, la clave pública y la arquitectura del ensamblado para confiar. Este valor identifica de forma única el ensamblado en el lado Common Language Runtime (CLR) y es el mismo que el valor clr_name de sys. Assemblies. |
|create_date |datetime2 |Fecha en que el ensamblado se agregó a la lista de ensamblados de confianza. |
|created_by |nvarchar(128) |Nombre de inicio de sesión de la entidad de seguridad que agregó el ensamblado a la lista. |
| | | |

### <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
 
## <a name="remarks"></a>Observaciones  
Use **[Sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)** para agregar y **[Sys.sp_drop_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-drop-trusted-assembly-transact-sql.md)** para quitar ensamblados de `sys.trusted_assemblies` .

## <a name="see-also"></a>Consulte también  
  [sys.sp_add_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-add-trusted-assembly-transact-sql.md)  
  [sys.sp_drop_trusted_assembly](../../relational-databases/system-stored-procedures/sys-sp-drop-trusted-assembly-transact-sql.md)  
  [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)  
  [sys.assemblies](../../relational-databases/system-catalog-views/sys-assemblies-transact-sql.md)  
  [sys.dm_clr_loaded_assemblies](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)  
