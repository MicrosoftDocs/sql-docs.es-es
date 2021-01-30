---
description: sp_dropdistpublisher (Transact-SQL)
title: sp_dropdistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropdistpublisher
- sp_dropdistpublisher_TSQL
helpviewer_keywords:
- sp_dropdistpublisher
ms.assetid: c0bdd3de-3be0-455c-898a-98d4660e7ce3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 09db4d47afee6795b403542c8442cc9d74724f4b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200702"
---
# <a name="sp_dropdistpublisher-transact-sql"></a>sp_dropdistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Quita un publicador de distribución. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dropdistpublisher [ @publisher = ] 'publisher'  
    [ , [ @no_checks = ] no_checks ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el publicador que se va a quitar. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @no_checks = ] no_checks` Especifica si **sp_dropdistpublisher** comprueba que el publicador ha desinstalado el servidor como distribuidor. *no_checks* es de **bit** y su valor predeterminado es **0**.  
  
 Si es **0**, la replicación comprueba que el publicador remoto ha desinstalado el servidor local como distribuidor. Si el publicador de distribución es local, la replicación comprueba que no quedan objetos de publicación o distribución en el servidor local.  
  
 Si es **1**, se quitan todos los objetos de replicación asociados al publicador de distribución, incluso si no se puede tener acceso a un publicador remoto. Después de hacerlo, el publicador remoto debe desinstalar la replicación mediante [sp_dropdistributor](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md) con **\@ ignore_distributor**  =  **1**.  
  
`[ @ignore_distributor = ] ignore_distributor` Especifica si los objetos de distribución se dejan en el distribuidor cuando se quita el publicador. *ignore_distributor* es de **bit** y puede tener uno de estos valores:  
  
 **1** = los objetos de distribución que pertenecen al *publicador* permanecen en el distribuidor.  
  
 **0** = los objetos de distribución del *publicador* se limpian en el distribuidor.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_dropdistpublisher** se utiliza en todos los tipos de replicación.  
  
 Al quitar un publicador de Oracle, si no puede quitar el publicador **sp_dropdistpublisher** devuelve un error y se quitan los objetos de distribuidor para el publicador.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistpublisher-tra_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_dropdistpublisher**.  
  
## <a name="see-also"></a>Consulte también  
 [Disable Publishing and Distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)  (Deshabilitar la publicación y la distribución)  
 [sp_adddistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
