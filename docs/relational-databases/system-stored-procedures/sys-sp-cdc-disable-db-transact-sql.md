---
description: sys.sp_cdc_disable_db (Transact-SQL)
title: sys.sp_cdc_disable_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_disable_db
- sys.sp_cdc_disable_db_TSQL
- sp_cdc_disable_db_TSQL
- sys.sp_cdc_disable_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_disable_db
- sys.sp_cdc_disable_db
- change data capture [SQL Server], disabling databases
ms.assetid: 420fb99e-e60f-445b-b568-da96471f1e8f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5964b470b527874b16e9341522bd96e9a4110455
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182030"
---
# <a name="syssp_cdc_disable_db-transact-sql"></a>sys.sp_cdc_disable_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Deshabilita la captura de datos de cambio en la base de datos actual. La captura de datos modificados no está disponible en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql  
sys.sp_cdc_disable_db  
```  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **Sys.sp_cdc_disable_db** deshabilita la captura de datos modificados para todas las tablas de la base de datos habilitada actualmente. Se quitan todos los objetos del sistema relacionados con la captura de datos modificados, como tablas de cambios, trabajos, funciones y procedimientos almacenados. La columna **is_cdc_enabled** para la entrada de la base de datos en la vista de catálogo [Sys. Databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) está establecida en 0.  
  
> [!NOTE]  
>  Si hay muchas instancias de captura definidas para la base de datos cuando la captura de datos modificados está deshabilitada, una transacción que se ejecute de manera prolongada puede provocar un error en la ejecución de sys.sp_cdc_disable_db. Este problema se puede evitar deshabilitando las instancias de captura individuales mediante sys.sp_cdc_disable_table antes de ejecutar sys.sp_cdc_disable_db.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se deshabilita la configuración de captura de datos modificados para la base de datos `AdventureWorks2012`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_disable_db;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.sp_cdc_enable_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md)   
 [sys.sp_cdc_disable_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)  
  
