---
title: Realizar una copia de seguridad del registro de transacciones cuando la base de datos está dañada (SQL Server) | Microsoft Docs
description: En este tema se describe cómo realizar la copia de seguridad de un registro de transacciones cuando la base de datos está dañada en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], damaged
- backing up [SQL Server]. damaged database
- transaction log backups [SQL Server], damaged databases
ms.assetid: 9b8873cc-df54-4336-ab9b-8f525132c2b0
author: cawrites
ms.author: chadam
ms.openlocfilehash: e03f51fa7a90e7a547e85f226148870043d89a0e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077093"
---
# <a name="back-up-the-transaction-log-when-the-database-is-damaged-sql-server"></a>Realizar una copia de seguridad del registro de transacciones cuando la base de datos está dañada (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo realizar la copia de seguridad de un registro de transacciones cuando la base de datos está dañada en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para realizar una copia de seguridad del registro de transacciones cuando la base de datos está dañada, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   La instrucción BACKUP no se permite en una transacción explícita o implícita.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   En una base de datos que utiliza el modelo de recuperación completo u optimizado para cargas masivas de registros, generalmente es necesario hacer una copia del final del registro antes de empezar a restaurar la base de datos. También debería hacer una copia del final del registro de la base de datos principal antes de conmutar por error a una configuración de trasvase de registros. Al restaurar la copia del final del registro como última copia de seguridad de registros antes de recuperar la base de datos, se evita perder el trabajo después de que se produzca un error. Para obtener más información sobre las copias del final del registro, vea [Copias del final del registro &#40;SQL Server&#41;](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, los permisos BACKUP DATABASE y BACKUP LOG corresponden a los miembros del rol fijo de servidor **sysadmin** y de los roles fijos de base de datos **db_owner** y **db_backupoperator** .  
  
 Los problemas de propiedad y permisos del archivo físico del dispositivo de copia de seguridad pueden interferir con una operación de copia de seguridad. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder leer y escribir en el dispositivo y la cuenta en la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener permisos de escritura. En cambio, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), que agrega una entrada para un dispositivo de copia de seguridad en las tablas del sistema, no comprueba los permisos de acceso a los archivos. Es posible que estos problemas con el archivo físico del dispositivo de copia de seguridad no aparezcan hasta que se tenga acceso al recurso físico, al intentar la copia de seguridad o la restauración.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-back-up-the-tail-of-the-transaction-log"></a>Para hacer una copia del final registro de transacciones  

1.  Después de conectarse a la instancia apropiada de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol correspondiente.  
  
2.  Expanda **Bases de datos** y, en función de la base de datos, seleccione la base de datos de un usuario o expanda **Bases de datos del sistema** y seleccione una base de datos del sistema.  
  
3.  Haga clic con el botón derecho en la base de datos, seleccione **Tareas** y haga clic en **Copia de seguridad**. Aparece el cuadro de diálogo **Copia de seguridad de base de datos** .  
  
4.  En el cuadro de lista **Base de datos** , compruebe el nombre de la base de datos. También puede seleccionar otra base de datos en la lista.  
  
5.  Compruebe que el modelo de recuperación sea **FULL** o **BULK_LOGGED**.  
  
6.  En el cuadro de lista **Tipo de copia de seguridad** , seleccione **Registro de transacciones**.  
  
7.  Anule la selección de la opción **Copia de seguridad de solo copia** .  
  
8.  En el área **Conjunto de copia de seguridad** , acepte el nombre del conjunto de copia de seguridad predeterminado sugerido en el cuadro de texto **Nombre** o especifique otro nombre.  
  
9. En el cuadro de texto **Descripción** , escriba una descripción para la copia del final del registro.  
  
10. Especifique cuándo expirará el conjunto de copia de seguridad:  
  
    -   Para que el conjunto de copia de seguridad expire al cabo de un número de días específico, haga clic en **Después de** (opción predeterminada) y escriba el número de días tras la creación del conjunto en que este expirará. Este valor puede estar entre 0 y 99999 días; el valor 0 significa que el conjunto de copia de seguridad no expirará nunca.  
  
         El valor predeterminado se establece en la opción **Tiempo predeterminado de retención de medios de copia de seguridad (días)** del cuadro de diálogo **Propiedades del servidor** (página **Configuración de base de datos** ). Para tener acceso a este cuadro de diálogo, haga clic con el botón derecho en el nombre del servidor en el Explorador de objetos y seleccione Propiedades. Después, seleccione la página **Configuración de base de datos** .  
  
    -   Para que el conjunto de copia de seguridad expire en una determinada fecha, haga clic en **El** y escriba la fecha en la que expirará.  
  
11. Elija el tipo de destino de la copia de seguridad haciendo clic en **Disco** o **Cinta**. Para seleccionar las rutas de acceso de hasta 64 unidades de disco o cinta que contienen un solo conjunto de medios, haga clic en **Agregar**. Las rutas seleccionadas se muestran en el cuadro de lista **Copia de seguridad en** .  
  
     Para eliminar un destino de copia de seguridad, selecciónelo y haga clic en **Quitar**. Para ver el contenido de un destino de copia de seguridad, selecciónelo y haga clic en **Contenido**.  
  
