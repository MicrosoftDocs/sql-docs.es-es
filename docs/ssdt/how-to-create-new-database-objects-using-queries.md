---
title: Creación de nuevos objetos de base de datos mediante consultas
description: Familiarícese con el Editor de Transact-SQL. Vea cómo abrir este editor y ver ejemplos que muestran cómo se usa para crear una tabla, función o vista.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: ac983ac7-f9c4-495d-8a99-e1ba370fb271
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: c7afdd34756511df443c472eb6b80280fea32d96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018305"
---
# <a name="how-to-create-new-database-objects-using-queries"></a>Procedimientos: Creación de nuevos objetos de base de datos mediante consultas

Si prefiere usar scripts para crear o editar vistas, procedimientos almacenados, funciones, desencadenadores, tipos definidos por el usuario, etc., puede usar el Editor de Transact\-SQL. El Editor de Transact\-SQL proporciona IntelliSense y compatibilidad con otros lenguajes. Para más información, consulte [Uso del Editor de Transact-SQL para editar y ejecutar scripts](../ssdt/use-transact-sql-editor-to-edit-and-execute-scripts.md).  
  
El Editor de Transact\-SQL se invoca cuando se usa el menú contextual **Ver código** para abrir una entidad de base de datos en una base de datos conectada o en un proyecto. También se abre automáticamente cuando se usa el menú contextual **Nueva consulta** desde el Explorador de objetos de SQL Server o cuando se agrega un nuevo objeto de script a un proyecto de base de datos. Si no está conectado a una base de datos pero desea ejecutar una consulta en ella, también puede usar el cuadro de diálogo **Nueva conexión de consulta** si selecciona el menú **Editor de Transact-SQL** del menú **SQL** para conectar con una base de datos e iniciar el Editor de Transact\-SQL.  
  
> [!WARNING]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de la sección [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md).  
  
### <a name="to-create-a-new-table-using-a-transact-sql-query"></a>Para crear una nueva tabla usando una consulta de Transact\-SQL  
  
1.  Haga clic con el botón derecho en el nodo de la base de datos **Trade** y seleccione **Nueva consulta**.  
  
2.  En el panel de scripts, pegue este código:  
  
    ```  
  
    CREATE TABLE [dbo].[Fruits] (  
        [Id]         INT NOT NULL,  
        [Perishable] BIT DEFAULT ((1)) NULL,  
        PRIMARY KEY CLUSTERED ([Id] ASC),  
        FOREIGN KEY ([Id]) REFERENCES [dbo].[Products] ([Id])   
    );  
    ```  
  
3.  Haga clic en el botón **Ejecutar consulta** de la barra de herramientas del Editor de Transact\-SQL para ejecutar esta consulta.  
  
4.  Haga clic con el botón derecho en la base de datos **Trade** en el **Explorador de objetos de SQL Server** y seleccione **Actualizar**. Observe que se ha agregado la nueva tabla **Fruits** a la base de datos.  
  
### <a name="to-create-a-new-function"></a>Para crear una nueva función  
  
1.  Reemplace el código del Editor de Transact\-SQL actual por lo siguiente:  
  
    ```  
  
    CREATE FUNCTION [dbo].GetProductsBySupplier  
    (  
    @SupplierId int  
    )  
    RETURNS @returntable TABLE   
    (  
    [Id] int NOT NULL,   
    [Name] NVARCHAR (128) NOT NULL,  
    [Shelflife] INT NOT NULL,  
    [SupplierId] INT NOT NULL,  
    [CustomerId] INT NOT NULL  
    )  
    AS  
    BEGIN  
    INSERT @returntable  
    SELECT *  from Products p  
    where p.SupplierId = @SupplierId  
    RETURN   
    END  
    ```  
  
    Esta función devolverá todas las filas de la tabla `Products` cuyo `SupplierId` es igual al parámetro especificado. Haga clic en el botón **Ejecutar consulta** de la barra de herramientas del Editor de Transact\-SQL para ejecutar esta consulta.  
  
2.  En el Explorador de objetos de SQL Server, en el nodo **Trade**, expanda los nodos **Programación** y **Funciones**. Puede encontrar la nueva función recién creada bajo **Funciones con valores de tabla**.  
  
### <a name="to-create-a-new-view"></a>Para crear una nueva vista  
  
1.  Reemplace el código del Editor de Transact\-SQL actual por lo siguiente. Después, haga clic en el botón **Ejecutar consulta** situado encima del editor para ejecutar esta consulta.  
  
    ```  
    CREATE VIEW [dbo].PerishableFruits   
    AS SELECT p.Id, p.Name FROM dbo.Products p  
    join dbo.Fruits f on f.Id = p.Id  
    where f.Perishable = 1  
    ```  
  
2.  En el Explorador de objetos de SQL Server, en el nodo **Trade**, expanda el nodo **Vista** para encontrar la nueva vista recién creada.  
  
## <a name="see-also"></a>Consulte también  
[Administrar tablas y relaciones y corregir errores](../ssdt/manage-tables-relationships-and-fix-errors.md)  
[Usar el Editor de Transact-SQL para editar y ejecutar scripts](../ssdt/use-transact-sql-editor-to-edit-and-execute-scripts.md)  
  
