---
title: bcp (utilidad)
description: La utilidad del programa de copia masiva (bcp) copia datos de forma masiva entre una instancia de SQL Server y un archivo de datos en un formato especificado por el usuario.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server]
- exporting data
- row exporting [SQL Server]
- copying data [SQL Server], bcp utility
- command prompt utilities [SQL Server], bcp
- tables [SQL Server], importing data
- column importing [SQL Server]
- bcp utility [SQL Server], command options
- file exporting [SQL Server]
- bulk copy [SQL Server]
- bcp utility [SQL Server], about bcp utility
- tables [SQL Server], exporting data
- row importing [SQL Server]
- importing data, bcp utility
- file importing [SQL Server]
- column exporting [SQL Server]
ms.assetid: c0af54f5-ca4a-4995-a3a4-0ce39c30ec38
author: markingmyname
ms.author: maghan
ms.reviewer: v-daenge
ms.custom: seo-lt-2019
ms.date: 09/11/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 9c732f7be3a25752c16e8e09e483097c8b9cac18
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536416"
---
# <a name="bcp-utility"></a>bcp (utilidad)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> Para usar bcp en Linux, consulte la página sobre [cómo instalar sqlcmd y bcp en Linux](../linux/sql-server-linux-setup-tools.md).
>
> Para información detallada sobre cómo usar BCP con Azure Synapse Analytics, vea [Carga de datos con BCP](/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp).

