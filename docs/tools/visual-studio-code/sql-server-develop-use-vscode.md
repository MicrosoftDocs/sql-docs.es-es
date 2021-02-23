---
title: Uso de la extensión mssql de Visual Studio Code
description: Use la extensión mssql de Visual Studio Code para editar y ejecutar scripts de Transact-SQL para SQL Server en Linux.
ms.topic: conceptual
ms.prod: sql
ms.technology: tools-other
ms.assetid: 9766ee75-32d3-4045-82a6-4c7968bdbaa6
author: markingmyname
ms.author: maghan
ms.date: 10/28/2019
ms.openlocfilehash: fdb3d5e695824117def89347da883a0a7a5d7a9c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340546"
---
# <a name="use-visual-studio-code-to-create-and-run-transact-sql-scripts"></a>Empleo de Visual Studio Code para crear y ejecutar scripts de Transact-SQL

[!INCLUDE [SQL Server - Linux](../../includes/applies-to-version/sql-linux.md)]

En este artículo se muestra cómo usar la extensión **mssql** de Visual Studio Code para desarrollar bases de datos de SQL Server. Dado que Visual Studio Code es multiplataforma, puede usar la extensión **mssql** en Linux, macOS y Windows.

## <a name="install-and-start-visual-studio-code"></a>Instalación e inicio de iniciar Visual Studio Code

Visual Studio Code es un editor de código gráfico multiplataforma que admite extensiones.

