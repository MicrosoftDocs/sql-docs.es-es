---
description: sp_syscollector_set_cache_directory (Transact-SQL)
title: sp_syscollector_set_cache_directory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_set_cache_directory_TSQL
- sp_syscollector_set_cache_directory
dev_langs:
- TSQL
helpviewer_keywords:
- data collector [SQL Server], stored procedures
- sp_syscollector_set_cache_directory stored procedure
ms.assetid: df56d5a5-8961-494f-a745-d752ca63805a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c77fa4764cc1456ba9a3cb2f9923182d0d384e0a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185271"
---
# <a name="sp_syscollector_set_cache_directory-transact-sql"></a>sp_syscollector_set_cache_directory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Especifica el directorio en el que se almacenan los datos recopilados antes de cargarlos en el almacén de administración de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_set_cache_directory [ @cache_directory = ] 'cache_directory'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @cache_directory = ] 'cache_directory'` El directorio del sistema de archivos donde se almacenan temporalmente los datos recopilados. *cache_directory* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Si no se especifica ningún valor, se utiliza el directorio temporal predeterminado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Debe deshabilitar el recopilador de datos antes de cambiar la configuración del directorio de la memoria caché. Se produce un error en este procedimiento almacenado si se habilita el recopilador de datos. Para obtener más información, vea [habilitar o deshabilitar la recopilación de datos](../../relational-databases/data-collection/enable-or-disable-data-collection.md)y administrar la recopilación de [datos](../../relational-databases/data-collection/manage-data-collection.md).  
  
 No es necesario que el directorio especificado exista en el momento en que se ejecuta el sp_syscollector_set_cache_directory; sin embargo, los datos no se pueden almacenar en caché y cargar correctamente hasta que se cree el directorio. Recomendamos crear el directorio antes de ejecutar este procedimiento almacenado.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de base de datos dc_admin (con permiso EXECUTE) para ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se deshabilita el recopilador de datos, se establece el directorio de caché para el recopilador de datos en `D:\tempdata` y, a continuación, se habilita el recopilador de datos.  
  
```sql  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_disable_collector;  
GO  
EXEC dbo.sp_syscollector_set_cache_directory N'D:\tempdata';  
GO  
EXECUTE dbo.sp_syscollector_enable_collector;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [sp_syscollector_set_cache_window &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-window-transact-sql.md)  
  
  
