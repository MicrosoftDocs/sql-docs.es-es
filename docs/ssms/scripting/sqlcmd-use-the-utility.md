---
title: Usar la utilidad sqlcmd
description: Aprenda a usar la utilidad sqlcmd para la ejecución ad hoc e interactiva de instrucciones y scripts de Transact-SQL, y para la automatización de tareas de creación de scripts de Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 06/06/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- Transact-SQL statements, executing
- command prompt utilities [SQL Server], sqlcmd
- statements [SQL Server], executing
- sqlcmd utility, about sqlcmd utility
ms.assetid: 3ec89119-7314-43ef-9e91-12e72bb63d62
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a1be2303d4496957f7352d4b103b548902b48d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339385"
---
# <a name="sqlcmd---use-the-utility"></a>sqlcmd: usar la utilidad
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  La utilidad **sqlcmd** es una herramienta de línea de comandos para la ejecución ad hoc e interactiva de instrucciones y scripts de [!INCLUDE[tsql](../../includes/tsql-md.md)] y para la automatización de tareas de creación de scripts de [!INCLUDE[tsql](../../includes/tsql-md.md)] . Para usar **sqlcmd** interactivamente o para compilar archivos de script que se ejecutan mediante **sqlcmd**, los usuarios deben estar familiarizados con [!INCLUDE[tsql](../../includes/tsql-md.md)]. La utilidad **sqlcmd** se usa normalmente de las formas siguientes:  
  
-   Los usuarios escriben instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] de una forma similar al modo en que trabajan con el símbolo del sistema. Los resultados se muestran en el símbolo del sistema. Para abrir una ventana del símbolo del sistema, escriba "cmd" en el cuadro de búsqueda de Windows y haga clic en **Símbolo del sistema** para abrirla. En el símbolo del sistema, escriba **sqlcmd** seguido de una lista de opciones que quiera. Para obtener una lista completa de las opciones admitidas por **sqlcmd**, vea [sqlcmd (utilidad)](../../tools/sqlcmd-utility.md).  
  
-   Los usuarios envían un trabajo **sqlcmd** especificando la ejecución de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] individual o dirigiendo la utilidad hacia un archivo de texto que contiene las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se van a ejecutar. El resultado se dirige normalmente hacia un archivo de texto, aunque también se puede mostrar en el símbolo del sistema.  
  
-   [Modo SQLCMD](./edit-sqlcmd-scripts-with-query-editor.md) en el Editor de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
-   Objetos de administración de SQL Server (SMO)  
  
-   Trabajos CmdExec del Agente SQL Server.  
  
## <a name="typically-used-sqlcmd-options"></a>Opciones de sqlcmd que se suelen usar  
  
-   La opción del servidor ( **-S**) que identifica la instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que se conecta **sqlcmd**.  
  
-   Las opciones de autenticación ( **-E**, **-U** y **-P**) que especifican las credenciales que usa **sqlcmd** para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **NOTA:** La opción **-E** es la predeterminada, por lo que no es necesario especificarla.  
  
-   Las opciones de entrada ( **-Q**, **-q** e **-i**) que identifican la ubicación de la entrada a **sqlcmd**.  
  
-   La opción de salida ( **-o**) que especifica el archivo en el que se guardará la salida de **sqlcmd**.  
  
## <a name="connect-to-the-sqlcmd-utility"></a>Conectarse a la utilidad sqlcmd  
  
