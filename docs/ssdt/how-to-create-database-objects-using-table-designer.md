---
title: Creación de objetos de base de datos usando el Diseñador de tablas
description: Aprenda a crear una base de datos en el Explorador de objetos de SQL Server. Vea cómo crear tablas, restricciones y referencias de clave externa en el Diseñador de tablas.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.design.table.scriptpanel
- sql.data.tools.design.table.context.view
- Microsoft.Data.Relational.Design.PW.RelationshipsDescriptor.OnDelete
ms.assetid: 9c9479c1-9bfc-4039-837e-e53fce67723d
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: a055bd1f8de41686e1c3477e3e4810e78d6b5025
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018315"
---
# <a name="how-to-create-database-objects-using-table-designer"></a>Procedimientos: Creación de objetos de base de datos usando el Diseñador de tablas

El nuevo nodo **SQL Server** del **Explorador de objetos de SQL Server** no solo es muy similar a SSMS visualmente, sino que puede crear nuevos objetos usando menús contextuales que funcionan de manera similar a sus homólogos de SSMS.  
  
Por ejemplo, puede crear una base de datos nueva bajo el nodo **Bases de datos**. Del mismo modo, puede seleccionar una base de datos específica y crear o editar definiciones de tabla y sus objetos de programación relacionados sobre la marcha usando el nuevo Diseñador de tablas. Desde el Diseñador de tablas, puede cambiar a un panel de scripts que le permite editar directamente el script que define esta tabla.  
  
### <a name="to-create-a-new-database"></a>Para crear una nueva base de datos  
  
1.  En el **Explorador de objetos de SQL Server**, en el nodo **SQL Server**, expanda la instancia de servidor a la que se ha conectado.  
  
2.  Haga clic con el botón derecho en el nodo **Bases de datos** y seleccione **Agregar nueva base de datos**.  
  
3.  Cambie el nombre de la nueva base de datos a **Trade**.  
  
### <a name="to-create-new-tables-using-the-table-designer"></a>Para crear nuevas tablas usando el Diseñador de tablas  
  
1.  Expanda el nodo **Trade** recién creado. Haga clic con el botón derecho en el nodo **Tablas** y seleccione **Agregar nueva tabla**.  
  
2.  El Diseñador de tablas se abrirá en una ventana nueva. El diseñador consta de la cuadrícula de columnas, el panel de scripts y el panel Contexto. La cuadrícula de columnas muestra todas las columnas de la tabla. Examinaremos otros componentes del diseñador en procedimientos posteriores.  
  
3.  En el panel de scripts, cambie el nombre de la nueva tabla a `Suppliers`. En concreto, reemplace  
  
    ```  
    CREATE TABLE [dbo].[Table1]  
    ```  
  
    con  
  
    ```  
    CREATE TABLE [dbo].[Suppliers]  
    ```  
  
4.  Haga clic en la fila vacía en la cuadrícula de columnas para agregar una nueva columna a la tabla.  Escriba **CompanyName** en el campo **Nombre**, **nvarchar (128)** en **Tipo de datos** y desactive el campo **Permitir valores NULL**. A medida que cambia entre los campos, observe que el panel de scripts se actualiza inmediatamente.  
  
5.  Agregue otra columna nueva. Escriba **Address** en el campo **Nombre**, **nvarchar (MAX)** en **Tipo de datos** y desactive el campo **Permitir valores NULL**.  
  
    > [!WARNING]  
    > Cuando esté editando objetos de una base de datos conectada, no los guarde en la unidad local. Para guardar los cambios a la base de datos correctamente, siga los pasos indicados en el siguiente procedimiento [Cómo: Actualizar una base de datos conectada con Power Buffer](../ssdt/how-to-update-a-connected-database-with-power-buffer.md).  
  
6.  Repita los pasos anteriores para crear otra tabla denominada **Customer**. Esta vez, agregue las siguientes columnas a la tabla Customer usando la cuadrícula de columnas. No olvide modificar el script para que el nombre de la tabla sea `[dbo].[Customer]`.  
  
    |Nombre|Tipo de datos|**Permitir valores NULL**|  
    |--------|-------------|-------------------|  
    |Identificador|int|Desactivado|  
    |Nombre|nvarchar (128)|Desactivado|  
  
7.  Cree una tabla más denominada **Products**. Agregue las siguientes columnas a la tabla Products usando la cuadrícula de columnas. No olvide modificar el script para que el nombre de la tabla sea `[dbo].[Products]`.  
  
    |Nombre|Tipo de datos|**Permitir valores NULL**|  
    |--------|-------------|-------------------|  
    |Identificador|int|Desactivado|  
    |Nombre|nvarchar (128)|Desactivado|  
    |ShelfLife|int|Activado|  
    |SupplierId|int|Activado|  
    |CustomerId|int|Activado|  
  
### <a name="to-create-a-new-check-constraint-using-the-table-designer"></a>Para crear una nueva restricción CHECK usando el Diseñador de tablas  
  
1.  El panel Contexto del Diseñador de tablas le ofrece una vista lógica de la definición de tabla (claves, restricciones, desencadenadores, etc.) y le permite seleccionar un objeto para resaltar sus relaciones con las columnas individuales.  
  
    Para la tabla Products, haga clic con el botón derecho en el nodo **Restricciones CHECK** en el panel Contexto del Diseñador de tablas y seleccione **Agregar nueva restricción CHECK**.  
  
2.  Observe que el número de nodo se incrementa automáticamente en 1.  
  
3.  Haga clic en el panel de scripts y reemplace la definición predeterminada de la restricción con lo siguiente.  
  
    ```  
    CONSTRAINT [CK_Products_ShelfLife] CHECK ([ShelfLife] <5),  
    ```  
  
    Esta restricción limitará el valor de ShelfLife para una fila a menos de 5.  
  
### <a name="to-create-new-foreign-key-references-using-the-table-designer"></a>Para crear nuevas referencias de clave externa usando el Diseñador de tablas  
  
1.  Para la tabla Products, haga clic con el botón derecho en el nodo **Claves externas** en el panel Contexto y seleccione **Agregar nueva clave externa**.  
  
2.  Observe que el número de nodo se incrementa automáticamente en 1.  
  
3.  Haga clic en el panel de scripts y reemplace la definición predeterminada de la referencia de clave externa con lo siguiente.  
  
    ```  
    CONSTRAINT [FK_Products_SupplierId] FOREIGN KEY ([SupplierId]) REFERENCES [dbo].[Suppliers] ([Id]),  
    ```  
  
4.  Repita los pasos anteriores para agregar otra referencia de clave externa a la tabla Products. Esta vez, reemplace la definición predeterminada con lo siguiente.  
  
    ```  
    CONSTRAINT [FK_Products_CustomerId] FOREIGN KEY ([CustomerId]) REFERENCES [dbo].[Customer] ([Id])  
    ```  
  
## <a name="see-also"></a>Consulte también  
[Administrar tablas y relaciones y corregir errores](../ssdt/manage-tables-relationships-and-fix-errors.md)  
  
