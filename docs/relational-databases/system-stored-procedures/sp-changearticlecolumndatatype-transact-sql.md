---
description: sp_changearticlecolumndatatype (Transact-SQL)
title: sp_changearticlecolumndatatype (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changearticlecolumndatatype
- sp_changearticlecolumndatatype_TSQL
helpviewer_keywords:
- sp_changearticlecolumndatatype
ms.assetid: 0db80e08-fb77-4d0c-aa41-455b13ffa9b4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 34f3f1878fcae59601a6ac94ff8536f3d22d6ea5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203709"
---
# <a name="sp_changearticlecolumndatatype-transact-sql"></a>sp_changearticlecolumndatatype (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia la asignación del tipo de datos de la columna del artículo en una publicación de Oracle. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
> [!NOTE]  
>  De forma predeterminada, se proporcionan las asignaciones de tipos de datos entre los tipos de publicadores admitidos. Utilice **sp_changearticlecolumndatatype** solo cuando reemplace estos valores predeterminados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changearticlecolumndatatype [ @publication= ] 'publication'  
    [ @article = ] 'article'   
    [ @column = ] 'column'  
    [ , [ @type = ] 'type' ]  
    [ , [ @length = ] length ]  
    [ , [ @precision = ] precision ]  
    [ , [ @scale = ] scale ]  
    [ , [ @publisher = ] 'publisher'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación de Oracle. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo. *article* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @column = ] 'column'` Es el nombre de la columna para la que se va a cambiar la asignación de tipo de datos. la *columna* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @type = ] 'type'` Es el nombre del [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de la columna de destino. *Type* es de tipo **sysname y su** valor predeterminado es NULL.  
  
`[ @length = ] length` Es la longitud del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de la columna de destino. *length* es de tipo **BIGINT** y su valor predeterminado es NULL.  
  
`[ @precision = ] precision` Es la precisión del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tipo de datos de la columna de destino. la *precisión* es **BIGINT** y su valor predeterminado es NULL.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **Sp_changearticlecolumndatatype** se utiliza para invalidar las asignaciones de tipos de datos predeterminados entre los tipos de publicadores admitidos (Oracle y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ). Para ver estas asignaciones de tipos de datos predeterminados, ejecute [sp_getdefaultdatatypemapping](../../relational-databases/system-stored-procedures/sp-getdefaultdatatypemapping-transact-sql.md).  
  
 **sp_changearticlecolumndatatype** solo se admite para publicadores de Oracle. Ejecutar este procedimiento almacenado contra una publicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] produce un error.  
  
 **sp_changearticlecolumndatatype** debe ejecutarse para cada asignación de columna de artículo que se debe cambiar.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_changearticlecolumndatatype**.  
  
## <a name="see-also"></a>Consulte también  
 [Cambiar las propiedades de la publicación y de los artículos](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Data Type Mapping for Oracle Publishers](../../relational-databases/replication/non-sql/data-type-mapping-for-oracle-publishers.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
