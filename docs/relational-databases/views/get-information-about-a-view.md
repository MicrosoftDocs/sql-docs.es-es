---
description: Obtener información acerca de una vista
title: Obtener información sobre una vista | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.viewproperties.general.f1
helpviewer_keywords:
- views [SQL Server], status information
- metadata [SQL Server], views
- dependencies [SQL Server], views
- displaying view information
- views [SQL Server], metadata
- viewing view information
- status information [SQL Server], views
- view dependencies
ms.assetid: 05a73e33-8f85-4fb6-80c1-1b659e753403
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ae6a5c1c83832dc5c50d19289caec44df57ebe0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195804"
---
# <a name="get-information-about-a-view"></a>Obtener información acerca de una vista
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Puede obtener información sobre la definición o las propiedades de una vista de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Es posible que necesite ver la definición de la vista para entender cómo derivan sus datos de las tablas de origen o para ver los datos que ella misma define.  
  
> [!IMPORTANT]  
>  Si cambia el nombre de un objeto al que hace referencia una vista, deberá modificar ésta para que el texto refleje el nuevo nombre. Por lo tanto, antes de cambiar el nombre de un objeto, observe las dependencias del mismo a fin de determinar si el cambio propuesto afecta a alguna vista.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para obtener información acerca de una vista, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 La utilización de `sp_helptext` para devolver la definición de una vista requiere la pertenencia al rol **público** . La utilización de `sys.sql_expression_dependencies` para buscar todas las dependencias de una vista requiere el permiso VIEW DEFINITION en la base de datos y el permiso SELECT en `sys.sql_expression_dependencies` para la base de datos. Las definiciones de objeto del sistema, como las que se devuelven en SELECT OBJECT_DEFINITION, son visibles de forma pública.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="get-view-properties-by-using-object-explorer"></a>Obtener las propiedades de la vista mediante el Explorador de objetos  
  
1.  En el **Explorador de objetos**, haga clic en el signo más situado junto a la base de datos que contiene la vista cuyas propiedades desea ver y haga clic en el signo más para expandir la carpeta **Vistas** .  
  
