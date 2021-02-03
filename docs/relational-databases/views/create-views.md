---
description: Crear vistas
title: Creación de vistas | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- views [SQL Server], creating
ms.assetid: 0b7bd2a1-544c-42ba-8e7b-4822f34d7b64
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d7c7e339afa580f81a834021670cc81fc61ab8ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161904"
---
# <a name="create-views"></a>Crear vistas
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Puede crear vistas en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Se puede usar una vista para lo siguiente:  
  
-   Para centrar, simplificar y personalizar la percepción de la base de datos para cada usuario.  
  
-   Como mecanismo de seguridad, que permite a los usuarios obtener acceso a los datos por medio de la vista, pero no les conceden el permiso de obtener acceso directo a las tablas base subyacentes de la vista.  
  
-   Para proporcionar una interfaz compatible con versiones anteriores para emular una tabla cuyo esquema ha cambiado.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear una vista, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Una vista solo se puede crear en la base de datos actual.  
  
 Una vista puede tener un máximo de 1.024 columnas.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se necesita el permiso CREATE VIEW en la base de datos y el permiso ALTER en el esquema en que se crea la vista.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-view-by-using-the-query-and-view-designer"></a>Para crear una vista mediante el Diseñador de consultas y vistas  
  
1.  En el **Explorador de objetos**, expanda la base de datos donde desea crear la nueva vista.  
  
2.  Haga clic con el botón derecho en la carpeta **Vistas** y después haga clic en **Nueva vista...**.  
  
3.  En el cuadro de diálogo **Agregar tabla** , seleccione el elemento o elementos que desea incluir en la nueva vista desde una de las siguientes pestañas: Tablas, Vistas, Funciones y Sinónimos.  
  
4.  Haga clic en **Agregar** y, a continuación, en **Cerrar**.  
  
5.  En el **Panel de diagrama**, seleccione las columnas u otros elementos que desee incluir en la nueva vista.  
  
6.  En el **Panel de criterios**, seleccione criterios de ordenación o filtro adicionales para las columnas.  
  
7.  En el menú **Archivo** , haga clic en **Guardar**_view name_.  
  
8.  En el cuadro de diálogo **Elegir nombre** , especifique un nombre para la nueva vista y haga clic en **Aceptar**.  

     Para obtener más información sobre el Diseñador de consultas y vistas, vea [Herramientas Diseñador de consultas y vistas &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-view"></a>Para crear una vista  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012 ;   
    GO  
    CREATE VIEW HumanResources.EmployeeHireDate  
    AS  
    SELECT p.FirstName, p.LastName, e.HireDate  
    FROM HumanResources.Employee AS e JOIN Person.Person AS  p  
    ON e.BusinessEntityID = p.BusinessEntityID ;   
    GO  
    -- Query the view  
    SELECT FirstName, LastName, HireDate  
    FROM HumanResources.EmployeeHireDate  
    ORDER BY LastName;  
  
    ```  
  
 Para obtener más información, vea [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
