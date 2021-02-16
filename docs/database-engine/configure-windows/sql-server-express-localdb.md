---
title: SQL Server Express LocalDB | Microsoft Docs
description: Familiarícese con SQL Server Express LocalDB. Los desarrolladores pueden usar este Motor de base de datos ligero para escribir y probar el código de Transact-SQL.
ms.custom: ''
ms.date: 04/17/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- user instances
- LocalDB, described
- local database runtime
- file database
- LocalDB
ms.assetid: 5a641a46-7cfb-4d7b-a90d-6e4625719d74
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b7f57127bd8d1bd6b289ba82291ba9c1dd87424
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340432"
---
# <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Microsoft SQL Server Express LocalDB es una característica de [SQL Server Express](../../sql-server/editions-and-components-of-sql-server-version-15.md) dirigida a los desarrolladores. Está disponible en SQL Server Express con Advanced Services.

La instalación de LocalDB copia un conjunto de archivos mínimo necesario para iniciar el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Una vez que LocalDB está instalado, puede iniciar una conexión mediante una cadena de conexión especial. Cuando se realiza la conexión, se crea y se inicia automáticamente la infraestructura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] necesaria, permitiendo que la aplicación use la base de datos sin tareas de configuración complejas. Las herramientas de desarrollo pueden proporcionar a los desarrolladores de software un [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] que les permite escribir y probar el código de [!INCLUDE[tsql](../../includes/tsql-md.md)] sin tener que administrar una instancia de servidor completa de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

## <a name="installation-media"></a>Medios de instalación 

LocalDB es una característica que se selecciona durante la instalación de SQL Server Express y está disponible al descargar los medios. Si descarga los medios, elija **Express Advanced** o el paquete de LocalDB. 

