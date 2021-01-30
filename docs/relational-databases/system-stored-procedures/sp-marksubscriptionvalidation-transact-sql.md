---
description: sp_marksubscriptionvalidation (Transact-SQL)
title: sp_marksubscriptionvalidation (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_marksubscriptionvalidation
- sp_marksubscriptionvalidation_TSQL
helpviewer_keywords:
- sp_marksubscriptionvalidation
ms.assetid: e68fe0b9-5993-4880-917a-b0f661f8459b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1fadb2c21aacab18741576d5809c28df989a67d6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185324"
---
# <a name="sp_marksubscriptionvalidation-transact-sql"></a>sp_marksubscriptionvalidation (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Marca la transacción abierta actual para que sea una transacción de validación de nivel de suscripción para el suscriptor especificado. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_marksubscriptionvalidation [ @publication = ] 'publication'  
        , [ @subscriber = ] 'subscriber'  
        , [ @destination_db = ] 'destination_db'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de tipo sysname y no tiene ningún valor predeterminado.  
  
`[ @destination_db = ] 'destination_db'` Es el nombre de la base de datos de destino. *destination_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* para una publicación que pertenece a un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_marksubscriptionvalidation** se utiliza en la replicación transaccional.  
  
 **sp_marksubscriptionvalidation** no admite suscriptores que no sean de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 En el caso de los publicadores que no son de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no se puede ejecutar **sp_marksubscriptionvalidation** desde una transacción explícita. Esto se debe a que las transacciones explícitas no se admiten en la conexión al servidor vinculado que se utiliza para obtener acceso al publicador.  
  
 **sp_marksubscriptionvalidation** debe usarse junto con [sp_article_validation &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md), especificando un valor de **1** para *subscription_level* y se puede usar con otras llamadas a **sp_marksubscriptionvalidation** para marcar la transacción abierta actual para otros suscriptores.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_marksubscriptionvalidation**.  
  
## <a name="example"></a>Ejemplo  
 La siguiente consulta puede aplicarse a la base de datos de publicaciones para exponer comandos de validación de suscripción. Los Agentes de distribución de los suscriptores especificados recogen estos comandos. Tenga en cuenta que la primera transacción valida el artículo '**art1**', mientras que la segunda transacción valida '**art2**'. Tenga en cuenta también que las llamadas a **sp_marksubscriptionvalidation** y [sp_article_validation &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) se han encapsulado en una transacción. Se recomienda una sola llamada a [sp_article_validation &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) por transacción. Esto se debe a que [sp_article_validation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-article-validation-transact-sql.md) mantiene un bloqueo de tabla compartido en la tabla de origen mientras dure la transacción. Debe mantener la transacción corta para maximizar la simultaneidad.  
  
```  
begin tran  
  
exec sp_marksubscriptionvalidation @publication = 'pub1',  
 @subscriber = 'Sub', @destination_db = 'SubDB'  
  
exec sp_marksubscriptionvalidation @publication = 'pub1',  
 @subscriber = 'Sub2', @destination_db = 'SubDB'  
  
exec sp_article_validation @publication = 'pub1', @article = 'art1',  
 @rowcount_only = 0, @full_or_fast = 0, @shutdown_agent = 0,  
 @subscription_level = 1  
  
commit tran  
  
begin tran  
  
exec sp_marksubscriptionvalidation @publication = 'pub1',  
 @subscriber = 'Sub', @destination_db = 'SubDB'  
  
exec sp_marksubscriptionvalidation @publication = 'pub1',  
 @subscriber = 'Sub2', @destination_db = 'SubDB'  
  
exec sp_article_validation @publication = 'pub1', @article = 'art2',  
 @rowcount_only = 0, @full_or_fast = 0, @shutdown_agent = 0,  
 @subscription_level = 1  
  
commit tran  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Validar datos replicados](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
