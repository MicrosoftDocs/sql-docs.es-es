---
description: sys.systypes (Transact-SQL)
title: Tipos de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.systypes_TSQL
- systypes_TSQL
- sys.systypes
- systypes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.systypes compatibility view
- systypes system table
ms.assetid: 1b0b1d0c-5f7b-470b-bd52-8bfa922d7889
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7f00dab371048ed8a1b39cb702897ce5f481c377
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160643"
---
# <a name="syssystypes-transact-sql"></a>sys.systypes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada tipo de datos que suministra el sistema o que define el usuario en la base de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del tipo de datos.|  
|**tipoDex**|**tinyint**|Tipo de almacenamiento físico.|  
|**status**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|Tipo extendido de usuario. Produce un desbordamiento o devuelve NULL si el número de tipos de datos es superior a 32.767.|  
|**length**|**smallint**|Longitud física del tipo de datos.|  
|**xprec**|**tinyint**|Precisión interna que usa el servidor. No debe utilizarse en consultas.|  
|**xscale**|**tinyint**|Escala interna que usa el servidor. No debe utilizarse en consultas.|  
|**tdefault**|**int**|Id. del procedimiento almacenado que contiene comprobaciones de integridad para este tipo de datos.|  
|**dominio**|**int**|Id. del procedimiento almacenado que contiene comprobaciones de integridad para este tipo de datos.|  
|**UID**|**smallint**|Id. de esquema del propietario del tipo.<br /><br /> Para bases de datos actualizadas a partir de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el identificador de esquema es igual que el identificador de usuario del propietario.<br /><br /> Importante: Si usa cualquiera de las siguientes instrucciones DDL, debe utilizar la vista de catálogo **\* Sys. Types en lugar desys.systipos. \* \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) <br /><br /> ALTER AUTHORIZATION ON TYPE<br /><br /> CREATE TYPE<br /><br /> Produce un desbordamiento o devuelve NULL si el número de usuarios y roles es superior a 32.767.|  
|**sector**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**collationid**|**int**|Si se basa en caracteres, **collationid** es el identificador de la intercalación de la base de datos actual; de lo contrario, es NULL.|  
|**usertype**|**smallint**|Identificador de tipo de usuario. Produce un desbordamiento o devuelve NULL si el número de tipos de datos es superior a 32.767.|  
|**variable**|**bit**|Tipo de datos de longitud variable.<br /><br /> 1 = True<br /><br /> 0 = False|  
|**allownulls**|**bit**|Indica la nulabilidad predeterminada para este tipo de datos. Este valor predeterminado es invalidado por si la nulabilidad se especifica mediante [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) o [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).|  
|**type**|**tinyint**|Tipo de datos de almacenamiento físico.|  
|**printfmt**|**VARCHAR(255**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|Nivel de precisión de este tipo de datos.<br /><br /> -1 = **XML** o tipos de valor grande.|  
|**scale**|**tinyint**|Escala del tipo de datos, basada en la precisión.<br /><br /> NULL = El tipo de datos no es numérico.|  
|**intercalación**|**sysname**|Si se basa en caracteres, la **Intercalación** es la intercalación de la base de datos actual; de lo contrario, es NULL.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