2.  Haga clic con el botón derecho en la vista cuyas propiedades quiere ver y seleccione **Propiedades**.  

     Las propiedades siguientes se muestran en el cuadro de diálogo **Propiedades de la vista** .  
  
     **Base de datos**  
     Nombre de la base de datos que contiene esta vista.  
  
     **Server**  
     Nombre de la instancia de servidor actual.  
  
     **User**  
     Nombre del usuario de esta conexión.  
  
     **Fecha de creación**  
     Muestra la fecha en la que se creó la vista.  
  
     **Nombre**  
     Nombre de la vista actual.  
  
     **Esquema**  
     Muestra el esquema al que pertenece la vista.  
  
     **Objeto de sistema**  
     Indica si la vista es un objeto de sistema. Los valores son True y False.  
  
     **Valores NULL ANSI**  
     Indica si el objeto se ha creado con la opción Valores NULL ANSI.  
  
     **Cifrado**  
     Indica si la vista está cifrada. Los valores son True y False.  
  
     **Identificador entre comillas**  
     Indica si el objeto se ha creado con la opción Identificador entre comillas.  
  
     **Enlazada a un esquema**  
     Indica si la vista está enlazada a un esquema. Los valores son True y False. Para obtener más información sobre las vistas enlazadas a esquemas, vea la sección SCHEMABINDING de [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
#### <a name="getting-view-properties-by-using-the-view-designer-tool"></a>Obtener propiedades de la vista con la herramienta Diseñador de vistas  
  
1.  En el **Explorador de objetos**, expanda la base de datos que contiene la vista cuyas propiedades desea ver y, a continuación, expanda la carpeta **Vistas** .  
  
2.  Haga clic con el botón derecho en la vista cuyas propiedades quiere ver y seleccione **Diseño**.  
  
3.  Haga clic con el botón derecho en el espacio en blanco del panel Diagrama y haga clic en **Propiedades**.  
  
     En el panel **Propiedades** se muestran las propiedades siguientes.  
  
     **(Nombre)**  
     Nombre de la vista actual.  
  
     **Nombre de la base de datos**  
     Nombre de la base de datos que contiene esta vista.  
  
     **Descripción**  
     Descripción breve de la vista actual.  
  
     **Esquema**  
     Muestra el esquema al que pertenece la vista.  
  
     **Nombre de servidor**  
     Nombre de la instancia de servidor actual.  
  
     **Enlace a esquema**  
     Evita que los usuarios modifiquen los objetos subyacentes que contribuyen a esta vista de cualquier forma que invalide la definición de la vista.  
  
     **Determinista**  
     Muestra si se puede determinar con exactitud el tipo de datos de la columna seleccionada  
  
     **Valores DISTINCT**  
     Especifica que la consulta filtrará los duplicados en la vista. Esta opción es útil cuando solo se usan algunas columnas de una tabla y dichas columnas podrían contener valores duplicados, o cuando el proceso de combinar dos o más tablas genera filas duplicadas en el conjunto de resultados. Elegir esta opción equivale a insertar la palabra clave DISTINCT en la instrucción en el panel SQL.  
  
     **Extensión GROUP BY**  
     Especifica que están disponibles las opciones adicionales para vistas basadas en consultas de agregado.  
  
     **Todas las columnas**  
     Indica si la vista seleccionada devuelve todas las columnas. Esto se establece en el momento de crear la vista.  
  
     **Comentario de SQL**  
     Muestra una descripción de las instrucciones SQL. Para ver o editar la descripción completa, haga clic en la descripción y después en el botón de puntos suspensivos **(...)** situado a la derecha de la propiedad. Los comentarios pueden incluir información, como quién usa la vista y cuándo.  
  
     **Especificación superior**  
     Se expande para mostrar las propiedades **Superior**, **Expresión**, **Porcentaje** y **Con valores equivalentes** .  
  
     **(Superior)**  
     Especifica que la vista incluirá una cláusula TOP, que solo devuelve las primeras n filas o el primer n por cierto de filas en el conjunto de resultados. De forma predeterminada, la vista devolverá las diez primeras filas en el conjunto de resultados. Use esto para cambiar el número de filas que se van a devolver o para especificar un porcentaje diferente.  
  
     **Expression**  
     Muestra qué porcentaje (si **Porcentaje** está establecido en **Sí**) o registros (si **Porcentaje** está establecido en **No**) devolverá la vista.  
  
     **Porcentaje**  
     Especifica que la consulta incluirá una cláusula **TOP** y solo devolverá el primer n por ciento de filas en el conjunto de resultados.  
  
     **Con valores equivalentes**  
     Especifica que la vista incluirá una cláusula **WITH TIES** . **WITH TIES** resulta útil si una vista incluye una cláusula **ORDER BY** y una cláusula **TOP** basadas en un porcentaje. Si se establece esta opción y el límite del porcentaje queda dentro de un conjunto de filas con valores idénticos en la cláusula **ORDER BY** , se ampliará la vista hasta que incluya todas esas filas.  
  
     **Especificación de actualización**  
     Se expande para mostrar las propiedades de **Actualizar con reglas de vista** y de **Opción CHECK** .  
  
     **(Actualizar con reglas de vista)**  
     Indica que Microsoft Data Access Components (MDAC) traducirá todas las actualizaciones e inserciones de la vista a instrucciones SQL que hacen referencia a la vista, en lugar de a las instrucciones SQL que hacen referencia directamente a las tablas base de la vista.  
  
     En algunos casos, MDAC indica las operaciones de inserción y actualización de la vista como actualizaciones y las inserta en las tablas base subyacentes de la vista. Si selecciona **Actualizar con reglas de vista**, puede asegurarse de que MDAC generará las operaciones de inserción y actualización en la propia vista.  
  
     **Opción CHECK**  
     Indica que al abrir esta vista y modificar el panel **Resultados** , el origen de datos comprueba si los datos agregados o modificados cumplen la cláusula **WHERE** de la definición de la vista. Si la edición no cumple la cláusula **WHERE** , aparecerá un error con más información.  
  
#### <a name="to-get-dependencies-on-the-view"></a>Para obtener las dependencias de la vista  
  
1.  En el **Explorador de objetos**, expanda la base de datos que contiene la vista cuyas propiedades desea ver y, a continuación, expanda la carpeta **Vistas** .  
  
2.  Haga clic con el botón derecho en la vista cuyas propiedades quiere ver y seleccione **Ver dependencias**.  
  
3.  Seleccione **Objetos que dependen de [nombre de vista]** para mostrar los objetos que hacen referencia a la vista.  
  
4.  Seleccione **Objetos de los que depende [nombre de vista]** para mostrar los objetos a los que hace referencia la vista.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-get-the-definition-and-properties-of-a-view"></a>Para obtener la definición y propiedades de una vista  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue uno de los ejemplos siguientes en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT definition, uses_ansi_nulls, uses_quoted_identifier, is_schema_bound  
    FROM sys.sql_modules  
    WHERE object_id = OBJECT_ID('HumanResources.vEmployee');   
    GO  
    ```  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT OBJECT_DEFINITION (OBJECT_ID('HumanResources.vEmployee')) AS ObjectDefinition;   
    GO  
    ```  
  
    ```  
    EXEC sp_helptext 'HumanResources.vEmployee';  
    ```  
  
 Para obtener más información, vea [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md), [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md) y [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md).  
  
#### <a name="to-get-the-dependencies-of-a-view"></a>Para obtener las dependencias de una vista  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
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
    WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');  
    GO  
    ```  
  
 Para obtener más información, vea [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) y [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
  
