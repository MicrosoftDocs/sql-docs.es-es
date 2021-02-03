---
title: Modificación de un procedimiento almacenado | Microsoft Docs
description: Obtenga información sobre cómo modificar un procedimiento almacenado en SQL Server 2019 (15.x) mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: conceptual
helpviewer_keywords:
- modifying stored procedures
- editing stored procedures
- stored procedures [SQL Server], modifying
ms.assetid: 13396239-6100-48ce-aa34-461358d99c92
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6f4a87c957c4a7ba83c3b855f77f625cb9dfa593
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250300"
---
# <a name="modify-a-stored-procedure"></a>Modificar un procedimiento almacenado
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
    
##  <a name="this-topic-describes-how-to-modify-a-stored-procedure-in-ssnoversion-by-using-ssmanstudiofull-or-tsql"></a><a name="Top"></a> En este tema se describe cómo modificar un procedimiento almacenado [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   **Antes de empezar:**  [Limitaciones y restricciones](#Restrictions), [Seguridad](#Security)  
  
-   **Para modificar un procedimiento con:**  [SQL Server Management Studio](#SSMSProcedure), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] no se pueden modificar para que sean procedimientos almacenados CLR y viceversa.  
  
 Si anteriormente se creó la definición de procedimiento mediante WITH ENCRYPTION o WITH RECOMPILE, estas opciones solo se habilitan si se incluyen en la instrucción ALTER PROCEDURE.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Es necesario el permiso ALTER PROCEDURE en el procedimiento.  
  
##  <a name="how-to-modify-a-stored-procedure"></a><a name="Procedures"></a> Cómo modificar un procedimiento almacenado  
 Puede usar cualquiera de los siguientes medios:  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para modificar un procedimiento en Management Studio**  
  
1.  En el Explorador de objetos, conéctese a una instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento y, a continuación, expanda **Programación**.  
  
3.  Expanda **Procedimientos almacenados**, haga clic con el botón derecho en el procedimiento que quiere modificar y luego haga clic en **Modificar**.  
  
4.  Modifique el texto del procedimiento almacenado.  
  
5.  Para probar la sintaxis, en el menú **Consulta** , haga clic en **Analizar**.  
  
6.  Para guardar las modificaciones en la definición de procedimiento, en el menú **Consulta** , haga clic en **Ejecutar**.  
  
7.  Para guardar la definición de procedimiento actualizada como un script de [!INCLUDE[tsql](../../includes/tsql-md.md)] , en el menú **Archivo** , haga clic en **Guardar como**. Acepte el nombre de archivo o reemplácelo por un nombre nuevo y, a continuación, haga clic en **Guardar**.  

> [!IMPORTANT]  
>  Valide todos los datos proporcionados por el usuario. No concatene ninguna entrada de usuario antes de validarla. No ejecute nunca un comando creado a partir de una entrada de usuario no validada.  
  
###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para modificar un procedimiento en el Editor de consultas**  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, expanda la base de datos a la que pertenece el procedimiento. O bien, en la barra de herramientas, seleccione la base de datos en la lista de bases de datos disponibles. En este ejemplo, seleccione la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
3.  En el menú **Archivo** , haga clic en **Nueva consulta**.  
  
4.  Copie y pegue el ejemplo siguiente en el editor de consultas. El ejemplo crea el procedimiento `uspVendorAllInfo` , que devuelve los nombres de todos los proveedores en la base de datos [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] , los productos que suministran, su solvencia y su disponibilidad.  
  
    ```  
  
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
  
5.  En el menú **Archivo** , haga clic en **Nueva consulta**.  
  
6.  Copie y pegue el ejemplo siguiente en el editor de consultas. El ejemplo modifica el procedimiento `uspVendorAllInfo` . La cláusula EXECUTE AS CALLER se quita y el cuerpo del procedimiento se modifica para devolver solo los proveedores que proporcionan el producto especificado. Las funciones `LEFT` y `CASE` permiten personalizar la apariencia del conjunto de resultados.  
  
    ```  
    ALTER PROCEDURE Purchasing.uspVendorAllInfo  
        @Product varchar(25)   
    AS  
        SET NOCOUNT ON;  
        SELECT LEFT(v.Name, 25) AS Vendor, LEFT(p.Name, 25) AS 'Product name',   
        'Rating' = CASE v.CreditRating   
            WHEN 1 THEN 'Superior'  
            WHEN 2 THEN 'Excellent'  
            WHEN 3 THEN 'Above average'  
            WHEN 4 THEN 'Average'  
            WHEN 5 THEN 'Below average'  
            ELSE 'No rating'  
            END  
        , Availability = CASE v.ActiveFlag  
            WHEN 1 THEN 'Yes'  
            ELSE 'No'  
            END  
        FROM Purchasing.Vendor AS v   
        INNER JOIN Purchasing.ProductVendor AS pv  
          ON v.BusinessEntityID = pv.BusinessEntityID   
        INNER JOIN Production.Product AS p   
          ON pv.ProductID = p.ProductID   
        WHERE p.Name LIKE @Product  
        ORDER BY v.Name ASC;  
    GO  
  
    ```  
  
7.  Para guardar las modificaciones en la definición de procedimiento, en el menú **Consulta** , haga clic en **Ejecutar**.  
  
8.  Para guardar la definición de procedimiento actualizada como un script de [!INCLUDE[tsql](../../includes/tsql-md.md)] , en el menú **Archivo** , haga clic en **Guardar como**. Acepte el nombre de archivo o reemplácelo por un nombre nuevo y, a continuación, haga clic en **Guardar**.  
  
9. Para ejecutar el procedimiento almacenado modificado, ejecute el siguiente ejemplo.  
  
    ```  
    EXEC Purchasing.uspVendorAllInfo N'LL Crankarm';  
    GO  
  
    ```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)  
  
  
