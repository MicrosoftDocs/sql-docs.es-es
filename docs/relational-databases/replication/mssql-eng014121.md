---
description: MSSQL_ENG014121
title: MSSQL_ENG014121 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014121 error
ms.assetid: c8595854-cce1-4566-ad64-d565555caded
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: cfec1cfbc231ad4a347bd6d674bbe88c812eb9e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198788"
---
# <a name="mssql_eng014121"></a>MSSQL_ENG014121
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|14121|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|No se pudo quitar el distribuidor '%s'. Tiene asociadas bases de datos de distribución.|  
  
## <a name="explanation"></a>Explicación  
 No se puede quitar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configurada como distribuidor del rol de distribuidor porque hay bases de datos de distribución asociadas con esa instancia. Este error se produce si intenta quitar una base de datos de distribución que está asociada a uno o más publicadores.  
  
## <a name="user-action"></a>Acción del usuario  
 Para buscar los nombres de todos los publicadores y bases de datos de distribución asociados a este distribuidor, ejecute [sp_helpdistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistpublisher-transact-sql.md) desde cualquier base de datos del distribuidor.  
  
 Ejecute [sp_dropdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md) para todas las bases de datos de distribución asociadas con este distribuidor. Cuando haya quitado todas las asociaciones con bases de datos de distribución, puede deshabilitar la distribución.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Configurar distribución](../../relational-databases/replication/configure-distribution.md)  
  
  
