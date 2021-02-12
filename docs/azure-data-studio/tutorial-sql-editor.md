---
title: usar el editor de Transact-SQL para crear objetos de la base de datos
description: Siga este tutorial para aprender a usar el editor de Transact-SQL para realizar tareas básicas de base de datos, incluida la creación y búsqueda de objetos de base de datos.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: tutorial
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18; seo-lt-2019
ms.date: 09/24/2018
ms.openlocfilehash: 1c91b067ff8396968b2d26c57bf5f78d4f5d4181
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039935"
---
# <a name="tutorial-use-the-transact-sql-editor-to-create-database-objects---azure-data-studio"></a>Tutorial: Uso del editor de Transact-SQL para crear objetos de base de datos: Azure Data Studio

Crea y ejecutar consultas, procedimientos almacenados, scripts, etc., son las tareas principales de los profesionales de bases de datos. En este tutorial, se muestran las características principales del editor de T-SQL para crear objetos de bases de datos.

En este tutorial, obtendrá información sobre cómo usar [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] para:
> [!div class="checklist"]
> * Buscar objetos de la base de datos
> * Editar datos de una tabla 
> * Fragmentos de código para escribir rápidamente T-SQL
> * Ver detalles de objetos de la base de datos mediante *Ver la definición* e *Ir a la definición*


## <a name="prerequisites"></a>Requisitos previos

En este tutorial se requiere la base de datos *TutorialDB* de SQL Server o Azure SQL Database. Para crear la base de datos *TutorialDB*, complete uno de los siguientes inicios rápidos:

- [Conectarse a una instancia de SQL Server y consultarla con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-server.md)
- [Conectarse a una instancia de Azure SQL Database y consultarla con [!INCLUDE[name-sos-short](../includes/name-sos-short.md)]](quickstart-sql-database.md)


## <a name="quickly-locate-a-database-object-and-perform-a-common-task"></a>Buscar rápidamente un objeto de la base de datos y realizar una tarea común

[!INCLUDE[name-sos-short](../includes/name-sos-short.md)] proporciona un widget de búsqueda para encontrar rápidamente objetos de la base de datos. La lista de resultados proporciona un menú contextual de tareas comunes relevantes para el objeto seleccionado, como *Editar datos* para una tabla.

1. Abra la barra lateral SERVIDORES (**Ctrl+G**), expanda **Bases de datos** y seleccione **TutorialDB**. 

1. Para abrir el *panel de TutorialDB*, haga clic con el botón derecho en **TutorialDB** y seleccione **Administrar** en el menú contextual:

   ![menú contextual: administrar](./media/tutorial-sql-editor/insight-open-dashboard.png)

1. En el panel, haga clic con el botón derecho en **dbo.Customers** (en el widget de búsqueda) y seleccione **Editar datos**.
   
   > [!TIP]
   > Para las bases de datos con un gran número de objetos, use el widget de búsqueda para encontrar rápidamente la tabla, vista, etc., que esté buscando.

   ![widget de búsqueda rápida](./media/tutorial-sql-editor/quick-search-widget.png)

1. Edite la columna **Correo electrónico** en la primera fila, escriba *orlando0\@adventure-works.com* y presione **ENTRAR** para guardar el cambio.

   ![editar datos](./media/tutorial-sql-editor/edit-data.png)

## <a name="use-t-sql-snippets-to-create-stored-procedures"></a>Usar fragmentos de código de T-SQL para crear procedimientos almacenados

Azure Data Studio proporciona una gran cantidad de fragmentos de código de T-SQL integrados para crear rápidamente instrucciones.


1. Abra un nuevo editor de consultas al presionar **Ctrl+N**.

2. Escriba **sql** en el editor, desplácese con la flecha abajo hasta **sqlCreateStoredProcedure** y, después, presione la tecla *TAB* (o *Entrar*) para cargar el fragmento de código del procedimiento almacenado y creado.

   ![Captura de pantalla que muestra el Editor de consultas tras escribir SQL en el editor y seleccionar la opción sqlCreateStoredProcedure.](./media/tutorial-sql-editor/snippet-list.png)

