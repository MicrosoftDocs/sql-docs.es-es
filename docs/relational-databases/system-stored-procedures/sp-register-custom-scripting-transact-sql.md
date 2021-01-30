---
description: sp_register_custom_scripting (Transact-SQL)
title: sp_register_custom_scripting (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_register_custom_scripting
- sp_register_custom_scripting_TSQL
helpviewer_keywords:
- sp_register_custom_scripting
ms.assetid: a8159282-de3b-4b9e-bdc9-3d3fce485c7f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f9239f89c5f65f0a4af3ddfd5588a92551dfbcdc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210452"
---
# <a name="sp_register_custom_scripting-transact-sql"></a>sp_register_custom_scripting (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La replicación permite que los procedimientos almacenados personalizados definidos por el usuario sustituyan a uno o más de los procedimientos predeterminados utilizados en la replicación transaccional. Cuando se realiza un cambio en el esquema en una tabla replicada, estos procedimientos almacenados se vuelven a crear. **sp_register_custom_scripting** registra un procedimiento almacenado o un [!INCLUDE[tsql](../../includes/tsql-md.md)] archivo de script que se ejecuta cuando se produce un cambio de esquema para crear un script para la definición de un nuevo procedimiento almacenado personalizado definido por el usuario. Este nuevo procedimiento almacenado personalizado definido por el usuario debe reflejar el nuevo esquema de la tabla. **sp_register_custom_scripting** se ejecuta en el publicador de la base de datos de publicación y el archivo de script o el procedimiento almacenado registrado se ejecuta en el suscriptor cuando se produce un cambio de esquema.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_register_custom_scripting [ @type  = ] 'type'  
    [ @value = ] 'value'   
    [ , [ @publication = ] 'publication' ]  
    [ , [ @article = ] 'article' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @type = ] 'type'` Es el tipo de procedimiento almacenado personalizado o el script que se está registrando. *Type* es de tipo **VARCHAR (16)**, no tiene ningún valor predeterminado y puede tener uno de los valores siguientes.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**insert**|El procedimiento almacenado personalizado registrado se ejecuta cuando se replica una instrucción INSERT.|  
|**update**|El procedimiento almacenado personalizado registrado se ejecuta cuando se replica una instrucción UPDATE.|  
|**delete**|El procedimiento almacenado personalizado registrado se ejecuta cuando se replica una instrucción DELETE.|  
|**custom_script**|El script se ejecuta al final del desencadenador de lenguaje de definición de datos (DDL).|  
  
`[ @value = ] 'value'` Nombre de un procedimiento almacenado o nombre y ruta de acceso completa al [!INCLUDE[tsql](../../includes/tsql-md.md)] archivo de script que se está registrando. el *valor* es **nvarchar (1024)** y no tiene ningún valor predeterminado.  
  
> [!NOTE]  
>  Si se especifica NULL para el parámetro de *valor*, se anulará el registro de un script registrado previamente, lo que equivale a ejecutar [sp_unregister_custom_scripting](../../relational-databases/system-stored-procedures/sp-unregister-custom-scripting-transact-sql.md).  
  
 Cuando el valor de *tipo* es **custom_script**, se espera el nombre y la ruta de acceso completa de un [!INCLUDE[tsql](../../includes/tsql-md.md)] archivo de script. De lo contrario, el *valor* debe ser el nombre de un procedimiento almacenado registrado.  
  
`[ @publication = ] 'publication'` Nombre de la publicación para la que se está registrando el procedimiento almacenado personalizado o el script. *Publication* es de **tipo sysname y su** valor predeterminado es **null**.  
  
`[ @article = ] 'article'` Nombre del artículo para el que se está registrando el procedimiento almacenado personalizado o el script. *article* es de **tipo sysname y su** valor predeterminado es **null**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_register_custom_scripting** se utiliza en la replicación de instantáneas y transaccional.  
  
 Se debe ejecutar este procedimiento almacenado antes de realizar un cambio en el esquema en una tabla replicada. Para obtener más información sobre el uso de este procedimiento almacenado, vea [regenerar procedimientos transaccionales personalizados para reflejar cambios de esquema](../../relational-databases/replication/transactional/transactional-articles-regenerate-to-reflect-schema-changes.md).  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , el rol fijo de base de datos **db_owner** o el rol fijo de base de datos **db_ddladmin** pueden ejecutar **sp_register_custom_scripting**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_unregister_custom_scripting &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unregister-custom-scripting-transact-sql.md)  
  
  
