---
title: Copia de seguridad de un registro de transacciones | Microsoft Docs
description: En este artículo se describe cómo realizar una copia de seguridad de un registro de transacciones en SQL Server mediante SQL Server Management Studio, Transact-SQL o PowerShell.
ms.custom: ''
ms.date: 02/02/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- transaction log backups [SQL Server], SQL Server Management Studio
- backups [SQL Server], creating
- backing up transaction logs [SQL Server], SQL Server Management Studio
ms.assetid: 3426b5eb-6327-4c7f-88aa-37030be69fbf
author: cawrites
ms.author: chadam
ms.openlocfilehash: e66f271830e686af36b450e27db2b5c98ad41244
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075947"
---
# <a name="back-up-a-transaction-log"></a>Copia de seguridad de un registro de transacciones
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo realizar una copia de seguridad de un registro de transacciones en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o PowerShell.  

## <a name="before-you-begin"></a>Antes de empezar
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
La instrucción `BACKUP` no se permite en una transacción explícita o [implícita](../../t-sql/statements/set-implicit-transactions-transact-sql.md). Una transacción explícita es aquella en que se define explícitamente el inicio y el final de la transacción.

### <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
- Si una base de datos usa el [modelo de recuperación](recovery-models-sql-server.md) optimizado para cargas masivas de registros o completo, debe hacer una copia de seguridad del registro de transacciones con suficiente regularidad para proteger los datos e impedir que el [registro de transacciones se llene](../logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md). De este modo se trunca el registro y se admite la restauración de la base de datos a un momento concreto. 
  
- De forma predeterminada, cada operación de copia de seguridad correcta agrega una entrada en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y en el registro de eventos del sistema. Si se hace una copia de seguridad del registro con frecuencia, estos mensajes que indican la corrección de la operación pueden acumularse rápidamente, lo que da lugar a registros de errores muy grandes que pueden dificultar la búsqueda de otros mensajes. En esos casos, puede suprimir estas entradas de registro usando la marca de seguimiento 3226 si ninguno de los scripts depende de esas entradas. Vea [Marcas de seguimiento &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  
  
### <a name="permissions"></a><a name="Permissions"></a> Permisos

De forma predeterminada, los permisos `BACKUP DATABASE` y `BACKUP LOG` necesarios se conceden a los miembros del rol fijo de servidor **sysadmin** y de los roles fijos de base de datos **db_owner** y **db_backupoperator**. Compruebe los permisos correctos antes de empezar.
  
 Los problemas de propiedad y permisos del archivo físico del dispositivo de copia de seguridad pueden interferir con una operación de copia de seguridad. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder leer y escribir en el dispositivo y la cuenta en la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener permisos de escritura. En cambio, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), que agrega una entrada para un dispositivo de copia de seguridad en las tablas del sistema, no comprueba los permisos de acceso a los archivos. Es posible que los problemas con los permisos del archivo físico del dispositivo de copia de seguridad no sean evidentes para usted hasta que intente tener acceso al [recurso físico](backup-devices-sql-server.md) al tratar de restaurar o realizar una copia de seguridad. Por tanto, una vez más, compruebe los permisos antes de empezar.

## <a name="using-sql-server-management-studio"></a>Uso de SQL Server Management Studio

1. Después de conectarse a la instancia apropiada de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol de servidores.  
  
1. Expanda **Bases de datos** y, en función de la base de datos, seleccione la base de datos de un usuario o expanda **Bases de datos del sistema** y seleccione una base de datos del sistema.  
  
1. Haga clic con el botón derecho en la base de datos, seleccione **Tareas** y haga clic en **Copia de seguridad**. Aparece el cuadro de diálogo **Copia de seguridad de base de datos** .  
 
1. En el cuadro de lista **Base de datos** , compruebe el nombre de la base de datos. También puede seleccionar otra base de datos en la lista.  
  
1. Compruebe que el modelo de recuperación sea **FULL** o **BULK_LOGGED**.  
  
1. En el cuadro de lista **Tipo de copia de seguridad** , seleccione **Registro de transacciones**.  
  
