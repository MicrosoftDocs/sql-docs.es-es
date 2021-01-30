---
description: sys.sp_xtp_checkpoint_force_garbage_collection (Transact-SQL)
title: sys.sp_xtp_checkpoint_force_garbage_collection (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_xtp_checkpoint_force_garbage_collection_TSQL
- sys.sp_xtp_checkpoint_force_garbage_collection
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_checkpoint_force_garbage_collection
ms.assetid: 82b35b2b-edbd-44ac-9fc8-80695f2fd1df
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 31e2afff0668cd17605ecf83dbd3ca891f521f24
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190557"
---
# <a name="syssp_xtp_checkpoint_force_garbage_collection-transact-sql"></a>sys.sp_xtp_checkpoint_force_garbage_collection (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Marca los archivos de origen utilizados en la operación de mezcla con el número de secuencia de registro (LSN) después del cual no son necesarios y pueden ser eliminados por el recolector de elementos no utilizados. Además, mueve los archivos cuyo LSN asociado es inferior al punto de truncamiento de registro a la recolección de elementos no utilizados FILESTREAM.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
 
## <a name="syntax"></a>Sintaxis  
  
```  
sys.sp_xtp_checkpoint_force_garbage_collection [[ @dbname=database_name]  
```  
  
## <a name="arguments"></a>Argumentos  
 *database_name*  
 La base de datos en la que ejecutar la recolección de elementos no utilizados. El valor predeterminado es la base de datos actual.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 para correcto. Distinto de cero para error.  
  
## <a name="result-set"></a>Tipo de cursor  
 Una fila devuelta contiene la siguiente información:  
  
|Columna|Descripción|  
|------------|-----------------|  
|num_collected_items|Indica el número de archivos que se han movido a la recolección de elementos no utilizados FILESTREAM. Estos son los archivos cuyo número de secuencia de registro (LSN) es menor que el LSN del punto de truncamiento del registro|  
|num_marked_for_collection_items|Indica el número de archivos delta o de datos cuyo LSN se ha actualizado con el blockID de registro del LSN de fin de registro.|  
|last_collected_xact_seqno|Devuelve el último LSN correspondiente hasta el que los archivos se han movido para la recolección de elementos no utilizados FILESTREAM.|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso de propietario de la base de datos.  
  
## <a name="sample"></a>Ejemplo  
  
```  
exec [sys].[sp_xtp_checkpoint_force_garbage_collection] hkdb1  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
