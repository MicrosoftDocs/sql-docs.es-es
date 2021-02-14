---
title: Movimiento de las bases de datos del servidor de informes a otro equipo (modo nativo) | Microsoft Docs
description: Las bases de datos del servidor de informes que se emplean en una instalación de Motor de base de datos de SQL Server se pueden mover a una instancia que se encuentre en otro equipo.
ms.date: 12/16/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: 44a9854d-e333-44f6-bdc7-8837b9f34416
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 58d7a46df8eb6580961d54e02f3ee6b0c57ee32f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059342"
---
# <a name="moving-report-server-databases-to-another-computer-ssrs-native-mode"></a>Movimiento de las bases de datos del servidor de informes a otro equipo (modo nativo de SSRS)

  Las bases de datos del servidor de informes que se emplean en una instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] se pueden mover a una instancia que se encuentre en otro equipo. Las bases de datos reportserver y reportservertempdb se deben mover o copiar en conjunto. Una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] requiere las dos bases de datos; la base de datos reportservertempdb debe estar relacionada por nombre con la base de datos reportserver principal que se vaya a mover.  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Modo nativo.  
  
 Mover una base de datos no afecta a las operaciones programadas que están actualmente definidas para los elementos del servidor de informes.  
  
-   Las programaciones se volverán a crear la primera vez que se reinicie el servicio del servidor de informes.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Los trabajos de agente que se utilizan para activar una programación se volverán a crear en la nueva instancia de la base de datos. No tiene que mover los trabajos al nuevo equipo, pero quizá desee eliminar los trabajos del equipo que ya no se va a utilizar.  
  
-   Las suscripciones, los informes almacenados en caché y las instantáneas se mantienen en la base de datos que se ha movido. Si una instantánea no está recopilando los datos actualizados después de que se mueve la base de datos, borre las opciones de instantánea, elija **Aplicar** para guardar los cambios, vuelva a crear la programación y seleccione **Aplicar** de nuevo para guardar los cambios.  
  
-   Los datos temporales de informes y de sesión de usuario que se almacenan en reportservertempdb permanecen al mover esa base de datos.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona varios métodos para mover bases de datos, como las operaciones de copia de seguridad y restauración, adjuntar y separar, y copiar. No todos los métodos son adecuados para reubicar una base de datos existente en una instancia de servidor nueva. El método recomendado para mover una base de datos del servidor de informes varía en función de sus requisitos de disponibilidad del sistema. La manera más sencilla de mover bases de datos del servidor de informes es adjuntarlas y separarlas. Sin embargo, este método requiere que el servidor de informes esté sin conexión mientras se separa la base de datos. Las operaciones de copia de seguridad y restauración son la opción más adecuada si se desean reducir al mínimo las interrupciones del servicio, pero deberán ejecutarse comandos [!INCLUDE[tsql](../../includes/tsql-md.md)] para realizarlas. No se recomienda copiar la base de datos, en especial mediante el Asistente para copiar bases de datos, ya que no se conserva la configuración de permisos de la base de datos.  
  
> [!IMPORTANT]  
>  Los pasos que se proporcionan en este artículo se recomiendan cuando la modificación de la ubicación de la base de datos del servidor de informes es el único cambio que se realiza en la instalación existente. Para migrar una instalación completa de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (es decir, mover la base de datos y cambiar la identidad del servicio Windows del servidor de informes que usa la base de datos), es necesario volver a configurar la conexión y restablecer una clave de cifrado.  
  
## <a name="detaching-and-attaching-the-report-server-databases"></a>Separar y adjuntar bases de datos del servidor de informes  
 Si puede poner el servidor de informes sin conexión, puede separar las bases de datos para moverlas a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desea usar. Con este método, se conservan los permisos de las bases de datos. Si usa una base de datos de SQL Server, debe moverla a otra instancia de SQL Server. Después de mover las bases de datos, será necesario volver a configurar la conexión del servidor de informes con la base de datos del servidor de informes. Si se utiliza una implementación escalada, será necesario volver a configurar la conexión de la base de datos del servidor de informes para cada servidor de informes de la implementación.  
  
 Lleve a cabo los siguientes pasos para mover las bases de datos:  
  
