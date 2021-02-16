---
title: CREATE DATABASE (Transact-SQL) | Microsoft Docs
description: Sintaxis de creación de bases de datos para SQL Server, Azure SQL Database, Azure Synapse Analytics y Analytics Platform System
ms.custom: references_regions
ms.date: 12/11/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_TSQL
- DATABASE
- CONTAINMENT_TSQL
- CREATE DATABASE
- CREATE_DATABASE_TSQL
- CONTAINS_FILESTREAM_TSQL
- CONTAINS FILESTREAM
- CONTAINMENT
dev_langs:
- TSQL
helpviewer_keywords:
- snapshots [SQL Server database snapshots], creating
- databases [SQL Server], creating
- model database [SQL Server], database creation
- mounted drives [SQL Server]
- CREATE DATABASE
- CREATE DATABASE statement
- file creation [SQL Server]
- creating databases
- containment
- filegroups [SQL Server], database creation
- database creation [SQL Server], CREATE DATABASE statement
- moving databases
- attaching databases [SQL Server], CREATE DATABASE...FOR ATTACH
ms.assetid: 29ddac46-7a0f-4151-bd94-75c1908c89f8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016'
ms.openlocfilehash: 509e4fc7445371c57d0fed7208d556e53aa18fe8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063041"
---
# <a name="create-database"></a>CREATE DATABASE

Crea una nueva base de datos.

Haga clic en una de las pestañas siguientes para obtener la sintaxis, los argumentos, los comentarios, los permisos y los ejemplos para una versión de SQL concreta con la que está trabajando.

Para obtener más información sobre las convenciones de sintaxis, vea [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

[!INCLUDE[select-product](../../includes/select-product.md)]

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Database](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

## <a name="overview"></a>Información general

En SQL Server, esta instrucción crea una base de datos nueva y los archivos usados y sus grupos de archivos. También puede usarse para crear una instantánea de base de datos o adjuntar archivos de base de datos para crear una base de datos de los archivos desasociados de otra base de datos.

## <a name="syntax"></a>Sintaxis

Crear una base de datos

```syntaxsql
CREATE DATABASE database_name
[ CONTAINMENT = { NONE | PARTIAL } ]
[ ON
      [ PRIMARY ] <filespec> [ ,...n ]
      [ , <filegroup> [ ,...n ] ]
      [ LOG ON <filespec> [ ,...n ] ]
]
[ COLLATE collation_name ]
[ WITH <option> [,...n ] ]
[;]

<option> ::=
{
      FILESTREAM ( <filestream_option> [,...n ] )
    | DEFAULT_FULLTEXT_LANGUAGE = { lcid | language_name | language_alias }
    | DEFAULT_LANGUAGE = { lcid | language_name | language_alias }
    | NESTED_TRIGGERS = { OFF | ON }
    | TRANSFORM_NOISE_WORDS = { OFF | ON}
    | TWO_DIGIT_YEAR_CUTOFF = <two_digit_year_cutoff>
    | DB_CHAINING { OFF | ON }
    | TRUSTWORTHY { OFF | ON }
    | PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='<Filepath to folder on DAX formatted volume>' )
}

<filestream_option> ::=
{
      NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }
    | DIRECTORY_NAME = 'directory_name'
}

<filespec> ::=
{
(
    NAME = logical_file_name ,
    FILENAME = { 'os_file_name' | 'filestream_path' }
    [ , SIZE = size [ KB | MB | GB | TB ] ]
    [ , MAXSIZE = { max_size [ KB | MB | GB | TB ] | UNLIMITED } ]
    [ , FILEGROWTH = growth_increment [ KB | MB | GB | TB | % ] ]
)
}

<filegroup> ::=
{
FILEGROUP filegroup name [ [ CONTAINS FILESTREAM ] [ DEFAULT ] | CONTAINS MEMORY_OPTIMIZED_DATA ]
    <filespec> [ ,...n ]
}
```

Adjuntar una base de datos

```syntaxsql
CREATE DATABASE database_name
    ON <filespec> [ ,...n ]
    FOR { { ATTACH [ WITH <attach_database_option> [ , ...n ] ] }
        | ATTACH_REBUILD_LOG }
[;]

<attach_database_option> ::=
{
      <service_broker_option>
    | RESTRICTED_USER
    | FILESTREAM ( DIRECTORY_NAME = { 'directory_name' | NULL } )
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
}
```

Crear una instantánea de base de datos

```syntaxsql
CREATE DATABASE database_snapshot_name
    ON
    (
        NAME = logical_file_name,
        FILENAME = 'os_file_name'
    ) [ ,...n ]
    AS SNAPSHOT OF
[;]
```

## <a name="arguments"></a>Argumentos

*database_name* Es el nombre de la nueva base de datos. Los nombres de base de datos deben ser únicos en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).

*database_name* puede tener 128 caracteres como máximo, a menos que no se especifique un nombre lógico para el archivo de registro. Si no se especifica un nombre de archivo de registro lógico, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera *logical_file_name* y *os_file_name* para el registro, anexando un sufijo a *database_name*. Esto limita *database_name* a 123 caracteres, por lo que el nombre de archivo lógico generado tiene como máximo 128 caracteres.

Si no se especifica el nombre de archivo de datos, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza *database_name* como el *logical_file_name* y como *os_file_name*. La ruta de acceso predeterminada se obtiene del Registro. La ruta de acceso predeterminada se puede cambiar utilizando las **Propiedades del servidor (página de configuración de base de datos)** en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Para cambiar la ruta de acceso predeterminada se requiere reiniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

CONTAINMENT = { NONE | PARTIAL }

**Válido para** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

Especifica el estado de contención de la base de datos. NONE = base de datos dependiente. PARTIAL = base de datos parcialmente independiente.

ON Especifica que los archivos de disco utilizados para almacenar las secciones de datos de la base de datos (archivos de datos) se definen explícitamente. ON es obligatorio cuando va seguido de una lista de elementos \<filespec> separados por comas que definen los archivos de datos del grupo de archivos principal. Detrás de la lista de archivos del grupo de archivos principal se puede colocar una lista opcional de elementos \<filegroup> separados por comas que definan los grupos de archivos de usuario y sus archivos.

PRIMARY Especifica que la lista de elementos \<filespec> asociada define el archivo principal. El primer archivo especificado en la entrada \<filespec> del grupo de archivos principal se convierte en el archivo principal. Una base de datos solo puede tener un archivo principal. Para más información, consulte [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).

Si no se especifica PRIMARY, el primer archivo enumerado en la instrucción CREATE DATABASE se convierte en el archivo principal.

LOG ON Especifica que los archivos de disco utilizados para almacenar el registro de la base de datos (archivos de registro) se definen explícitamente. LOG ON va seguido de una lista de elementos \<filespec> separados por comas que definen los archivos de registro. Si no se especifica LOG ON, se crea automáticamente un archivo de registro cuyo tamaño es el 25 % de la suma de los tamaños de todos los archivos de datos de la base de datos o 512 KB, el valor que sea mayor. Este archivo se coloca en la ubicación del archivo de registro predeterminado. Para obtener más información sobre esta ubicación, consulte [Ver o cambiar las ubicaciones predeterminadas de los archivos de datos y registro - SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md).

LOG ON no se puede especificar en una instantánea de base de datos.

COLLATE *collation_name* Especifica la intercalación predeterminada de la base de datos. El nombre de intercalación puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Si no se especifica, se asigna a la base de datos la intercalación predeterminada de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. No se puede especificar un nombre de intercalación en una instantánea de base de datos.

No se puede especificar un nombre de intercalación con las cláusulas FOR ATTACH o FOR ATTACH_REBUILD_LOG. Para obtener información acerca de cómo cambiar la intercalación de una base de datos adjuntada, visite este [sitio web de Microsoft](https://go.microsoft.com/fwlink/?linkid=16419&kbid=325335).

Para más información sobre los nombres de intercalación de Windows y de SQL, consulte [COLLATE](~/t-sql/statements/collations.md).

> [!NOTE]
> Las bases de datos independientes se intercalan de modo diferente al de las bases de datos dependientes. Para obtener más información, consulte [Intercalaciones de bases de datos independientes](../../relational-databases/databases/contained-database-collations.md).

WITH \<option>
 **\<filestream_option>**

NON_TRANSACTED_ACCESS = { **OFF** | READ_ONLY | FULL } **Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores

Especifica el nivel de acceso no transaccional de FILESTREAM a la base de datos.

|Value|Descripción|
|-----------|-----------------|
|Apagado|El acceso no transaccional está deshabilitado.|
|READONLY|Los procesos no transaccionales pueden leer los datos de FILESTREAM en esta base de datos.|
|FULL|El acceso no transaccional total a objetos FileTable de FILESTREAM está habilitado.|

DIRECTORY_NAME = \<directory_name>
**Se aplica a**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores

Un nombre de directorio compatible con Windows. Este nombre debe ser único entre todos los nombres de Database_Directory en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La comparación de unicidad no distingue mayúsculas de minúsculas, independientemente de la configuración de intercalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta opción se debe establecer antes de crear un objeto FileTable en esta base de datos.

Las opciones siguientes se permiten solo cuando CONTAINMENT se ha establecido en PARTIAL. Si CONTAINMENT se establece en NONE, se producirán errores.

- **DEFAULT_FULLTEXT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **Válido para** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

  Para obtener más información, consulte [Establecer la opción de configuración del servidor Idioma de texto completo predeterminado](../../database-engine/configure-windows/configure-the-default-full-text-language-server-configuration-option.md).

- **DEFAULT_LANGUAGE = \<lcid> | \<language name> | \<language alias>**

  **Válido para** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

  Para obtener más información, consulte [Establecer la opción de configuración del servidor Idioma predeterminado](../../database-engine/configure-windows/configure-the-default-language-server-configuration-option.md).

- **NESTED_TRIGGERS = { OFF | ON}**

  **Válido para** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

  Para obtener más información, consulte [Establecer la opción de configuración del servidor Desencadenadores anidados](../../database-engine/configure-windows/configure-the-nested-triggers-server-configuration-option.md).

- **TRANSFORM_NOISE_WORDS = { OFF | ON}**

  **Válido para** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

  Para obtener más información, consulte [transform noise words (opción de configuración del servidor)](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md).

- **TWO_DIGIT_YEAR_CUTOFF = { 2049 | \<any year between 1753 and 9999> }**

  Cuatro dígitos que representan un año. El valor predeterminado es 2049. Para obtener más información, consulte [Establecer la opción de configuración del servidor Fecha límite de año de dos dígitos](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md).

- **DB_CHAINING { OFF | ON }**

  Si se especifica ON, la base de datos puede ser el origen o destino de una cadena de propiedad entre bases de datos.

  Si es OFF, la base de datos no puede participar en encadenamientos de propiedad entre bases de datos. El valor predeterminado es OFF.

  > [!IMPORTANT]
  > La instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconoce esta configuración si la opción del servidor cross db ownership chaining server es 0 (OFF). Si cross db ownership chaining es 1 (ON), todas las bases de datos de usuario pueden participar en cadenas de propiedad entre bases de datos, independientemente del valor de esta opción. Esta opción se establece mediante [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).

  Para establecer esta opción, debe pertenecer al rol fijo de servidor sysadmin. La opción DB_CHAINING no se puede establecer en estas bases de datos del sistema: master, model, tempdb.

- **TRUSTWORTHY { OFF | ON }**

  Cuando se especifica ON, los módulos de base de datos (por ejemplo, vistas, funciones definidas por el usuario o procedimientos almacenados) que utilicen un contexto de suplantación pueden tener acceso a recursos externos a la base de datos.

  Si es OFF, los módulos de base de datos en un contexto de suplantación no pueden tener acceso a recursos externos a la base de datos. El valor predeterminado es OFF.

  TRUSTWORTHY se establece en OFF siempre que la base de datos se adjunta.

  De forma predeterminada, el valor TRUSTWORTHY se establece en OFF en todas las bases de datos de sistema, excepto msdb. El valor no se puede cambiar en las bases de datos model ni tempdb. Se recomienda no establecer la opción TRUSTWORTHY en ON en la base de datos maestra.

- **PERSISTENT_LOG_BUFFER=ON ( DIRECTORY_NAME='' )**

  Cuando se especifica esta opción, se crea el búfer de registro de transacciones en un volumen que se encuentra en un dispositivo de disco respaldado por la memoria de clase de almacenamiento (almacenamiento permanente de NVDIMM-N), también conocido como un búfer de registro persistente. Para más información, consulte [Transaction Commit latency acceleration using Storage Class Memory](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1) (Aceleración de latencia de confirmación de transacciones con memoria de clase de almacenamiento). **Válido para**: [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y versiones más recientes.

FOR ATTACH [ WITH \< attach_database_option > ] Especifica que la base de datos se crea al [adjuntar](../../relational-databases/databases/database-detach-and-attach-sql-server.md) un conjunto existente de archivos del sistema operativo. Debe haber una entrada \<filespec> que especifique el archivo principal. Las demás entradas \<filespec> obligatorias son las correspondientes a los archivos con una ruta de acceso diferente de la que tenían al crear la base de datos o cuando se adjuntó por última vez. Se debe especificar una entrada \<filespec> para estos archivos.

FOR ATTACH tiene los siguientes requisitos:

- Todos los archivos de datos (MDF y NDF) deben estar disponibles.
- Si hay varios archivos de registro, todos ellos deben estar disponibles.

Si una base de datos de lectura/escritura tiene un único archivo de registro que no está disponible actualmente y si la base de datos se cerró sin usuarios o transacciones abiertas antes de la operación de adjuntar, FOR ATTACH regenera automáticamente el archivo de registro y actualiza el archivo principal. En cambio, en el caso de una base de datos de solo lectura, el registro no se puede volver a generar porque el archivo principal no se puede actualizar. Por lo tanto, cuando se adjunta una base de datos de solo lectura con un registro que no está disponible, se deben proporcionar los archivos de registro o los archivos de la cláusula FOR ATTACH.

> [!NOTE]
> Una base de datos creada por una versión más reciente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede adjuntarse en versiones anteriores.

En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los archivos de texto completo que formen parte de la base de datos que se va a adjuntar se adjuntarán con la base de datos. Para especificar una nueva ruta de acceso al catálogo de texto completo, escriba la nueva ubicación sin incluir el nombre de archivo de texto completo del sistema operativo. Para obtener más información, vea la sección Ejemplos.

Si se adjunta una base de datos que contiene una opción FILESTREAM de "Nombre de directorio" en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se solicitará a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que compruebe que el nombre de Database_Directory es único. Si no lo es, se producirá un error en la operación de adjuntar con el mensaje "El nombre \<name> Database_Directory de FILESTREAM no es único en esta instancia de SQL Server". Para evitar este error, se debe pasar a esta operación el parámetro opcional *directory_name*.

FOR ATTACH no se puede especificar en una instantánea de base de datos.

FOR ATTACH puede especificar la opción RESTRICTED_USER. RESTRICTED_USER permite que solamente se conecten a la base de datos los miembros del rol fijo de base de datos db_owner y de los roles fijos de servidor dbcreator y sysadmin, aunque no limita su número. Los intentos de los usuarios no calificados se rechazarán.

Si la base de datos usa [!INCLUDE[ssSB](../../includes/sssb-md.md)], utilice WITH \<service_broker_option> en la cláusula FOR ATTACH:

\<service_broker_option> Controla la entrega de mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)] y el identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] para la base de datos. Las opciones de [!INCLUDE[ssSB](../../includes/sssb-md.md)] solo se pueden especificar cuando se usa la cláusula FOR ATTACH.

