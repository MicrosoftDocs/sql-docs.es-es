---
title: Conceder permisos para un procedimiento almacenado | Microsoft Docs
description: Obtenga información sobre cómo conceder permiso en un procedimiento almacenado en SQL Server 2019 (15.x) mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], permissions
ms.assetid: a7d15816-a788-4099-ad91-dc4b26618299
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b41f60b80dcb13d9e61b854666e3298b2b295cf
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250313"
---
# <a name="grant-permissions-on-a-stored-procedure"></a>Conceder permisos para un procedimiento almacenado
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  En este tema se describe cómo conceder permisos para un procedimiento almacenado en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Se pueden conceder permisos a un usuario, un rol de base de datos o un rol de aplicación existentes en la base de datos.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para conceder permisos para un procedimiento almacenado, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   No se puede usar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para conceder permisos para procedimientos del sistema o funciones del sistema. En su lugar, use [GRANT (permisos de objeto de Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md) .  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 El otorgante del permiso (o la entidad de seguridad especificada con la opción AS) debe tener el permiso con GRANT OPTION, o un permiso superior que implique el permiso que se va a conceder. Requiere el permiso ALTER en el esquema al que pertenece el procedimiento o el permiso CONTROL en el procedimiento. Para obtener más información, vea [Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-grant-permissions-on-a-stored-procedure"></a>Para conceder permisos para un procedimiento almacenado  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento y, a continuación, expanda **Programación**.  
  
3.  Expanda **Procedimientos almacenados**, haga clic con el botón derecho en el procedimiento en el que quiera conceder permisos y, luego, haga clic en **Propiedades**.  
  
4.  En **Propiedades del procedimiento almacenado**, seleccione la página **Permisos** .  
  
5.  Para conceder permisos a un usuario, un rol de base de datos o un rol de aplicación, haga clic en **Buscar**.  
  
6.  En **Seleccionar usuarios o roles**, haga clic en **Tipos de objeto** para agregar o borrar los usuarios y los roles que desee.  
  
7.  Haga clic en **Examinar** para mostrar la lista de usuarios o de roles. Seleccione los usuarios o los roles a los que deben concederse los permisos.  
  
8.  En la cuadrícula **Permisos explícitos** , seleccione los permisos que desea conceder al rol o al usuario especificados. Para obtener una descripción de los permisos, vea [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md).  

 Al seleccionar **Conceder** , se indica que se concederá el permiso especificado al receptor. Al seleccionar **Grant With** , se indica que el receptor también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-grant-permissions-on-a-stored-procedure"></a>Para conceder permisos para un procedimiento almacenado  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se concede el permiso `EXECUTE` para el procedimiento almacenado `HumanResources.uspUpdateEmployeeHireInfo` a un rol de aplicación denominado `Recruiting11`.  
  
```sql  
USE AdventureWorks2012;   
GRANT EXECUTE ON OBJECT::HumanResources.uspUpdateEmployeeHireInfo  
    TO Recruiting11;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [Permisos de objeto GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-object-permissions-transact-sql.md)   
 [Crear un procedimiento almacenado](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [Modificar un procedimiento almacenado](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [Eliminar un procedimiento almacenado](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [Cambiar el nombre de un procedimiento almacenado](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)  
  
  
