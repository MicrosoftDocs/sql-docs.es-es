---
description: sys.sp_cdc_enable_db (Transact-SQL)
title: sys.sp_cdc_enable_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cdc_enable_db_TSQL
- sp_cdc_enable_db
- sys.sp_cdc_enable_db
- sys.sp_cdc_enable_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_cdc_enable_db
- change data capture [SQL Server], enabling databases
- sp_cdc_enable_db
ms.assetid: 176d83b3-493d-43cd-800e-aa123c3bdf17
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a0756c34e9fc914abdb99cddef865d0968767ab3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205998"
---
# <a name="syssp_cdc_enable_db-transact-sql"></a>sys.sp_cdc_enable_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Habilita la captura de datos modificados en la base de datos actual. Este procedimiento se debe ejecutar para una base de datos antes de que se puedan habilitar las tablas para la captura de datos modificados de esa base de datos. La captura de datos modificados registra las operaciones de inserción, actualización y eliminación aplicadas a las tablas habilitadas, proporcionando los detalles de los cambios en un formato relacional de uso sencillo. Para las filas modificadas, se captura la información de columna que duplica la estructura de las columnas de una tabla de origen sometida a seguimiento, junto con los metadatos necesarios para aplicar los cambios a un entorno de destino.  
  
> [!IMPORTANT]
>  La captura de datos modificados no está disponible en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.sp_cdc_enable_db  
```  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 No se puede habilitar la captura de datos modificados en [las bases de datos del sistema](../../relational-databases/databases/system-databases.md) o en las bases de datos de distribución.  
  
 sys.sp_cdc_enable_db crea los objetos de captura de datos modificados con un ámbito aplicable a toda la base de datos, incluidas las tablas de metadatos y los desencadenadores DDL. También crea el esquema CDC y el usuario de la base de datos CDC, y establece la columna is_cdc_enabled para la entrada de la base de datos en la vista de catálogo [Sys. Databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) en 1.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se habilita la captura de datos modificados.  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_cdc_enable_db;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.sp_cdc_disable_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md)  
  
  
