---
title: Copia de seguridad diferencial
description: En este artículo se muestra cómo crear una copia de seguridad diferencial de la base de datos en SQL Server con SQL Server Management Studio o Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full differential backups [SQL Server]
- database backups [SQL Server], full differential backups
- backing up databases [SQL Server], full differential backups
- backups [SQL Server], creating
ms.assetid: 70f49794-b217-4519-9f2a-76ed61fa9f99
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5a59b05df4805b580aade70a9d2bf11cc3399f04
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076127"
---
# <a name="create-a-differential-database-backup-sql-server"></a>Crear una copia de seguridad diferencial de una base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Crear una copia de seguridad diferencial de base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] con [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **Secciones de este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Requisitos previos](#Prerequisites)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para crear una copia de seguridad de una base de datos diferencial, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de empezar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   La instrucción BACKUP no se permite en una transacción explícita o implícita.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   La creación de una copia de seguridad diferencial de base de datos requiere una copia de seguridad de base de datos completa previa. Si nunca se ha hecho una copia de seguridad de la base de datos, realice una copia de seguridad completa de base de datos antes de crear la copia de seguridad diferencial. Para obtener más información, vea [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   A medida que se incrementa el tamaño de las copias de seguridad diferenciales, la restauración de una copia de seguridad diferencial aumentará significativamente el tiempo necesario para restaurar una base de datos. Se recomienda que realice una nueva copia de seguridad completa a intervalos definidos para establecer una nueva base diferencial para los datos. Por ejemplo, cada semana podría realizar una copia de seguridad completa de toda la base de datos (es decir, una copia de seguridad completa de la base de datos) seguida de una serie de copias de seguridad diferenciales de la base de datos realizadas periódicamente durante la semana.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="check-your-permissions-first"></a><a name="Permissions"></a> Compruebe los permisos en primer lugar.  
 De manera predeterminada, los permisos de BACKUP DATABASE y BACKUP LOG corresponden a los miembros del rol fijo de servidor **sysadmin** y de los roles fijos de base de datos **db_owner** y **db_backupoperator** .  
  
 Los problemas de propiedad y permisos del archivo físico del dispositivo de copia de seguridad interferirán con una operación de copia de seguridad. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder leer y escribir en el dispositivo, y la cuenta en la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener permisos de escritura. En cambio, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), que agrega una entrada para un dispositivo de copia de seguridad en las tablas del sistema, **no** comprueba los permisos de acceso a los archivos. Los problemas con los permisos del archivo físico del dispositivo de copia de seguridad no serán evidentes hasta que se acceda al recurso físico al intentar la copia de seguridad o la restauración.  
  
##  <a name="sql-server-management-studio"></a><a name="SSMSProcedure"></a> SQL Server Management Studio  
  
#### <a name="create-a-differential-database-backup"></a>Crear una copia de seguridad diferencial de una base de datos  

1.  Después de conectarse a la instancia apropiada de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol correspondiente.  
  
2.  Expanda **Bases de datos** y, dependiendo de la base de datos, seleccione una base de datos de usuario o expanda **Bases de datos del sistema** y seleccione una base de datos del sistema.  
  
3.  Haga clic con el botón derecho en la base de datos, seleccione **Tareas** y haga clic en **Copia de seguridad**. Aparece el cuadro de diálogo **Copia de seguridad de base de datos** .  
  
4.  En el cuadro de lista **Base de datos** , compruebe el nombre de la base de datos. También puede seleccionar otra base de datos en la lista.  
  
     Puede realizar una copia de seguridad diferencial para cualquier modelo de recuperación (completa, registro masivo o simple).  
  
5.  En el cuadro de lista **Tipo de copia de seguridad** , seleccione **Diferencial**.  
  
    > [!IMPORTANT]  
    >  Una vez seleccionada la opción **Diferencial** , compruebe que esté desactivada la casilla **Copia de seguridad de solo copia** .  
  
6.  En **Componente de copia de seguridad**, haga clic en **Base de datos**.  
  
7.  Acepte el nombre del conjunto de copia de seguridad predeterminado sugerido en el cuadro de texto **Nombre** o especifique otro nombre.  
  
8.  Opcionalmente, en el cuadro de texto **Descripción** , escriba una descripción del conjunto de copia de seguridad.  
  
9. Especifique cuándo expirará el conjunto de copia de seguridad:  
  
    -   Para que el conjunto de copia de seguridad expire al cabo de un número de días específico, haga clic en **Después de** (opción predeterminada) y escriba el número de días tras la creación del conjunto en que este expirará. Este valor puede estar entre 0 y 99999 días; el valor 0 significa que el conjunto de copia de seguridad no expirará nunca.  
  
         El valor predeterminado se establece en la opción **Tiempo predeterminado de retención de medios de copia de seguridad (días)** del cuadro de diálogo **Propiedades del servidor** (página **Configuración de base de datos** ). Para acceder a esta opción, en el Explorador de objetos, haga clic con el botón derecho en el nombre del servidor y seleccione Propiedades; después, seleccione la página **Configuración de base de datos** .  
  
    -   Para que el conjunto de copia de seguridad expire en una determinada fecha, haga clic en **El** y escriba la fecha en la que expirará.  
  
