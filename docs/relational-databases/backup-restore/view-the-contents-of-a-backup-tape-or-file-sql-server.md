---
title: Consulta del contenido de copia de seguridad (archivo o cinta)
description: En este artículo se muestra cómo ver el contenido de una cinta o un archivo de copia de seguridad en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- backup devices [SQL Server], tapes
- displaying backup content
- viewing backup content
- tape backup devices, viewing contents
- database backups [SQL Server], viewing content
- backing up databases [SQL Server], viewing content
ms.assetid: cd6674a2-ca55-4b5a-a971-878ba001821e
author: cawrites
ms.author: chadam
ms.openlocfilehash: c50e7b4a46e7abf2f0c99720b622057ed3f29729
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076873"
---
# <a name="view-the-contents-of-a-backup-tape-or-file-sql-server"></a>Consulta del contenido de una cinta o un archivo de copia de seguridad (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo ver el contenido de un archivo o una cinta de copia de seguridad en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  La compatibilidad con dispositivos de cinta de copia de seguridad se quitará en una versión futura de SQL Server. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para ver el contenido de una cinta o un archivo de copia de seguridad, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Para obtener más información sobre seguridad, vea [RESTORE HEADERONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-headeronly-transact-sql.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 En [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, la obtención de información sobre un conjunto de copia de seguridad o un dispositivo de copia de seguridad requiere el permiso CREATE DATABASE. Para obtener más información, vea [GRANT &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-view-the-content-of-a-backup-tape-or-file"></a>Para ver el contenido de una cinta o un archivo de copia de seguridad  
  
1.  Después de conectarse a la instancia apropiada de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol correspondiente.  
  
2.  Expanda **Bases de datos** y, en función de la base de datos, seleccione la base de datos de un usuario o expanda **Bases de datos del sistema** y seleccione una base de datos del sistema.  
  
3.  Haga clic con el botón derecho en la base de datos de la que quiera hacer una copia de seguridad, seleccione **Tareas** y haga clic en **Copia de seguridad**. Aparece el cuadro de diálogo **Copia de seguridad de base de datos** .  
  
4.  En la sección **Destino** de la página **General** , haga clic en **Disco** o **Cinta**. En el cuadro de lista **Copia de seguridad en** , busque el archivo de disco o la cinta que desea.  
  
     Si el archivo de disco o la cinta no se muestra en el cuadro de lista, haga clic en **Agregar**. Seleccione una unidad de cinta o un nombre de archivo. Para agregarlo al cuadro de lista **Copia de seguridad en** , haga clic en **Aceptar**.  
  
5.  En el cuadro de lista **Copia de seguridad en** , seleccione la ruta de acceso del disco o de la unidad de cinta que quiera ver y haga clic en **Contenido**. De este modo se abre el cuadro de diálogo **Contenido del dispositivo** .  
  
6.  En el panel derecho se muestra información sobre el conjunto de medios y los conjuntos de copia de seguridad de la cinta o del archivo seleccionados.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-view-the-content-of-a-backup-tape-or-file"></a>Para ver el contenido de una cinta o un archivo de copia de seguridad  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Use la instrucción [RESTORE HEADERONLY](../../t-sql/statements/restore-statements-headeronly-transact-sql.md) . En este ejemplo se devuelve información sobre el archivo denominado `AdventureWorks2012-FullBackup.bak`.  
  
```sql  
USE AdventureWorks2012;  
RESTORE HEADERONLY   
FROM DISK = N'C:\AdventureWorks2012-FullBackup.bak' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)   
 [Definir un dispositivo lógico de copia de seguridad para un archivo de disco &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)   
 [Definir un dispositivo lógico de copia de seguridad en una unidad de cinta &#40;SQL Server&#41;](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
  