-   Conectarse a la instancia predeterminada mediante la Autenticación de Windows para ejecutar de forma interactiva instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    sqlcmd -S <ComputerName>  
    ```  
  
    > **NOTA:** En el ejemplo anterior, **-E** no se especifica porque es el valor predeterminado y **sqlcmd** se conecta a la instancia predeterminada mediante Autenticación de Windows.  
  
-   Conectarse a la instancia con nombre mediante la Autenticación de Windows para ejecutar de forma interactiva instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    ```  
    sqlcmd -S <ComputerName>\<InstanceName>  
    ```  
  
     or  
  
    ```  
    sqlcmd -S .\<InstanceName>  
    ```  
  
-   Conectarse a una instancia con nombre mediante la Autenticación de Windows y especificar los archivos de entrada y salida:  
  
    ```  
    sqlcmd -S <ComputerName>\<InstanceName> -i <MyScript.sql> -o <MyOutput.rpt>  
    ```  
  
-   Conectarse a la instancia predeterminada del equipo local mediante la Autenticación de Windows, ejecutar una consulta y mantener la ejecución de **sqlcmd** después de la finalización de la consulta:  
  
    ```  
    sqlcmd -q "SELECT * FROM AdventureWorks2012.Person.Person"  
    ```  
  
-   Conectarse a la instancia predeterminada del equipo local mediante la Autenticación de Windows, ejecutar una consulta, dirigir la salida a un archivo y cerrar **sqlcmd** después de la finalización de la consulta:  
  
    ```  
    sqlcmd -Q "SELECT * FROM AdventureWorks2012.Person.Person" -o MyOutput.txt  
    ```  
  
-   Conectarse a una instancia con nombre mediante la Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar interactivamente instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] y hacer que **sqlcmd** solicite una contraseña:  
  
    ```  
    sqlcmd -U MyLogin -S <ComputerName>\<InstanceName>  
    ```  
  
    > **SUGERENCIA:** Para ver una lista de opciones admitidas por la utilidad **sqlcmd** , ejecute: `sqlcmd -?`.  
  
## <a name="run-transact-sql-statements-interactively-by-using-sqlcmd"></a>Ejecutar instrucciones Transact-SQL de forma interactiva mediante sqlcmd  
 Se puede usar la utilidad **sqlcmd** de forma interactiva para ejecutar instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] en una ventana del símbolo del sistema. Para ejecutar de forma interactiva instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] mediante **sqlcmd**, ejecute la utilidad sin usar las opciones **-Q**, **-q**, **-Z** o **-i** para especificar archivos de entrada o consultas. Por ejemplo:  
  
 `sqlcmd -S <ComputerName>\<InstanceName>`  
  
 Cuando se ejecuta el comando sin archivos de entrada ni consultas, **sqlcmd** se conecta a la instancia especificada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, después, muestra una nueva línea con un `1>` seguido de un carácter de subrayado intermitente, denominado símbolo del sistema **sqlcmd** . El `1` significa que se trata de la primera línea de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] y el símbolo del sistema **sqlcmd** es el punto en el que empezará la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] cuando la escriba.  
  
 En el símbolo de sistema **sqlcmd** , puede escribir instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] y comandos de **sqlcmd** , como **GO** y **EXIT**. Cada instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] se coloca en un búfer llamado caché de instrucciones. Estas instrucciones se enviarán a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando escriba el comando **GO** y pulse ENTRAR. Para salir de **sqlcmd**, escriba **EXIT** o **QUIT** al principio de una línea nueva.  
  
 Para borrar la memoria caché de instrucciones, escriba **:RESET**. Si escribe **^C** , **sqlcmd** se cerrará. **^C** se puede usar también para detener la ejecución de la memoria caché de instrucciones después de emitir un comando **GO** .  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] que se escriben en una sesión interactiva pueden editarse si se escribe el comando **:ED** y el símbolo de sistema de **sqlcmd** . Se abrirá el editor y, después de editar la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] y cerrar el editor, la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] revisada aparecerá en la ventana de comandos. Escriba **GO** para ejecutar la instrucción revisada [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
## <a name="quoted-strings"></a>Cadenas entre comillas  
 Los caracteres que están entre comillas se usan sin ningún procesamiento previo adicional, con la excepción de que las comillas se pueden insertar en una cadena especificando dos comillas consecutivas. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] trata esta secuencia de caracteres como una comilla. Sin embargo, la traducción se lleva a cabo en el servidor. Las variables de scripting no se expandirán si aparecen en una cadena.  
  
 Por ejemplo:  
  
 `sqlcmd`  
  
 `PRINT "Length: 5"" 7'";`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Length: 5" 7'`  
  
