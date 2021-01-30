---
description: sp_adddistributiondb (Transact-SQL)
title: sp_adddistributiondb (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/30/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistributiondb_TSQL
- sp_adddistributiondb
helpviewer_keywords:
- sp_adddistributiondb
ms.assetid: e9bad56c-d2b3-44ba-a4d7-ff2fd842e32d
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: a5a80b39ca68abd37f3e9e843c2e76e8e71669d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182817"
---
# <a name="sp_adddistributiondb-transact-sql"></a>sp_adddistributiondb (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una nueva base de datos de distribución e instala el esquema del distribuidor. La base de datos de distribución almacena los procedimientos, esquema y metadatos utilizados en la replicación. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos maestra con el fin de crear la base de datos de distribución e instalar las tablas y procedimientos almacenados necesarios para habilitar la distribución de replicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_adddistributiondb [ @database= ] 'database'   
    [ , [ @data_folder= ] 'data_folder' ]   
    [ , [ @data_file= ] 'data_file' ]   
    [ , [ @data_file_size= ] data_file_size ]   
    [ , [ @log_folder= ] 'log_folder' ]   
    [ , [ @log_file= ] 'log_file' ]   
    [ , [ @log_file_size= ] log_file_size ]   
    [ , [ @min_distretention= ] min_distretention ]   
    [ , [ @max_distretention= ] max_distretention ]   
    [ , [ @history_retention= ] history_retention ]   
    [ , [ @security_mode= ] security_mode ]   
    [ , [ @login= ] 'login' ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @createmode= ] createmode ]  
    [ , [ @from_scripting = ] from_scripting ] 
    [ , [ @deletebatchsize_xact = ] deletebatchsize_xact ] 
    [ , [ @deletebatchsize_cmd = ] deletebatchsize_cmd ] 
```  
  
## <a name="arguments"></a>Argumentos  
`[ @database = ] database'` Es el nombre de la base de datos de distribución que se va a crear. *Database* es de **tipo sysname** y no tiene ningún valor predeterminado. Si la base de datos especificada ya existe y no está ya marcada como base de datos de distribución, entonces los objetos necesarios para habilitar la distribución están instalados y la base de datos está marcada como base de datos de distribución. Si la base de datos especificada ya está habilitada como base de datos de distribución, se obtiene un error.  
  
`[ @data_folder = ] 'data_folder'_` Es el nombre del directorio que se utiliza para almacenar el archivo de datos de la base de datos de distribución. *data_folder* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Si es NULL, se utiliza el directorio de datos para esa instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , por ejemplo, `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data` .  
  
`[ @data_file = ] 'data_file'` Es el nombre del archivo de base de datos. *data_file* es de tipo **nvarchar (255)** y su valor predeterminado es **Database**. Si es NULL, el procedimiento almacenado crea un nombre de archivo que utiliza el nombre de la base de datos.  
  
`[ @data_file_size = ] data_file_size` Es el tamaño inicial del archivo de datos en megabytes (MB). *data_file_size i* s **int**, con un valor predeterminado de 5 MB.  
  
`[ @log_folder = ] 'log_folder'` Es el nombre del directorio del archivo de registro de la base de datos. *log_folder* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Si es NULL, se utiliza el directorio de datos para esa instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (por ejemplo, `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data`).  
  
`[ @log_file = ] 'log_file'` Es el nombre del archivo de registro. *log_file* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Si es NULL, el procedimiento almacenado crea un nombre de archivo que utiliza el nombre de la base de datos.  
  
`[ @log_file_size = ] log_file_size` Es el tamaño inicial del archivo de registro en megabytes (MB). *log_file_size* es de **tipo int** y su valor predeterminado es 0 MB, lo que significa que el tamaño del archivo se crea con el tamaño de archivo de registro más pequeño permitido por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
`[ @min_distretention = ] min_distretention` Es el período de retención mínimo, en horas, antes de que se eliminen las transacciones de la base de datos de distribución. *min_distretention* es de **tipo int** y su valor predeterminado es 0 horas.  
  
