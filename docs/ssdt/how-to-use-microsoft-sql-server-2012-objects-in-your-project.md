---
title: Objetos de SQL Server 2012 en un proyecto
description: Familiarícese con las secuencias de SQL Server 2012. Vea cómo agregar estos objetos a proyectos de base de datos y usarlos en consultas.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9baf122f-cf22-4860-98db-ef782cd972fc
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 9e27db8dbb247c65664006784d44966d0914d8bc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018045"
---
# <a name="how-to-use-microsoft-sql-server-2012-objects-in-your-project"></a>Procedimientos: Uso de objetos de Microsoft SQL Server 2012 en un proyecto

En este ejemplo, agregará un objeto de secuencia a un proyecto de base de datos destinado a Microsoft SQL Server 2012.  
  
Las secuencias se incluyeron en Microsoft SQL Server 2012. Una secuencia es un objeto enlazado a un esquema definido por el usuario que genera una secuencia de valores numéricos según la especificación con la que se creó la secuencia. La secuencia de valores numéricos se genera en orden ascendente o descendente en un intervalo definido y puede repetirse cuando se solicite.  Para más información sobre los objetos de secuencia consulte [Números de secuencia](../relational-databases/sequence-numbers/sequence-numbers.md). Para obtener información sobre las novedades de Microsoft SQL Server 2012, vea [Novedades de SQL Server 2012](/previous-versions/sql/sql-server-2012/bb500435(v=sql.110)).  
  
> [!WARNING]  
> Los procedimientos siguientes usan entidades creadas en procedimientos anteriores de las secciones [Desarrollo de bases de datos conectadas](../ssdt/connected-database-development.md) y [Desarrollo de bases de datos sin conexión orientado a proyectos](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-add-a-new-sequence-object-to-your-project"></a>Para agregar un nuevo objeto de secuencia a un proyecto  
  
1.  Haga clic con el botón derecho en el proyecto de base de datos **TradeDev** en el **Explorador de soluciones**, seleccione **Agregar** y, a continuación, haga clic en **Nuevo elemento**.  
  
2.  Haga clic en **Programación** en el panel izquierdo y seleccione **Secuencia**. Haga clic en **Agregar** para agregar el nuevo objeto al proyecto.  
  
3.  Reemplace el código predeterminado por el siguiente.  
  
    ```  
    CREATE SEQUENCE [dbo].[Seq1]  
    AS INT  
    START WITH 1  
    INCREMENT BY 1  
    MAXVALUE 1000  
    NO CYCLE  
    CACHE 10  
    ```  
  
4.  Si la plataforma de destino del proyecto no se ha establecido en Microsoft SQL Server 2012, la **Lista de errores** mostrará un error de sintaxis para la instrucción `CREATE SEQUENCE`. Para corregir este problema, vea [Cómo: Cambiar la plataforma de destino y publicar un proyecto de base de datos](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md) para cambiar la plataforma de destino según corresponda.  
  
5.  Siga el tema [Cómo: Cambiar la plataforma de destino y publicar un proyecto de base de datos](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md) para publicar el proyecto en una base de datos del servidor de Microsoft SQL Server 2012 conectado.  
  
### <a name="to-use-the-new-sequence-object"></a>Para usar el nuevo objeto de secuencia  
  
1.  En el Explorador de objetos de SQL Server, haga clic con el botón derecho en la base de datos en la que ha publicado en el procedimiento anterior y seleccione **Nueva consulta**.  
  
2.  Pegue el código siguiente en la ventana de consulta.  
  
    ```  
    DECLARE @counter INT  
    SET @counter=0  
    WHILE @counter<10  
    BEGIN  
        SET @counter = @counter +1  
         INSERT dbo.Products (Id, Name, CustomerId) VALUES (NEXT VALUE FOR dbo.Seq1, 'ProductItem'+cast(@counter as varchar), 1)  
    END   
    GO  
    ```  
  
3.  Presione el botón **Ejecutar consulta**.  
  
4.  En el **Explorador de objetos de SQL Server**, vaya a la tabla **Products** de la base de datos. Haga clic con el botón derecho y seleccione **Ver datos** para examinar las filas recién agregadas.  
