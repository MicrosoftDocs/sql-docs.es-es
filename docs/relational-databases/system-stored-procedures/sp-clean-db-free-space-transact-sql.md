---
description: sp_clean_db_free_space (Transact-SQL)
title: sp_clean_db_free_space (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_clean_db_free_space_TSQL
- sp_clean_db_free_space
dev_langs:
- TSQL
helpviewer_keywords:
- sp_clean_db_free_space
- ghost records
ms.assetid: faa96f7e-be92-47b1-8bc5-4dbba5331655
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1779f07e6c8923f56bb4597623a9b079679b3779
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206526"
---
# <a name="sp_clean_db_free_space-transact-sql"></a>sp_clean_db_free_space (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita la información residual que queda en las páginas de base de datos a causa de las rutinas de modificación de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sp_clean_db_free_space limpia todas las páginas de todos los archivos de la base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql 
sp_clean_db_free_space   
  [ @dbname = ] 'database_name'   
  [ , [ @cleaning_delay = ] 'delay_in_seconds' ] [;]  
```  
  
## <a name="arguments"></a>Argumentos  
 @dbname = '*database_name*'  
 Es el nombre de la base de datos que se va a limpiar. *dbname* es de **tipo sysname** y no puede ser null.  
  
 @cleaning_delay = '*delay_in_seconds*'  
 Especifica un intervalo de retardo entre la limpieza de páginas. Esto ayuda a reducir el efecto en el sistema de E/S. *delay_in_seconds* es de **tipo int** y su valor predeterminado es 0.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Las operaciones de eliminación de una tabla o de las operaciones de actualización que hacen que una fila se mueva pueden liberar inmediatamente el espacio de una página quitando las referencias a la fila. Sin embargo, en determinadas circunstancias, la fila puede permanecer físicamente en la página de datos como un registro fantasma. Los registros fantasma se quitan periódicamente mediante un proceso en segundo plano. No devuelve estos datos residuales [!INCLUDE[ssDE](../../includes/ssde-md.md)] en respuesta a las consultas. Sin embargo, en entornos en los que la seguridad física de los datos o los archivos de copia de seguridad está en riesgo, puede utilizar `sp_clean_db_free_space` para limpiar estos registros fantasma. Para realizar esta operación por cada archivo de base de datos, use [sp_clean_db_file_free_space (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md). 
  
 El período de tiempo necesario para ejecutar sp_clean_db_free_space depende del tamaño del archivo, del espacio disponible y de la capacidad del disco. Dado `sp_clean_db_free_space` que la ejecución de puede afectar significativamente a la actividad de e/s, se recomienda ejecutar este procedimiento fuera de las horas de funcionamiento habituales.  
  
 Antes de ejecutar `sp_clean_db_free_space` , se recomienda crear una copia de seguridad completa de la base de datos.  
  
 El procedimiento almacenado de [sp_clean_db_file_free_space](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md) relacionado puede limpiar un solo archivo.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al `db_owner` rol de base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se limpia toda la información residual de la base de datos `AdventureWorks2012`.  
  
```sql  
USE master;  
GO  
EXEC sp_clean_db_free_space @dbname = N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Motor de base de datos procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Guía del proceso de limpieza de Ghost](../ghost-record-cleanup-process-guide.md)    
 [sp_clean_db_file_free_space (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
  
  