`[ @max_distretention = ] max_distretention` Es el período de retención máximo, en horas, antes de que se eliminen las transacciones. *max_distretention* es de **tipo int** y su valor predeterminado es de 72 horas. Las suscripciones que no han recibido comandos replicados más antiguos que el período máximo de retención de la distribución se marcarán como inactivas y tendrán que reinicializarse. Se emite RAISERROR 21011 por cada suscripción inactiva. Un valor de **0** significa que las transacciones replicadas no se almacenan en la base de datos de distribución.  
  
`[ @history_retention = ] history_retention` Es el número de horas que se va a conservar el historial. *history_retention* es de **tipo int** y su valor predeterminado es de 48 horas.  
  
`[ @security_mode = ] security_mode` Es el modo de seguridad que se va a utilizar al conectarse al distribuidor. *security_mode* es de **tipo int** y su valor predeterminado es 1. **0** especifica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación; **1** especifica la autenticación integrada de Windows.  
  
`[ @login = ] 'login'` Es el nombre de inicio de sesión que se usa al conectarse al distribuidor para crear la base de datos de distribución. Esto es necesario si *security_mode* está establecido en **0**. *login* es de tipo **sysname** y su valor predeterminado es NULL.  
  
`[ @password = ] 'password'` Es la contraseña utilizada para conectarse al distribuidor. Esto es necesario si *security_mode* está establecido en **0**. *password* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @createmode = ] createmode`*createmode* es de **tipo int**, su valor predeterminado es 1 y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**1** (predeterminado)|CREAR base de datos o usar base de datos existente y, a continuación, aplicar el archivo **instdist. SQL** para crear objetos de replicación en la base de datos de distribución.|  
|**2**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
 
`[ @deletebatchsize_xact = ] deletebatchsize_xact` Especifica el tamaño del lote que se va a usar durante la limpieza de transacciones expiradas de las tablas de MSRepl_Transactions. *deletebatchsize_xact* es de **tipo int** y su valor predeterminado es 5000. Este parámetro se presentó por primera vez en SQL Server 2017, seguido de las versiones en SQL Server 2012 SP4 y SQL Server 2016 SP2.  

`[ @deletebatchsize_cmd = ] deletebatchsize_cmd` Especifica el tamaño del lote que se va a utilizar durante la limpieza de los comandos expirados de las tablas de MSRepl_Commands. *deletebatchsize_cmd* es de **tipo int** y su valor predeterminado es 2000. Este parámetro se presentó por primera vez en SQL Server 2017, seguido de las versiones en SQL Server 2012 SP4 y SQL Server 2016 SP2. 
 
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_adddistributiondb** se utiliza en todos los tipos de replicación. Sin embargo, este procedimiento almacenado se ejecuta únicamente en un distribuidor.  
  
 Debe configurar el distribuidor ejecutando [sp_adddistributor](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) antes de ejecutar **sp_adddistributiondb**.  
  
 Ejecute **sp_adddistributor** antes de ejecutar **sp_adddistributiondb**.  
  
## <a name="example"></a>Ejemplo  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
-- Specify the replication working directory.  
SET @directory = N'\\' + $(DistPubServer) + '\repldata';  
-- Specify the publication database.  
SET @publicationDB = N'AdventureWorks2012';   
  
-- Install the server MYDISTPUB as a Distributor using the defaults,  
-- including autogenerating the distributor password.  
USE master  
EXEC sp_adddistributor @distributor = @distributor;  
  
-- Create a new distribution database using the defaults, including  
-- using Windows Authentication.  
USE master  
EXEC sp_adddistributiondb @database = @distributionDB,   
    @security_mode = 1;  
GO  
  
-- Create a Publisher and enable AdventureWorks2012 for replication.  
-- Add MYDISTPUB as a publisher with MYDISTPUB as a local distributor  
-- and use Windows Authentication.  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
USE [distribution]  
EXEC sp_adddistpublisher @publisher=@publisher,   
    @distribution_db=@distributionDB,   
    @security_mode = 1;  
GO  
  
```  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_adddistributiondb**.  
  
## <a name="see-also"></a>Consulte también  
 [Configurar la publicación y la distribución](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributiondb &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changedistributiondb-transact-sql.md)   
 [sp_dropdistributiondb &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md)   
 [sp_helpdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurar distribución](../../relational-databases/replication/configure-distribution.md)  
  
  
