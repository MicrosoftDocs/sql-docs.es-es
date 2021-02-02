---
title: Eliminación de copia de seguridad (SQL Server) | Microsoft Docs
description: En este artículo se muestra cómo eliminar un dispositivo de copia de seguridad en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- database backups [SQL Server], deleting devices
- backup devices [SQL Server], deleting
- deleting backup devices
- removing backup devices
- backing up databases [SQL Server], backup devices
ms.assetid: 7be62480-ed6a-4262-a071-1feba73b1c02
author: cawrites
ms.author: chadam
ms.openlocfilehash: ea8bb759da76fef8336efd8493aee1bd8d692452
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076287"
---
# <a name="delete-a-backup-device-sql-server"></a>Eliminar un dispositivo de copia de seguridad (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo eliminar un dispositivo de copia de seguridad en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para eliminar un dispositivo de copia de seguridad, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Debe pertenecer al rol fijo de servidor **diskadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-delete-a-backup-device"></a>Para eliminar un dispositivo de copia de seguridad  
  
1.  Después de conectarse a la instancia apropiada de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol de servidores.  
  
2.  Expanda **Objetos de servidor** y, a continuación, **Dispositivos de copia de seguridad**.  
  
3.  Haga clic con el botón derecho en el dispositivo que quiera y, después, haga clic en **Eliminar**.  
  
4.  En el cuadro de diálogo **Eliminar objeto** , compruebe que aparezca el nombre del dispositivo correcto en la columna **Nombre de objeto** .  
  
5.  Haga clic en **OK**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-delete-a-backup-device"></a>Para eliminar un dispositivo de copia de seguridad  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el ejemplo siguiente en la consulta. En este ejemplo se muestra cómo usar [sp_dropdevice](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md) para eliminar un dispositivo de copia de seguridad. Ejecute el primer ejemplo para crear el dispositivo de copia de seguridad `mybackupdisk` y el nombre físico `c:\backup\backup1.bak`. Ejecute **sp_dropdevice** para eliminar el dispositivo de copia de seguridad `mybackupdisk` . El parámetro `delfile` elimina el nombre físico.  
  
```sql  
--Define a backup device and physical name.   
USE AdventureWorks2012 ;  
GO  
EXEC sp_addumpdevice 'disk', 'mybackupdisk', 'c:\backup\backup1.bak' ;  
GO  
--Delete the backup device and the physical name.  
USE AdventureWorks2012 ;  
GO  
EXEC sp_dropdevice ' mybackupdisk ', 'delfile' ;  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Ver las propiedades y el contenido de un dispositivo lógico de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)   
 [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)  
  
  
