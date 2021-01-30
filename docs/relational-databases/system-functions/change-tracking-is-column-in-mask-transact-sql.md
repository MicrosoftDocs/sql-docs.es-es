---
description: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
title: CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CHANGE_TRACKING_IS_COLUMN_IN_MASK_TSQL
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
dev_langs:
- TSQL
helpviewer_keywords:
- change tracking [SQL Server], CHANGE_TRACKING_IS_COLUMN_IN_MASK
- CHANGE_TRACKING_IS_COLUMN_IN_MASK
ms.assetid: 649b370b-da54-4915-919d-1b597a39d505
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8d6566cb13adcc131aa8e4731c85ddb63e53f202
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196164"
---
# <a name="change_tracking_is_column_in_mask-transact-sql"></a>CHANGE_TRACKING_IS_COLUMN_IN_MASK (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Interpreta el valor SYS_CHANGE_COLUMNS devuelto por la función CHANGETABLE (CHANGes...). Esto permite a una aplicación determinar si la columna especificada está incluida en los valores devueltos para SYS_CHANGE_COLUMNS.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
CHANGE_TRACKING_IS_COLUMN_IN_MASK ( column_id , change_columns )  
```  
  
## <a name="arguments"></a>Argumentos  
 *column_id*  
 Es el Id. de la columna que se está comprobando. El ID. de columna se puede obtener mediante la función [COLUMNPROPERTY](../../t-sql/functions/columnproperty-transact-sql.md) .  
  
 *change_columns*  
 Son los datos binarios de la columna SYS_CHANGE_COLUMNS de los datos [CHANGETABLE](../../relational-databases/system-functions/changetable-transact-sql.md) .  
  
## <a name="return-type"></a>Tipo de valor devuelto  
 **bit**  
  
## <a name="return-values"></a>Valores devueltos  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK devuelve los siguientes valores.  
  
|Valor devuelto|Descripción|  
|------------------|-----------------|  
|0|La columna especificada no está en la lista de *change_columns* .|  
|1|La columna especificada está en la lista de *change_columns* .|  
  
## <a name="remarks"></a>Observaciones  
 CHANGE_TRACKING_IS_COLUMN_IN_MASK no realiza ninguna comprobación para validar el valor *column_id* o si el parámetro *change_columns* se obtuvo de la tabla de la que se obtuvo el *column_id* .  
  
## <a name="examples"></a>Ejemplos  
 El siguiente ejemplo determina si la columna `Salary` de la tabla `Employees` está actualizada. La `COLUMNPROPERTY` función devuelve el ID. de columna de la `Salary` columna. La variable local `@change_columns` debe establecerse en los resultados de una consulta con CHANGETABLE como origen de datos.  
  
```sql  
SET @SalaryChanged = CHANGE_TRACKING_IS_COLUMN_IN_MASK  
    (COLUMNPROPERTY(OBJECT_ID('Employees'), 'Salary', 'ColumnId')  
    ,@change_columns);  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de seguimiento de cambios &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [CHANGETABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/changetable-transact-sql.md)   
 [Seguimiento de cambios de datos &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)  
  
  