3. El fragmento de código para crear un procedimiento almacenado tiene dos campos configurados para su edición rápida (*StoredProcedureName* y *SchemaName*). Seleccione *StoredProcedureName*, haga clic con el botón derecho y, después, seleccione **Cambiar todas las repeticiones**. Ahora, escriba *getCustomer* y todas las entradas de *StoredProcedureName* cambiarán a *getCustomer*.

   ![Captura de pantalla que muestra el Editor de consultas tras seleccionar la opción Cambiar todas las repeticiones.](./media/tutorial-sql-editor/snippet.png)

5. Cambie todas las repeticiones de *SchemaName* a *dbo*. 
6. El fragmento de código contiene texto de cuerpo y parámetros de marcador de posición que es necesario actualizar. La instrucción *EXECUTE* también contiene texto de marcador de posición porque no se sabe cuántos parámetros tendrá el procedimiento. Para este tutorial, actualice el fragmento de código de forma que se parezca al código siguiente:

    ```sql
    -- Create a new stored procedure called 'getCustomer' in schema 'dbo'
    -- Drop the stored procedure if it already exists
    IF EXISTS (
    SELECT *
    FROM INFORMATION_SCHEMA.ROUTINES
    WHERE SPECIFIC_SCHEMA = N'dbo'
    AND SPECIFIC_NAME = N'getCustomer'
    )
    DROP PROCEDURE dbo.getCustomer
    GO
    -- Create the stored procedure in the specified schema
    CREATE PROCEDURE dbo.getCustomer
    @ID int
    -- add more stored procedure parameters here
    AS
    -- body of the stored procedure
    SELECT  c.CustomerId, 
    c.Name, 
    c.Location, 
    c.Email
    FROM dbo.Customers c
    WHERE c.CustomerId = @ID
    FOR JSON PATH

    GO
    -- example to execute the stored procedure we just created
    EXECUTE dbo.getCustomer 1
    GO
    ```
    
5. Para crear el procedimiento almacenado y realizar una ejecución de prueba, presione **F5**.

Se creará el procedimiento almacenado y, en el panel de **RESULTADOS**, se mostrará el cliente devuelto en formato JSON. Para ver el código JSON con formato, haga clic en el registro devuelto. 


## <a name="use-peek-definition"></a>Usar Ver la definición 

Azure Data Studio permite ver la definición de un objeto con la característica "Ver la definición". En esta sección, se crea un segundo procedimiento almacenado y se usa opción “Ver la definición” para ver las columnas de una tabla y crear rápidamente el cuerpo del procedimiento almacenado.

1. Para abrir un nuevo editor, presione **Ctrl+N**. 

2. Escriba *sql* en el editor, desplácese con la flecha abajo hasta *sqlCreateStoredProcedure* y, después, presione la tecla *TAB* (o *Entrar*) para cargar el fragmento de código del procedimiento almacenado y creado.
3. Escriba *setCustomer* para *StoredProcedureName* y *dbo* para *SchemaName*.

3. Reemplace los marcadores de posición de @param por la siguiente definición de parámetros:

   ```sql
   @json_val nvarchar(max)
   ```

4. Reemplace el cuerpo del procedimiento almacenado por el código siguiente:
   ```sql
   INSERT INTO dbo.Customers
   ```

5. En la línea *INSERT* que acaba de agregar, haga clic con el botón derecho en **dbo.Customers** y seleccione **Ver la definición**.

   ![ver la definición](./media/tutorial-sql-editor/peek-definition.png)

6. Se mostrará la definición de la tabla para que pueda ver rápidamente las columnas de la tabla. Vea la lista de columnas para completar fácilmente las instrucciones del procedimiento almacenado. Termine de crear la instrucción INSERT que ha agregado anteriormente para completar el cuerpo del procedimiento almacenado y, después, cierre la ventana “Ver la definición”:

   ```sql
   INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
    )
   ```