## <a name="strings-that-span-multiple-lines"></a>Cadenas que abarcan varias líneas  
 **sqlcmd** admite scripts con cadenas que abarcan varias líneas. Por ejemplo, la siguiente instrucción `SELECT` abarca varias líneas, pero es una única cadena que se ejecuta cuando se presiona la tecla ENTRAR después de escribir `GO`.  
  
 `SELECT First line`  
  
 `FROM Second line`  
  
 `WHERE Third line;`  
  
 `GO`  
  
## <a name="interactive-sqlcmd-example"></a>Ejemplo de sqlcmd interactivo  
 Este es un ejemplo de lo que se ve cuando se ejecuta **sqlcmd** de forma interactiva.  
  
 Cuando se abre una ventana del símbolo del sistema, solo hay una línea similar a:  
  
 `C:\> _`  
  
 Esto significa que la carpeta `C:\` es la carpeta actual y, si se especifica un nombre de archivo, Windows buscará ese archivo en esa carpeta.  
  
 Escriba **sqlcmd** para conectarse a la instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo local; de esta forma, el contenido de la ventana del símbolo del sistema será:  
  
 `C:\>sqlcmd`  
  
 `1> _`  
  
 Esto significa que se ha conectado a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y `sqlcmd` está listo para aceptar instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] y comandos de `sqlcmd` . El carácter de subrayado intermitente después de `1>` es el símbolo del sistema `sqlcmd` que marca la ubicación donde se mostrarán las instrucciones y los comandos que se escriban. Escriba **USE AdventureWorks2012** y pulse ENTRAR; a continuación, escriba **GO** y pulse ENTRAR. El contenido de la ventana del símbolo del sistema será:  
  
 `sqlcmd`  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Changed database context to 'AdventureWorks2012'.`  
  
 `1> _`  
  
 Presionar ENTRAR después de escribir `USE AdventureWorks2012` indica a `sqlcmd` que inicie una línea nueva. Al presionar ENTRAR, después de escribir `GO,` , se indicó que `sqlcmd` enviara la instrucción `USE AdventureWorks2012` a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. `sqlcmd` devuelve un mensaje para indicar que la instrucción `USE` se completó correctamente y muestra un nuevo símbolo del sistema `1>` como señal de que el usuario puede escribir una instrucción o un comando nuevos.  
  
 En el ejemplo siguiente se muestra qué contiene la ventana del símbolo del sistema si escribe una instrucción `SELECT` , `GO` para ejecutar `SELECT`y `EXIT` para finalizar `sqlcmd`:  
  
 `sqlcmd`  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 `SELECT TOP (3) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `BusinessEntityID   FirstName                 LastName`  
  
 `----------- -------------------------------- -----------`  
  
 `1           Syed                             Abbas`  
  
 `2           Catherine                        Abel`  
  
 `3           Kim                              Abercrombie`  
  
 `(3 rows affected)`  
  
 `1> EXIT`  
  
 `C:\>`  
  
 Las líneas posteriores a la línea `3> GO` son la salida de una instrucción `SELECT` . Para generar una salida, `sqlcmd` restablece el símbolo de sistema `sqlcmd` y muestra `1>`. Después de escribir `EXIT` en la línea `1>`, la ventana del símbolo del sistema muestra la misma línea que se mostró cuando se la abrió por primera vez. Esto indica que `sqlcmd` ha finalizado la sesión. Ahora ya puede cerrar la ventana del símbolo del sistema escribiendo otro comando `EXIT` .  
  
## <a name="running-transact-sql-script-files-using-sqlcmd"></a>Ejecutar archivos de script Transact-SQL mediante sqlcmd  
 Puede usar **sqlcmd** para ejecutar los archivos de script de base de datos. Los archivos de script son archivos de texto que contienen una combinación de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] , comandos de **sqlcmd** y variables de scripting. Para obtener más información sobre cómo incluir variables en scripts, vea [Usar sqlcmd con variables de script](./sqlcmd-use-with-scripting-variables.md). **sqlcmd** funciona con las instrucciones, los comandos y las variables de scripting en un archivo de script de una forma parecida a como opera con instrucciones y comandos indicados de forma interactiva. La diferencia principal es que **sqlcmd** lee el archivo de entrada sin pausas, en lugar de esperar a que un usuario indique las instrucciones, los comandos y las variables de scripting.  
  
 Hay distintas maneras de crear archivos de script de base de datos:  
  
-   Puede generar y depurar de forma interactiva un conjunto de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y, luego, guardar el contenido de la ventana Consulta como archivo de script.  
  
-   Puede crear un archivo de texto que contenga instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] usando un editor de texto, como el Bloc de notas.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-running-a-script-by-using-sqlcmd"></a>A. Ejecutar un script con sqlcmd  
 Inicie el Bloc de notas y escriba las siguientes instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 `SELECT TOP (3) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 Cree una carpeta llamada `MyFolder` y guarde el script como el archivo `MyScript.sql` en la carpeta `C:\MyFolder`. Escriba lo siguiente en el símbolo del sistema para ejecutar el script y coloque la salida en `MyOutput.txt` de `MyFolder`:  
  
 `sqlcmd -i C:\MyFolder\MyScript.sql -o C:\MyFolder\MyOutput.txt`  
  
 Al mostrar el contenido de `MyOutput.txt` en el Bloc de notas, verá lo siguiente:  
  
 `Changed database context to 'AdventureWorks2012'.`  
  
 `BusinessEntityID FirstName   LastName`  
  
 `---------------- ----------- -----------`  
  
 `1                Syed        Abbas`  
  
 `2                Catherine   Abel`  
  
 `3                Kim         Abercrombie`  
  
 `(3 rows affected)`  
  
