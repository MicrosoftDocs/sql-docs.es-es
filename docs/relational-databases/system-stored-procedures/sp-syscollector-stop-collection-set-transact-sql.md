---
description: sp_syscollector_stop_collection_set (Transact-SQL)
title: sp_syscollector_stop_collection_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_stop_collection_set_TSQL
- sp_syscollector_stop_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_stop_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 4668cfb7-462f-40d0-948c-8f740a792a4d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 335e4966896ef6e08cdec78612caa2a53b3641b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159492"
---
# <a name="sp_syscollector_stop_collection_set-transact-sql"></a>sp_syscollector_stop_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Detiene un conjunto de recopilación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_stop_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @stop_collection_job = ] stop_collection_job ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @collection_set_id =] *collection_set_id*  
 Es el identificador único local del conjunto de recopilaciones. *collection_set_id* es de **tipo int** y su valor predeterminado es NULL. *collection_set_id* debe tener un valor si *el nombre* es NULL.  
  
 [ @name =] '*nombre*'  
 Es el nombre del conjunto de recopilación. *Name* es de **tipo sysname y su** valor predeterminado es NULL. *el nombre* debe tener un valor si *collection_set_id* es NULL.  
  
 [ @stop_collection_job =] *stop_collection_job*  
 Especifica que el trabajo de recopilación del conjunto de recopilación debe detenerse si está en ejecución. *stop_collection_job* es de **bit** y su valor predeterminado es 1.  
  
 *stop_collection_job* solo se aplica a los conjuntos de recopilación con el modo de recopilación establecido en almacenado en caché. Para obtener más información, vea [sp_syscollector_create_collection_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_syscollector_create_collection_set se debe ejecutar en el contexto de la base de datos del sistema msdb.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos dc_operator (con permiso EXECUTE) para ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se detiene un conjunto de recopilación mediante su identificador.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_stop_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)   
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)  
  
  
