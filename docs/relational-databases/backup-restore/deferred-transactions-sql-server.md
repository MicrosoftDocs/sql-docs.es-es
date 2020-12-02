---
title: Transacciones diferidas (SQL Server) | Microsoft Docs
description: Una transacción diferida de SQL Server Enterprise se produce si los datos necesarios para la reversión están sin conexión. Aprenda a sacarlas del estado diferido.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- I/O [SQL Server], database recovery
- restoring pages [SQL Server]
- deferred transactions
- modifying transaction deferred state
ms.assetid: 6fc0f9b6-d3ea-4971-9f27-d0195d1ff718
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8d748a5997df33ab9448fc9ba604ae700b87c0ee
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127034"
---
# <a name="deferred-transactions-sql-server"></a>Transacciones diferidas (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise, una transacción dañada puede ser diferida si los datos necesarios para la reversión se encuentran sin conexión durante el inicio de la base de datos. Una *transacción diferida* es aquella que está sin confirmar cuando termina la fase de puesta al día y que se encuentra con un error que impide que se revierta. Si la transacción no se puede revertir, se convierte en una transacción diferida.  
  
> [!NOTE]  
>  Las transacciones dañadas solo se difieren en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise. En otras versiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], una transacción dañada hace que se produzca un error de inicio.  
  
 Normalmente, las transacciones diferidas se producen porque, cuando la base de datos se estaba poniendo al día, un error de E/S impidió que se leyera una página necesaria para la transacción. Sin embargo, un error en el nivel de archivo también puede provocar transacciones diferidas. También puede producirse una transacción diferida cuando una secuencia de restauración parcial se detiene en un punto donde se requiere la reversión de transacciones y una transacción necesita datos que se encuentran sin conexión.  
  
 Las transacciones de usuario que se están revirtiendo y se encuentran un error de E/S hacen que toda la base de datos se quede sin conexión. Cuando la base de datos está en línea de nuevo, la acción de rehacer vuelve a adquirir todos los bloqueos que tenía e intenta revertir todas las transacciones no confirmadas. Todos los datos modificados por una transacción permanecen adecuadamente bloqueados hasta que la transacción se puede revertir. Las transacciones que no pueden revertirse renuncian a sus bloqueos cuando los daños se arreglan y la base de datos se reinicia o, después de una restauración en línea, cuando las transacciones diferidas se resuelven mientras la base de datos está en línea. Hasta ese momento, una transacción diferida puede mantener los bloqueos que impiden ciertas operaciones en la base de datos en su totalidad. Por ejemplo, si una transacción diferida contiene una instrucción CREATE TABLE, ningún usuario puede crear una tabla hasta que la transacción diferida se haya resuelto.  
  
 Las transacciones diferidas pueden producirse también porque una restauración por etapas recupera una base de datos a un punto en que una o varias transacciones activas afectan a un grupo de archivos que todavía no se ha restaurado y sigue estando sin conexión. Como las transacciones no se pueden revertir, se convierten en transacciones diferidas.  
  
 En la siguiente tabla se enumeran las acciones que hacen que una base de datos lleve a cabo la recuperación y el resultado si se produce un problema de E/S.  
  
|Acción|Resolución (si se producen problemas de E/S o los datos necesarios están sin conexión)|  
|------------|-----------------------------------------------------------------------|  
|Iniciar el servidor|transacción diferida|  
|Restauración|transacción diferida|  
|Attach|Error en la operación de adjuntar|  
|Reinicio automático|transacción diferida|  
|Crear una base de datos o una instantánea de base de datos|Error en la creación|  
|Puesta al día en la creación de reflejo de la base de datos|transacción diferida|  
|Grupo de archivos sin conexión|transacción diferida|  
  
