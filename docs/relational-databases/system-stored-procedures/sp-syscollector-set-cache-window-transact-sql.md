---
description: sp_syscollector_set_cache_window (Transact-SQL)
title: sp_syscollector_set_cache_window (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_set_cache_window
- sp_syscollector_set_cache_window_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_set_cache_window stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 660f2749-392f-46bf-89f3-27764d848507
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a2fdab11e6cdd22b06005b2aa530e11b0c7e6173
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203672"
---
# <a name="sp_syscollector_set_cache_window-transact-sql"></a>sp_syscollector_set_cache_window (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Establece el número de veces que se va a intentar la carga de los datos en caso de error. Al reintentar una carga en caso de error, disminuye el riesgo de perder los datos recopilados.  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_set_cache_window [ @cache_window = ] cache_window   
```  
  
## <a name="arguments"></a>Argumentos  
 [ @cache_window =] *cache_window*  
 Es el número de veces que se va a reintentar una carga de datos al almacén de administración de datos en caso de error sin perder datos. *cache_window* es de **tipo int** y su valor predeterminado es 1. *cache_window* puede tener uno de los valores siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|-1|Almacenar en memoria caché todos los datos de carga provenientes de los errores de carga anteriores.|  
|0|No almacenar en memoria caché ningún dato proveniente de un error de carga.|  
|*n*|Almacenar en caché los datos de errores de carga anteriores, donde *n* >= 1.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Debe deshabilitar el recopilador de datos antes de cambiar la configuración de la ventana de la caché. Se produce un error en este procedimiento almacenado si se habilita el recopilador de datos. Para obtener más información, vea [habilitar o deshabilitar la recopilación de datos](../../relational-databases/data-collection/enable-or-disable-data-collection.md)y administrar la recopilación de [datos](../../relational-databases/data-collection/manage-data-collection.md).  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de base de datos dc_admin (con permiso EXECUTE) para ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se deshabilita el recopilador de datos, se configura la ventana de la caché para retener los datos hasta que se produzcan tres errores en las cargas y, a continuación, se habilita el recopilador de datos.  
  
```sql  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_disable_collector;  
GO  
EXECUTE dbo.sp_syscollector_set_cache_window 3;  
GO  
EXECUTE dbo.sp_syscollector_enable_collector;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_syscollector_set_cache_directory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-directory-transact-sql.md)  
  
  