ENABLE_BROKER Indica que se habilite [!INCLUDE[ssSB](../../includes/sssb-md.md)] para la base de datos especificada. En otras palabras, se inicia la entrega de mensajes y se establece is_broker_enabled en True en la vista de catálogo sys.databases. La base de datos conserva el identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] existente.

NEW_BROKER Crea un valor service_broker_guid en sys.databases y en la base de datos restaurada y finaliza todos los extremos de conversación con limpieza. El agente se habilita, pero no se envía ningún mensaje a los extremos de conversación remotos. Cualquier ruta que haga referencia al identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] anterior se debe volver a crear con el nuevo identificador.

ERROR_BROKER_CONVERSATIONS Finaliza todas las conversaciones con un error que indica que la base de datos está adjunta o restaurada. El agente está deshabilitado hasta que finaliza esta operación y, después, se habilita. La base de datos conserva el identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] existente.

Si adjunta una base de datos replicada que fue copiada en lugar de ser separada, tenga en cuenta lo siguiente:

- Si adjunta la base de datos a la misma versión e instancia de servidor que la base de datos original, no es necesario realizar ningún paso adicional.
- Si adjunta la base de datos a la misma instancia de servidor pero con una versión actualizada, debe ejecutar [sp_vupgrade_replication](../../relational-databases/system-stored-procedures/sp-vupgrade-replication-transact-sql.md) para actualizar la replicación una vez que se complete la operación de adjuntar.
- Si adjunta la base de datos a una instancia de servidor diferente, independientemente de la versión, debe ejecutar [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) para quitar la replicación una vez que se complete la operación de adjuntar.

> [!NOTE]
> Adjunte los trabajos con el formato de almacenamiento **vardecimal**, pero el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] se debe actualizar al menos a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] SP2. No puede adjuntar ninguna base de datos que use el formato de almacenamiento vardecimal a una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información sobre el formato de almacenamiento **vardecimal**, consulte [Compresión de datos](../../relational-databases/data-compression/data-compression.md).

La primera vez que se adjunta una base de datos o se restaura en una instancia nueva de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], aún no se ha almacenado una copia de la clave maestra de la base de datos (cifrada por la clave maestra de servicio) en el servidor. Debe usar la instrucción **OPEN MASTER KEY** para descifrar la clave maestra de la base de datos (DMK). Una vez que se ha descifrado la clave maestra de la base de datos, tiene la posibilidad de habilitar el descifrado automático en el futuro usando la instrucción **ALTER MASTER KEY REGENERATE** para proporcionar al servidor una copia de la clave maestra de la base de datos cifrada con la clave maestra de servicio (SMK). Cuando una base de datos se haya actualizado desde una versión anterior, se debe volver a generar la DMK para usar el algoritmo AES más reciente. Para obtener más información sobre cómo volver a generar la DMK, vea [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). El tiempo necesario para volver a generar la DMK con el fin de actualizarse a AES depende del número de objetos protegidos por la DMK. Solo es necesario volver a generar la DMK una vez y no tiene ningún efecto sobre las nuevas generaciones futuras como parte de una estrategia de rotación de claves. Para obtener información sobre cómo actualizar una base de datos mediante el uso de adjuntar, vea [Actualizar una base de datos mediante Separar y Adjuntar](../../relational-databases/databases/upgrade-a-database-using-detach-and-attach-transact-sql.md).

> [!IMPORTANT]
> Se recomienda no adjuntar bases de datos de orígenes desconocidos o que no sean de confianza. Es posible que dichas bases de datos contengan código malintencionado que podría ejecutar código [!INCLUDE[tsql](../../includes/tsql-md.md)] no deseado o provocar errores al modificar el esquema o la estructura de la base de datos física. Para usar una base de datos desde un origen desconocido o que no sea de confianza, ejecute [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) en la base de datos de un servidor que no sea de producción y examine también el código, como procedimientos almacenados u otro código definido por el usuario, en la base de datos.
> [!NOTE]
> Las opciones **TRUSTWORTHY** y **DB_CHAINING** no tienen ningún efecto cuando se adjunta una base de datos.

FOR ATTACH_REBUILD_LOG Especifica que la base de datos se crea adjuntando un conjunto existente de archivos del sistema operativo. Esta opción está limitada a las bases de datos de lectura y escritura. Debe haber una entrada *\<filespec>* que especifique el archivo principal. Si no se encuentran uno o varios archivos de registro de transacciones, se volverá a generar el archivo de registro. ATTACH_REBUILD_LOG crea automáticamente un nuevo archivo de registro de 1 MB. Este archivo se coloca en la ubicación del archivo de registro predeterminado. Para obtener más información sobre esta ubicación, consulte [Ver o cambiar las ubicaciones predeterminadas de los archivos de datos y registro - SSMS](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md).

> [!NOTE]
> Si los archivos de registro están disponibles, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] utiliza esos archivos en lugar de volver a generar los archivos de registro.

FOR ATTACH_REBUILD_LOG requiere lo siguiente:

- Un cierre limpio de la base de datos.
- Todos los archivos de datos (MDF y NDF) deben estar disponibles.

> [!IMPORTANT]
> Esta operación interrumpe la cadena de copias de seguridad de registros. Se recomienda realizar una copia de seguridad completa de la base de datos después de finalizar la operación. Para obtener más información, vea [BACKUP](../../t-sql/statements/backup-transact-sql.md).

Normalmente, FOR ATTACH_REBUILD_LOG se utiliza cuando se copia una base de datos de lectura/escritura con un registro grande en otro servidor donde la copia se va a utilizar mayoritariamente, o únicamente, para operaciones de lectura y, por lo tanto, requiere menos espacio de registro que la base de datos original.

FOR ATTACH_REBUILD_LOG no se puede especificar en una instantánea de base de datos.

Para obtener más información sobre cómo se adjuntan y separan las bases de datos, vea [Adjuntar y separar bases de datos](../../relational-databases/databases/database-detach-and-attach-sql-server.md).

\<filespec> Controla las propiedades de archivo.

NAME *logical_file_name* Especifica el nombre lógico del archivo. NAME es obligatorio si se especifica FILENAME, excepto cuando se especifica una de las cláusulas FOR ATTACH. Un grupo de archivos FILESTREAM no se puede denominar PRIMARY.

*logical_file_name* Es el nombre lógico utilizado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando se hace referencia al archivo. *Logical_file_name* debe ser único en la base de datos y debe cumplir las mismas reglas que los [identificadores](../../relational-databases/databases/database-identifiers.md). El nombre puede ser una constante de caracteres o Unicode, o un identificador regular o delimitado.

FILENAME { **'** _os\_file\_name_ **'** | **'** _filestream\_path_ **'** } especifica el nombre de archivo (físico) del sistema operativo.

**'** *os_file_name* **'** es la ruta de acceso y el nombre de archivo que el sistema operativo utiliza cuando se crea el archivo. El archivo debe residir en uno de los siguientes dispositivos: el servidor local en el que se ha instalado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], una red de área de almacenamiento (SAN) o una red basada en iSCSI. La ruta de acceso especificada debe existir antes de ejecutar la instrucción CREATE DATABASE. Para obtener más información, vea "Archivos y grupos de archivos de base de datos" en la sección Comentarios.

Los parámetros SIZE, MAXSIZE y FILEGROWTH se pueden establecer si se ha especificado una ruta UNC para el archivo.

Si el archivo se encuentra en una partición sin formato, *os_file_name* solo debe indicar la letra de unidad de una partición sin formato existente. Solo se puede crear un archivo de datos en cada partición sin procesar.

Los archivos de datos no deben guardarse en sistemas de archivo comprimidos a menos que se trate de archivos secundarios de solo lectura o que la base de datos sea de solo lectura. Los archivos de registro no se deben almacenar en sistemas de archivos comprimidos.

**"** *filestream_path* **"** Para un grupo de archivos FILESTREAM, FILENAME hace referencia a la ruta de acceso donde se almacenarán los datos FILESTREAM. La ruta de acceso hasta la última carpeta debe existir y la última carpeta no debe existir. Por ejemplo, si especifica la ruta de acceso C:\MyFiles\MyFilestreamData, C:\MyFiles debe existir antes de ejecutar ALTER DATABASE, pero la carpeta MyFilestreamData no debe existir.

El grupo de archivos y el archivo (`<filespec>`) se deben crear en la misma instrucción.

Las propiedades SIZE y FILEGROWTH no se aplican a un grupo de archivos FILESTREAM.

SIZE *size* Especifica el tamaño del archivo.

SIZE no se puede especificar si se especifica *os_file_name* como ruta UNC. SIZE no se aplica a un grupo de archivos FILESTREAM.

*size* Es el tamaño inicial del archivo.

