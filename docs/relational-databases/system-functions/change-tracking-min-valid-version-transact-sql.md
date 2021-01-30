---
description: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
title: CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CHANGE_TRACKING_CLEANUP_VERSION
- CHANGE_TRACKING_CLEANUP_VERSION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGE_TRACKING_MIN_VALID_VERSION
- change tracking [SQL Server], CHANGE_TRACKING_MIN_VALID_VERSION
ms.assetid: 5a43d23f-adcf-4c0b-95ad-07cee03c1f9d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 860a3ed404cca81a0f07d19e247e3517bbfdc844
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196160"
---
# <a name="change_tracking_min_valid_version-transact-sql"></a>CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve la versión mínima del cliente que es válida para su uso en la obtención de información de seguimiento de cambios de la tabla especificada, cuando se usa la función [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) .  
    
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
CHANGE_TRACKING_MIN_VALID_VERSION ( table_object_id )  
```  
  
## <a name="arguments"></a>Argumentos  
 *table_object_id*  
 Es el identificador del objeto de una tabla. *table_object_id* es de **tipo int**.  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **bigint**  
  
## <a name="remarks"></a>Observaciones  
 Use esta función para validar el valor del parámetro *last_sync_version* de CHANGETABLE. Si *last_sync_version* es menor que el valor indicado por esta función, es posible que los resultados que se devuelven de una llamada posterior a CHANGETABLE no sean válidos.  
  
 CHANGE_TRACKING_MIN_VALID_VERSION utiliza la información siguiente para determinar el valor devuelto:  
  
-   Si la tabla estaba habilitada para el seguimiento de cambios.  
  
-   Si se ejecutó la tarea de limpieza en segundo plano para quitar la información de seguimiento de cambios más antigua que el período de la retención especificado para la base de datos.  
  
-   Si la tabla estaba truncada. Se quita toda la información del seguimiento de cambios asociada a la tabla.  
  
 La función devuelve NULL si se cumple una cualquiera de las siguientes condiciones:  
  
-   El seguimiento de cambios no está habilitado para la base de datos.  
  
-   El identificador del objeto de tabla especificado no es válido para la base de datos actual.  
  
-   Permiso insuficiente para la tabla especificada por el identificador del objeto.  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente determina si la versión especificada es una versión válida. El ejemplo obtiene la versión válida mínima de la tabla `dbo.Employees` y, a continuación, la compara con el valor de la variable `@last_sync_version`. Si el valor de `@last_sync_version` es menor que el valor de `@min_valid_version`, la lista de filas cambiadas no será válida.  
  
> [!NOTE]  
>  Normalmente, se obtendrá el valor de una tabla o de otra ubicación donde se almacenó el último número de versión utilizado para sincronizar los datos.  
  
```  
-- The tracked change is tagged with the specified context   
DECLARE @min_valid_version bigint, @last_sync_version bigint;  
  
SET @min_valid_version =   
CHANGE_TRACKING_MIN_VALID_VERSION(OBJECT_ID('dbo.Employees'));  
  
SET @last_sync_version = 11  
IF (@last_sync_version < @min_valid_version)  
-- Error � do not obtain changes  
ELSE  
-- Obtain changes using CHANGETABLE(CHANGES ...)  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de seguimiento de cambios &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [sys.change_tracking_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)  
  
  
