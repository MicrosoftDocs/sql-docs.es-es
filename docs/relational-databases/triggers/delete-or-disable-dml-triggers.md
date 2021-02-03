---
description: Eliminar o deshabilitar desencadenadores DML
title: Eliminación o deshabilitación de desencadenadores DML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- DML triggers, disabling
- removing DML triggers
- disabling DML triggers
- dropping DML triggers
- deleting DML triggers
- DML triggers, removing
ms.assetid: 0f97f953-33c5-4b26-afeb-db2a26ce38b4
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6fdcb062fc8a7865b8dd43e11dcff0b967270b9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192850"
---
# <a name="delete-or-disable-dml-triggers"></a>Eliminar o deshabilitar desencadenadores DML
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo eliminar o deshabilitar un desencadenador DML en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para eliminar o deshabilitar un desencadenador DML, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Cuando el desencadenador se elimina, se quita de la base de datos actual. Ni la tabla ni los datos en los que se basa se ven afectados. Si se elimina una tabla, se eliminarán automáticamente todos los desencadenadores que contenga.  
  
-   Un desencadenador se habilita de forma predeterminada cuando se crea.  
  
-   Al deshabilitar un desencadenador no se quita. Sigue siendo un objeto de la base de datos actual. Sin embargo, el desencadenador no se activará cuando se ejecute la instrucción INSERT, UPDATE o DELETE en la que se programó. Los desencadenadores deshabilitados se pueden volver a habilitar. Si se habilita un desencadenador, éste no se vuelve a crear. El desencadenador se activa de la misma forma que cuando se creó originalmente.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para eliminar un desencadenador DML se necesita el permiso ALTER en la tabla o vista en la que está definido el desencadenador.  
  
 Para deshabilitar o habilitar un desencadenador DML, el usuario debe contar como mínimo con el permiso ALTER en la tabla o vista en la que se creó el desencadenador.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-delete-a-dml-trigger"></a>Para eliminar un desencadenador DML  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda la base de datos que desee, expanda **Tablas** y, a continuación, expanda la tabla que contiene el desencadenador que desea eliminar.  
  
3.  Expanda **Desencadenadores**, haga clic con el botón derecho en el desencadenador que quiera eliminar y luego haga clic en **Eliminar**.  
  
4.  En el cuadro de diálogo **Eliminar objeto** , compruebe que se ha especificado el desencadenador deseado y haga clic en **Aceptar**.  
  
#### <a name="to-disable-and-enable-a-dml-trigger"></a>Para deshabilitar y habilitar un desencadenador DML  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda la base de datos que desee, expanda **Tablas** y, a continuación, expanda la tabla que contiene el desencadenador que desea deshabilitar.  
  
3.  Expanda **Desencadenadores**, haga clic con el botón derecho en el desencadenador que quiera deshabilitar y luego haga clic en **Deshabilitar**.  
  
4.  Para habilitar el desencadenador, haga clic en **Habilitar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-delete-a-dml-trigger"></a>Para eliminar un desencadenador DML  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue los ejemplos siguientes en la ventana de consultas. Ejecute la instrucción [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md) para crear el desencadenador `Sales.bonus_reminder` . Para eliminar el desencadenador, ejecute la instrucción [DROP TRIGGER](../../t-sql/statements/drop-trigger-transact-sql.md) .  
  
```sql  
--Create the trigger.  
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
--Delete the trigger.  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('Sales.bonus_reminder', 'TR') IS NOT NULL  
   DROP TRIGGER Sales.bonus_reminder;  
GO  
  
```  
  
#### <a name="to-disable-and-enable-a-dml-trigger"></a>Para deshabilitar y habilitar un desencadenador DML  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue los ejemplos siguientes en la ventana de consultas. Ejecute la instrucción [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md) para crear el desencadenador `Sales.bonus_reminder` . Para deshabilitar y habilitar el desencadenador, ejecute las instrucciones [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md) y [HABILITE TRIGGER](../../t-sql/statements/enable-trigger-transact-sql.md) , respectivamente.  
  
```sql  
--Create the trigger.  
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
--Disable the trigger.  
USE AdventureWorks2012;  
GO  
DISABLE TRIGGER Sales.bonus_reminder ON Sales.SalesPersonQuotaHistory;  
GO  
  
```  
  
```sql  
--Enable the trigger.  
USE AdventureWorks2012;  
GO  
ENABLE TRIGGER Sales.bonus_reminder ON Sales.SalesPersonQuotaHistory;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
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
  
  
