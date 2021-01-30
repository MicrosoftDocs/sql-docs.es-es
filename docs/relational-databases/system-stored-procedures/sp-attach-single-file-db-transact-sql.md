---
description: sp_attach_single_file_db (Transact-SQL)
title: sp_attach_single_file_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_attach_single_file_db
- sp_attach_single_file_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_attach_single_file_db
ms.assetid: 13bd1044-9497-4293-8390-1f12e6b8e952
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e8fd050ff5706cc2406fcb4502c5d93a8253bc4c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197705"
---
# <a name="sp_attach_single_file_db-transact-sql"></a>sp_attach_single_file_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Adjunta al servidor actual una base de datos que tiene un solo archivo de datos. no se puede usar **sp_attach_single_file_db** con varios archivos de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, se recomienda usar CREATE DATABASE *database_name* para Attach. Para obtener más información, vea [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md). No utilice este procedimiento en una base de datos replicada.  
  
> [!IMPORTANT]  
>  Se recomienda no adjuntar ni restaurar bases de datos de orígenes desconocidos o que no sean de confianza. Es posible que dichas bases de datos contengan código malintencionado que podría ejecutar código [!INCLUDE[tsql](../../includes/tsql-md.md)] no deseado o provocar errores al modificar el esquema o la estructura de la base de datos física. Para usar una base de datos desde un origen desconocido o que no sea de confianza, ejecute [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) en la base de datos de un servidor que no sea de producción y examine también el código, como procedimientos almacenados u otro código definido por el usuario, en la base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_attach_single_file_db [ @dbname= ] 'dbname'  
    , [ @physname= ] 'physical_name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @dbname = ] 'dbname'` Es el nombre de la base de datos que se va a adjuntar al servidor. El nombre debe ser único. *dbname* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @physname = ] 'physical_name'` Es el nombre físico, incluida la ruta de acceso, del archivo de base de datos. *physical_name* es de tipo **nvarchar (260)** y su valor predeterminado es NULL.  
  
> [!NOTE]  
>  Este argumento se asigna al parámetro FILENAME de la instrucción CREATE DATABASE. Para obtener más información, vea [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
 Al adjuntar una base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] que contiene los archivos de catálogo de texto completo a una instancia del servidor de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , los archivos de catálogo se adjuntan de su ubicación anterior junto con los demás archivos de base de datos, igual que en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Para obtener más información, vea [Actualizar la búsqueda de texto completo](../../relational-databases/search/upgrade-full-text-search.md).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Utilice **sp_attach_single_file_db** solo en las bases de datos que se hayan desasociado previamente del servidor mediante una operación de **sp_detach_db** explícita o en bases de datos copiadas.  
  
 **sp_attach_single_file_db** solo funciona en las bases de datos que tienen un único archivo de registro. Cuando **sp_attach_single_file_db** adjunta la base de datos al servidor, crea un nuevo archivo de registro. Si la base de datos es de solo lectura, el archivo de registro se crea en su ubicación anterior.  
  
> [!NOTE]  
>  No puede separar ni adjuntar una instantánea de base de datos.  
  
 No utilice este procedimiento en una base de datos replicada.  
  
## <a name="permissions"></a>Permisos  
 Para obtener información sobre cómo se administran los permisos cuando se adjunta una base de datos, vea [Create database &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se separa [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y, después, se adjunta un archivo de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] al servidor actual.  
  
```  
USE master;  
GO  
EXEC sp_detach_db @dbname = 'AdventureWorks2012';  
EXEC sp_attach_single_file_db @dbname = 'AdventureWorks2012',   
    @physname =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_Data.mdf';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Adjuntar y separar bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
