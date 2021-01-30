---
description: sp_syscollector_run_collection_set (Transact-SQL)
title: sp_syscollector_run_collection_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_run_collection_set_TSQL
- sp_syscollector_run_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_run_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 7bbaee48-dfc7-45c0-b11f-c636b6a7e720
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dba48d2ba3e3945a137df381e4b403ebf1e6dfb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185261"
---
# <a name="sp_syscollector_run_collection_set-transact-sql"></a>sp_syscollector_run_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inicia un conjunto de recopilación si el recopilador ya está habilitado y el conjunto de recopilación está configurado para el modo de recopilación de datos no almacenados en caché.  
  
> [!NOTE]  
>  Este procedimiento producirá un error cuando se ejecute con un conjunto de recopilación que esté configurado para el modo de recopilación de datos almacenados en caché.  
  
 sp_syscollector_run_collection_set permite al usuario tomar instantáneas de datos a petición.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_run_collection_set [[ @collection_set_id = ] collection_set_id ]  
          , [[ @name = ] 'name' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @collection_set_id = ] collection_set_id` Es el identificador local único para el conjunto de recopilación. *collection_set_id* es de **tipo int** y debe tener un valor si *el nombre* es NULL.  
  
`[ @name = ] 'name'` Es el nombre del conjunto de recopilación. *Name* es de **tipo sysname** y debe tener un valor si *collection_set_id* es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 *Collection_set_id* o *Name* deben tener un valor, ambos no pueden ser null.  
  
 Este procedimiento iniciará los trabajos de recopilación y carga para el conjunto de recopilación especificado e iniciará inmediatamente el trabajo del agente de recopilación si el conjunto de recopilación tiene su **\@ collection_mode** establecido en sin almacenamiento en caché (1). Para obtener más información, vea [sp_syscollector_create_collection_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
 sp_sycollector_run_collection_set también se puede utilizar para ejecutar un conjunto de recopilación que no tenga una programación.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **dc_operator** (con permiso Execute) para ejecutar este procedimiento.  
  
## <a name="example"></a>Ejemplo  
 Iniciar un conjunto de recopilación mediante su identificador.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_run_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)  
  
  