- [SQL Server Express 2019](https://go.microsoft.com/fwlink/?LinkID=866658)
- [SQL Server Express 2017](https://go.microsoft.com/fwlink/?LinkID=853017)
- [SQL Server Express 2016](https://go.microsoft.com/fwlink/?LinkID=799012)

Como alternativa, puede instalar LocalDB a través del [Instalador de Visual Studio](https://visualstudio.microsoft.com/downloads/), como parte de la carga de trabajo de **almacenamiento y procesamiento de datos**, la carga de trabajo de **ASP.NET y desarrollo web** o como un componente individual.


## <a name="install-localdb"></a>Instalar LocalDB

Instale LocalDB mediante el asistente para la instalación o el programa SqlLocalDB.msi. LocalDB es una opción al instalar LocalDB de SQL Server Express. 
 
Seleccione LocalDB en la página **Selección de características/Características compartidas** durante la instalación. Solo puede haber una instalación de los archivos binarios de LocalDB para cada versión principal de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Se pueden iniciar varios procesos de [!INCLUDE[ssDE](../../includes/ssde-md.md)] y todos usarán los mismos binarios. Una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] iniciada como LocalDB tiene las mismas limitaciones que [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].

Una instancia de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] LocalDB se administra con la utilidad `SqlLocalDB.exe`. Se debe usar [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] LocalDB en lugar de la característica de instancia de usuario de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], que ha quedado en desuso.

## <a name="description"></a>Descripción

El programa de instalación de LocalDB usa el programa `SqlLocalDB.msi` para instalar los archivos necesarios en el equipo. Una vez instalado, LocalDB es una instancia de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] que puede crear y abrir bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los archivos de base de datos del sistema de la base de datos se almacenan en la ruta de acceso de AppData local, que suele estar oculta. Por ejemplo, `C:\Users\<user>\AppData\Local\Microsoft\Microsoft SQL Server Local DB\Instances\LocalDBApp1\`. Los archivos de la base de datos de usuarios se almacenan donde designa el usuario, normalmente en alguna parte de la carpeta `C:\Users\<user>\Documents\`.

Para más información sobre cómo incluir LocalDB en una aplicación, vea [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] [Introducción a los datos locales](/previous-versions/visualstudio/visual-studio-2012/ms233817(v=vs.110)), [Crear una base de datos y agregar las tablas en Visual Studio](/visualstudio/data-tools/create-a-sql-database-by-using-a-designer).

Para más información sobre la API LocalDB, consulte [Referencia de SQL Server Express LocalDB](../../relational-databases/sql-server-express-localdb-reference.md).

La utilidad `SqlLocalDb` puede crear instancias de LocalDB e iniciar y detener una instancia de LocalDB; incluye opciones para ayudarle a administrar LocalDB. Para más información sobre la utilidad `SqlLocalDb`, consulte [Utilidad SqlLocalDB](../../tools/sqllocaldb-utility.md).

La intercalación de instancias de LocalDB está establecida en `SQL_Latin1_General_CP1_CI_AS` y no se puede cambiar. Normalmente se admiten las intercalaciones de nivel de base de datos, nivel de columna y nivel de expresión. Las bases de datos independientes siguen las reglas de metadatos e intercalaciones de `tempdb` definidas por [Intercalaciones de bases de datos independientes](../../relational-databases/databases/contained-database-collations.md).

### <a name="restrictions"></a>Restricciones

- No se pueden aplicar otras revisiones a LocalDB que no sean los Service Packs. No se pueden aplicar y no se aplicarán actualizaciones de seguridad y acumulativas a través de Windows Update, Windows Update para empresas u otros métodos.

- LocalDB no se puede administrar de forma remota a través de SQL Management Studio.

- LocalDB no puede ser un suscriptor de replicación de mezcla.

- LocalDB no admite FILESTREAM.

- LocalDB solo permite colas locales de Service Broker.

- Una instancia de LocalDB propiedad de las cuentas integradas, como `NT AUTHORITY\SYSTEM`, puede presentar problemas de capacidad de administración debido a la redirección del sistema de archivos de Windows. Use en su lugar una cuenta de Windows normal como propietario.

### <a name="automatic-and-named-instances"></a>Instancias automáticas y con nombre

LocalDB admite dos tipos de instancias: instancias automáticas e instancias con nombre.

- Las instancias automáticas de LocalDB son públicas. Se crean y se administran automáticamente para el usuario y se pueden utilizar en cualquier aplicación. Hay una instancia automática de LocalDB en cada versión de LocalDB instalada en el equipo del usuario. Las instancias automáticas de LocalDB proporcionan una administración de instancias agilizada. No hay ninguna necesidad de crear la instancia; solo funciona. Esta característica permite la instalación y la migración fáciles de las aplicaciones en un equipo diferente. Si la máquina de destino tiene la versión especificada de LocalDB instalada, la instancia automática de LocalDB de esa versión también estará disponible en la máquina de destino. Las instancias automáticas de LocalDB cuentan con un patrón especial para el nombre de instancia que pertenece a un espacio de nombres reservado. Las instancias automáticas impiden conflictos de nombres con las instancias con nombre de LocalDB. El nombre de la instancia automática es **MSSQLLocalDB**.

- Las instancias con nombre de LocalDB son privadas. Son propiedad de una sola aplicación que es la responsable de la creación y administración de la instancia. Las instancias con nombre proporcionan el aislamiento de otras instancias y pueden mejorar el rendimiento reduciendo la contención de recursos con otros usuarios de bases de datos. El usuario debe crear explícitamente las instancias con nombre mediante la API de administración de LocalDB o implícitamente a través del archivo app.config de una aplicación administrada (aunque la aplicación administrada también puede usar la API, si así lo quiere). Cada instancia con nombre de LocalDB tiene una versión de LocalDB asociada que señala al conjunto respectivo de binarios de LocalDB. El nombre de instancia de LocalDB es de tipo de datos **sysname** y puede tener hasta 128 caracteres. (Esta característica es diferente para las instancias con nombre normales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en que los nombres están limitados a nombres de NetBIOS normales de 16 caracteres ASCII). El nombre de una instancia de LocalDB puede contener cualquier carácter Unicode que sea válido dentro de un nombre de archivo. Una instancia con nombre que usa un nombre de instancia automática se convierte en una instancia automática.

Usuarios diferentes de un equipo pueden tener instancias con el mismo nombre. Cada instancia es un proceso diferente que se ejecuta como un usuario diferente.

## <a name="shared-instances-of-localdb"></a>Instancias compartidas de LocalDB

Para admitir escenarios donde varios usuarios del equipo deben conectarse a una instancia de LocalDB, LocalDB admite el uso compartido de instancias. Un propietario de una instancia puede decidir permitir que otros usuarios del equipo se conecten a la instancia. Las instancias tanto automáticas como con nombre de LocalDB se pueden compartir. Para compartir una instancia de LocalDB, un usuario selecciona un nombre compartido (alias) para ella. Dado que el nombre compartido está visible para todos los usuarios del equipo, este nombre compartido debe ser único en el equipo. El nombre compartido de una instancia de LocalDB tiene el mismo formato que la instancia con nombre de LocalDB.

Solo un administrador del equipo puede crear una instancia compartida de LocalDB. Una instancia compartida de LocalDB puede dejar de ser compartida por un administrador o el propietario de la instancia compartida de LocalDB. Para compartir y dejar de compartir una instancia de LocalDB, use los métodos `LocalDBShareInstance` y `LocalDBUnShareInstance` de la API de LocalDB o las opciones para compartir y dejar de compartir de la utilidad `SqlLocalDb`.

## <a name="start-localdb-and-connect-to-localdb"></a>Inicio de LocalDB y conexión a LocalDB

### <a name="connect-to-the-automatic-instance"></a>Conexión a la instancia automática

La manera más fácil de usar LocalDB es conectarse a la instancia automática propiedad del usuario actual mediante la cadena de conexión `Server=(localdb)\MSSQLLocalDB;Integrated Security=true`. Para conectarse a una base de datos específica usando el nombre de archivo, conéctese con una cadena de conexión similar a `Server=(LocalDB)\MSSQLLocalDB;Integrated Security=true;AttachDbFileName=D:\Data\MyDB1.mdf`.

La convención de nomenclatura y la cadena de conexión para el formato de LocalDB cambió en SQL Server 2014. Anteriormente, el nombre de instancia era un solo carácter v seguido de LocalDB y el número de versión. A partir de SQL Server 2014, este formato de nombre de instancia ya no se admite y se debe usar en su lugar la cadena de conexión mencionada anteriormente.  

>[!NOTE]
> - La primera vez que un usuario de un equipo intenta conectarse a LocalDB, se debe crear e iniciar la instancia automática. El tiempo adicional para la creación de la instancia puede hacer que el intento de conexión deje de funcionar con un mensaje de fin de tiempo de espera. Cuando sucede esto, espere unos segundos para que el proceso de creación se complete y, a continuación, conéctese de nuevo.


### <a name="create-and-connect-to-a-named-instance"></a>Creación de una instancia con nombre y conexión a ella

Además de la instancia automática, LocalDB también admite instancias con nombre. Use el programa SqlLocalDB.exe para crear, iniciar y detener una instancia con nombre de LocalDB. Para obtener más información sobre SqlLocalDB.exe, vea [SqlLocalDB (utilidad)](../../tools/sqllocaldb-utility.md).

```console
REM Create an instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" create LocalDBApp1
REM Start the instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" start LocalDBApp1
REM Gather information about the instance of LocalDB
"C:\Program Files\Microsoft SQL Server\130\Tools\Binn\SqlLocalDB.exe" info LocalDBApp1
```

 La última línea anterior devuelve información similar a la siguiente.

|Category|Value|
|-|-|
|Nombre|`LocalDBApp1`|
|Versión|\<Current Version>|
|Nombre compartido|""|
|Propietario|"\<Your Windows User>"|
|Creación automática|No|
|State|en ejecución|
|Última hora de inicio|\<Date and Time>|
|Nombre de canalización de instancia|np:\\\\.\pipe\LOCALDB#F365A78E\tsql\query|

>[!NOTE]
>Si la aplicación usa una versión de .NET anterior a 4.0.2, debe conectarse directamente a la canalización con nombre de LocalDB. El valor de nombre de canalización de la instancia es la canalización con nombre en la que escucha la instancia de LocalDB. La parte de nombre de canalización de la instancia que sigue a LOCALDB# cambiará cada vez que se inicie la instancia de LocalDB. Para conectarse a la instancia de LocalDB mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], escriba el nombre de canalización de la instancia en el cuadro **Nombre de servidor** del cuadro de diálogo **Conectar a[!INCLUDE[ssDE](../../includes/ssde-md.md)]** . Desde su programa personalizado puede establecer la conexión a la instancia de LocalDB mediante una cadena de conexión similar a `SqlConnection conn = new SqlConnection(@"Server=np:\\.\pipe\LOCALDB#F365A78E\tsql\query");`.

### <a name="connect-to-a-shared-instance-of-localdb"></a>Conexión a una instancia compartida de LocalDB

Para conectarse a una instancia compartida de LocalDB, agregue `\.\` (barra diagonal inversa + punto + barra diagonal inversa) a la cadena de conexión para hacer referencia al espacio de nombres reservado para las instancias compartidas. Por ejemplo, para conectarse a una instancia compartida de LocalDB denominada `AppData`, use una cadena de conexión, como `(localdb)\.\AppData`, como parte de la cadena de conexión. Un usuario que se conecta a una instancia compartida de LocalDB de la que no es propietario debe tener un inicio de sesión con autenticación de Windows o autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="troubleshooting"></a>Solución de problemas

Para información sobre cómo solucionar problemas de LocalDB, consulte la [página que trata la solución de problemas de SQL Server 2012 Express LocalDB](https://social.technet.microsoft.com/wiki/contents/articles/4609.aspx).

## <a name="permissions"></a>Permisos

Una instancia de SQL Server Express LocalDB es una instancia creada por el usuario para utilizarla. Cualquier usuario del equipo puede crear una base de datos mediante una instancia de LocalDB, almacenar archivos en su perfil de usuario y ejecutar el proceso con sus credenciales. De forma predeterminada, el acceso a la instancia de LocalDB está limitado a su propietario. Los datos contenidos en LocalDB están protegidos por el acceso del sistema de archivos a los archivos de base de datos. Si los archivos de base de datos de usuario se almacenan en una ubicación compartida, cualquiera que tenga acceso al sistema de archivos en esa ubicación puede abrir la base de datos usando una instancia de LocalDB que sea de su propiedad. Si los archivos de base de datos están en una ubicación protegida, como la carpeta de datos de los usuarios, solo el usuario y los administradores con acceso a la carpeta pueden abrir la base de datos. Los archivos de LocalDB solo se pueden abrir con una instancia de LocalDB cada vez.

>[!NOTE]
>LocalDB siempre se ejecuta en el contexto de seguridad de los usuarios; es decir, LocalDB nunca se ejecuta con credenciales del grupo local Administradores. Esto significa que todos los archivos de base de datos usados por una instancia de LocalDB deben ser accesibles mediante la cuenta de Windows del usuario propietario, sin tener en cuenta la pertenencia al grupo local Administradores.

## <a name="see-also"></a>Consulte también

[SqlLocalDB (utilidad)](../../tools/sqllocaldb-utility.md)
