---
title: Empleo de SSMS para administrar SQL Server en Linux
description: En este artículo se presenta SQL Server Management Studio, un entorno integrado que permite acceder a componentes de SQL Server, configurarlos, administrarlos y desarrollarlos.
author: VanMSFT
ms.author: vanto
ms.date: 05/21/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b2fcf858-21c3-462a-8d49-50c85647d092
ms.openlocfilehash: 758c4e8165efd9fd24a979bfd3a1390e88b765ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345640"
---
# <a name="use-sql-server-management-studio-on-windows-to-manage-sql-server-on-linux"></a>Empleo de SQL Server Management Studio en Windows para administrar SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se presenta [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) y se realizan un par de tareas comunes. SSMS es una aplicación Windows, así que use SSMS cuando tenga un equipo Windows que pueda conectarse a una instancia remota de SQL Server en Linux.

> [!TIP]
> Si no tiene un equipo Windows en el que ejecutar SSMS, tenga en cuenta el nuevo [Azure Data Studio](../azure-data-studio/index.yml). Proporciona una herramienta gráfica para administrar SQL Server y se ejecuta tanto en Linux como en Windows.

[SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) forma parte de un conjunto de herramientas de SQL que Microsoft ofrece de forma gratuita para las necesidades de desarrollo y administración. SSMS es un entorno integrado que permite acceder a todos los componentes de SQL Server, configurarlos, administrarlos y desarrollarlos. Puede conectarse a una instancia de SQL Server que se ejecute en cualquier plataforma, tanto en entornos locales como en contenedores de Docker y en la nube. También se conecta a Azure SQL Database y Azure Synapse Analytics. SSMS combina un amplio grupo de herramientas gráficas con una serie de editores de scripts enriquecidos que permiten a los desarrolladores y administradores de cualquier nivel de conocimientos acceder a SQL Server.

SSMS ofrece un amplio conjunto de capacidades de desarrollo y administración para SQL Server, incluidas herramientas para:

- Configurar, supervisar y administrar una o varias instancias de SQL Server
- Implementar, supervisar y actualizar componentes de capa de datos como bases de datos y almacenes de datos
- Realizar copias de seguridad de bases de datos y restaurarlas
- Compilar y ejecutar consultas y scripts de T-SQL y ver los resultados
- Generar scripts de T-SQL para objetos de base de datos
- Ver y editar datos de bases de datos
- Diseñar visualmente consultas de T-SQL y objetos de base de datos como vistas, tablas y procedimientos almacenados

Vea [¿Qué es SQL Server Management Studio (SSMS)?](../ssms/sql-server-management-studio-ssms.md) para obtener más información sobre SSMS.

## <a name="install-the-newest-version-of-sql-server-management-studio-ssms"></a>Instalar la última versión de SQL Server Management Studio (SSMS)

Cuando trabaje con SQL Server, siempre debe usar la versión más reciente de SQL Server Management Studio (SSMS). La versión más reciente de SSMS se actualiza y optimiza continuamente y, en la actualidad, funciona con SQL Server en Linux. Para descargar e instalar la versión más reciente, vea [Descarga de SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md). Para que se mantenga actualizado, la versión más reciente de SSMS le indica cuándo hay una nueva versión disponible para descargar.

> [!NOTE]
> Antes de usar SSMS para administrar Linux, vea los [problemas conocidos](sql-server-linux-release-notes.md) de SSMS en Linux.

## <a name="connect-to-sql-server-on-linux"></a>Conectarse a SQL Server en Linux

Siga los pasos básicos siguientes para conectarse:

1. Para iniciar SSMS, escriba **Microsoft SQL Server Management Studio** en el cuadro de búsqueda de Windows y, luego, haga clic en la aplicación de escritorio.

    ![SQL Server Management Studio](./media/sql-server-linux-manage-ssms/ssms.png)

1. En la ventana **Conectar al servidor**, escriba la información siguiente (si SSMS ya se está ejecutando, haga clic en **Conectar > Motor de base de datos** para abrir la ventana **Conectar al servidor**):

   | Configuración | Descripción |
   |-----|-----|
   | **Tipo de servidor** | El valor predeterminado es motor de base de datos; no cambie este valor. |
   | **Nombre del servidor** | Escriba el nombre del equipo de SQL Server para Linux de destino o su dirección IP y su puerto con el formato `IP,port`. |
   | **Autenticación** | En SQL Server en Linux, use **Autenticación de SQL Server**. |
   | **Inicio de sesión** | Escriba el nombre de un usuario con acceso a una base de datos en el servidor (por ejemplo, la cuenta de **SA** predeterminada creada durante la instalación). |
   | **Contraseña** | Escriba la contraseña del usuario especificado (para la cuenta de **SA** creada durante la instalación). |

    ![SQL Server Management Studio: Conexión con el servidor de SQL Database](./media/sql-server-linux-manage-ssms/connect.png)