1. [Descargue e instale Visual Studio Code](https://code.visualstudio.com/) en el equipo.

2. Inicie Visual Studio Code.

    >[!NOTE]
    >Si Visual Studio Code no se inicia cuando se conecta a través de una sesión de escritorio remoto de xrdp, vea [VS Code no funciona en Ubuntu al conectarse mediante XRDP](https://github.com/Microsoft/vscode/issues/3451).

## <a name="install-the-mssql-extension"></a>Instalación de la extensión mssql

La [extensión mssql de Visual Studio Code](https://aka.ms/mssql-marketplace) permite conectarse a SQL Server, consultar con Transact-SQL (T-SQL) y ver los resultados.

1. En Visual Studio Code, seleccione **Ver** > **Paleta de comandos**, presione **Ctrl**+**Mayús**+**P** o presione **F1** para abrir la **Paleta de comandos**.

2. En la **paleta de comandos**, seleccione **Extensiones: Instalar extensiones** en la lista desplegable.

3. En el panel **Extensiones**, escriba *mssql*.

4. Seleccione la extensión **SQL Server (mssql)** y luego **Instalar**.

   ![Instalación de la extensión mssql](./media/sql-server-develop-use-vscode/vscode-extension.png)

5. Una vez finalizada la instalación, seleccione **Recargar** para habilitar la extensión.

## <a name="create-or-open-a-sql-file"></a>Creación o apertura de un archivo SQL

La extensión mssql habilita los comandos de mssql y T-SQL IntelliSense en el editor de código cuando el modo de lenguaje está establecido en **SQL**.

1. Seleccione **Archivo** > **Nuevo archivo** o presione **Ctrl**+**N**. Visual Studio Code abre de forma predeterminada un nuevo archivo de texto sin formato. 

2. Seleccione **Texto sin formato** en la barra de estado inferior o presione **Ctrl**+**K** > **M** y seleccione **SQL** en la lista desplegable de lenguajes. 

   ![Modo de lenguaje SQL](./media/sql-server-develop-use-vscode/vscode-language-mode.png)

   > [!NOTE]
   > Si es la primera vez que usa la extensión, esta instala herramientas auxiliares de SQL Server.

Si abre un archivo existente con una extensión de archivo *.sql*, el modo de lenguaje se establece automáticamente en SQL.  

## <a name="connect-to-sql-server"></a>Conectar a SQL Server

Siga estos pasos para crear un perfil de conexión y conectarse a SQL Server.

1. Presione **Ctrl**+**Mayús**+**P** o **F1** para abrir la **Paleta de comandos**. 

2. Escriba *sql* para mostrar los comandos de mssql o escriba *sqlcon* y, luego, seleccione **MS SQL: Connect** (Conectar) en la lista desplegable.

   ![Comandos de mssql](./media/sql-server-develop-use-vscode/vscode-commands.png)

   >[!NOTE]
   >Un archivo SQL, como el archivo SQL vacío creado, debe tener el foco en el editor de código para poder ejecutar los comandos de mssql.

3. Seleccione el comando **MS SQL: Manage Connection Profiles** (Administrar perfiles de conexión).

4. Luego seleccione **Crear** para crear un nuevo perfil de conexión para SQL Server.

5. Siga los mensajes para especificar las propiedades del nuevo perfil de conexión. Después de especificar cada valor, presione **Entrar** para continuar.

   | Propiedad Conexión | Descripción |
   |---|---|
   | **Nombre del servidor o cadena de conexión ADO** | Especifique el nombre de la instancia de SQL Server. Use *localhost* para conectarse a una instancia de SQL Server en el equipo local. Para conectarse a una instancia remota de SQL Server, escriba el nombre del servidor SQL Server de destino o su dirección IP. Para conectarse a un contenedor de SQL Server, especifique la dirección IP del equipo host del contenedor. Si necesita especificar un puerto, use una coma para separarlo del nombre. Por ejemplo, en el caso de un servidor que escucha en el puerto 1401, escriba `<servername or IP>,1401`.<br/><br/>Como alternativa, puede escribir la cadena de conexión ADO de la base de datos aquí. |
   | **Nombre de la base de datos** (opcional) | Base de datos que se quiere usar. Para conectarse a la base de datos predeterminada, no especifique aquí ningún nombre de base de datos. |
   | **Tipo de autenticación** | Seleccione **Integrado** o **Inicio de sesión de SQL**. |
   | **Nombre de usuario** | Si ha seleccionado **Inicio de sesión de SQL**, escriba el nombre de un usuario con acceso a una base de datos en el servidor. |
   | **Contraseña** | Escriba la contraseña del usuario especificado. |
   | **Guardar contraseña** | Presione **Entrar** para seleccionar **Sí** y guardar la contraseña. Seleccione **No** para que se le pida la contraseña cada vez que se use el perfil de conexión. |
   | **Nombre de perfil** (opcional) | Escriba un nombre para el perfil de conexión, como *localhost profile*. |

   Después de escribir todos los valores y seleccionar **Entrar**, Visual Studio Code crea el perfil de conexión y se conecta a SQL Server.

   > [!TIP]
   > Si se produce un error en la conexión, intente diagnosticar el problema a partir del mensaje de error del panel **Salida** de Visual Studio Code. Para abrir el panel **Salida**, seleccione **Ver** > **Salida**. Además, revise las [recomendaciones para solucionar problemas de conexión](../../linux/sql-server-linux-troubleshooting-guide.md).

6. Compruebe la conexión en la barra de estado inferior.

   ![Estado de conexión](./media/sql-server-develop-use-vscode/vscode-connection-status.png)

Como alternativa a los pasos anteriores, también puede crear y editar perfiles de conexión en el archivo de configuración de usuario (*settings.json*). Para abrir el archivo de configuración, seleccione **Archivo** > **Preferencias** > **Configuración**. Para obtener más información, vea [Administración de perfiles de conexión](https://github.com/Microsoft/vscode-mssql/wiki/manage-connection-profiles).

## <a name="create-a-sql-database"></a>Creación de una base de datos SQL

1. En el nuevo archivo SQL que ha iniciado anteriormente, escriba *sql* para mostrar una lista de fragmentos de código modificables.

   ![Fragmentos de código SQL](./media/sql-server-develop-use-vscode/vscode-sql-snippets.png)

2. Seleccione **sqlCreateDatabase**.

3. En el fragmento de código, escriba `TutorialDB` para reemplazar "DatabaseName":

   ```sql
   -- Create a new database called 'TutorialDB'
   -- Connect to the 'master' database to run this snippet
   USE master
   GO
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO
   ```

4. Presione **Ctrl**+**Mayús**+**E** para ejecutar los comandos de Transact-SQL. Vea los resultados en la ventana de consulta.

    ![Creación de mensajes de base de datos](./media/sql-server-develop-use-vscode/vscode-create-database-messages.png)

    > [!TIP]
    > Puede personalizar las teclas de método abreviado de los comandos de mssql. Vea [Personalización de accesos directos](https://github.com/Microsoft/vscode-mssql/wiki/customize-shortcuts).

## <a name="create-a-table"></a>Creación de una tabla

1. Elimine el contenido de la ventana del editor de código.

2. Presione **Ctrl**+**Mayús**+**P** o **F1** para abrir la **Paleta de comandos**.

3. Escriba *sql* para mostrar los comandos de mssql o escriba *sqluse* y, luego, seleccione el comando **MS SQL: Use Database** (Usar base de datos).

4. Seleccione la nueva base de datos **TutorialDB**.

   ![Uso de las bases de datos](./media/sql-server-develop-use-vscode/vscode-use-database.png)

5. En el editor de código, escriba *sql* para mostrar los fragmentos de código, seleccione **sqlCreateTable** y presione **Entrar**.

6. En el fragmento de código, escriba `Employees` como nombre de la tabla.

7. Presione **Tab** para ir al siguiente campo y luego escriba `dbo` como nombre del esquema.

8. Reemplace las definiciones de columna por las columnas siguientes:

   ```sql
   EmployeesId INT NOT NULL PRIMARY KEY,
   Name [NVARCHAR](50)  NOT NULL,
   Location [NVARCHAR](50)  NOT NULL
   ```

9. Presione **Ctrl**+**Mayús**+**E** para crear la tabla.

## <a name="insert-and-query"></a>Inserción y consulta

1. Agregue las siguientes instrucciones para insertar cuatro filas en la tabla **Employees**.

   ```sql
   -- Insert rows into table 'Employees'
   INSERT INTO Employees
      ([EmployeesId],[Name],[Location])
   VALUES
      ( 1, N'Jared', N'Australia'),
      ( 2, N'Nikita', N'India'),
      ( 3, N'Tom', N'Germany'),
      ( 4, N'Jake', N'United States')
   GO
   -- Query the total count of employees
   SELECT COUNT(*) as EmployeeCount FROM dbo.Employees;
   -- Query all employee information
   SELECT e.EmployeesId, e.Name, e.Location
   FROM dbo.Employees as e
   GO
   ```

   A medida que escribe, T-SQL IntelliSense le ayuda a completar las instrucciones:

   ![T-SQL IntelliSense](./media/sql-server-develop-use-vscode/vscode-intellisense.png)

   > [!TIP]
   > La extensión mssql también tiene comandos que ayudan a crear instrucciones INSERT y SELECT. No se han usado en el ejemplo anterior.

2. Presione **Ctrl**+**Mayús**+**E** para ejecutar los comandos. Los dos conjuntos de resultados se muestran en la ventana **Resultados**.

   ![Results](./media/sql-server-develop-use-vscode/vscode-result-grid.png)

## <a name="view-and-save-the-result"></a>Consulta y guardado del resultado

1. Seleccione **Ver** > **Editor de diseño** > **Voltear diseño** para cambiar entre un diseño dividido vertical u horizontal.

2. Seleccione los encabezados de panel **Resultados** y **Mensajes** para contraer y expandir los paneles.

   ![Alternancia de encabezados](./media/sql-server-develop-use-vscode/vscode-toggle-messages-pannel.png)

   > [!TIP]
   > Puede personalizar el comportamiento predeterminado de la extensión mssql. Vea [Personalización de las opciones de extensión](https://github.com/Microsoft/vscode-mssql/wiki/customize-options).

3. Seleccione el icono de maximizar cuadrícula en la segunda cuadrícula de resultados para acercar los resultados.

   ![Maximización de la cuadrícula](./media/sql-server-develop-use-vscode/vscode-maximize-grid.png)

   > [!NOTE]
   > El icono de maximizar aparece cuando el script de T-SQL genera dos o más cuadrículas de resultados.

4. Abra el menú contextual de la cuadrícula al hacer clic con el botón derecho en la cuadrícula.

   ![Menú contextual](./media/sql-server-develop-use-vscode/vscode-grid-context-menu.png)

5. Seleccione **Seleccionar todo**.

6. Vuelva a abrir el menú contextual de la cuadrícula y seleccione **Guardar como JSON** para guardar el resultado en un archivo *.json*.

7. Especifique un nombre de archivo para el archivo JSON.

8. Compruebe que el archivo JSON se guarda y se abre en Visual Studio Code.

   ![Guardado como JSON](./media/sql-server-develop-use-vscode/vscode-save-as-json.png)

Si necesita guardar y ejecutar scripts de SQL más adelante, para la administración o un proyecto de desarrollo de mayor tamaño, guarde los scripts con una extensión *.sql*.

## <a name="next-steps"></a>Pasos siguientes

Si no está familiarizado con T-SQL, consulte los artículos [Tutorial: Escritura de instrucciones Transact-SQL](../../t-sql/tutorial-writing-transact-sql-statements.md) y [Referencia de Transact-SQL (motor de base de datos)](../../t-sql/language-reference.md).

Para obtener más información sobre el uso o la contribución a la extensión mssql, vea la [wiki del proyecto de extensión mssql](https://github.com/Microsoft/vscode-mssql/wiki).

Para obtener más información sobre el uso de Visual Studio Code, vea la [documentación de Visual Studio Code](https://code.visualstudio.com/docs).