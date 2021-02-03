---
description: MSSQL_ENG021797
title: MSSQL_ENG021797 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG021797 error
ms.assetid: 54d83a1e-43fd-449c-a2b2-fdda2609a534
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1c7a625832a9cbe43dfc01969c7a2cce6f434e0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193975"
---
# <a name="mssql_eng021797"></a>MSSQL_ENG021797
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|21797|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|"%s" debe ser un inicio de sesión de Windows válido en el formato: "MACHINE\Login" o "DOMAIN\Login". Vea la documentación de '%s'.|  
  
## <a name="explanation"></a>Explicación  
 Este error lo generan los siguientes procedimientos almacenados de replicación si el valor especificado para el parámetro `@job_login` es nulo o no es válido. Este error puede producirse si un miembro del rol fijo de base de datos **db_owner** ejecuta scripts de versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El modelo de seguridad de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]ha cambiado, por lo que dichos scripts deben actualizarse.  
  
-   [sp_addlogreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogreader-agent-transact-sql.md)  
  
-   [sp_addqreader_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addqreader-agent-transact-sql.md)  
  
-   [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)  
  
-   [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)  
  
-   [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)  
  
-   [sp_addmergepushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)  
  
-   [sp_addmergepullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)  
  
 Estos procedimientos almacenados pueden ser ejecutados por un miembro del rol fijo de servidor **sysadmin** del servidor correspondiente o por un miembro del rol fijo de base de datos **db_owner** de la base de datos correspondiente. Cada uno de los procedimientos almacenados crea un trabajo de agente y permite especificar la cuenta de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows con la que se ejecuta el agente. Para los usuarios del rol **sysadmin** , los trabajos de agente se crean de forma implícita, aun cuando no se especifique ninguna cuenta de Windows (si se especifica una cuenta, ésta debe ser válida); los agentes se ejecutan en el contexto de la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servidor correspondiente. Aunque la cuenta no es necesaria, por motivos de seguridad se recomienda especificar una cuenta independiente para los agentes. Para más información, consulte [Modelo de seguridad del agente de replicación](../../relational-databases/replication/security/replication-agent-security-model.md).  
  
## <a name="user-action"></a>Acción del usuario  
 Asegúrese de especificar una cuenta válida de Windows para el parámetro `@job_login` de cada procedimiento. Si tiene scripts de replicación de versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], actualice dichos scripts para incluir los procedimientos almacenados y los parámetros requeridos por [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Para obtener más información, vea [Actualizar scripts de replicación &#40;programación de la replicación con Transact-SQL&#41;](../../relational-databases/replication/administration/upgrade-replication-scripts-replication-transact-sql-programming.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
