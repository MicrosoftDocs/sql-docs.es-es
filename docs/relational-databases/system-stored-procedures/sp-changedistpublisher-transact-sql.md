---
description: sp_changedistpublisher (Transact-SQL)
title: sp_changedistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changedistpublisher_TSQL
- sp_changedistpublisher
helpviewer_keywords:
- sp_changedistpublisher
ms.assetid: 7ef5c89d-faaa-4f8e-aef7-00649ebc8bc9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d48757f7404ba4f915e7b92609c9077ab702adb4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159035"
---
# <a name="sp_changedistpublisher-transact-sql"></a>sp_changedistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Cambia las propiedades del publicador de distribución. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changedistpublisher [ @publisher = ] 'publisher'  
    [ , [ @property = ] 'property' ]  
    [ , [ @value = ] 'value' ]  
    [ , [ @storage_connection_string = ] 'storage_connection_string']
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @property = ] 'property'` Es una propiedad que se va a cambiar para el publicador especificado. *Property* es de **tipo sysname** y puede tener uno de estos valores.  
  
`[ @value = ] 'value'` Es el valor de la propiedad especificada. el *valor* es **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @storage_connection_string = ] 'storage_connection_string'` Se requiere para SQL Instancia administrada, debe coincidir con la clave de acceso del volumen de almacenamiento Azure SQL Database. 


 > [!INCLUDE[Azure SQL Database link](../../includes/azure-sql-db-repl-for-more-information.md)]
 
 En esta tabla se describen las propiedades de los publicadores y los valores de esas propiedades.  
  
|Propiedad|Valores|Descripción|  
|--------------|------------|-----------------|  
|**active**|**true**|Activa el publicador.|  
||**false**|Desactiva el publicador.|  
|**distribution_db**||Nombre de la base de datos de distribución.|  
|**login**||Nombre de inicio de sesión.|  
|**password**||Contraseña segura para el inicio de sesión que se ha proporcionado.|  
|**security_mode**|**1**|Se utiliza la autenticación de Windows para la conexión con el publicador. *No se puede cambiar para un no* [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *publicador.*|  
||**0**|Se utiliza la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la conexión con el publicador. *No se puede cambiar para un no* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *publicador.*|  
|**working_directory**||Directorio de trabajo utilizado para almacenar archivos de datos y esquemas para la publicación.|  
|NULL (predeterminado)||Se imprimen todas las opciones de *propiedad* disponibles.| 
|**storage_connection_string**| Clave de acceso | La clave de acceso para el directorio de trabajo cuando la base de datos es Azure SQL Instancia administrada. 
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changedistpublisher** se utiliza en todos los tipos de replicación.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_changedistpublisher**.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar las propiedades del distribuidor y del publicador](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
