---
title: Cambio de los valores de configuración de una base de datos | Microsoft Docs
description: Obtenga información sobre cómo cambiar las opciones de nivel de base de datos en SQL Server 2019 mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- database configuration [SQL Server]
- configuration options [SQL Server], databases
- modifying database configuration settings
ms.assetid: c29c3385-5043-400f-bb4e-044a4c9a9a4b
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d3e1e312a539ccd3526e252facd523980d751e4b
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813604"
---
# <a name="change-the-configuration-settings-for-a-database"></a>Cambiar los valores de configuración de una base de datos
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo cambiar las opciones de nivel de base de datos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Las opciones son únicas para cada base de datos y no afectan a otras bases de datos.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para cambiar la configuración de las opciones de una base de datos, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Solo el administrador del sistema o los propietarios de la base de datos, los miembros de los roles fijos de servidor **sysadmin** y **dbcreator** y de los roles fijos de base de datos **db_owner** pueden modificar estas opciones.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la base de datos.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-change-the-option-settings-for-a-database"></a>Para cambiar la configuración de las opciones de una base de datos  
  
1.  En el Explorador de objetos, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , expanda el servidor, expanda **Bases de datos**, haga clic con el botón derecho en una base de datos y, después, haga clic en **Propiedades**.  
  
2.  En el cuadro de diálogo **Propiedades de la base de datos** , haga clic en **Opciones** para obtener acceso a la mayoría de los valores de configuración. Las configuraciones de archivo y grupo de archivos, la creación de reflejos y el trasvase de registros se encuentran en sus páginas correspondientes.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-change-the-option-settings-for-a-database"></a>Para cambiar la configuración de las opciones de una base de datos  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se establece el modelo de recuperación y las opciones de comprobación de páginas de datos para la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
 [!code-sql[DatabaseDDL#AlterDatabase7](../../relational-databases/databases/codesnippet/tsql/change-the-configuration_1.sql)]  
  
 Para ver más ejemplos, vea [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
## <a name="see-also"></a>Consulte también  
 [Nivel de compatibilidad de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)   
 [Reflejo de la base de datos ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [ALTER DATABASE SET HADR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)   
 [Cambiar el nombre de una base de datos](../../relational-databases/databases/rename-a-database.md)   
 [Reducir una base de datos](../../relational-databases/databases/shrink-a-database.md)  
  
  