12. En la página **Opciones** , seleccione una opción de **Sobrescribir medios** haciendo clic en una de las opciones siguientes:  
  
    -   **Hacer copia de seguridad en el conjunto de medios existente**  
  
         Para esta opción, haga clic en **Anexar al conjunto de copia de seguridad existente** o **Sobrescribir todos los conjuntos de copia de seguridad existentes**.  
  
         Opcionalmente, seleccione **Comprobar nombre de conjunto de medios y fecha de expiración del conjunto de copia de seguridad** para que la operación de copia de seguridad compruebe la fecha y la hora en que expiran el conjunto de medios y el conjunto de copia de seguridad.  
  
         También puede escribir un nombre en el cuadro de texto **Nombre del conjunto de medios** . Si no especifica ningún nombre, se creará un conjunto de medios con un nombre en blanco. Si especifica un nombre para el conjunto, los medios (cinta o disco) se comprueban para ver si el nombre real coincide con el nombre especificado aquí.  
  
         Si deja el nombre del conjunto de medios en blanco y selecciona la casilla para comprobarlo con los medios, el resultado correcto significará que el nombre del conjunto en los medios también está en blanco.  
  
    -   **Hacer copia de seguridad en un nuevo conjunto de medios y borrar todos los conjuntos de copia de seguridad existentes**  
  
         Para esta opción, especifique un nombre en el cuadro de texto **Nuevo nombre del conjunto de medios** y, si lo desea, describa el conjunto de medios en el cuadro de texto **Nueva descripción del conjunto de medios** .  
  
     Para obtener más información sobre los conjuntos de medios, vea [Conjuntos de medios, familias de medios y conjuntos de copias de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md).  
  
13. Opcionalmente, en la sección **Confiabilidad** , seleccione:  
  
    -   **Comprobar copia de seguridad al finalizar**.  
  
    -   **Realizar suma de comprobación antes de escribir en los medios**.  
  
    -   **Continuar después de un error de suma de comprobación**  
  
     Para obtener más información sobre las sumas de comprobación, vea [Errores posibles de medios durante copia de seguridad y restauración &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
14. En la sección **Registro de transacciones** , active **Realizar copia del final del registro y dejar la base de datos en estado de restauración**.  
  
     Esto es equivalente a especificar la instrucción [BACKUP](../../t-sql/statements/backup-transact-sql.md) siguiente:  
  
     `BACKUP LOG <database_name> TO <backup_device> WITH NORECOVERY`  
  
    > [!IMPORTANT]  
    >  En el momento de la restauración, el cuadro de diálogo Restaurar base de datos muestra el tipo de una copia del final del registro como **Registro de transacciones (solo copia)** .  
  
15. Si va a realizar copias de seguridad en una unidad de cinta (según se haya especificado en la sección **Destino** de la página **General** ), la opción **Descargar la cinta después de realizar la copia de seguridad** está activa. Al hacer clic en esta opción se activa la opción **Rebobinar la cinta antes de descargar** .  
  
16. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] y las versiones posteriores admiten la [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md). De forma predeterminada, el hecho de que se comprima una copia de seguridad depende del valor de la opción de configuración del servidor **backup-compression default** . Pero, independientemente del valor predeterminado actual de nivel de servidor, puede comprimir una copia de seguridad si activa **Comprimir copia de seguridad** e impedir la compresión si activa **No comprimir copia de seguridad**.  
  
     **Para ver el valor predeterminado actual de la compresión de copia de seguridad**  
  
    -   [Ver o establecer la opción de configuración del servidor de compresión de copia de seguridad predeterminada](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-backup-of-the-currently-active-transaction-log"></a>Para crear una copia de seguridad del registro de transacciones activo  
  
1.  Ejecute la instrucción BACKUP LOG para realizar una copia de seguridad del registro de transacciones activo especificando:  
  
    -   El nombre de la base de datos a la que pertenece el registro de transacciones del que se va a hacer una copia de seguridad.  
  
    -   El dispositivo de copia de seguridad en el que se va a escribir la copia de seguridad del registro de transacciones.  
  
    -   La cláusula NO_TRUNCATE.  
  
         Esta cláusula permite realizar una copia de seguridad de la parte activa del registro de transacciones aunque no se tenga acceso a la base de datos, siempre y cuando se pueda tener acceso al archivo del registro de transacciones y éste no esté dañado.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
  
> [!NOTE]  
>  En este ejemplo se utiliza [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], que usa el modelo de recuperación simple. Con el fin de permitir copias de seguridad de registros, antes de realizar una copia de seguridad completa de la base de datos, la base de datos se ha configurado para usar el modelo de recuperación completa. Para obtener más información, vea [Ver o cambiar el modelo de recuperación de una base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server.md).  
  
 En este ejemplo se realiza el registro de transacciones actualmente activo cuando una base de datos está dañada e inaccesible, si el registro de transacciones no está dañado y está accesible.  
  
```sql  
BACKUP LOG AdventureWorks2012  
   TO MyAdvWorks_FullRM_log1  
   WITH NO_TRUNCATE;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Restaurar una copia de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [Restaurar una base de datos de SQL Server a un momento dado &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)   
 [Copia de seguridad de la base de datos &#40;página Opciones de copia de seguridad&#41;](../../relational-databases/backup-restore/back-up-database-backup-options-page.md)   
 [Copia de seguridad de base de datos &#40;página General&#41;](../../relational-databases/backup-restore/back-up-database-general-page.md)   
 [Aplicar copias de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Restauraciones de archivos &#40;modelo de recuperación simple&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Restauraciones de archivos &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)  
  
  
