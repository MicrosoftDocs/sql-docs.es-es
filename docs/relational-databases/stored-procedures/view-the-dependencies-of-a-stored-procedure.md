---
title: Ver las dependencias de un procedimiento almacenado | Microsoft Docs
description: Obtenga información sobre cómo ver las dependencias de un procedimiento almacenado en SQL Server 2019 (15.x) mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], dependencies
- displaying stored procedure dependencies
- viewing stored procedure dependencies
ms.assetid: 6ae0a369-1bc7-4ae4-be89-2b483697cd1f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 91886e00f10f3aa4a84b054145e8078310c37024
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250164"
---
# <a name="view-the-dependencies-of-a-stored-procedure"></a>Ver las dependencias de un procedimiento almacenado
[!INCLUDE[appliesto-ss-asdb-xxxx-pdw-md](../../includes/appliesto-ss-asdb-xxxx-pdw-md.md)]
  En este tema se describe cómo ver las dependencias de procedimiento almacenado en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
##  <a name="Top"></a>   
-   **Antes de empezar:**  [Limitaciones y restricciones](#Restrictions), [Seguridad](#Security)  
  
-   **Para ver las dependencias de un procedimiento con:**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Función del sistema: **sys.dm_sql_referencing_entities**  
 Requiere el permiso CONTROL en la entidad a la que se hace referencia y el permiso SELECT en sys.dm_sql_referencing_entities. Cuando la entidad a la que se hace referencia es una función de partición, se requiere el permiso CONTROL en la base de datos. De forma predeterminada, se concede el permiso SELECT a public.  
  
 Función del sistema: **sys.dm_sql_referenced_entities**  
 Requiere el permiso SELECT en sys.dm_sql_referenced_entities y el permiso VIEW DEFINITION en la entidad de referencia. De forma predeterminada, se concede el permiso SELECT a public. Requiere el permiso VIEW DEFINITION en la base de datos o el permiso ALTER DATABASE DDL TRIGGER en la base de datos si la entidad de referencia es un desencadenador DDL de base de datos. Requiere el permiso VIEW ANY DEFINITION en el servidor si la entidad de referencia es un desencadenador DDL de servidor.  
  
 Vista de catálogo de objetos: **sys.sql_expression_dependencies**  
 Necesita el permiso VIEW DEFINITION en la base de datos y el permiso SELECT en sys.sql_expression_dependencies para la base de datos. De forma predeterminada, solo se permite el permiso SELECT a los miembros del rol fijo de base de datos db_owner. Si se conceden los permisos SELECT y VIEW DEFINITION a otro usuario, el receptor puede ver todas las dependencias de la base de datos.  
  
##  <a name="how-to-view-the-dependencies-of-a-stored-procedure"></a><a name="Procedures"></a> Cómo ver las dependencias de un procedimiento almacenado  
 Puede usar cualquiera de los siguientes medios:  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para ver las dependencias de un procedimiento en el Explorador de objetos**  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento y, a continuación, expanda **Programación**.  
  
3.  Expanda **Procedimientos almacenados**, haga clic con el botón derecho en el procedimiento y, luego, haga clic en **Ver dependencias**.  
  
4.  Examine la lista de objetos que dependen del procedimiento.  
  
5.  Examine la lista de objetos de los cuales depende el procedimiento.  
  
6.  Haga clic en **OK**.  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para ver las dependencias de un procedimiento en el Editor de consultas**  
  
 Función del sistema: **sys.dm_sql_referencing_entities**  
 Esta función se usa para mostrar los objetos que dependen de un procedimiento.  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento.  
  
3.  En el menú **Archivo** , haga clic en **Nueva consulta** .  
  
4.  Copie y pegue los ejemplos siguientes en el editor de consultas. El primer ejemplo crea el procedimiento `uspVendorAllInfo` , que devuelve los nombres de todos los proveedores en la base de datos [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , los productos que suministran, su solvencia y su disponibilidad.  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
    ```  
  
5.  Después de crear el procedimiento, el segundo ejemplo usa la función sys.dm_sql_referencing_entities para mostrar los objetos que dependen del procedimiento.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT referencing_schema_name, referencing_entity_name, referencing_id, referencing_class_desc, is_caller_dependent  
    FROM sys.dm_sql_referencing_entities ('Purchasing.uspVendorAllInfo', 'OBJECT');   
    GO  
  
    ```  
  
 Función del sistema: **sys.dm_sql_referenced_entities**  
 Esta función se usa para mostrar los objetos de los que depende un procedimiento.  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento.  
  
3.  En el menú **Archivo** , haga clic en **Nueva consulta** .  
  
4.  Copie y pegue los ejemplos siguientes en el editor de consultas. El primer ejemplo crea el procedimiento `uspVendorAllInfo` , que devuelve los nombres de todos los proveedores en la base de datos [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , los productos que suministran, su solvencia y su disponibilidad.  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
    ```  
  
5.  Después de crear el procedimiento, el segundo ejemplo usa la función sys.dm_sql_referenced_entities para mostrar los objetos de los que depende el procedimiento.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT referenced_schema_name, referenced_entity_name,  
    referenced_minor_name,referenced_minor_id, referenced_class_desc,  
    is_caller_dependent, is_ambiguous  
    FROM sys.dm_sql_referenced_entities ('Purchasing.uspVendorAllInfo', 'OBJECT');  
    GO  
    ```  
  
 Vista de catálogo de objetos: **sys.sql_expression_dependencies**  
 Esta vista se puede usar para mostrar los objetos de los que depende un procedimiento o que dependen de un procedimiento.  
  
 Mostrar los objetos que dependen de un procedimiento.  
 1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento.  
  
3.  En el menú **Archivo** , haga clic en **Nueva consulta** .  
  
4.  Copie y pegue los ejemplos siguientes en el editor de consultas. El primer ejemplo crea el procedimiento `uspVendorAllInfo` , que devuelve los nombres de todos los proveedores en la base de datos [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , los productos que suministran, su solvencia y su disponibilidad.  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
    ```  
  
5.  Después de crear el procedimiento, el segundo ejemplo usa la vista sys.sql_expression_dependencies para mostrar los objetos que dependen del procedimiento.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_SCHEMA_NAME ( referencing_id ) AS referencing_schema_name,  
        OBJECT_NAME(referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        referencing_class_desc, referenced_class_desc,  
        referenced_server_name, referenced_database_name, referenced_schema_name,  
        referenced_entity_name,   
        COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
        is_caller_dependent, is_ambiguous  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    WHERE referenced_id = OBJECT_ID(N'Purchasing.uspVendorAllInfo')  
    GO  
    ```  
  
 Mostrar los objetos de los que depende un procedimiento.  
 1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento.  
  
3.  En el menú **Archivo** , haga clic en **Nueva consulta** .  
  
4.  Copie y pegue los ejemplos siguientes en el editor de consultas. El primer ejemplo crea el procedimiento `uspVendorAllInfo` , que devuelve los nombres de todos los proveedores en la base de datos [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , los productos que suministran, su solvencia y su disponibilidad.  
  
    ```  
    USE AdventureWorks2008R2;  
    GO  
    IF OBJECT_ID ( 'Purchasing.uspVendorAllInfo', 'P' ) IS NOT NULL   
        DROP PROCEDURE Purchasing.uspVendorAllInfo;  
    GO  
    CREATE PROCEDURE Purchasing.uspVendorAllInfo  
    WITH EXECUTE AS CALLER  
    AS  
        SET NOCOUNT ON;  
        SELECT v.Name AS Vendor, p.Name AS 'Product name',   
          v.CreditRating AS 'Rating',   
          v.ActiveFlag AS Availability  
        FROM Purchasing.Vendor v   
        INNER JOIN Purchasing.ProductVendor pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product p  
          ON pv.ProductID = p.ProductID   
        ORDER BY v.Name ASC;  
    GO  
    ```  
  
5.  Después de crear el procedimiento, el segundo ejemplo usa la vista sys.sql_expression_dependencies para mostrar los objetos de los que depende el procedimiento.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        referencing_class_desc, referenced_class_desc,  
        referenced_server_name, referenced_database_name, referenced_schema_name,  
        referenced_entity_name,   
        COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
        is_caller_dependent, is_ambiguous  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    WHERE referencing_id = OBJECT_ID(N'Purchasing.uspVendorAllInfo');  
    GO  
    ```  
  
## <a name="see-also"></a>Consulte también  
 [Cambiar el nombre de un procedimiento almacenado](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)   
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  
