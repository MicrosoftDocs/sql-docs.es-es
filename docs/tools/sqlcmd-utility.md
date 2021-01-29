---
title: Utilidad sqlcmd
description: La utilidad sqlcmd permite escribir instrucciones Transact-SQL, procedimientos del sistema y archivos de script con diferentes modos, y usa ODBC para ejecutar lotes de Transact-SQL.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- sqlcmd commands
- ED command
- sqlcmd utility
- command prompt utilities [SQL Server], sqlcmd
- '!! command'
- stored procedures [SQL Server], command prompt
- system stored procedures [SQL Server], command prompt
- sqlcmd utility, about sqlcmd utility
- scripts [SQL Server], command prompt
- RESET command
- GO command
ms.assetid: e1728707-5215-4c04-8320-e36f161b834a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 09/11/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: b499c7236391c7286c4a267c433ad991a963dab5
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688934"
---
# <a name="sqlcmd-utility"></a>Utilidad sqlcmd

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> Para SQL Server 2014 y versiones anteriores, consulte [Utilidad SQLCMD](/previous-versions/sql/2014/tools/sqlcmd-utility?view=sql-server-2014&preserve-view=true).
>
> Para usar sqlcmd en Linux, consulte la página sobre [cómo instalar sqlcmd y bcp en Linux](../linux/sql-server-linux-setup-tools.md).

 La utilidad **sqlcmd** permite escribir instrucciones Transact-SQL, procedimientos del sistema y archivos de script a través de diversos modos disponibles:

- En el símbolo del sistema.
- En el modo SQLCMD del **Editor de consultas**.
- En un archivo de script de Windows.
- En un paso de trabajo del sistema operativo (cmd.exe) de un trabajo del Agente SQL Server.

Esta utilidad usa ODBC para ejecutar lotes de Transact-SQL.

## <a name="download-the-latest-version-of-sqlcmd-utility"></a>Descarga de la versión más reciente de la utilidad sqlcmd

