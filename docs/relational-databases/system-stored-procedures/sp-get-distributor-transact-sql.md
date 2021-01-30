---
description: sp_get_distributor (Transact-SQL)
title: sp_get_distributor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_get_distributor
- sp_get_distributor_TSQL
helpviewer_keywords:
- sp_get_distributor
ms.assetid: f0134448-bc17-4f2f-bd81-619351ce56ac
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 82c9e743cf3c1ddce87642fb3320b74eb5f7b6e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99105786"
---
# <a name="sp_get_distributor-transact-sql"></a>sp_get_distributor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Determina si hay un distribuidor instalado en un servidor. Este procedimiento almacenado se ejecuta en el equipo donde se está buscando el distribuidor, en cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_get_distributor   
```  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**instalación**|**int**|**0** = no; **1** = sí|  
|**servidor de distribución**|**sysname**|Nombre del servidor distribuidor|  
|**base de de distribución instalada**|**int**|**0** = no; **1** = sí|  
|**is distribution publisher**|**int**|**0** = no; **1** = sí|  
|**tiene publicador de distribución remoto**|**int**|**0** = no; **1** = sí|  
  
## <a name="remarks"></a>Observaciones  
 **sp_get_distributor** utiliza principalmente [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] en la replicación de instantáneas, transaccional y de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Cualquier usuario puede ejecutar **sp_get_distributor**. Se devuelve un conjunto de resultados no NULL cuando este procedimiento almacenado lo ejecutan los miembros de los roles fijos de base de datos **db_owner** o **replmonitor** en la base de datos de distribución, o los miembros del rol fijo de base de datos **db_owner** en al menos una base de datos publicada. También se devuelve un conjunto de resultados no NULL cuando los usuarios de la lista de acceso a la publicación (PAL) de al menos una base de datos publicada ejecutan este procedimiento almacenado, o en la PAL de la base de datos de distribución para un publicador que no es de SQL Server, también pueden ejecutar **sp_get_distributor**.  
  
## <a name="see-also"></a>Consulte también  
 [Configurar la publicación y la distribución](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Distributor and Publisher Information Script](../../relational-databases/replication/administration/distributor-and-publisher-information-script.md)  (Script de información del distribuidor y del publicador)  
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
