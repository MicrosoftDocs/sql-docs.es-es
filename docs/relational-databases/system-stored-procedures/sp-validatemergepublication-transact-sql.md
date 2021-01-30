---
description: sp_validatemergepublication (Transact-SQL)
title: sp_validatemergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_validatemergepublication
- sp_validatemergepublication_TSQL
helpviewer_keywords:
- sp_validatemergepublication
ms.assetid: 5a862f1a-2be1-4758-9954-4cdc8c77d149
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0cf0b85db695458be4b9a48b74bfdc598f76f544
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201832"
---
# <a name="sp_validatemergepublication-transact-sql"></a>sp_validatemergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Realiza una validación en toda la publicación en la que se validan una sola vez todas las suscripciones (anónimas, de inserción y de extracción). Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_validatemergepublication [@publication=] 'publication'  
        , [ @level = ] level  
```  
  
## <a name="arguments"></a>Argumentos  
 [**\@ publicación =**] **'**_publicación_*_'_*  
 Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @level = ] level` Es el tipo de validación que se va a realizar. *LEVEL* es de **tinyint** y no tiene ningún valor predeterminado. Puede ser uno de estos valores.  
  
|Valor de nivel|Descripción|  
|-----------------|-----------------|  
|**1**|Validación solo del recuento de filas|  
|**2**|Validación del recuento de filas y de la suma de comprobación. En el caso de los [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] suscriptores, se establece automáticamente en **3**.|  
|**3**|Este es el valor recomendado.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_validatemergepublication** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_validatemergepublication**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Validar datos replicados](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_validatemergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validatemergesubscription-transact-sql.md)  
  
  
