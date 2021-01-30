---
description: sp_repladdcolumn (Transact-SQL)
title: sp_repladdcolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repladdcolumn_TSQL
- sp_repladdcolumn
helpviewer_keywords:
- sp_repladdcolumn
ms.assetid: d6220f9f-c738-4f9c-bcf8-419994e86c81
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3579b68a229bd939be61bdb233e5d429dd0ca4c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193540"
---
# <a name="sp_repladdcolumn-transact-sql"></a>sp_repladdcolumn (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Agrega una columna a un artículo de tabla existente que ha sido publicado. Permite agregar la nueva columna a todos los publicadores que publican esta tabla o, simplemente, agregar la columna a una publicación específica que publica la tabla. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
> [!IMPORTANT]
>  Este procedimiento almacenado ha quedado desusado y se admite por cuestiones de compatibilidad con las versiones anteriores. Solo se debe usar con [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] publicadores y [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] suscriptores de republicación. Este procedimiento no se debería utilizar en columnas con tipos de datos introducidos en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o posterior.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_repladdcolumn [ @source_object = ] 'source_object', [ @column = ] 'column' ]  
    [ , [ @typetext = ] 'typetext' ]  
    [ , [ @publication_to_add = ] 'publication_to_add' ]  
    [ , [ @from_agent = ] from_agent ]  
    [ , [ @schema_change_script = ] 'schema_change_script' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @source_object =] '*source_object*'  
 Es el nombre del artículo de la tabla que contiene la nueva columna que se va a agregar. *source_object* es de tipo **nvarchar (358**) y no tiene ningún valor predeterminado.  
  
 [ @column =] '*columna*'  
 Es el nombre de la columna de la tabla que se va a agregar para replicación. la *columna* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
 [ @typetext = ] '*TypeText*'  
 Es la definición de la columna que se va a agregar. *TypeText* es de tipo **nvarchar (3000)** y no tiene ningún valor predeterminado. Por ejemplo, si se agrega la columna order_filled, y es un campo de un solo carácter, no NULL, y tiene un valor predeterminado de **N**, order_filled sería el parámetro de *columna* , mientras que la definición de la columna, la **restricción Char (1) not null constraint_name valor predeterminado ' N '** sería el valor del parámetro *TypeText* .  
  
 [ @publication_to_add =] '*publication_to_add*'  
 Es el nombre de la publicación a la que se agrega la nueva columna. *publication_to_add* es de tipo **nvarchar (4000)** y su valor predeterminado es **All**. Si es **All**, se ven afectadas todas las publicaciones que contienen esta tabla. Si se especifica *publication_to_add* , solo se agregará la nueva columna a esta publicación.  
  
 [ @from_agent =] *from_agent*  
 Si un agente de replicación está ejecutando el procedimiento almacenado. *from_agent* es de **tipo int** y su valor predeterminado es **0**, donde un valor de **1** se utiliza cuando un agente de replicación está ejecutando este procedimiento almacenado y, en cualquier otro caso, se debe usar el valor predeterminado de **0**.  
  
 [ @schema_change_script =] '*schema_change_script*'  
 Especifica el nombre y la ruta de acceso de un script de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizado para modificar los procedimientos almacenados personalizados generados por el sistema. *schema_change_script* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL. La replicación permite que los procedimientos almacenados personalizados definidos por el usuario sustituyan a uno o más de los procedimientos predeterminados utilizados en la replicación transaccional. *schema_change_script* se ejecuta después de realizar un cambio de esquema en un artículo de tabla replicada mediante sp_repladdcolumn y se puede usar para realizar una de las acciones siguientes:  
  
-   Si los procedimientos almacenados personalizados se vuelven a generar automáticamente, *schema_change_script* se pueden utilizar para quitar estos procedimientos almacenados personalizados y reemplazarlos con procedimientos almacenados personalizados definidos por el usuario que admitan el nuevo esquema.  
  
-   Si los procedimientos almacenados personalizados no se vuelven a generar automáticamente, *schema_change_script* se pueden usar para volver a generar estos procedimientos almacenados o para crear procedimientos almacenados personalizados definidos por el usuario.  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 Habilita o deshabilita la capacidad de que se invalide una instantánea. *force_invalidate_snapshot* es un **bit**, con un valor predeterminado de **1**.  
  
 **1** especifica que los cambios en el artículo pueden hacer que la instantánea no sea válida y, en ese caso, el valor **1** concede permiso para que se produzca la nueva instantánea.  
  
 **0** especifica que los cambios en el artículo no hacen que la instantánea no sea válida.  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 Habilita o deshabilita la capacidad de reinicializar la suscripción. *force_reinit_subscription* es un **bit** con un valor predeterminado de **0**.  
  
 **0** especifica que los cambios en el artículo no harán que se reinicialice la suscripción.  
  
 **1** especifica que los cambios en el artículo pueden hacer que la suscripción se reinicialice y, en tal caso, el valor **1** concede permiso para que se produzca la reinicialización de la suscripción.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor sysadmin y del rol fijo de base de datos db_owner pueden ejecutar sp_repladdcolumn.  
  
## <a name="see-also"></a>Consulte también  
 [Características desusadas en Replicación de SQL Server](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