### <a name="b-using-sqlcmd-with-a-dedicated-administrative-connection"></a>B. Usar sqlcmd con una conexión administrativa dedicada  
 En el siguiente ejemplo, se utiliza `sqlcmd` para conectarse a un servidor que tiene un problema de bloqueo mediante la conexión de administrador dedicada (DAC).  
  
 `C:\>sqlcmd -S ServerName -A`  
  
 `1> SELECT session_id, blocking_session_id FROM sys.dm_exec_requests WHERE blocking_session_id <> 0;`  
  
 `2> GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `session_id   blocking_session_id`  
  
 `------ -------`  
  
 `62       64`  
  
 `(1 rows affected)`  
  
 Utilice `sqlcmd` para finalizar el proceso de bloqueo.  
  
 `1> KILL 64;`  
  
 `2> GO`  
  
### <a name="c-using-sqlcmd-to-execute-a-stored-procedure"></a>C. Usar sqlcmd para ejecutar un procedimiento almacenado  
 En el ejemplo siguiente se muestra cómo ejecutar un procedimiento almacenado con `sqlcmd`. Cree el siguiente procedimiento almacenado.  
  
 `USE AdventureWorks2012;`  
  
 `IF OBJECT_ID ( ' dbo.ContactEmailAddress, 'P' ) IS NOT NULL`  
  
 `DROP PROCEDURE dbo.ContactEmailAddress;`  
  
 `GO`  
  
 `CREATE PROCEDURE dbo.ContactEmailAddress`  
  
 `(`  
  
 `@FirstName nvarchar(50)`  
  
 `,@LastName nvarchar(50)`  
  
 `)`  
  
 `AS`  
  
 `SET NOCOUNT ON`  
  
 `SELECT EmailAddress`  
  
 `FROM Person.Person`  
  
 `WHERE FirstName = @FirstName`  
  
 `AND LastName = @LastName;`  
  
 `SET NOCOUNT OFF`  
  
 En el símbolo del sistema `sqlcmd` , escriba lo siguiente:  
  
 `C:\sqlcmd`  
  
 `1> :Setvar FirstName Gustavo`  
  
 `1> :Setvar LastName Achong`  
  
 `1> EXEC dbo.ContactEmailAddress $(FirstName),$(LastName)`  
  
 `2> GO`  
  
 `EmailAddress`  
  
 `-----------------------------`  
  
 `gustavo0@adventure-works.com`  
  
### <a name="d-using-sqlcmd-for-database-maintenance"></a>D. Usar sqlcmd para mantenimiento de la base de datos  
 El siguiente ejemplo muestra cómo utilizar `sqlcmd` para una tarea de mantenimiento de base de datos. Cree `C:\BackupTemplate.sql` con el siguiente código.  
  
 `USE master;`  
  
 `BACKUP DATABASE [$(db)] TO DISK='$(bakfile)';`  
  
 En el símbolo del sistema `sqlcmd` , escriba lo siguiente:  
  
 `C:\ >sqlcmd`  
  
 `1> :connect <server>`  
  
 `Sqlcmd: Successfully connected to server <server>.`  
  
 `1> :setvar db msdb`  
  
 `1> :setvar bakfile c:\msdb.bak`  
  
 `1> :r c:\BackupTemplate.sql`  
  
 `2> GO`  
  
 `Changed database context to 'master'.`  
  
 `Processed 688 pages for database 'msdb', file 'MSDBData' on file 2.`  
  
 `Processed 5 pages for database 'msdb', file 'MSDBLog' on file 2.`  
  
 `BACKUP DATABASE successfully processed 693 pages in 0.725 seconds (7.830 MB/sec)`  
  
### <a name="e-using-sqlcmd-to-execute-code-on-multiple-instances"></a>E. Usar sqlcmd para ejecutar código en múltiples instancias  
 El siguiente código en un archivo muestra un script que se conecta a dos instancias. Observe el comando `GO` antes de la conexión a la segunda instancia.  
  
 `:CONNECT <server>\,<instance1>`  
  
 `EXEC dbo.SomeProcedure`  
  
 `GO`  
  
 `:CONNECT <server>\,<instance2>`  
  
 `EXEC dbo.SomeProcedure`  
  
 `GO`  
  
### <a name="e-returning-xml-output"></a>E. Devolver salida XML  
 El siguiente ejemplo muestra cómo se devuelve la salida XML sin formato, en un flujo continuo.  
  
 `C:\>sqlcmd -d AdventureWorks2012`  
  
 `1> :XML ON`  
  
 `1> SELECT TOP 3 FirstName + ' ' + LastName + ', '`  
  
 `2> FROM Person.Person`  
  
 `3> GO`  
  
 `Syed Abbas, Catherine Abel, Kim Abercrombie,`  
  
### <a name="f-using-sqlcmd-in-a-windows-script-file"></a>F. Usar sqlcmd en un archivo de script de Windows  
 Un comando **sqlcmd** como `sqlcmd -i C:\InputFile.txt -o C:\OutputFile.txt,` se puede ejecutar en un archivo .bat junto con VBScript. En este caso, no se deben usar opciones interactivas. **sqlcmd** se debe instalar en el equipo que ejecuta el archivo .bat.  
  
 Primero, cree los siguientes cuatro archivos:  
  
-   C:\badscript.sql  
  
    ```  
    SELECT batch_1_this_is_an_error  
    GO  
    SELECT 'batch #2'  
    GO  
    ```  
  
-   C:\goodscript.sql  
  
    ```  
    SELECT 'batch #1'  
    GO  
    SELECT 'batch #2'  
    GO  
    ```  
  
-   C:\returnvalue.sql  
  
    ```  
    :exit(select 100)  
    ```  
  
-   C:\windowsscript.bat  
  
    ```  
    @echo off  
  
    echo Running badscript.sql  
    sqlcmd -i badscript.sql -b -o out.log  
    if not errorlevel 1 goto next1  
    echo == An error occurred   
  
    :next1  
  
    echo Running goodscript.sql  
    sqlcmd -i goodscript.sql -b -o out.log  
    if not errorlevel 1 goto next2  
    echo == An error occurred   
  
    :next2  
    echo Running returnvalue.sql  
    sqlcmd -i returnvalue.sql -o out.log  
    echo SQLCMD returned %errorlevel% to the command shell  
  
    :exit  
    ```  
  
 Después, en el símbolo del sistema, ejecute `C:\windowsscript.bat`:  
  
 `C:\>windowsscript.bat`  
  
 `Running badscript.sql`  
  
 `== An error occurred`  
  
 `Running goodscript.sql`  
  
 `Running returnvalue.sql`  
  
 `SQLCMD returned 100 to the command shell`  
  
### <a name="g-using-sqlcmd-to-set-encryption-on-azure-sql-database"></a>G. Usar sqlcmd para establecer el cifrado en Azure SQL Database  
 Se puede ejecutar un comando **sqlcmd** en una conexión con datos de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] para especificar el cifrado y la confianza del certificado. Hay dos opciones **sqlcmd** disponibles:  
  
