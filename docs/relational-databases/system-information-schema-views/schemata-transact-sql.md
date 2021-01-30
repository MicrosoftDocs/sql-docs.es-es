---
description: SCHEMATA (Transact-SQL)
title: ESQUEMAs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- SCHEMATA_TSQL
- SCHEMATA
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.SCHEMATA view
- SCHEMATA view
ms.assetid: 69617642-0f54-4b25-b62f-5f39c8909601
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cbb311366b6d2d036d44570761a7d198450b37a7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185509"
---
# <a name="schemata-transact-sql"></a>SCHEMATA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada esquema de la base de datos actual. Para recuperar información de estas vistas, especifique el nombre completo de **INFORMATION_SCHEMA.** _view_name_. Para recuperar información sobre todas las bases de datos en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte la vista de catálogo [Sys. databases &#40;TRANSACT-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**CATALOG_NAME**|**sysname**|Nombre de la base de datos actual|  
|**SCHEMA_NAME**|**nvarchar (** 128 **)**|Devuelve el nombre del esquema.|  
|**SCHEMA_OWNER**|**nvarchar (** 128 **)**|Nombre del propietario del esquema.<br /><br /> **&#42;&#42; importante &#42;&#42;** No use vistas INFORMATION_SCHEMA para determinar el esquema de un objeto. INFORMATION_SCHEMA vistas solo representan un subconjunto de los metadatos de un objeto. La única manera confiable de localizar el esquema de un objeto consiste en consultar la vista de catálogo sys.objects.|  
|**DEFAULT_CHARACTER_SET_CATALOG**|**VARCHAR (** 6 **)**|Siempre devuelve NULL.|  
|**DEFAULT_CHARACTER_SET_SCHEMA**|**VARCHAR (** 3 **)**|Siempre devuelve NULL.|  
|**DEFAULT_CHARACTER_SET_NAME**|**sysname**|Devuelve el nombre del juego de caracteres predeterminado.|  

**Ejemplo**  
En el ejemplo siguiente, se devuelve información acerca de los esquemas de la base de datos maestra:  
```sql  
SELECT * FROM master.INFORMATION_SCHEMA.SCHEMATA;
```  

## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.schemas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)   
 [sys.sysde juegos de caracteres &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)  
  
