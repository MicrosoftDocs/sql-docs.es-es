---
description: MSdistpublishers (Transact-SQL)
title: MSdistpublishers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdistpublishers
- MSdistpublishers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSdistpublishers system table
ms.assetid: 31844099-4b33-4dc9-84b4-bac70aa82598
author: cawrites
ms.author: chadam
ms.openlocfilehash: cfb8cfe834006d0d23ca0ed41959d73351208c25
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160426"
---
# <a name="msdistpublishers-transact-sql"></a>MSdistpublishers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La tabla **MSdistpublishers** contiene una fila por cada publicador remoto que admite el distribuidor local. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del distribuidor publicador.|  
|**distribution_db**|**sysname**|El nombre de la base de datos de distribución.|  
|**working_directory**|**nvarchar(255)**|Nombre del directorio de trabajo utilizado para almacenar archivos de datos y de esquema para la publicación.|  
|**security_mode**|**int**|Modo de seguridad aplicado en el distribuidor:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.<br /><br /> **1** = autenticación de Windows.|  
|**login**|**sysname**|Identificador de inicio de sesión para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**password**|**nvarchar (524)**|Contraseña (cifrada) para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**active**|**bit**|Indica si el publicador remoto está usando el distribuidor local.|  
|**trusted**|**bit**|Indica si el publicador remoto utiliza la misma contraseña que el distribuidor local:<br /><br /> **0** = se necesita una contraseña en el publicador remoto para conectarse al distribuidor.<br /><br /> **1** = no se necesita ninguna contraseña.|  
|**third_party**|**bit**|Indica si el publicador es una instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:<br /><br /> instalación de **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .**1** = origen de datos heterogéneo.|  
|**publisher_type**|**sysname**|Tipo de publicador:<br /><br />   =  MSSQLSERVER [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Publicador.<br /><br /> **Oracle** = publicador estándar de Oracle.<br /><br /> **Puerta de enlace de Oracle** = publicador de puerta de enlace de Oracle.|  
|**storage_connection_string**|**nvarchar (779)**|Valor de Azure SQL Database cadena de conexión de almacenamiento.|  

  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
