---
description: sys.security_policies (Transact-SQL)
title: sys.security_policies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SYS.SECURITY_POLICIES_TSQL
- SECURITY_POLICIES_TSQL
- SYS.SECURITY_POLICIES
- SECURITY_POLICIES
dev_langs:
- TSQL
helpviewer_keywords:
- sys.security_policies catalog view
- security_policies catalog view
ms.assetid: 35362f5b-e601-4049-9e1d-c5307e823831
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c33e75e803b7b5d9972d373f4cc5738601197860
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182503"
---
# <a name="syssecurity_policies-transact-sql"></a>sys.security_policies (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Devuelve una fila para cada directiva de seguridad en la base de datos.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nombre de la directiva de seguridad, único dentro de la base de datos.|  
|object_id|**int**|Identificador de la directiva de seguridad.|  
|principal_id|**int**|Identificador del propietario de la directiva de seguridad, tal y como se registró en la base de datos. Es NULL si el propietario se determina con el esquema.|  
|schema_id|**int**|Identificador del esquema en el que reside el objeto.|  
|parent_object_id|**int**|Identificador del objeto al que pertenece la directiva. Debe ser 0.|  
|tipo|**vachar (2)**|Debe ser **SP**.|  
|type_desc|**nvarchar(60)**|**SECURITY_POLICY**.|  
|create_date|**datetime**|Fecha UTC de creación de la directiva de seguridad.|  
|modify_date|**datetime**|Fecha UTC en la que la directiva de seguridad se modificó por última vez.|  
|is_ms_shipped|**bit**|Siempre es false.|  
|is_enabled|**bit**|Estado de la especificación de la directiva de seguridad:<br /><br /> 0 = deshabilitado<br /><br /> 1 = habilitado|  
|is_not_for_replication|**bit**|La directiva se creó con la opción NOT FOR REPLICATION.|  
|uses_database_collation|**bit**|Utiliza la misma intercalación que la base de datos.|  
|is_schemabinding_enabled|**bit**|Estado de SCHEMABINDING para la Directiva de seguridad:<br /><br /> 0 o NULL = habilitado<br /><br /> 1 = deshabilitado|  
  
## <a name="permissions"></a>Permisos  
 Las entidades de seguridad con el permiso **ALTER any Security Policy** tienen acceso a todos los objetos de esta vista de catálogo, así como a cualquier persona con **View definition** en el objeto.  
  
## <a name="see-also"></a>Consulte también  
 [Seguridad de nivel de fila](../../relational-databases/security/row-level-security.md)   
 [sys.security_predicates &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-security-predicates-transact-sql.md)   
 [CREATE SECURITY POLICY &#40;Transact-SQL&#41;](../../t-sql/statements/create-security-policy-transact-sql.md)   
 [Vistas de catálogo de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