1. Opcionalmente, seleccione **Copia de seguridad de solo copia** para crear una copia de seguridad de solo copia. Una *copia de seguridad de solo copia* es una copia de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que es independiente de la secuencia de copias de seguridad convencionales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vea [Copias de seguridad de solo copia &#40;SQL Server&#41;](../../relational-databases/backup-restore/copy-only-backups-sql-server.md).  
  
    > [!NOTE]
    > Cuando la opción **Diferencial** está seleccionada, no puede crear una copia de seguridad de solo copia.  
  
1. Acepte el nombre del conjunto de copia de seguridad predeterminado sugerido en el cuadro de texto **Nombre** o especifique otro nombre.  
  
1. Opcionalmente, en el cuadro de texto **Descripción**, describa el conjunto de copia de seguridad.  
  
1. Especifique cuándo expirará el conjunto de copia de seguridad:  
  
    - Para que el conjunto de copia de seguridad expire al cabo de un número de días específico, haga clic en **Después de** (opción predeterminada) y escriba el número de días tras la creación del conjunto en que este expirará. Este valor puede estar entre 0 y 99999 días; el valor 0 significa que el conjunto de copia de seguridad no expirará nunca.  
  
         El valor predeterminado se establece en la opción **Tiempo predeterminado de retención de medios de copia de seguridad (días)** del cuadro de diálogo **Propiedades del servidor** (página **Configuración de base de datos** ). Para tener acceso a este cuadro de diálogo, haga clic con el botón derecho en el nombre del servidor en el Explorador de objetos y seleccione Propiedades. Después, seleccione la página **Configuración de base de datos** .  
  
    - Para que el conjunto de copia de seguridad expire en una determinada fecha, haga clic en **El** y escriba la fecha en la que expirará.  
  
1. Elija el tipo de destino de la copia de seguridad haciendo clic en **Disco**, **Dirección URL** o **Cinta**. Para seleccionar las rutas de acceso de hasta 64 unidades de disco o cinta que contienen un solo conjunto de medios, haga clic en **Agregar**. Las rutas seleccionadas se muestran en el cuadro de lista **Copia de seguridad en** .  
  
     Para eliminar un destino de copia de seguridad, selecciónelo y haga clic en **Quitar**. Para ver el contenido de un destino de copia de seguridad, selecciónelo y haga clic en **Contenido**.  
  
1. Para ver o seleccionar las opciones avanzadas, haga clic en **Opciones** , en el panel **Seleccionar una página** .  
  
1. Seleccione una opción de **Sobrescribir medios** ; para ello, haga clic en una de las opciones siguientes:  
  
    - **Hacer copia de seguridad en el conjunto de medios existente**  
  
         Para esta opción, haga clic en **Anexar al conjunto de copia de seguridad existente** o **Sobrescribir todos los conjuntos de copia de seguridad existentes**. Vea [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
         - Opcionalmente, seleccione **Comprobar nombre de conjunto de medios y fecha de expiración del conjunto de copia de seguridad** para que la operación de copia de seguridad compruebe la fecha y la hora en que expiran el conjunto de medios y el conjunto de copia de seguridad.  
  
         - También puede escribir un nombre en el cuadro de texto **Nombre del conjunto de medios**. Si no especifica ningún nombre, se creará un conjunto de medios con un nombre en blanco. Si especifica un nombre para el conjunto, los medios (cinta o disco) se comprueban para ver si el nombre real coincide con el nombre especificado aquí.  
  
         Si deja el nombre del conjunto de medios en blanco y selecciona la casilla para comprobarlo con los medios, el resultado correcto significará que el nombre del conjunto en los medios también está en blanco.  
  
    - **Hacer copia de seguridad en un nuevo conjunto de medios y borrar todos los conjuntos de copia de seguridad existentes**  
  
         Para esta opción, especifique un nombre en el cuadro de texto **Nuevo nombre del conjunto de medios** y, si lo desea, describa el conjunto de medios en el cuadro de texto **Nueva descripción del conjunto de medios**. Vea [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
1. Opcionalmente, en la sección **Confiabilidad** , seleccione:  
  
    - **Comprobar copia de seguridad al finalizar**.  
  
    - **Realizar suma de comprobación antes de escribir en los medios** y, si lo desea, **Continuar después de un error de suma de comprobación**.
    
       Para obtener más información sobre las sumas de comprobación, vea [Errores posibles de medios durante copia de seguridad y restauración &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
1. En la sección **Registro de transacciones** :  
  
    - Para las copias de seguridad rutinarias del registro, conserve la selección predeterminada **Truncar el registro de transacciones quitando las entradas inactivas**.  
  
    - Para hacer una copia de seguridad del final del registro (es decir, del registro activo), active **Realizar copia de seguridad del final del registro y dejar la base de datos en estado de restauración**.  
  
         Una copia del final del registro se lleva a cabo cuando no se consigue realizar la copia de seguridad del final de registro para impedir la pérdida de trabajo. Realice una copia de seguridad del registro activo (copia del final del registro) después de un error, antes de comenzar la restauración de la base de datos o cuando se conmuta por error a una base de datos secundaria. Esta opción equivale a especificar la opción NORECOVERY en la instrucción BACKUP LOG de Transact-SQL.
         
         Para obtener más información sobre las copias del final del registro, vea [Copias del final del registro &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
1. Si va a realizar copias de seguridad en una unidad de cinta (según se haya especificado en la sección **Destino** de la página **General** ), la opción **Descargar la cinta después de realizar la copia de seguridad** está activa. Al hacer clic en esta opción se activa la opción **Rebobinar la cinta antes de descargar** .  
  
1. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] y las versiones posteriores admiten la [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md). De forma predeterminada, el hecho de que se comprima una copia de seguridad depende del valor de la opción de configuración del servidor **backup-compression default** . Pero, independientemente del valor predeterminado actual de nivel de servidor, puede comprimir una copia de seguridad si activa **Comprimir copia de seguridad** e impedir la compresión si activa **No comprimir copia de seguridad**.  
  
     Para ver la compresión de copia de seguridad actual predeterminada, vea [Ver o establecer la opción de configuración del servidor de compresión de copia de seguridad predeterminada](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).
  
     Para cifrar el archivo de copia de seguridad, active la casilla **Cifrar copia de seguridad** . Seleccione un algoritmo de cifrado que utilizar para cifrar el archivo de copia de seguridad y proporcione un certificado o clave asimétrica. Los algoritmos disponibles para el cifrado son:  
  
   - AES 128  
  
   - AES 192  
  
   - AES 256  
  
   - Triple DES  

## <a name="using-transact-sql"></a>Usar Transact-SQL  
  
Ejecute la instrucción BACKUP LOG para realizar una copia de seguridad del registro de transacciones y especifique:  
  
- El nombre de la base de datos a la que pertenece el registro de transacciones del que se desea hacer una copia de seguridad.  
  
- El dispositivo de copia de seguridad en el que se va a escribir la copia de seguridad del registro de transacciones.  
  
> [!IMPORTANT]
> En este ejemplo se utiliza la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] , que utiliza el modelo de recuperación simple. Con el fin de permitir copias de seguridad de registros, antes de realizar una copia de seguridad completa de la base de datos, la base de datos se ha configurado para usar el modelo de recuperación completa.
>
> Para obtener más información, vea [Ver o cambiar el modelo de recuperación de una base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
 En este ejemplo se crea una copia de seguridad del registro de transacciones de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en el dispositivo de copia de seguridad creado anteriormente, `MyAdvWorks_FullRM_log1`.  
  
```sql  
BACKUP LOG AdventureWorks2012  
   TO MyAdvWorks_FullRM_log1;  
GO  
```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell

Configure y use el [Proveedor de SQL Server PowerShell](../../powershell/sql-server-powershell-provider.md). Use el cmdlet **Backup-SqlDatabase** y especifique **Log** como valor del parámetro **-BackupAction** .  
  
En el ejemplo siguiente se crea una copia de seguridad de registro de la base de datos `<myDatabase>` en la ubicación de copia de seguridad predeterminada de la instancia de servidor `Computer\Instance`.  
  
```powershell
Backup-SqlDatabase -ServerInstance Computer\Instance -Database <myDatabase> -BackupAction Log  
```
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
  
- [Restaurar una copia de seguridad de registros de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
- [Restaurar una base de datos de SQL Server a un momento dado &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
- [Solucionar problemas de un registro de transacciones lleno &#40;Error 9002 de SQL Server&#41;](../../relational-databases/logs/troubleshoot-a-full-transaction-log-sql-server-error-9002.md)  
  
## <a name="see-also"></a>Consulte también

 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Aplicar copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [Planes de mantenimiento](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Copias de seguridad de archivos completas &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)