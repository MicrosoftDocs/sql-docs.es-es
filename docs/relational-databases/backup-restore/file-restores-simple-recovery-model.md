---
title: Restauraciones de archivos (modelo de recuperación simple) | Microsoft Docs
description: En SQL Server, una restauración de archivos se aplica a uno o varios archivos dañados sin necesidad de restaurar la base de datos completa.
ms.custom: ''
ms.date: 03/24/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- file restores [SQL Server]
- simple recovery model [SQL Server]
- restoring files [SQL Server], Transact-SQL restore sequence
- restoring files [SQL Server]
- Transact-SQL restore sequence
- restoring files [SQL Server], simple recovery model
- file restores [SQL Server], simple recovery model
- file restores [SQL Server], Transact-SQL restore sequence
ms.assetid: b6d07386-7c6f-4cc6-be32-93289adbd3d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5e2df8edb5b31ff2ffcd45eba9b5d9cf234c528d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076893"
---
# <a name="file-restores-simple-recovery-model"></a>Restauraciones de archivos (modelo de recuperación simple)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Este tema solo es relevante para las bases de datos de modelo simple que incluyen como mínimo un grupo de archivos secundario de solo lectura.  
  
 El objetivo de una restauración de archivos consiste en restaurar uno o varios archivos dañados sin necesidad de restaurar la totalidad de la base de datos. En el modelo de recuperación simple, las copias de seguridad de archivos se admiten únicamente para los archivos de solo lectura. El grupo de archivos primario y los grupos de archivos secundarios de lectura/escritura se restauran siempre juntos, mediante la restauración de una base de datos o de una copia de seguridad parcial.  
  
 Los escenarios de restauración de archivos son los siguientes:  
  
-   Restauración de archivos sin conexión  
  
     En una *restauración de archivos sin conexión*, la base de datos permanece sin conexión mientras se restauran los archivos o grupos de archivos dañados. Al final de la secuencia de restauración, la base de datos pasará a estar en línea.  
  
     Todas las ediciones de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] admiten restauraciones de archivos sin conexión.  
  
-   Restauración de archivos en línea  
  
     En *restauración de archivos en línea*, si la base de datos está en línea durante una restauración de archivos, permanecerá en línea durante la restauración de archivos. Sin embargo, cada grupo de archivos en el que se restaura un archivo está sin conexión durante la operación de restauración. Una vez recuperados todos los archivos de un grupo de archivos sin conexión, este se conecta automáticamente.  
  
     Para información sobre la compatibilidad con la restauración en línea de la página y de archivo, vea [Características y tareas del motor de base de datos](../../sql-server/what-s-new-in-sql-server-ver15.md). Para obtener más información sobre la restauración con conexión, vea [Restauración con conexión &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md).  
  
    > [!TIP]  
    >  Si quiere que la base de datos esté sin conexión durante una restauración de archivos, deje sin conexión la base de datos antes de iniciar la secuencia de restauración mediante la ejecución de la instrucción [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md) siguiente: ALTER DATABASE *nombre_base_de_datos* SET OFFLINE.  
  
 **En este tema:**  
  
-   [Información general acerca de la restauración de archivos y grupos de archivos con el modelo de recuperación simple](#Overview)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="overview-of-file-and-filegroup-restore-under-the-simple-recovery-model"></a><a name="Overview"></a> Información general acerca de la restauración de archivos y grupos de archivos con el modelo de recuperación simple  
 Un escenario de restauración de archivos está formado por una única secuencia de restauración que copia, pone al día y recupera los datos apropiados de la siguiente manera:  
  
1.  Restaure cada archivo dañado a partir de su copia de seguridad de archivo más reciente.  
  
2.  Restaure la copia de seguridad diferencial de archivos más reciente para cada archivo restaurado y recupere la base de datos.  
  
### <a name="transact-sql-steps-for-file-restore-sequence-simple-recovery-model"></a>Secuencia de restauración de Transact-SQL para la restauración de archivos (modelo de recuperación simple)  
 Esta sección muestra las opciones fundamentales de [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) de [!INCLUDE[tsql](../../includes/tsql-md.md)] de una secuencia de restauración de archivos simple. La sintaxis y los detalles no pertinentes para este propósito se omiten.  
  
 La secuencia de restauración solo contiene dos instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] . La primera instrucción restaura un archivo secundario, el archivo `A`, que se restaura usando WITH NORECOVERY. La segunda operación restaura otros dos archivos, `B` y `C` , que se restauran usando WITH RECOVERY desde un dispositivo de copia de seguridad diferente:  
  
1.  RESTORE DATABASE *base_de_datos* FILE **=** _nombre_de_archivo_A_  
  
     FROM *copia_de_seguridad_de_archivo_A*  
  
     WITH NORECOVERY **;**  
  
2.  RESTORE DATABASE *base_de_datos* FILE **=** _nombre_de_archivo_B_ **,** _nombre_de_archivo_C_  
  
     FROM *copia_de_seguridad_de_archivos_B_y_C*  
  
     WITH RECOVERY **;**  
  
### <a name="examples"></a>Ejemplos  
  
-   [Ejemplo: restauración con conexión de un archivo de solo lectura &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Ejemplo: restauración sin conexión del grupo de archivos principal y de otro grupo de archivos &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/example-offline-restore-of-primary-and-one-other-filegroup-full-recovery-model.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para restaurar archivos y grupos de archivos**  
  
-   [Restaurar archivos y grupos de archivos en archivos existentes &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [Restaurar archivos y grupos de archivos &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Restaurar archivos y grupos de archivos &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
-   [Método Restore.SqlRestore (servidor) (SMO)](/dotnet/api/microsoft.sqlserver.management.smo.restore.sqlrestore)   
  
## <a name="see-also"></a>Consulte también  
 [Copias de seguridad y restauración: interoperabilidad y coexistencia &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-and-restore-interoperability-and-coexistence-sql-server.md)   
 [Copias de seguridad diferenciales &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Copias de seguridad de archivos completas &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)   
 [Información general de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Información general sobre restauración y recuperación &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Restauraciones de base de datos completas &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)   
 [Restauraciones por etapas &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
