---
title: Eliminación de archivos de datos o de registro de una base de datos | Microsoft Docs
description: Obtenga información sobre cómo eliminar archivos de registro o datos en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], files
- deleting files
- removing files
- removing data
- data deletions [SQL Server]
- file deletion [SQL Server]
- deleting data
ms.assetid: 0db4018c-ce2c-4ba1-bb29-1e4f3791c925
author: stevestein
ms.author: sstein
ms.openlocfilehash: 062ac3ee839f62a2e789be180484d3d3e8a18d44
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812960"
---
# <a name="delete-data-or-log-files-from-a-database"></a>Eliminar archivos de datos o de registro de una base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo eliminar archivos de datos o de registro en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Requisitos previos](#Prerequisites)  
  
     [Seguridad](#Security)  
  
-   **Para eliminar archivos de datos o de registro de una base de datos, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Para poder eliminar un archivo debe estar vacío. Para obtener más información, vea [Reducir un archivo](../../relational-databases/databases/shrink-a-file.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la base de datos.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-delete-data-or-log-files-from-a-database"></a>Para eliminar archivos de datos o de registro de una base de datos  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, haga clic con el botón derecho en la base de datos de la que quiera eliminar el archivo y, después, haga clic en **Propiedades**.  
  
3.  Seleccione la página **Archivos** .  
  
4.  En la cuadrícula **Archivos de base de datos** , seleccione el archivo que desee eliminar y haga clic en **Quitar**.  
  
5.  Haga clic en **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-delete-data-or-log-files-from-a-database"></a>Para eliminar archivos de datos o de registro de una base de datos  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. Este ejemplo quita el archivo `test1dat4`.  
  
 [!code-sql[DatabaseDDL#AlterDatabase4](../../relational-databases/databases/codesnippet/tsql/delete-data-or-log-files_1.sql)]  
  
 Para obtener más ejemplos, vea [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
## <a name="see-also"></a>Consulte también  
 [Reducir una base de datos](../../relational-databases/databases/shrink-a-database.md)   
 [Agregar archivos de datos o de registro a una base de datos](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)  
  
  
