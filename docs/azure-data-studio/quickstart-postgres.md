---
title: 'Inicio rápido: Conexión a PostgreSQL y consulta'
description: Realice este inicio rápido para usar Azure Data Studio para conectarse a PostgreSQL y, luego, emplear instrucciones SQL para crear y consultar una base de datos.
ms.custom: seodec18
ms.date: 09/18/2019
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu, maghan, sstein
ms.topic: quickstart
author: sunilagarwal
ms.author: sunila
ms.openlocfilehash: dad9f6348b3df9d7589be739af5ba7c748d37b6e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040087"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-postgresql"></a>Inicio rápido: Uso de Azure Data Studio para conectarse y consultar PostgreSQL

En este inicio rápido se muestra cómo usar Azure Data Studio para conectarse a PostgreSQL y, luego, emplear instrucciones SQL para crear la base de datos *tutorialdb* y consultarla.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido, necesita Azure Data Studio, la extensión PostgreSQL para Azure Data Studio y acceder a un servidor PostgreSQL.

- [Instale Azure Data Studio](./download-azure-data-studio.md).
- [Instale la extensión PostgreSQL para Azure Data Studio](./extensions/postgres-extension.md).
- [Instale PostgreSQL](https://www.postgresql.org/download/). (También puede crear una base de datos de Postgres en la nube mediante [az postgres up](/azure/postgresql/quickstart-create-server-up-azure-cli)). 

## <a name="connect-to-postgresql"></a>Conexión con PostgreSQL

1. Inicie **Azure Data Studio**.

2. La primera vez que lo inicie, se abre el cuadro de diálogo **Conexión**. Si el cuadro de diálogo **Conexión** no se abre, haga clic en el icono **Nueva conexión** en la página **SERVIDORES**:

   ![Icono de Nueva conexión](media/quickstart-postgresql/new-connection-icon.png)

3. En el formulario que aparece, vaya a **Tipo de conexión** y seleccione **PostgreSQL** en el menú desplegable.


4. Rellene los campos restantes con el nombre del servidor, el nombre del usuario y la contraseña del servidor PostgreSQL. 

   ![Pantalla Nueva conexión](media/quickstart-postgresql/new-connection-screen.png)  

   | Configuración       | Valor de ejemplo | Descripción |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Nombre del servidor** | localhost | Nombre completo del servidor |
   | **Nombre de usuario** | postgres | Nombre de usuario con el que quiere iniciar sesión. |
   | **Contraseña (Inicio de sesión de SQL)** | *password* | Contraseña de la cuenta con la que inicia sesión. |
   | **Contraseña** | *Comprobación* | Active esta casilla si no quiere escribir la contraseña cada vez que se conecte. |
   | **Nombre de la base de datos** | \<Default\> | Rellene esta opción si quiere que la conexión especifique una base de datos. |
   | **Grupo del servidor** | \<Default\> | Esta opción le permite asignar esta conexión a un grupo de servidores específico que cree. | 
   | **Nombre (opcional)** | *dejar en blanco* | Esta opción permite especificar un nombre descriptivo para el servidor. | 

5. Seleccione **Conectar**. 

Después de conectarse correctamente, el servidor se abre en la barra lateral **SERVIDORES**.


## <a name="create-a-database"></a>Crear una base de datos

En los pasos siguientes se crea una base de datos denominada **tutorialdb**:

1. Haga clic con el botón derecho en el servidor PostgreSQL en la barra lateral **SERVIDORES** y seleccione **Nueva consulta**.

2. Pegue esta instrucción SQL en el editor de consultas que se abre.

   ```sql
   CREATE DATABASE tutorialdb;
   ```

3. En la barra de herramientas, seleccione **Ejecutar** para ejecutar la consulta. Aparecen notificaciones en el panel **MENSAJES** que muestran el progreso de la consulta.

>[!TIP]
> Puede usar **F5** en el teclado para ejecutar la instrucción en lugar de utilizar **Ejecutar**.

Una vez finalizada la consulta, haga clic con el botón derecho en **Bases de datos** y seleccione **Actualizar** para ver **tutorialdb** en la lista del nodo **Bases de datos**.


## <a name="create-a-table"></a>Creación de una tabla

 En los pasos siguientes se crea una tabla en la **tutorialdb**:

1. Cambie el contexto de la conexión a **tutorialdb** con la lista desplegable del editor de consultas. 

   ![Cambiar el contexto](media/quickstart-postgresql/change-context.png)

2. Pegue la siguiente instrucción SQL en el editor de consultas y haga clic en **Ejecutar**. 

   > [!NOTE]
   > Puede anexarla o sobrescribir la consulta existente en el editor. Tenga en cuenta que al hacer clic en **Ejecutar** solo se ejecuta la consulta resaltada. Si no hay nada resaltado, al hacer clic en **Ejecutar** se ejecutan todas las consultas en el editor.

   ```sql
   -- Drop the table if it already exists
   DROP TABLE IF EXISTS customers;
   -- Create a new table called 'customers'
   CREATE TABLE customers(
       customer_id SERIAL PRIMARY KEY,
       name VARCHAR (50) NOT NULL,
       location VARCHAR (50) NOT NULL,
       email VARCHAR (50) NOT NULL
   );
   ```

## <a name="insert-rows"></a>Insertar filas

Pegue el siguiente fragmento de código en la ventana de consulta y haga clic en **Ejecutar**:

   ```sql
   -- Insert rows into table 'customers'
   INSERT INTO customers
       (customer_id, name, location, email)
    VALUES
      ( 1, 'Orlando', 'Australia', ''),
      ( 2, 'Keith', 'India', 'keith0@adventure-works.com'),
      ( 3, 'Donna', 'Germany', 'donna0@adventure-works.com'),
      ( 4, 'Janet', 'United States','janet1@adventure-works.com');
   ```

## <a name="query-the-data"></a>Consultar los datos

1. Pegue el siguiente fragmento de código en el editor de consultas y haga clic en **Ejecutar**:
   
   ```sql
   -- Select rows from table 'customers'
   SELECT * FROM customers; 
   ```

2. Se muestran los resultados de la consulta:

   ![Vista de resultados](media/quickstart-postgresql/view-results.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre los [escenarios disponibles para Postgres en Azure Data Studio](./extensions/postgres-extension.md).