7. Elimine (o marque como comentario) el comando *EXECUTE* de la parte inferior de la consulta.
8. La instrucción completa tiene que parecerse al código siguiente:

   ```sql
   -- Create a new stored procedure called 'setCustomer' in schema 'dbo'
   -- Drop the stored procedure if it already exists
   IF EXISTS (
   SELECT *
       FROM INFORMATION_SCHEMA.ROUTINES
       WHERE SPECIFIC_SCHEMA = N'dbo'
       AND SPECIFIC_NAME = N'setCustomer'
   )
   DROP PROCEDURE dbo.setCustomer
   GO
   -- Create the stored procedure in the specified schema
   CREATE PROCEDURE dbo.setCustomer
       @json_val nvarchar(max) 
   AS
       -- body of the stored procedure
       INSERT INTO dbo.Customers (CustomerId, Name, Location, Email)
       SELECT CustomerId, Name, Location, Email
       FROM OPENJSON (@json_val)
       WITH(   CustomerId int, 
               Name nvarchar(50), 
               Location nvarchar(50), 
               Email nvarchar(50)
       )
   GO
   ```

8. Para crear el procedimiento almacenado *setCustomer*, presione **F5**.

## <a name="use-save-query-results-as-json-to-test-the-setcustomer-stored-procedure"></a>Usar los resultados de una consulta guardada como JSON para probar el procedimiento almacenado setCustomer

El procedimiento almacenado *setCustomer* creado en la sección anterior necesita que se pasen datos JSON al parámetro *\@json_val*. En esta sección, se muestra cómo obtener código JSON con el formato correcto para pasarlo al parámetro con el fin de probar el procedimiento almacenado.

1. En la barra lateral **SERVIDORES**, haga clic con el botón derecho en la tabla *dbo.Customers* y seleccione **SELECT TOP 1000 Rows** (SELECCIONAR PRIMERAS 100 filas).

2. Seleccione la primera fila de la vista de resultados, asegúrese de que esté seleccionada toda la fila (haga clic en el número 1 de la columna del extremo izquierdo) y seleccione **Guardar como JSON**.  
3. Cambie la carpeta a una ubicación que recuerde para que pueda eliminar el archivo más tarde (por ejemplo, el Escritorio) y haga clic en **Guardar**. Se abrirá el archivo con formato JSON.

   ![guardar como JSON](./media/tutorial-sql-editor/save-as-json.png)

4. Seleccione los datos JSON en el editor y cópielos.
5. Para abrir un nuevo editor, presione **Ctrl+N**.
6. En los pasos anteriores, se muestra cómo obtener fácilmente los datos con el formato correcto para completar la llamada al procedimiento *setCustomer*. Como puede ver, en el código siguiente se usa el mismo formato JSON con nuevos datos de clientes para probar el procedimiento *setCustomer*. La instrucción contiene sintaxis para declarar el parámetro y ejecutar los nuevos procedimientos de obtener y establecer. Puede pegar los datos copiados de la sección anterior y editarlos para que coincidan con el ejemplo siguiente, o bien puede pegar la instrucción siguiente en el editor de consultas.

   ```sql
   -- example to execute the stored procedure we just created
   declare @json nvarchar(max) =
   N'[
       {
           "CustomerId": 5,
           "Name": "Lucy",
           "Location": "Canada",
           "Email": "lucy0@adventure-works.com"
       }
   ]'

   EXECUTE dbo.setCustomer @json_val = @json
   GO

   EXECUTE dbo.getCustomer @ID = 5
   ```

7. Para ejecutar el script, presione **F5**. El script inserta un nuevo cliente y devuelve la información del nuevo cliente en formato JSON. Haga clic en el resultado para abrir una vista con formato.

   ![resultado de la prueba](./media/tutorial-sql-editor/test-result.png)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido a:
> [!div class="checklist"]
> * Objetos del esquema de búsqueda rápida
> * Editar datos de una tabla 
> * Escribir un script de T-SQL con fragmentos de código
> * Información sobre detalles de objetos de la base de datos mediante “Ver la definición” e “Ir a la definición”


Para obtener información sobre cómo habilitar el widget de las **cinco consultas más lentas**, complete el tutorial siguiente:

> [!div class="nextstepaction"]
> [Habilitar el widget de conclusiones de ejemplo de consultas lentas](tutorial-qds-sql-server.md)
