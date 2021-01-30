---
description: sys.syscolumns (Transact-SQL)
title: Columnas sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-enginel, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscolumns
- sys.syscolumns_TSQL
- syscolumns_TSQL
- syscolumns
dev_langs:
- TSQL
helpviewer_keywords:
- syscolumns system table
- sys.syscolumns compatibility view
ms.assetid: 863fd87b-ff33-4ac5-9aa9-df21140681da
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9a90b11d3bd6905fd71e253e27b03ef0e44e716
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180715"
---
# <a name="syssyscolumns-transact-sql"></a>sys.syscolumns (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada columna de cada tabla y vista, y una fila por cada parámetro de un procedimiento almacenado de la base de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del parámetro de columna o procedimiento.|  
|**id**|**int**|Id. de objeto de la tabla a la que pertenece la columna o Id. del procedimiento almacenado al que está asociado el parámetro.|  
|**tipoDex**|**tinyint**|Tipo de almacenamiento físico de **Sys. Types**.|  
|**typestat**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xusertype**|**smallint**|IDENTIFICADOR del tipo de datos extendido definido por el usuario. Produce un desbordamiento o devuelve NULL si el número de tipos de datos es superior a 32.767.|  
|**length**|**smallint**|Longitud máxima de almacenamiento físico de **Sys**. **tipos**.|  
|**xprec**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**xscale**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colid**|**smallint**|Id. de columna o parámetro.|  
|**xoffset**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**bitpos**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**sector**|**tinyint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**colstat**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**cdefault**|**int**|Id. del valor predeterminado de esta columna.|  
|**dominio**|**int**|Id. de la regla o restricción CHECK de esta columna.|  
|**número**|**smallint**|Número del subprocedimiento cuando el procedimiento está agrupado.<br /><br /> 0 = Entradas que no son de procedimiento|  
|**colorder**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**autoval**|**varbinary(8000)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**offset**|**smallint**|Desplazamiento en la fila en la que aparece esta columna.|  
|**collationid**|**int**|Id. de la intercalación de la columna. Es NULL para columnas no basadas en caracteres.|  
|**status**|**tinyint**|Mapa de bits utilizado para describir una propiedad de la columna o parámetro:<br /><br /> 0x08 = La columna admite valores NULL.<br /><br /> 0x10 = el relleno ANSI estaba activo cuando se agregaron columnas **VARCHAR** o **varbinary** . Se conservan los espacios en blanco finales para **VARCHAR** y los ceros a la derecha para las columnas **varbinary** .<br /><br /> 0x40 = El parámetro es de tipo OUTPUT.<br /><br /> 0x80 = La columna es de identidad.|  
|**type**|**tinyint**|Tipo de almacenamiento físico de **Sys**. **tipos**.|  
|**usertype**|**smallint**|IDENTIFICADOR del tipo de datos definido por el usuario de **Sys. Types**. Produce un desbordamiento o devuelve NULL si el número de tipos de datos es superior a 32.767.|  
|**printfmt**|**VARCHAR(255**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**prec**|**smallint**|Nivel de precisión de la columna.<br /><br /> -1 = **XML** o tipo de valor grande.|  
|**scale**|**int**|Escala para esta columna.<br /><br /> NULL = El tipo de datos no es numérico.|  
|**iscomputed**|**int**|Marca que señala si es una columna calculada:<br /><br /> 0 = No calculada<br /><br /> 1 = Calculada|  
|**isoutparam**|**int**|Indica si el parámetro de procedimiento es un parámetro de salida:<br /><br /> 1 = True<br /><br /> 0 = False|  
|**IsNullable**|**int**|Indica si la columna admite valores NULL:<br /><br /> 1 = True<br /><br /> 0 = False|  
|**intercalación**|**sysname**|Nombre de la intercalación de la columna. Es NULL si la columna no está basada en caracteres.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