10. Elija el tipo de destino de la copia de seguridad haciendo clic en **Disco** o **Cinta**. Para seleccionar la ruta de acceso de hasta 64 unidades de disco o cinta que contengan un solo conjunto de medios, haga clic en **Agregar**. Las rutas seleccionadas se muestran en el cuadro de lista **Copia de seguridad en** .  
  
     Para eliminar un destino de copia de seguridad, selecciónelo y haga clic en **Quitar**. Para ver el contenido de un destino de copia de seguridad, selecciónelo y haga clic en **Contenido**.  
  
11. Para ver o seleccionar las opciones avanzadas, haga clic en **Opciones** , en el panel **Seleccionar una página** .  
  
12. Seleccione una opción de **Sobrescribir medios** ; para ello, haga clic en una de las opciones siguientes:  
  
    -   **Hacer copia de seguridad en el conjunto de medios existente**  
  
         Para esta opción, haga clic en **Anexar al conjunto de copia de seguridad existente** o **Sobrescribir todos los conjuntos de copia de seguridad existentes**. Opcionalmente, seleccione la casilla **Comprobar nombre de conjunto de medios y fecha de expiración de conjunto de copia** y, si lo desea, especifique un nombre en el cuadro de texto **Nombre del conjunto de medios** . Si no especifica ningún nombre, se creará un conjunto de medios con un nombre en blanco. Si especifica un nombre para el conjunto, los medios (cinta o disco) se comprueban para ver si el nombre real coincide con el nombre especificado aquí.  
  
         Si deja el nombre del conjunto de medios en blanco y selecciona la casilla para comprobarlo con los medios, el resultado correcto significará que el nombre del conjunto en los medios también está en blanco.  
  
    -   **Hacer copia de seguridad en un nuevo conjunto de medios y borrar todos los conjuntos de copia de seguridad existentes**  
  
         Para esta opción, especifique un nombre en el cuadro de texto **Nuevo nombre del conjunto de medios** y, si lo desea, describa el conjunto de medios en el cuadro de texto **Nueva descripción del conjunto de medios** .  
  
13. Opcionalmente, en la sección **Confiabilidad** , seleccione:  
  
    -   **Comprobar copia de seguridad al finalizar**.  
  
    -   **Realizar suma de comprobación antes de escribir en los medios** y, si lo desea, **Continuar después de un error de suma de comprobación**. Para obtener más información sobre las sumas de comprobación, vea [Errores posibles de medios durante copia de seguridad y restauración &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md).  
  
14. Si va a realizar copias de seguridad en una unidad de cinta (según se haya especificado en la sección **Destino** de la página **General** ), la opción **Descargar la cinta después de realizar la copia de seguridad** está activa. Al hacer clic en esta opción se activa la opción **Rebobinar la cinta antes de descargar** .  
  
    > [!NOTE]  
    >  Las opciones de la sección **Registro de transacciones** se encuentran inactivas salvo que vaya a realizar una copia de seguridad de un registro de transacciones (según se haya especificado en la sección **Tipo de copia de seguridad** de la página **General** ).  
  
15. [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] y las versiones posteriores admiten la [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md). De forma predeterminada, el hecho de que se comprima una copia de seguridad depende del valor de la opción de configuración del servidor **backup-compression default** . Pero, independientemente del valor predeterminado actual de nivel de servidor, puede comprimir una copia de seguridad si activa **Comprimir copia de seguridad** e impedir la compresión si activa **No comprimir copia de seguridad**.  
  
     **Para ver el valor predeterminado actual de la compresión de copia de seguridad**  
  
    -   [Ver o establecer la opción de configuración del servidor de compresión de copia de seguridad predeterminada](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)  
  
    > [!NOTE]  
    >  Como alternativa para crear copias de seguridad diferenciales de bases de datos, puede utilizar el Asistente para planes de mantenimiento.  
  
##  <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL  
  
#### <a name="create-a-differential-database-backup"></a>Crear una copia de seguridad diferencial de una base de datos  
  
1.  Ejecute la instrucción BACKUP DATABASE para crear la copia de seguridad de base de datos diferencial, especificando:  
  
    -   El nombre de la base de datos de la que se va a realizar una copia de seguridad.  
  
    -   El dispositivo de copia de seguridad en el que se escribe la copia de seguridad de base de datos completa.  
  
    -   La cláusula DIFFERENTIAL, para especificar que solo se realice una copia de seguridad de las partes de la base de datos que han cambiado desde la última copia de seguridad de base de datos completa.  
  
     La sintaxis necesaria es:  
  
     BACKUP DATABASE *nombre_base_de_datos* TO <dispositivo_copia_seguridad> WITH DIFFERENTIAL  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En este ejemplo se crea una copia de seguridad de una base de datos diferencial y una base de datos completa para la base de datos `MyAdvWorks` .  
  
```sql  
-- Create a full database backup first.  
BACKUP DATABASE MyAdvWorks   
   TO MyAdvWorks_1   
   WITH INIT;  
GO  
-- Time elapses.  
-- Create a differential database backup, appending the backup  
-- to the backup device containing the full database backup.  
BACKUP DATABASE MyAdvWorks  
   TO MyAdvWorks_1  
   WITH DIFFERENTIAL;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Copias de seguridad diferenciales &#40;SQL Server&#41;](../../relational-databases/backup-restore/differential-backups-sql-server.md)   
 [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)   
 [Realizar copias de seguridad de archivos y grupos de archivos &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)   
 [Restaurar una copia de seguridad diferencial de la base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)   
 [Restaurar una copia de seguridad del registro de transacciones &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)   
 [Planes de mantenimiento](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Copias de seguridad de archivos completas &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-file-backups-sql-server.md)  
  
  
