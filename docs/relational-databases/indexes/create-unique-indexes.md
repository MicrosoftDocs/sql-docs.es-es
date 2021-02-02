---
description: Crear índices únicos
title: Creación de nombre de los índices | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- unique indexes
- designing indexes [SQL Server], unique
- clustered indexes, unique
- indexes [SQL Server], unique
- nonclustered indexes [SQL Server], unique
- unique indexes, design guidelines
ms.assetid: 56b5982e-cb94-46c0-8fbb-772fc275354a
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e2c41e97ad36387fe1f3644e5039eb5cd2b6a61d
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048963"
---
# <a name="create-unique-indexes"></a>Crear índices únicos
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  En este tema se describe cómo crear un índice único en una tabla de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un índice único garantiza que la clave de índice no contiene valores duplicados y, por tanto, cada fila de la tabla es en cierta forma única. No existen diferencias significativas entre crear una restricción UNIQUE y crear un índice único que es independiente de una restricción. La validación de datos se produce de igual modo y el optimizador de consultas no distingue entre un índice único creado mediante una restricción o creado manualmente. Sin embargo, la creación de una restricción UNIQUE en la columna aclara el objetivo del índice. Para obtener más información acerca de las restricciones UNIQUE, vea [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
 Cuando cree o modifique un índice único, puede establecer una opción para omitir claves duplicadas. Si esta opción está establecida en **Sí** e intenta crear claves duplicadas al agregar datos que afectan a varias filas (con la instrucción INSERT), no se agregará la fila que contenga un duplicado. Si el valor es **No**, se producirá un error en toda la operación de inserción y se revertirán todos los datos.  
  
> [!NOTE]  
>  No puede crear un índice único en una sola columna si ésta contiene valores NULL en más de una fila. De forma similar, no puede crear un índice único en varias columnas si la combinación de columnas contiene valores NULL en más de una fila. Estos valores se tratan como duplicados a efectos de indización.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Ventajas de un índice único](#Benefits)  
  
     [Implementaciones típicas](#Implementations)  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear un índice único en una tabla, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="benefits-of-a-unique-index"></a><a name="Benefits"></a> Ventajas de un índice único  
  
-   Los índices únicos para varias columnas garantizan que cada combinación de valores de la clave de índice sea única. Por ejemplo, si se crea un índice único en una combinación de columnas **LastName**, **FirstName** y **MiddleName** , dos filas de la tabla no podrán tener la misma combinación de valores para estas columnas.  
  
-   Siempre que los datos de cada columna sean únicos, puede crear un índice clúster único y varios índices no clúster únicos en la misma tabla.  
  
-   Los índices únicos aseguran la integridad de los datos de las columnas definidas.  
  
-   Los índices únicos proporcionan información adicional útil al optimizador de consultas, que puede generar planes más eficaces de ejecución.  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> Implementaciones típicas  
 Los índices únicos se implementan de las formas siguientes:  
  
-   **Restricción PRIMARY KEY o UNIQUE**  
  
     Cuando se crea una restricción PRIMARY KEY, se crea automáticamente un índice clúster único en las columnas si aún no existe un índice clúster en la tabla o no se ha especificado un índice no clúster. La columna de clave principal no puede permitir valores NULL.  
  
     Cuando cree una restricción UNIQUE, se creará un índice no clúster único para exigir una restricción UNIQUE de forma predeterminada. Puede especificarse un índice clúster único si todavía no existe un índice clúster en la tabla.  
  
     Para obtener más información, consulte [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md) y [Primary and Foreign Key Constraints](../../relational-databases/tables/primary-and-foreign-key-constraints.md).  
  
-   **Índice independiente de una restricción**  
  
     Es posible definir varios índices no clúster únicos en cualquier tabla.  
  
     Para obtener más información, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
-   **Vistas indizadas**  
  
     Para crear una vista indizada, se define un índice clúster único en una o varias columnas de la vista. La vista se ejecuta y el conjunto de resultados se almacena en el nivel hoja del índice, del mismo modo en que los datos de tabla se almacenan en un índice clúster. Para obtener más información, vea [Crear vistas indexadas](../../relational-databases/views/create-indexed-views.md).  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Un índice único, una restricción UNIQUE o una restricción PRIMARY KEY no se pueden crear si existen valores de clave duplicados en los datos.  
  
-   Un índice no clúster único puede incluir columnas sin clave. Para más información, consulte [Create Indexes with Included Columns](../../relational-databases/indexes/create-indexes-with-included-columns.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla o la vista. El usuario debe ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_ddladmin** y **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-unique-index-by-using-the-table-designer"></a>Para crear un índice único mediante el Diseñador de tablas  
  
1.  En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea crear un índice único.  
  
2.  Expanda la carpeta **Tablas** .  
  
3.  Haga clic con el botón derecho en la tabla en la que quiere crear un índice único y seleccione **Diseño**.  
  
4.  En el menú **Diseñador de tablas** , seleccione **Índices o claves**.  
  
5.  En el cuadro de diálogo **Índices o claves** , haga clic en **Agregar**.  
  
6.  Seleccione el nuevo índice en el cuadro de texto **Clave principal o única, o índice seleccionado** .  
  
7.  En la cuadrícula principal, en **(General)** , seleccione **Tipo** y luego **Índice** en la lista.  
  
8.  Seleccione **Columnas** y luego haga clic en el botón de puntos suspensivos **(...)**.  
  
9. En el cuadro de diálogo **Columnas de índice** , debajo de **Nombre de columna**, seleccione las columnas que desea indizar. Puede seleccionar hasta 16 columnas. Para obtener un rendimiento óptimo, no seleccione más de una o dos columnas por cada índice. Para cada columna que seleccione, puede indicar si el índice organiza los valores de esta columna en orden ascendente o descendente.  
  
10. Cuando haya seleccionado todas las columnas del índice, haga clic en **Aceptar**.  
  
11. En la cuadrícula, en **(General)** , seleccione **Es Unique** y luego **Sí** en la lista.  
  
12. Opcional: en la cuadrícula principal, debajo de **Diseñador de tablas**, seleccione **Omitir claves duplicadas** y elija **Sí** en la lista. Haga esto si desea omitir los intentos de agregar datos que crearían una clave duplicada en el índice único.  
  
13. Haga clic en **Cerrar**.  
  
14. En el menú **Archivo**, haga clic en **Guardar**_nombre\_tabla_.  
  
#### <a name="create-a-unique-index-by-using-object-explorer"></a>Crear un índice único mediante el Explorador de objetos  
  
1.  En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea crear un índice único.  
  
2.  Expanda la carpeta **Tablas** .  
  
3.  Expanda la tabla en la que desea crear un índice único.  
  
4.  Haga clic con el botón derecho en la carpeta **Índices**, seleccione **Nuevo índice** y, luego, **Índice no agrupado...** .  
  
5.  En el cuadro de diálogo **Nuevo índice** , en la página **General** , escriba el nombre del nuevo índice en el cuadro **Nombre de índice** .  
  
6.  Active la casilla **Único** .  
  
7.  Debajo de **Columnas de clave de índice**, haga clic en **Agregar...** .  
  
8.  En el cuadro de diálogo **Seleccionar columnas de**_nombre\_tabla_, active las casillas de las columnas de tabla que se van a agregar al índice único.  
  
9. Haga clic en **OK**.  
  
10. En el cuadro de diálogo **Nuevo índice** , haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-unique-index-on-a-table"></a>Para crear un índice único en una tabla  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Find an existing index named AK_UnitMeasure_Name and delete it if found  
    IF EXISTS (SELECT name from sys.indexes  
               WHERE name = N'AK_UnitMeasure_Name')   
       DROP INDEX AK_UnitMeasure_Name ON Production.UnitMeasure;   
    GO  
    -- Create a unique index called AK_UnitMeasure_Name  
    -- on the Production.UnitMeasure table using the Name column.  
    CREATE UNIQUE INDEX AK_UnitMeasure_Name   
       ON Production.UnitMeasure (Name);   
    GO  
    ```  
  
 Para obtener más información, vea [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
