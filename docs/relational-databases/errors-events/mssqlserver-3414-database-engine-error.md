---
description: MSSQLSERVER_3414
title: MSSQLSERVER_3414 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3414 (Database Engine error)
ms.assetid: f25852f9-b91c-4356-b817-78bec9ec8db4
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: fe68125b8b7b5d98e707a9e38d956d1d114db74e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195163"
---
# <a name="mssqlserver_3414"></a>MSSQLSERVER_3414
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|3414|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|REC_GIVEUP|  
|Texto del mensaje|Se ha producido un error durante la recuperación que impide reiniciar la base de datos '%.*ls' (id. de base de datos %d). Diagnostique los errores de recuperación y corríjalos, o restaure desde una copia de seguridad en buen estado. Si los errores no se han corregido o se espera que haya errores, póngase en contacto con el soporte técnico.|  
  
## <a name="explanation"></a>Explicación  
Se recuperó la base de datos especificada, pero no se inició porque se produjeron errores durante la recuperación. Este error ha colocado la base de datos en el estado SUSPECT. El grupo de archivos principal, y posiblemente otros grupos de archivos, es sospechoso y puede estar dañado. La base de datos no se puede recuperar durante el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, por consiguiente, no está disponible. Se requiere una acción por parte del usuario para resolver el problema. Verá el estado SUSPECT en SQL Server Management Studio (junto al icono de la base de datos) y en la columna sys.databases.state_desc. Cualquier intento de usar una base de datos en este estado producirá el siguiente error:

```
Msg 926, Level 14, State 1, Line 1 
Database 'mydb' cannot be opened. It has been marked SUSPECT by recovery. See the SQL Server errorlog for more information
```
  
Tenga en cuenta que, si este error se produce en **tempdb**, la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se cerrará.  

## <a name="cause"></a>Causa
Una condición transitoria que se dio en el sistema durante un intento determinado de iniciar la instancia del servidor o recuperar una base de datos puede producir este error. También puede deberse a un error permanente que se produce cada vez que se intenta iniciar la base de datos. La causa del error de recuperación suele encontrarse en los errores que preceden al error 3414 en el registro de errores o en el registro de eventos. El error anterior en el archivo de registro contiene el mismo valor spid<n>. Por ejemplo, el siguiente error de recuperación se debe a un error de suma de comprobación al intentar leer un bloque de registro. Tenga en cuenta que *spid15s* está presente en todas las líneas:

```
2020-03-31 17:33:13.00 spid15s     Error: 824, Severity: 24, State: 4.  
2020-03-31 17:33:13.00 spid15s     SQL Server detected a logical consistency-based I/O error: (bad checksum). It occurred during a read of page (0:-1) in database ID 13 at offset 0x0000000000b800 in file 'C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\mydb_log.LDF'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.   
2020-03-31 17:33:13.16 spid15s     Error: 3414, Severity: 21, State: 1.  
2020-03-31 17:33:13.16 spid15s     An error occurred during recovery, preventing the database 'mydb' (database ID 13) from restarting. Diagnose the recovery errors and fix them, or restore from a known good backup. If errors are not corrected or expected, contact Technical Support
```


Hay una amplia gama de errores que podrían provocar errores en la recuperación de la base de datos. Aunque debe evaluar cada error caso por caso, la resolución de un error de recuperación de la base de datos suele ser la misma que se describe en la sección Acción del usuario siguiente.

## <a name="user-action"></a>Acción del usuario  
 
