---
title: Permisos necesarios
description: Obtenga información sobre los permisos que necesita para varias tareas de SQL Server Data Tools, como la creación de una base de datos, la ejecución de pruebas unitarias y la comparación de esquemas.
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: b27038c4-94ab-449c-90b7-29d87ce37a8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.openlocfilehash: 6e519a3786b45d87344f8f5f5355353fd5865d62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072459"
---
# <a name="required-permissions-for-sql-server-data-tools"></a>Permisos necesarios para SQL Server Data Tools

Para poder realizar una acción en una base de datos en Visual Studio, debe iniciar sesión con una cuenta que tenga determinados permisos para esa base de datos. Los permisos específicos que necesita varían en función de la acción que desee realizar. En las siguientes secciones se describe cada una de las acciones que puede querer realizar y el permiso específico que necesita para realizarla.  
  
-   [Permisos para crear o implementar una base de datos](#DatabaseCreationAndDeploymentPermissions)  
  
-   [Permisos para refactorizar una base de datos](#DatabaseRefactoringPermissions)  
  
-   [Permisos para ejecutar pruebas unitarias en una base de datos de SQL Server](#DatabaseUnitTestingPermissions)  
  
-   [Permisos para generar datos](#DataGenerationPermissions)  
  
-   [Permisos para comparar esquemas y datos](#SchemaAndDataComparePermissions)  
  
-   [Permisos para ejecutar el editor de Transact-SQL](#Transact-SQLEditorPermissions)  
  
-   [Permisos para proyectos de Common Language Run-time de SQL Server (CLR de SQL)](#SQLCLRPermissions)  
  
## <a name="permissions-to-create-or-deploy-a-database"></a><a name="DatabaseCreationAndDeploymentPermissions"></a>Permisos para crear o implementar una base de datos  
Para crear o implementar una base de datos, debe disponer de los siguientes permisos.  
  
|Acciones|Permisos necesarios|  
|-|-|  
|Importar objetos y configuración de base de datos|Debe poder conectarse a la base de datos de origen.<br /><br />Si la base de datos de origen se basa en SQL Server 2005, también debe ser el propietario o tener el permiso **VIEW DEFINITION** en cada objeto.<br /><br />Si la base de datos de origen se basa en SQL Server 2008 o una versión posterior, también debe ser el propietario o tener el permiso **VIEW DEFINITION** en cada objeto. Su inicio de sesión debe tener el permiso **VIEW SERVER STATE** (para claves de cifrado de base de datos).|  
|Importar objetos y configuración de servidor|Debe poder conectarse a la base de datos maestra en el servidor especificado.<br /><br />Si el servidor ejecuta SQL Server 2005, debe tener el permiso **VIEW ANY DEFINITION** en el servidor.<br /><br />Si la base de datos de origen se basa en SQL Server 2008 o una versión posterior, también debe ser el propietario o tener el permiso **VIEW ANY DEFINITION** en el servidor. Su inicio de sesión debe tener el permiso **VIEW SERVER STATE** (para claves de cifrado de base de datos).|  
|Crear o actualizar un proyecto de base de datos|No se necesita ningún permiso de base de datos para crear o modificar un proyecto de base de datos.|  
|Implementación de una nueva base de datos o implementación con la opción **Volver a crear siempre la base de datos** establecida|Debe tener el permiso **CREATE DATABASE** o ser un miembro del rol **dbcreator** en el servidor de destino.<br /><br />Cuando crea una base de datos, Visual Studio se conecta a la base de datos modelo y copia su contenido. El inicio de sesión inicial (por ejemplo, *yourLogin*) que utiliza para conectarse a la base de datos de destino debe tener permisos **db_creator** y **CONNECT SQL**. Este inicio de sesión debe contar con una asignación de usuario en la base de datos modelo. Si tiene permisos **sysadmin**, puede crear la asignación ejecutando las siguientes instrucciones Transact\-SQL:<br /><br />`USE [model] CREATE USER yourUser FROM LOGIN yourLogin`<br /><br />El usuario (en este ejemplo, yourUser) debe tener los permisos **CONNECT** y **VIEW DEFINITION** en la base de datos de modelo. Si tiene permisos **sysadmin**, puede conceder estos permisos si emite las siguientes instrucciones Transact\-SQL:<br /><br />`USE [model] GRANT CONNECT to yourUser GRANT VIEW DEFINITION TO yourUser`<br /><br />Si implementa una base de datos que contiene restricciones sin nombre y la opción **CheckNewContraints** está habilitada (lo está de manera predeterminada), debe tener el permiso **db_owner** o **sysadmin** o se producirá un error en la implementación. Esto solo ocurre con las restricciones sin nombre. Para obtener más información sobre la opción **CheckNewConstraints**, consulte [Configuración del proyecto de base de datos](../ssdt/database-project-settings.md).|  
|Implementar actualizaciones en una base de datos existente|Debe ser un usuario válido en la base de datos. Debe ser también miembro del rol **db_ddladmin**, ser el propietario del esquema o ser el propietario de los objetos que desea crear o modificar en la base de datos de destino. Necesita otros permisos para trabajar con conceptos más avanzados como inicios de sesión o servidores vinculados en sus scripts anteriores o posteriores a la implementación.<br /><br />**NOTA:** Si realiza la implementación en la base de datos maestra, también debe tener el permiso **VIEW ANY DEFINITION** en el servidor en el que va a realizar la implementación.|  
|Usar un ensamblado con la opción EXTERNAL_ACCESS en un proyecto de base de datos|Debe establecer la propiedad TRUSTWORTHY para el proyecto de base de datos. Debe tener el permiso EXTERNAL ACCESS ASSEMBLY para el inicio de sesión en SQL Server.|  
|Implementar ensamblados en una base de datos nueva o existente|Debe ser miembro del rol sysadmin en el servidor de implementación de destino.|  
  
Para obtener más información, vea Libros en pantalla de SQL Server.  
  
## <a name="permissions-to-refactor-a-database"></a><a name="DatabaseRefactoringPermissions"></a>Permisos para refactorizar una base de datos  
La *refactorización de una base de datos* solo tiene lugar dentro del proyecto de base de datos. Debe tener permisos para usar el proyecto de base de datos. No necesita permisos en una tabla de destino hasta que implemente los cambios en ella.  
  
## <a name="permissions-to-perform-unit-testing-on-a-sql-server-database"></a><a name="DatabaseUnitTestingPermissions"></a>Permisos para ejecutar pruebas unitarias en una base de datos de SQL Server  
Para ejecutar pruebas unitarias en una base de datos, debe disponer de los siguientes permisos.  
  
|Acciones|Permisos necesarios|  
|-|-|   
|Ejecutar una acción de prueba|Debe usar la conexión de base de datos del contexto de ejecución. Para obtener más información, consulte [Información general acerca de las cadenas de conexión y los permisos](../ssdt/overview-of-connection-strings-and-permissions.md).|  
|Ejecutar una acción anterior o posterior a la prueba|Debe usar la conexión de base de datos del contexto con privilegios. Esta conexión de base de datos puede tener más permisos que la conexión del contexto de ejecución.|  
|Ejecutar los scripts TestInitialize y TestCleanup|Debe usar la conexión de base de datos del contexto con privilegios.|  
|Implementar cambios en una base de datos antes de ejecutar las pruebas|Debe usar la conexión de base de datos del contexto con privilegios. Para más información, vea: [Cómo: Configurar una ejecución de prueba unitaria de SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).|  
|Generar datos antes de ejecutar las pruebas|Debe usar la conexión de base de datos del contexto con privilegios. Para más información, vea: [Cómo: Configurar una ejecución de prueba unitaria de SQL Server](../ssdt/how-to-configure-sql-server-unit-test-execution.md).|  
  
## <a name="permissions-to-generate-data"></a><a name="DataGenerationPermissions"></a>Permisos para generar datos  
Debe tener los permisos **INSERT** y **SELECT** en los objetos de la base de datos de destino para generar datos de prueba con el Generador de datos. Si depura los datos antes de generarlos, debe tener también el permiso **DELETE** en los objetos de la base de datos de destino. Para restablecer la columna **IDENTITY** en una tabla, debe ser el propietario de la tabla o un miembro del rol db_owner o db_ddladmin.  
  
## <a name="permissions-to-compare-schemas-and-data"></a><a name="SchemaAndDataComparePermissions"></a>Permisos para comparar esquemas y datos  
Para comparar esquemas o datos, debe disponer de los siguientes permisos.  
  
|Acciones|Permisos necesarios|  
|-|-|   
|Comparar los esquemas de dos bases de datos|Debe tener los permisos para importar objetos y configuraciones de las bases de datos que se describen en [Permisos para crear o implementar una base de datos](#DatabaseCreationAndDeploymentPermissions).|  
|Comparar los esquemas de una base de datos y un proyecto de base de datos|Debe tener los permisos para importar objetos y configuraciones de la base de datos que se describen en [Permisos para crear o implementar una base de datos](#DatabaseCreationAndDeploymentPermissions). Además, el proyecto de base de datos debe estar abierto en Visual Studio.|  
|Escribir actualizaciones en la base de datos de destino|Debe tener los permisos para implementar actualizaciones en la base de datos de destino que se describen en [Permisos para crear o implementar una base de datos](#DatabaseCreationAndDeploymentPermissions).|  
|Comparar los datos de dos bases de datos|Además de los permisos necesarios para comparar los esquemas de dos bases de datos, necesita también el permiso **SELECT** en todas las tablas que desee comparar y el permiso **VIEW DATABASE STATE**.|  
  
Para obtener más información, vea Libros en pantalla de SQL Server.  
  
## <a name="permissions-to-run-the-transact-sql-editor"></a><a name="Transact-SQLEditorPermissions"></a>Permisos para ejecutar el editor de Transact\-SQL  
Las acciones que puede realizar con el editor de Transact\-SQL vienen determinadas por el contexto de ejecución de la base de datos de destino.  
  
## <a name="permissions-for-sql-server-common-language-run-time-projects"></a><a name="SQLCLRPermissions"></a>Permisos para proyectos de Common Language Run-time de SQL Server  
En la tabla siguiente se muestran los permisos que debe tener para implementar o depurar proyectos de CLR:  
  
|Acciones|Permisos necesarios|  
|-----------|------------------------|  
|Implementación (inicial o incremental) de un ensamblado con un conjunto de permisos seguros|db_DDLAdmin: este permiso otorga los permisos CREATE y ALTER para los ensamblados y tipos de objetos que va a implementar<br /><br />VIEW DEFINITION en el nivel de base de datos: necesario para la implementación<br /><br />CONNECT en el nivel de base de datos: permite conectarse a la base de datos|  
|Implementar un ensamblado con el conjunto de permisos external_access|db_DDLAdmin: este permiso otorga los permisos CREATE y ALTER para los ensamblados y tipos de objetos que va a implementar<br /><br />VIEW DEFINITION en el nivel de base de datos: necesario para la implementación<br /><br />CONNECT en el nivel de base de datos: permite conectarse a la base de datos<br /><br />Además, debe tener:<br /><br />La opción de base de datos TRUSTWORTHY establecida en ON<br /><br />El inicio de sesión que usa para la implementación debe tener el permiso de servidor External Access Assembly.|  
|Implementar un ensamblado con un conjunto de permisos no seguros|db_DDLAdmin: este permiso otorga los permisos CREATE y ALTER para los ensamblados y tipos de objetos que va a implementar<br /><br />VIEW DEFINITION en el nivel de base de datos: necesario para la implementación<br /><br />CONNECT en el nivel de base de datos: permite conectarse a la base de datos<br /><br />Además, debe tener:<br /><br />La opción de base de datos TRUSTWORTHY establecida en ON<br /><br />El inicio de sesión que usa para la implementación debe tener el permiso de servidor Unsafe Assembly.|  
|Depuración remota de un ensamblado CLR de SQL|Debe tener el permiso de rol fijo sysadmin.|  
  
> [!IMPORTANT]  
> En todos los casos, el propietario del ensamblado debe ser el usuario que va a implementar el ensamblado o el propietario debe tener un rol del que el usuario sea miembro. Este requisito se aplica también a todos los ensamblados a los que haga referencia el ensamblado que va a implementar.  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[SQL Server Data Tools](../ssdt/sql-server-data-tools.md)  
  