1.  Haga una copia de seguridad de las claves de cifrado para la base de datos del servidor de informes que desea mover. Puede utilizar la herramienta de configuración [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para hacer una copia de seguridad de las claves.  
  
2.  Detenga el servicio del servidor de informes. Puede utilizar la herramienta de configuración [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para detener el servicio.  
  
3.  Inicie [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y abra una conexión con la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hospeda las bases de datos del servidor de informes.  
  
4.  Haga clic con el botón derecho en la base de datos del servidor de informes, seleccione Tareas y haga clic en **Separar**. Repita este paso para la base de datos temporal del servidor de informes.  
  
5.  Copie o mueva los archivos .mdf y .ldf a la carpeta de datos de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desea utilizar. Dado que se van a mover dos bases de datos, asegúrese de que mueve o copia los cuatro archivos.  
  
6.  En [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], abra una conexión con la nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que alojará las bases de datos del servidor de informes.  
  
7.  Haga clic con el botón derecho en el nodo Bases de datos y luego haga clic en **Adjuntar**.  
  
8.  Haga clic en **Agregar** para seleccionar los archivos .mdf y .ldf de la base de datos del servidor de informes que desea adjuntar. Repita este paso para la base de datos temporal del servidor de informes.  
  
9. Una vez adjuntadas las bases de datos, compruebe que **RSExecRole** existe como rol de la base de datos en la base de datos del servidor de informes y en la base de datos temporal. **RSExecRole** debe tener permisos de selección, inserción, actualización, eliminación y referencia en las tablas de base de datos del servidor de informes, además de permisos de ejecución en los procedimientos almacenados. Para obtener más información, vea [Crear el RSExecRole](../../reporting-services/security/create-the-rsexecrole.md).  
  
10. Inicie la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y abra una conexión con el servidor de informes.  
  
11. En la página Base de datos, seleccione la nueva instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y haga clic en **Conectar**.  
  
12. Seleccione la base de datos del servidor de informes que acaba de mover y haga clic en **Aplicar**.  
  
13. En la página Claves de cifrado, haga clic en Restaurar. Especifique el archivo que contiene la copia de seguridad de las claves y la contraseña para desbloquear el archivo.  
  
14. Reinicie el servicio del servidor de informes.  
  
## <a name="backing-up-and-restoring-the-report-server-databases"></a>Realizar copias de seguridad de las bases de datos del servidor de informes y restaurarlas  
 Si no es posible que el servidor de informes esté sin conexión, puede realizar una copia de seguridad de las bases de datos del servidor de informes y restaurarlas para cambiar su ubicación. Debe usar instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] para realizar copias de seguridad y restauraciones. Después de restaurar las bases de datos, debe configurar el servidor de informes para que utilice la base de datos en la nueva instancia del servidor. Para obtener más información, vea las instrucciones que se incluyen al final de este tema.  
  
### <a name="using-backup-and-copy_only-to-backup-the-report-server-databases"></a>Usar BACKUP y COPY_ONLY para la copia de seguridad de las bases de datos del servidor de informes  
 Cuando realice copias de seguridad de las bases de datos, establezca el argumento COPY_ONLY. Asegúrese de incluir ambas bases de datos y los archivos de registro en la copia de seguridad.  
  
```  
-- To permit log backups, before the full database backup, alter the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE ReportServer  
   SET RECOVERY FULL  
  
-- If the ReportServerData device does not exist yet, create it.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerData',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\ReportServerData.bak'  
  
-- Create a logical backup device, ReportServerLog.  
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerLog',   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\BACKUP\ReportServerLog.bak'  
  
-- Back up the full ReportServer database.  
BACKUP DATABASE ReportServer  
   TO ReportServerData  
   WITH COPY_ONLY  
  
-- Back up the ReportServer log.  
BACKUP LOG ReportServer  
   TO ReportServerLog  
   WITH COPY_ONLY  
  
-- To permit log backups, before the full database backup, alter the database   
-- to use the full recovery model.  
USE master;  
GO  
ALTER DATABASE ReportServerTempdb  
   SET RECOVERY FULL  
  
-- If the ReportServerTempDBData device does not exist yet, create it.   
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerTempDBData',   
'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\BACKUP\ReportServerTempDBData.bak'  
  
-- Create a logical backup device, ReportServerTempDBLog.  
USE master  
GO  
EXEC sp_addumpdevice 'disk', 'ReportServerTempDBLog',   
'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\BACKUP\ReportServerTempDBLog.bak'  
  
-- Back up the full ReportServerTempDB database.  
BACKUP DATABASE ReportServerTempDB  
   TO ReportServerTempDBData  
   WITH COPY_ONLY  
  
-- Back up the ReportServerTempDB log.  
BACKUP LOG ReportServerTempDB  
   TO ReportServerTempDBLog  
   WITH COPY_ONLY  
```  
  
### <a name="using-restore-and-move-to-relocate-the-report-server-databases"></a>Usar RESTORE y MOVE para cambiar la ubicación de las bases de datos del servidor de informes  
 Al restaurar las bases de datos, no olvide incluir el argumento MOVE para especificar una ruta de acceso. Use el argumento NORECOVERY para realizar la restauración inicial; de esta manera, la base de datos se mantendrá en estado RESTORING y tendrá tiempo para revisar las copias de seguridad de los registros y determinar cuál desea restaurar. En el último paso se repite la operación RESTORE con el argumento RECOVERY.  
  
 El argumento MOVE utiliza el nombre lógico del archivo de datos. Para encontrar el nombre lógico, ejecute la siguiente instrucción: `RESTORE FILELISTONLY FROM DISK='C:\ReportServerData.bak';`  
  
 En los ejemplos siguientes se incluye el argumento FILE para poder especificar la posición del archivo de registro que se desea restaurar. Para encontrar la posición del archivo, ejecute la siguiente instrucción: `RESTORE HEADERONLY FROM DISK='C:\ReportServerData.bak';`  
  
 Al restaurar la base de datos y los archivos de registro, cada operación RESTORE se debe ejecutar por separado.  
  
```  
-- Restore the report server database and move to new instance folder   
RESTORE DATABASE ReportServer  
   FROM DISK='C:\ReportServerData.bak'  
   WITH NORECOVERY,   
      MOVE 'ReportServer' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer.mdf',   
      MOVE 'ReportServer_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer_Log.ldf';  
GO  
  
-- Restore the report server log file to new instance folder   
RESTORE LOG ReportServer  
   FROM DISK='C:\ReportServerData.bak'  
   WITH NORECOVERY, FILE=2  
      MOVE 'ReportServer' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer.mdf',   
      MOVE 'ReportServer_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServer_Log.ldf';  
GO  
  
-- Restore and move the report server temporary database  
RESTORE DATABASE ReportServerTempdb  
   FROM DISK='C:\ReportServerTempDBData.bak'  
   WITH NORECOVERY,   
      MOVE 'ReportServerTempDB' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServerTempDB.mdf',   
      MOVE 'ReportServerTempDB_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\REportServerTempDB_Log.ldf';  
GO  
  
-- Restore the temporary database log file to new instance folder   
RESTORE LOG ReportServerTempdb  
   FROM DISK='C:\ReportServerTempDBData.bak'  
   WITH NORECOVERY, FILE=2  
      MOVE 'ReportServerTempDB' TO   
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\ReportServerTempDB.mdf',   
      MOVE 'ReportServerTempDB_log' TO  
         'C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Data\REportServerTempDB_Log.ldf';  
GO  
  
-- Perform final restore  
RESTORE DATABASE ReportServer  
   WITH RECOVERY  
GO  
  
-- Perform final restore  
RESTORE DATABASE ReportServerTempDB  
   WITH RECOVERY  
GO  
```  
  
### <a name="how-to-configure-the-report-server-database-connection"></a>Configurar la conexión de la base de datos del servidor de informes  
  
1.  Inicie el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y abra una conexión con el servidor de informes.  
  
2.  En la página Base de datos, haga clic en **Cambiar base de datos**. Haga clic en **Next**.  
  
3.  Haga clic en **Elija una base de datos del servidor de informes existente**. Haga clic en **Next**.  
  
4.  Seleccione [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ahora aloja la base de datos del servidor de informes y haga clic en **Probar conexión**. Haga clic en **Next**.  
  
5.  En Nombre de la base de datos, seleccione la base de datos del servidor de informes que desea utilizar. Haga clic en **Next**.  
  
6.  En Credenciales, especifique las credenciales que utilizará el servidor de informes para conectarse a la base de datos del servidor de informes. Haga clic en **Next**.  
  
7.  Haga clic en **Siguiente** y, a continuación, en **Finalizar**.  
  
> [!NOTE]  
>  Una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] requiere que la instancia [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] incluya el rol **RSExecRole** . La creación de roles, el registro de inicio de sesión y las asignaciones de roles tienen lugar cuando se establece la conexión de la base de datos del servidor de informes a través de la herramienta de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si se utilizan métodos alternativos (concretamente, si se utiliza la herramienta del símbolo del sistema rsconfig.exe) para configurar la conexión, el servidor de informes no estará en estado de funcionamiento. Es posible que tenga que escribir código WMI para que el servidor de informes esté disponible. Para obtener más información, vea [Obtener acceso al proveedor WMI de Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  

## <a name="next-steps"></a>Pasos siguientes

[Crear el RSExecRole](../../reporting-services/security/create-the-rsexecrole.md)   
[Iniciar y detener el servicio del servidor de informes](../../reporting-services/report-server/start-and-stop-the-report-server-service.md)   
[Configurar una conexión a la base de datos del servidor de informes](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)   
[Configurar la cuenta de ejecución desatendida](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)   
[Administrador de configuración del servidor de informes](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
[Utilidad rsconfig](../../reporting-services/tools/rsconfig-utility-ssrs.md)   
[Configurar y administrar las claves de cifrado](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)   
[Base de datos del servidor de informes](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
