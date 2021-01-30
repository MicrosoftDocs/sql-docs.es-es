---
description: sp_syscollector_start_collection_set (Transact-SQL)
title: sp_syscollector_start_collection_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_start_collection_set_TSQL
- sp_syscollector_start_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_start_collection_set
ms.assetid: d8357180-f51e-4681-99f9-0596fe2d2b53
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 839b9ef5a7d20850d2f8a9658a9c7ba875757ede
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190581"
---
# <a name="sp_syscollector_start_collection_set-transact-sql"></a>sp_syscollector_start_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inicia un conjunto de recopilación si el recopilador ya está habilitado y el conjunto de recopilación no se está ejecutando. Si el recopilador no está habilitado, habilite el recopilador ejecutando [sp_syscollector_enable_collector](../../relational-databases/system-stored-procedures/sp-syscollector-enable-collector-transact-sql.md) y, a continuación, use este procedimiento almacenado para iniciar un conjunto de recopilación.  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_start_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [[ @name = ] 'name' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @collection_set_id = ] collection_set_id` Es el identificador local único para el conjunto de recopilación. *collection_set_id* es de **tipo int** y su valor predeterminado es NULL. *collection_set_id* debe tener un valor si *el nombre* es NULL.  
  
`[ @name = ] 'name'` Es el nombre del conjunto de recopilación. *Name* es de **tipo sysname y su** valor predeterminado es NULL. *el nombre* debe tener un valor si *collection_set_id* es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_syscollector_create_collection_set se debe ejecutar en el contexto de la base de datos del sistema msdb y el Agente SQL Server debe estar habilitado.  
  
 Este procedimiento producirá un error cuando se ejecute con un conjunto de recopilación que no tenga una programación. Si el conjunto de recopilación no tiene una programación (porque su modo de recopilación está establecido en sin almacenamiento en caché, por ejemplo), use el [sp_syscollector_run_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-run-collection-set-transact-sql.md) procedimiento almacenado para iniciar el conjunto de recopilación.  
  
 Este procedimiento habilita los trabajos de recopilación y carga para el conjunto de recopilación especificado e iniciará inmediatamente el trabajo del agente de recopilación si el conjunto de recopilación tiene su modo de recopilación establecido en sin caché (0). Para obtener más información, vea [sp_syscollector_create_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
 Si el conjunto de recopilación no contiene ningún elemento de recopilación, esta operación no tiene ningún efecto. Se devuelve el error 14685 como una advertencia.  
  
## <a name="permissions"></a>Permisos  
 Es preciso pertenecer al rol fijo de base de datos dc_operator para ejecutar este procedimiento. Si el conjunto de recopilación no tiene una cuenta de proxy, es necesaria la pertenencia al rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se inicia un conjunto de recopilación mediante su identificador.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_start_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)   
 [syscollector_collection_sets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)  
  
  
