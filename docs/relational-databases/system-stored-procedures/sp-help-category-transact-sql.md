---
description: sp_help_category (Transact-SQL)
title: sp_help_category (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_category
- sp_help_category_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_category
ms.assetid: 8cad1dcc-b43e-43bd-bea0-cb0055c84169
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 68c8d1fcf243de008f889d4037bbd38a590c6403
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208931"
---
# <a name="sp_help_category-transact-sql"></a>sp_help_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información acerca de las clases especificadas de trabajos, alertas u operadores.  
   
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_category [ [ @class = ] 'class' ]   
     [ , [ @type = ] 'type' ]   
     [ , [ @name = ] 'name' ]   
     [ , [ @suffix = ] suffix ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @class = ] 'class'` Clase sobre la que se solicita información. la *clase* es **VARCHAR (8)** y su valor predeterminado es **Job**. la *clase* puede ser uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**TRABAJO**|Proporciona información acerca de una categoría de trabajo.|  
|**ONALERT**|Proporciona información acerca de una categoría de alerta.|  
|**OPERATOR**|Proporciona información acerca de una categoría de operador.|  
  
`[ @type = ] 'type'` Tipo de categoría para la que se solicita información. *Type* es de tipo **VARCHAR (12)**, su valor predeterminado es NULL y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**LOCAL**|Categoría de trabajo local.|  
|**MULTI -SERVER**|Categoría de trabajo multiservidor.|  
|**NONE**|Categoría para una clase distinta de **Job**.|  
  
`[ @name = ] 'name'` Nombre de la categoría para la que se solicita información. *Name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @suffix = ] suffix` Especifica si la columna de **category_type** del conjunto de resultados es un identificador o un nombre. el *sufijo* es de **bit** y su valor predeterminado es **0**. **1** muestra el **category_type** como un nombre y **0** lo muestra como un identificador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Cuando el **\@ sufijo** es **0**, **sp_help_category** devuelve el siguiente conjunto de resultados:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**category_id**|**int**|Id. de categoría|  
|**category_type**|**tinyint**|Tipo de categoría:<br /><br /> **1** = local<br /><br /> **2** = multiservidor<br /><br /> **3** = ninguno|  
|**name**|**sysname**|Nombre de la categoría|  
  
 Cuando el **\@ sufijo** es **1**, **sp_help_category** devuelve el siguiente conjunto de resultados:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**category_id**|**int**|Id. de categoría|  
|**category_type**|**sysname**|Tipo de categoría. Uno de los **locales**, **varios servidores** o **ninguno**|  
|**name**|**sysname**|Nombre de la categoría|  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_category** se debe ejecutar desde la base de datos **msdb** .  
  
 Si no se especifica ningún parámetro, el conjunto de resultados proporciona información acerca de todas las categorías de trabajo.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-local-job-information"></a>A. Devolver información de un trabajo local  
 Este ejemplo devuelve información acerca de los trabajos que se administran localmente.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_category  
    @type = N'LOCAL' ;  
GO  
```  
  
### <a name="b-returning-alert-information"></a>B. Devolver información de alertas  
 Este ejemplo devuelve información acerca de la categoría de alertas de replicación.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_category  
    @class = N'ALERT',  
    @name = N'Replication' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-category-transact-sql.md)   
 [sp_delete_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-category-transact-sql.md)   
 [sp_update_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-category-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
