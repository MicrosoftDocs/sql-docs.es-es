---
description: sp_lookupcustomresolver (Transact-SQL)
title: sp_lookupcustomresolver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_lookupcustomresolver_TSQL
- sp_lookupcustomresolver
helpviewer_keywords:
- sp_lookupcustomresolver
ms.assetid: 356a7b8a-ae53-4fb5-86ee-fcfddbf23ddd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bdafb84d97bad1672780008ae845287ce105bb14
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185368"
---
# <a name="sp_lookupcustomresolver-transact-sql"></a>sp_lookupcustomresolver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de un controlador de lógica de negocios o el valor del identificador de clase (CLSID) de un componente de solucionador personalizado basado en COM registrado en el distribuidor. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_lookupcustomresolver [ @article_resolver = ] 'article_resolver'   
    [, [ @resolver_clsid = ] 'resolver_clsid' OUTPUT ]  
    [ , [ @is_dotnet_assembly = ] is_dotnet_assembly OUTPUT ]  
    [ , [ @dotnet_assembly_name = ] 'dotnet_assembly_name' OUTPUT ]  
    [ , [ @dotnet_class_name = ] 'dotnet_class_name' OUTPUT ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @article_resolver = ] 'article_resolver'` Especifica el nombre de la lógica de negocios personalizada que se va a eliminar del registro. *article_resolver* es de tipo **nvarchar (255)** y no tiene ningún valor predeterminado. Si la lógica de negocios que se va a quitar es un componente COM, este parámetro es el nombre descriptivo del componente. Si la lógica de negocios es un ensamblado de [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework, este parámetro es el nombre del ensamblado.  
  
`[ @resolver_clsid = ] 'resolver_clsid' OUTPUT` Es el valor CLSID del objeto COM asociado al nombre de la lógica de negocios personalizada especificada en el parámetro *article_resolver* . *resolver_clsid* es de tipo **nvarchar (50)** y su valor predeterminado es NULL.  
  
`[ @is_dotnet_assembly = ] 'is_dotnet_assembly' OUTPUT` Especifica el tipo de lógica de negocios personalizada que se va a registrar. *is_dotnet_assembly* es de **bit** y su valor predeterminado es 0. **1** indica que la lógica de negocios personalizada que se va a registrar es un ensamblado de controlador de lógica de negocios; **0** indica que se trata de un componente com.  
  
`[ @dotnet_assembly_name = ] 'dotnet_assembly_name' OUTPUT` Es el nombre del ensamblado que implementa el controlador de lógica de negocios. *dotnet_assembly_name* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @dotnet_class_name = ] 'dotnet_class_name' OUTPUT` Es el nombre de la clase que invalida <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> para implementar el controlador de lógica de negocios. *dotnet_class_name* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL. Utilice este parámetro si no se llama al procedimiento almacenado desde el publicador. Si no se especifica, se da por supuesto que el servidor local es el publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_lookupcustomresolver** se utiliza en la replicación de mezcla.  
  
 **sp_lookupcustomresolver** devuelve un valor NULL para *resolver_clsid* cuando el componente no está registrado en la distribución y un valor de "00000000-0000-0000-0000-000000000000" cuando el registro pertenece a un ensamblado de .NET Framework registrado como un controlador de lógica de negocios.  
  
 [sp_addmergearticle](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) y [sp_changemergearticle](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) llama a **sp_lookupcustomresolver** para validar el *article_resolver* especificado.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de base de datos **db_owner** en la base de datos de publicación pueden ejecutar **sp_lookupcustomresolver**.  
  
## <a name="see-also"></a>Consulte también  
 [Detección y resolución de conflictos de replicación de mezcla avanzada](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Ejecutar lógica de negocios durante la sincronización de mezcla](../../relational-databases/replication/merge/execute-business-logic-during-merge-synchronization.md)   
 [Implement a Business Logic Handler for a Merge Article](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)  (Implementar un controlador de lógica de negocios para un artículo de mezcla)  
 [Especificar un solucionador de artículos de mezcla](../../relational-databases/replication/publish/specify-a-merge-article-resolver.md)   
 [sp_registercustomresolver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-registercustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
