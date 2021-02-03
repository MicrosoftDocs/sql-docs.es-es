---
description: MSSQL_REPL020011
title: MSSQL_REPL020011 | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_REPL020011 error
ms.assetid: f72072d7-bbb6-48ad-ac88-afa74aeb4d58
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: c5184aa24cd4405ee60a65c3a45ed56daf11645f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206181"
---
# <a name="mssql_repl020011"></a>MSSQL_REPL020011
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|20011|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|El proceso no pudo ejecutar '%1' en '%2'.|  
  
## <a name="explanation"></a>Explicación  
 Este error se puede generar en una serie de circunstancias durante el procesamiento de la replicación transaccional, por ejemplo, cuando el Agente de registro del LOG ejecuta **sp_replcmds** (el proceso no pudo ejecutar "sp_replcmds" en \<ServerName>) o **sp_repldone** (el proceso no pudo ejecutar "sp_repldone" en \<ServerName>).  
  
## <a name="user-action"></a>Acción del usuario  
 Si este error se genera en una base de datos que acaba de restaurar de una copia de seguridad, asegúrese de que ha seguido los pasos descritos en la documentación referente a copias de seguridad y restauración, incluida la ejecución de **sp_replrestart** si fuese necesario. Para más información, consulte [Estrategias para hacer copias de seguridad y restaurar replicación de instantáneas o replicación transaccional](../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
 Se trata de un error de procesamiento interno y si se genera en circunstancias diferentes a una restauración, normalmente indica que la replicación se debe quitar y volver a configurar. Si no puede quitar la replicación, póngase en contacto con el soporte técnico para obtener ayuda.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)  
  
  
