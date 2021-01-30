---
description: sp_syscollector_update_collection_set (Transact-SQL)
title: sp_syscollector_update_collection_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_update_collection_set_TSQL
- sp_syscollector_update_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_update_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 2dccc3cd-0e93-4e3e-a4e5-8fe89b31bd63
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 207d5b2f02bce0a3c9a9e89cdccdb5fe80790f20
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161888"
---
# <a name="sp_syscollector_update_collection_set-transact-sql"></a>sp_syscollector_update_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Se usa para modificar las propiedades o el nombre de un conjunto de recopilación definido por el usuario.  
  
> [!WARNING]  
>  En los casos en que la cuenta de Windows configurada como proxy pertenezca a un usuario no interactivo o interactivo que aún no se ha conectado, el directorio del perfil no existirá y se producirá un error al crear el directorio de almacenamiento temporal. Por tanto, si utiliza una cuenta de proxy en un controlador de dominio, debe especificar una cuenta interactiva que se haya utilizado al menos una vez para asegurarse de que se ha creado el directorio del perfil.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syscollector_update_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @new_name = ] 'new_name' ]  
    , [ [ @target = ] 'target' ]  
    , [ [ @collection_mode = ] collection_mode ]  
    , [ [ @days_until_expiration = ] days_until_expiration ]  
    , [ [ @proxy_id = ] proxy_id ]  
    , [ [ @proxy_name = ] 'proxy_name' ]  
    ,[ [ @schedule_uid = ] 'schedule_uid' ]  
    ,[ [ @schedule_name = ] 'schedule_uid' ]  
    , [ [ @logging_level = ] logging_level ]  
    , [ [ @description = ] 'description' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @collection_set_id = ] collection_set_id` Es el identificador local único para el conjunto de recopilación. *collection_set_id* es de **tipo int** y debe tener un valor si *el nombre* es NULL.  
  
`[ @name = ] 'name'` Es el nombre del conjunto de recopilación. *Name* es de **tipo sysname** y debe tener un valor si *collection_set_id* es NULL.  
  
`[ @new_name = ] 'new_name'` Es el nuevo nombre del conjunto de recopilación. *new_name* es de **tipo sysname** y, si se utiliza, no puede ser una cadena vacía. *new_name* debe ser único. Para obtener una lista de los nombres de conjuntos de recopilación actuales, consulte la vista del sistema syscollector_collection_sets.  
  
`[ @target = ] 'target'` Reservado para uso futuro.  
  
`[ @collection_mode = ] collection_mode` Es el tipo de recopilación de datos que se va a usar. *collection_mode* es **smallint** y puede tener uno de los valores siguientes:  
  
 0 - Modo de almacenamiento en caché. La recopilación y la carga de datos están en programaciones independientes. Especifique el modo de almacenamiento en caché para la recopilación continua.  
  
 1 - Modo sin almacenamiento en caché. La recopilación y la carga de datos están en la misma programación. Establezca el modo sin almacenamiento en caché para la recopilación ad hoc o la recopilación de instantáneas.  
  
 Si se cambia del modo sin almacenamiento en caché al modo de almacenamiento en caché (0), también se debe especificar *schedule_uid* o *schedule_name*.  
  
`[ @days_until_expiration = ] days_until_expiration` Es el número de días que los datos recopilados se guardan en el almacén de administración de datos. *days_until_expiration* es **smallint**. *days_until_expiration* debe ser 0 o un entero positivo.  
  
`[ @proxy_id = ] proxy_id` Es el identificador único de una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuenta de proxy del agente. *proxy_id* es de **tipo int**.  
  
`[ @proxy_name = ] 'proxy_name'` Es el nombre del proxy. *proxy_name* es **sysname** y admite valores NULL.  
  
`[ @schedule_uid = ] 'schedule_uid'` Es el GUID que apunta a una programación. *schedule_uid* es de tipo **uniqueidentifier**.  
  
 Para obtener *schedule_uid*, consulte la tabla del sistema sysschedules.  
  
 Cuando *collection_mode* se establece en 0, se deben especificar *schedule_uid* o *schedule_name* . Cuando *collection_mode* está establecido en 1, *schedule_uid* o *schedule_name* se omite si se especifica.  
  
`[ @schedule_name = ] 'schedule_name'` Es el nombre de la programación. *schedule_name* es **sysname** y admite valores NULL. Si se especifica, *schedule_uid* debe ser null. Para obtener *schedule_name*, consulte la tabla del sistema sysschedules.  
  
`[ @logging_level = ] logging_level` Es el nivel de registro. *logging_level* es **smallint** con uno de los siguientes valores:  
  
 0: registrar la información de ejecución y [!INCLUDE[ssIS](../../includes/ssis-md.md)] los eventos que realizan el seguimiento:  
  
-   Iniciar/detener los conjuntos de recopilaciones  
  
-   Iniciar/detener los paquetes  
  
-   Información de error  
  
 1: registro de nivel 0 y:  
  
-   Estadísticas de ejecución  
  
-   Progreso de recopilaciones que se ejecutan continuamente  
  
-   Eventos de advertencia de [!INCLUDE[ssIS](../../includes/ssis-md.md)]  
  
 2 - nivel de registro 1 e información detallada de eventos de [!INCLUDE[ssIS](../../includes/ssis-md.md)].  
  
 El valor predeterminado para *logging_level* es 1.  
  
`[ @description = ] 'description'` Es la descripción del conjunto de recopilación. la *Descripción* es **nvarchar (4000)**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 sp_syscollector_update_collection_set se debe ejecutar en el contexto de la base de datos del sistema msdb.  
  
 *Collection_set_id* o *Name* deben tener un valor, ambos no pueden ser null. Para obtener estos valores, consulte la vista del sistema syscollector_collection_sets.  
  
 Si el conjunto de recopilación se está ejecutando, solo puede actualizar *schedule_uid* y la *Descripción*. Para detener el conjunto de recopilación, use [sp_syscollector_stop_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-stop-collection-set-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia a un rol fijo de base de datos dc_admin o dc_operator (con permiso EXECUTE) para ejecutar este procedimiento. Aunque dc_operator puede ejecutar este procedimiento almacenado, las propiedades que pueden cambiar los miembros de este rol son limitadas. Las propiedades siguientes solo puede cambiarlas dc_admin:  
  
-   @new_name  
  
-   @target  
  
-   @proxy_id  
  
-   @description  
  
-   @collection_mode  
  
-   @days_until_expiration  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-renaming-a-collection-set"></a>A. Cambiar el nombre de un conjunto de recopilación  
 En el ejemplo siguiente se cambia el nombre de un conjunto de recopilación definido por el usuario.  
  
```  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_update_collection_set  
@name = N'Simple collection set test 1',  
@new_name = N'Collection set test 1 in cached mode';  
GO  
```  
  
### <a name="b-changing-the-collection-mode-from-non-cached-to-cached"></a>B. Cambiar el modo de recopilación de sin almacenamiento en caché al modo de almacenamiento en caché  
 En el ejemplo siguiente se cambia el modo de recopilación de sin almacenamiento en caché al modo de almacenamiento en caché. Este cambio requiere que se especifique un identificador o un nombre para la programación.  
  
```  
USE msdb;  
GO  
EXECUTE dbo.sp_syscollector_update_collection_set  
@name = N'Collection set test 1 in cached mode',  
@collection_mode = 0,  
@schedule_uid = 'C7022AF3-51B8-4011-B159-64C47C88FF70';  
-- alternatively, use @schedule_name.  
-- @schedule_name = N'CollectorSchedule_Every_15min;  
GO  
```  
  
### <a name="c-changing-other-collection-set-parameters"></a>C. Cambiar otros parámetros del conjunto de recopilación  
 En el ejemplo siguiente se actualizan varias propiedades del conjunto de recopilación denominado "Simple collection set test 2'.  
  
```  
USE msdb;  
GO  
EXEC dbo.sp_syscollector_update_collection_set  
@name = N'Simple collection set test 2',  
@collection_mode = 1,  
@days_until_expiration = 5,  
@description = N'This is a test collection set that runs in noncached mode.',  
@logging_level = 0;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)   
 [syscollector_collection_sets &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)   
 [ Programaciones dedbo.sys&#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
  
  
