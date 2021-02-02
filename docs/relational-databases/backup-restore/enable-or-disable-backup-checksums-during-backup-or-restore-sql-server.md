---
title: Habilitación o deshabilitación de sumas de comprobación de copia de seguridad
description: En este artículo se muestra cómo habilitar o deshabilitar las sumas de comprobación de copia de seguridad de una base de datos en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup checksums [SQL Server]
- disabling checksums
- checksums [SQL Server]
ms.assetid: 6786bd1e-ad97-430a-8dfb-d4ba952d6c4d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 47716f50091f695a43d751c57e9dbfbbe6b7001e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075917"
---
# <a name="enable-or-disable-backup-checksums-during-backup-or-restore-sql-server"></a>Habilitación o deshabilitación de sumas de comprobación de copia de seguridad durante copia de seguridad o restauración (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo habilitar o deshabilitar sumas de comprobación de copia de seguridad cuando se realiza una copia de seguridad o se restaura una base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para habilitar o deshabilitar sumas de comprobación de copias de seguridad, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 BACKUP  
 De forma predeterminada, los permisos BACKUP DATABASE y BACKUP LOG corresponden a los miembros del rol fijo de servidor **sysadmin** y de los roles fijos de base de datos **db_owner** y **db_backupoperator** .  
  
 Los problemas de propiedad y permisos del archivo físico del dispositivo de copia de seguridad pueden interferir con una operación de copia de seguridad. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe poder leer y escribir en el dispositivo y la cuenta en la que se ejecuta el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener permisos de escritura. En cambio, [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md), que agrega una entrada para un dispositivo de copia de seguridad en las tablas del sistema, no comprueba los permisos de acceso a los archivos. Es posible que estos problemas con el archivo físico del dispositivo de copia de seguridad no aparezcan hasta que se tenga acceso al recurso físico, al intentar la copia de seguridad o la restauración.  
  
 RESTORE  
 Si la base de datos que se va a restaurar no existe, el usuario debe tener permisos CREATE DATABASE para poder ejecutar RESTORE. Si la base de datos existe, los permisos RESTORE corresponden de forma predeterminada a los miembros de los roles fijos de servidor **sysadmin** y **dbcreator** , y al propietario (**dbo**) de la base de datos (para la opción FROM DATABASE_SNAPSHOT, la base de datos siempre existe).  
  
 Los permisos RESTORE se conceden a los roles en los que la información acerca de la pertenencia está siempre disponible para el servidor. Debido a que la pertenencia a un rol fijo de base de datos solo se puede comprobar cuando la base de datos es accesible y no está dañada, lo que no siempre ocurre cuando se ejecuta RESTORE, los miembros del rol fijo de base de datos **db_owner** no tienen permisos RESTORE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-enable-or-disable-checksums-during-a-backup-operation"></a>Para habilitar o deshabilitar sumas de comprobación durante una operación de copia de seguridad  
  
1.  Siga los pasos para [crear una copia de seguridad de la base de datos](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
2.  En la página **Opciones** , en la sección de **Confiabilidad** , haga clic en **Realizar suma de comprobación antes de escribir en los medios**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-enable-or-disable-backup-checksum-for-a-backup-operation"></a>Para habilitar o deshabilitar una suma de comprobación de copia de seguridad para una operación de copia de seguridad  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Para habilitar las sumas de comprobación de copia de seguridad en una instrucción [BACKUP](../../t-sql/statements/backup-transact-sql.md) , especifique la opción WITH CHECKSUM. Para deshabilitar sumas de comprobación de copia de seguridad, especifique la opción WITH NO_CHECKSUM. Es el comportamiento predeterminado, salvo para una copia de seguridad comprimida. En el siguiente ejemplo se especifica que se realicen sumas de comprobación.  
  
```sql  
BACKUP DATABASE AdventureWorks2012   
 TO DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'  
   WITH CHECKSUM;  
GO  
```  
  
#### <a name="to-enable-or-disable-backup-checksum-for-a-restore-operation"></a>Para habilitar o deshabilitar una suma de comprobación de copia de seguridad para una operación de restauración  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Para habilitar las sumas de comprobación de copia de seguridad en una instrucción [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) , especifique la opción WITH CHECKSUM. Es el comportamiento predeterminado para una copia de seguridad comprimida. Para deshabilitar sumas de comprobación de copia de seguridad, especifique la opción WITH NO_CHECKSUM. Es el comportamiento predeterminado, salvo para una copia de seguridad comprimida. En el siguiente ejemplo se especifica que se realicen sumas de comprobación de copia de seguridad.  
  
```sql  
RESTORE DATABASE AdventureWorks2012   
 FROM DISK = 'Z:\SQLServerBackups\AdvWorksData.bak'  
   WITH CHECKSUM;  
GO  
```  
  
> [!WARNING]  
>  Si solicita de forma explícita a CHECKSUM para una operación de restauración y la copia de seguridad contiene sumas de comprobación de copia de seguridad, se comprueban estas sumas y las sumas de comprobación de página, como en el caso predeterminado. No obstante, si el conjunto de copia de seguridad no tiene sumas de comprobación de copia de seguridad, la operación de restauración da error con un mensaje que indica que no hay sumas de comprobación.  
  
## <a name="see-also"></a>Consulte también  
 [RESTORE FILELISTONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-filelistonly-transact-sql.md)   
 [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md)   
 [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md)   
 [RESTORE VERIFYONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-verifyonly-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [Argumentos RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-arguments-transact-sql.md)   
 [Errores posibles de medios durante copia de seguridad y restauración &#40;SQL Server&#41;](../../relational-databases/backup-restore/possible-media-errors-during-backup-and-restore-sql-server.md)   
 [Especificar si una operación de copia de seguridad o restauración continúa o se detiene después de encontrar un error &#40;SQL Server&#41;](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
  
