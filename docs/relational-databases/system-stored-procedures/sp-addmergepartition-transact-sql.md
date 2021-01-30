---
description: sp_addmergepartition (Transact-SQL)
title: sp_addmergepartition (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergepartition
- sp_addmergepartition_TSQL
helpviewer_keywords:
- sp_addmergepartition
ms.assetid: 02a5f46b-e5ff-4932-a3ff-7f0fd82d0981
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5129bcd9ee63b0c1e074294c8b69f997432240f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192323"
---
# <a name="sp_addmergepartition-transact-sql"></a>sp_addmergepartition (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea una partición filtrada dinámicamente para una suscripción que se filtra por los valores de [host_name](../../t-sql/functions/host-name-transact-sql.md) o [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) en el suscriptor. Este procedimiento almacenado se ejecuta en el publicador en la base de datos que se está publicando y se utiliza para generar particiones manualmente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addmergepartition [ @publication = ] 'publication'  
        , [ @suser_sname = ] 'suser_sname'  
        , [ @host_name = ] 'host_name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es la publicación de combinación en la que se crea la partición. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado. Si se especifica *SUSER_SNAME* , el valor de *hostname* debe ser null.  
  
`[ @suser_sname = ] 'suser_sname'` Es el valor utilizado al crear la partición para una suscripción que se filtra por el valor de la función [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) en el suscriptor. *SUSER_SNAME* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @host_name = ] 'host_name'` Es el valor utilizado al crear la partición para una suscripción que se filtra por el valor de la función [host_name](../../t-sql/functions/host-name-transact-sql.md) en el suscriptor. *host_name* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addmergepartition** se utiliza en la replicación de mezcla.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_MergeDynamicPubPlusPartition](../../relational-databases/replication/codesnippet/tsql/sp-addmergepartition-tra_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addmergepartition**.  
  
## <a name="see-also"></a>Consulte también  
 [Crear una instantánea para una publicación de combinación con filtros con parámetros](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)   
 [Filtros de fila con parámetros](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
