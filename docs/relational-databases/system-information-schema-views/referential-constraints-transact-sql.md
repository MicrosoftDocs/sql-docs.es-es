---
description: REFERENTIAL_CONSTRAINTS (Transact-SQL)
title: REFERENTIAL_CONSTRAINTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- REFERENTIAL_CONSTRAINTS
- REFERENTIAL_CONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.REFERENTIAL_CONSTRAINTS view
- REFERENTIAL_CONSTRAINTS view
ms.assetid: 5d358f18-0a85-4b55-af4b-98d5f4cd1020
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e3ee577dbd1aef1da2da8d22e253bfb90bb9616a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189624"
---
# <a name="referential_constraints-transact-sql"></a>REFERENTIAL_CONSTRAINTS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila para cada restricción FOREIGN KEY de la base de datos actual. Esta vista de esquema de información devuelve información acerca de los objetos para los que el usuario actual tiene permisos.  
  
 Para recuperar información de estas vistas, especifique el nombre completo de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Calificador de la restricción.|  
|**CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nombre del esquema que contiene la restricción.<br /><br /> **&#42;&#42; importante &#42;&#42;** No use vistas INFORMATION_SCHEMA para determinar el esquema de un objeto. INFORMATION_SCHEMA vistas solo representan un subconjunto de los metadatos de un objeto. La única manera confiable de encontrar el esquema de un objeto es consultar la vista de catálogo sys. Objects.|  
|**CONSTRAINT_NAME**|**sysname**|Nombre de la restricción.|  
|**UNIQUE_CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Calificador de la restricción UNIQUE.|  
|**UNIQUE_CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nombre del esquema que contiene la restricción UNIQUE.<br /><br /> **&#42;&#42; importante &#42;&#42;** No use vistas INFORMATION_SCHEMA para determinar el esquema de un objeto. INFORMATION_SCHEMA vistas solo representan un subconjunto de los metadatos de un objeto. La única manera confiable de encontrar el esquema de un objeto es consultar la vista de catálogo sys. Objects.|  
|**UNIQUE_CONSTRAINT_NAME**|**sysname**|Restricción UNIQUE.|  
|**MATCH_OPTION**|**VARCHAR (** 7 **)**|Condiciones de coincidencia de restricción referencial. Siempre devuelve SIMPLE. Esto significa que no se define ninguna coincidencia. La condición se considera una coincidencia en una de las siguientes circunstancias:<br /><br /> Al menos un valor de la columna de clave externa es NULL.<br /><br /> Ninguno de los valores de la columna de clave externa es NULL y una fila de la tabla de clave principal tiene la misma clave.|  
|**UPDATE_RULE**|**VARCHAR (** 11 **)**|Acción realizada cuando una [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción infringe la integridad referencial definida por esta restricción. Devuelve una de las siguientes opciones: <br />NO ACTION<br />CASCADE<br />SET NULL<br />SET DEFAULT<br /><br /> Si para esta restricción se especifica NO ACTION en ON UPDATE, la actualización de la clave principal a la que se hace referencia en la restricción no se propagará a la clave externa. Si una actualización de una clave principal de estas características provocara una infracción de la integridad referencial porque al menos una clave externa contiene el mismo valor, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no realizará ningún cambio en las tablas primaria y de referencia. Asimismo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generará un error.<br /><br /> Si para esta restricción se especifica CASCADE en ON UPDATE, cualquier cambio en el valor de clave principal se propagará automáticamente al valor de clave externa.|  
|**DELETE_RULE**|**VARCHAR (** 11 **)**|Acción que se realiza cuando una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] infringe la integridad referencial definida por esta restricción. Devuelve una de las siguientes opciones: <br />NO ACTION<br />CASCADE<br />SET NULL<br />SET DEFAULT<br /><br /> Si para esta restricción se especifica NO ACTION en ON DELETE, la eliminación de la clave principal a la que se hace referencia en la restricción no se propagará a la clave externa. Si una eliminación de una clave principal de estas características provocara una infracción de la integridad referencial porque al menos una clave externa contiene el mismo valor, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no realizará ningún cambio en las tablas primaria y de referencia. Asimismo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generará un error.<br /><br /> Si para esta restricción se especifica CASCADE en ON DELETE, cualquier cambio en el valor de clave principal se propagará automáticamente al valor de clave externa.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.foreign_keys &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)  
  
