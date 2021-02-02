---
title: Habilitar o deshabilitar una guía de plan | Microsoft Docs
description: Obtenga información sobre cómo deshabilitar y habilitar las guías de plan mediante SQL Server Management Studio o Transact-SQL. Deshabilite o habilite una o todas las guías de plan en una base de datos.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- plan guides [SQL Server], disabling
- enabling plan guides
- plan guides [SQL Server], enabling
- disabling plan guides
ms.assetid: b00ab550-5308-4cb8-8330-483cd1d25654
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e6fd86e44411fafef9e70e8981d0fa8728af9e76
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048969"
---
# <a name="enable-or-disable-a-plan-guide"></a>Habilitar o deshabilitar una guía de plan
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Puede deshabilitar y habilitar las guías de plan de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Se puede habilitar o deshabilitar una única guía de plan o todas las guías de plan de una base de datos.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para deshabilitar y habilitar guías de plan mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Se producirá un error si se intenta quitar o modificar una función, procedimiento almacenado o desencadenador DML al que una guía de plan, habilitada o deshabilitada, haga referencia. Compruebe siempre las dependencias antes de quitar o modificar alguno de los objetos enumerados anteriormente.  
  
-   Al deshabilitar una guía de plan deshabilitada o habilitar una guía de plan habilitada, no se produce ningún cambio o error.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 La deshabilitación o habilitación de una guía de plan OBJECT requiere el permiso ALTER en el objeto (por ejemplo: función, procedimiento almacenado) al que hace referencia la guía de plan. Todas las demás guías de plan requieren el permiso ALTER DATABASE.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-disable-or-enable-a-plan-guide"></a>Para deshabilitar o habilitar una guía de plan  
  
1.  Haga clic en el signo más para expandir la base de datos en que desea deshabilitar o habilitar una guía de plan y, a continuación, haga clic en el signo más para expandir la carpeta **Programación** .  
  
2.  Haga clic en el signo más para expandir la carpeta **Guías de plan** .  
  
3.  Haga clic con el botón derecho en la guía de plan que quiera deshabilitar o habilitar y seleccione **Deshabilitar** o **Habilitar**.  
  
4.  En el cuadro de diálogo **Deshabilitar guía de plan** o **Habilitar guía de plan** , compruebe que la acción elegida se ha realizado correctamente y haga clic en **Cerrar**.  

#### <a name="to-disable-or-enable-all-plan-guides-in-a-database"></a>Para habilitar o deshabilitar todas las guías de plan de una base de datos  
  
1.  Haga clic en el signo más para expandir la base de datos en que desea deshabilitar o habilitar una guía de plan y, a continuación, haga clic en el signo más para expandir la carpeta **Programación** .  
  
2.  Haga clic con el botón derecho en la carpeta **Guías de plan** y, después, seleccione **Habilitar todo** o **Deshabilitar todo**.  
  
3.  En el cuadro de diálogo **Deshabilitar todas las guías de plan** o **Habilitar todas las guías de plan** , compruebe que la acción elegida se ha realizado correctamente y haga clic en **Cerrar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-disable-or-enable-a-plan-guide"></a>Para deshabilitar o habilitar una guía de plan  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    --Create a procedure on which to define the plan guide.  
    IF OBJECT_ID(N'Sales.GetSalesOrderByCountry', N'P') IS NOT NULL  
        DROP PROCEDURE Sales.GetSalesOrderByCountry;  
    GO  
    CREATE PROCEDURE Sales.GetSalesOrderByCountry   
        (@Country nvarchar(60))  
    AS  
    BEGIN  
        SELECT *  
        FROM Sales.SalesOrderHeader AS h   
        INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
        INNER JOIN Sales.SalesTerritory AS t ON c.TerritoryID = t.TerritoryID  
        WHERE t.CountryRegionCode = @Country;  
    END  
    GO  
    --Create the plan guide.  
    EXEC sp_create_plan_guide N'Guide3',  
        N'SELECT *  
        FROM Sales.SalesOrderHeader AS h   
        INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
        INNER JOIN Sales.SalesTerritory AS t ON c.TerritoryID = t.TerritoryID  
        WHERE t.CountryRegionCode = @Country',  
        N'OBJECT',  
        N'Sales.GetSalesOrderByCountry',  
        NULL,  
        N'OPTION (OPTIMIZE FOR (@Country = N''US''))';  
    --Disable the plan guide.  
    EXEC sp_control_plan_guide N'DISABLE', N'Guide3';  
    GO  
    --Enable the plan guide.  
    EXEC sp_control_plan_guide N'ENABLE', N'Guide3';  
    GO  
  
    ```  
  
#### <a name="to-disable-or-enable-all-plan-guides-in-a-database"></a>Para habilitar o deshabilitar todas las guías de plan de una base de datos  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    --Disable all plan guides in the database.  
    EXEC sp_control_plan_guide N'DISABLE ALL';  
    GO  
    --Enable all plan guides in the database.  
    EXEC sp_control_plan_guide N'ENABLE ALL';  
    GO  
  
    ```  
  
 Para obtener más información, vea [sp_control_plan_guide &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-control-plan-guide-transact-sql.md).  
  
  
