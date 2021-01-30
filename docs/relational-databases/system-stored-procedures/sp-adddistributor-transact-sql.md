---
description: sp_adddistributor (Transact-SQL)
title: sp_adddistributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistributor
- sp_adddistributor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adddistributor
ms.assetid: 35415502-68d0-40f6-993c-180e50004f1e
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 2e84824fec7a45fcfea24a2d97ced622b2147f34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182803"
---
# <a name="sp_adddistributor-transact-sql"></a>sp_adddistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una entrada en la tabla [ servidores desys.sys](../../relational-databases/system-compatibility-views/sys-sysservers-transact-sql.md) (si no hay ninguna), marca la entrada del servidor como distribuidor y almacena la información de la propiedad. Este procedimiento almacenado se ejecuta en el distribuidor de la base de datos maestra para registrar y marcar el servidor como un distribuidor. En el caso de un distribuidor remoto, se ejecuta también en el publicador desde la base de datos maestra para registrar el distribuidor remoto.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_adddistributor [ @distributor= ] 'distributor'   
    [ , [ @heartbeat_interval= ] heartbeat_interval ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @from_scripting= ] from_scripting ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @distributor = ] 'distributor'` Es el nombre del servidor de distribución. *Distributor* es de **tipo sysname** y no tiene ningún valor predeterminado. Este parámetro se utiliza únicamente si se está configurando un distribuidor remoto. Agrega entradas para las propiedades del distribuidor en **msdb. Tabla MSdistributor** .  

> [!NOTE]
> El nombre del servidor se puede especificar como `<Hostname>,<PortNumber>` . Es posible que tenga que especificar el número de puerto para la conexión cuando SQL Server se implementa en Linux o Windows con un puerto personalizado, y el servicio explorador está deshabilitado.

`[ @heartbeat_interval = ] heartbeat_interval` Es el número máximo de minutos que un agente puede pasar sin registrar un mensaje de progreso. *heartbeat_interval* es de **tipo int** y su valor predeterminado es 10 minutos. Se crea un trabajo del Agente SQL Server que se ejecuta en este intervalo para comprobar el estado de los agentes de replicación que se están ejecutando.  
  
`[ @password = ] 'password']` Es la contraseña del inicio de sesión de **distributor_admin** . *password* es de **tipo sysname y su** valor predeterminado es NULL. Si es NULL o una cadena vacía, la contraseña se restablece a un valor aleatorio. La contraseña debe configurarse cuando se agrega el primer distribuidor remoto. **distributor_admin** inicio de sesión y la *contraseña* se almacenan para las entradas del servidor vinculado utilizadas para una conexión RPC del *distribuidor* , incluidas las conexiones locales. Si el *distribuidor* es local, la contraseña de **distributor_admin** se establece en un valor nuevo. En el caso de los publicadores con un distribuidor remoto, se debe especificar el mismo valor de *contraseña* al ejecutar **Sp_adddistributor** en el publicador y el distribuidor. [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) se puede usar para cambiar la contraseña del distribuidor.  
  
> [!IMPORTANT]  
>  Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_adddistributor** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistributor-transa_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_adddistributor**.  
  
## <a name="see-also"></a>Consulte también  
 [Configurar la publicación y la distribución](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributor_property &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_dropdistributor &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Configurar distribución](../../relational-databases/replication/configure-distribution.md)  
  
  
