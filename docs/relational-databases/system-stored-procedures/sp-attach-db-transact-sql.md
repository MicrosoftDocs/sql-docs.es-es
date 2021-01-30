---
description: sp_attach_db (Transact-SQL)
title: sp_attach_db (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_attach_db_TSQL
- sp_attach_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_attach_db
ms.assetid: 59bc993e-7913-4091-89cb-d2871cffda95
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cb1d6447fb73174817fe8105c3dea1866d09b789
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171776"
---
# <a name="sp_attach_db-transact-sql"></a>sp_attach_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Adjunta una base de datos a un servidor.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, se recomienda usar CREATE DATABASE *database_name* para Attach. Para obtener más información, vea [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
> [!NOTE]  
>  Para volver a generar varios archivos de registro cuando uno o varios tengan una nueva ubicación, utilice CREATE DATABASE *database_name* for ATTACH_REBUILD_LOG.  
  
> [!IMPORTANT]  
>  Se recomienda no adjuntar ni restaurar bases de datos de orígenes desconocidos o que no sean de confianza. Es posible que dichas bases de datos contengan código malintencionado que podría ejecutar código [!INCLUDE[tsql](../../includes/tsql-md.md)] no deseado o provocar errores al modificar el esquema o la estructura de la base de datos física. Para usar una base de datos desde un origen desconocido o que no sea de confianza, ejecute [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) en la base de datos de un servidor que no sea de producción y examine también el código, como procedimientos almacenados u otro código definido por el usuario, en la base de datos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_attach_db [ @dbname= ] 'dbname'  
    , [ @filename1= ] 'filename_n' [ ,...16 ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @dbname = ] 'dbnam_ '` Es el nombre de la base de datos que se va a adjuntar al servidor. El nombre debe ser único. *dbname* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @filename1 = ] 'filename_n'` Es el nombre físico, incluida la ruta de acceso, de un archivo de base de datos. *filename_n* es de tipo **nvarchar (260)** y su valor predeterminado es NULL. Se pueden especificar hasta 16 nombres de archivo. Los nombres de parámetro comienzan en **\@ nombreDeArchivo1** y se incrementan a **\@ filename16**. La lista de nombres de archivo debe incluir al menos el archivo principal. El archivo principal contiene las tablas del sistema que señalan a otros archivos de la base de datos. La lista también debe contener los archivos que se hayan movido después de separar la base de datos.  
  
> [!NOTE]  
>  Este argumento se asigna al parámetro FILENAME de la instrucción CREATE DATABASE. Para obtener más información, vea [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md).  
>   
>  Al adjuntar una base de datos de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] que contiene los archivos de catálogo de texto completo a una instancia del servidor de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , los archivos de catálogo se adjuntan de su ubicación anterior junto con los demás archivos de base de datos, igual que en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Para obtener más información, vea [Actualizar la búsqueda de texto completo](../../relational-databases/search/upgrade-full-text-search.md).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 El **sp_attach_db** procedimiento almacenado solo debe ejecutarse en bases de datos que se hayan desasociado previamente del servidor de base de datos mediante una operación de **sp_detach_db** explícita o en bases de datos copiadas. Si tiene que especificar más de 16 archivos, use crear *database_name* de base de datos para adjuntar o crear base de datos *database_name* FOR_ATTACH_REBUILD_LOG. Para obtener más información, vea [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
 Se da por supuesto que cualquier archivo que no se especifique se encuentra en su última ubicación conocida. Para utilizar un archivo que se encuentra en una ubicación diferente, debe especificar la nueva ubicación.  
  
 Una base de datos creada por una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede adjuntarse en versiones anteriores.  
  
> [!NOTE]  
>  No puede separar ni adjuntar una instantánea de base de datos.  
  
 Si adjunta una base de datos replicada que fue copiada en lugar de ser separada, tenga en cuenta lo siguiente:  
  
-   Si adjunta la base de datos a la misma versión e instancia de servidor que la base de datos original, no es necesario realizar ningún paso adicional.  
  
-   Si adjunta la base de datos a la misma instancia de servidor pero con una versión actualizada, debe ejecutar [sp_vupgrade_replication](../../relational-databases/system-stored-procedures/sp-vupgrade-replication-transact-sql.md) para actualizar la replicación una vez que se complete la operación de adjuntar.  
  
-   Si adjunta la base de datos a una instancia de servidor diferente, independientemente de la versión, debe ejecutar [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) para quitar la replicación una vez que se complete la operación de adjuntar.  
  
 La primera vez que se adjunta una base de datos o se restaura en una instancia nueva de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], aún no se ha almacenado una copia de la clave maestra de la base de datos (cifrada por la clave maestra de servicio) en el servidor. Debe usar la instrucción **OPEN MASTER KEY** para descifrar la clave maestra de la base de datos (DMK). Una vez que se ha descifrado la clave maestra de la base de datos, tiene la posibilidad de habilitar el descifrado automático en el futuro usando la instrucción **ALTER MASTER KEY REGENERATE** para proporcionar al servidor una copia de la clave maestra de la base de datos cifrada con la clave maestra de servicio (SMK). Cuando una base de datos se haya actualizado desde una versión anterior, se debe volver a generar la DMK para usar el algoritmo AES más reciente. Para obtener más información sobre cómo volver a generar la DMK, vea [ALTER MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-master-key-transact-sql.md). El tiempo necesario para volver a generar la DMK con el fin de actualizarse a AES depende del número de objetos protegidos por la DMK. Solo es necesario volver a generar la DMK una vez y no tiene ningún efecto sobre las nuevas generaciones futuras como parte de una estrategia de rotación de claves.  
  
## <a name="permissions"></a>Permisos  
 Para obtener información sobre cómo se administran los permisos cuando se adjunta una base de datos, vea [Create database &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se adjuntan archivos de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] al servidor actual.  
  
```  
EXEC sp_attach_db @dbname = N'AdventureWorks2012',   
    @filename1 =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_Data.mdf',   
    @filename2 =   
N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_log.ldf';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Adjuntar y separar bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_detach_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)   
 [sp_helpfile &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpfile-transact-sql.md)   
 [sp_removedbreplication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
