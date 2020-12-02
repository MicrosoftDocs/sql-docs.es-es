---
title: Modelos de recuperación (SQL Server) | Microsoft Docs
description: En SQL Server, un modelo de recuperación controla la forma en la que se registran las transacciones, si el registro de transacciones requiere que se realicen copias de seguridad y qué operaciones de restauración hay disponibles.
ms.custom: ''
ms.date: 07/16/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- database backups [SQL Server], recovery models
- bulk-logged recovery model [SQL Server]
- recovery [SQL Server], recovery model
- restoring transaction logs [SQL Server], recovery models
- backing up databases [SQL Server], recovery models
- recovery models [SQL Server], about
- transaction log backups [SQL Server]
- simple recovery model [SQL Server]
- backups [SQL Server], recovery models
- recovery models [SQL Server]
- recovery models [SQL Server], transaction log management
- database restores [SQL Server], recovery models
- transaction log restores [SQL Server]
- logs [SQL Server], recovery models
- restoring databases [SQL Server], recovery models
- full recovery model [SQL Server]
- backing up transaction logs [SQL Server], recovery models
ms.assetid: 8cfea566-8f89-4581-b30d-c53f1f2c79eb
author: cawrites
ms.author: chadam
ms.openlocfilehash: a7b746b8bd323b7037a7a477b33596b554aed096
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130356"
---
# <a name="recovery-models-sql-server"></a>Modelos de recuperación (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se producen en el contexto del modelo de recuperación de la base de datos. Los modelos de recuperación se han diseñado para controlar el mantenimiento del registro de transacciones. Un *modelo de recuperación* es una propiedad de base de datos que controla la forma en que se registran las transacciones, si el registro de transacciones requiere que se realice la copia de seguridad y si lo permite, y qué tipos de operaciones de restauración hay disponibles. Existen tres modelos de recuperación: simple, completa y por medio de registros de operaciones masivas. Normalmente, en las bases de datos se usa el modelo de recuperación completa o el modelo de recuperación simple. El modelo de recuperación de las bases de datos se puede cambiar en cualquier momento.  
  
 **En este tema:**  
  
-   [Introducción al modelo de recuperación](#RMov)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="recovery-model-overview"></a><a name="RMov"></a> Introducción al modelo de recuperación  
 En la tabla siguiente se resumen los tres modelos de recuperación.  
  
|modelo de recuperación|Descripción|Riesgo de pérdida de trabajo|¿Recuperación hasta un momento dado?|  
|--------------------|-----------------|------------------------|-------------------------------|  
|**Simple**|Sin copias de seguridad de registros.<br /><br /> Recupera automáticamente el espacio de registro para mantener al mínimo los requisitos de espacio, eliminando, en esencia, la necesidad de administrar el espacio del registro de transacciones. Para obtener información sobre las copias de seguridad de base de datos en el modelo de recuperación simple, vea [Copias de seguridad completas de bases de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md).<br /><br /> Las operaciones que requieren copias de seguridad del registro de transacciones no son compatibles con el modelo de recuperación simple. Las características siguientes no se pueden utilizar en modo de recuperación simple:<br /><br /> -Trasvase de registros<br /><br /> -AlwaysOn o creación de reflejo de la base de datos<br /><br /> -Recuperación de medios sin pérdida de datos<br /><br /> -Restauraciones a un momento dado|Los cambios realizados después de la copia de seguridad más reciente no están protegidos. En caso de desastre, es necesario volver a realizar dichos cambios.|Solo se puede recuperar hasta el final de una copia de seguridad. Para obtener más información, vea [Restauraciones de base de datos completas &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md). <br><br> Para obtener una explicación más amplia sobre el modelo de recuperación simple, vea [Modelo de recuperación simple de SQL Server](https://www.mssqltips.com/sqlservertutorial/4/sql-server-simple-recovery-model/) proporcionado por los compañeros de [MSSQLTips!](https://www.mssqltips.com).|  
|**Completa**|Requiere copias de seguridad de registros.<br /><br /> No se pierde trabajo si un archivo de datos se pierde o resulta dañado.<br /><br /> Se puede recuperar hasta cualquier momento, por ejemplo, antes del error de aplicación o usuario. Para obtener información sobre las copias de seguridad de base de datos en el modelo de recuperación completa, vea [Copias de seguridad completas de bases de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md) y [Restauraciones de base de datos completas &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).|Normalmente ninguno.<br /><br /> Si el final del registro resulta dañado, se deben repetir los cambios realizados desde la última copia de seguridad de registros.|Se puede recuperar hasta determinado momento, siempre que las copias de seguridad se hayan completado hasta ese momento. Para obtener más información sobre cómo usar copias de seguridad de registros para restaurar hasta el momento del error, vea [Restaurar una base de datos de SQL Server a un momento dado &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md).<br /><br /> Nota: Si tiene dos o más bases de datos con el modelo de recuperación completa que deben ser lógicamente coherentes, es posible que tenga que implementar procedimientos especiales para garantizar la posibilidad de recuperación de estas bases de datos. Para obtener más información, vea [Recuperación de bases de datos relacionadas que contienen transacciones marcadas](../../relational-databases/backup-restore/recovery-of-related-databases-that-contain-marked-transaction.md).|  
|**Por medio de registros de operaciones masivas**|Requiere copias de seguridad de registros.<br /><br /> Complemento del modelo de recuperación completa que permite operaciones de copia masiva de alto rendimiento.<br /><br /> Reduce el uso del espacio de registro mediante el registro mínimo de la mayoría de las operaciones masivas. Para obtener información sobre las operaciones que se pueden registrar mínimamente, vea [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).<br /><br /> Las copias de seguridad de registros pueden tener un tamaño considerable debido a que las operaciones registradas mínimamente se capturan en la copia de seguridad de registros. Para obtener información sobre las copias de seguridad de base de datos en el modelo de recuperación optimizado para cargas masivas de registros, vea [Copias de seguridad completas de bases de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md) y [Restauraciones de base de datos completas &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md).|Si el registro resulta dañado o se han realizado operaciones masivas desde la última copia de seguridad de registros, se pueden repetir los cambios desde esa última copia de seguridad.<br /><br /> En caso contrario, no se pierde el trabajo.|Se puede recuperar hasta el final de cualquier copia de seguridad. No admite recuperaciones a un momento dado.|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Ver o cambiar el modelo de recuperación de una base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md)  
  
-   [Solucionar problemas de un registro de transacciones lleno &#40;Error 9002 de SQL Server&#41;](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
## <a name="see-also"></a>Consulte también  
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [Realizar copias de seguridad y restaurar bases de datos de SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Tareas administrativas automatizadas &#40;Agente SQL Server&#41;](../../ssms/agent/automated-administration-tasks-sql-server-agent.md)   
 [Información general sobre restauración y recuperación &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)  
  