Cuando no se suministra *size* para el archivo principal, [!INCLUDE[ssDE](../../includes/ssde-md.md)] utiliza el tamaño del archivo principal de la base de datos modelo. El tamaño predeterminado del modelo es de 8 MB (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) o 1 MB (para versiones anteriores). Cuando se especifica un archivo de datos secundario o un archivo de registro, pero no se especifica *size* para el archivo, [!INCLUDE[ssDE](../../includes/ssde-md.md)] hace que el tamaño del archivo sea de 8 MB (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]) o 1 MB (para versiones anteriores). El tamaño especificado para el archivo principal debe tener al menos el tamaño del archivo principal de la base de datos model.

Se pueden utilizar los sufijos kilobyte (KB), megabyte (MB), gigabyte (GB) o terabyte (TB). El valor predeterminado es MB. Especifique un número entero; no incluya decimales. *Size* es un valor entero. Para valores mayores que 2147483647, utilice unidades más grandes.

MAXSIZE *max_size* Especifica el tamaño máximo que puede alcanzar el archivo. MAXSIZE no se puede especificar si se especifica *os_file_name* como ruta UNC.

*max_size* Es el tamaño máximo del archivo. Se pueden utilizar los sufijos KB, MB, GB y TB. El valor predeterminado es MB. Especifique un número entero; no incluya decimales. Si no se especifica *max_size*, el archivo aumenta de tamaño hasta que el disco esté lleno. *Max_size* es un valor entero. Para valores mayores que 2147483647, utilice unidades más grandes.

UNLIMITED Especifica que el archivo crecerá hasta que el disco esté lleno. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si se especifica un crecimiento ilimitado para un archivo de registro, su tamaño máximo será de 2 TB, y para un archivo de datos será de 16 TB.

> [!NOTE]
> No hay un tamaño máximo cuando esta opción se especifica para un contenedor de FILESTREAM. Continúa creciendo hasta que el disco se completa.

FILEGROWTH *growth_increment* Especifica el incremento de crecimiento automático del archivo. El valor FILEGROWTH de un archivo no puede superar el valor MAXSIZE. FILEGROWTH no se puede especificar si se especifica *os_file_name* como ruta UNC. FILEGROWTH no se aplica a un grupo de archivos FILESTREAM.

*growth_increment* Es la cantidad de espacio que se agrega al archivo siempre que se necesita más espacio.

El valor se puede especificar en MB, KB, GB, TB o como porcentaje (%). Si se especifica un número sin los sufijos MB, KB o %, el valor predeterminado es MB. Cuando se especifica %, el incremento de crecimiento es el porcentaje especificado del tamaño del archivo en el momento en que tiene lugar el incremento. El tamaño especificado se redondea al múltiplo de 64 KB más cercano y el valor mínimo es 64 KB.

El valor 0 indica que el crecimiento automático está desactivado y no se permite más espacio.

Si no se especifica FILEGROWTH, los valores predeterminados son:

|Versión|Valores predeterminados|
|-------------|--------------------|
|A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|Datos: 64 MB. Archivos de registro: 64 MB.|
|A partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Datos: 1 MB. Archivos de registro: 10 %.|
|Antes de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|Datos: 10 %. Archivos de registro: 10 %.|

\<filegroup> Controla las propiedades del grupo de archivos. Filegroup no se puede especificar en una instantánea de base de datos.

FILEGROUP *filegroup_name* Es el nombre lógico del grupo de archivos.

*filegroup_name*
*filegroup_name* debe ser único en la base de datos y no puede ser los nombres PRIMARY ni PRIMARY_LOG suministrados por el sistema. El nombre puede ser una constante de caracteres o Unicode, o un identificador regular o delimitado. El nombre debe cumplir las reglas de los [identificadores](../../relational-databases/databases/database-identifiers.md).

CONTAINS FILESTREAM Especifica que el grupo de archivos almacena objetos binarios grandes (BLOB) de FILESTREAM en el sistema de archivos.

CONTAINS MEMORY_OPTIMIZED_DATA

**Válido para** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.

Especifica que el grupo de archivos almacena los datos memory_optimized en el sistema de archivos. Para obtener más información, vea [OLTP en memoria (optimización en memoria)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md). Solo se admite un grupo de archivos MEMORY_OPTIMIZED_DATA por cada base de datos. Para que los ejemplos de código que crean un grupo de archivos almacenen datos optimizados para memoria, consulte [Crear una tabla con optimización para memoria y un procedimiento almacenado compilado de forma nativa](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md).

DEFAULT Especifica que el grupo de archivos indicado es el grupo de archivos predeterminado de la base de datos.

*database_snapshot_name* Es el nombre de la nueva instantánea de base de datos. Los nombres de instantánea de base de datos deben ser únicos dentro de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y cumplir las reglas de los identificadores. *database_snapshot_name* puede tener un máximo de 128 caracteres.

ON **(** NAME **=** _logical\_file\_name_ **,** FILENAME **='** _os\_file\_name_ **')** [ **,** ... *n* ] Para la creación de una instantánea de base de datos, especifica una lista de archivos de la base de datos de origen. Para que la instantánea funcione, todos los archivos de datos deben especificarse individualmente. Sin embargo, no se permiten archivos de registro para las instantáneas de base de datos. Los grupos de archivos FILESTREAM no son compatibles con instantáneas de base de datos. Si se incluye un archivo de datos FILESTREAM en una cláusula CREATE DATABASE ON, se producirá un error en la instrucción y se generará el mensaje correspondiente.

Para obtener las descripciones de NAME y FILENAME, y sus valores, vea las descripciones de los valores \<filespec> equivalentes.

> [!NOTE]
> Cuando se crea una instantánea de base de datos, no se admiten las demás opciones de \<filespec> ni la palabra clave PRIMARY.

AS SNAPSHOT OF *source_database_name* Indica que la base de datos que se va a crear es una instantánea de base de datos de origen especificada en *source_database_name*. La instantánea y la base de datos de origen deben estar en la misma instancia.

Para obtener más información, vea [Instantáneas de base de datos](#database-snapshots) en la sección Notas.

## <a name="remarks"></a>Observaciones

Cada vez que se crea, modifica o quita una base de datos de usuario, se debe hacer una copia de seguridad de la [base de datos maestra](../../relational-databases/databases/master-database.md).

La instrucción `CREATE DATABASE` se debe ejecutar en el modo de confirmación automática (modo de administración de transacciones predeterminado) y no se permite en una transacción explícita o implícita.

Se puede usar una instrucción `CREATE DATABASE` para crear una base de datos y los archivos donde se almacena. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa la instrucción CREATE DATABASE de la siguiente manera:

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza una copia de la [base de datos modelo](../../relational-databases/databases/model-database.md) para inicializar la base de datos y sus metadatos.
2. Se asigna un GUID de Service Broker a la base de datos.
3. A continuación, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] rellena el resto de la base de datos con páginas vacías, excepto las páginas que tengan datos internos que registren cómo se emplea el espacio en la base de datos.

En una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]se pueden especificar 32.767 bases de datos como máximo.

Cada base de datos tiene un propietario que puede realizar actividades especiales en ella. El propietario es el usuario que crea la base de datos. El propietario de la base de datos se puede cambiar mediante [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md).

Algunas características de la base de datos dependen de características o capacidades que están en el sistema de archivos para que la base de datos funcione completamente. Estos son algunos ejemplos de características que dependen de un conjunto de características del sistema de archivos:

- DBCC CHECKDB
- Secuencia de archivos
- Copias de seguridad en línea con VSS e instantáneas de archivos
- Creación de instantáneas de bases de datos
- Grupo de archivos de datos optimizados para memoria

## <a name="database-files-and-filegroups"></a>Archivos y grupos de archivos de base de datos

Todas las bases de datos tienen al menos dos archivos (un *archivo principal* y un *archivo de registro de transacciones*) y un grupo de archivos como mínimo. Para cada base de datos pueden especificarse hasta 32.767 archivos y 32.767 grupos de archivos.

Cuando cree una base de datos, defina el mayor tamaño posible para los archivos de datos según la cantidad de datos máxima prevista para la base datos.

Se recomienda usar una red de área de almacenamiento (SAN), una red basada en iSCSI o un disco conectado localmente para almacenar los archivos de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], porque esta configuración optimiza el rendimiento y la confiabilidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="database-snapshots"></a>Instantáneas de base de datos

Se puede utilizar la instrucción `CREATE DATABASE` para crear una vista estática de solo lectura, es decir, una *instantánea* de la *base de datos de origen*. Desde el punto de vista transaccional, una instantánea de base de datos es coherente con la base de datos de origen tal como se encontraba en el momento de crear la instantánea. Una base de datos de origen puede tener varias instantáneas.

> [!NOTE]
> Al crear una instantánea de base de datos, la instrucción `CREATE DATABASE` no puede hacer referencia a archivos de registro, archivos sin conexión, archivos de restauración ni archivos inactivos.

Si se produce un error al crear una instantánea de base de datos, se sospecha de la instantánea y debe eliminarse. Para obtener más información, vea [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md).

Las instantáneas se conservan hasta que se eliminan mediante `DROP DATABASE`.

Para más información, vea [Instantáneas de base de datos](../../relational-databases/databases/database-snapshots-sql-server.md).

## <a name="database-options"></a>Opciones de base de datos

Cuando se crea una base de datos, se establecen automáticamente varias opciones de base de datos. Para obtener una lista de estas opciones, vea [Opciones de ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md).

## <a name="the-model-database-and-creating-new-databases"></a>Base de datos modelo y creación de nuevas bases de datos

Todos los objetos definidos por el usuario en la [base de datos modelo](../../relational-databases/databases/model-database.md) se copiarán en todas las bases de datos recién creadas. Puede agregar a la base de datos model todos los objetos (tablas, vistas, procedimientos almacenados, tipos de datos, etc.) que desee incluir en todas las bases de datos creadas recientemente.

Al especificar una instrucción `CREATE DATABASE <database_name>` sin parámetros de tamaño adicionales, el archivo de datos principal pasa a tener el mismo tamaño que el archivo principal de la base de datos modelo.

A menos que se especifique `FOR ATTACH`, todas las bases de datos nuevas heredan los valores de las opciones de la base de datos modelo. Por ejemplo, la opción de base de datos de reducción automática se establece en **true** en la base de datos model y en cualquier base de datos nueva que se cree. Si se cambian las opciones de la base de datos model, los nuevos valores de estas opciones se utilizarán en las nuevas bases de datos que se creen. Las operaciones de modificación de la base de datos model no afectan a las bases de datos existentes. Si se especifica FOR ATTACH en la instrucción CREATE DATABASE, la nueva base de datos hereda los valores de las opciones de la base de datos original.

## <a name="viewing-database-information"></a>Ver la información de la base de datos

Se pueden utilizar vistas de catálogo, funciones del sistema y procedimientos almacenados del sistema para devolver información sobre bases de datos, archivos y grupos de archivos. Para obtener más información, vea [Vistas del sistema](../language-reference.md).

## <a name="permissions"></a>Permisos

Requiere el permiso `CREATE DATABASE`, `CREATE ANY DATABASE` o `ALTER ANY DATABASE`.

Para mantener el control del uso del disco en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el permiso para crear bases de datos suele limitarse a un número reducido de cuentas de inicio de sesión.

En el ejemplo siguiente se proporciona el permiso para crear una base de datos al usuario de base de datos Fay.

```sql
USE master;
GO
GRANT CREATE DATABASE TO [Fay];
GO
```

### <a name="permissions-on-data-and-log-files"></a>Permisos en archivos de datos y de registro

En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], algunos permisos se establecen en los archivos de datos y de registro de cada base de datos. Siempre que se realizan las operaciones siguientes en una base de datos, se establecen estos permisos:

- Adjuntada
- Realización de copia de seguridad
- Creado
- Separada
- Modificada para agregar un nuevo archivo
- Restaurada

Los permisos evitan que los archivos se modifiquen accidentalmente si residen en un directorio sin restricción de permisos.

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)] no establece permisos de archivos de datos y registro.

## <a name="examples"></a>Ejemplos

### <a name="a-creating-a-database-without-specifying-files"></a>A. Crear una base de datos sin especificar archivos