### <a name="requirements-and-limitations"></a>Requisitos y limitaciones

 - La base de datos debe usar el modelo de recuperación FULL o BULK-LOGGED.
 - Se debe haber completado al menos una copia de seguridad del registro de transacciones y de la base de datos para la base de datos.
 - Las transacciones aplazadas no se aplican a los errores detectados durante la reversión de una transacción cuando la base de datos está en línea (por ejemplo, error en tiempo de ejecución).
 - Las transacciones no se pueden diferir por errores de recuperación mientras se adjunta una base de datos.
 - Algunas transacciones, como las del sistema (por ejemplo, la asignación de páginas), no se pueden diferir.

## <a name="moving-a-transaction-out-of-the-deferred-state"></a>Eliminar el estado DEFERRED de una transacción  
  
> [!IMPORTANT]  
>  Las transacciones diferidas hacen que el registro de transacciones esté activo. Un archivo de registro virtual que contenga transacciones diferidas no se puede truncar hasta que dichas transacciones salen del estado diferido. Para obtener más información sobre el truncamiento de registros, vea [El registro de transacciones &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 Para hacer que la transacción salga del estado diferido, la base de datos debe iniciarse correctamente, sin ningún error de E/S. Si existen transacciones diferidas, debe corregir el origen de los errores de E/S. Las soluciones disponibles, que aparecen en la lista en el orden en que se suelen aplicar, son las siguientes:  
  
-   Reinicie la base de datos. Si el problema fue transitorio, la base de datos se iniciará sin transacciones diferidas.  
  
-   Si las transacciones se aplazaron debido a que el grupo de archivos estaba sin conexión, vuelva a ponerlo en línea.  
  
     Para volver poner en línea un grupo de archivos sin conexión utilice la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    RESTORE DATABASE database_name FILEGROUP=<filegroup_name>  
    ```  
  
-   Restaure la base de datos. Después de una restauración en línea, se resuelven todas las transacciones diferidas.  
  
     En el modelo de recuperación completa o el modelo de recuperación optimizado para cargas masivas de registros, si las transacciones diferidas se deben a unas pocas páginas dañadas, es posible que una restauración de las páginas en línea resuelva los errores (donde se admita).  
  
-   Si ya no necesita un grupo de archivos cuyo estado sin conexión crea transacciones diferidas, haga que el grupo de archivos sin conexión pase a estar inactivo. Las transacciones que estaban diferidas porque el grupo de archivos estaba sin conexión salen del estado diferido una vez que el grupo de archivos queda inactivo.  
  
    > [!IMPORTANT]  
    >  Un grupo de archivos inactivo no puede recuperarse.  
  
     Para obtener más información, vea [Quitar grupos de archivos inactivos &#40;SQL Server&#41;](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md).  
  
-   Si las transacciones estaban diferidas a causa de de una página dañada y no se dispone de buena copia de seguridad correcta de la base de datos, siga este procedimiento para reparar la base de datos:  
  
    -   Ponga la base de datos en modo de emergencia mediante la ejecución de la siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
        ```  
        ALTER DATABASE <database_name> SET EMERGENCY  
        ```  
  
         Para obtener información acerca del modo de emergencia, vea [Database States](../../relational-databases/databases/database-states.md).  
  
    -   Después, repare la base de datos mediante la opción DBCC REPAIR_ALLOW_DATA_LOSS de una de las siguientes instrucciones DBCC: [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md), [DBCC CHECKALLOC](../../t-sql/database-console-commands/dbcc-checkalloc-transact-sql.md) o [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).  
  
         Cuando encuentre la página dañada, DBCC la desasignará y reparará cualquier error relacionado. Este enfoque permite volver a poner en línea la base de datos en un estado físicamente coherente. Sin embargo, también podrían perderse otros datos, por lo que este enfoque debe ser el último recurso.  
  
## <a name="see-also"></a>Consulte también  
 [Información general sobre restauración y recuperación &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [Quitar grupos de archivos inactivos &#40;SQL Server&#41;](../../relational-databases/backup-restore/remove-defunct-filegroups-sql-server.md)   
 [Restauraciones de archivos &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Restauraciones de archivos &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Restaurar páginas &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Restauraciones por etapas &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
