---
description: MSSQL_ENG018752
title: MSSQL_ENG018752 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG018752 error
ms.assetid: 405b2655-acb4-4e15-bcc6-b8f86bb22b37
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 00d3e098ca0b3686a96be5741d2800973d632147
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205622"
---
# <a name="mssql_eng018752"></a>MSSQL_ENG018752
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|18752|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|El Agente de registro del LOG y los procedimientos relacionados con el registro (sp_repldone, sp_replcmds y sp_replshowcmds) solamente pueden conectarse a la base de datos de uno en uno. Si ejecutó un procedimiento relacionado con el registro, quite la conexión mediante la cual se ejecutó el procedimiento o ejecute sp_replflush en esa conexión antes de iniciar el Agente de registro del LOG o de ejecutar otro procedimiento relacionado con el registro.|  
  
## <a name="explanation"></a>Explicación  
 Más de una conexión actual intenta ejecutar uno de los siguientes procedimientos: **sp_repldone**, **sp_replcmds** o **sp_replshowcmds**. [Sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md) y [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) son procedimientos almacenados usados por el Agente de registro del LOG para buscar información sobre transacciones replicadas en una base de datos publicada y actualizarla. El procedimiento almacenado [sp_replshowcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replshowcmds-transact-sql.md) se usa para resolver ciertos tipos de problemas con la replicación transaccional.  
  
 Este error se produce bajo las siguientes circunstancias:  
  
-   Si se está ejecutando el Agente de registro del LOG en una base de datos publicada y un segundo Agente de registro del LOG intenta ejecutarse en la misma base de datos, se produce el error en el segundo agente y aparece en el historial del agente.  
  
     En una situación en la que parece haber varios agentes, es posible que uno de ellos sea el resultado de un proceso huérfano.  
  
-   Si se inicia el Agente de registro del LOG en una base de datos publicada y un usuario ejecuta **sp_repldone**, **sp_replcmds** o **sp_replshowcmds** en la misma base de datos, se produce el error en la aplicación donde se ejecutó el procedimiento almacenado (por ejemplo **sqlcmd**).  
  
-   Si no se está ejecutando el Agente de registro del LOG en una base de datos publicada y un usuario ejecuta **sp_repldone**, **sp_replcmds** o **sp_replshowcmds** y, a continuación, no cierra la conexión en la que se ejecutó el procedimiento, se produce el error cuando el Agente de registro del LOG intenta conectarse a la base de datos.  
  
## <a name="user-action"></a>Acción del usuario  
 Los siguientes pasos pueden ayudar a resolver el problema. Si un paso permite al Agente de registro del LOG iniciarse sin errores, no es necesario completar los pasos restantes.  
  
-   Compruebe en el historial del Agente de registro del LOG otros errores que podrían contribuir a este error. Para obtener información sobre la visualización del estado y los errores en Monitor de replicación, vea [Visualización de información y realización de tareas mediante el Monitor de replicación](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
-   Compruebe el resultado de [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md) para los números de identificación de proceso específico (SPID) que estén conectados a la base de datos publicada. Cierre las conexiones que puedan haber ejecutado **sp_repldone**, **sp_replcmds** o **sp_replshowcmds**.  
  
-   Reinicie el Agente de registro del LOG. Para obtener más información, vea [Iniciar y detener un agente de replicación &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md).  
  
-   Reinicie el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (déjelo sin conexión o conéctelo en un clúster) en el distribuidor. Si existe la posibilidad de que un trabajo programado pueda ejecutar **sp_repldone**, **sp_replcmds** o **sp_replshowcmds** desde cualquier otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , reinicie también el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en esas instancias. Para obtener más información, vea [Iniciar, detener o pausar el servicio del Agente SQL Server](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md).  
  
-   Ejecute [sp_replflush &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md) en el publicador de la base de datos de publicación y reinicie el Agente de registro del LOG.  
  
-   Si el error persiste, aumente el registro del agente y especifique un archivo de salida para el registro. Dependiendo del contexto del error, esto puede proporcionar los pasos que conducen al error o a mensajes de error adicionales.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Agente de registro del LOG de replicación](../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
