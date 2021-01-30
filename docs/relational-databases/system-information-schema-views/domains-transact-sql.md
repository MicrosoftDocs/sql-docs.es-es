---
description: DOMAINS (Transact-SQL)
title: Dominios (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DOMAINS_TSQL
- DOMAINS
dev_langs:
- TSQL
helpviewer_keywords:
- DOMAINS view
- INFORMATION_SCHEMA.DOMAINS view
ms.assetid: f0b734d5-816f-4b10-a60c-615931b515c2
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 19c906eb2c32884b02e9c7835c88186823516aa2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190094"
---
# <a name="domains-transact-sql"></a>DOMAINS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila por cada tipo de datos de alias al que puede tener acceso el usuario actual en la base de datos actual.  
  
 Para recuperar información de estas vistas, especifique el nombre completo de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Base de datos donde existe el tipo de datos de alias.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Nombre del esquema que contiene el tipo de datos del alias.<br /><br /> **&#42;&#42; importante &#42;&#42;** No use vistas de INFORMATION_SCHEMA para determinar el esquema de un tipo de datos. La única manera confiable de localizar el esquema de un tipo consiste en utilizar la función TYPEPROPERTY.|  
|**DOMAIN_NAME**|**sysname**|Tipo de datos de alias.|  
|**DATA_TYPE**|**sysname**|Tipo de datos proporcionado por el sistema.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Longitud máxima, en caracteres, de los datos binarios, de caracteres, o de texto e imagen.<br /><br /> -1 para **XML** y datos de tipo de valor grande. En caso contrario se devuelve NULL. Para obtener más información, vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md).|  
|**CHARACTER_OCTET_LENGTH**|**int**|Longitud máxima, en bytes, para datos binarios, datos de caracteres o datos de texto e imagen.<br /><br /> -1 para **XML** y datos de tipo de valor grande. En caso contrario se devuelve NULL.|  
|**COLLATION_CATALOG**|**VARCHAR (** 6 **)**|Siempre devuelve NULL.|  
|**COLLATION_SCHEMA**|**VARCHAR (** 3 **)**|Siempre devuelve NULL.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Devuelve el nombre único del criterio de ordenación si la columna es de tipo de datos de caracteres o de **texto** . En caso contrario se devuelve NULL.|  
|**CHARACTER_SET_CATALOG**|**VARCHAR (** 6 **)**|Devuelve **Master**. Esto indica la base de datos en la que se encuentra el juego de caracteres, si la columna es de tipo de datos de caracteres o de **texto** . En caso contrario se devuelve NULL.|  
|**CHARACTER_SET_SCHEMA**|**VARCHAR (** 3 **)**|Siempre devuelve NULL.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Devuelve el nombre único del juego de caracteres si esta columna es de tipo de datos de caracteres o de **texto** . En caso contrario se devuelve NULL.|  
|**NUMERIC_PRECISION**|**tinyint**|Precisión de los datos numéricos aproximados, datos numéricos exactos, datos enteros o datos monetarios. En caso contrario se devuelve NULL.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Base de la precisión de datos numéricos aproximados, datos numéricos exactos, datos enteros o datos monetarios. En caso contrario se devuelve NULL.|  
|**NUMERIC_SCALE**|**tinyint**|Escala de datos numéricos aproximados, datos numéricos exactos, datos enteros o datos monetarios. En caso contrario se devuelve NULL.|  
|**DATETIME_PRECISION**|**smallint**|Código de subtipo para el tipo de datos **DateTime** e **Interval** ISO. Para otros tipos de datos, esta columna devuelve un valor NULL.|  
|**DOMAIN_DEFAULT**|**nvarchar (** 4000 **)**|Texto real de la instrucción de definición de [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.sysde juegos de caracteres &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syscharsets-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.configurations&#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
