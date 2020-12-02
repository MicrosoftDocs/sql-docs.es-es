---
title: 'Restauración por etapas: modelo de recuperación completa'
description: En este ejemplo se muestra una restauración por etapas en SQL Server de una base de datos con el modelo de recuperación completa, a partir de una copia del final del registro.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full recovery model [SQL Server], RESTORE example
- piecemeal restores [SQL Server], full recovery model
- restore sequences [SQL Server], piecemeal
ms.assetid: 0a84892d-2f7a-4e77-b2d0-d68b95595210
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0da932c7e2834074cece2224c1bdb94bc756caf8
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130411"
---
# <a name="example-piecemeal-restore-of-database-full-recovery-model"></a>Ejemplo: Restauración por etapas de la base de datos (modelo de recuperación completa)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En una secuencia de restauración por etapas se restaura y recupera una base de datos en fases en el nivel del grupo de archivos, empezando con los grupos de archivos principales, los de lectura y escritura, y los secundarios.  
  
 En este ejemplo, la base de datos `adb` se restaura en un nuevo equipo después de un desastre. La base de datos está utilizando el modelo de recuperación completa, por lo que, antes de iniciar la restauración, debe crearse una copia del final del registro de la base de datos. Antes del desastre, todos los grupos de archivos están en línea. El grupo de archivos `B` es de solo lectura. Se deben restaurar todos los grupos de archivos secundarios, pero por orden de importancia: `A` (el de mayor importancia), `C`y, por último, `B`. En este ejemplo, hay cuatro copias de seguridad de registros, incluida la copia del final del registro.  
  
## <a name="tail-log-backup"></a>Copia del final del registro  
 Antes de restaurar la base de datos, el administrador de la base de datos debe realizar una copia de seguridad de registros después del error. Puesto que la base de datos está dañada, es necesario usar la opción NO_TRUNCATE al realizar la copia del final del registro:  
  
```  
BACKUP LOG adb TO tailLogBackup WITH NORECOVERY, NO_TRUNCATE  
```  
  
 La copia del final del registro es la última copia de seguridad que se aplica en las secuencias de restauración siguientes.  
  
## <a name="restore-sequences"></a>Secuencias de restauración  
  
> [!NOTE]  
>  La sintaxis de un flujo de restauración en línea es la misma que la de un flujo de restauración sin conexión.  
  
1.  Restauración parcial del grupo de archivos principal y secundario `A`.  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='Primary' FROM backup1   
       WITH PARTIAL, NORECOVERY  
    RESTORE DATABASE adb FILEGROUP='A' FROM backup2   
       WITH NORECOVERY  
    RESTORE LOG adb FROM log_backup3 WITH NORECOVERY  
    RESTORE LOG adb FROM log_backup4 WITH NORECOVERY  
    RESTORE LOG adb FROM log_backup5 WITH NORECOVERY  
    RESTORE LOG adb FROM tailLogBackup WITH RECOVERY  
    ```  
  
2.  Restauración con conexión del grupo de archivos `C`.  
  
     En este momento, el grupo de archivos principal y el secundario `A` están en línea. Todos los archivos de los grupos de archivos `B` y `C` están pendientes de recuperación y sin conexión.  
  
     Los mensajes de la última instrucción `RESTORE LOG` del paso 1 indican que la reversión de las transacciones en las que interviene el grupo de archivos `C` se ha diferido porque este grupo de archivos no está disponible. Las operaciones periódicas pueden continuar, pero estas transacciones mantienen los bloqueos y no se truncará el registro hasta que se pueda completar la reversión.  
  
     En la segunda secuencia de restauración, el administrador de la base de datos restaura el grupo de archivos `C`:  
  
    ```  
    RESTORE DATABASE adb FILEGROUP='C' FROM backup2a WITH NORECOVERY  
    RESTORE LOG adb FROM log_backup3 WITH NORECOVERY  
    RESTORE LOG adb FROM log_backup4 WITH NORECOVERY  
    RESTORE LOG adb FROM log_backup5 WITH NORECOVERY  
    RESTORE LOG adb FROM tailLogBackup WITH RECOVERY  
    ```  
  
     En este momento, los grupos de archivos principal, `A` y `C` están en línea. Los archivos del grupo `B` permanecen pendientes de recuperación, con el grupo de archivos sin conexión. Las transacciones diferidas se han resuelto y se trunca el registro.  
  
3.  Restauración con conexión del grupo de archivos `B`.  

   En la tercera secuencia de restauración, el administrador de la base de datos restaura el grupo de archivos `B`. La copia de seguridad del grupo de archivos `B` se realizó después de cambiar el grupo a solo lectura, por lo que no es necesario ponerlo al día durante la recuperación.  
  
   ```sql  
   RESTORE DATABASE adb FILEGROUP='B' FROM backup2b WITH RECOVERY  
   ```  
  
   Todos los grupos de archivos están ahora en línea.  
  
## <a name="additional-examples"></a>Otros ejemplos  
  
-   [Ejemplo: restauración por etapas de la base de datos &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [Ejemplo: restauración por etapas exclusiva para algunos grupos de archivos &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [Ejemplo: restauración con conexión de un archivo de solo lectura &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [Ejemplo: restauración por etapas exclusiva para algunos grupos de archivos &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [Ejemplo: restauración con conexión de un archivo de lectura/escritura &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [Ejemplo: restauración con conexión de un archivo de solo lectura &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a>Consulte también  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Restauración con conexión &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Aplicar copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Restauraciones por etapas &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  