En este ejemplo se crea la base de datos `mytest`, y los archivos principal y de registro de transacciones correspondientes. Como la instrucción no tiene elementos \<filespec>, el archivo de la base de datos principal tiene el tamaño del archivo principal de la base de datos de modelo. El registro de transacciones se establece en el mayor de los valores siguientes: 512 KB o el 25 % del tamaño del archivo de datos principal. Como no se ha especificado MAXSIZE, los archivos pueden crecer hasta llenar todo el espacio disponible en el disco. En este ejemplo también se muestra la forma de quitar la base de datos denominada `mytest` si existe, antes de crear la base de datos `mytest`.

```sql
USE master;
GO
IF DB_ID (N'mytest') IS NOT NULL
DROP DATABASE mytest;
GO
CREATE DATABASE mytest;
GO
-- Verify the database files and sizes
SELECT name, size, size*1.0/128 AS [Size in MBs]
FROM sys.master_files
WHERE name = N'mytest';
GO
```

### <a name="b-creating-a-database-that-specifies-the-data-and-transaction-log-files"></a>B. Crear una base de datos que especifica los archivos de datos y de registro de transacciones

En el ejemplo siguiente se crea la base de datos `Sales`. Debido a que no se usa la palabra clave PRIMARY, el primer archivo (`Sales_dat`) se convierte en el principal. Como no se especifica MB ni KB en el parámetro SIZE del archivo `Sales_dat` , se utiliza MB y el tamaño se asigna en megabytes. Cada vez que se crea, modifica o quita una base de datos de usuario, se debe hacer una copia de seguridad de la base de datos `Sales_log` se asigna en megabytes porque el sufijo `MB` se ha indicado explícitamente en el parámetro `SIZE` .

