---
description: sys.sp_cdc_disable_table (Transact-SQL)
title: sys.sp_cdc_disable_table (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_cdc_disable_table
- sp_cdc_disable_table
- sys.sp_cdc_disable_table_TSQL
- sp_cdc_disable_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cdc_disable_table
- sys.sp_cdc_disable_table
- change data capture [SQL Server], disabling tables
ms.assetid: da2156c0-504e-4d76-b9a0-4448becf9bda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 53984d18923cef297e267374045f0f4cb4364419
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206022"
---
# <a name="syssp_cdc_disable_table-transact-sql"></a>sys.sp_cdc_disable_table (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Deshabilita la captura de datos modificados para la tabla de origen especificada y la instancia de captura en la base de datos actual. La captura de datos modificados no está disponible en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.sp_cdc_disable_table   
  [ @source_schema = ] 'source_schema' ,   
  [ @source_name = ] 'source_name'  
  [ , [ @capture_instance = ] 'capture_instance' | 'all' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @source_schema = ] 'source\_schema'` Es el nombre del esquema en el que se encuentra la tabla de origen. *source_schema* es de **tipo sysname**, no tiene ningún valor predeterminado y no puede ser null.  
  
 *source_schema* debe existir en la base de datos actual.  
  
`[ @source_name = ] 'source\_name'` Es el nombre de la tabla de origen desde la que se va a deshabilitar la captura de datos modificados. *source_name* es de **tipo sysname**, no tiene ningún valor predeterminado y no puede ser null.  
  
 *source_name* debe existir en la base de datos actual.  
  
`[ @capture_instance = ] 'capture\_instance' | 'all'` Es el nombre de la instancia de captura que se va a deshabilitar para la tabla de origen especificada. *capture_instance* es de **tipo sysname** y no puede ser null.  
  
 Cuando se especifica ' All ', se deshabilitan todas las instancias de captura definidas para *source_name* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 **Sys.sp_cdc_disable_table** quita la tabla de cambios de la captura de datos modificados y las funciones del sistema asociadas a la tabla de origen especificada y a la instancia de captura. Elimina las filas asociadas a la instancia de captura especificada de las tablas del sistema de captura de datos modificados y establece en 0 la columna de **is_tracked_by_cdc** para la entrada de la tabla en la vista de catálogo [Sys. Tables](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md) .  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **db_owner** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se deshabilita la captura de datos modificados para la tabla `HumanResources.Employee`.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_disable_table   
    @source_schema = N'HumanResources',   
    @source_name = N'Employee',  
    @capture_instance = N'HumanResources_Employee';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)  
  
  