La utilidad de **p** rograma **d** e **p** copia masiva (**bcp**) hace copias masivas de los datos entre una instancia de [!INCLUDE[msCoName](../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y un archivo de datos en un formato especificado por el usuario. La utilidad **bcp** se puede usar para importar un número elevado de filas nuevas en tablas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o para exportar datos de tablas a archivos de datos. Excepto cuando se usa con la opción **queryout** , la utilidad no requiere ningún conocimiento de [!INCLUDE[tsql](../includes/tsql-md.md)]. Para importar datos en una tabla, debe usar un archivo de formato creado para esa tabla o comprender la estructura de la tabla y los tipos de datos que son válidos para sus columnas.  

![Icono de vínculo de tema](../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") Para conocer las convenciones que se emplean en la sintaxis de **bcp**, consulte [Convenciones de sintaxis Transact-SQL &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  

> [!NOTE]
> Si usa **bcp** para hacer una copia de seguridad de los datos, cree un archivo de formato para registrar el formato de datos. Los archivos de datos de **bcp** **no incluyen ningún** esquema ni información de formato, de modo que si se quita una tabla o vista, y no tiene un archivo de formato, es posible que no pueda importar los datos.

## <a name="download-the-latest-version-of-bcp-utility"></a>Descarga de la versión más reciente de la utilidad bcp

**[![Descargar bcp para x64](../ssdt/media/download.png) Descargar utilidades de la línea de comandos 15 de Microsoft para SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2142258)**
<br>**[![Descargar bcp para x86](../ssdt/media/download.png) Descargar utilidades de la línea de comandos 15 de Microsoft para SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2142257)**

Las herramientas de la línea de comandos se encuentran en fase de disponibilidad general (GA), pero se lanzan con el paquete del instalador de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

### <a name="version-information"></a>Información de versión

Número de versión: 15.0.2 <br>
Número de compilación: 15.0.2000.5<br>
Fecha de lanzamiento: 11 de septiembre de 2020

La nueva versión de SQLCMD admite la autenticación de Azure AD, incluida la compatibilidad de Multi-Factor Authentication (MFA) con las características SQL Database, Azure Synapse Analytics y Always Encrypted.
La nueva versión de BCP admite la autenticación de Azure AD, incluida la compatibilidad de Multi-Factor Authentication (MFA) con SQL Database y Azure Synapse Analytics.

### <a name="system-requirements"></a>Requisitos del sistema

Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008, Windows Server 2008 R2, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019

Este componente requiere [Windows Installer 4.5](https://windows-installer.soft32.com/) y [Microsoft ODBC Driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md).

Para comprobar la versión de BCP, ejecute el comando `bcp /v` y confirme que 15.0.2000.5 o posterior está en uso.

<table><th>Sintaxis</th><tr><td><pre>
bcp [<a href="#db_name">database_name.</a>] <a href="#schema">schema</a>.{<a href="#tbl_name">table_name</a> | <a href="#vw_name">view_name</a> | <a href="#query">"query"</a>}
    {<a href="#in">in</a> <a href="#data_file">data_file</a> | <a href="#out">out</a> <a href="#data_file">data_file</a> | <a href="#qry_out">queryout</a> <a href="#data_file">data_file</a> | <a href="#format">format</a> <a href="#format">nul</a>}
<a>                                                                                                         </a>
    [<a href="#a">-a packet_size</a>]
    [<a href="#b">-b batch_size</a>]
    [<a href="#c">-c</a>]
    [<a href="#C">-C { ACP | OEM | RAW | code_page } </a>]
    [<a href="#d">-d database_name</a>]
    [<a href="#D">-D</a>]
    [<a href="#e">-e err_file</a>]
    [<a href="#E">-E</a>]
    [<a href="#f">-f format_file</a>]
    [<a href="#F">-F first_row</a>]
    [<a href="#G">-G Azure Active Directory Authentication</a>]
    [<a href="#h">-h"hint [,...n]"</a>]
    [<a href="#i">-i input_file</a>]
    [<a href="#k">-k</a>]
    [<a href="#K">-K application_intent</a>]
    [<a href="#l">-l login_timeout</a>]
    [<a href="#L">-L last_row</a>]
    [<a href="#m">-m max_errors</a>]
    [<a href="#n">-n</a>]
    [<a href="#N">-N</a>]
    [<a href="#o">-o output_file</a>]
    [<a href="#P">-P password</a>]
    [<a href="#q">-q</a>]
    [<a href="#r">-r row_term</a>]
    [<a href="#R">-R</a>]
    [<a href="#S">-S [server_name[\instance_name]</a>]
    [<a href="#t">-t field_term</a>]
    [<a href="#T">-T</a>]
    [<a href="#U">-U login_id</a>]
    [<a href="#v">-v</a>]
    [<a href="#V">-V (80 | 90 | 100 | 110 | 120 | 130 ) </a>]
    [<a href="#w">-w</a>]
    [<a href="#x">-x</a>]
</pre></td></tr></table>

## <a name="arguments"></a>Argumentos

 _**data\_file**_<a name="data_file"></a>  
 Es la ruta completa del archivo de datos. Cuando se importan datos de forma masiva a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], el archivo de datos contiene los datos que se van a copiar en la tabla o vista especificada. Cuando se realiza una exportación de datos de forma masiva desde [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], el archivo de datos contiene los datos copiados desde la tabla o desde la vista. La ruta de acceso puede contener de 1 a 255 caracteres. El archivo de datos puede contener 2^63 - 1 filas, como máximo.  

 _**database\_name**_<a name="db_name"></a>  
 Es el nombre de la base de datos en la que reside la tabla o vista especificada. Si no se especifica, es la base de datos predeterminada para el usuario.  

 También puede especificar explícitamente el nombre de la base de datos con **d-** .  

 **in** *data_file* \| **out** *data_file* \| **queryout** *data_file* \| **format nul**  
 Especifica la dirección de la copia masiva, de la siguiente manera:  
  
- **in**<a name="in"></a> copia desde un archivo en la vista o la tabla de la base de datos.  
  
- **out**<a name="out"></a> copia desde la tabla o la vista de la base de datos a un archivo. Si se especifica un archivo ya existente, este se sobrescribe. Al extraer datos, la utilidad **bcp** representa una cadena vacía como nula y una cadena nula como vacía.  
  
- **queryout**<a name="qry_out"></a> copia desde una consulta y debe especificarse solo cuando se copian datos de forma masiva desde una consulta.  
  
- **format**<a name="format"></a> crea un archivo de formato basado en la opción especificada ( **-n**, **-c**, **-w** o **-N**) y los delimitadores de la vista o de la tabla. Cuando se copian datos de forma masiva, el comando **bcp** puede hacer referencia a un archivo de formato, lo que evita tener que especificar de nuevo la información de formato interactivamente. La opción **format** necesita la opción **-f** ; la creación de un archivo de formato XML también requiere la opción **-x** . Para obtener más información, vea [Crear un archivo de formato &#40;SQL Server&#41;](../relational-databases/import-export/create-a-format-file-sql-server.md). Hay que especificar **nul** como el valor (**format nul**).  
  
 _**owner**_<a name="schema"></a>  
 Es el nombre del propietario de la tabla o vista. *owner* es opcional si el usuario que realiza la operación es propietario de la tabla o vista especificada. Si *owner* no se especifica y el usuario que realiza la acción no es el propietario de la tabla o la vista especificada, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] devuelve un mensaje de error y se cancela la operación.  
  
**"** _**query**_ **"** <a name="query"></a> Es una consulta [!INCLUDE[tsql](../includes/tsql-md.md)] que devuelve un conjunto de resultados. Si la consulta devuelve múltiples conjuntos de resultados, solo se copiará el primero en el archivo de datos; los conjuntos de resultados siguientes se omitirán. Utilice comillas dobles para la consulta y comillas simples en los elementos que incruste en la consulta. **queryout** también se debe especificar cuando se copian datos desde una consulta.  
  
 La consulta puede hacer referencia a un procedimiento almacenado siempre que todas las tablas a las que se haga referencia dentro del procedimiento almacenado existan antes de ejecutar la instrucción bcp. Por ejemplo, si el procedimiento almacenado genera una tabla temporal, se produce un error en la instrucción **bcp** porque la tabla temporal solamente está disponible en tiempo de ejecución y no cuando se ejecuta la instrucción. En este caso, considere la posibilidad de insertar los resultados del procedimiento almacenado en una tabla y, después, usar **bcp** para copiar los datos de la tabla en un archivo de datos.  
  
 _**table\_name**_<a name="tbl_name"></a>  
 Es el nombre de la tabla de destino cuando se importan datos a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**in**) y la tabla de origen cuando se exportan datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**out**).  
  
 _**view\_name**_<a name="vw_name"></a>   
 Es el nombre de la vista de destino cuando se copian datos en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**in**) y la vista de origen cuando se copian datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**out**). Solamente pueden usarse como vistas de destino aquellas vistas en las que todas las columnas hacen referencia a la misma tabla. Para obtener más información sobre las restricciones para copiar datos en vistas, vea [INSERT &#40;Transact-SQL&#41;](../t-sql/statements/insert-transact-sql.md).  
  
 **-a** _**packet\_size**_<a name="a"></a>  
 Especifica el número de bytes por paquete de red enviados y recibidos por el servidor. Se puede establecer una opción de configuración de servidor con [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (o el procedimiento almacenado del sistema **sp_configure** ). No obstante, la opción de configuración de servidor puede sustituirse individualmente mediante esta opción. *packet_size* puede oscilar entre 4096 y 65535; el valor predeterminado es 4096.  
  
 Un tamaño mayor de los paquetes puede mejorar el rendimiento de las operaciones de copia masiva. Si se pide un tamaño de paquete mayor, pero no puede concederse, se usa el valor predeterminado. Las estadísticas de rendimiento generadas por la utilidad **bcp** muestran el tamaño del paquete usado.  
  
 **-b** _**batch\_size**_<a name="b"></a>  
 Especifica el número de filas por lote de datos importados. Cada lote se importa y registra como una transacción aparte que importa el lote entero antes de confirmarse. De forma predeterminada, todas las filas del archivo de datos se importan en un solo lote. Para distribuir las filas en varios lotes, especifique un valor de *batch_size* inferior al número de filas del archivo de datos. Si se produce un error en la transacción de un lote, solamente se revierten las inserciones del lote actual. Los lotes importados por transacciones confirmadas no se ven afectados por los errores posteriores.  
  
 No use esta opción junto con la opción **-h "** ROWS_PER_BATCH **=** _bb_ **"** .  
 
 **-c**<a name="c"></a>  
 Realiza la operación con un tipo de datos de caracteres. Esta opción no realiza una petición para cada campo; usa **char** como tipo de almacenamiento, sin prefijos y con **\t** (carácter de tabulación) como separador de campos y **\r\n** (carácter de nueva línea) como terminador de filas. **-c** no es compatible con **-w**.  
  
 Para obtener más información, vea [Usar el formato de caracteres para importar o exportar datos &#40;SQL Server&#41;](../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).  
  
 **-C** { **ACP** \| **OEM** \| **RAW** \| *code_page* }<a name="C"></a>   
 Especifica la página de códigos de los datos incluidos en el archivo de datos. *code_page* solo es pertinente si los datos contienen columnas de tipo **char**, **varchar** o **text** con valores de caracteres mayores que 127 o menores que 32.  
  
> [!NOTE]
> Se recomienda especificar un nombre de intercalación para cada columna en un archivo de formato, excepto cuando quiera que la opción 65001 tenga prioridad sobre la especificación de la página de códigos o la intercalación.
  
|Valor de página de códigos|Descripción|  
|---------------------|-----------------|  
|ACP|[!INCLUDE[vcpransi](../includes/vcpransi-md.md)]/Microsoft Windows (ISO 1252).|  
|OEM|Página de códigos predeterminada, utilizada por el cliente. Esta es la página de códigos que se usa de forma predeterminada si no se especifica **-C** .|  
|RAW|No se realiza ninguna conversión entre páginas de códigos. Se trata de la opción más rápida porque no se producen conversiones.|  
|*code_page*|Número específico de una página de códigos, por ejemplo, 850.<br /><br /> Las versiones anteriores a la versión 13 ([!INCLUDE[sssql15-md](../includes/sssql16-md.md)]) no admiten la página de códigos 65001 (codificación UTF-8). Las versiones a partir de la versión 13 pueden importar codificación UTF-8 a versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|  
|||
  
 **-d** _**database\_name**_<a name="d"></a>   
 Especifica la base de datos a la que conectarse. De forma predeterminada, bcp.exe se conecta a la base de datos predeterminada del usuario. Si se especifican -d database_name y un nombre de tres partes (database_name.schema.table, pasado como el primer parámetro a bcp.exe), se producirá un error porque no es posible especificar dos veces el nombre de la base de datos. Si *database_name* comienza con un guion (-) o una barra diagonal (/), no agregue un espacio entre **-d** y el nombre de la base de datos.  

**-D**<a name="D"></a>  
Hace que el valor transmitido a la opción `bcp` `-S` se interprete como un nombre de origen de datos (DSN). Se puede usar un DSN para insertar opciones de controlador con el fin de simplificar las líneas de comandos, aplicar opciones de controlador a los que, de otro modo, no se podría acceder desde la línea de comandos, como MultiSubnetFailover, o para ayudar a proteger las credenciales confidenciales para que no se puedan detectar como argumentos de la línea de comandos. Para más información, consulte la sección _Compatibilidad con DSN en sqlcmd y bcp_ de [Conexión con sqlcmd](../connect/odbc/linux-mac/connecting-with-sqlcmd.md).


 **-e** _**err\_file**_<a name="e"></a>  
 Especifica la ruta de acceso completa de un archivo de error que se usa para almacenar las filas que la utilidad **bcp** no puede transferir del archivo a la base de datos. Los mensajes de error del comando **bcp** van a la estación de trabajo del usuario. Si no se usa esta opción, no se creará el archivo de errores.  
  
 Si *err_file* comienza con un guión (-) o una barra diagonal (/), no incluya un espacio entre **-e** y el valor de *err_file* .  
  
**-E**<a name="E"></a>

Especifica que se usará el valor o valores de identidad del archivo de datos importado para la columna de identidad. Si no se especifica **-E** , se omiten los valores de identidad de esta columna en el archivo de datos que se importa y [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] asigna automáticamente valores únicos basados en los valores de inicialización y de incremento especificados durante la creación de la tabla.  Para más información, consulte [DBCC CHECKDB](../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).
  
 Si el archivo de datos no contiene valores para la columna de identidad de la tabla o la vista, use un archivo de formato para especificar que se debe omitir la columna de identidad de la tabla o la vista al importar los datos; [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] asigna automáticamente valores únicos para la columna.
  
 La opción **-E** tiene un requisito de permisos especial. Para más información, consulte la sección "[Comentarios](#remarks)" que aparece más adelante en este tema.  
   
 **-f** _**format\_file**_<a name="f"></a>  
 Especifica la ruta de acceso completa de un archivo de formato. El significado de esta opción depende del entorno en el que se utiliza, como se indica a continuación:  
  
- Si se usa **-f** con la opción **format** , se crea el archivo *format_file* especificado para la tabla o la vista especificada. Para crear un archivo de formato XML, especifique también la opción **-x** . Para obtener más información, vea [Crear un archivo de formato &#40;SQL Server&#41;](../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
- Si se usa con las opciones **in** u **out** , **-f** requiere un archivo de formato existente.  
  
    > [!NOTE]
    > El uso de un archivo de formato con la opción **in** o **out** es opcional. En ausencia de la opción **-f** , si no se especifica **-n**, **-c**, **-w** o **-N** , el comando solicita información de formato y permite guardar las respuestas en un archivo de formato (cuyo nombre de archivo predeterminado es Bcp.fmt).
  
 Si *format_file* comienza por un guión (-) o una barra diagonal (/), no incluya un espacio entre **-f** y el valor de *format_file* .  
  
**-F** _**first\_row**_<a name="F"></a>  
 Especifica el número de la primera fila que se exportará desde una tabla o que se importará desde un archivo de datos. Este parámetro requiere un valor superior a (>) 0 pero inferior a (<) o igual que (=) el número total de filas. En ausencia de este parámetro, el valor predeterminado es la primera fila del archivo.  
  
 *first_row* puede ser un valor entero positivo hasta 2^63-1. **-F** *first_row* es de base 1.  

**-G**<a name="G"></a>

 El cliente usa este modificador al conectarse a Azure SQL Database o a Azure Synapse Analytics para especificar que el usuario se autentica mediante la autenticación de Azure Active Directory. El modificador -G requiere la [versión 14.0.3008.27 o versiones posteriores](https://go.microsoft.com/fwlink/?LinkID=825643). Para determinar su versión, ejecute bcp -v. Para más información, vea [Uso de la autenticación de Azure Active Directory para autenticación con SQL Database o Azure Synapse Analytics](/azure/sql-database/sql-database-aad-authentication). 

> [!IMPORTANT]
> La opción **-G** solo es válida para Base de datos SQL de Azure y el almacenamiento de datos de Azure.
> La autenticación integrada de AAD y la autenticación interactiva no se admiten actualmente ni en Linux ni en macOS.

> [!TIP]
> Para comprobar si la versión de bcp incluye compatibilidad con la autenticación de Azure Active Directory (AAD), escriba **bcp --** (bcp\<space>\<dash>\<dash>) y compruebe que aparece -G en la lista de argumentos disponibles.

- **Nombre de usuario y contraseña de Azure Active Directory:** 

    Si desea utilizar un nombre de usuario y una contraseña de Azure Active Directory, puede proporcionar la opción **- G** y usar también el nombre de usuario y la contraseña proporcionando las opciones **- U** y **-P** . 

    En el ejemplo siguiente se exportan datos mediante el nombre de usuario y la contraseña de Azure AD, donde el usuario y la contraseña es una credencial de AAD. En el ejemplo se exporta la tabla `bcptest` desde la base de datos `testdb` desde el servidor de Azure `aadserver.database.windows.net` y se almacenan los datos en el archivo `c:\last\data1.dat`:

    ```cmd
    bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com -P xxxxx
    ```

    En el ejemplo siguiente se importan datos mediante el nombre de usuario y la contraseña de Azure AD, donde el usuario y la contraseña es una credencial de AAD. En el ejemplo se importan datos desde el archivo `c:\last\data1.dat` en la tabla `bcptest` para la base de datos `testdb` en el servidor de Azure `aadserver.database.windows.net` con el usuario y la contraseña de Azure AD:

    ```cmd
    bcp bcptest in "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com -P xxxxx
    ```

- **Autenticación integrada de Azure Active Directory**

    Para autenticación integrada de Azure Active Directory, proporcione la opción **-G** sin un nombre de usuario o contraseña. Esta configuración supone que la cuenta de usuario de Windows actual (la cuenta en la que se ejecuta el comando bcp) está federada con Azure AD:

    En el ejemplo siguiente se exportan datos mediante la cuenta de Azure AD integrado. En el ejemplo se exporta la tabla `bcptest` desde la base de datos `testdb` con Azure AD integrado desde el servidor de Azure `aadserver.database.windows.net` y los datos se almacenan en el archivo `c:\last\data2.dat`:

    ```cmd
    bcp bcptest out "c:\last\data2.dat" -S aadserver.database.windows.net -d testdb -G -c -t
    ```

    En el ejemplo siguiente se importan datos mediante la autenticación de Azure AD integrado. En el ejemplo se importan datos desde el archivo `c:\last\data2.txt` en la tabla `bcptest` para la base de datos `testdb` en el servidor de Azure `aadserver.database.windows.net` con la autenticación de Azure AD integrado:

    ```cmd
    bcp bcptest in "c:\last\data2.dat" -S aadserver.database.windows.net -d testdb -G -c -t
    ```

- **Autenticación interactiva de Azure Active Directory**  

   La autenticación interactiva de Azure AD para Azure SQL Database y Azure Synapse Analytics permite usar un método interactivo que admite la autenticación multifactor. Para más información, consulte [Autenticación interactiva de Active Directory](../ssdt/azure-active-directory.md#active-directory-interactive-authentication).

   La autenticación interactiva de Azure AD requiere **bcp** [versión 15.0.1000.34](#download-the-latest-version-of-bcp-utility) o posterior, así como [ODBC versión 17.2 o posterior](../connect/odbc/download-odbc-driver-for-sql-server.md).  

   Para habilitar la autenticación interactiva, indique la opción -G solo con el nombre de usuario (-U), sin contraseña.

   En el siguiente ejemplo se exportan datos mediante el modo interactivo de Azure AD que indica el nombre de usuario, donde el usuario representa una cuenta de AAD. Este es el mismo ejemplo que se utilizó en la sección anterior: *Nombre de usuario y contraseña de Azure Active Directory*.  

   El modo interactivo requiere que se especifique una contraseña manualmente, o en el caso de las cuentas que tengan la autenticación multifactor habilitada, complete el método de autenticación MFA configurado.

   ```cmd
   bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com
   ```

   En caso de que un usuario de Azure AD sea un usuario federado de dominio con una cuenta de Windows, el nombre de usuario que se requiera en la línea de comandos contendrá su cuenta de dominio (por ejemplo, joe@contoso.com; consulte a continuación):

   ```cmd
   bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U joe@contoso.com
   ```

   Si los usuarios invitados ya se encuentran en un servicio Azure AD específico y forman parte de un grupo que existe en la base de datos de SQL que tiene permisos de base de datos para ejecutar el comando bcp, se usa su alias de usuario invitado (por ejemplo, *keith0@adventureworks.com* ).
  
**-h**_**"load hints**_[ ,... *n*] **"** <a name="h"></a> Especifica las sugerencias que se deben usar durante una importación masiva de datos en una tabla o una vista.  
  
* **ORDER**(**_column_[ASC | DESC] [** , **..._n_])**  
Indica el criterio de ordenación de los datos en el archivo de datos. El rendimiento de la importación masiva mejora si los datos importados se ordenan según el índice clúster de la tabla, si lo hay. Si el archivo de datos se ordena siguiendo otro criterio que no sea el orden de una clave de índice clúster, o si no hay ningún índice clúster en la tabla, la cláusula ORDER se pasa por alto. Los nombres de columna facilitados deben ser nombres válidos en la tabla de destino. De forma predeterminada, **bcp** supone que el archivo de datos no está ordenado. En las importaciones masivas optimizadas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] también valida que los datos importados estén ordenados.  
  
* **ROWS_PER_BATCH** **=** _**bb**_  
Número de filas de datos por lote (como *bb*). Se usa cuando no se especifica **-b** , por lo que el archivo de datos completo se envía al servidor en una sola transacción. El servidor optimiza la carga masiva según el valor *bb*. De forma predeterminada, el valor de ROWS_PER_BATCH es desconocido.  
  
* **KILOBYTES_PER_BATCH** **=** _**cc**_  
Número aproximado de kilobytes (KB) de datos por lote (igual que *cc*). De forma predeterminada, el valor de KILOBYTES_PER_BATCH es desconocido.  
  
* **TABLOCK**  
Especifica que se adquiera un bloqueo de tabla de actualización masiva para la duración de la operación de carga masiva; de lo contrario, se adquiere un bloqueo de fila. Esta sugerencia mejora notablemente el rendimiento, dado que, al mantenerse el bloqueo durante la operación de copia masiva, se reduce la contención en la tabla por bloqueo. Varios clientes pueden cargar una tabla simultáneamente si esta no tiene índices y se especifica **TABLOCK** . De forma predeterminada, el comportamiento del bloqueo viene determinado por la opción de tabla **table lock on bulkload**.  
  
  > [!NOTE]
  > Si la tabla de destino es el índice columnstore agrupado, la sugerencia TABLOCK no es necesaria para cargar mediante varios clientes simultáneos porque cada subproceso simultáneo se asigna a un grupo de filas independiente en el índice y en los datos de cargas. Para más detalles, consulte los temas conceptuales del índice de almacén de columnas.
  
  **CHECK_CONSTRAINTS**  
  Especifica que deben comprobarse todas las restricciones de la tabla o vista de destino durante la operación de importación masiva. Sin la sugerencia CHECK_CONSTRAINTS, se omiten las restricciones CHECK y FOREIGN KEY y, después de la operación, la restricción sobre la tabla se marca como de no confianza.  
  
  > [!NOTE]
  > Las restricciones UNIQUE, PRIMARY KEY y NOT NULL se aplican siempre.
  
  En algún momento deberá comprobar las restricciones de toda la tabla. Si la tabla no estaba vacía antes de la operación de importación masiva, el costo de revalidar la restricción puede exceder el costo de aplicar restricciones CHECK a los datos incrementales. Por lo tanto, se recomienda que se habilite normalmente la comprobación de restricciones durante una importación incremental masiva.  
  
  Una situación en la que quizá desee que las restricciones estén deshabilitadas (comportamiento predeterminado) es si los datos de entrada contienen filas que infringen las restricciones. Con las restricciones CHECK deshabilitadas, puede importar los datos y después usar instrucciones de [!INCLUDE[tsql](../includes/tsql-md.md)] para quitar los datos que no son válidos.  
  
  > [!NOTE]
  > **bcp** valida y comprueba ahora los datos, y ello podría dar lugar a errores en los scripts si se ejecutan con datos no válidos de un archivo.

  > [!NOTE]
  > El modificador **-m** *max_errors* no se aplica a la comprobación de restricciones.
  
* **FIRE_TRIGGERS**  
Cuando se especifica con el argumento **in** , se ejecutarán todos los desencadenadores de inserción definidos en la tabla de destino durante la operación de copia masiva. Si no se especifica FIRE_TRIGGERS, no se ejecutará ningún desencadenador. FIRE_TRIGGERS se ignora para los argumentos **out**, **queryout** y **format** .  
  
**-i** _**input\_file**_<a name="i"></a>  
Especifica el nombre de un archivo de respuesta, que contiene respuestas a las preguntas del símbolo del sistema para cada campo de datos cuando se realiza una copia masiva con el modo interactivo (cuando no se especifica **-n**, **-c**, **-w** o **-N** ).  
  
Si *input_file* comienza por un guión (-) o una barra diagonal (/), no incluya un espacio entre **-i** y el valor de *input_file* .  
  
**-k**<a name="k"></a>  
Especifica que las columnas vacías deben conservar un valor NULL durante la operación, en vez de tener valores predeterminados para las columnas insertadas. Para obtener más información, vea [Mantener valores NULL o usar valores predeterminados durante la importación en bloque &#40;SQL Server&#41;](../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
**-K** _**application\_intent**_<a name="K"></a>   
Declara el tipo de carga de trabajo de la aplicación al conectarse a un servidor. El único valor que es posible es **ReadOnly**. Si no se especifica **-K** , la utilidad bcp no admitirá la conectividad con una réplica secundaria en el grupo de disponibilidad AlwaysOn. Para más información, consulte [Secundarias activas: réplicas secundarias legibles &#40;grupos de disponibilidad AlwaysOn&#41;](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
**-l** _**login\_timeout**_<a name="l"></a>  
Especifica un tiempo de espera de inicio de sesión. La opción -l especifica el número de segundos que tienen que transcurrir antes de que un inicio de sesión de SQL Server agote el tiempo de espera al intentar conectarse a un servidor. El tiempo de espera de inicio de sesión predeterminado es de 15 segundos. El período de tiempo de espera de inicio de sesión debe ser un número comprendido entre 0 y 65534. Si el valor proporcionado no es numérico o no está dentro de ese intervalo, bcp generará un mensaje de error. Un valor de 0 especifica un tiempo de espera infinito.
  
**-L** _**last\_row**_<a name="L"></a>  
Especifica el número de la última fila que se exportará desde una tabla o que se importará desde un archivo de datos. Este parámetro requiere un valor superior a (>) 0 pero inferior a (<) o igual al (=) el número de la última fila. En ausencia de este parámetro, el valor predeterminado es la última fila del archivo.  
  
*last_row* puede ser un valor entero positivo hasta 2^63-1.  
  
**-m** _**max\_errors**_<a name="m"></a>  
Especifica el número máximo de errores de sintaxis que pueden producirse antes de que se cancele la operación de **bcp** . Un error de sintaxis implica un error de conversión de datos en el tipo de datos de destino. El total de *max_errors* excluye cualquier error que pueda detectarse solamente en el servidor, como las infracciones de restricciones.  
  
 Una fila que no puede copiarse con la utilidad **bcp** se omite y se cuenta como un error. Si no se incluye esta opción, el valor predeterminado es 10.  
  
> [!NOTE]
> Además, la opción **-m** no es válida en la conversión de tipos de datos **money** o **bigint** .
  
**-n**<a name="n"></a>  
Realiza la operación de copia masiva con los tipos de datos nativos (base de datos) de los datos. Esta opción no efectúa una petición para cada campo, sino que usa los valores nativos.  
  
Para obtener más información, vea [Usar el formato nativo para importar o exportar datos &#40;SQL Server&#41;](../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).  
  
**-N**<a name="N"></a>  
Realiza la operación de copia masiva con los tipos de datos nativos (de la base de datos) para datos que no sean de caracteres y con datos Unicode para los datos de caracteres. Esta opción es una alternativa de mayor rendimiento que la opción **-w** y tiene como objeto la transferencia de datos de una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a otra mediante un archivo de datos. No realiza una petición para cada campo. Utilice esta opción cuando vaya a transferir datos que contengan caracteres extendidos ANSI y desee aprovechar el rendimiento del modo nativo.  
  
 Para obtener más información, vea [Usar el formato nativo Unicode para importar o exportar datos &#40;SQL Server&#41;](../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).  
  
 Si exporta y, después, importa datos al mismo esquema de tabla mediante bcp.exe con **-N**,puede aparecer una advertencia de truncamiento si hay una columna de caracteres no Unicode de longitud fija (por ejemplo, **char(10)** ).  
  
 La advertencia puede omitirse. Una manera de resolver esta advertencia es usar **-n** en lugar de **-N**.  
  
 **-o** _**output\_file**_<a name="o"></a>  
 Especifica el nombre de un archivo que recibe la salida redirigida desde el símbolo del sistema.  
  
 Si *output_file* comienza por un guión (-) o una barra diagonal (/), no incluya un espacio entre **-o** y el valor de *output_file* .  
  
 **-P** _**password**_<a name="P"></a>  
 Especifica la contraseña para el identificador de inicio de sesión. Si no se usa esta opción, el comando **bcp** solicitará una contraseña. Si se usa esta opción al final del símbolo del sistema sin especificar ninguna contraseña, **bcp** usa la contraseña predeterminada (NULL).  
  
> [!IMPORTANT]
> [!INCLUDE[ssNoteStrongPass](../includes/ssnotestrongpass-md.md)]
  
 Para enmascarar la contraseña, no especifique la opción **-P** junto con la opción **-U** . En su lugar, después de especificar **bcp** junto con la opción **-U** y otros modificadores (sin especificar **-P**), pulse ENTRAR y el comando le solicitará una contraseña. Este método garantiza que la contraseña se enmascare al especificarla.  
  
 Si *password* comienza por un guión (-) o una barra diagonal (/), no incluya un espacio entre **-P** y el valor de *password* .  
  
 **-q**<a name="q"></a>  
 Ejecuta la instrucción SET QUOTED_IDENTIFIERS ON en la conexión entre la utilidad **bcp** y una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Use esta opción para especificar una base de datos, un propietario, una tabla o un nombre de vista que contenga un espacio o una comilla simple. Englobe la totalidad del nombre de tres partes de la vista o tabla entre comillas (" ").  
  
 Para especificar un nombre de base de datos que contenga un espacio o una comilla simple, debe usar la opción **-q**.  
  
 **-q** no es válido para los valores que se pasan a **-d**.  
  
 Para más información, consulte la sección [Comentarios](#remarks)que aparece más adelante en este tema.  
  
 **-r** _**row\_term**_<a name="r"></a>  
 Especifica el terminador de la fila. El valor predeterminado es **\n** (carácter de nueva línea). Use este parámetro para sustituir el terminador de fila predeterminado. Para obtener más información, vea [Especificar terminadores de campo y de fila &#40;SQL Server&#41;](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
 Si especifica el terminador de fila en notación hexadecimal en un comando bcp.exe, el valor se truncará en 0x00. Por ejemplo, si especifica 0x410041, se usará 0x41.  
  
 Si *row_term* comienza por un guión (-) o una barra diagonal (/), no incluya un espacio entre **-r** y el valor de *row_term* .  
  
 **-R**<a name="R"></a>  
 Especifica que se realice la copia masiva de datos de moneda, fecha y hora en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con el formato regional definido para la configuración regional del equipo cliente. De forma predeterminada, la configuración regional se omite.  
  
 **-S** _**server\_name**_ [\\_**instance\_name**_]<a name="S"></a> Especifica la instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] a la que se debe conectar. Si no se especifica ningún servidor, la utilidad **bcp** se conecta a la instancia predeterminada de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en el equipo local. Esta opción es necesaria cuando se ejecuta un comando **bcp** desde un equipo remoto de la red o desde una instancia local con nombre. Para establecer una conexión con la instancia predeterminada de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un servidor, especifique únicamente *server_name*. Para conectar con una instancia con nombre de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], especifique _server\_name_ **\\** _instance\_name_.  
  
 **-t** _**field\_term**_<a name="t"></a>  
 Especifica el terminador del campo. El valor predeterminado es **\t** (carácter de tabulación). Use este parámetro para invalidar el terminador de campo predeterminado. Para obtener más información, vea [Especificar terminadores de campo y de fila &#40;SQL Server&#41;](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
 Si especifica el terminador de campo en notación hexadecimal en un comando bcp.exe, el valor se truncará en 0x00. Por ejemplo, si especifica 0x410041, se usará 0x41.  
  
 Si *field_term* comienza por un guión (-) o una barra diagonal (/), no incluya un espacio entre **-t** y el valor de *field_term* .  
  
 **-T**<a name="T"></a>  
 Especifica que la utilidad **bcp** se conecta a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con una conexión de confianza utilizando la seguridad integrada. No es necesario usar las credenciales de seguridad del usuario de la red, *login_id* y *password* . Si no se especifica **-T** , es necesario especificar **-U** y **-P** para iniciar sesión correctamente.

> [!IMPORTANT]
> Si la utilidad **bcp** se conecta a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mediante una conexión de confianza que usa seguridad integrada, use la opción **-T** (conexión de confianza) en lugar de la combinación *user name* y *password* . Cuando la utilidad **BCP** está conectada a SQL Database o Azure Synapse Analytics, no se admite la autenticación de Windows ni la autenticación de Azure Active Directory. Use las opciones **-U** y **-P** . 
  
 **-U** _**login\_id**_<a name="U"></a>  
 Especifica el identificador de inicio de sesión para conectar con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]
> Si la utilidad **bcp** se conecta a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mediante una conexión de confianza que usa seguridad integrada, use la opción **-T** (conexión de confianza) en lugar de la combinación *user name* y *password* . Cuando la utilidad **BCP** está conectada a SQL Database o Azure Synapse Analytics, no se admite la autenticación de Windows ni la autenticación de Azure Active Directory. Use las opciones **-U** y **-P** .
  
 **-v**<a name="v"></a>  
 Informa del número de versión y los derechos de autor de la utilidad **bcp** .  
  
 **-V** (**80** \| **90** \| **100** \| **110** \| **120** \| **130**)<a name="V"></a>  
 Realiza la operación de copia masiva con tipos de datos de versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Esta opción no realiza una petición para cada campo, sino que utiliza los valores predeterminados.  
  
 **80** = [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]  
  
 **90** = [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]  
  
 **100** = [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] y [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]  
  
 **110** = [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]  
  
 **120** = [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]  
  
 **130** = [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]  
  
 Por ejemplo, para generar datos de tipos no compatibles con [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)], pero que se introdujeron en versiones posteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], use la opción -V80.  
  
 Para obtener más información, vea [Importar datos con formato nativo y de caracteres de versiones anteriores de SQL Server](../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
 **-w**<a name="w"></a>  
 Realiza la operación de copia masiva con caracteres Unicode. Esta opción no realiza una petición para cada campo; usa **nchar** como tipo de almacenamiento, sin prefijos, con **\t** (carácter de tabulación) como separador de campos y **\n** (carácter de nueva línea) como terminador de filas. **-w** no es compatible con **-c**.  
  
 Para obtener más información, vea [Usar el formato de caracteres Unicode para importar o exportar datos &#40;SQL Server&#41;](../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).  
  
 **-x**<a name="x"></a>  
 Si se usa con las opciones **format** y **-f** *format_file*, genera un archivo de formato basado en XML en lugar del archivo de formato predeterminado que no es XML. **-x** no funciona cuando se importan o exportan datos. Genera un error si se usa sin **format** ni **-f** *format_file*.  

## <a name="remarks"></a>Comentarios<a name="remarks"></a>

- La utilidad **bcp** 13.0 se instala al instalar las herramientas de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[sssql19-md.md](../includes/sssql19-md.md)]. Si hay instaladas herramientas para varias versiones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], según el orden de valores de la variable de entorno PATH, es posible que se use el cliente **bcp** anterior en lugar del cliente **bcp** 13.0. Esta variable de entorno define el conjunto de directorios que Windows usa para buscar archivos ejecutables. Para saber qué versión se usa, ejecute el comando **bcp /v** o **bcp -v** en el símbolo del sistema de Windows. Para obtener información sobre cómo establecer la ruta de comandos en la variable de entorno PATH, vea [Variables de entorno](/windows/win32/shell/user-environment-variables), o bien busque Variables de entorno en la Ayuda de Windows.

    Para asegurarse de que se ejecuta la versión más reciente de la utilidad bcp, debe quitar las versiones anteriores de esta utilidad.

    Para determinar dónde están instaladas todas las versiones de la utilidad bcp, escriba en el símbolo del sistema:

    ```cmd
    where bcp.exe
    ```

- La utilidad bcp también se puede descargar por separado desde el [Feature Pack de Microsoft SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=56833).  Seleccione `ENU\x64\MsSqlCmdLnUtils.msi` o `ENU\x86\MsSqlCmdLnUtils.msi`.

- Los archivos con formato XML solamente se admiten cuando se instalan herramientas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client.

- Para obtener más información sobre dónde encontrar o cómo ejecutar la utilidad **bcp** y sobre las convenciones de sintaxis de las utilidades del símbolo del sistema, vea [Referencia de la utilidad del símbolo del sistema &#40;motor de base de datos&#41;](../tools/command-prompt-utility-reference-database-engine.md).

- Para obtener más información sobre cómo preparar datos para operaciones de importación y exportación en bloque, vea [Preparar los datos para exportar o importar en bloque &#40;SQL Server&#41;](../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

- Para obtener más información sobre cuándo se incluyen en el registro de transacciones las operaciones de inserción de filas que se efectúan durante una importación en bloque, vea [Requisitos previos para el registro mínimo durante la importación en bloque](../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).

- [Uso de caracteres especiales adicionales](/windows-server/administration/windows-commands/set_1#remarks)

    <, >, |, &, ^ son caracteres especiales del shell de comandos y deben ir precedidos del carácter de escape (^) o incluidos entre comillas cuando se usan en una cadena (por ejemplo, "StringContaining&Symbol"). Si usa comillas para incluir una cadena que contenga uno de los caracteres especiales, las comillas se establecerán como parte del valor de la variable de entorno.

## <a name="native-data-file-support"></a>Compatibilidad de los archivos de datos nativos

 En [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)], la utilidad **bcp** admite archivos de datos nativos compatibles con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], a partir de [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)].

## <a name="computed-columns-and-timestamp-columns"></a>Columnas calculadas y columnas de marca de tiempo

 Los valores del archivo de datos que se importan para las columnas calculadas o **timestamp** se omiten y [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] asigna valores automáticamente. Si el archivo de datos no contiene valores para las columnas calculadas o **timestamp** de la tabla, use un archivo de formato para especificar que deben pasarse por alto las columnas calculadas o **timestamp** de la tabla al importar datos; [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] asigna valores para la columna automáticamente.  
  
 Las columnas calculadas y **timestamp** se copian en bloque desde [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un archivo de datos de la forma habitual.  

## <a name="specifying-identifiers-that-contain-spaces-or-quotation-marks"></a>Especificar identificadores que incluyen espacios o comillas

 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pueden incluir caracteres tales como espacios insertados y comillas. Tales identificadores deben tratarse de la siguiente manera:  
  
- Cuando especifique un identificador o nombre de archivo que incluya un espacio o comillas en el símbolo del sistema, coloque el identificador entre comillas dobles (" ").  

    Por ejemplo, el siguiente comando `bcp out` crea un archivo de datos denominado `Currency Types.dat`:  

    ```cmd
    bcp AdventureWorks2012.Sales.Currency out "Currency Types.dat" -T -c  
    ```
  
- Para especificar un nombre de base de datos que contenga un espacio o comillas, hay que usar la opción **-q** .  
  
- Para los nombres de vista, tabla o propietario que contienen espacios insertados o comillas, puede hacer lo siguiente:  
  
    - Especificar la opción **-q** , o bien  
  
    - Incluir el nombre de vista, tabla o propietario entre corchetes ([]) dentro de las comillas.  

## <a name="data-validation"></a>Validación de datos

 **bcp** valida y comprueba ahora los datos, y ello podría dar lugar a errores en los scripts si se ejecutan con datos no válidos de un archivo. Por ejemplo, **bcp** ahora comprueba que:  
  
- Las representaciones nativas de los tipos de datos float o real son válidas.  
  
- Los datos Unicode tienen una longitud de bytes uniforme.  
  
 Es posible que los formularios de datos no válidos que podían importarse de forma masiva en versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no se carguen ahora, mientras que, en anteriores versiones, el error no se producía hasta que un cliente intentaba tener acceso a datos no válidos. La validación agregada evita sorpresas cuando se consultan los datos después de una carga masiva.  

## <a name="bulk-exporting-or-importing-sqlxml-documents"></a>Exportación o importación masiva de documentos SQLXML

Para importar o exportar de forma masiva datos SQLXML, utilice uno de los tipos de datos siguientes en el archivo de formato.  
  
|Tipo de datos|Efecto|  
|---------------|------------|  
|SQLCHAR o SQLVARYCHAR|Los datos se envían a la página de códigos del cliente o a la página de códigos implícita por la intercalación. Tiene el mismo efecto que especificar el modificador **-c** sin indicar un archivo de formato.|  
|SQLNCHAR o SQLNVARCHAR|Los datos se envían como datos Unicode. Tiene el mismo efecto que especificar el modificador **-w** sin indicar un archivo de formato.|  
|SQLBINARY o SQLVARYBIN|Los datos se envían sin realizar ninguna conversión.|  

## <a name="permissions"></a>Permisos

 Una operación **bcp out** requiere permisos SELECT en la tabla de origen.  
  
 Una operación **bcp in** requiere como mínimo permisos SELECT/INSERT en la tabla de destino. Además, se requiere el permiso ALTER TABLE si cualquiera de las siguientes afirmaciones es verdadera:  
  
- Existen restricciones y la sugerencia CHECK_CONSTRAINTS no se ha especificado.  
  
    > [!NOTE]
    > La deshabilitación de restricciones es el comportamiento predeterminado. Para habilitar las restricciones de forma explícita, use la opción **-h** con la sugerencia CHECK_CONSTRAINTS.
  
- Existen desencadenadores y la sugerencia FIRE_TRIGGER no se ha especificado.  
  
    > [!NOTE]
    > De forma predeterminada, los desencadenadores no se activan. Para activarlos de forma explícita, use la opción **-h** con la sugerencia FIRE_TRIGGERS.
  
- La opción **-E** se usa para importar valores de identidad de un archivo de datos.  
  
> [!NOTE]
> El requisito del permiso ALTER TABLE en la tabla de destino era nuevo en [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]. Este nuevo requisito podría hacer que los scripts de **bcp** que no exigen las comprobaciones de restricciones y desencadenadores devuelvan un error si la cuenta de usuario no tiene los permisos de tabla ALTER para la tabla de destino.

## <a name="character-mode--c-and-native-mode--n-best-practices"></a>Prácticas recomendadas para el modo de carácter (-c) y el modo nativo (-n)

 Esta sección contiene recomendaciones para el modo de carácter (-c) y el modo nativo (-n).  
  
- (Administrador o usuario) Cuando sea posible, utilice el formato nativo (-n) para evitar problemas de separador. Utilice el formato nativo para exportar e importar utilizando [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Exporte datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] utilizando la opción -c o -w si los datos se van a importar a una base de datos que no es de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
- (Administrador) Compruebe los datos al utilizar BCP OUT. Por ejemplo, cuando se utiliza BCP OUT, BCP IN y luego BCP OUT, compruebe que los datos se han exportado correctamente y que los valores de terminador no se utilizan como parte de algún valor de datos. Considere la posibilidad de reemplazar los terminadores predeterminados (utilizando las opciones -t y -r) por valores hexadecimales aleatorios para evitar conflictos entre los valores de terminador y los valores de datos.  
  
- (Usuario) Utilice un terminador largo y único (cualquier secuencia de bytes o caracteres) para minimizar la posibilidad de conflicto con el valor de cadena real. Esto se puede realizar utilizando las opciones -t y -r.  

## <a name="examples"></a>Ejemplos

En esta sección se incluyen los ejemplos siguientes:

A. Identificar la versión de la utilidad **bcp**

B. Copiar filas de tablas en un archivo de datos (con una conexión de confianza)

C. Copiar filas de tablas en un archivo de datos (con autenticación de modo mixto)

D. Copiar datos de un archivo en una tabla

E. Copiar una columna específica en un archivo de datos

F. Copiar una fila específica en un archivo de datos

G. Copiar datos de una consulta en un archivo de datos

H. Creación de archivos de formato

I. Usar un archivo de formato para importar de forma masiva con **bcp**

J. Especificar una página de códigos

### <a name="example-test-conditions"></a>**Condiciones de prueba de ejemplo**

Los ejemplos siguientes usan la base de datos de ejemplo `WideWorldImporters` para SQL Server (desde 2016) y Azure SQL Database.  `WideWorldImporters` se puede descargar desde [https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0).  Consulte [RESTORE (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) para conocer la sintaxis para restaurar la base de datos de ejemplo.  Salvo que se especifique lo contrario, los ejemplos suponen que usa la autenticación de Windows y que existe una conexión de confianza con la instancia del servidor en la que se ejecuta el comando **bcp** .  Muchos de los ejemplos usarán un directorio denominado `D:\BCP` .

El script siguiente crea una copia vacía de la tabla `WideWorldImporters.Warehouse.StockItemTransactions` y luego agrega una restricción PRIMARY KEY.  Ejecute el siguiente script T-SQL en SQL Server Management Studio (SSMS)

```sql  
USE WideWorldImporters;  
GO  

SET NOCOUNT ON;

IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Warehouse.StockItemTransactions_bcp')
BEGIN
    SELECT * INTO WideWorldImporters.Warehouse.StockItemTransactions_bcp
    FROM WideWorldImporters.Warehouse.StockItemTransactions  
    WHERE 1 = 2;  

    ALTER TABLE Warehouse.StockItemTransactions_bcp 
    ADD CONSTRAINT PK_Warehouse_StockItemTransactions_bcp PRIMARY KEY NONCLUSTERED 
    (StockItemTransactionID ASC);
END
```

> [!NOTE]
> Trunque la tabla `StockItemTransactions_bcp` según sea necesario.
>
> TRUNCATE TABLE WideWorldImporters.Warehouse.StockItemTransactions_bcp;

### <a name="a--identify-bcp-utility-version"></a>A.  Identificar la versión de la utilidad **bcp**

En el símbolo del sistema, escriba el siguiente comando:

```cmd
bcp -v
```
  
### <a name="b-copying-table-rows-into-a-data-file-with-a-trusted-connection"></a>B. Copiar filas de tablas en un archivo de datos (con una conexión de confianza)

En los siguientes ejemplos se ilustra la opción **out** de la tabla `WideWorldImporters.Warehouse.StockItemTransactions`.

- **Básico** En este ejemplo se crea un archivo de datos con el nombre `StockItemTransactions_character.bcp` y se usa el formato de **caracteres** para copiar los datos de la tabla en ese archivo.

  En el símbolo del sistema, escriba el siguiente comando:

  ```cmd
  bcp WideWorldImporters.Warehouse.StockItemTransactions out D:\BCP\StockItemTransactions_character.bcp -c -T
  ```

- **Expandido** En este ejemplo se crea un archivo de datos con el nombre `StockItemTransactions_native.bcp` y se usa el formato **nativo** para copiar los datos de la tabla en ese archivo.  En el ejemplo también se especifica el número máximo de errores de sintaxis, un archivo de error y un archivo de salida.

    En el símbolo del sistema, escriba el siguiente comando:

    ```cmd
    bcp WideWorldImporters.Warehouse.StockItemTransactions OUT D:\BCP\StockItemTransactions_native.bcp -m 1 -n -e D:\BCP\Error_out.log -o D:\BCP\Output_out.log -S -T
    ```

Revise `Error_out.log` y `Output_out.log`.  `Error_out.log` debe estar en blanco.  Compare los tamaños de archivo entre `StockItemTransactions_character.bcp` y `StockItemTransactions_native.bcp`.

### <a name="c-copying-table-rows-into-a-data-file-with-mixed-mode-authentication"></a>C. Copiar filas de tablas en un archivo de datos (con autenticación de modo mixto)

En el siguiente ejemplo se ilustra la opción **out** de la tabla `WideWorldImporters.Warehouse.StockItemTransactions` .  En este ejemplo se crea un archivo de datos con el nombre `StockItemTransactions_character.bcp` y se usa el formato de **caracteres** para copiar los datos de la tabla en ese archivo.  
  
 En el ejemplo se da por hecho que usa la autenticación de modo mixto; hay que usar el modificador **-U** para especificar su identificador de inicio de sesión. Además, a menos que se esté conectando a la instancia predeterminada de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en el equipo local, use el modificador **-S** para especificar el nombre del sistema y, opcionalmente, un nombre de instancia.  

En el símbolo del sistema, escriba el siguiente comando: \(El sistema le solicitará la contraseña.\)

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions out D:\BCP\StockItemTransactions_character.bcp -c -U<login_id> -S<server_name\instance_name>
```

### <a name="d-copying-data-from-a-file-to-a-table"></a>D. Copiar datos de un archivo en una tabla

Los ejemplos siguientes ilustran la opción **in** en la tabla `WideWorldImporters.Warehouse.StockItemTransactions_bcp` con los archivos que se crearon anteriormente.

- **Básico** En este ejemplo se usa el archivo de datos `StockItemTransactions_character.bcp` que se creó anteriormente.

  En el símbolo del sistema, escriba el siguiente comando:

  ```cmd
  bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp IN D:\BCP\StockItemTransactions_character.bcp -c -T
  ```

- **Expandido** En este ejemplo se usa el archivo de datos `StockItemTransactions_native.bcp` que se creó anteriormente.  En el ejemplo también se usa la sugerencia **TABLOCK**, se especifica el tamaño del lote, el número máximo de errores de sintaxis, un archivo de error y un archivo de salida.
  
En el símbolo del sistema, escriba el siguiente comando:

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp IN D:\BCP\StockItemTransactions_native.bcp -b 5000 -h "TABLOCK" -m 1 -n -e D:\BCP\Error_in.log -o D:\BCP\Output_in.log -S -T
```

  Revise `Error_in.log` y `Output_in.log`.

### <a name="e-copying-a-specific-column-into-a-data-file"></a>E. Copiar una columna específica en un archivo de datos

Para copiar una columna específica, puede usar la opción **queryout** .  El siguiente ejemplo copia únicamente la columna `StockItemTransactionID` de la tabla `Warehouse.StockItemTransactions` en un archivo de datos.
  
En el símbolo del sistema, escriba el siguiente comando:

```cmd
bcp "SELECT StockItemTransactionID FROM WideWorldImporters.Warehouse.StockItemTransactions WITH (NOLOCK)" queryout D:\BCP\StockItemTransactionID_c.bcp -c -T
```

### <a name="f-copying-a-specific-row-into-a-data-file"></a>F. Copiar una fila específica en un archivo de datos

Para copiar una fila específica, puede usar la opción **queryout** . El siguiente ejemplo copia únicamente la fila correspondiente a la persona con el nombre `Amy Trefl` de la tabla `WideWorldImporters.Application.People` en un archivo de datos `Amy_Trefl_c.bcp`.  Nota: El modificador **-d** se usa para identificar la base de datos.
  
En el símbolo del sistema, escriba el siguiente comando:

```cmd
bcp "SELECT * from Application.People WHERE FullName = 'Amy Trefl'" queryout D:\BCP\Amy_Trefl_c.bcp -d WideWorldImporters -c -T
```

### <a name="g-copying-data-from-a-query-to-a-data-file"></a>G. Copiar datos de una consulta en un archivo de datos

Para copiar el conjunto de resultados de una instrucción Transact-SQL en un archivo de datos, use la opción **queryout** .  El ejemplo siguiente copia los nombres de la tabla `WideWorldImporters.Application.People` , ordenados por nombre completo, en el archivo de datos `People.txt` .  Nota: El modificador **-t** se usa para crear un archivo delimitado por comas.

En el símbolo del sistema, escriba el siguiente comando:

```cmd
bcp "SELECT FullName, PreferredName FROM WideWorldImporters.Application.People ORDER BY FullName" queryout D:\BCP\People.txt -t, -c -T
```

### <a name="h-creating-format-files"></a>H. Creación de archivos de formato

El ejemplo siguiente crea tres archivos de formato distintos para la tabla `Warehouse.StockItemTransactions` en la base de datos `WideWorldImporters` .  Revise el contenido de cada archivo creado.

En un símbolo del sistema, escriba los comandos siguientes:

```cmd
REM non-XML character format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_c.fmt -c -T 

REM non-XML native format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_n.fmt -n -T

REM XML character format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_c.xml -x -c -T
```

> [!NOTE]
> Para usar el modificador **-x** , debe contar con un cliente **bcp** 9.0. Para información sobre cómo usar el cliente **bcp** 9.0, consulte la sección "[Comentarios](#remarks)".
  
 Para más información, consulte [Archivos de formato no XML &#40;SQL Server&#41;](../relational-databases/import-export/non-xml-format-files-sql-server.md) y [Archivos de formato XML &#40;SQL Server&#41;](../relational-databases/import-export/xml-format-files-sql-server.md).
  
### <a name="i-using-a-format-file-to-bulk-import-with-bcp"></a>I. Usar un archivo de formato para importar de forma masiva con bcp

Para usar un archivo de formato creado anteriormente al importar datos a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], use el modificador **-f** con la opción **in** .  Por ejemplo, el siguiente comando copia de forma masiva el contenido de un archivo de datos, `StockItemTransactions_character.bcp`, en una copia de la tabla `Warehouse.StockItemTransactions_bcp` mediante el archivo de formato creado anteriormente, `StockItemTransactions_c.xml`.  Nota: El modificador **-L** se usa para importar únicamente los 100 primeros registros.

En el símbolo del sistema, escriba el siguiente comando:

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp in D:\BCP\StockItemTransactions_character.bcp -L 100 -f D:\BCP\StockItemTransactions_c.xml -T
```

> [!NOTE]
> Los archivos de formato son útiles cuando los campos del archivo de datos son diferentes a los de las columnas de la tabla; por ejemplo, en su numeración, orden o tipos de datos. Para obtener más información, vea [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  

### <a name="j-specifying-a-code-page"></a>J. Especificar una página de códigos

En el siguiente ejemplo de código parcial se muestra la importación de bcp al especificar una página de código 65001:

```cmd
bcp.exe MyTable in "D:\data.csv" -T -c -C 65001 -t , ...  
```

## <a name="additional-examples"></a>Otros ejemplos

|Los temas siguientes incluyen ejemplos de uso de bcp: |
|---|
|Formatos de datos para importación o exportación masivas (SQL Server)<br />&emsp;&#9679;&emsp;[Usar el formato nativo para importar o exportar datos (SQL Server)](../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar el formato de caracteres para importar o exportar datos (SQL Server)](../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar el formato nativo Unicode para importar o exportar datos (SQL Server)](../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar el formato de caracteres Unicode para importar o exportar datos (SQL Server)](../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br /><br />[Especificar terminadores de campo y de fila (SQL Server)](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)<br /><br />[Mantener valores NULL o usar valores predeterminados durante la importación masiva (SQL Server)](../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)<br /><br />[Mantener valores de identidad al importar datos de forma masiva (SQL Server)](../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)<br /><br />Archivos de formato para importar o exportar datos (SQL Server)<br />&emsp;&#9679;&emsp;[Crear un archivo de formato (SQL Server)](../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar un archivo de formato para importar datos de forma masiva (SQL Server)](../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar un archivo de formato para omitir una columna de tabla (SQL Server)](../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar un archivo de formato para omitir un campo de datos (SQL Server)](../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar un archivo de formato para asignar columnas de tabla a campos de un archivo de datos (SQL Server)](../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)<br /><br />[Ejemplos de importación y exportación de forma masiva documentos XML (SQL Server)](../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)<br /><p>  </p>|

## <a name="considerations-and-limitations"></a>Consideraciones y limitaciones

- La utilidad bcp limita a 512 bytes los caracteres que muestra el mensaje de error. Solo se muestran los primeros 512 bytes del mensaje de error.

## <a name="next-steps"></a>Pasos siguientes

- [Preparar los datos para exportar o importar en bloque &#40;SQL Server&#41;](../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [BULK INSERT &#40;Transact-SQL&#41;](../t-sql/statements/bulk-insert-transact-sql.md)
- [OPENROWSET &#40;Transact-SQL&#41;](../t-sql/functions/openrowset-transact-sql.md)
- [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../t-sql/statements/set-quoted-identifier-transact-sql.md)
- [sp_configure &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_tableoption &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)
- [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
