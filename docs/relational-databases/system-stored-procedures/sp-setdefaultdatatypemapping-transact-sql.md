---
description: sp_setdefaultdatatypemapping (Transact-SQL)
title: sp_setdefaultdatatypemapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_setdefaultdatatypemapping
- sp_setdefaultdatatypemapping_TSQL
helpviewer_keywords:
- sp_setdefaultdatatypemapping
ms.assetid: 7394e8ca-4ce1-4e99-a784-205007c2c248
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 46b17720b7d57110dc334a425976bf621a1395bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209244"
---
# <a name="sp_setdefaultdatatypemapping-transact-sql"></a>sp_setdefaultdatatypemapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Marca como predeterminada una asignación de tipo de datos existente entre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y un sistema de administración de bases de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (DBMS) que no es de. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_setdefaultdatatypemapping [ [ @mapping_id = ] mapping_id ]  
    [ , [ @source_dbms = ] 'source_dbms' ]  
    [ , [ @source_version = ] 'source_version' ]  
    [ , [ @source_type = ] 'source_type' ]   
    [ , [ @source_length_min = ] source_length_min ]  
    [ , [ @source_length_max = ] source_length_max ]  
    [ , [ @source_precision_min = ] source_precision_min ]  
    [ , [ @source_precision_max = ] source_precision_max ]  
    [ , [ @source_scale_min = ] source_scale_min ]  
    [ , [ @source_scale_max = ] source_scale_max ]  
    [ , [ @source_nullable = ] source_nullable ]  
    [ , [ @destination_dbms = ] 'destination_dbms' ]  
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' ]  
    [ , [ @destination_length = ] destination_length ]  
    [ , [ @destination_precision = ] destination_precision ]  
    [ , [ @destination_scale = ] destination_scale ]  
    [ , [ @destination_nullable = ] source_nullable ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @mapping_id = ] mapping_id` Identifica una asignación de tipo de datos existente.  *mapping_id* es de **tipo int** y su valor predeterminado es NULL. Si especifica *mapping_id*, no se requieren los parámetros restantes.  
  
`[ @source_dbms = ] 'source_dbms'` Es el nombre del DBMS desde el que se asignan los tipos de datos. *source_dbms* es de **tipo sysname** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**MSSQLSERVER**|El origen es una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|El origen es una base de datos de Oracle.|  
|NULL (predeterminado)||  
  
 Debe especificar este parámetro si *mapping_id* es NULL.  
  
`[ @source_version = ] 'source_version'` Es el número de versión del DBMS de origen. *source_version* es de tipo **VARCHAR (10)** y su valor predeterminado es NULL.  
  
`[ @source_type = ] 'source_type'` Es el tipo de datos del DBMS de origen. *source_type* es de **tipo sysname**. Debe especificar este parámetro si *mapping_id* es NULL.  
  
`[ @source_length_min = ] source_length_min` Es la longitud mínima del tipo de datos en el DBMS de origen. *source_length_min* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @source_length_max = ] source_length_max` Es la longitud máxima del tipo de datos en el DBMS de origen. *source_length_max* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @source_precision_min = ] source_precision_min` Es la precisión mínima del tipo de datos en el DBMS de origen. *source_precision_min* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @source_precision_max = ] source_precision_max` Es la precisión máxima del tipo de datos en el DBMS de origen. *source_precision_max* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @source_scale_min = ] source_scale_min` Es la escala mínima del tipo de datos en el DBMS de origen. *source_scale_min* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @source_scale_max = ] source_scale_max` Es la escala máxima del tipo de datos en el DBMS de origen. *source_scale_max* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @source_nullable = ] source_nullable` Es si el tipo de datos del DBMS de origen admite un valor NULL. *source_nullable* es de **bit** y su valor predeterminado es NULL. **1** significa que se admiten valores NULL.  
  
`[ @destination_dbms = ] 'destination_dbms'` Es el nombre del DBMS de destino. *destination_dbms* es de **tipo sysname** y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**MSSQLSERVER**|El destino es una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|El destino es una base de datos de Oracle.|  
|**DB2**|El destino es una base de datos IBM DB2.|  
|**SYBASE**|El destino es una base de datos Sybase.|  
|NULL (predeterminado)||  
  
`[ @destination_version = ] 'destination_version'` Es la versión del producto del DBMS de destino. *destination_version* es de tipo **VARCHAR (10)** y su valor predeterminado es NULL.  
  
`[ @destination_type = ] 'destination_type'` Es el tipo de datos que aparece en el DBMS de destino. *destination_type* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @destination_length = ] destination_length` Es la longitud del tipo de datos en el DBMS de destino. *destination_length* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @destination_precision = ] destination_precision` Es la precisión del tipo de datos en el DBMS de destino. *destination_precision* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @destination_scale = ] destination_scale` Es la escala del tipo de datos en el DBMS de destino. *destination_scale* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @destination_nullable = ] destination_nullable` Es si el tipo de datos del DBMS de destino admite un valor NULL. *destination_nullable* es de **bit** y su valor predeterminado es NULL. **1** significa que se admiten valores NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_setdefaultdatatypemapping** se utiliza en todos los tipos de replicación entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y un DBMS que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Las asignaciones de tipos de datos predeterminados se aplican a todas las topologías de replicación que incluyen el DBMS especificado.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_setdefaultdatatypemapping**.  
  
## <a name="see-also"></a>Consulte también  
 [Especificar asignaciones de tipos de datos para un publicador de Oracle](../../relational-databases/replication/publish/specify-data-type-mappings-for-an-oracle-publisher.md)   
 [sp_getdefaultdatatypemapping &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md)   
 [sp_helpdatatypemap &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)  
  
  