-   El modificador -N lo usa el cliente para solicitar una conexión cifrada. Esta opción es equivalente a ADO.net `ENCRYPT = true`.  
  
-   El modificador -C lo emplea el cliente para configurarlo implícitamente para el certificado de servidor confiable y no validarlo. Esta opción es equivalente a ADO.net `TRUSTSERVERCERTIFICATE = true`.  
  
 El servicio [!INCLUDE[ssSDS](../../includes/sssds-md.md)] no admite todas las opciones disponibles `SET` en una instancia de SQL Server. Las siguientes opciones producen un error cuando la opción `SET` correspondiente está establecida en `ON` u `OFF`:  
  
-   SET ANSI_DEFAULTS  
  
-   SET ANSI_NULLS  
  
-   SET REMOTE_PROC_TRANSACTIONS  
  
-   SET ANSI_NULL_DEFAULT  
  
 Las siguientes opciones de SET no producen excepciones pero no se pueden usar. Se han quedado en desuso:  
  
-   SET CONCAT_NULL_YIELDS_NULL  
  
-   SET ANSI_PADDING  
  
-   SET QUERY_GOVERNOR_COST_LIMIT  
  
#### <a name="syntax"></a>Sintaxis  
 En los siguientes ejemplos se hace referencia a los casos donde la configuración del proveedor Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye: `ForceProtocolEncryption = False`, `Trust Server Certificate = No`  
  
 Conecte usando las credenciales de Windows y cifre la comunicación:  
  
