---
description: sp_addsynctriggers (Transact-SQL)
title: sp_addsynctriggers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addsynctriggers_TSQL
- sp_addsynctriggers
helpviewer_keywords:
- sp_addsynctriggers
ms.assetid: e37d0c3b-19bf-4719-9535-96ba361372b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 00782a900c02976c06f2cfcdf6c9184d90e8059d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211948"
---
# <a name="sp_addsynctriggers-transact-sql"></a>sp_addsynctriggers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea desencadenadores en el suscriptor utilizado con todos los tipos de suscripciones actualizables (actualización inmediata, actualización en cola, actualización inmediata con actualización en cola como conmutación por error). Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
> [!IMPORTANT]  
>  Se debe utilizar el procedimiento [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) en lugar de **sp_addsynctrigger**. [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) genera un script que contiene las llamadas a **sp_addsynctrigger** .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addsynctriggers [ @sub_table = ] 'sub_table'  
        , [ @sub_table_owner = ] 'sub_table_owner'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @ins_proc = ] 'ins_proc'   
        , [ @upd_proc = ] 'upd_proc'   
        , [ @del_proc = ] 'del_proc'   
        , [ @cftproc = ] 'cftproc'  
        , [ @proc_owner = ] 'proc_owner'  
    [ , [ @identity_col = ] 'identity_col' ]  
    [ , [ @ts_col = ] 'timestamp_col' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]   
        , [ @primary_key_bitmap = ] 'primary_key_bitmap'  
    [ , [ @identity_support = ] identity_support ]  
    [ , [ @independent_agent = ] independent_agent ]  
        , [ @distributor = ] 'distributor'   
    [ , [ @pubversion = ] pubversion  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @sub_table = ] 'sub_table'` Es el nombre de la tabla del suscriptor. *sub_table* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @sub_table_owner = ] 'sub_table_owner'` Es el nombre del propietario de la tabla del suscriptor. *sub_table_owner* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher = ] 'publisher'` Es el nombre del servidor del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado. Si es NULL, se utiliza la base de datos actual.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @ins_proc = ] 'ins_proc'` Es el nombre del procedimiento almacenado que admite inserciones de transacciones sincrónicas en el publicador. *ins_proc* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @upd_proc = ] 'upd_proc'` Es el nombre del procedimiento almacenado que admite actualizaciones de transacciones sincrónicas en el publicador. *ins_proc* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @del_proc = ] 'del_proc'` Es el nombre del procedimiento almacenado que admite eliminaciones de transacciones sincrónicas en el publicador. *ins_proc* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @cftproc = ] 'cftproc'` Es el nombre del procedimiento generado automáticamente que usan las publicaciones que permiten la actualización en cola. *cftproc* es de **tipo sysname** y no tiene ningún valor predeterminado. En las publicaciones que permiten la actualización inmediata, este valor es NULL. Este parámetro se aplica a las publicaciones que permiten la actualización en cola (actualización en cola y actualización inmediata con actualización en cola como conmutación por error).  
  
`[ @proc_owner = ] 'proc_owner'` Especifica la cuenta de usuario en el publicador en la que se crearon todos los procedimientos almacenados generados automáticamente para la publicación de actualización (en cola o inmediato). *proc_owner* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @identity_col = ] 'identity_col'` Es el nombre de la columna de identidad en el publicador. *identity_col* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @ts_col = ] 'timestamp_col'` Es el nombre de la columna **timestamp** en el publicador. *timestamp_col* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @filter_clause = ] 'filter_clause'` Es una cláusula de restricción (WHERE) que define un filtro horizontal. Al especificar la cláusula de restricción, omita la palabra clave WHERE. *filter_clause* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL.  
  
`[ @primary_key_bitmap = ] 'primary_key_bitmap'` Es un mapa de bits de las columnas de clave principal de la tabla. *primary_key_bitmap* es **varbinary (4000)** y no tiene ningún valor predeterminado.  
  
`[ @identity_support = ] identity_support` Habilita y deshabilita el control automático de intervalos de identidad cuando se utiliza la actualización en cola. *identity_support* es de **bit** y su valor predeterminado es **0**. **0** significa que no hay compatibilidad con el intervalo de identidad, **1** habilita el control automático del intervalo de identidad.  
  
`[ @independent_agent = ] independent_agent` Indica si hay un único Agente de distribución (un agente independiente) para esta publicación, o una Agente de distribución por cada base de datos de publicación y par de base de datos de suscripciones (un agente compartido). Este valor refleja el valor de la propiedad independent_agent de la publicación definida en el publicador. *independent_agent* es un bit con un valor predeterminado de **0**. Si es **0**, el agente es un agente compartido. Si es **1**, el agente es un agente independiente.  
  
`[ @distributor = ] 'distributor'` Es el nombre del distribuidor. *Distributor* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @pubversion = ] pubversion` Indica la versión del publicador. *pubversion* es de **tipo int** y su valor predeterminado es 1. **1** significa que la versión del publicador es [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 2 o anterior; **2** significa que el publicador es [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 3 (SP3) o posterior. *pubversion* debe establecerse explícitamente en **2** cuando la versión del publicador sea [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] SP3 o posterior.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 el Agente de distribución usa **sp_addsynctriggers** como parte de la inicialización de la suscripción. Este procedimiento almacenado no lo suelen ejecutar los usuarios, pero puede resultar útil si el usuario debe configurar manualmente una suscripción de tipo no sync.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addsynctriggers**.  
  
## <a name="see-also"></a>Consulte también  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