**[![Descargar sqlcmd para x64](../ssdt/media/download.png) Descargar utilidades de la línea de comandos 15 de Microsoft para SQL Server (x64) (2,6 MB)](https://go.microsoft.com/fwlink/?linkid=2142258)**
<br>**[![Descargar sqlcmd para x86](../ssdt/media/download.png) Descargar utilidades de la línea de comandos 15 de Microsoft para SQL Server (x86) (2,3 MB)](https://go.microsoft.com/fwlink/?linkid=2142257)**

Las herramientas de la línea de comandos se encuentran en fase de disponibilidad general (GA), pero se lanzan con el paquete del instalador de [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)].

**Información de versión**

Número de versión: 15.0.2<br>
Número de compilación: 15.0.2000.5<br>
Fecha de lanzamiento: 11 de septiembre de 2020

La nueva versión de SQLCMD admite la autenticación de Azure AD, incluida la compatibilidad de Multi-Factor Authentication (MFA) con las características SQL Database, Azure Synapse Analytics y Always Encrypted.
La nueva versión de BCP admite la autenticación de Azure AD, incluida la compatibilidad de Multi-Factor Authentication (MFA) con SQL Database y Azure Synapse Analytics.

**Requisitos del sistema** Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008 - 2019.

Este componente requiere [Windows Installer 4.5](https://www.microsoft.com/download/details.aspx?id=8483) y [Microsoft ODBC Driver 17 para SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md).
 
Para comprobar la versión de SQLCMD, ejecute el comando `sqlcmd -?` y confirme que la versión de 15.0.2000.5 o superior está usándose.

> [!NOTE]
> Se necesita al menos la versión 13.1 para admitir la autenticación de Always Encrypted (`-g`) y Azure Active Directory (`-G`). (Puede tener varias versiones de sqlcmd.exe instaladas en el equipo. Asegúrese de que está utilizando la versión correcta. Para determinar el número de versión, ejecute `sqlcmd -?`.)

Puede probar la utilidad sqlcmd desde Azure Cloud Shell, ya que está preinstalada de forma predeterminada: [![Iniciar Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Iniciar Cloud Shell")](https://shell.azure.com)

Para ejecutar instrucciones sqlcmd en SSMS, seleccione Modo SQLCMD en la lista desplegable del menú de consulta de la navegación superior.  

> [!IMPORTANT]
> [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] (SSMS) usa [!INCLUDE[dnprdnshort_md](../includes/dnprdnshort-md.md)]SqlClient de para la ejecución en modo normal y SQLCMD en el **Editor de consultas**. Cuando **sqlcmd** se ejecuta desde la línea de comandos, **sqlcmd** usa el controlador ODBC. Dado que se pueden aplicar diferentes opciones predeterminadas, podría obtener un comportamiento diferente al ejecutar la misma consulta en el modo SQLCMD de [!INCLUDE[ssManStudioFull_md](../includes/ssmanstudiofull-md.md)] y en la utilidad **sqlcmd** .  
>

Actualmente, **sqlcmd** no requiere un espacio entre la opción de línea de comandos y el valor. Sin embargo, en versiones futuras, se puede requerir un espacio entre la opción de línea de comandos y el valor.  

 Otros temas: 

- [Iniciar la utilidad sqlcmd](../ssms/scripting/sqlcmd-start-the-utility.md)
- [Usar la utilidad sqlcmd](../ssms/scripting/sqlcmd-use-the-utility.md)
  
## <a name="syntax"></a>Sintaxis

```cmd
sqlcmd
   -a packet_size
   -A (dedicated administrator connection)
   -b (terminate batch job if there is an error)
   -c batch_terminator
   -C (trust the server certificate)
   -d db_name
   -D
   -e (echo input)
   -E (use trusted connection)
   -f codepage | i:codepage[,o:codepage] | o:codepage[,i:codepage]
   -g (enable column encryption)
   -G (use Azure Active Directory for authentication)
   -h rows_per_header
   -H workstation_name
   -i input_file
   -I (enable quoted identifiers)
   -j (Print raw error messages)
   -k[1 | 2] (remove or replace control characters)  
   -K application_intent  
   -l login_timeout  
   -L[c] (list servers, optional clean output)  
   -m error_level  
   -M multisubnet_failover  
   -N (encrypt connection)  
   -o output_file  
   -p[1] (print statistics, optional colon format)  
   -P password  
   -q "cmdline query"  
   -Q "cmdline query" (and exit)  
   -r[0 | 1] (msgs to stderr)  
   -R (use client regional settings)  
   -s col_separator  
   -S [protocol:]server[instance_name][,port]  
   -t query_timeout  
   -u (unicode output file)  
   -U login_id  
   -v var = "value"
   -V error_severity_level
   -w column_width
   -W (remove trailing spaces)
   -x (disable variable substitution)
   -X[1] (disable commands, startup script, environment variables, optional exit)
   -y variable_length_type_display_width
   -Y fixed_length_type_display_width
   -z new_password
   -Z new_password (and exit)
   -? (usage)
```
  
## <a name="command-line-options"></a>Opciones de línea de comandos

**Opciones relacionadas con el inicio de sesión**

**-A**  
Inicia sesión en SQL Server con una conexión de administrador dedicada (DAC). Este tipo de conexión se utiliza para solucionar problemas de un servidor. Esta conexión solo funciona con equipos servidores que admitan DAC. Si DAC no está disponible, **sqlcmd** genera un mensaje de error y, después, se cierra. Para obtener más información sobre DAC, vea [Conexión de diagnóstico para administradores de bases de datos](../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md). La opción -A no se admite con la opción -G. Al conectarse a SQL Database mediante -A, debe ser administrador de SQL Server. DAC no está disponible para administradores de Azure Active Directory.

**-C**  
Este modificador lo usa el cliente para configurarlo de forma que confíe implícitamente en el certificado de servidor sin validación. Esta opción es equivalente a la opción de ADO.NET `TRUSTSERVERCERTIFICATE = true`.  

**-d** _db_name_  
Emite una instrucción `USE` *db_name* al iniciar **sqlcmd**. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDDBNAME. Este parámetro especifica la base de datos inicial. El valor predeterminado es la propiedad de base de datos predeterminada del inicio de sesión. Si la base de datos no existe, se genera un mensaje de error y **sqlcmd** se cierra.  

**-D**  
Interpreta el nombre de servidor proporcionado a -S como un DSN en lugar de un nombre de host. Para más información, consulte la sección _Compatibilidad con DSN en sqlcmd y bcp_ de [Conexión con sqlcmd](../connect/odbc/linux-mac/connecting-with-sqlcmd.md).

> [!NOTE]
> La opción -D solo está disponible en clientes de Linux y MacOS. En clientes de Windows, antes hacía referencia a una opción que ahora es obsoleta, se ha quitado y se ha omitido.

**-l** _login_timeout_  
Especifica el número de segundos que tienen que transcurrir antes de que un inicio de sesión de **sqlcmd** en el controlador ODBC agote el tiempo de espera cuando se trate de conectar a un servidor. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDLOGINTIMEOUT. El tiempo de espera predeterminado de inicio de sesión de **sqlcmd** es de ocho segundos. Cuando se usa la opción **-G** para conectarse a Base de datos SQL o a Azure Synapse Analytics y autenticarse con Azure Active Directory, se recomienda establecer un valor de tiempo de espera de al menos 30 segundos. El período de tiempo de espera de inicio de sesión debe ser un número comprendido entre 0 y 65534. Si el valor proporcionado no es numérico o no está dentro de ese intervalo, **sqlcmd** generará un mensaje de error. El valor 0 especifica que el tiempo de espera es infinito.

**-E**  
Usa una conexión de confianza en lugar de usar un nombre de usuario y una contraseña para iniciar sesión en SQL Server. Si no se especifica **-E** , **sqlcmd** usa de forma predeterminada la opción de conexión de confianza.  

La opción **-E** omite la posible configuración de la variable de entorno de nombre de usuario y contraseña, como SQLCMDPASSWORD. Si se usa la opción **-E** junto con la opción **-U** o **-P** , se genera un mensaje de error.  

**-g**  
Establece el valor de cifrado de columnas en `Enabled`. Para obtener más información, vea [Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md). Solo se admiten claves maestras almacenadas en el almacén de certificados de Windows. El modificador -g requiere al menos **sqlcmd** versión [13.1](https://go.microsoft.com/fwlink/?LinkID=825643). Para determinar su versión, ejecute `sqlcmd -?`.

**-G**  
El cliente usa este modificador al conectarse a SQL Database o a Azure Synapse Analytics para especificar que el usuario se autentica mediante la autenticación de Azure Active Directory. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDUSEAAD en true. El modificador -G requiere al menos **sqlcmd** versión [13.1](https://go.microsoft.com/fwlink/?LinkID=825643). Para determinar su versión, ejecute `sqlcmd -?`. Para más información, vea [Conexión a SQL Database o a Azure Synapse Analytics mediante autenticación de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview). La opción -A no se admite con la opción -G.

> [!IMPORTANT]
> La opción `-G` solo se aplica a Azure SQL Database y Almacenamiento de datos de Azure.
> La autenticación interactiva de AAD no se admite actualmente ni en Linux ni en macOS. La autenticación integrada de AAD requiere la versión 17.6.1 o superior de [Microsoft ODBC Driver 17 for SQL  Server](../connect/odbc/download-odbc-driver-for-sql-server.md) y un entorno de Kerberos configurado correctamente.

- **Nombre de usuario y contraseña de Azure Active Directory:** 

    Si desea utilizar un nombre de usuario y una contraseña de Azure Active Directory, puede proporcionar la opción **- G** y usar también el nombre de usuario y la contraseña proporcionando las opciones **- U** y **-P** .

    ```cmd
    Sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -U bob@contoso.com -P MyAADPassword -G
    ```

    El parámetro -G genera la siguiente cadena de conexión en el back-end: 

    ```cmd
     SERVER = Target_DB_or_DW.testsrv.database.windows.net;UID= bob@contoso.com;PWD=MyAADPassword;AUTHENTICATION = ActiveDirectoryPassword 
    ```

- **Autenticación integrada de Azure Active Directory**

   Para autenticación integrada de Azure Active Directory, proporcione la opción **-G** sin un nombre de usuario o contraseña.
   *La autenticación integrada de AAD no se admite actualmente ni en Linux ni en macOS*.

    ```cmd
    Sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G
    ```

    Esto generará la siguiente cadena de conexión en el back-end: 

    ```cmd
    SERVER = Target_DB_or_DW.testsrv.database.windows.net Authentication = ActiveDirectoryIntegrated; Trusted_Connection=NO
    ```

    > [!NOTE]
    > La opción `-E` (Trusted_Connection) no se puede utilizar junto con la opción `-G`.

- **Autenticación interactiva de Azure Active Directory**

    La autenticación interactiva de Azure AD para Azure SQL Database y Azure Synapse Analytics permite usar un método interactivo que admite la autenticación multifactor. Para obtener más información, vea [Autenticación interactiva de Active Directory](../ssdt/azure-active-directory.md#active-directory-interactive-authentication). 

   La autenticación interactiva de Azure AD requiere **sqlcmd**, [versión 15.0.1000.34](#download-the-latest-version-of-sqlcmd-utility) o posterior, así como [ODBC, versión 17.2 o posterior](../connect/odbc/download-odbc-driver-for-sql-server.md).  

   Para habilitar la autenticación interactiva, indique la opción -G solo con el nombre de usuario (-U), sin contraseña.

   En el siguiente ejemplo se exportan datos mediante el modo interactivo de Azure AD que indica el nombre de usuario, donde el usuario representa una cuenta de AAD. Este es el mismo ejemplo que se utilizó en la sección anterior: *Nombre de usuario y contraseña de Azure Active Directory*.  

   El modo interactivo requiere que se especifique una contraseña manualmente, o en el caso de las cuentas que tengan la autenticación multifactor habilitada, complete el método de autenticación MFA configurado.

   ```cmd
   sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -G -U alice@aadtest.onmicrosoft.com
   ```

   El comando anterior genera la siguiente cadena de conexión en el back-end:  

   ```cmd
   SERVER = Target_DB_or_DW.testsrv.database.windows.net;UID=alice@aadtest.onmicrosoft.com; AUTHENTICATION = ActiveDirectoryInteractive   
   ```

   En caso de que un usuario de Azure AD sea un usuario federado de dominio con una cuenta de Windows, el nombre de usuario que se requiera en la línea de comandos contendrá su cuenta de dominio (por ejemplo, joe@contoso.com; véase a continuación):

   ```cmd
   sqlcmd -S testsrv.database.windows.net -d Target_DB_or_DW -G -U joe@contoso.com  
   ```

   Si los usuarios invitados ya se encuentran en un servicio Azure AD específico y forman parte de un grupo que existe en la base de datos de SQL que tiene permisos de base de datos para ejecutar el comando sqlcmd, se usa su alias de usuario invitado (por ejemplo, *keith0@adventureworks.com* ).

  >[!IMPORTANT]
  >Existe un problema conocido cuando se usa la opción `-G` y `-U` con SQLCMD, en el que, si se coloca la opción `-U` antes de la opción `-G`, se puede provocar un error de autenticación. Comience siempre con la opción `-G`, seguida de la opción `-U`.

**-H** _workstation_name_  
 Un nombre de estación de trabajo. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDWORKSTATION. El nombre de la estación de trabajo se muestra en la columna **hostname** de la vista de catálogo **sys.sysprocesses** y se puede devolver mediante el procedimiento almacenado **sp_who**. Si no se especifica esta opción, el nombre actual del equipo es el valor predeterminado. Este nombre se puede usar para otras 

sesiones de **sqlcmd**.  

**-j** Imprime mensajes de error sin formato en la pantalla.
  
 **-K** _application_intent_  
 Declara el tipo de carga de trabajo de la aplicación al conectarse a un servidor. El único valor actualmente admitido es **de solo lectura**. Si no se especifica **-K** , la utilidad sqlcmd no admitirá la conectividad con una réplica secundaria en el grupo de disponibilidad AlwaysOn. Para más información, consulte [Secundarias activas: réplicas secundarias legibles (grupos de disponibilidad de AlwaysOn)](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)  
  
**-M** _multisubnet_failover_  
 Especifique siempre **-M** al conectarse a una escucha de un grupo de disponibilidad de SQL Server o a una instancia de clúster de conmutación por error de SQL Server. **-M** proporciona una detección más rápida del servidor activo actualmente y de la conexión a este. Si no se especifica **-M**, **-M** se desactiva. Para más información, consulte [Agentes de escucha, conectividad de cliente y conmutación por error de una aplicación](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md), [Creación y configuración de grupos de disponibilidad &#40;SQL Server&#41;](../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md), [Clústeres de conmutación por error y grupos de disponibilidad AlwaysOn (SQL Server)](../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md) y [Secundarias activas: réplicas secundarias legibles (grupos de disponibilidad AlwaysOn)](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).
  
 **-N**  
 Este modificador lo usa el cliente para solicitar una conexión cifrada.  
  
 **-P** _password_  
 Es una contraseña especificada por el usuario. En las contraseñas se distingue entre mayúsculas y minúsculas. Si se usa la opción -U y no la opción **-P** y, además, no se ha establecido la variable de entorno SQLCMDPASSWORD, **sqlcmd** solicita al usuario una contraseña. No se recomienda usar la contraseña nula, pero puede especificarla usando un par de comillas dobles contiguas en el valor del parámetro:

- **-P ""**

No. Sin embargo, se recomienda el uso de una contraseña segura.

#### <a name="use-a-strong-password"></a>[**Utilice una contraseña segura.**](../relational-databases/security/strong-passwords.md)

 El mensaje de contraseña se muestra en la consola de la siguiente manera: `Password:`  
  
 La entrada del usuario queda oculta. Esto significa que no se muestra nada y que el cursor permanece en su posición.  
  
 La variable de entorno SQLCMDPASSWORD permite establecer una contraseña predeterminada para la sesión actual. Por lo tanto, las contraseñas no tienen que estar codificadas de forma rígida en los archivos por lotes.  
  
 En el siguiente ejemplo primero se establece la variable SQLCMDPASSWORD en el símbolo del sistema y, después, se obtiene acceso a la utilidad **sqlcmd** . En el símbolo del sistema, escriba:  
  
 `SET SQLCMDPASSWORD= p@a$$w0rd`  
 En el siguiente símbolo del sistema, escriba:  
  
 `sqlcmd`  
  
 Si la combinación de nombre de usuario y contraseña no es correcta, se genera un mensaje de error.  
  
**NOTA:**  La variable de entorno OSQLPASSWORD se conservó por motivos de compatibilidad. La variable de entorno SQLCMDPASSWORD tiene prioridad sobre la variable de entorno OSQLPASSWORD. Como OSQLPASSWORD ya no se puede compartir más, las utilidades **sqlcmd** y **osql** se pueden usar la una con la otra sin interferencias. Los scripts antiguos seguirán funcionando.  
  
 Si se usa la opción **-P** junto con la opción **-E** , se genera un mensaje de error.  
  
 Si la opción **-P** va seguida de más de un argumento, se genera un mensaje de error y el programa se cierra.  
  
 **-S** [*protocolo*:]*servidor*[ **\\** _nombre\_instancia_][ **,** _puerto_]  
 Especifica la instancia de SQL Server a la que hay que conectarse. Establece la variable de scripting de **sqlcmd** SQLCMDSERVER.  
  
 Especifique *nombre_de_servidor* para conectar con la instancia predeterminada de SQL Server en ese equipo servidor. Especifique *nombre_de_servidor* [ **\\** _nombre\_instancia_ ] para conectar con una instancia con nombre de SQL Server en ese equipo servidor. Si no se especifica ningún equipo servidor, **sqlcmd** se conecta a la instancia predeterminada de SQL Server en el equipo local. Esta opción es necesaria si **sqlcmd** se ejecuta desde un equipo remoto conectado a la red.  
  
 *protocolo* puede ser **tcp** (TCP/IP), **lpc** (memoria compartida) o **np** (canalizaciones con nombre).  
  
 Si no especifica *nombre_de_servidor* [ **\\** _nombre\_instancia_ ] al iniciar **sqlcmd**, SQL Server comprueba si existe la variable de entorno SQLCMDSERVER y la usa.  
  
> [!NOTE]  
>  La variable de entorno OSQLSERVER se ha conservado por motivos de compatibilidad. La variable de entorno SQLCMDSERVER tiene prioridad sobre la variable de entorno OSQLSERVER; esto significa que **sqlcmd** y **osql** se pueden usar una junto a la otra sin interferencias y que los scripts anteriores seguirán funcionando.  
  
 **-U** _login_id_  
 Es el nombre de inicio de sesión o el nombre de usuario de base de datos independiente. Debe proporcionar la opción de nombre de base de datos para los usuarios de la base de datos independiente (-d).  
  
> [!NOTE]  
>  La variable de entorno OSQLUSER está disponible por motivos de compatibilidad con versiones anteriores. La variable de entorno SQLCMDUSER tiene prioridad sobre OSQLUSER. Esto significa que **sqlcmd** y **osql** se pueden usar una junto a la otra sin interferencias. También significa que los scripts de **osql** existentes seguirán funcionando.  
  
 Si no se especifica la opción **-U** o la opción **-P**, **sqlcmd** intenta conectarse mediante el modo de autenticación de Microsoft Windows. La autenticación se basa en la cuenta de Windows del usuario que está ejecutando **sqlcmd**.  
  
 Si se usa la opción **-U** junto con la opción **-E** (descrita más adelante en este artículo), se genera un mensaje de error. Si la opción **-U** va seguida de más de un argumento, se genera un mensaje de error y el programa se cierra.  
  
 **-z** _new_password_  
 Cambiar contraseña:  
  
 `sqlcmd -U someuser -P s0mep@ssword -z a_new_p@a$$w0rd`  
  
 **-Z** _new_password_  
 Cambiar contraseña y salir:  
  
 `sqlcmd -U someuser -P s0mep@ssword -Z a_new_p@a$$w0rd`  
  
 **Opciones de entrada o salida**  
  **-f** _codepage_ | **i:** _codepage_[ **,o:** _codepage_] | **o:** _codepage_[ **,i:** _codepage_]  
 Especifica las páginas de códigos de entrada y de salida. El número de página de códigos es un valor numérico que especifica una página de códigos instalada en Windows.  
  
 Reglas de conversión de páginas de códigos:  
  
- Si no se especifica ninguna página de códigos, **sqlcmd** usará la página de códigos actual para los archivos de entrada y salida, a menos que el archivo de entrada sea un archivo Unicode, en cuyo caso no es necesaria la conversión.  
  
- **sqlcmd** reconoce automáticamente los archivos de entrada Unicode "big endian" y "little endian". Si se ha especificado la opción **-u** , la salida siempre será Unicode "little endian".  
  
- Si no se especifica ningún archivo de salida, la página de códigos de salida será la página de códigos de la consola. Este enfoque permite que la salida se muestre correctamente en la consola.  
  
- Si hay varios archivos de entrada, se considera que pertenecen a la misma página de códigos. Los archivos de entrada Unicode y no Unicode pueden ser mixtos.  
  
 Escriba **chcp** en el símbolo del sistema para comprobar la página de códigos de Cmd.exe.  
  
 **-i** _input_file_[ **,** _input\_file2_...]  
 Identifica el archivo que contiene un lote de instrucciones SQL o procedimientos almacenados. Se pueden especificar varios archivos que se leerán y se procesarán en orden. No use ningún espacio entre los nombres de archivo. **sqlcmd** comprobará primero si todos los archivos especificados existen. Si uno o más archivos no existen, **sqlcmd** se cerrará. Las opciones -i y -Q/-q se excluyen mutuamente.  
  
 Ejemplos de rutas de acceso:  

```cmd
-i C:\<filename>  
-i \\<Server>\<Share$>\<filename>  
-i "C:\Some Folder\<file name>"  
```
  
 Las rutas de acceso a archivos que contengan espacios deben escribirse entre comillas.  
  
 Esta opción se puede usar más de una vez: **-i**_input\_file_ **-I**_I input_file._  
  
 **-o** _output_file_  
 Identifica el archivo que recibe la salida de **sqlcmd**.  
  
 Si se especifica **-u** , *archivo_de_salida* se almacena en formato Unicode. Si el nombre de archivo no es válido, se genera un mensaje de error y **sqlcmd** se cierra. **sqlcmd** no admite la escritura simultánea de varios procesos de **sqlcmd** en el mismo archivo. El archivo de salida estará dañado o será incorrecto. Observe que el modificador **-f** también es relevante para los formatos de archivo. Este archivo se creará si no existe. Se sobrescribirá cualquier archivo con el mismo nombre que pertenezca a una sesión de **sqlcmd** anterior. El archivo que se especifica aquí no es el archivo **stdout** . Si se especifica un archivo **stdout**, este archivo no se usará.  
  
 Ejemplos de rutas de acceso:  

```cmd
-o C:< filename>  
-o \\<Server>\<Share$>\<filename>  
-o "C:\Some Folder\<file name>"  
 ```
 Las rutas de acceso a archivos que contengan espacios deben escribirse entre comillas.  
  
 **-r**[**0** | **1**]  
 Redirige la salida del mensaje de error a la pantalla (**stderr**). Si no especifica ningún parámetro o si especifica **0**, solo se redirigirán los mensajes de error con un nivel de gravedad 11 o superior. Si especifica **1**, toda salida de mensaje, incluida PRINT, se redirigirá. No surte efecto si se usa -o. De forma predeterminada, los mensajes se envían a **stdout**.  
  
 **-R**  
 Hace que **sqlcmd** localice las columnas numéricas, de moneda, fecha y hora recuperadas de SQL Server según la configuración regional del cliente. De forma predeterminada, estas columnas se muestran con la configuración regional del servidor.  
  
 **-u**  
 Especifica que *archivo_de_salida* se almacena en formato Unicode, independientemente del formato de *archivo_de_entrada*.  
  
 **Opciones de ejecución de consultas**  
  **-e**  
 Escribe scripts de entrada en el dispositivo de salida estándar (**stdout**).  
  
 **-I**  
 Activa (establece en ON) la opción de conexión SET QUOTED_IDENTIFIER. De forma predeterminada, la opción está establecida en OFF. Para obtener más información, vea [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](~/t-sql/statements/set-quoted-identifier-transact-sql.md).  
  
 **-q "** _cmdline query_ **"**  
 Ejecuta una consulta cuando **sqlcmd** se inicia, pero no cierra **sqlcmd** cuando la consulta finaliza. Se pueden ejecutar varias consultas delimitadas por punto y coma. Utilice las comillas alrededor de la consulta, como se muestra en el siguiente ejemplo.  
  
 En el símbolo del sistema, escriba:  
  
 `sqlcmd -d AdventureWorks2012 -q "SELECT FirstName, LastName FROM Person.Person WHERE LastName LIKE 'Whi%';"`  
  
 `sqlcmd -d AdventureWorks2012 -q "SELECT TOP 5 FirstName FROM Person.Person;SELECT TOP 5 LastName FROM Person.Person;"`  
  
> [!IMPORTANT]  
>  No use el terminador GO en la consulta.  
  
 Si se especifica **-b** junto con esta opción, **sqlcmd** se cierra en caso de error. **-b** se describe más adelante en este artículo.  
  
 **-Q "** _cmdline query_ **"**  
 Ejecuta una consulta cuando se inicia **sqlcmd** e inmediatamente después cierra **sqlcmd**. Se pueden ejecutar varias consultas delimitadas por punto y coma.  
  
 Utilice las comillas alrededor de la consulta, como se muestra en el siguiente ejemplo.  
  
 En el símbolo del sistema, escriba:  
  
 `sqlcmd -d AdventureWorks2012 -Q "SELECT FirstName, LastName FROM Person.Person WHERE LastName LIKE 'Whi%';"`  
  
 `sqlcmd -d AdventureWorks2012 -Q "SELECT TOP 5 FirstName FROM Person.Person;SELECT TOP 5 LastName FROM Person.Person;"`  
  
> [!IMPORTANT]  
>  No use el terminador GO en la consulta.  
  
 Si se especifica **-b** junto con esta opción, **sqlcmd** se cierra en caso de error. **-b** se describe más adelante en este artículo.  
  
 **-t** _query_timeout_  
 Especifica el número de segundos que tienen que transcurrir antes de que un comando (o la instrucción de SQL) exceda el tiempo de espera. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDSTATTIMEOUT. Si no se especifica ningún valor para *tiempo_de_espera* , el comando no tiene tiempo de espera. El valor de *query* *time_out* debe ser un número entre 1 y 65534. Si el valor proporcionado no es numérico o no está dentro de ese intervalo, **sqlcmd** generará un mensaje de error.  
  
> [!NOTE]  
>  El tiempo de espera real puede variar unos segundos con respecto al valor de *tiempo_de_espera* especificado.  
  
 **-vvar =**  _value_[ **var =** _value_...]  
 Crea una variable de scripting de **sqlcmd** que puede usarse en un script de **sqlcmd** . Si el valor contiene espacios en blanco, especifíquelo entre comillas. Puede especificar varios valores _**var**_= **"** _values_ **"** . Si hay errores en alguno de los valores especificados, **sqlcmd** genera un mensaje de error y después se cierra.  
  
 `sqlcmd -v MyVar1=something MyVar2="some thing"`  
  
 `sqlcmd -v MyVar1=something -v MyVar2="some thing"`  
  
 **-x**  
 Hace que **sqlcmd** omita las variables de scripting. Este parámetro es útil cuando un script contiene muchas instrucciones INSERT que pueden contener cadenas con el mismo formato que las variables normales, por ejemplo, $(*nombre_de_variable*).  
  
 **Opciones de formato**  
  **-h** _headers_  
 Especifica el número de filas que se van a imprimir entre los encabezados de las columnas. La opción predeterminada es imprimir los encabezados una vez para cada conjunto de resultados de la consulta. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDHEADERS. Use **-1** para especificar que no se impriman los encabezados. Cualquier valor no válido hará que **sqlcmd** genere un mensaje de error y se cierre.  
  
 **-k** [**1** | **2**]  
 Quita todos los caracteres de control, como tabulaciones y nuevos caracteres de línea de la salida. Este parámetro conserva el formato de las columnas cuando se devuelven datos. Si se especifica 1, los caracteres de control se reemplazan con un solo espacio. Si se especifica 2, los caracteres de control consecutivos se reemplazan por un solo espacio. **-k** equivale a **-k1**.  
  
 **-s** _col_separator_  
 Especifica el carácter separador de columnas. El valor predeterminado es un espacio en blanco. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDCOLSEP. Para usar caracteres que tienen un significado especial para el sistema operativo, como la y comercial (&) o el punto y coma (;), incluya el carácter entre comillas ("). El separador de columnas puede ser cualquier carácter de 8 bits.  
  
 **-w** _column_width_  
 Especifica el ancho de pantalla para la salida. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDCOLWIDTH. El ancho de columna debe ser un número mayor que 8 y menor que 65536. Si el ancho de columna especificado no está en ese intervalo, **sqlcmd** generará un mensaje de error. El ancho predeterminado es 80 caracteres. Cuando la línea de salida supera el ancho de columna especificado, se ajusta a la siguiente línea.  
  
 **-W**  
 Esta opción quita los espacios finales de una columna. Use esta opción junto con la opción **-s** cuando prepare datos que se vayan a exportar a otra aplicación. No se puede usar con las opciones **-y** ni **-Y** .  
  
 **-y** _variable_length_type_display_width_  
 Establece la variable de scripting de **sqlcmd**`SQLCMDMAXVARTYPEWIDTH`. El valor predeterminado es 256. Limita el número de caracteres que se devuelve para tipos de datos de longitud variable y gran tamaño:  
  
- **ntext**  
  
- **nvarchar(max)**  
  
- **varbinary(max)**  
  
- **xml**  
  
- **Tipos de datos UDT (definidos por el usuario)**  
  
- **text**  
  
- **ntext**  
  
- **image**  
  
> [!NOTE]  
>  Los UDT pueden ser de longitud fija, en función de la implementación. Si esta longitud de un UDT de longitud fija es más corta que *anchura_de_visualización*, el valor del UDT devuelto no se ve afectado. No obstante, si la longitud es mayor que *anchura_de_visualización*, la salida queda truncada.  
   
  
> [!IMPORTANT]  
>  Use la opción **-y 0** con mucha precaución, ya que puede causar graves problemas de rendimiento en el servidor y en la red, según el tamaño de los datos devueltos.  
  
 **-Y** _fixed_length_type_display_width_  
 Establece la variable de scripting de **sqlcmd**`SQLCMDMAXFIXEDTYPEWIDTH`. El valor predeterminado es 0 (ilimitado). Limita el número de caracteres que se devuelve para los siguientes tipos de datos:  
  
- **char(** _n_ **)** , donde 1<=n<=8000  
  
- **nchar(n** _n_ **)** , donde 1<=n<=4000  
  
- **varchar(n** _n_ **)** , donde 1<=n<=8000  
  
- **nvarchar(n** _n_ **)** , donde 1<=n<=4000  
  
- **varbinary(n** _n_ **)** , donde 1<=n\<=4000  
  
- **variant**  
  
 **Opciones de informes de errores**  
  **-b**  
 Especifica que **sqlcmd** se cierre y devuelva un valor de DOS ERRORLEVEL cuando se produce un error. El valor que se devuelve a la variable DOS ERRORLEVEL es **1** cuando el mensaje de error de SQL Server tiene un nivel de gravedad superior a 10; de lo contrario, el valor devuelto es **0**. Si se ha establecido la opción **-V** además de **-b**, **sqlcmd** no notificará un error si el nivel de gravedad es inferior a los valores establecidos mediante **-V**. Los archivos por lotes del símbolo del sistema pueden probar el valor de ERRORLEVEL y controlar el error apropiadamente. **sqlcmd** no notifica los mensajes de error con un nivel de gravedad de 10 (mensajes informativos).  
  
 Si el script de **sqlcmd** contiene un comentario incorrecto, un error de sintaxis o carece de una variable de scripting, el valor de ERRORLEVEL devuelto es 1.  
  
 **-m** _error_level_  
 Controla qué mensajes de error se envían a **stdout**. Se envían los mensajes que tienen un nivel de gravedad mayor o igual que este nivel. Cuando este valor se establece en **-1**, se envían todos los mensajes, incluidos los informativos. No se permiten espacios entre **-m** y **-1**. Por ejemplo, **-m-1** es válido, pero **-m-1** no.  
  
 Esta opción también establece la variable de scripting de **sqlcmd** SQLCMDERRORLEVEL. El valor predeterminado de esta variable es 0.  
  
 **-V** _error_severity_level_  
 Controla el nivel de gravedad que se usa para establecer la variable ERRORLEVEL. Los mensajes de error que tienen niveles de gravedad mayores o iguales que este valor establecen ERRORLEVEL. Los valores menores que 0 se notifican como 0. Los archivos CMD y por lotes se pueden usar para probar el valor de la variable ERRORLEVEL.  
  
 **Otras opciones**  
  **-a** _packet_size_  
 Solicita un paquete de un tamaño diferente. Esta opción establece la variable de scripting de **sqlcmd** SQLCMDPACKETSIZE. *tamaño_paquete* debe ser un valor entre 512 y 32767. El valor predeterminado es 4096. Un tamaño de paquete mayor puede mejorar el rendimiento de la ejecución de scripts que comprenden gran cantidad de instrucciones de SQL entre los comandos GO. Puede solicitar un tamaño de paquete mayor. No obstante, si se deniega la solicitud, **sqlcmd** usa el valor predeterminado de servidor para el tamaño de paquete.  
  
 **-c** _batch_terminator_  
 Especifica el terminador del lote. De forma predeterminada, los comandos se terminan y se envían a SQL Server escribiendo la palabra "GO" en una línea aparte. Cuando restablezca el terminador del lote, no use palabras claves de SQL Server ni caracteres especiales que tengan un significado especial para el sistema operativo, incluso aunque vayan precedidos de una barra diagonal invertida.  
  
 **-L**[**c**]  
 Enumera los equipos servidores configurados localmente y los nombres de los equipos servidores que difunden en la red. Este parámetro no se puede usar en combinación con otros parámetros. El número máximo de equipos de servidor que se puede enumerar es 3000. Si la lista de servidor se trunca debido al tamaño del búfer, aparece un mensaje de advertencia.  
  
> [!NOTE]  
>  Debido a la naturaleza de las difusiones en las redes, **sqlcmd** podría no recibir una respuesta de todos los servidores a tiempo. Por lo tanto, la lista de servidores devuelta puede variar en cada invocación de esta opción.  
  
 Si se especifica el parámetro opcional **c**, la salida aparece sin la línea de encabezado **Servers:** y cada línea de servidor se muestra sin espacios iniciales. Esta presentación se conoce como salida limpia. La salida limpia mejora el rendimiento del procesamiento de los lenguajes de scripting.  
  
 **-p**[**1**]  
 Imprime estadísticas de rendimiento para cada conjunto de resultados. La siguiente pantalla muestra un ejemplo del formato para las estadísticas de rendimiento:  
  
 `Network packet size (bytes): n`  
  
 `x xact[s]:`  
  
 `Clock Time (ms.): total       t1  avg       t2 (t3 xacts per sec.)`  
  
 Donde:  
  
 `x` = número de transacciones que procesa SQL Server.  
  
 `t1` indica el tiempo total de todas las transacciones.  
  
 `t2` = tiempo medio de una única transacción.  
  
 `t3` = número medio de transacciones por segundo.  
  
 Todos los tiempos se indican en milisegundos.  
  
 Si se especifica el parámetro opcional **1** , el formato de salida de las estadísticas es el separado por dos puntos, que se puede importar fácilmente en una hoja de cálculo o se puede procesar en un script.  
  
 Si el parámetro opcional tiene cualquier valor distinto de **1**, se genera un error y **sqlcmd** se cierra.  
  
 **-X**[**1**]  
 Deshabilita los comandos que pueden poner en peligro la seguridad del sistema cuando se ejecuta **sqlcmd** desde un archivo por lotes. Los comandos deshabilitados se siguen reconociendo; **sqlcmd** emite un mensaje de advertencia y sigue ejecutándose. Si se especifica el parámetro opcional **1** , **sqlcmd** genera un mensaje de error y después se cierra. Los siguientes comandos se deshabilitan cuando se usa la opción **-X** :  
  
- **ED**  
  
- **!!** _command_  
  
 Si se especifica la opción **-X** , se evita que se pasen variables de entorno a **sqlcmd**. También impide que el script de inicio especificada mediante la variable de scripting SQLCMDINI se ejecute. Para más información sobre variables de scripting **sqlcmd** , vea [Usar sqlcmd con variables de script](~/relational-databases/scripting/sqlcmd-use-with-scripting-variables.md).  
  
 **-?**  
 Muestra la versión de **sqlcmd** y un resumen de la sintaxis de las opciones de **sqlcmd** .  
  
## <a name="remarks"></a>Observaciones

Las opciones no tienen que utilizarse forzosamente en el orden mostrado en la sección de sintaxis.

Cuando se devuelven varios resultados, **sqlcmd** imprime una línea en blanco entre cada conjunto de resultados de un lote. Además, el mensaje `<x> rows affected` no aparece cuando no se aplica a la instrucción ejecutada.

Para usar **sqlcmd** de forma interactiva, escriba **sqlcmd** en el símbolo del sistema con una o varias de las opciones descritas anteriormente en este artículo. Para más información, vea [Usar la utilidad sqlcmd](~/relational-databases/scripting/sqlcmd-use-the-utility.md).

> [!NOTE]  
>  Las opciones **-L**, **-Q**, **-Z** o **-i** hacen que **sqlcmd** se cierre después de la ejecución.
  
 La longitud total de la línea de comandos de **sqlcmd** en el entorno de comandos (Cmd.exe), incluidos todos los argumentos y las variables expandidas, es la que determina el sistema operativo para Cmd.exe.
  
## <a name="variable-precedence-low-to-high"></a>Precedencia de variables (menor a mayor)
  
1.  Variables de entorno de nivel de sistema.
  
2.  Variables de entorno de nivel de usuario.  
  
3.  El shell de comandos (**SET** X=Y) se establece en el símbolo del sistema antes de ejecutar **sqlcmd**.  
  
4.  **sqlcmd-v** X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  Para ver las variables de entorno, en el **Panel de control**, abra **Sistema** y haga clic en la pestaña **Opciones avanzadas** .  
  
## <a name="sqlcmd-scripting-variables"></a>Variables de scripting sqlcmd  
  
|Variable|Modificador relacionado|L/E|Valor predeterminado|  
|--------------|--------------------|----------|-------------|  
|SQLCMDUSER|-U|R|""|  
|SQLCMDPASSWORD|-P|--|""|  
|SQLCMDSERVER|-S|R|"DefaultLocalInstance"|  
|SQLCMDWORKSTATION|-H|R|"ComputerName"|  
|SQLCMDDBNAME|-d|R|""|  
|SQLCMDLOGINTIMEOUT|-l|L/E|"8" (segundos)|  
|SQLCMDSTATTIMEOUT|-T|L/E|"0" = esperar indefinidamente|  
|SQLCMDHEADERS|-H|L/E|"0"|  
|SQLCMDCOLSEP|-S|L/E|" "|  
|SQLCMDCOLWIDTH|-w|L/E|"0"|  
|SQLCMDPACKETSIZE|-a|R|"4096"|  
|SQLCMDERRORLEVEL|-M|L/E|0|  
|SQLCMDMAXVARTYPEWIDTH|-y|L/E|"256"|  
|SQLCMDMAXFIXEDTYPEWIDTH|-y|L/E|"0" = ilimitado|  
|SQLCMDEDITOR||L/E|"edit.com"|  
|SQLCMDINI||R|""|
|SQLCMDUSEAAD  | -G | L/E | "" |  
  
 SQLCMDUSER, SQLCMDPASSWORD y SQLCMDSERVER se establecen cuando se usa **:Connect**.  
  
 L indica que el valor solo puede establecerse una vez durante la inicialización del programa.  
  
 L/E indica que el valor puede modificarse mediante el comando **setvar** y que los comandos siguientes se verán influidos por el nuevo valor.  
  
## <a name="sqlcmd-commands"></a>Comandos de sqlcmd  
 Además de las instrucciones Transact-SQL de **sqlcmd**, también están disponibles los siguientes comandos:  
  
|||  
|-|-|  
|**GO** [*count*]|**:List**|  
|[ **:** ] **RESET**|**:Error**|  
|[ **:** ] **ED**|**:Out**|  
|[**:**] **!!**|**:Perftrace**|  
|[ **:** ] **QUIT**|**:Connect**|  
|[ **:** ] **EXIT**|**:On Error**|  
|**:r**|**:Help**|  
|**:ServerList**|**:XML** [**ON** &#124; **OFF**]|  
|**:Setvar**|**:Listvar**|  
  
 Tenga en cuenta lo siguiente cuando use comandos de **sqlcmd** :  
  
- Todos los comandos de **sqlcmd** , excepto GO, deben ir precedidos de dos puntos (:).  
  
    > [!IMPORTANT]  
    >  Para mantener la compatibilidad con los scripts de **osql** existentes, algunos de los comandos se reconocerán sin los dos puntos, indicado por [ **:** ].
  
- Los comandos de **sqlcmd** se reconocen solo si aparecen al principio de una línea.  
  
- Los comandos de **sqlcmd** no distinguen entre mayúsculas y minúsculas.  
  
- Cada comando debe estar en una línea separada. Un comando no puede ir seguido de una instrucción de Transact-SQL o de otro comando.  
  
- Los comandos se ejecutan inmediatamente. No se colocan en el búfer de ejecución, como es el caso de las instrucciones Transact-SQL.  
  
 **Editar comandos**  
  [ **:** ] **ED**  
 Inicia el editor de texto. Este editor se puede utilizar para editar el lote actual de Transact-SQL o el último lote ejecutado. Para editar el último lote ejecutado, el comando **ED** debe escribirse inmediatamente después de que se complete la ejecución del último lote.  
  
 El editor de texto se define mediante la variable de entorno SQLCMDEDITOR. El editor predeterminado es "Edit". Para cambiar el editor, establezca la variable de entorno SQLCMDEDITOR. Por ejemplo, para establecer el editor en el Bloc de notas de Microsoft, en el símbolo del sistema, escriba:  
  
 `SET SQLCMDEDITOR=notepad`  
  
 [ **:** ] **RESET**  
 Borra la caché de instrucciones.  
  
 **:List**  
 Imprime el contenido de la memoria caché de instrucciones.  
  
 **Variables**  
  **:Setvar** \<**var**> [ **"** _valor_ **"** ]  
 Define variables de scripting de **sqlcmd** . Las variables de scripting tienen el siguiente formato: `$(VARNAME)`.  
  
 Los nombres de variables no distinguen entre mayúsculas y minúsculas.  
  
 Las variables de scripting pueden establecerse de los siguientes modos:  
  
- Implícitamente mediante una opción de línea de comandos. Por ejemplo, la opción **-l** establece la variable de **sqlcmd** SQLCMDLOGINTIMEOUT.  
  
- Explícitamente mediante el comando **:Setvar** .  
  
- Al definir una variable de entorno antes de ejecutar **sqlcmd**.  
  
> [!NOTE]  
>  La opción **-X** impide que las variables de entorno se pasen a **sqlcmd**.  
  
 Si una variable definida mediante **:Setvar** y una variable de entorno tienen el mismo nombre, la variable definida mediante **:Setvar** tiene prioridad.  
  
 Los nombres de variables no deben contener caracteres de espacio en blanco.  
  
 Los nombres de variable no pueden tener el mismo formato que una expresión variable como $(var).  
  
 Si el valor de la cadena de la variable de script contiene espacios en blanco, incluya el valor entre comillas. Si un valor para la variable del script no se especifica, la variable de script se elimina.  
  
 **:Listvar**  
 Muestra una lista de variables de scripting que están establecidas actualmente.  
  
> [!NOTE]  
>  Solo se mostrarán las variables de scripting establecidas mediante **sqlcmd** y las variables establecidas con el comando **:Setvar** .  
  
 **Comandos de salida**  
  **:Error**   
 _**\<**_  _filename_  **_>|_ STDERR|STDOUT**  
 Redirige toda la salida de error al archivo especificado por *nombre_de_archivo*, a **stderr** o a **stdout**. El comando **Error** puede aparecer varias veces en un script. De forma predeterminada, la salida de error se envía a **stderr**.  
  
 *Nombre de archivo*  
 Crea y abre un archivo que recibirá la salida. Si el archivo ya existe, se truncará en cero bytes. Si el archivo no está disponible a causa de los permisos u otros motivos, la salida no se cambiará y se enviará al último destino especificado o al predeterminado.  
  
 **STDERR**  
 Cambia la salida del error al flujo **stderr** . Si se ha redirigido, el destino al cual se redirige el flujo recibirá la salida del error.  
  
 **STDOUT**  
 Cambia la salida del error al flujo **stdout** . Si se ha redirigido, el destino al cual se redirige el flujo recibirá la salida del error.  
  
 **:Out \<** _filename_ **>** | **STDERR**| **STDOUT**  
 Crea y redirige todos los resultados de consulta al archivo especificado por *file name*, a **stderr** o a **stdout**. De forma predeterminada, la salida se envía a **stdout**. Si el archivo ya existe, se trunca en cero bytes. El comando **Out** puede aparecer varias veces en un script.  
  
 **:Perftrace \<** _filename_ **>** | **STDERR**| **STDOUT**  
 Crea y redirige toda la información de seguimiento de rendimiento al archivo especificado por *nombre_de_archivo*, a **stderr** o a **stdout**. De forma predeterminada, la salida de seguimiento de rendimiento se envía a **stdout**. Si el archivo ya existe, se trunca en cero bytes. El comando **Perftrace** puede aparecer varias veces en un script.  
  
 **Comandos de control de ejecución**  
  **:On Error**[ **exit** | **ignore**]  
 Establece la acción que se llevará a cabo cuando se produzca un error durante la ejecución del script o del lote.  
  
 Cuando se usa la opción **exit** , **sqlcmd** se cierra con el valor de error correspondiente.  
  
 Cuando se usa la opción **ignore** , **sqlcmd** pasa por alto el error y continúa con la ejecución del lote o del script. De forma predeterminada, se imprime un mensaje de error.  
  
 [ **:** ] **QUIT**  
 Hace que **sqlcmd** se cierre.  
  
 [ **:** ] **EXIT**[ **(** _instrucción_ **)** ]  
 Permite usar el resultado de una instrucción SELECT como valor devuelto de **sqlcmd**. Si es numérica, la primera columna de la última fila del resultado se convierte en un entero de 4 bytes (long). MS-DOS, Linux y Mac pasan el byte inferior al proceso primario o al nivel de errores del sistema operativo. Windows 200x pasa el entero de 4 bytes completo. La sintaxis es:  
  
 `:EXIT(query)`  
  
 Por ejemplo:  
  
 `:EXIT(SELECT @@ROWCOUNT)`  
  
 También puede incluir el parámetro **EXIT** como parte de un archivo por lotes. Por ejemplo, en el símbolo del sistema, escriba:  
  
 `sqlcmd -Q "EXIT(SELECT COUNT(*) FROM '%1')"`  
  
 La utilidad **sqlcmd** envía todo lo que está entre paréntesis **()** al servidor. Si un procedimiento almacenado del sistema selecciona un conjunto y devuelve un valor, solo se devuelve la selección. La instrucción EXIT **()** sin nada entre los paréntesis ejecuta todo lo que precede a estos en el lote y, después, se cierra sin ningún valor devuelto.  
  
 Cuando se especifica una consulta incorrecta, **sqlcmd** se cierra sin devolver ningún valor.  
  
 Aquí se muestra una lista de formatos de EXIT:  
  
- :EXIT  
  
 No ejecuta el lote y, después, sale de forma inmediata y no devuelve ningún valor.  
  
- :EXIT( )  
  
 Ejecuta el lote y, a continuación, sale sin devolver ningún valor.  
  
- :EXIT(query)  
  
 Ejecuta el lote que incluye la consulta y, a continuación, se cierra tras devolver el resultado de la consulta.  
  
 Si se usa RAISERROR en un script de **sqlcmd** y se genera el estado 127, **sqlcmd** se cerrará y devolverá un identificador de mensaje al cliente. Por ejemplo:  
  
 `RAISERROR(50001, 10, 127)`  
  
 Este error hará que el script de **sqlcmd** finalice y devuelva el identificador de mensaje 50001 al cliente.  
  
 Los valores devueltos -1 a -99 están reservados para SQL Server; y **sqlcmd** define los siguientes valores devueltos adicionales:  
  
|Valores devueltos|Descripción|  
|-------------------|-----------------|  
|-100|Error encontrado antes de seleccionar el valor devuelto.|  
|-101|No se encontró ninguna fila al seleccionar el valor devuelto.|  
|-102|Error de conversión al seleccionar el valor devuelto.|  
  
 **GO** [*count*]  
 GO marca tanto el final de un lote como la ejecución de cualquier instrucción de Transact-SQL almacenada en caché. El lote se ejecuta varias veces como lotes independientes. No se puede declarar una variable más de una vez en un único lote.
  
 **Otros comandos**  
  **:r \<** _filename_ **>**  
 Analiza instrucciones de Transact-SQL y comandos **sqlcmd** adicionales desde el archivo especificado por **\<**_filename_**>** en la caché de instrucciones.  
  
 Si el archivo contiene instrucciones Transact-SQL que no van seguidas de **GO**, debe escribir **GO** en la línea que sigue a **:r**.  
  
> [!NOTE]  
>  **\<** _filename_ **>** se lee de forma relativa al directorio de inicio en el que se ha ejecutado **sqlcmd**.  
  
 El archivo se leerá y se ejecutará después de que se encuentre un terminador de lote. Puede emitir varios comandos **:r** . El archivo puede incluir cualquier comando de **sqlcmd** . Eso incluye el terminador de lote **GO**.  
  
> [!NOTE]  
>  El recuento de líneas que se muestra en el modo interactivo aumentará en uno por cada comando `:r` que se encuentre. El comando `:r` aparecerá en la salida del comando de lista.  
  
 **:Serverlist**  
 Enumera los servidores configurados localmente y los nombres de los servidores que difunden en la red.  
  
 **:Connect**  _server_name_[ **\\** _instance\_name_] [-l *timeout*] [-U *user_name* [-P *password*]]  
 Conecta con una instancia de SQL Server. También cierra la conexión actual.  
  
 Opciones de tiempo de espera:  
  
|Value|Comportamiento|  
|-|-|  
|0|espera indefinidamente|  
|n>0|espera durante n segundos|  
  
 La variable de scripting SQLCMDSERVER reflejará la conexión activa actual.  
  
 Si no se especifica *timeout* , el valor de la variable SQLCMDLOGINTIMEOUT es el predeterminado.  
  
 Si solo se especifica *nombre_del_usuario* (como opción o como variable de entorno), se solicitará al usuario que especifique una contraseña. No se avisa a los usuarios si se han establecido las variables de entorno SQLCMDUSER o SQLCMDPASSWORD. Si no se proporcionan opciones ni variables de entorno, se iniciará sesión en modo Autenticación de Windows. Por ejemplo, para conectar con una instancia, `instance1`, de SQL Server, `myserver`, mediante seguridad integrada, se necesitaría el siguiente comando:  
  
 `:connect myserver\instance1`  
  
 Para conectar con la instancia predeterminada de `myserver` con variables de script, utilizaría lo siguiente:  
  
 `:setvar myusername test`  
  
 `:setvar myservername myserver`  
  
 `:connect $(myservername) $(myusername)`  
  
 [ **:** ] **!!** < *command*>  
 Ejecuta comandos del sistema operativo. Para ejecutar un comando del sistema operativo, inicie una línea con dos signos de exclamación ( **!!** ) seguidos por el comando del sistema operativo. Por ejemplo:  
  
 `:!! Dir`  
  
> [!NOTE]  
>  El comando se ejecuta en el equipo en el que se ejecuta **sqlcmd** .  
  
 **:XML** [**ON** | **OFF**]  
 Para más información, construye [Formato de salida XML](#OutputXML) y [Formato de salida de JSON](#OutputJSON), más adelante en este artículo  
  
 **:Help**  
 Muestra los comandos de **sqlcmd** junto con una breve descripción de cada comando.  
  
### <a name="sqlcmd-file-names"></a>Nombres de archivo de sqlcmd  
 Los archivos de entrada de **sqlcmd** se pueden especificar con la opción **-i** o con el comando **:r** . Los archivos de salida se pueden especificar con la opción **-o** o con los comandos **:Error**, **:Out** y **:Perftrace** . A continuación se incluyen algunas directrices para trabajar con estos archivos:  
  
- **:Error**, **:Out** y **:Perftrace** deben usar un valor **\<**_filename_**>** distinto. Si se usa el mismo valor **\<**_filename_**>** , es posible que las entradas de los comandos se entremezclen.  
  
- Si **sqlcmd** llama a un archivo de entrada ubicado en un servidor remoto desde un equipo local y el archivo contiene una ruta de acceso de archivo del tipo :Out c:\archivoDeSalida.txt. El archivo de salida se crea en el equipo local y no en el servidor remoto.  
  
- Entre las rutas de archivo válidas se incluyen: `C:\<filename>`, `\\<Server>\<Share$>\<filename>` y `"C:\Some Folder\<file name>"`. Si hay algún espacio en blanco en la ruta de acceso, use comillas.  
  
- Cada nueva sesión de **sqlcmd** sobrescribirá los archivos existentes que tengan el mismo nombre.  
  
### <a name="informational-messages"></a>Mensajes informativos

**sqlcmd** imprime los mensajes informativos enviados por el servidor. En el siguiente ejemplo, tras ejecutar las instrucciones de Transact-SQL, se imprime un mensaje informativo.
  
En el símbolo del sistema, escriba el comando:

`sqlcmd`
  
En el tipo de símbolo del sistema de sqlcmd:

`USE AdventureWorks2012;`

`GO`

Al presionar ENTRAR, se imprime el siguiente mensaje informativo: "Se cambió el contexto de la base de datos a 'AdventureWorks2012'."  
  
### <a name="output-format-from-transact-sql-queries"></a>Formato de salida de consultas de Transact-SQL  
 **sqlcmd** primero imprime un encabezado de columna que contiene los nombres de columna especificados en la lista de selección. Los nombres de columna se separan mediante el carácter SQLCMDCOLSEP. De forma predeterminada, es un espacio en blanco. Si el nombre de la columna es más corto que el ancho de la columna, la salida se rellena con espacios hasta la siguiente columna.  
  
 Esta línea irá seguida de una línea separadora, que es una serie de caracteres de guión. La siguiente salida muestra un ejemplo.  
  
 Inicie **sqlcmd**. En el símbolo del sistema de **sqlcmd**, escriba lo siguiente:  
  
 `USE AdventureWorks2012;`  
  
 `SELECT TOP (2) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 Cuando presione ENTRAR, se devolverá el siguiente conjunto de resultados.  
  
 `BusinessEntityID FirstName    LastName`  
  
 `---------------- ------------ ----------`  
  
 `285              Syed         Abbas`  
  
 `293              Catherine    Abel`  
  
 `(2 row(s) affected)`  
  
 Aunque la columna `BusinessEntityID` tiene solo cuatro caracteres de ancho, se ha expandido para acomodar el nombre de columna más largo. De forma predeterminada, la salida finaliza a los 80 caracteres. Esto se puede cambiar mediante la opción **-w** o estableciendo la variable de scripting SQLCMDCOLWIDTH.  
  
###  <a name="xml-output-format"></a><a name="OutputXML"></a> Formato de salida XML  
 La salida XML de una cláusula FOR XML se ofrece sin formato en un flujo continuo.  
  
 Cuando espere una salida XML, use el siguiente comando: `:XML ON`.  
  
> [!NOTE]  
>  **sqlcmd** devuelve mensajes de error en el formato habitual. Tenga en cuenta que los mensajes de error también salen en el flujo de texto XML en formato XML. Con `:XML ON`, **sqlcmd** no muestra mensajes informativos.  
  
 Para desactivar el modo XML, use el siguiente comando: `:XML OFF`.  
  
 El comando GO no debe aparecer antes de que se emita el comando XML OFF, ya que el comando XML OFF vuelve a cambiar **sqlcmd** a la salida orientada a filas.  
  
 Los datos XML (de flujo) y los datos del conjunto de filas no se pueden mezclar. Si el comando XML ON no se ha emitido antes de ejecutar una instrucción Transact-SQL que genera flujos XML, la salida será confusa. Cuando se ha emitido el comando XML ON, no se pueden ejecutar instrucciones Transact-SQL que den como resultado conjuntos de filas normales.  
  
> [!NOTE]  
>  El comando `:XML` no admite la instrucción SET STATISTICS XML.  
  
###  <a name="json-output-format"></a><a name="OutputJSON"></a> Formato de salida de JSON  
 Cuando espere una salida de JSON, use el siguiente comando: `:XML ON`. De lo contrario, la salida incluye el nombre de columna y el texto JSON, que no es JSON válido.  
  
 Para desactivar el modo XML, use el siguiente comando: `:XML OFF`.  
  
 Para más información, consulte [Formato de salida XML](#OutputXML) en este artículo.  

### <a name="using-azure-active-directory-authentication"></a>Uso de la autenticación de Azure Active Directory

Ejemplos de uso de la autenticación de Azure Active Directory:

```cmd
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  -l 30
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -G -U bob@contoso.com -P MyAADPassword -l 30
```
  
## <a name="sqlcmd-best-practices"></a>Prácticas recomendadas para sqlcmd

Use las siguientes prácticas para maximizar la seguridad y la eficacia.  

- Use seguridad integrada.  

- Use **-X[1]** en entornos automatizados.

- Proteja los archivos de entrada y salida mediante los permisos del sistema de archivos NTFS adecuados.

- Para aumentar el rendimiento, haga tanto como pueda en una sola sesión de **sqlcmd** en vez de emplear varias.

- Establezca valores de tiempo de espera para la ejecución de lotes y consultas superiores a los que prevea para la ejecución de cada lote o consulta.

Use los procedimientos siguientes para ayudar a maximizar la exactitud:

- Use **-V16** para registrar los [mensajes con una gravedad de nivel 16](../relational-databases/errors-events/database-engine-error-severities.md#levels-of-severity).  Los mensajes de gravedad 16 indican errores generales que el usuario puede corregir.

- Una vez finalizado el proceso, compruebe el código de salida y la variable DOS ERRORLEVEL.  **sqlcmd** devolverá normalmente 0; de lo contrario, establecerá el valor de ERRORLEVEL configurado en **-V**.  En otras palabras, no cabe esperar que ERRORLEVEL tenga el mismo valor que el número de error que se ha comunicado desde SQL Server. El número de error es un valor específico de SQL Server correspondiente a la función del sistema [ **@@ERROR**](../t-sql/functions/error-transact-sql.md).  ERRORLEVEL es un valor específico de SQLCMD que indica por qué ha finalizado (es decir, SQLCMD) y la especificación del argumento de la línea de comandos **-b** influye en su valor.

El uso de **-V16** en combinación con la comprobación del código de salida y DOS ERRORLEVEL puede ayudar a detectar errores en entornos automatizados, especialmente en las pruebas de calidad antes de una versión de producción.

## <a name="next-steps"></a>Pasos siguientes

- [Iniciar la utilidad sqlcmd](~/relational-databases/scripting/sqlcmd-start-the-utility.md)
- [Ejecutar archivos de scripts Transact-SQL mediante sqlcmd](~/relational-databases/scripting/sqlcmd-run-transact-sql-script-files.md)
- [Usar la utilidad sqlcmd](~/relational-databases/scripting/sqlcmd-use-the-utility.md)
- [Usar sqlcmd con variables de script](~/relational-databases/scripting/sqlcmd-use-with-scripting-variables.md)
- [Conectarse al motor de base de datos con sqlcmd](~/relational-databases/scripting/sqlcmd-connect-to-the-database-engine.md)
- [Modificar scripts SQLCMD con el Editor de consultas](~/relational-databases/scripting/edit-sqlcmd-scripts-with-query-editor.md)
- [Administrar pasos de trabajo](~/ssms/agent/manage-job-steps.md)   
- [Crear un paso de trabajo CmdExec](~/ssms/agent/create-a-cmdexec-job-step.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]