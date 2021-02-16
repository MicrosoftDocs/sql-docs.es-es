---
title: Cambio de nombre de un procedimiento almacenado | Microsoft Docs
description: Obtenga información sobre cómo cambiar el nombre de un procedimiento almacenado en SQL Server 2019 (15.x) mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], renaming
- renaming stored procedures
ms.assetid: 5d2e4c68-7e0b-4405-8919-f5b203e46770
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 433fa034c85418919aa48886c3a7c06757de890f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347670"
---
# <a name="rename-a-stored-procedure"></a>Cambiar el nombre de un procedimiento almacenado

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

En este tema se describe cómo cambiar el nombre de un procedimiento almacenado [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para cambiar el nombre de un procedimiento almacenado, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Los nombres de los procedimientos se deben ajustar a las reglas para los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
-   Cambiar el nombre de un procedimiento almacenado retiene `object_id` y todos los permisos asignados de forma específica al procedimiento. Al quitar y volver a crear el objeto se crea un nuevo `object_id` y se quita cualquier permiso asignado de forma específica al procedimiento.

-   Al cambiar el nombre de un procedimiento almacenado no se cambia el nombre del objeto correspondiente en la columna de definición de la vista de catálogo **sys.sql_modules**. Para ello, debe quitar el procedimiento almacenado y volver a crearlo con su nuevo nombre.  

-   El hecho de cambiar el nombre o la definición de un procedimiento puede provocar errores en los objetos dependientes si no se actualizan para reflejar los cambios realizados en el procedimiento. Para obtener más información, vea [Ver las dependencias de un procedimiento almacenado](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 CREATE PROCEDURE  
 Necesita el permiso CREATE PROCEDURE en la base de datos y el permiso ALTER en el esquema en el que se va a crear el procedimiento o la pertenencia al rol fijo de base de datos **db_ddladmin**.  
  
 ALTER PROCEDURE  
 Necesita el permiso ALTER en el procedimiento o la pertenencia al rol fijo de base de datos **db_ddladmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-rename-a-stored-procedure"></a>Para cambiar el nombre de un procedimiento almacenado  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento y, a continuación, expanda **Programación**.  
3.  [Cómo ver las dependencias de un procedimiento almacenado (SQL Server Management Studio)](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md).  
4.  Expanda **Procedimientos almacenados**, haga clic con el botón derecho en el procedimiento cuyo nombre quiere cambiar y, después, haga clic en **Cambiar nombre**.  
5.  Modifique el nombre del procedimiento.  
6.  Modifique el nombre del procedimiento al que se hace referencia en cualquier objeto dependiente o script.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-rename-a-stored-procedure"></a>Para cambiar el nombre de un procedimiento almacenado  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo cambiar el nombre de un procedimiento quitando el procedimiento y volviendo a crearlo con otro nombre. En el primer ejemplo se crea el procedimiento almacenado `'HumanResources.uspGetAllEmployeesTest`. En el segundo ejemplo se cambia el nombre del procedimiento almacenado a `HumanResources.uspEveryEmployeeTest`.  
  
```sql  
--Create the stored procedure.  
USE AdventureWorks2012;  
GO  

CREATE PROCEDURE HumanResources.uspGetAllEmployeesTest  
AS  
    SET NOCOUNT ON;  
    SELECT LastName, FirstName, Department  
    FROM HumanResources.vEmployeeDepartmentHistory;  
GO  
  
--Rename the stored procedure.  
EXEC sp_rename 'HumanResources.uspGetAllEmployeesTest', 'uspEveryEmployeeTest'; 
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [Crear un procedimiento almacenado](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modificar un procedimiento almacenado](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Eliminar un procedimiento almacenado](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [Ver la definición de un procedimiento almacenado](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)   
 [Ver las dependencias de un procedimiento almacenado](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)  
  
  
