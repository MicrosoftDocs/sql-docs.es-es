---
description: sp_changesubscriptiondtsinfo (Transact-SQL)
title: sp_changesubscriptiondtsinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changesubscriptiondtsinfo
- sp_changesubscriptiondtsinfo_TSQL
helpviewer_keywords:
- sp_changesubscriptiondtsinfo
ms.assetid: 64fc085f-f81b-493b-b59a-ee6192d9736d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 63762571085653afea03470cfc1e453a85e8adad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194418"
---
# <a name="sp_changesubscriptiondtsinfo-transact-sql"></a>sp_changesubscriptiondtsinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia las propiedades del paquete de Servicios de transformación de datos (DTS) de una suscripción. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changesubscriptiondtsinfo [ [ @job_id = ] job_id ]  
    [ , [ @dts_package_name= ] 'dts_package_name' ]  
    [ , [ @dts_package_password= ] 'dts_package_password' ]  
    [ , [ @dts_package_location= ] 'dts_package_location' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @job_id = ] job_id` Es el ID. de trabajo de la Agente de distribución para la suscripción de extracción. *job_id* es **varbinary (16)** y no tiene ningún valor predeterminado. Para buscar el identificador de trabajo de distribución, ejecute **sp_helpsubscription** o **sp_helppullsubscription**.  
  
`[ @dts_package_name = ] 'dts_package_name'` Especifica el nombre del paquete DTS. *dts_package_name* es de **tipo sysname y su** valor predeterminado es NULL. Por ejemplo, para especificar un paquete denominado **DTSPub_Package**, debe especificar `@dts_package_name = N'DTSPub_Package'` .  
  
`[ @dts_package_password = ] 'dts_package_password'` Especifica la contraseña del paquete. *dts_package_password* es de **tipo sysname y su** valor predeterminado es null, lo que especifica que la propiedad de contraseña se dejará sin cambios.  
  
> [!NOTE]  
>  Un paquete DTS debe tener una contraseña.  
  
`[ @dts_package_location = ] 'dts_package_location'` Especifica la ubicación del paquete. *dts_package_location* es de tipo **nvarchar (12)** y su valor predeterminado es null, que especifica que la ubicación del paquete se dejará sin cambios. La ubicación del paquete se puede cambiar al **distribuidor** o al **suscriptor**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changesubscriptiondtsinfo** se usa para la replicación de instantáneas y la replicación transaccional que solo son suscripciones de extracción.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , del rol fijo de base de datos **db_owner** o del creador de la suscripción pueden ejecutar **sp_changesubscriptiondtsinfo**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
