---
description: Modificar o cambiar el nombre de desencadenadores DML
title: Modificación o cambio de nombre de desencadenadores DML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- renaming triggers
- modifying triggers
- DML triggers, modifying
ms.assetid: c7317eec-c0e9-479e-a4a7-83b6b6c58d59
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 34947ff934ead8d308c6da1df2f1d0a6bd1b07c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187288"
---
# <a name="modify-or-rename-dml-triggers"></a>Modificar o cambiar el nombre de desencadenadores DML
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo modificar o cambiar el nombre de un desencadenador DML en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para modificar o cambiar el nombre de un desencadenador DML, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Al cambiar el nombre de un desencadenador, el desencadenador debe estar en la base de datos actual y el nuevo nombre debe seguir las reglas para los [identificadores](../../relational-databases/databases/database-identifiers.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Se recomienda no usar el procedimiento almacenado [sp_rename](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md) para cambiar el nombre de un desencadenador. Al cambiar cualquier parte del nombre de un objeto se pueden interrumpir scripts y procedimientos almacenados. Al cambiar el nombre de un desencadenador no se cambia el nombre del objeto correspondiente en la columna de definición de la vista de catálogo [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) . En su lugar, se recomienda quitar y volver a crear el desencadenador.  
  
-   Si cambia el nombre de un objeto al que hace referencia un desencadenador DML, deberá modificar este último para que el texto refleje el nuevo nombre. Por tanto, antes de cambiar el nombre de un objeto, vea primero las dependencias del mismo para determinar si algún desencadenador va a verse afectado por el cambio propuesto.  
  
-   También es posible modificar un desencadenador DML para cifrar su definición.  
  
-   Para ver las dependencias de un desencadenador, puede usar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o la función y las vistas de catálogo siguientes:  
  
    -   [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
    -   [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)  
  
    -   [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para modificar un desencadenador DML se requiere el permiso ALTER en la tabla o vista en la que se define el desencadenador.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-modify-a-dml-trigger"></a>Para modificar un desencadenador DML  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda la base de datos que desee, expanda **Tablas** y, a continuación, expanda la tabla que contiene el desencadenador que desea modificar.  
  
3.  Expanda **Desencadenadores**, haga clic con el botón derecho en el desencadenador que quiera cambiar y, luego, haga clic en **Modificar**.  
  
4.  Modifique el desencadenador y, a continuación, haga clic en **Ejecutar**.  
  
#### <a name="to-rename-a-dml-trigger"></a>Para cambiar el nombre de un desencadenador DML  
  
1.  [Elimine el desencadenador](../../relational-databases/triggers/delete-or-disable-dml-triggers.md) cuyo nombre desea cambiar.  
  
2.  [Vuelva a crear el desencadenador](../../relational-databases/triggers/create-dml-triggers.md), especificando el nuevo nombre.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-modify-a-trigger-using-alter-trigger"></a>Para modificar un desencadenador mediante ALTER TRIGGER  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue los ejemplos siguientes en la consulta. Ejecute el primer ejemplo para crear un desencadenador DML que muestre al cliente un mensaje definido por el usuario cuando un usuario intente agregar o cambiar los datos de la tabla `SalesPersonQuotaHistory` . Ejecute la instrucción [ALTER TRIGGER](../../t-sql/statements/alter-trigger-transact-sql.md) para modificar el desencadenador de manera que solo se active en las actividades `INSERT` . Este desencadenador es útil porque recuerda al usuario que actualiza o inserta filas en esta tabla que debe notificar también al departamento `Compensation` .  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID(N'Sales.bonus_reminder', N'TR') IS NOT NULL  
    DROP TRIGGER Sales.bonus_reminder;  
GO  
CREATE TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
WITH ENCRYPTION  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  
  
```  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER TRIGGER Sales.bonus_reminder  
ON Sales.SalesPersonQuotaHistory  
AFTER INSERT  
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  
  
```  
  
#### <a name="to-rename-a-trigger-using-drop-trigger-and-alter-trigger"></a>Para cambiar el nombre de un desencadenador mediante DROP TRIGGER y ALTER TRIGGER  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se usan las instrucciones [DROP TRIGGER](../../t-sql/statements/drop-trigger-transact-sql.md) y [ALTER TRIGGER](../../t-sql/statements/alter-trigger-transact-sql.md) para cambiar el nombre del desencadenador `Sales.bonus_reminder` a `Sales.bonus_reminder_2`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID(N'Sales.bonus_reminder', N'TR') IS NOT NULL  
    DROP TRIGGER Sales.bonus_reminder;  
GO  
CREATE TRIGGER Sales.bonus_reminder_2  
ON Sales.SalesPersonQuotaHistory  
WITH ENCRYPTION  
AFTER INSERT, UPDATE   
AS RAISERROR ('Notify Compensation', 16, 10);  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [Obtener información acerca de los desencadenadores DML](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)  
  
  
