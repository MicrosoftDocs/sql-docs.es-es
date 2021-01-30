---
description: sp_registercustomresolver (Transact-SQL)
title: sp_registercustomresolver (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_registercustomresolver
- sp_registercustomresolver_TSQL
helpviewer_keywords:
- sp_registercustomresolver
ms.assetid: 6d2b0472-0e1f-4005-833c-735d1940fe93
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4a4c08e8643aedaecd415177ef87f9fadc69563f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185690"
---
# <a name="sp_registercustomresolver-transact-sql"></a>sp_registercustomresolver (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Registra un controlador de lógica de negocios o un solucionador personalizado basado en COM que se pueda invocar durante el proceso de sincronización de replicación de mezcla. Este procedimiento almacenado se ejecuta en el distribuidor.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_registercustomresolver [ @article_resolver = ] 'article_resolver'   
    [ , [ @resolver_clsid = ] 'resolver_clsid' ]  
    [ , [ @is_dotnet_assembly = ] 'is_dotnet_assembly' ]  
    [ , [ @dotnet_assembly_name = ] 'dotnet_assembly_name' ]  
    [ , [ @dotnet_class_name = ] 'dotnet_class_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @article_resolver = ] 'article_resolver'` Especifica el nombre descriptivo de la lógica de negocios personalizada que se va a registrar. *article_resolver* es de tipo **nvarchar (255)** y no tiene ningún valor predeterminado.  
  
`[ @resolver_clsid = ] 'resolver_clsid'` Especifica el valor CLSID del objeto COM que se va a registrar. La lógica de negocios personalizada *resolver_clsid* es **nvarchar (50)** y su valor predeterminado es NULL. Este parámetro debe estar establecido en un valor CLSID válido o en NULL cuando se registre un ensamblado de controlador de lógica de negocios.  
  
`[ @is_dotnet_assembly = ] 'is_dotnet_assembly'` Especifica el tipo de lógica de negocios personalizada que se va a registrar. *is_dotnet_assembly* es de tipo **nvarchar (50)** y su valor predeterminado es false. **true** indica que la lógica de negocios personalizada que se va a registrar es un ensamblado de controlador de lógica de negocios; **false** indica que se trata de un componente com.  
  
`[ @dotnet_assembly_name = ] 'dotnet_assembly_name'` Es el nombre del ensamblado que implementa el controlador de lógica de negocios. *dotnet_assembly_name* es de tipo **nvarchar (255)** y su valor predeterminado es NULL. Debe especificar la ruta de acceso completa al ensamblado si no está implementado en el mismo directorio que el ejecutable del Agente de mezcla, en el mismo directorio que la aplicación que inicia de forma sincrónica el Agente de mezcla o en la caché de ensamblados global (GAC).  
  
`[ @dotnet_class_name = ] 'dotnet_class_name'` Es el nombre de la clase que invalida <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> para implementar el controlador de lógica de negocios. El nombre debe especificarse con el formato **espaciodenombres. nombreDeClase**. *dotnet_class_name* es de tipo **nvarchar (255)** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_registercustomresolver** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_registercustomresolver**.  
  
## <a name="see-also"></a>Consulte también  
 [Implement a Business Logic Handler for a Merge Article](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)  (Implementar un controlador de lógica de negocios para un artículo de mezcla)  
 [Implementar un solucionador de conflictos personalizado para un artículo de mezcla](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [sp_lookupcustomresolver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
