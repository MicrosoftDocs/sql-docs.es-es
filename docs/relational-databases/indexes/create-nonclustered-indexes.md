---
description: Crear índices no clúster
title: Creación de índices no agrupados | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- index creation [SQL Server], nonclustered indexes
- nonclustered indexes [SQL Server], creating
- nonclustered indexes [SQL Server], UNIQUE constraint
- indexes [SQL Server], nonclustered
- nonclustered indexes [SQL Server], PRIMARY KEY constraint
ms.assetid: 9402029a-1227-46c4-93aa-c2122eb1b943
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9200edd16f0a33a52b12493660b18cfe45ae6086
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99048991"
---
# <a name="create-nonclustered-indexes"></a>Crear índices no clúster
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Puede crear índices no clúster en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un índice no clúster es una estructura de índice independiente de los datos almacenados en una tabla que reordena una o más columnas seleccionadas. Con frecuencia, los índices no clúster pueden ayudarle a encontrar datos más rápidamente que cuando se busca en la tabla subyacente; las consultas a veces pueden responderse completamente con los datos del índice no clúster o el índice no clúster puede apuntar el [!INCLUDE[ssDE](../../includes/ssde-md.md)] hacia las filas de la tabla subyacente. Normalmente, los índices no clúster se crean para mejorar el rendimiento de las consultas usadas con frecuencia no cubiertas por el índice clúster o para buscar filas en una tabla sin un índice clúster (denominado montón). Se pueden crear varios índices no clúster en una tabla o una vista indizada.  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="typical-implementations"></a><a name="Implementations"></a> Implementaciones típicas  
 Los índices no clúster se implementan de las formas siguientes:  
  
-   **Restricciones UNIQUE**  
  
     Cuando cree una restricción UNIQUE, se creará un índice no clúster único para exigir una restricción UNIQUE de forma predeterminada. Puede especificarse un índice clúster único si todavía no existe un índice clúster en la tabla. Para más información, consulte [Unique Constraints and Check Constraints](../../relational-databases/tables/unique-constraints-and-check-constraints.md).  
  
-   **Índice independiente de una restricción**  
  
     De forma predeterminada, se crea un índice no clúster si no hay ninguno clúster especificado. El número máximo de índices no clúster que se pueden crear por tabla es 999. Se incluyen los índices creados por restricciones PRIMARY KEY o UNIQUE, pero no los índices XML.  
  
-   **Índice no clúster en una vista indizada**  
  
     Una vez creado un índice clúster único en una vista, se pueden crear índices no clúster. Para obtener más información, vea [Crear vistas indexadas](../../relational-databases/views/create-indexed-views.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla o la vista. El usuario debe ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_ddladmin** y **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-nonclustered-index-by-using-the-table-designer"></a>Para crear un índice no clúster mediante el Diseñador de tablas  
  
1.  En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea crear un índice no clúster.  
  
2.  Expanda la carpeta **Tablas** .  
  
3.  Haga clic con el botón derecho en la tabla en la que quiere crear un índice no agrupado y seleccione **Diseño**.  
  
4.  En el menú **Diseñador de tablas** , haga clic en **Índices o claves**.  
  
5.  En el cuadro de diálogo **Índices o claves** , haga clic en **Agregar**.  
  
6.  Seleccione el nuevo índice en el cuadro de texto **Clave principal o única, o índice seleccionado** .  
  
7.  En la cuadrícula, seleccione **Crear como CLUSTERED** y elija **No** en la lista desplegable que aparece a la derecha de la propiedad.  
  
8.  Haga clic en **Cerrar**.  
  
9. En el menú **Archivo** , haga clic en **Guardar**_table_name_.  

#### <a name="to-create-a-nonclustered-index-by-using-object-explorer"></a>Para crear un índice no clúster mediante el Explorador de objetos  
  
1.  En el Explorador de objetos, expanda la base de datos que contiene la tabla en la que desea crear un índice no clúster.  
  
2.  Expanda la carpeta **Tablas** .  
  
3.  Expanda la tabla en la que desea crear un índice no clúster.  
  
4.  Haga clic con el botón derecho en la carpeta **Índices**, seleccione **Nuevo índice** y, luego, **Índice no agrupado...** .  
  
5.  En el cuadro de diálogo **Nuevo índice** , en la página **General** , escriba el nombre del nuevo índice en el cuadro **Nombre de índice** .  
  
6.  Debajo de **Columnas de clave de índice**, haga clic en **Agregar...** .  
  
7.  En el cuadro de diálogo **Seleccionar columnas de**_nombre_tabla_ , active las casillas de las columnas de tabla que se van a agregar al índice no agrupado.  
  
8.  Haga clic en **OK**.  
  
9. En el cuadro de diálogo **Nuevo índice** , haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-nonclustered-index-on-a-table"></a>Para crear un índice no clúster en una tabla  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```sql  
    USE AdventureWorks2012;  
    GO  
    -- Find an existing index named IX_ProductVendor_VendorID and delete it if found.   
    IF EXISTS (SELECT name FROM sys.indexes  
                WHERE name = N'IX_ProductVendor_VendorID')   
        DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;   
    GO  
    -- Create a nonclustered index called IX_ProductVendor_VendorID   
    -- on the Purchasing.ProductVendor table using the BusinessEntityID column.   
    CREATE NONCLUSTERED INDEX IX_ProductVendor_VendorID   
        ON Purchasing.ProductVendor (BusinessEntityID);   
    GO  
    ```  
  
## <a name="related-content"></a>Contenido relacionado  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
[Guía de diseño de índices de SQL Server](../../relational-databases/sql-server-index-design-guide.md) 