Para obtener información sobre la causa de esta repetición del error 3414, busque en el registro de eventos o en el registro de errores de Windows un error anterior que indique el error específico. La acción del usuario adecuada depende de si la información del registro de eventos de Windows indica que el error de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo causó una condición transitoria o un error permanente. El mensaje de error indica que hay que "diagnosticar los errores de recuperación y corregirlos, o restaurar desde una copia de seguridad en buen estado". Por lo tanto, puede intentar corregir el error que encuentre para permitir que se complete la recuperación (consulte [Errores corregibles y transacciones diferidas](#correctable-errors-and-deferred-transactions)).

Si los errores no se pueden corregir, la primera y mejor opción para resolver este problema consistirá en restaurar una copia de seguridad correcta. Sin embargo, si no puede realizar la recuperación a partir de una copia de seguridad, tiene dos opciones adicionales, que no garantizan la recuperación completa de los datos: usar la reparación de emergencia con [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) o intentar copiar tantos datos como sea posible en otra base de datos. 

 1. Realice la restauración desde la última copia de seguridad de datos válida conocida.
 1. Use el método de reparación de emergencia proporcionado por [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).
 1. Intente copiar tantos datos como sea posible en otra base de datos.

El primer método para restaurar una copia de seguridad de datos válida es la mejor opción para tener la base de datos en un estado coherente conocido.  

La segunda opción, si no hay ninguna copia de seguridad disponible, es poner la base de datos en línea y accesible. Sin embargo, debe saber que no se puede garantizar la coherencia transaccional porque la recuperación ha fallado. No hay ninguna forma de saber qué transacciones deberían haberse revertido o puesto al día, pero no se han admitido porque la recuperación ha fallado. Los pasos para continuar con la reparación de emergencia se describen en la sección [Resolución de errores en modo de emergencia de base de datos](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md#resolving-errors-in-database-emergency-mode) de la documentación de DBCC CHECKDB. 

Si la reparación de emergencia no funciona y quiere intentar conservar algunos datos en otra base de datos, la forma de obtener acceso a ella es configurarla en modo de emergencia con el comando ALTER DATABASE <dbname> SET EMERGENCY. Después, puede intentar copiar los datos de las tablas.

### <a name="correctable-errors-and-deferred-transactions"></a>Errores corregibles y transacciones diferidas
No todos los errores detectados durante la recuperación de la base de datos producirán un error de recuperación y una base de datos sospechosa:

Los errores producidos al abrir la base de datos o los archivos de registro de transacciones realizados por primera vez se producen antes de la recuperación. Algunos ejemplos de estos errores son [17204](mssqlserver-17204-database-engine-error.md) y [17207](mssqlserver-17207-database-engine-error.md). Una vez corregidos estos errores, es posible que se permita la recuperación, aunque no está garantizado que se complete si se producen otros errores de recuperación. Los errores como 17204 y 17207 no dan como resultado una base de datos SUSPECT. De hecho, cuando se producen estos problemas, el estado de la base de datos es RECOVERY_PENDING. 

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite que se complete la recuperación incluso cuando se produce un error en el nivel de página y mantiene la coherencia transaccional. Este proceso ha reducido el número de escenarios, lo que ha dado como resultado una base de datos SUSPECT. Este concepto se conoce como [transacciones diferidas](../backup-restore/deferred-transactions-sql-server.md).

Si el error detectado durante la recuperación indica un problema con una página de base de datos, por ejemplo, como un error de suma de comprobación o el mensaje 824, puede que se permita que la recuperación se complete con errores pendientes. Si una transacción no está confirmada, un error en una página puede dar lugar a una [transacción diferida](../backup-restore/deferred-transactions-sql-server.md), lo que permite que se complete la recuperación.  

Las siguientes entradas ERRORLOG muestran un ejemplo de un mensaje de error [824](mssqlserver-824-database-engine-error.md) detectado durante la recuperación, pero se pudo completar la recuperación con una transacción diferida. Observe la ausencia del error 3414 en esta situación y el mensaje que indica que la recuperación de la base de datos se ha completado:

```2010-03-31 19:17:18.45 spid7s      Error: 824, Severity: 24, State: 2.   
2010-03-31 19:17:18.45 spid7s      SQL Server detected a logical consistency-based I/O error: incorrect checksum (expected: 0xb2c87a0a; actual: 0xb6c0a5e2). It occurred during a read of page (1:153) in database ID 13 at offset 0x00000000132000 in file 'C:\Program Files\Microsoft SQL Server\MSSQL10.SQL2008\MSSQL\DATA\mydb.mdf'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.   
2010-03-31 19:17:18.45 spid7s      Error: 3314, Severity: 21, State: 1.   
2010-03-31 19:17:18.45 spid7s      During undoing of a logged operation in database 'mydb', an error occurred at log record ID (25:100:19). Typically, the specific failure is logged previously as an error in the Windows Event Log service. Restore the database or file from a backup, or repair the database.
2010-03-31 19:17:18.45 spid7s      Errors occurred during recovery while rolling back a transaction. The transaction was deferred. Restore the bad page or file, and re-run recovery.   
2010-03-31 19:17:18.45 spid7s      Recovery completed for database mydb (database ID 13) in 2 second(s) (analysis 204 ms, redo 25 ms, undo 1832 ms.) This is an informational message only. No user action is required.   
```

Si se va a poner al día una transacción confirmada, la página se puede marcar como inaccesible (cualquier intento futuro de acceder a la página tiene como resultado el mensaje 829) y se puede completar la recuperación. En esta situación, se debe corregir el error restaurando la página a partir de una copia de seguridad o desasignando la página con DBCC CHECKDB con reparación.


  
## <a name="see-also"></a>Consulte también  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[DBCC CHECKDB &#40;Transact-SQL&#41;](~/t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
[Restauraciones de base de datos completas &#40;modelo de recuperación simple&#41;](~/relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
[Transacciones diferidas](../backup-restore/deferred-transactions-sql-server.md)  
[MSSQLSERVER_824](~/relational-databases/errors-events/mssqlserver-824-database-engine-error.md)  
[sys.databases &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-databases-transact-sql.md)

  
