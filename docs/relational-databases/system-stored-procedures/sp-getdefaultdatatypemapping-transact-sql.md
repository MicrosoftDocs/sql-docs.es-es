---
description: sp_getdefaultdatatypemapping (Transact-SQL)
title: sp_getdefaultdatatypemapping (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getdefaultdatatypemapping_TSQL
- sp_getdefaultdatatypemapping
helpviewer_keywords:
- sp_getdefaultdatatypemapping
ms.assetid: b8401de1-f135-41d0-ba79-ce8fe1f48c00
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 64345f8471caf5d757e0c8fbdbc843b2a18680ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183161"
---
# <a name="sp_getdefaultdatatypemapping-transact-sql"></a>sp_getdefaultdatatypemapping (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información sobre la asignación predeterminada para el tipo de datos especificado entre [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y un sistema de administración de bases de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (DBMS) que no es de. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_getdefaultdatatypemapping [ @source_dbms = ] 'source_dbms'   
    [ , [ @source_version = ] 'source_version' ]  
        , [ @source_type = ] 'source_type'    
    [ , [ @source_length = ] source_length ]  
    [ , [ @source_precision = ] source_precision ]  
    [ , [ @source_scale = ] source_scale ]  
    [ , [ @source_nullable = ] source_nullable ]  
        , [ @destination_dbms = ] 'destination_dbms'   
    [ , [ @destination_version = ] 'destination_version' ]  
    [ , [ @destination_type = ] 'destination_type' OUTPUT ]  
    [ , [ @destination_length = ] destination_length OUTPUT ]  
    [ , [ @destination_precision = ] destination_precision OUTPUT ]  
    [ , [ @destination_scale = ] destination_scale OUTPUT ]  
    [ , [ @destination_nullable = ] source_nullable OUTPUT ]  
    [ , [ @dataloss = ] dataloss OUTPUT ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @source_dbms = ] 'source_dbms'` Es el nombre del DBMS desde el que se asignan los tipos de datos. *source_dbms* es de **tipo sysname** y puede tener uno de los valores siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**MSSQLSERVER**|El origen es una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|El origen es una base de datos de Oracle.|  
  
 Es necesario especificar este parámetro.  
  
`[ @source_version = ] 'source_version'` Es el número de versión del DBMS de origen. *source_version* es de tipo **VARCHAR (10)** y su valor predeterminado es NULL.  
  
`[ @source_type = ] 'source_type'` Es el tipo de datos del DBMS de origen. *source_type* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @source_length = ] source_length` Es la longitud del tipo de datos en el DBMS de origen. *source_length* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @source_precision = ] source_precision` Es la precisión del tipo de datos en el DBMS de origen. *source_precision* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @source_scale = ] source_scale` Es la escala del tipo de datos en el DBMS de origen. *source_scale* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @source_nullable = ] source_nullable` Es si el tipo de datos del DBMS de origen admite un valor NULL. *source_nullable* es de **bits**, con un valor predeterminado de **1**, lo que significa que se admiten valores NULL.  
  
`[ @destination_dbms = ] 'destination_dbms'` Es el nombre del DBMS de destino. *destination_dbms* es de **tipo sysname** y puede tener uno de los valores siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**MSSQLSERVER**|El destino es una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**ORACLE**|El destino es una base de datos de Oracle.|  
|**DB2**|El destino es una base de datos IBM DB2.|  
|**SYBASE**|El destino es una base de datos Sybase.|  
  
 Es necesario especificar este parámetro.  
  
`[ @destination_version = ] 'destination_version'` Es la versión del producto del DBMS de destino. *destination_version* es de tipo **VARCHAR (10)** y su valor predeterminado es NULL.  
  
`[ @destination_type = ] 'destination_type' OUTPUT` Es el tipo de datos que aparece en el DBMS de destino. *destination_type* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @destination_length = ] destination_length OUTPUT` Es la longitud del tipo de datos en el DBMS de destino. *destination_length* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @destination_precision = ] destination_precision OUTPUT` Es la precisión del tipo de datos en el DBMS de destino. *destination_precision* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @destination_scale = ] _destination_scaleOUTPUT` Es la escala del tipo de datos en el DBMS de destino. *destination_scale* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @destination_nullable = ] _destination_nullableOUTPUT` Es si el tipo de datos del DBMS de destino admite un valor NULL. *destination_nullable* es de **bit** y su valor predeterminado es NULL. **1** significa que se admiten valores NULL.  
  
`[ @dataloss = ] _datalossOUTPUT` Es si la asignación tiene el potencial de pérdida de datos. *loss* es de **bit** y su valor predeterminado es NULL. **1** significa que hay una posibilidad de pérdida de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_getdefaultdatatypemapping** se utiliza en todos los tipos de replicación entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y un DBMS que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **sp_getdefaultdatatypemapping** devuelve el tipo de datos de destino predeterminado que es la coincidencia más cercana con el tipo de datos de origen especificado.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_getdefaultdatatypemapping**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_helpdatatypemap &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)   
 [sp_setdefaultdatatypemapping &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-setdefaultdatatypemapping-transact-sql.md)   
 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)   
 [Oracle Subscribers](../../relational-databases/replication/non-sql/oracle-subscribers.md)  
  
  
