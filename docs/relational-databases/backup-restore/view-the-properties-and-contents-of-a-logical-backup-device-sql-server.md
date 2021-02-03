---
title: Consulta del contenido de un dispositivo lógico de copia de seguridad
description: Aprenda a ver las propiedades y el contenido de un dispositivo lógico de copia de seguridad en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- displaying backup content
- viewing backup content
- database backups [SQL Server], viewing content
- backing up databases [SQL Server], viewing content
- backing up databases [SQL Server], properties
- displaying backup properties
- backup devices [SQL Server], viewing information
- viewing backup properties
- database backups [SQL Server], properties
ms.assetid: 3a309074-e816-454d-b6c3-fcfdde0cbf74
author: cawrites
ms.author: chadam
ms.openlocfilehash: 059b327db0726f21a7dcc2e7d78d471b88097292
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251203"
---
# <a name="view-the-properties-and-contents-of-a-logical-backup-device-sql-server"></a>Ver las propiedades y el contenido de un dispositivo lógico de copia de seguridad (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo ver las propiedades y el contenido de un dispositivo lógico de copia de seguridad de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para ver las propiedades y el contenido de un dispositivo lógico de copia de seguridad, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
 Para obtener información sobre seguridad, vea [RESTORE LABELONLY &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-labelonly-transact-sql.md).  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 En [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, la obtención de información sobre un conjunto de copia de seguridad o un dispositivo de copia de seguridad requiere el permiso CREATE DATABASE. Para obtener más información, vea [GRANT &#40;permisos de base de datos de Transact-SQL&#41;](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-view-the-properties-and-contents-of-a-logical-backup-device"></a>Para ver las propiedades y el contenido de un dispositivo lógico de copia de seguridad  
  
1.  Después de conectarse a la instancia apropiada de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], en el Explorador de objetos, haga clic en el nombre del servidor para expandir el árbol correspondiente.  
  
2.  Expanda **Objetos de servidor** y expanda **Dispositivos de copia de seguridad**.  
  
3.  Haga clic en el dispositivo y, luego, con el botón derecho, haga clic en **Propiedades** para abrir el cuadro de diálogo **Dispositivo de copia de seguridad** .  
  
4.  En la página **General** se muestra el nombre del dispositivo y el destino, que puede ser un dispositivo de cinta o la ruta de acceso de un archivo.  
  
5.  En el panel **Seleccionar una página** , haga clic en **Contenido de los medios**.  
  
6.  El panel de la derecha se muestra en los siguientes paneles de propiedades:  
  
    -   **Elementos multimedia**  
  
         Información de secuencia del medio (número de secuencia del medio, número de secuencia de la familia e identificador del reflejo, si está presente) y la fecha y hora de creación del medio.  
  
    -   **Conjunto de medios**  
  
         Información del conjunto de medios: nombre del conjunto de medios y su descripción, si está disponible, y el número de familias dentro del conjunto de medios.  
  
7.  La cuadrícula **Conjuntos de copia de seguridad** muestra información sobre el contenido del conjunto de medios.  
  
> [!NOTE]  
>  Para obtener más información, vea [Media Contents Page (Página Contenido de los medios)](../../relational-databases/backup-restore/backup-device-media-contents-page.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-view-the-properties-and-contents-of-a-logical-backup-device"></a>Para ver las propiedades y el contenido de un dispositivo lógico de copia de seguridad  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Use la instrucción [RESTORE LABELONLY](../../t-sql/statements/restore-statements-labelonly-transact-sql.md) . En este ejemplo se devuelve información acerca del dispositivo lógico de copia de seguridad `AdvWrks2008R2Backup` .  
  
```sql  
USE AdventureWorks2012 ;  
RESTORE LABELONLY  
   FROM AdvWrks2008R2Backup ;  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [backupfilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfilegroup-transact-sql.md)   
 [backupfile &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupfile-transact-sql.md)   
 [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)   
 [backupmediaset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediaset-transact-sql.md)   
 [backupmediafamily &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupmediafamily-transact-sql.md)   
 [sp_addumpdevice &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addumpdevice-transact-sql.md)   
 [Dispositivos de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-devices-sql-server.md)  
  
  
