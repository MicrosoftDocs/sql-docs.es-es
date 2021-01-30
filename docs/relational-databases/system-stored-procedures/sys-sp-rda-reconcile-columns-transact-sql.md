---
title: sys.sp_rda_reconcile_columns (Transact-SQL) | Microsoft Docs
description: Más información sobre sys.sp_rda_reconcile_columns. Use este procedimiento almacenado para conciliar las columnas de las tablas remotas de Azure y las tablas de SQL Server habilitadas para Stretch.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.sp_rda_reconcile_columns
- sys.sp_rda_reconcile_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reconcile_columns stored procedure
ms.assetid: 60d9cc4e-1828-450b-9d88-5b8485800d73
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8eb33cbb1fd2975d96a727f6a7fde457c9827cc8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211781"
---
# <a name="syssp_rda_reconcile_columns-transact-sql"></a>sys.sp_rda_reconcile_columns (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Concilia las columnas de la tabla remota de Azure con las columnas de la tabla SQL Server habilitada para Stretch.  
    
  **sp_rda_reconcile_columns** agrega columnas a la tabla remota que existen en la tabla de SQL Server habilitada para Stretch, pero no en la tabla remota. Estas columnas pueden ser columnas que eliminó accidentalmente de la tabla remota. Sin embargo, **sp_rda_reconcile_columns** no elimina las columnas de la tabla remota que existen en la tabla remota, pero no en la tabla SQL Server.
  
  > [!IMPORTANT]
  > Cuando **sp_rda_reconcile_columns** vuelve a crear las columnas que eliminó por error de la tabla remota, no restaura los datos que había antes en las columnas eliminadas.
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
   
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_rda_reconcile_columns @objname = '@objname'  
  
```  
  
## <a name="arguments"></a>Argumentos  
 \@objName = '*\@ objName*'  
 Nombre de la tabla de SQL Server habilitada para Stretch.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o >0 (error)  
  
## <a name="permissions"></a>Permisos  
 Requiere permisos de db_owner.  
   
## <a name="remarks"></a>Observaciones  
 Si hay columnas en la tabla remota de Azure que ya no existen en la tabla de SQL Server habilitada para Stretch, estas columnas adicionales no impiden que Stretch Database funcione con normalidad. También puede quitar estas columnas adicionales de forma manual.  
  
## <a name="example"></a>Ejemplo  
 Para conciliar las columnas de la tabla remota de Azure, ejecute la siguiente instrucción.  
  
```sql  
EXEC sp_rda_reconcile_columns @objname = N'StretchEnabledTableName';  
```  
  
  