```sql
USE master;
GO
CREATE DATABASE Sales
ON
( NAME = Sales_dat,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\saledat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="c-creating-a-database-by-specifying-multiple-data-and-transaction-log-files"></a>C. Crear una base de datos especificando múltiples archivos de datos y de registro de transacciones

En el ejemplo siguiente se crea la base de datos `Archive`, que tiene tres archivos de datos de `100-MB` y dos archivos de registro de transacciones de `100-MB`. El archivo principal es el primer archivo de la lista y se especifica explícitamente con la palabra clave `PRIMARY`. Los archivos de registro de transacciones se especifican a continuación de las palabras clave `LOG ON`. Tenga en cuenta las extensiones usadas para los archivos en la opción `FILENAME`: `.mdf` se usa para archivos de datos principales, `.ndf` para archivos de datos secundarios y `.ldf` para archivos de registro de transacciones. En este ejemplo se coloca la base de datos en la unidad `D:`, en lugar de con la base de datos `master`.

```sql
USE master;
GO
CREATE DATABASE Archive
ON
PRIMARY
    (NAME = Arch1,
    FILENAME = 'D:\SalesData\archdat1.mdf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch2,
    FILENAME = 'D:\SalesData\archdat2.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
    ( NAME = Arch3,
    FILENAME = 'D:\SalesData\archdat3.ndf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20)
LOG ON
  (NAME = Archlog1,
    FILENAME = 'D:\SalesData\archlog1.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20),
  (NAME = Archlog2,
    FILENAME = 'D:\SalesData\archlog2.ldf',
    SIZE = 100MB,
    MAXSIZE = 200,
    FILEGROWTH = 20) ;
GO
```

### <a name="d-creating-a-database-that-has-filegroups"></a>D. Crear una base de datos que tenga grupos de archivos

En el ejemplo siguiente se crea la base de datos `Sales`, que tiene los siguientes grupos de archivos:

- El grupo de archivos principal, con los archivos `Spri1_dat` y `Spri2_dat`. El incremento de FILEGROWTH para estos archivos se especifica como `15%`.
- Un grupo de archivos denominado `SalesGroup1`, con los archivos `SGrp1Fi1` y `SGrp1Fi2`.
- Un grupo de archivos denominado `SalesGroup2`, con los archivos `SGrp2Fi1` y `SGrp2Fi2`.

En este ejemplo se colocan los archivos de datos y de registro en discos diferentes para mejorar el rendimiento.

```sql
USE master;
GO
CREATE DATABASE Sales
ON PRIMARY
( NAME = SPri1_dat,
    FILENAME = 'D:\SalesData\SPri1dat.mdf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
( NAME = SPri2_dat,
    FILENAME = 'D:\SalesData\SPri2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 15% ),
FILEGROUP SalesGroup1
( NAME = SGrp1Fi1_dat,
    FILENAME = 'D:\SalesData\SG1Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp1Fi2_dat,
    FILENAME = 'D:\SalesData\SG1Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
FILEGROUP SalesGroup2
( NAME = SGrp2Fi1_dat,
    FILENAME = 'D:\SalesData\SG2Fi1dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 ),
( NAME = SGrp2Fi2_dat,
    FILENAME = 'D:\SalesData\SG2Fi2dt.ndf',
    SIZE = 10,
    MAXSIZE = 50,
    FILEGROWTH = 5 )
LOG ON
( NAME = Sales_log,
    FILENAME = 'E:\SalesLog\salelog.ldf',
    SIZE = 5MB,
    MAXSIZE = 25MB,
    FILEGROWTH = 5MB ) ;
GO
```

### <a name="e-attaching-a-database"></a>E. Adjuntar una base de datos

En el ejemplo siguiente se separa la base de datos `Archive` creada en el ejemplo D y, a continuación, se adjunta mediante la cláusula `FOR ATTACH`. `Archive` se ha definido para contener varios archivos de datos y de registro. Sin embargo, dado que la ubicación de los archivos no ha cambiado desde que se crearon, solo es necesario especificar el archivo principal en la cláusula `FOR ATTACH`. A partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], los archivos de texto completo que formen parte de la base de datos que se va a adjuntar se adjuntarán con la base de datos.

```sql
USE master;
GO
sp_detach_db Archive;
GO
CREATE DATABASE Archive
  ON (FILENAME = 'D:\SalesData\archdat1.mdf')
  FOR ATTACH ;
GO
```

### <a name="f-creating-a-database-snapshot"></a>F. Crear una instantánea de base de datos

En el ejemplo siguiente se crea la instantánea de base de datos `sales_snapshot0600`. Debido a que la instantánea de base de datos es de solo lectura, no se puede especificar un archivo de registro. De acuerdo con la sintaxis, se especifican todos los archivos de la base de datos de origen, pero los grupos de archivos no se especifican.

La base de datos de origen en este ejemplo es la base de datos `Sales` creada en el ejemplo D.

```sql
USE master;
GO
CREATE DATABASE sales_snapshot0600 ON
    ( NAME = SPri1_dat, FILENAME = 'D:\SalesData\SPri1dat_0600.ss'),
    ( NAME = SPri2_dat, FILENAME = 'D:\SalesData\SPri2dt_0600.ss'),
    ( NAME = SGrp1Fi1_dat, FILENAME = 'D:\SalesData\SG1Fi1dt_0600.ss'),
    ( NAME = SGrp1Fi2_dat, FILENAME = 'D:\SalesData\SG1Fi2dt_0600.ss'),
    ( NAME = SGrp2Fi1_dat, FILENAME = 'D:\SalesData\SG2Fi1dt_0600.ss'),
    ( NAME = SGrp2Fi2_dat, FILENAME = 'D:\SalesData\SG2Fi2dt_0600.ss')
AS SNAPSHOT OF Sales ;
GO
```

### <a name="g-creating-a-database-and-specifying-a-collation-name-and-options"></a>G. Crear una base de datos y especificar un nombre de intercalación y sus opciones

En el ejemplo siguiente se crea la base de datos `MyOptionsTest`. Se especifica un nombre de intercalación y las opciones `TRUSTYWORTHY` y `DB_CHAINING` se establecen en `ON`.

```sql
USE master;
GO
IF DB_ID (N'MyOptionsTest') IS NOT NULL
DROP DATABASE MyOptionsTest;
GO
CREATE DATABASE MyOptionsTest
COLLATE French_CI_AI
WITH TRUSTWORTHY ON, DB_CHAINING ON;
GO
--Verifying collation and option settings.
SELECT name, collation_name, is_trustworthy_on, is_db_chaining_on
FROM sys.databases
WHERE name = N'MyOptionsTest';
GO
```

### <a name="h-attaching-a-full-text-catalog-that-has-been-moved"></a>H. Adjuntar un catálogo de texto completo que se ha movido

En el ejemplo siguiente se adjunta el catálogo de texto completo `AdvWksFtCat` junto con los archivos de datos y de registro de `AdventureWorks2012`. En el ejemplo, el catálogo de texto completo se mueve desde su ubicación predeterminada hasta una nueva ubicación `c:\myFTCatalogs`. Los archivos de datos y de registro permanecen en sus ubicaciones predeterminadas.

```sql
USE master;
GO
--Detach the AdventureWorks2012 database
sp_detach_db AdventureWorks2012;
GO
-- Physically move the full text catalog to the new location.
--Attach the AdventureWorks2012 database and specify the new location of the full-text catalog.
CREATE DATABASE AdventureWorks2012 ON
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_data.mdf'),
    (FILENAME = 'c:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Data\AdventureWorks2012_log.ldf'),
    (FILENAME = 'c:\myFTCatalogs\AdvWksFtCat')
FOR ATTACH;
GO
```

### <a name="i-creating-a-database-that-specifies-a-row-filegroup-and-two-filestream-filegroups"></a>I. Crear una base de datos que especifique un grupo de archivos de filas y dos grupos de archivos FILESTREAM

En el ejemplo siguiente se crea la base de datos `FileStreamDB`. La base de datos se crea con un grupo de archivos de filas y dos grupos de archivos FILESTREAM. Cada grupo de archivos contiene un archivo:

- `FileStreamDB_data` contiene los datos de fila. Contiene un archivo, `FileStreamDB_data.mdf` con la ruta de acceso predeterminada.
- `FileStreamPhotos` contiene los datos FILESTREAM. Contiene dos contenedores de datos FILESTREAM: `FSPhotos`, que se encuentra en `C:\MyFSfolder\Photos`, y `FSPhotos2`, que se encuentra en `D:\MyFSfolder\Photos`. Se marca como el grupo de archivos FILESTREAM predeterminado.
- `FileStreamResumes` contiene los datos FILESTREAM. Contiene un contenedor de datos FILESTREAM, `FSResumes`, que se encuentra en `C:\MyFSfolder\Resumes`.

```sql
USE master;
GO
-- Get the SQL Server data path.
DECLARE @data_path nvarchar(256);
SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

 -- Execute the CREATE DATABASE statement.
EXECUTE ('CREATE DATABASE FileStreamDB
ON PRIMARY
    (
    NAME = FileStreamDB_data
    ,FILENAME = ''' + @data_path + 'FileStreamDB_data.mdf''
    ,SIZE = 10MB
    ,MAXSIZE = 50MB
    ,FILEGROWTH = 15%
    ),
FILEGROUP FileStreamPhotos CONTAINS FILESTREAM DEFAULT
    (
    NAME = FSPhotos
    ,FILENAME = ''C:\MyFSfolder\Photos''
-- SIZE and FILEGROWTH should not be specified here.
-- If they are specified an error will be raised.
, MAXSIZE = 5000 MB
    ),
    (
      NAME = FSPhotos2
      , FILENAME = ''D:\MyFSfolder\Photos''
      , MAXSIZE = 10000 MB
     ),
FILEGROUP FileStreamResumes CONTAINS FILESTREAM
    (
    NAME = FileStreamResumes
    ,FILENAME = ''C:\MyFSfolder\Resumes''
    )
LOG ON
    (
    NAME = FileStream_log
    ,FILENAME = ''' + @data_path + 'FileStreamDB_log.ldf''
    ,SIZE = 5MB
    ,MAXSIZE = 25MB
    ,FILEGROWTH = 5MB
    )'
);
GO
```

### <a name="j-creating-a-database-that-has-a-filestream-filegroup-with-multiple-files"></a>J. Crear una base de datos que tenga un grupo de archivos FILESTREAM con varias filas

En el ejemplo siguiente se crea la base de datos `BlobStore1`. La base de datos se crea con un grupo de archivos de filas y un grupo de archivos FILESTREAM, `FS`. El grupo de archivos FILESTREAM contiene dos archivos, `FS1` y `FS2`. Después, la base de datos se altera agregando un tercer archivo, `FS3`, al grupo de archivos FILESTREAM.

```sql
USE master;
GO

CREATE DATABASE [BlobStore1]
CONTAINMENT = NONE
ON PRIMARY
(
    NAME = N'BlobStore1',
    FILENAME = N'C:\BlobStore\BlobStore1.mdf',
    SIZE = 100MB,
    MAXSIZE = UNLIMITED,
    FILEGROWTH = 1MB
),
FILEGROUP [FS] CONTAINS FILESTREAM DEFAULT
(  
    NAME = N'FS1',
    FILENAME = N'C:\BlobStore\FS1',
    MAXSIZE = UNLIMITED
),
(
    NAME = N'FS2',
    FILENAME = N'C:\BlobStore\FS2',
    MAXSIZE = 100MB
)
LOG ON
(
    NAME = N'BlobStore1_log',
    FILENAME = N'C:\BlobStore\BlobStore1_log.ldf',
    SIZE = 100MB,
    MAXSIZE = 1GB,
    FILEGROWTH = 1MB
);
GO

ALTER DATABASE [BlobStore1]
ADD FILE
(
    NAME = N'FS3',
    FILENAME = N'C:\BlobStore\FS3',
    MAXSIZE = 100MB
)
TO FILEGROUP [FS];
GO
```

## <a name="see-also"></a>Consulte también

- [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md)
- [Adjuntar y separar bases de datos](../../relational-databases/databases/database-detach-and-attach-sql-server.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [EVENTDATA](../../t-sql/functions/eventdata-transact-sql.md)
- [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)
- [sp_detach_db](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)
- [sp_removedbreplication](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md)
- [Instantáneas de base de datos](../../relational-databases/databases/database-snapshots-sql-server.md)
- [Mover archivos de base de datos](../../relational-databases/databases/move-database-files.md)
- [Bases de datos](../../relational-databases/databases/databases.md)
- [Objeto binario grande - Datos de blob](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_**
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL Database

## <a name="overview"></a>Información general

En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], esta instrucción puede utilizarse con un servidor Azure SQL Server para crear una base de datos única o una en un grupo elástico. Con esta instrucción, se especifica el nombre de la base de datos, la intercalación, el tamaño máximo, la edición, el objetivo de servicio y, si corresponde, el grupo elástico para la nueva base de datos. También puede usarse para crear la base de datos en un grupo elástico. Además, puede utilizarse para crear una copia de la base de datos en otro servidor de SQL Database.

## <a name="syntax"></a>Sintaxis

### <a name="create-a-database"></a>Crear una base de datos

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
{
  (<edition_options> [, ...n])
}
[ WITH <with_options> [,..n]]
[;]

<with_options> ::=
{
  CATALOG_COLLATION = { DATABASE_DEFAULT | SQL_Latin1_General_CP1_CI_AS }
  | BACKUP_STORAGE_REDUNDANCY = { 'LOCAL' | 'ZONE' | 'GEO' }
}

<edition_options> ::=
{

  MAXSIZE = { 100 MB | 500 MB | 1 ... 1024 ... 4096 GB }
  | ( EDITION = { 'Basic' | 'Standard' | 'Premium' | 'GeneralPurpose' | 'BusinessCritical' | 'Hyperscale' }
  | SERVICE_OBJECTIVE =
    { 'Basic' | 'S0' | 'S1' | 'S2' | 'S3' | 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
}
```

### <a name="copy-a-database"></a>Copia de una base de datos

```syntaxsql
CREATE DATABASE database_name
    AS COPY OF [source_server_name.] source_database_name
    [ ( SERVICE_OBJECTIVE =
      { 'Basic' |'S0' | 'S1' | 'S2' | 'S3'| 'S4'| 'S6'| 'S7'| 'S9'| 'S12'
      | 'P1' | 'P2' | 'P4'| 'P6' | 'P11' | 'P15'
      | 'GP_Gen4_1' | 'GP_Gen4_2' | 'GP_Gen4_3' | 'GP_Gen4_4' | 'GP_Gen4_5' | 'GP_Gen4_6'
      | 'GP_Gen4_7' | 'GP_Gen4_8' | 'GP_Gen4_9' | 'GP_Gen4_10' | 'GP_Gen4_16' | 'GP_Gen4_24'
      | 'GP_Gen5_2' | 'GP_Gen5_4' | 'GP_Gen5_6' | 'GP_Gen5_8' | 'GP_Gen5_10' | 'GP_Gen5_12' | 'GP_Gen5_14'
      | 'GP_Gen5_16' | 'GP_Gen5_18' | 'GP_Gen5_20' | 'GP_Gen5_24' | 'GP_Gen5_32' | 'GP_Gen5_40' | 'GP_Gen5_80'
      | 'GP_Fsv2_8' | 'GP_Fsv2_10' | 'GP_Fsv2_12' | 'GP_Fsv2_14' | 'GP_Fsv2_16' | 'GP_Fsv2_18'
      | 'GP_Fsv2_20' | 'GP_Fsv2_24' | 'GP_Fsv2_32' | 'GP_Fsv2_36' | 'GP_Fsv2_72'
      | 'GP_S_Gen5_1' | 'GP_S_Gen5_2' | 'GP_S_Gen5_4' | 'GP_S_Gen5_6' | 'GP_S_Gen5_8'
      | 'GP_S_Gen5_10' | 'GP_S_Gen5_12' | 'GP_S_Gen5_14' | 'GP_S_Gen5_16'
      | 'GP_S_Gen5_18' | 'GP_S_Gen5_20' | 'GP_S_Gen5_24' | 'GP_S_Gen5_32' | 'GP_S_Gen5_40'
      | 'BC_Gen4_1' | 'BC_Gen4_2' | 'BC_Gen4_3' | 'BC_Gen4_4' | 'BC_Gen4_5' | 'BC_Gen4_6'
      | 'BC_Gen4_7' | 'BC_Gen4_8' | 'BC_Gen4_9' | 'BC_Gen4_10' | 'BC_Gen4_16' | 'BC_Gen4_24'
      | 'BC_Gen5_2' | 'BC_Gen5_4' | 'BC_Gen5_6' | 'BC_Gen5_8' | 'BC_Gen5_10' | 'BC_Gen5_12' | 'BC_Gen5_14'
      | 'BC_Gen5_16' | 'BC_Gen5_18' | 'BC_Gen5_20' | 'BC_Gen5_24' | 'BC_Gen5_32' | 'BC_Gen5_40' | 'BC_Gen5_80'
      | 'BC_M_8' | 'BC_M_10' | 'BC_M_12' | 'BC_M_14' | 'BC_M_16' | 'BC_M_18'
      | 'BC_M_20' | 'BC_M_24' | 'BC_M_32' | 'BC_M_64' | 'BC_M_128'
      | 'HS_GEN4_1' | 'HS_GEN4_2' | 'HS_GEN4_4' | 'HS_GEN4_8' | 'HS_GEN4_16' | 'HS_GEN4_24'
      | 'HS_GEN5_2' | 'HS_GEN5_4' | 'HS_GEN5_8' | 'HS_GEN5_16' | 'HS_GEN5_24' | 'HS_GEN5_32' | 'HS_GEN5_48' | 'HS_GEN5_80'
      | { ELASTIC_POOL(name = <elastic_pool_name>) } })
   ]
[;]
```

## <a name="arguments"></a>Argumentos

*database_name* El nombre de la nueva base de datos. Este nombre debe ser único en el elemento [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y debe cumplir las reglas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para los identificadores. Para obtener más información, consulte [Identificadores](../../relational-databases/databases/database-identifiers.md).

*Collation_name* especifica la intercalación predeterminada de la base de datos. El nombre de intercalación puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Si no se especifica, a la base de datos se le asigna la intercalación predeterminada, que es SQL_Latin1_General_CP1_CI_AS.

Para obtener más información sobre los nombres de intercalación de Windows y de SQL, consulte [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md).

CATALOG_COLLATION especifica la intercalación predeterminada del catálogo de metadatos. *DATABASE_DEFAULT* especifica que el catálogo de metadatos usado para las vistas y las tablas del sistema se intercalará para coincidir con la intercalación predeterminada de la base de datos. Este es el comportamiento en SQL Server.

*SQL_Latin1_General_CP1_CI_AS* especifica que el catálogo de metadatos usado para las tablas y vistas del sistema se intercalará en una intercalación fija SQL_Latin1_General_CP1_CI_AS. Se trata de la configuración predeterminada en Azure SQL Database si no se especifica.

BACKUP_STORAGE_REDUNDANCY especifica cómo se replican la restauración a un momento dado y las copias de seguridad de retención a largo plazo de una base de datos. La restauración geográfica o la capacidad de recuperación desde una interrupción regional solo está disponible cuando se crea la base de datos con la redundancia de almacenamiento de copia de seguridad "GEO". A menos que se especifique explícitamente, las bases de datos creadas con T-SQL usan el almacenamiento de copia de seguridad con redundancia geográfica. 

> [!IMPORTANT]
> La opción BACKUP_STORAGE_REDUNDANCY para Azure SQL Database solo está disponible en la versión preliminar pública en el Sur de Brasil y normalmente en la región Sudeste de Asia de Azure.  

EDITION especifica el nivel de servicio de la base de datos.

Bases de datos únicas y agrupadas. Los valores disponibles son: "Basic", "Standard", "Premium", "GeneralPurpose", "BusinessCritical" e "Hyperscale".

MAXSIZE especifica el tamaño máximo de la base de datos. El valor de MAXSIZE debe ser válido para el valor de EDITION (nivel de servicio) especificado A continuación se indican los valores de MAXSIZE admitidos y los valores predeterminados (D) de los niveles de servicio.

> [!NOTE]
> El argumento **MAXSIZE** no es aplicable a bases de datos únicas en el nivel de servicio Hyperscale. Las bases de datos de nivel Hyperscale crecen según sea necesario, hasta 100 TB. El servicio SQL Database agrega almacenamiento automáticamente; no es necesario establecer un tamaño máximo.

**Modelo de DTU para bases de datos únicas y agrupadas en un servidor de SQL Database**

|**MAXSIZE**|**Basic**|**S0-S2**|**S3-S12**|**P1-P6**| **P11-P15** |
|:---|:---|:---|:---|:---|:---|
|100 MB|√|√|√|√|√|
|500 MB|√|√|√|√|√|
|1 GB|√|√|√|√|√|
|2 GB|√ (D)|√|√|√|√|
|5 GB|N/D|√|√|√|√|
|10 GB|N/D|√|√|√|√|
|20 GB|N/D|√|√|√|√|
|30 GB|N/D|√|√|√|√|
|40 GB|N/D|√|√|√|√|
|50 GB|N/D|√|√|√|√|
|100 GB|N/D|√|√|√|√|
|150 GB|N/D|√|√|√|√|
|200 GB|N/D|√|√|√|√|
|250 GB|N/D|√ (D)|√ (D)|√|√|
|300 GB|N/D|N/D|√|√|√|
|400 GB|N/D|N/D|√|√|√|
|500 GB|N/D|N/D|√|√ (D)|√|
|750 GB|N/D|N/D|√|√|√|
|1024 GB|N/D|N/D|√|√|√ (D)|
|Desde 1024 GB hasta 4096 GB en incrementos de 256 GB* |N/D|N/D|N/D|N/A|√|√|

\* P11 y P15 permiten un valor de MAXSIZE de hasta 4 TB, con 1024 GB como tamaño predeterminado. P11 y P15 pueden usar hasta 4 TB de almacenamiento incluido sin cargos adicionales. En el nivel Premium, un valor de MAXSIZE mayor de 1 TB está actualmente disponible en las regiones siguientes: Este de EE. UU. 2, Oeste de EE. UU., US Gov Virginia, Oeste de Europa, Centro de Alemania, Sudeste de Asia, Este de Japón, Este de Australia, Centro de Canadá y Este de Canadá. Para obtener más información sobre las limitaciones de recursos para el modelo de DTU, consulte [Límites de recursos de DTU](/azure/sql-database/sql-database-dtu-resource-limits).

El valor MAXSIZE para el modelo de DTU, si se especifica, tiene que ser válido según lo que se indica en la tabla anterior para el nivel de servicio especificado.

**Modelo de núcleo virtual**

**Uso general, proceso aprovisionado, Gen4 (parte 1)**

|MAXSIZE|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6|
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Tamaño máximo de datos (GB)|1024|1024|1024|1536|1536|1536|

**Uso general, proceso aprovisionado, Gen4 (parte 2)**

|MAXSIZE|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:----- | ------: |-------: |-------: |-------: |-------: |--------:|
|Tamaño máximo de datos (GB)|1536|3072|3072|3072|4096|4096|

**Uso general, proceso aprovisionado, Gen5 (parte 1)**

|MAXSIZE|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1536|1536|1536|1536|

**Uso general, proceso aprovisionado, Gen5 (parte 2)**

|MAXSIZE|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:----- | ------: |-------: |-------: |-------: |--------: |---------:|--------: |
|Tamaño máximo de datos (GB)|3072|3072|3072|4096|4096|4096|4096|

**Uso general, proceso aprovisionado: serie Fsv2 (parte 1)**

|MAXSIZE|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14|GP_Fsv2_16|GP_Fsv2_18|
|:----- | ------: | ------: | ------: | ------: | ------: | ------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1536|1536|

**Uso general, proceso aprovisionado: serie Fsv2 (parte 2)**

|MAXSIZE|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32|GP_Fsv2_36|GP_Fsv2_72|
|:----- | ------: | ------: | ------: | ------: | ------: |
|Tamaño máximo de datos (GB)|1536|1536|3072|3072|4096|

**Uso general, proceso sin servidor, Gen5 (parte 1)**

|MAXSIZE|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|GP_S_Gen5_6|GP_S_Gen5_8|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Número máximo de núcleos virtuales|1|2|4|6|8|

**Uso general, proceso sin servidor, Gen5 (parte 2)**

|MAXSIZE|GP_S_Gen5_10|GP_S_Gen5_12|GP_S_Gen5_14|GP_S_Gen5_16|
|:----- | ------: |-------: |-------: |-------: |
|Número máximo de núcleos virtuales|10|12|14|16|

**Uso general, proceso sin servidor, Gen5 (parte 3)**

|MAXSIZE|GP_S_Gen5_18|GP_S_Gen5_20|GP_S_Gen5_24|GP_S_Gen5_32|GP_S_Gen5_40|
|:----- | ------: |-------: |-------: |-------: |--------: |
|Número máximo de núcleos virtuales|18|20|24|32|40|

**Crítico para la empresa, proceso aprovisionado, Gen4 (parte 1)**

|Tamaño de proceso (objetivo de servicio)|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--------------- | ------: |-------: |-------: |-------: |-------: |-------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|

**Crítico para la empresa, proceso aprovisionado, Gen4 (parte 2)**

|Tamaño de proceso (objetivo de servicio)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--------------- | ------: |-------: |-------: |--------: |--------: |--------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1024|1024|1024|

**Crítico para la empresa, proceso aprovisionado, Gen5 (parte 1)**

|MAXSIZE|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:----- | ------: |-------: |-------: |-------: |---------: |--------:|--------: |
|Tamaño máximo de datos (GB)|1024|1024|1024|1536|1536|1536|1536|

**Crítico para la empresa, proceso aprovisionado, Gen5 (parte 2)**

|MAXSIZE|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:----- | -------: |--------: |--------: |--------: |--------: |---------:|--------: |
|Tamaño máximo de datos (GB)|3072|3072|3072|4096|4096|4096|4096|

**Crítico para la empresa, proceso aprovisionado: serie M (parte 1)**

|MAXSIZE|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:----- | -------: | -------: | -------: | -------: | -------: | -------: |
|Tamaño máximo de datos (GB)|512|640|768|896|1024|1152|

**Crítico para la empresa, proceso aprovisionado: serie M (parte 2)**

|MAXSIZE|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:----- | -------: | -------: | -------: | -------: | -------: |
|Tamaño máximo de datos (GB)|1280|1536|2048|4096|4096|

Si no se establece ningún valor de `MAXSIZE` al utilizar el modelo de núcleo virtual, el valor predeterminado es 32 GB. Para obtener más información sobre las limitaciones de recursos para el modelo basado en núcleo virtual, vea [Límites de recursos del núcleo virtual](/azure/sql-database/sql-database-dtu-resource-limits).

Las reglas siguientes se aplican a los argumentos MAXSIZE y EDITION:

- Si se especifica EDITION pero no se especifica MAXSIZE, se usa el valor predeterminado de la edición. Por ejemplo, si EDITION está establecido en Standard y el valor de MAXSIZE no se especifica, este último se establece automáticamente en 250 MB.
- Si no se especifica MAXSIZE ni EDITION, EDITION se establece en `GeneralPurpose` y MAXSIZE en 32 GB.

SERVICE_OBJECTIVE

- **Solo bases de datos únicas y agrupadas**

  - Especifica el tamaño de proceso (objetivo de servicio). Los valores disponibles para el objetivo de servicio son estos: `S0`, `S1`, `S2`, `S3`, `S4`, `S6`, `S7`, `S9`, `S12`, `P1`, `P2`, `P4`, `P6`, `P11`, `P15`, `GP_GEN4_1`, `GP_GEN4_2`, `GP_GEN4_3`, `GP_GEN4_4`, `GP_GEN4_5`, `GP_GEN4_6`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_7`, `GP_GEN4_8`, `GP_GEN4_9`, `GP_GEN4_10`, `GP_GEN4_16`, `GP_GEN4_24`, `BC_GEN4_1`, `BC_GEN4_2`, `BC_GEN4_3`, `BC_GEN4_4`, `BC_GEN4_5`, `BC_GEN4_6`, `BC_GEN4_7`, `BC_GEN4_8`, `BC_GEN4_9`, `BC_GEN4_10`, `BC_GEN4_16`, `BC_GEN4_24`, `GP_Gen5_2`, `GP_Gen5_4`, `GP_Gen5_6`, `GP_Gen5_8`, `GP_Gen5_10`, `GP_Gen5_12`, `GP_Gen5_14`, `GP_Gen5_16`, `GP_Gen5_18`, `GP_Gen5_20`, `GP_Gen5_24`, `GP_Gen5_32`, `GP_Gen5_40`, `GP_Gen5_80`, `GP_Fsv2_8`, `GP_Fsv2_10`, `GP_Fsv2_12`, `GP_Fsv2_14`, `GP_Fsv2_16`, `GP_Fsv2_18`, `GP_Fsv2_20`, `GP_Fsv2_24`, `GP_Fsv2_32`, `GP_Fsv2_36`, `GP_Fsv2_72`, `BC_Gen5_2`, `BC_Gen5_4`, `BC_Gen5_6`, `BC_Gen5_8`, `BC_Gen5_10`, `BC_Gen5_12`, `BC_Gen5_14`, `BC_Gen5_16`, `BC_Gen5_18`, `BC_Gen5_20`, `BC_Gen5_24`, `BC_Gen5_32`,`BC_Gen5_40`, `BC_Gen5_80`, `BC_M_8`, `BC_M_10`, `BC_M_12`, `BC_M_14`, `BC_M_16`, `BC_M_18`, `BC_M_20`, `BC_M_24`, `BC_M_32`, `BC_M_64`, `BC_M_128`.

- **Para bases de datos sin servidor**
- 
  - Especifica el tamaño de proceso (objetivo de servicio). Los valores disponibles para el objetivo de servicio son: `GP_S_Gen5_1`, `GP_S_Gen5_2`, `GP_S_Gen5_4`, `GP_S_Gen5_6`, `GP_S_Gen5_8`, `GP_S_Gen5_10`, `GP_S_Gen5_12`, `GP_S_Gen5_14`, `GP_S_Gen5_16`, `GP_S_Gen5_18`, `GP_S_Gen5_20`, `GP_S_Gen5_24`, `GP_S_Gen5_32`, `GP_S_Gen5_40`.

- **Para bases de datos en el nivel de servicio Hyperscale**

  - Especifica el tamaño de proceso (objetivo de servicio). Los valores disponibles para el objetivo de servicio son: `HS_GEN4_1` `HS_GEN4_2` `HS_GEN4_4` `HS_GEN4_8` `HS_GEN4_16`, `HS_GEN4_24`, `HS_Gen5_2`, `HS_Gen5_4`, `HS_Gen5_8`, `HS_Gen5_16`, `HS_Gen5_24`, `HS_Gen5_32`, `HS_Gen5_48`, `HS_Gen5_80`.

Para obtener las descripciones de los objetivos de servicio y más información sobre las combinaciones de tamaño, ediciones y objetivos de servicio, vea [¿Qué son los niveles de servicio de Azure SQL Database?](/azure/sql-database/sql-database-service-tiers). Si EDITION no admite el valor SERVICE_OBJECTIVE especificado, se devuelve un error. Para cambiar el valor SERVICE_OBJECTIVE de un nivel a otro (por ejemplo, de S1 a P1), también debe cambiar el valor EDITION. Para conocer las descripciones de los objetivos de servicio y obtener más información sobre las combinaciones de tamaño, ediciones y objetivos de servicio, consulte [Niveles de servicio y niveles de rendimiento de Azure SQL Database](/azure/azure-sql/database/purchasing-models), [Límites de recursos de DTU](/azure/sql-database/sql-database-dtu-resource-limits) y [Límites de recursos del núcleo virtual](/azure/sql-database/sql-database-dtu-resource-limits). Se ha quitado la compatibilidad para los objetivos de servicio de PRS. Si tiene preguntas, use este alias de correo electrónico: premium-rs@microsoft.com.

ELASTIC_POOL (name = \<elastic_pool_name>) **Se aplica a:** Solo bases de datos únicas y agrupadas. No se aplica a bases de datos en el nivel de servicio Hyperscale.
Para crear una base de datos en un grupo de bases de datos elásticas, establezca el valor SERVICE_OBJECTIVE de la base de datos en ELASTIC_POOL e indique el nombre del grupo. Para obtener más información, vea [Create and manage a SQL Database elastic pool](/azure/azure-sql/database/elastic-pool-overview) (Creación y administración de un grupo elástico de SQL Database).

AS COPY OF [source_server_name.]source_database_name **Se aplica a:** Solo bases de datos únicas y agrupadas.
Para copiar una base de datos al mismo o a otro servidor de [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

*nombre_del_servidor_de_origen*: el nombre del servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)] donde está ubicada la base de datos de origen. Este parámetro es opcional cuando la base de datos de origen y la de destino van a estar ubicadas en el mismo servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)].

> [!NOTE]
> El argumento `AS COPY OF` no admite nombres de dominio únicos completos. En otras palabras, si el nombre de dominio completo del servidor es `serverName.database.windows.net`, utilice solo `serverName` durante la copia de la base de datos.

*source_database_name*

El nombre de la base de datos que se va a copiar.

## <a name="remarks"></a>Observaciones

Las bases de datos de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] tienen varios parámetros predeterminados que se establecen al crear la base de datos. Para obtener más información sobre estos parámetros predeterminados, consulte la lista de valores de [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

`MAXSIZE` proporciona la capacidad de limitar el tamaño de la base de datos. Si el tamaño de la base de datos alcanza el valor `MAXSIZE`, se le mostrará el código de error 40544. Cuando esto sucede, no es posible insertar ni actualizar datos, ni tampoco crear nuevos objetos (como tablas, procedimientos almacenados, vistas y funciones). Sin embargo, todavía puede leer y eliminar datos, truncar tablas, quitar tablas e índices, y volver a generar índices. Seguidamente, puede actualizar `MAXSIZE` a un valor mayor que el tamaño actual de la base de datos o eliminar algunos datos para liberar espacio de almacenamiento. Puede haber un retraso de hasta quince minutos antes de que pueda insertar nuevos datos.

Para cambiar los valores de tamaño, edición u objetivo de servicio, use [ALTER DATABASE - Azure SQL Database](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true).

El argumento `CATALOG_COLLATION` solo está disponible durante la creación de la base de datos.

## <a name="database-copies"></a>Copias de la base de datos

**Se aplica a:** Solo bases de datos únicas y agrupadas.

La copia de una base de datos mediante la instrucción `CREATE DATABASE` es una operación asincrónica. Por tanto, no es necesaria una conexión con el servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)] durante todo el proceso de copia. La instrucción `CREATE DATABASE` devuelve el control al usuario una vez que se crea la entrada en sys.databases, pero antes de que se complete la operación de copia de la base de datos. Es decir, la instrucción `CREATE DATABASE` vuelve correctamente cuando la copia de la base de datos aún está en curso.

- Supervisión del proceso de copia en un servidor de [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]: Consulte las columnas `percentage_complete` o `replication_state_desc` de [dm_database_copies](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md) o la columna `state` en la vista **sys.databases**. También se puede usar la vista [sys.dm_operation_status](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md), ya que devuelve el estado de las operaciones de la base de datos, incluida la copia de esta.

Cuando se completa correctamente el proceso de copia, la base de datos de destino es transaccionalmente coherente con la base de datos de origen.

Se aplican las siguientes reglas semánticas y de sintaxis al uso del argumento `AS COPY OF`:

- El nombre del servidor de origen y el nombre del servidor del destino de la copia pueden ser iguales o diferentes. Si son iguales, este parámetro es opcional y se usa el contexto de servidor de la sesión actual de forma predeterminada.
- Los nombres de las bases de datos de origen y de destino deben especificarse, ser únicos y ajustarse a las reglas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para los identificadores. Para obtener más información, consulte [Identificadores](../../relational-databases/databases/database-identifiers.md).
- La instrucción `CREATE DATABASE` debe ejecutarse dentro del contexto de la base de datos maestra del servidor [!INCLUDE[ssSDS](../../includes/sssds-md.md)] donde se creará la nueva base de datos.
- Una vez finalizada la copia, la base de datos de destino debe administrarse como una base de datos independiente. Puede ejecutar las instrucciones `ALTER DATABASE` y `DROP DATABASE` en la nueva base de datos de forma independiente de la base de datos de origen. También puede copiar la nueva base de datos en otra base de datos nueva.
- Se puede seguir teniendo acceso a la base de datos de origen mientras la copia de la base de datos está en curso.

Para obtener más información, consulte [Crear una copia de una base de datos de SQL Azure mediante Transact-SQL](/azure/azure-sql/database/database-copy).

> [!IMPORTANT]
> De forma predeterminada, la copia de la base de datos se crea con la misma redundancia de almacenamiento de copia de seguridad que la de la base de datos de origen. No se admite el cambio de la redundancia de almacenamiento de copia de seguridad al crear una copia de base de datos a través de T-SQL. 


## <a name="permissions"></a>Permisos

Para crear una base de datos, el inicio de sesión debe ser uno de los siguientes:

- El inicio de sesión de entidad de seguridad a nivel de servidor
- El administrador de Azure AD para el Azure SQL Server local
- El inicio de sesión de un miembro del rol de base de datos `dbmanager`

**Requisitos adicionales para usar la sintaxis `CREATE DATABASE ... AS COPY OF`:** el inicio de sesión que ejecute la instrucción en el servidor local debe tener también al menos el rol de `db_owner` en el servidor de origen. Si el inicio de sesión se basa en la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el inicio de sesión que ejecute la instrucción en el servidor local debe tener un inicio de sesión correspondiente en el servidor de origen de [!INCLUDE[ssSDS](../../includes/sssds-md.md)], con un nombre y una contraseña idénticos.

## <a name="examples"></a>Ejemplos

### <a name="simple-example"></a>Ejemplo sencillo

Un ejemplo sencillo para crear una base de datos.

```sql
CREATE DATABASE TestDB1;
```

### <a name="simple-example-with-edition"></a>Ejemplo sencillo con Edition

Un ejemplo sencillo para crear una base de datos de uso general.

```sql
CREATE DATABASE TestDB2
( EDITION = 'GeneralPurpose' );
```

### <a name="example-with-additional-options"></a>Ejemplo con opciones adicionales

Un ejemplo en el que se usan varias opciones.

```sql
CREATE DATABASE hito
COLLATE Japanese_Bushu_Kakusu_100_CS_AS_KS_WS
( MAXSIZE = 500 MB, EDITION = 'GeneralPurpose', SERVICE_OBJECTIVE = 'GP_GEN4_8' ) ;
```

### <a name="creating-a-copy"></a>Crear una copia

Un ejemplo en el que se crea una copia de una base de datos.

**Se aplica a:** Solo bases de datos únicas y agrupadas.

```sql
CREATE DATABASE escuela
AS COPY OF school;
```

### <a name="creating-a-database-in-an-elastic-pool"></a>Crear una base de datos en un grupo elástico

Crea una nueva base de datos en un grupo denominado S3M100:

**Se aplica a:** Solo bases de datos únicas y agrupadas.

```sql
CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL ( name = S3M100 ) ) ;
```

### <a name="creating-a-copy-of-a-database-on-another-server"></a>Crear una copia de una base de datos en otro servidor

En el ejemplo siguiente se crea una copia de la base de datos db_original, denominada db_copy, con el tamaño de proceso P2 (objetivo de servicio) para una única base de datos. Esto es válido con independencia de si db_original está en un grupo elástico o un tamaño de proceso (objetivo de servicio) para una única base de datos.

**Se aplica a:** Solo bases de datos únicas y agrupadas.

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original ( SERVICE_OBJECTIVE = 'P2' );
```

En el siguiente ejemplo se crea una copia de la base de datos db_original, denominada db_copy, en un grupo elástico llamado ep1. Esto es válido con independencia de si db_original está en un grupo elástico o un tamaño de proceso (objetivo de servicio) para una única base de datos. Si db_original está en un grupo elástico con un nombre distinto, db_copy se sigue creando en ep1.

**Se aplica a:** Solo bases de datos únicas y agrupadas.

```sql
CREATE DATABASE db_copy
  AS COPY OF ozabzw7545.db_original
  (SERVICE_OBJECTIVE = ELASTIC_POOL( name = ep1 ) ) ;
```

### <a name="create-database-with-specified-catalog-collation-value"></a>Crear una base de datos con un valor de intercalación de catálogo especificado

En el siguiente ejemplo se establece la intercalación de catálogo en DATABASE_DEFAULT durante la creación de la base de datos, lo que establece la intercalación de catálogo como la misma que la intercalación de base de datos.

```sql
CREATE DATABASE TestDB3 COLLATE Japanese_XJIS_140 (MAXSIZE = 100 MB, EDITION = 'Basic')
  WITH CATALOG_COLLATION = DATABASE_DEFAULT
```

### <a name="create-database-using-zone-redundancy-for-backups"></a>Creación de bases de datos con redundancia de zona para copias de seguridad

En el ejemplo siguiente, se establece la redundancia de zona para las copias de seguridad de base de datos. Tanto las copias de seguridad de restauración a un momento dado como las copias de seguridad de retención a largo plazo (si se configuran) usarán la misma redundancia de almacenamiento de copia de seguridad.

```sql
CREATE DATABASE test_zone_redundancy 
  WITH BACKUP_STORAGE_REDUNDANCY = 'ZONE';
```

## <a name="see-also"></a>Consulte también

- [sys.dm_database_copies - Azure SQL Database](../../relational-databases/system-dynamic-management-views/sys-dm-database-copies-azure-sql-database.md)
- [ALTER DATABASE - Azure SQL Database](alter-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_**
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

## <a name="overview"></a>Información general

En Azure SQL Managed Instance, esta instrucción se utiliza para crear una base de datos. Al crear una base de datos en una instancia administrada, especifique el nombre de la base de datos y la intercalación.

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;]
```

> [!IMPORTANT]
> Para agregar archivos o establecer la independencia de una base de datos en una instancia administrada, use la instrucción [ALTER DATABASE](alter-database-transact-sql.md?tabs=sqldbmi).

## <a name="arguments"></a>Argumentos

*database_name* El nombre de la nueva base de datos. Este nombre debe ser único en el servidor SQL Server y debe cumplir las reglas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para los identificadores. Para obtener más información, consulte [Identificadores](../../relational-databases/databases/database-identifiers.md).

*Collation_name* especifica la intercalación predeterminada de la base de datos. El nombre de intercalación puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Si no se especifica, a la base de datos se le asigna la intercalación predeterminada, que es SQL_Latin1_General_CP1_CI_AS.

Para obtener más información sobre los nombres de intercalación de Windows y de SQL, consulte [COLLATE (Transact-SQL)](../../t-sql/statements/collations.md).

## <a name="remarks"></a>Observaciones

Las bases de datos de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] tienen varios parámetros predeterminados que se establecen al crear la base de datos. Para obtener más información sobre estos parámetros predeterminados, consulte la lista de valores de [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!IMPORTANT]
> La instrucción `CREATE DATABASE` debe ser la única de un lote [!INCLUDE[tsql](../../includes/tsql-md.md)].

Estas son limitaciones `CREATE DATABASE`:

- No se pueden definir archivos y grupos de archivos.
- No se admiten las opciones `WITH`.

  > [!TIP]
  > Como solución alternativa, use [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true). después de `CREATE DATABASE` para establecer opciones de base de datos y para agregar archivos.

## <a name="permissions"></a>Permisos

Para crear una base de datos, el inicio de sesión debe ser uno de los siguientes:

- El inicio de sesión de entidad de seguridad a nivel de servidor
- El administrador de Azure AD para el Azure SQL Server local
- El inicio de sesión de un miembro del rol de base de datos `dbcreator`

## <a name="examples"></a>Ejemplos

### <a name="simple-example"></a>Ejemplo sencillo

Un ejemplo sencillo para crear una base de datos.

```sql
CREATE DATABASE TestDB1;
```

## <a name="see-also"></a>Consulte también

Consulte [ALTER DATABASE](alter-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_**
    :::column-end:::
    :::column:::
        [Analytics Platform<br />System (PDW)](create-database-transact-sql.md?view=aps-pdw-2016&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="overview"></a>Información general

En Azure Synapse, esta instrucción se puede usar con un servidor de Azure SQL Database para crear una base de datos de SQL Analytics. Con esta instrucción, especifica el nombre de la base de datos, la intercalación, el tamaño máximo, la edición y el objetivo de servicio.

## <a name="syntax"></a>Sintaxis

### <a name="dedicated-sql-pool"></a>[Grupo de SQL dedicado](#tab/sqlpool)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
(
    [ MAXSIZE = {
          250 | 500 | 750 | 1024 | 5120 | 10240 | 20480 | 30720
        | 40960 | 51200 | 61440 | 71680 | 81920 | 92160 | 102400
        | 153600 | 204800 | 245760
      } GB ,
    ]
    EDITION = 'datawarehouse',
    SERVICE_OBJECTIVE = {
         'DW100' | 'DW200' | 'DW300' | 'DW400' | 'DW500' | 'DW600'
        | 'DW1000' | 'DW1200' | 'DW1500' | 'DW2000' | 'DW3000' | 'DW6000'
        |'DW100c' | 'DW200c' | 'DW300c' | 'DW400c' | 'DW500c'
        | 'DW1000c' | 'DW1500c' | 'DW2000c' | 'DW2500c' | 'DW3000c' | 'DW5000c'
        | 'DW6000c' | 'DW7500c' | 'DW10000c' | 'DW15000c' | 'DW30000c'
    }
)
[;]
```
### <a name="serverless-sql-pool"></a>[Grupo de SQL sin servidor](#tab/sqlod)
```syntaxsql
CREATE DATABASE database_name [ COLLATE collation_name ]
[;] 
```
---

## <a name="arguments"></a>Argumentos

*database_name* El nombre de la nueva base de datos. Este nombre debe ser único en el servidor SQL, que puede hospedar tanto bases de datos de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] como de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], y cumplir con las reglas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para identificadores. Para obtener más información, consulte [Identificadores](../../relational-databases/databases/database-identifiers.md).

*collation_name* Especifica la intercalación predeterminada de la base de datos. El nombre de intercalación puede ser un nombre de intercalación de Windows o un nombre de intercalación de SQL. Si no se especifica, a la base de datos se le asigna la intercalación predeterminada, que es SQL_Latin1_General_CP1_CI_AS.

Para obtener más información sobre los nombres de intercalación de Windows y SQL, consulte [COLLATE (Transact-SQL)](./collations.md).

*EDITION* Especifica el nivel de servicio de la base de datos. Para [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], use "datawarehouse".

*MAXSIZE* El valor predeterminado es 245, 760 GB (240 TB).

**Se aplica a:** Optimizado para Compute Gen1

El tamaño máximo permitido para la base de datos. La base de datos no puede superar el valor de MAXSIZE.

**Se aplica a:** Optimizado para Compute Gen2

Tamaño máximo permitido para los datos de almacenamiento de filas de la base de datos. Los datos almacenados en tablas de almacenamiento de filas, en el almacén delta de un índice de almacén de columnas o en un índice no agrupado de un índice de almacén de columnas agrupado no pueden superar el valor de MAXSIZE. Los datos comprimidos en formato de almacén de columnas no tienen un límite de tamaño y no están restringidos por el valor de MAXSIZE.

SERVICE_OBJECTIVE Especifica el tamaño de proceso (objetivo de servicio). Para más información sobre los objetivos de servicio para Azure Synapse, consulte [Unidades de almacenamiento de datos (DWU)](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu).

## <a name="general-remarks"></a>Notas generales

Use [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) para ver las propiedades de la base de datos.

Use [ALTER DATABASE - Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true) para cambiar el tamaño máximo o los valores del objetivo de servicio más adelante.

Azure Synapse se establece en COMPATIBILITY_LEVEL 130 y no se puede cambiar. Para más información, vea [Rendimiento de consultas mejorado con el nivel de compatibilidad 130 en Azure SQL Database](./alter-database-transact-sql-compatibility-level.md).

## <a name="permissions"></a>Permisos

Permisos necesarios:

- Inicio de sesión principal en el nivel de servidor, creado por el proceso de aprovisionamiento, o
- Miembro del rol de base de datos `dbmanager`.

## <a name="error-handling"></a>Tratamiento de errores

Si el tamaño de la base de datos alcanza el valor de MAXSIZE, recibirá el código de error 40544. Cuando esto sucede, no es posible insertar ni actualizar datos, ni tampoco crear nuevos objetos (como tablas, procedimientos almacenados, vistas y funciones). Sin embargo, todavía puede leer y eliminar datos, truncar tablas, quitar tablas e índices, y recompilar índices. Seguidamente, puede actualizar MAXSIZE a un valor mayor que el tamaño actual de la base de datos o eliminar algunos datos para liberar espacio de almacenamiento. Puede haber un retraso de hasta quince minutos antes de que pueda insertar nuevos datos.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Debe estar conectado a la base de datos maestra para crear una base de datos.

La instrucción `CREATE DATABASE` debe ser la única de un lote [!INCLUDE[tsql](../../includes/tsql-md.md)].

No se puede cambiar la intercalación de base de datos una vez creada la base de datos.

## <a name="examples-sssdwfull"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]

### <a name="a-simple-example"></a>A. Ejemplo sencillo
Un ejemplo sencillo de cómo crear una base de datos de almacenamiento de datos. De esta forma se crea la base de datos con el tamaño más pequeño, 10 240 GB, la intercalación predeterminada, SQL_Latin1_General_CP1_CI_AS, y la potencia de proceso más reducida, DW100.

```sql
CREATE DATABASE TestDW
(EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
```

### <a name="b-create-a-data-warehouse-database-with-all-the-options"></a>B. Crear una base de datos de almacenamiento de datos con todas las opciones
Un ejemplo de cómo crear un almacenamiento de datos de 10 terabytes usando todas las opciones.

```sql
CREATE DATABASE TestDW COLLATE Latin1_General_100_CI_AS_KS_WS
(MAXSIZE = 10240 GB, EDITION = 'datawarehouse', SERVICE_OBJECTIVE = 'DW1000');
```

## <a name="see-also"></a>Consulte también

- [ALTER DATABASE: Azure Synapse Analytics](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [CREATE TABLE: Azure Synapse Analytics](../../t-sql/statements/create-table-azure-sql-data-warehouse.md)
- [DROP DATABASE - Transact-SQL](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end
::: moniker range=">=aps-pdw-2016"

:::row:::
    :::column:::
        [SQL Server](create-database-transact-sql.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](create-database-transact-sql.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](create-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Analytics Platform<br />System (PDW) \*_**
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="analytics-platform-system"></a>Sistema de la plataforma de análisis

## <a name="overview"></a>Información general

En Analytics Platform System, esta instrucción se utiliza para crear una nueva base de datos en un dispositivo de Analytics Platform System. Use esta instrucción para crear todos los archivos asociados con una base de datos del dispositivo y para establecer las opciones de crecimiento automático y de tamaño máximo de las tablas de base de datos y del registro de transacciones.

## <a name="syntax"></a>Sintaxis

```syntaxsql
CREATE DATABASE database_name
WITH (
    [ AUTOGROW = ON | OFF , ]
    REPLICATED_SIZE = replicated_size [ GB ] ,
    DISTRIBUTED_SIZE = distributed_size [ GB ] ,
    LOG_SIZE = log_size [ GB ] )
[;]
```

## <a name="arguments"></a>Argumentos

*database_name* El nombre de la nueva base de datos. Para más información sobre los nombres de base de datos permitidos, vea las secciones sobre las reglas de nomenclatura de objetos y sobre los nombres reservados de base de datos en la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

AUTOGROW = ON | **OFF** Especifica si los parámetros *replicated_size*, *distributed_size* y *log_size* de esta base de datos van a crecer automáticamente según sea necesario y más allá de sus tamaños establecidos. El valor predeterminado es **OFF**.

Si AUTOGROW se establece en ON, *replicated_size*, *distributed_size* y *log_size* aumentarán según sea necesario (no en bloques con el tamaño inicial especificado) con cada inserción de datos, actualización u otra acción que requiera más almacenamiento del que ya hay asignado.

Si AUTOGROW se establece en OFF, el tamaño no aumentará automáticamente. [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] devolverá un error si se intenta realizar una acción que requiera que *replicated_size*, *distributed_size* o *log_size* aumenten por encima de su valor especificado.

AUTOGROW se establece en ON o en OFF invariablemente para todos los tamaños. Por ejemplo, no es posible establecer AUTOGROW en ON para *log_size* y no establecerlo para *replicated_size*.

*replicated_size* [ GB ] Un número positivo. Establece el tamaño (en gigabytes en forma de número entero o con decimales) del espacio total asignado a las tablas replicadas y a los datos correspondientes *en cada nodo de ejecución*. Para conocer los requisitos mínimos y máximos de *replicated_size*, vea la sección sobre valores mínimos y máximos de la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Si AUTOGROW está establecido en ON, las tablas replicadas podrán crecer más allá de este límite.

Si AUTOGROW está establecido en OFF, se devolverá un error si un usuario intenta crear una tabla replicada, insertar datos en una tabla replicada ya existente o actualizar una tabla replicada ya existente de forma que pueda aumentar el tamaño más allá de *replicated_size*.

*distributed_size* [ GB ] Un número positivo. Se trata del tamaño (en gigabytes en forma de número entero o con decimales) del espacio total asignado a las tablas distribuidas (y a los datos correspondientes) *en todo el dispositivo*. Para conocer los requisitos mínimos y máximos de *distributed_size*, vea la sección sobre valores mínimos y máximos de la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Si AUTOGROW está establecido en ON, las tablas distribuidas podrán crecer más allá de este límite.

Si AUTOGROW está establecido en OFF, se devolverá un error si un usuario intenta crear una tabla distribuida, insertar datos en una tabla distribuida ya existente o actualizar una tabla distribuida ya existente de forma que pueda aumentar el tamaño más allá de *distributed_size*.

*log_size* [ GB ] Un número positivo. Se trata del tamaño (en gigabytes en forma de número entero o con decimales) del registro de transacciones *en todo el dispositivo*.

Para conocer los requisitos mínimos y máximos de *log_size*, vea la sección sobre valores mínimos y máximos de la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

Si AUTOGROW está establecido en ON, el archivo de registro podrá crecer más allá de este límite. Use la instrucción [DBCC SHRINKLOG (Azure Synapse Analytics)](../../t-sql/database-console-commands/dbcc-shrinklog-azure-sql-data-warehouse.md) para reducir el tamaño de los archivos de registro a su tamaño original.

Si AUTOGROW está establecido en OFF, se devolverá un error al usuario si intenta realizar cualquier acción que suponga aumentar el tamaño del registro más allá de *log_size* en un nodo de ejecución concreto.

## <a name="permissions"></a>Permisos

Requiere el permiso `CREATE ANY DATABASE` en la base de datos maestra o pertenecer al rol fijo de servidor **sysadmin**.

En el ejemplo siguiente se proporciona el permiso para crear una base de datos al usuario de base de datos Fay.

```sql
USE master;
GO
GRANT CREATE ANY DATABASE TO [Fay];
GO
```

## <a name="general-remarks"></a>Notas generales

Las bases de datos se crean con el nivel de compatibilidad de base de datos 120, que es el nivel de compatibilidad de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. De este modo, se asegura que la base de datos podrá usar toda la funcionalidad de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] que PDW usa.

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

La instrucción CREATE DATABASE no se puede usar en una transacción explícita. Para más información, vea [Statements](../../t-sql/statements/statements.md) (Instrucciones).

Para más información sobre las limitaciones mínimas y máximas en las bases de datos, vea la sección sobre valores mínimos y máximos de la [!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)].

En el momento en que se cree una base de datos, debe haber suficiente espacio libre disponible *en cada nodo de ejecución* que permita asignar el total combinado de los siguientes tamaños:

- Base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con tablas que tengan el tamaño de *replicated_table_size*.
- Base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con tablas que tengan un tamaño de (*distributed_table_size*/número de nodos de ejecución).
- Registros de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que tengan un tamaño de (*log_size*/número de nodos de ejecución).

## <a name="locking"></a>Bloqueo

Toma un bloqueo compartido en el objeto DATABASE.

## <a name="metadata"></a>Metadatos

Después de que esta operación se realice correctamente, aparecerá una entrada relativa a esta base de datos en las vistas de metadatos [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) y [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).

## <a name="examples-sspdw"></a>Ejemplos: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

### <a name="a-basic-database-creation-examples"></a>A. Ejemplos básicos de creación de base de datos

En el siguiente ejemplo se crea la base de datos `mytest` con una asignación de almacenamiento de 100 GB por nodo de ejecución para las tablas replicadas, 500 GB por dispositivo para las tablas distribuidas y 100 GB por dispositivo para el registro de transacciones. En este ejemplo, AUTOGROW está desactivado de forma predeterminada.

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB );
```

En el siguiente ejemplo se crea la base de datos `mytest` con los mismos parámetros que el anterior, excepto que AUTOGROW está activado. Esto permite que la base de datos crezca fuera de los parámetros de tamaño especificado.

```sql
CREATE DATABASE mytest
  WITH
    (AUTOGROW = ON,
    REPLICATED_SIZE = 100 GB,
    DISTRIBUTED_SIZE = 500 GB,
    LOG_SIZE = 100 GB);
```

### <a name="b-creating-a-database-with-partial-gigabyte-sizes"></a>B. Crear una base de datos con tamaños de gigabyte parciales

En el siguiente ejemplo se crea la base de datos `mytest` con AUTOGROW desactivado y con una asignación de almacenamiento de 1,5 GB por nodo de ejecución para las tablas replicadas, 5,25 GB por dispositivo para las tablas distribuidas y 10 GB por dispositivo para el registro de transacciones.

```sql
CREATE DATABASE mytest
  WITH
    (REPLICATED_SIZE = 1.5 GB,
    DISTRIBUTED_SIZE = 5.25 GB,
    LOG_SIZE = 10 GB);
```

## <a name="see-also"></a>Consulte también

- [ALTER DATABASE - Analytics Platform System](../../t-sql/statements/alter-database-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)

::: moniker-end