```  
SQLCMD -E -N  
  
```  
  
 Conecte usando las credenciales de Windows y el certificado de servidor de confianza:  
  
```  
SQLCMD -E -C  
  
```  
  
 Conecte usando las credenciales de Windows, comunicación cifrada y el certificado de servidor de confianza:  
  
```  
SQLCMD -E -N -C  
  
```  
  
 En los siguientes ejemplos se hace referencia a los casos donde la configuración del proveedor Native Client [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye: `ForceProtocolEncryption = True`, `TrustServerCertificate = Yes`.  
  
 Conecte usando las credenciales de Windows, comunicación cifrada y el certificado de servidor de confianza:  
  
```  
SQLCMD -E  
  
```  
  
 Conecte usando las credenciales de Windows, comunicación cifrada y el certificado de servidor de confianza:  
  
```  
SQLCMD -E -N  
  
```  
  
 Conecte usando las credenciales de Windows, comunicación cifrada y el certificado de servidor de confianza:  
  
```  
SQLCMD -E -C  
  
```  
  
 Conecte usando las credenciales de Windows, comunicación cifrada y el certificado de servidor de confianza:  
  
```  
SQLCMD -E -N -C  
  
```  
  
 Si el proveedor especifica `ForceProtocolEncryption = True` se habilita el cifrado aun cuando `Encrypt=No` en la cadena de conexión.  
  
## <a name="more-about-sqlcmd"></a>Más información sobre sqlcmd  
 [sqlcmd (utilidad)](../../tools/sqlcmd-utility.md)   
 [Usar sqlcmd con variables de script](./sqlcmd-use-with-scripting-variables.md)   
 [Modificar scripts SQLCMD con el Editor de consultas](./edit-sqlcmd-scripts-with-query-editor.md)   
 [Administrar pasos de trabajo](../../ssms/agent/manage-job-steps.md)   
 [Crear un paso de trabajo CmdExec](../../ssms/agent/create-a-cmdexec-job-step.md)  
  
