---
title: 'Definición de un dispositivo lógico de copia de seguridad: disco'
description: En este artículo se muestra cómo definir un dispositivo lógico de copia de seguridad para un archivo de disco en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup devices [SQL Server], defining
- backup devices [SQL Server], disks
- disk backup devices [SQL Server]
- database backups [SQL Server], disks
- backing up databases [SQL Server], disks
ms.assetid: 86331d43-c738-4523-ae3d-7d6700348ed1
author: cawrites
ms.author: chadam
ms.openlocfilehash: fffcdfb7ad88a2bf3b8c72f9a15b4ff30b796166
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075977"
---
# <a name="define-a-logical-backup-device-for-a-disk-file-sql-server"></a>Definir un dispositivo lógico de copia de seguridad para un archivo de disco (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo definir un dispositivo lógico de copia de seguridad para un archivo de disco en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un dispositivo lógico es un nombre definido por el usuario que señala un dispositivo físico de copia de seguridad específico (un archivo de disco o unidad de cinta).  La inicialización del dispositivo físico tiene lugar posteriormente, cuando se escribe una copia de seguridad en el dispositivo de copia de seguridad.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para definir un dispositivo lógico de copia de seguridad en un archivo de disco, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El nombre de dispositivo lógico debe ser único entre todos los dispositivos lógicos de copia de seguridad de la instancia de servidor. Para ver los nombres de los dispositivos lógicos existentes, consulte la vista de catálogo [sys.backup_devices](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md) .  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Se recomienda que el disco de copia de seguridad no sea el disco de datos o del registro de la base de datos. Esto es necesario para garantizar el acceso a las copias de seguridad si el disco de datos o del registro presenta errores.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Debe pertenecer al rol fijo de servidor **diskadmin** .  
  
 Requiere permiso para escribir en el disco.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-define-a-logical-backup-device-for-a-disk-file"></a>Para definir un dispositivo lógico de copia de seguridad en un archivo de disco  
  
1.  Después de conectarse a la instancia apropiada de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol correspondiente.  
  
2.  Expanda **Objetos de servidor** y, después, haga clic con el botón derecho en **Dispositivos de copia de seguridad**.  
  
3.  Haga clic en **Nuevo dispositivo de copia de seguridad**. Se abrirá el cuadro de diálogo **Dispositivo de copia de seguridad** .  
  
4.  Escriba un nombre para el dispositivo.  
  
5.  Para indicar el destino, haga clic en **Archivo** y especifique la ruta de acceso completa del archivo.  
  
6.  Para definir el nuevo dispositivo, haga clic en **Aceptar**.  
  
 Para realizar una copia de seguridad en este nuevo dispositivo, agréguelo en el campo **Copia de seguridad en:** del cuadro de diálogo **Copia de seguridad de base de datos** (**General**). Para obtener más información, vea [Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-define-a-logical-backup-for-a-disk-file"></a>Para definir un dispositivo lógico de copia de seguridad para un archivo de disco  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_addumpdevice](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md) para definir un dispositivo lógico de copia de seguridad para un archivo de disco. El ejemplo agrega el dispositivo de copia de seguridad de disco denominado `mydiskdump`, con el nombre físico `c:\dump\dump1.bak`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_addumpdevice 'disk', 'mydiskdump', 'c:\dump\dump1.bak' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [sys.backup_devices &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-backup-devices-transact-sql.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [sp_dropdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdevice-transact-sql.md)   
 [Definir un dispositivo lógico de copia de seguridad en una unidad de cinta &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)   
 [Ver las propiedades y el contenido de un dispositivo lógico de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
  
