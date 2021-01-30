---
description: IHsyscolumns (Transact-SQL)
title: IHsyscolumns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHsyscolumns
- IHsyscolumns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHsyscolumns view
ms.assetid: 263452f1-9708-48f0-9536-402a89e7f5bf
author: stevestein
ms.author: sstein
ms.openlocfilehash: c9c0733638fd79c025b94b2e63cf9b5350dd0178
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204307"
---
# <a name="ihsyscolumns-transact-sql"></a>IHsyscolumns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **IHsyscolumns** expone información de columna para los artículos publicados desde un publicador que no es de SQL Server. Esta vista se almacena en basededatosdedistribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre de la columna o parámetro de procedimiento.|  
|**id**|**int**|Id. de objeto de la tabla a la que pertenece esta columna o Id. del procedimiento almacenado al que está asociado este parámetro.|  
|**tipoDex**|**tinyint**|Tipo de almacenamiento físico de [sys.systipos &#40;&#41;de Transact-SQL ](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md).|  
|**typestat**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**tinyint**|Id. del tipo de datos extendido definido por el usuario.|  
|**length**|**bigint**|La longitud máxima de almacenamiento físico de los [ tipos desys.sys&#40;&#41;de Transact-SQL ](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md).|  
|**xprec**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xscale**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colid**|**int**|Id. de la columna o parámetro.|  
|**xoffset**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bitpos**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**sector**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colstat**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cdefault**|**int**|IDENTIFICADOR del valor predeterminado para esta columna.|  
|**dominio**|**int**|IDENTIFICADOR de la regla o restricción CHECK para esta columna.|  
|**número**|**int**|El número de subprocedimiento cuando el procedimiento está agrupado (**0** para las entradas que no son de procedimiento).|  
|**colorder**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**autoval**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offset**|**int**|Desplazamiento en la fila en la que aparece esta columna.|  
|**collationid**|**int**|IDENTIFICADOR de la intercalación de la columna. Es NULL para las columnas no basadas en caracteres.|  
|**language**|**int**|Identificador de idioma de la columna.|  
|**status**|**int**|El mapa de bits que se usa para describir una propiedad de la columna o el parámetro:<br /><br /> **0x08** = columna permite valores NULL.<br /><br /> **0x10** = el relleno ANSI estaba activo cuando se agregaron columnas **VARCHAR** o **varbinary** . Se conservan los espacios en blanco finales para **VARCHAR** y los ceros a la derecha para las columnas **varbinary** .<br /><br /> **0x40** = el parámetro es un parámetro de salida.<br /><br /> **0x80** = la columna es una columna de identidad.|  
|**type**|**int**|Tipo de almacenamiento físico de [sys.systipos &#40;&#41;de Transact-SQL ](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md).|  
|**usertype**|**tinyint**|IDENTIFICADOR del tipo de datos definido por el usuario de [sys.systipos &#40;&#41;de Transact-SQL ](../../relational-databases/system-compatibility-views/sys-systypes-transact-sql.md).|  
|**printfmt**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**int**|Nivel de precisión de esta columna.|  
|**scale**|**int**|Escala de esta columna.|  
|**iscomputed**|**int**|Marca que especifica si es una columna calculada:<br /><br /> **0** = no calculado.<br /><br /> **1** = Calculada.|  
|**isoutparam**|**int**|Indica si el parámetro de procedimiento es un parámetro de salida:<br /><br /> **1** = true.<br /><br /> **0** = false.|  
|**IsNullable**|**int**|Indica si la columna admite valores NULL:<br /><br /> **1** = true.<br /><br /> **0** = false.|  
|**intercalación**|**int**|Nombre de la intercalación de la columna. Es NULL para las columnas no basadas en caracteres.|  
|**tdscollation**|**int**|Nombre de la intercalación de la columna cuando se devuelve en un flujo TDS.|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)  
  
  
