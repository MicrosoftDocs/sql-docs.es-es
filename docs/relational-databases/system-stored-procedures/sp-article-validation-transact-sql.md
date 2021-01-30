---
description: sp_article_validation (Transact-SQL)
title: sp_article_validation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_article_validation_TSQL
- sp_article_validation
helpviewer_keywords:
- sp_article_validation
ms.assetid: 44e7abcd-778c-4728-a03e-7e7e78d3ce22
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1b2dda5e359b062716c9c0fead89b4ad2628488a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203225"
---
# <a name="sp_article_validation-transact-sql"></a>sp_article_validation (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Inicia una solicitud de validación de datos del artículo especificado. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicaciones y en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_article_validation [ @publication = ] 'publication'  
    [ , [ @article = ] 'article' ]  
    [ , [ @rowcount_only = ] type_of_check_requested ]  
    [ , [ @full_or_fast = ] full_or_fast ]  
    [ , [ @shutdown_agent = ] shutdown_agent ]  
    [ , [ @subscription_level = ] subscription_level ]  
    [ , [ @reserved = ] reserved ]  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación en la que existe el artículo. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo que se va a validar. *article* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @rowcount_only = ] type_of_check_requested` Especifica si solo se devuelve el recuento de filas de la tabla. *type_of_check_requested* es de **smallint** y su valor predeterminado es **1**.  
  
 Si es **0**, se realiza un recuento de filas y una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suma de comprobación compatible con 7,0.  
  
 Si es **1**, realizar solo una comprobación del recuento de filas.  
  
 Si es **2**, realice un recuento de filas y una suma de comprobación binaria.  
  
`[ @full_or_fast = ] full_or_fast` Es el método utilizado para calcular el recuento de filas. *full_or_fast* es **tinyint** y puede tener uno de estos valores.  
  
|**Valor**|**Descripción**|  
|---------------|---------------------|  
|**0**|Realiza un recuento completo mediante COUNT (*).|  
|**1**|Realiza un recuento rápido desde **sysindexes. Rows**. Contar las filas en **sysindexes** es más rápido que contar las filas de la tabla real. Sin embargo, **sysindexes** se actualiza de forma diferida y es posible que el recuento de filas no sea preciso.|  
|**2** (predeterminado)|Realiza un recuento rápido condicional probando primero con el método rápido. Si el método rápido muestra diferencias, se utiliza el método completo. Si *expected_rowcount* es NULL y se usa el procedimiento almacenado para obtener el valor, siempre se usa un recuento completo (*).|  
  
`[ @shutdown_agent = ] shutdown_agent` Especifica si el agente de distribución debe cerrarse inmediatamente al completarse la validación. *shutdown_agent* es de **bit** y su valor predeterminado es **0**. Si es **0**, el agente de distribución no se cierra. Si es **1**, el agente de distribución se cierra después de validar el artículo.  
  
`[ @subscription_level = ] subscription_level` Especifica si un conjunto de suscriptores recoge o no la validación. *subscription_level* es de **bit** y su valor predeterminado es **0**. Si es **0**, la validación se aplica a todos los suscriptores. Si es **1**, la validación solo se aplica a un subconjunto de los suscriptores especificados por las llamadas a **sp_marksubscriptionvalidation** en la transacción abierta actual.  
  
`[ @reserved = ] reserved` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al solicitar la validación en un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_article_validation** se utiliza en la replicación transaccional.  
  
 **sp_article_validation** hace que la información de validación se recopile en el artículo especificado y publique una solicitud de validación en el registro de transacciones. Cuando el Agente de distribución recibe la solicitud, compara la información de validación de la solicitud con la tabla del suscriptor. El resultado de la validación se muestra en el Monitor de replicación y en las alertas del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Permisos  
 Solo los usuarios con permisos SELECT ALL en la tabla de origen para el artículo que se va a validar pueden ejecutar **sp_article_validation**.  
  
## <a name="see-also"></a>Consulte también  
 [Validar datos replicados](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_marksubscriptionvalidation &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-marksubscriptionvalidation-transact-sql.md)   
 [sp_publication_validation &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-publication-validation-transact-sql.md)   
 [sp_table_validation &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-table-validation-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