1. Haga clic en **Conectar**.

    > [!TIP]
    > Si recibe un error de conexión, intente primero diagnosticar el problema a partir del mensaje de error. Luego revise las [recomendaciones para solucionar problemas de conexión](sql-server-linux-troubleshooting-guide.md#connection).
 
1. Después de conectarse correctamente a SQL Server, se abre el **Explorador de objetos** y ya puede acceder a la base de datos para realizar tareas administrativas o consultar datos.

## <a name="run-transact-sql-queries"></a>Ejecutar consultas de Transact-SQL

Después de conectarse al servidor, puede conectarse a una base de datos y ejecutar consultas de Transact-SQL. Las consultas de Transact-SQL se pueden usar para prácticamente cualquier tarea de base de datos.

1. En el **Explorador de objetos**, vaya a la base de datos de destino en el servidor. Por ejemplo, expanda **Bases de datos del sistema** para trabajar con la base de datos **maestra**.

1. Haga clic con el botón derecho en la base de datos y, luego, seleccione **Nueva consulta**.

1. En la ventana de consulta, escriba una consulta de Transact-SQL para devolver los nombres de todas las bases de datos del servidor.

   ```sql
   SELECT [Name]
   FROM sys.Databases
   ```

   Si no está familiarizado con la escritura de consultas, vea [Escritura de instrucciones Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md).

1. Haga clic en el botón **Ejecutar** para ejecutar la consulta y ver los resultados.

   ![Correcto. Conexión al servidor de SQL Database: SQL Server Management Studio](./media/sql-server-linux-manage-ssms/execute-query.png)

Aunque con las consultas de Transact-SQL es posible realizar prácticamente cualquier tarea de administración, SSMS es una herramienta gráfica que facilita la administración de SQL Server. En las secciones siguientes se proporcionan algunos ejemplos del uso de la interfaz gráfica de usuario.

## <a name="create-and-manage-databases"></a>Crear y administrar bases de datos

Mientras está conectado a la base de datos *maestra*, puede crear bases de datos en el servidor y modificar o quitar bases de datos existentes. En los pasos siguientes se explica cómo realizar varias tareas comunes de administración de bases de datos mediante Management Studio. Para realizar estas tareas, asegúrese de que está conectado a la base de datos *maestra* con el inicio de sesión de entidad de seguridad a nivel de servidor creado al configurar SQL Server en Linux.

### <a name="create-a-new-database"></a>Creación de una base de datos

1. Inicie SSMS y conéctese al servidor en SQL Server en Linux.

2. En el Explorador de objetos, haga clic con el botón derecho en la carpeta *Bases de datos* y, luego, haga clic en *Nueva base de datos...".

3. En el cuadro de diálogo *Nueva base de datos*, escriba un nombre para la nueva base de datos y haga clic en *Aceptar*.

La nueva base de datos se crea correctamente en el servidor. Si prefiere crear una nueva base de datos mediante T-SQL, vea [CREATE DATABASE (SQL Server Transact-SQL)](../t-sql/statements/create-database-transact-sql.md).

### <a name="drop-a-database"></a>Quitar una base de datos

1. Inicie SSMS y conéctese al servidor en SQL Server en Linux.

2. En el Explorador de objetos, expanda la carpeta *Bases de datos* para ver una lista de todas las bases de datos del servidor.

3. En el Explorador de objetos, haga clic con el botón derecho en la base de datos que quiere quitar y, luego, haga clic en *Eliminar*.

4. En el cuadro de diálogo *Eliminar objeto*, active *Cerrar conexiones existentes* y haga clic en *Aceptar*.

La base de datos se quita correctamente del servidor. Si prefiere quitar una base de datos mediante T-SQL, vea [DROP DATABASE (SQL Server Transact-SQL)](../t-sql/statements/drop-database-transact-sql.md).

## <a name="use-activity-monitor-to-see-information-about-sql-server-activity"></a>Uso de Monitor de actividad para ver información sobre la actividad de SQL Server

La herramienta [Monitor de actividad](../relational-databases/performance-monitor/activity-monitor.md) está integrada en SQL Server Management Studio (SSMS) y muestra información sobre los procesos de SQL Server y cómo afectan a la instancia actual de SQL Server.

1. Inicie SSMS y conéctese al servidor en SQL Server en Linux.

1. En el Explorador de objetos, haga clic con el botón derecho en el nodo *servidor* y, luego, haga clic en *Monitor de actividad*.

Monitor de actividad muestra paneles que se pueden expandir y contraer con la siguiente información:

- Información general
- Procesos
- Esperas de recursos
- E/S de archivo de datos
- Consultas costosas recientes
- Consultas costosas activas

Cuando un panel está expandido, Monitor de actividad consulta información a la instancia. Si el panel está contraído, cualquier actividad de consulta se detiene en ese panel. Se pueden expandir uno o varios paneles al mismo tiempo para ver diferentes tipos de actividad en la instancia.

## <a name="see-also"></a>Consulte también
- [¿Qué es SSMS?](../ssms/sql-server-management-studio-ssms.md)
- [Exportación e importación de una base de datos con SSMS](sql-server-linux-migrate-ssms.md)
- [Tutorial: SQL Server Management Studio](../ssms/quickstarts/ssms-connect-query-sql-server.md)
- [Tutorial: Escribir instrucciones Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md)
- [Supervisión de la actividad y rendimiento del servidor](../relational-databases/performance/server-performance-and-activity-monitoring.md)