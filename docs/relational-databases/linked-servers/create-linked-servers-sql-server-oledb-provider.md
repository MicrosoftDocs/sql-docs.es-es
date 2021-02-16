---
description: 'Consultas distribuidas de Microsoft SQL Server: Conectividad OLE DB'
title: Creación de un proveedor de servidor vinculado
ms.date: 07/01/2019
ms.prod: sql
ms.technology: ''
ms.prod_service: database-engine
ms.reviewer: MikeRayMSFT
ms.topic: conceptual
author: pmasl
ms.author: pelopes
manager: rothj
ms.custom: seo-dt-2019
ms.openlocfilehash: 75134031d7567dcc5a653d28ecb1c92a26891d23
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080416"
---
# <a name="microsoft-sql-server-distributed-queries-ole-db-connectivity"></a>Consultas distribuidas de Microsoft SQL Server: Conectividad OLE DB

En este artículo se describe cómo interactúa el procesador de consultas de Microsoft SQL Server con un proveedor OLE DB para habilitar consultas heterogéneas y distribuidas. Está destinado principalmente a los desarrolladores de proveedores OLE DB y asume un conocimiento sólido de la especificación OLE DB. El énfasis recae en la interfaz de OLE DB entre el procesador de consultas de SQL Server y el proveedor OLE DB, y no en la propia funcionalidad de consultas distribuidas. Para obtener una descripción completa de la funcionalidad de consultas distribuidas, vea [Servidores vinculados](../../relational-databases/linked-servers/linked-servers-database-engine.md).

## <a name="overview-and-terminology"></a>Introducción y terminología

 En Microsoft SQL Server, las consultas distribuidas permiten a los usuarios de SQL Server acceder a datos que no se encuentran en un servidor basado en SQL Server, sino que están dentro de otros servidores en los que se ejecuta SQL Server o en otros orígenes de datos que exponen una interfaz OLE DB. OLE DB proporciona una manera de acceder de forma uniforme a datos tabulares de orígenes de datos heterogéneos.

Una consulta distribuida, para el propósito de este artículo, es cualquier instrucción `SELECT`, `INSERT`, `UPDATE` o `DELETE` que hace referencia a tablas y conjuntos de filas de uno o varios orígenes de datos de OLE DB externos.

Una tabla remota es la que se almacena en un origen de datos OLE DB y que es externa al servidor que ejecuta SQL Server para la ejecución de la consulta. Una consulta distribuida accede a una o varias tablas remotas.

### <a name="ole-db-provider-categories"></a>Categorías de proveedores OLE DB

A continuación se muestra una clasificación de los proveedores OLE DB en función de sus características desde la perspectiva de las consultas distribuidas de SQL Server. Según su definición, no son mutuamente excluyentes; un proveedor determinado puede pertenecer a más de una de las categorías siguientes:

- Proveedores de comandos SQL

- Proveedores de índices

- Proveedores de tablas simples

- Proveedores de comandos que no son de SQL

#### <a name="sql-command-providers"></a>Proveedores de comandos SQL

Los proveedores que admiten el objeto `Command` con un dialecto estándar de SQL reconocido por SQL Server pertenecen a esta categoría. Los requisitos específicos para que SQL Server trate a un proveedor OLE DB determinado como un proveedor de comandos SQL son los siguientes:

- El proveedor debe admitir el objeto `Command` y todas sus interfaces OLE DB obligatorias: `ICommand`, `ICommandText`, `IColumnsInfo`, `ICommandProperties` e `IAccessor`.

- El dialecto SQL que admite el proveedor debe ser al menos SQL Subminimum. El proveedor debe notificar este dialecto por medio de la propiedad `DBPROP_SQLSUPPORT`.

Ejemplos de proveedores de comandos SQL son el Proveedor Microsoft OLE DB para SQL Server y el Proveedor Microsoft OLE DB para ODBC.

#### <a name="index-providers"></a>Proveedores de índices

Los proveedores de índices son los que admiten y exponen índices de acuerdo con OLE DB y permiten la búsqueda de tablas base basada en índices. Los requisitos específicos para que SQL Server considere a un proveedor OLE DB determinado como proveedor de índices son los siguientes:

- El proveedor debe admitir la interfaz `IDBSchemaRowset` con los conjuntos de filas de esquema TABLES, COLUMNS e INDEXES.

- El proveedor debe admitir la apertura de un conjunto de filas en un índice a través de `IOpenRowset` y, para ello, debe especificarse el nombre del índice y el de la tabla base correspondiente.

- El objeto `Index` debe admitir todas sus interfaces obligatorias: `IRowset`, `IRowsetIndex`, `IAccessor`, `IColumnsInfo`, `IRowsetInfo` e `IConvertTypes`.

- Los conjuntos de filas que se abran sobre la tabla base indexada (a través de `IOpenRowset`) deben admitir la interfaz `IRowsetLocate` para la ubicación de una fila basada en un marcador.

Si el proveedor OLE DB cumple los requisitos anteriores, los usuarios pueden establecer la opción de proveedor `Index As Access Path` para habilitar SQL Server con el fin de usar los índices del proveedor para evaluar las consultas. De forma predeterminada, SQL Server no intenta usar los índices del proveedor a menos que se establezca esta opción.

>[!NOTE]
>SQL Server admite varias opciones que afectan a cómo accede SQL Server a un proveedor OLE DB. Para establecer estas opciones se puede usar el cuadro de diálogo `Linked Server Properties` de SQL Server Enterprise Manager.

#### <a name="simple-table-providers"></a>Proveedores de tablas simples

Son proveedores que exponen la apertura de un conjunto de filas sobre una tabla base a través de la interfaz `IOpenRowset`. Estos proveedores no son ni de comandos SQL ni de índices; son la clase de proveedores más simples con los que pueden trabajar las consultas distribuidas de SQL Server.

Sobre este tipo de proveedores, SQL Server solo puede realizar recorridos de tablas durante la evaluación de las consultas distribuidas.

#### <a name="non-sql-command-providers"></a>Proveedores de comandos que no son de SQL

A esta categoría se corresponden los proveedores que admiten el objeto `Command` y sus interfaces obligatorias, pero no un dialecto estándar de SQL reconocido por SQL Server.

Dos ejemplos de proveedores de comandos que no son de SQL son el proveedor Microsoft OLE DB para el servicio de indexación y el [proveedor Microsoft OLE DB para el servicio Microsoft Active Directory](../../ado/guide/appendixes/microsoft-ole-db-provider-for-microsoft-active-directory-service.md).

### <a name="transact-sql-subset"></a>Subconjunto de Transact-SQL

Cada una de las clases siguientes de instrucciones Transact-SQL es compatible con las consultas distribuidas si el proveedor admite las interfaces OLE DB necesarias.

- Se permiten todas las instrucciones `SELECT` excepto para las instrucciones `SELECT` INTO con una tabla remota como tabla de destino.

- Se permiten las instrucciones `INSERT` sobre las tablas remotas si el proveedor admite las interfaces necesarias para la inserción. Para más información sobre los requisitos de OLE DB para INSERT, vea \"Instrucción INSERT\" más adelante en este artículo.

- Se permiten las instrucciones `UPDATE` y DELETE en tablas remotas si el proveedor cumple los requisitos de la interfaz de OLE DB en la tabla especificada. Para conocer los requisitos de la interfaz de OLE DB y las condiciones bajo las que se puede actualizar o eliminar una tabla remota, vea \"Instrucciones UPDATE y DELETE\" más adelante en este artículo.

### <a name="cursor-support"></a>Compatibilidad con cursores

Si el proveedor admite la funcionalidad de OLE DB necesaria, se admiten los cursores de instantánea y de conjunto de claves en las consultas distribuidas. No se permiten los cursores dinámicos sobre las consultas distribuidas. Una solicitud de usuario para un cursor dinámico en una consulta distribuida se degrada a un cursor de conjunto de claves.

Los cursores de instantánea se rellenan en el momento de apertura del cursor y el conjunto de resultados permanece inalterado; las actualizaciones, inserciones y eliminaciones en las tablas subyacentes no se reflejan en el cursor.

Los cursores de conjunto de claves se rellenan en el momento de apertura del cursor y el conjunto de resultados permanece inalterado a lo largo de la duración del cursor. Pero las actualizaciones y eliminaciones en las tablas subyacentes son visibles en el cursor a medida que se visitan las filas. Las inserciones en las tablas subyacentes que puedan afectar a la pertenencia al cursor no son visibles.

Una tabla remota se puede actualizar o eliminar mediante un cursor definido en una consulta y que hace referencia a la tabla remota, siempre que el proveedor cumpla las condiciones para realizar actualizaciones y eliminaciones en ella, por ejemplo table `UPDATE` \| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`. Para más información, vea \"Instrucciones UPDATE y DELETE\" más adelante en este artículo.

#### <a name="keyset-cursor-support-requirements"></a>Requisitos de compatibilidad con los cursores de conjunto de claves

En una consulta distribuida se admite un cursor de conjunto de claves si se cumplen todos los requisitos de la sintaxis de Transact-SQL y existe uno de estos casos:

- El proveedor OLE DB admite marcadores reutilizables en todas las tablas remotas de la consulta. Los marcadores reutilizables se pueden usar desde un conjunto de filas de una tabla dada y en otro conjunto de filas de la misma tabla. La compatibilidad con marcadores reutilizables se indica a través del conjunto de filas de esquema TABLES_INFO de `IDBSchemaRowset` al establecer la columna BOOKMARK_DURABILITY en BMK_DURABILITY_INTRANSACTION o una durabilidad más alta.

- Todas las tablas remotas exponen una clave única a través del conjunto de filas INDEXES de la interfaz `IDBSchemaRowset`. En la columna UNIQUE debe haber una entrada de índice con la columna UNIQUE establecida en VARIANT_TRUE.

No se admiten cursores de conjunto de claves en consultas distribuidas que impliquen la función *OpenQuery*.

#### <a name="updatable-keyset-cursor-requirements"></a>Requisitos de los cursores dinámicos actualizables

Una tabla remota se puede actualizar o eliminar mediante un cursor de conjunto de claves definido en una consulta distribuida, por ejemplo, `UPDATE` \| DELETE `<remote-table>` `WHERE` CURRENT OF `<cursor-name>`. Éstas son las condiciones que se deben cumplir para poder utilizar cursores actualizables en las consultas distribuidas:

- Los cursores actualizables se permiten si el proveedor también cumple las condiciones para las actualizaciones y eliminaciones en la tabla remota. Para más información, vea \"Instrucciones UPDATE y DELETE\" más adelante en este artículo.

- Todas las operaciones de cursor de conjunto de claves actualizable deben estar en una transacción definida por el usuario con un nivel de aislamiento de lectura repetible o superior. Es más, el proveedor debe admitir el uso de transacciones distribuidas con la interfaz `ITransactionJoin`.

## <a name="ole-db-provider-interaction-phases"></a>Fases de interacción de los proveedores OLE DB

 Todos los escenarios de ejecución de consultas distribuidas comparten seis operaciones comunes:

- Las operaciones de establecimiento de conexiones y recuperación de propiedades indican cómo se conecta SQL Server a un proveedor OLE DB y qué propiedades del proveedor se usan.

- Las opciones de resolución de nombres de tabla y de recuperación de metadatos indican cómo resuelve SQL Server el nombre de la tabla remota (lo que se especifica de dos formas: un nombre basado en un servidor vinculado o un nombre ad hoc) en el correspondiente objeto de datos en el proveedor. Esto también incluye los metadatos de tabla que recupera SQL Server del proveedor para poder compilar y optimizar una consulta distribuida.

- Las operaciones de administración de transacciones especifican todas las interacciones relacionadas con transacciones con el proveedor OLE DB.

- Las operaciones de control de tipos de datos indican cómo administra SQL Server los tipos de datos OLE DB cuando consume datos de un proveedor OLE DB o los exporta a uno mientras se procesa una consulta distribuida.

- Las operaciones de control de errores indican cómo usa SQL Server la información de error ampliada del proveedor.

- Las operaciones de seguridad especifican cómo interactúa la seguridad de SQL Server con la del proveedor.

### <a name="connection-establishment-and-property-retrieval"></a>Establecimiento de conexiones y recuperación de propiedades

SQL Server admite dos convenciones de nomenclatura para los objetos de datos remotos: nombres de cuatro partes basados en servidores vinculados y nombres ad hoc con la función `OPENROWSET`.

#### <a name="linked-server-based-names"></a>Nombres basados en servidores vinculados

Un servidor vinculado actúa como abstracción para un origen de datos de OLE DB. Un nombre basado en servidor vinculado es un nombre de cuatro partes con el formato `<linked-server>.<catalog>`. `<schema>.<object>`, donde `<linked-server>` es el nombre del servidor vinculado. SQL Server interpreta `<linked-server>` para derivar el proveedor OLE DB y los atributos de conexión que identifican el origen de datos para el proveedor. El origen de datos de OLE DB interpreta las otras tres partes del nombre para identificar la tabla remota específica. :::

#### <a name="ad-hoc-names"></a>Nombres ad hoc

Un nombre ad hoc es un nombre basado en la función `OPENROWSET` u `OPENDATASOURCE`. Incluye toda la información de conexión (es decir, el proveedor OLE DB que se va a usar, los atributos necesarios para identificar el origen de datos, el identificador de usuario y la contraseña) cada vez que se hace referencia a la tabla remota en una consulta distribuida.

No se permite el uso de nombres ad hoc de forma predeterminada, excepto para los miembros del rol sysadmin. Para poder usar nombres ad hoc en un proveedor OLE DB, la opción de proveedor `DisallowAdhocAccess` se debe establecer en `0`.

Si se usa un nombre de servidor vinculado, SQL Server extrae de la definición del servidor vinculado el nombre del proveedor OLE DB y las propiedades de inicialización para el proveedor. Si se usa un nombre ad hoc, SQL Server extrae la misma información de los argumentos de la función `OPENROWSET`.

Para obtener instrucciones detalladas sobre cómo configurar un servidor vinculado mediante un nombre de cuatro partes y la sintaxis basada en nombres ad hoc, vea [Creación de servidores vinculados](create-linked-servers-sql-server-database-engine.md).

### <a name="connecting-to-an-ole-db-provider"></a>Conexión a un proveedor OLE DB

Estos son los pasos generales que realiza SQL Server cuando se conecta a un proveedor OLE DB:

1. SQL Server crea un objeto de origen de datos.

   SQL Server usa el valor `ProgID` del proveedor para crear una instancia de su objeto de origen de datos (DSO). El valor ProgID se especifica como el parámetro `provider_name` de la configuración de un servidor vinculado o como el primer argumento de la función `OPENROWSET` en el caso de un nombre ad hoc.

   SQL Server crea instancias del DSO del proveedor a través de la interfaz de componentes del servicio OLE DB `IDataInitialize`. Esto permite que el administrador de componentes de servicio agregue sus servicios, como la compatibilidad con el desplazamiento y la actualización, sobre la funcionalidad nativa del proveedor. Además, la creación de instancias del proveedor a través de `IDataInitialize` permite al componente del servicio OLE DB agrupar las conexiones al proveedor, lo que reduce parte de la sobrecarga de conexión e inicialización.

   Se puede configurar la creación de instancias de un proveedor determinado en el mismo proceso que SQL Server o en su propio proceso. La creación de instancias en un proceso independiente protege el proceso de SQL Server de errores en el proveedor. Al mismo tiempo, existe una sobrecarga de rendimiento asociada con la serialización de las llamadas OLE DB fuera de proceso desde SQL Server. Se puede configurar la creación de una instancia del proveedor en proceso o fuera de proceso si se establece la opción de proveedor `Allow In Process`. Para más información, vea [Configuración de opciones de proveedor](../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md).

   Para obtener más información sobre componentes de servicio de OLE DB y la agrupación de sesiones, consulte los requisitos del proveedor en la documentación de OLE DB.

2. El origen de datos se ha inicializado.

   Una vez que se ha creado el DSO, la interfaz `IDBProperties` establece la propiedad de inicialización DBPROP_INIT_TIMEOUT si la opción de configuración del servidor `remote login timeout` es mayor que 0; esta propiedad es obligatoria.

   Estas propiedades se establecen si están especificadas o implícitas en la definición del servidor vinculado o en el segundo argumento de la función `OPENROWSET`:

   - `DBPROP_INIT_PROVIDERSTRING`

   - `DBPROP_INIT_DATASOURCE`

   - `DBPROP_INIT_LOCATION`

   - `DBPROP_INIT_CATALOG`

   - `DBPROP_AUTH_USERID`

   - `DBPROP_AUTH_PASSWORD`

   Una vez que se han establecido estas propiedades, se llama a `IDBInitialize::Initialize` para inicializar el DSO con las propiedades especificadas.

3. SQL Server recopila información específica del proveedor.

   SQL Server recopila varias propiedades de proveedor que se usarán en la evaluación de la consulta distribuida; estas propiedades se recuperan mediante una llamada a `IDBProperties::GetProperties`. Todas estas propiedades son opcionales; pero si se admiten todas las propiedades relevantes, SQL Server puede sacar el máximo partido de las funciones del proveedor. Por ejemplo, se necesita `DBPROP_SQLSUPPORT` para determinar si SQL Server puede enviar consultas al proveedor. Si esta propiedad no se admite, SQL Server no usará el proveedor remoto como proveedor de comandos SQL aunque lo sea. En la tabla siguiente, en la columna Valor predeterminado se indica qué valor asume SQL Server si el proveedor no admite la propiedad.

Propiedad| Valor predeterminado| Usar |
|:----|:----|:----|
|`DBPROP_DBMSNAME`|Ninguno|Se usa para mensajes de error.|
|`DBPROP_DBMSVER` |Ninguno|Se usa para mensajes de error.|
|`DBPROP_PROVIDERNAME`|Ninguno|Se usa para mensajes de error.|
|`DBPROP_PROVIDEROLEDBVER1`|1.5|Se usa para determinar la disponibilidad de las características de la versión 2.0.
|`DBPROP_CONCATNULLBEHAVIOR`|Ninguno|Se usa para determinar si el comportamiento de concatenación `NULL` del proveedor es el mismo que el de SQL Server.|
|`DBPROP_NULLCOLLATION`|Ninguno|Permite la ordenación o el uso de índices solo si `NULLCOLLATION` coincide con el comportamiento de intercalación NULL de la instancia de SQL Server.|
|`DBPROP_OLEOBJECTS`|Ninguno|Determina si el proveedor admite interfaces de almacenamiento estructurado para columnas de objetos de datos de gran tamaño.|
|`DBPROP_STRUCTUREDSTORAGE`|Ninguno|Determina cuáles de las interfaces de almacenamiento estructurado se admiten para los tipos de objetos grandes (entre `ILockBytes`, `Istream` e `ISequentialStream`).|
|`DBPROP_MULTIPLESTORAGEOBJECTS`|Falso|Determina si se puede abrir más de una columna de objetos grandes al mismo tiempo.|
|`DBPROP_SQLSUPPORT`|Ninguno|Determina si se pueden enviar consultas SQL al proveedor.|
|`DBPROP_CATALOGLOCATION`|`DBPROPVAL_CL_START`|Se usa para crear nombres de tabla de varias partes.
|`SQLPROP_DYNAMICSQL`|Falso|Propiedad específica de SQL Server: si devuelve `VARIANT_TRUE`, indica que se admiten marcadores de parámetro `?` para la ejecución de consultas con parámetros.
|`SQLPROP_NESTEDQUERIES`|Falso|Propiedad específica de SQL Server: si devuelve `VARIANT_TRUE`, indica que el proveedor admite instrucciones `SELECT` anidadas en la cláusula `FROM`.
|`SQLPROP_GROUPBY`|Falso|Propiedad específica de SQL Server: si devuelve `VARIANT_TRUE`, indica que el proveedor admite la cláusula GROUP BY en la instrucción `SELECT` como se especifica en el estándar SQL-92.
|`SQLPROP_DATELITERALS `|Falso|Propiedad específica de SQL Server: si devuelve `VARIANT_TRUE`, indica que el proveedor admite literales de fecha y hora según la sintaxis Transact-SQL de SQL Server.
|`SQLPROP_ANSILIKE `|Falso|Propiedad específica de SQL Server: esta propiedad es de interés para un proveedor que admite el nivel Minimum de SQL y el operador `LIKE` de acuerdo al nivel de entrada SQL-92 (\'%\' y \'_\' como caracteres comodín).
|`SQLPROP_SUBQUERIES `|Falso|Propiedad de SQL Server: esta propiedad es de interés para un proveedor que admita el nivel Minimum de SQL. Esta propiedad indica que el proveedor admite subconsultas, como se especifica en el nivel de entrada SQL-92. Esto incluye las subconsultas de la lista `SELECT` y la cláusula `WHERE` con compatibilidad con las subconsultas correlacionadas, y los operadores `IN`, `EXISTS`, `ALL` y `ANY`.
|`SQLPROP_INNERJOIN`|Falso|Propiedad específica de SQL Server: esta propiedad es de interés para los proveedores que admiten el nivel Minimum de SQL. Esta propiedad indica la compatibilidad con las combinaciones que usan varias tablas en la cláusula `FROM`. ------ ---

Los tres literales siguientes se recuperan de `IDBInfo::GetLiteralInfo`: `DBLITERAL_CATALOG_SEPARATOR`, `DBLITERAL_SCHEMA_SEPARATOR` (para crear un nombre de objeto completo a partir de sus partes de catálogo, esquema y nombre de objeto) y `DBLITERAL_QUOTE` (para incluir entre comillas los nombres de identificador en una consulta SQL enviada al proveedor).

Si el proveedor no admite los literales de separador, SQL Server usa un punto (.) como carácter separador predeterminado. Si el proveedor solo admite el carácter separador de catálogo, pero no el de esquema, SQL Server también usa el carácter separador de catálogo como carácter separador de esquema. Si el proveedor no admite `DBLITERAL_QUOTE`, SQL Server usa una comilla simple (`'`) como carácter de comillas.

>[!NOTE]
>Si los literales de separador de nombre del proveedor no coinciden con estos valores predeterminados, el proveedor debe exponerlos a través de `IDBInfo` para que SQL Server acceda a sus tablas a través de nombres de cuatro partes. Si no se exponen estos literales, en este tipo de proveedor solo se pueden usar consultas de paso a través.

Para obtener información sobre cómo exponer las propiedades `SQLPROP_DYNAMICSQL` y `SQLPROP_NESTEDQUERIES`, vea [Propiedades específicas de SQL Server](#appendixc).

### <a name="table-name-resolution-and-meta-data-retrieval"></a>Resolución de nombres de tabla y recuperación de metadatos

SQL Server resuelve un nombre de tabla remota determinado en una consulta distribuida en una tabla o vista específica de un origen de datos de OLE DB. Los esquemas de nomenclatura ad hoc y basado en servidores vinculados dan como resultado un nombre de tres partes que el proveedor debe interpretar. En el caso del nombre basado en servidor vinculado, las tres últimas partes del nombre de cuatro partes forman los nombres de catálogo, esquema y objeto. En el caso del nombre ad hoc, el tercer argumento de la función `OPENROWSET` especifica un nombre de tres partes que describe los nombres de catálogo, esquema y objeto. Uno de los nombres de catálogo y esquema, o los dos, pueden estar vacíos. (Un nombre de cuatro partes con un nombre de catálogo y un nombre de esquema vacíos sería similar a `<server-name>...<object-name>`). En ese caso, SQL Server usa `NULL` como el valor correspondiente que se va a buscar en las tablas del conjunto de filas de esquema.

Las reglas de resolución de nombres y los pasos de recuperación de metadatos que usa SQL Server dependen de si el proveedor admite la interfaz `IDBSchemaRowset` en el objeto `Session`.

Si se admite `IDBSchemaRowset`, se usan los conjuntos de filas de esquema `TABLES`, `COLUMNS`, `INDEXES` y `TABLES_INFO` de la interfaz `IDBSchemaRowset`. (El conjunto de filas de esquema `TABLES_INFO` se define en OLE DB 2.0). SQL Server restringe los conjuntos de filas de esquema devueltos por la conjunto de filas de interfaz `IDBSchemaRowset` para buscar filas de esquema que coincidan con las partes del nombre de tabla remota especificadas. A continuación se muestran las reglas relacionadas con las restricciones admitidas por el proveedor en los conjuntos de filas de esquema y cómo las usa SQL Server para recuperar los metadatos de una tabla remota:

- Las restricciones en las columnas `TABLE_NAME` y `COLUMN_NAME` siempre son obligatorias.

- Si el proveedor admite una restricción en `TABLE_CATALOG` (o `TABLE_SCHEMA`), SQL Server usa esa restricción en `TABLE_CATALOG` (o `TABLE_SCHEMA`). Si no se especifica el nombre de catálogo (o de esquema) en el nombre de la tabla remota, se usa un valor `NULL` como el valor de restricción correspondiente. Si se especifica un nombre de catálogo (o de esquema), el proveedor debe admitir la restricción correspondiente en `TABLE_CATALOG` (o `TABLE_SCHEMA`).

- El proveedor debe admitir la restricción en la columna `TABLE_SCHEMA` tanto en `TABLES` como en `COLUMNS`, o bien en ninguna. El proveedor debe admitir la restricción del nombre de catálogo en los conjuntos de filas `TABLES` y `COLUMNS`, o bien en ninguno de ellos.

- Si se admiten restricciones en INDEXES, el proveedor debe admitir la restricción de esquema en `TABLES` y los conjuntos de filas INDE`XES or support them on neither. The provider must either support catalog name restriction on both `TABLES` and `INDEXES, o bien en ninguno de ellos.

Del conjunto de filas de esquema `TABLES`, SQL Server recupera las columnas `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `TABLE_TYPE` y `TABLE_GUID` mediante el establecimiento de restricciones de acuerdo con las reglas anteriores.

Del conjunto de filas de esquema `COLUMNS`, SQL Server recupera la columnas `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `COLUMN_NAME`, `COLUMN_GUID`, `ORDINAL_POSITION`, `COLUMN_FLAGS`, `IS_NULLABLE`, `DATA_TYPE`, `TYPE_GUID`, `CHARACTER_MAXIMUM_LENGTH`, `NUMERIC_PRECISION` y `NUMERIC_SCALE`. `COLUMN_NAME`, `DATA_TYPE` y `ORDINAL_POSITION` deben devolver valores que no sean NULL válidos. Si `DATA_TYPE` es `DBTYPE_NUMERIC` o `DBTYPE_DECIMAL`, los valores `NUMERIC_PRECISION` y `NUMERIC_SCALE` correspondientes deben ser valores no NULL válidos.

En el conjunto de filas de esquema `INDEXES` opcional, SQL Server busca índices en la tabla remota especificada mediante el establecimiento de restricciones según las reglas anteriores. De las entradas de índice coincidentes que se han encontrado, SQL Server recupera las columnas `TABLE_CATALOG`, `TABLE_SCHEMA`, `TABLE_NAME`, `INDEX_CATALOG`, `INDEX_SCHEMA`, `INDEX_NAME`, `PRIMARY_KEY`, `UNIQUE`, `CLUSTERED`, `FILL_FACTOR`, `ORDINAL_POSITION`, `COLUMN_NAME`, `COLLATION`, `CARDINALITY` y `PAGES`.

En el conjunto de filas `TABLES_INFO` opcional, SQL Server busca información adicional en la tabla remota especificada, como la compatibilidad con marcadores, el tipo y la longitud del marcador. Se usan todas las columnas excepto la columna `DESCRIPTION` del conjunto de filas `TABLES_INFO`. La información del conjunto de filas `TABLES_INFO` se usa de esta forma:

- La columna `BOOKMARK_DURABILITY` se usa para implementar cursores de conjunto de claves más eficaces. Si esta columna tiene un valor de `BMK_DURABILITY_INTRANSACTION` o un valor de durabilidad más alto, SQL Server usa la recuperación basada en marcadores y las actualizaciones de las filas de la tabla remota para implementar un cursor de conjunto de claves.

- Las columnas `BOOKMARK_TYPE`, `BOOKMARK_DATA TYPE` y `BOOKMARK_MAXIMUM_LENGTH` se usan para determinar los metadatos de marcador en el momento de compilación de la consulta. Si estas columnas no se admiten, SQL Server abre el conjunto de filas de la tabla base mediante `IOpenRowset` durante la compilación para obtener la información del marcador.

Si no se admite `IDBSchemaRowset` y en el nombre de la tabla remota se incluye un nombre de catálogo o de esquema, SQL Server requiere `IDBSchemaRowset` y devuelve un error. Pero si no se suministran los nombres de catálogo ni de esquema, SQL Server abre el conjunto de filas correspondiente a la tabla remota y recupera los metadatos de columna de la interfaz obligatoria `IColumnsInfo` del objeto de conjunto de filas.

SQL Server abre el conjunto de filas correspondiente a la tabla mediante una llamada a `IOpenRowset::OpenRowset`. El nombre de tabla proporcionado a `OPENROWSET` se crea a partir de las partes de nombre de catálogo, esquema y objeto.

- Cada una de las partes del nombre (`catalog`, `schema`, `object name`) se incluye entre comillas con el carácter de comillas del proveedor (`DBLITERAL_QUOTE`) y, después, se concatenan con el carácter `DBLITERAL_CATALOG_SEPARATOR` y el carácter `DBLITERAL_SCHEMA_SEPARATOR` se inserta entre ellas. La construcción del nombre sigue las reglas de OLE DB en `IOpenRowset`.

- Los metadatos de columna para la tabla se recuperan a través de `IColumnsInfo::GetColumnInfo` después de abrir el objeto de conjunto de filas.

Si no se admite `IDBSchemaRowset` con los conjuntos de filas TABLES, COLUMNS y TABLES_INFO, SQL Server abre dos veces el conjunto de filas en la tabla base: una vez durante la compilación de la consulta para recuperar los metadatos y otra durante la ejecución de la consulta. Los proveedores que incurren en efectos secundarios al abrir el conjunto de filas (por ejemplo, ejecutan código que modifica el estado de un dispositivo en tiempo real, envían un correo electrónico, ejecutan código arbitrario proporcionado por el usuario) deben tener en cuenta este comportamiento.

### <a name="statistics-retrieval"></a>Recuperación de estadísticas

Si el proveedor admite estadísticas de distribución en las tablas base, SQL Server las usará. Hay dos tipos de estadísticas que son de interés para el procesador de consultas de SQL Server:

- **Cardinalidades de columna (o tupla**). Es el número de valores únicos de una columna (o una combinación de columnas) de una tabla. Se puede usar para calcular la selectividad de predicados en las columnas. Un proveedor que admita estadísticas de distribución debe admitir al menos un tipo de cardinalidad.

- **Histogramas**. Si la distribución de valores no es uniforme, el número de valores únicos no es suficiente para estimar con precisión la selectividad de los predicados. En este caso, se puede proporcionar un histograma que ofrezca información más detallada sobre la distribución de valores de columna en una tabla.

La disponibilidad de las estadísticas permite que el optimizador de consultas de SQL Server calcule mejor las cardinalidades de las operaciones intermedias en una consulta, lo que le permite generar mejores planes de ejecución para ellas.

El proveedor OLE DB debe admitir las estadísticas de distribución de la siguiente manera:

- **Obligatoria**. Admite las propiedades (1) `DBPROP_TABLESTATISTICS`, que indica si se admiten cardinalidades de columna o tupla, y si se admiten histogramas, y (2) `DBPROP_OPENROWSETSUPPORT`, lo que indica que se usa el bit `DBPROPVAL_ORS_HISTOGRAM`, con independencia de que se admitan los histogramas.

- **Obligatoria**. El conjunto de filas de esquema `TABLE_STATISTICS`. El conjunto de filas de esquema `TABLE_STATISTICS` enumera las estadísticas disponibles en una base de datos determinada. También incluye las cardinalidades de columna y tupla en el conjunto de filas de esquema e indica si se admiten histogramas en las columnas específicas. Para que SQL Server use estadísticas, las columnas `TABLE_NAME`, `STATISTICS_NAME`, `STATISTICS_TYPE`, `COLUMN_NAME` y `ORDINAL_POSITION` son obligatorias en este conjunto de filas de esquema. De `COLUMN_CARDINALITY` o `TUPLE_CARDINALITY`, al menos una es obligatoria. Si se admiten los histogramas, `NO_OF_RANGES` también es obligatoria.

- **Opcional**. Opcionalmente, si el proveedor admite histogramas, debe admitir una mejora en el método `IOpenRowset::OpenRowset` que permite abrir un conjunto de filas de histograma mediante la especificación del valor `DBID` de la estadística correspondiente.

Para obtener información completa sobre las interfaces de estadísticas, consulte la especificación OLE DB 2.6.

### <a name="constraints"></a>Restricciones

El optimizador de consultas de SQL Server también usa las restricciones `CHECK` definidas en las tablas base de un origen de datos remoto, si el proveedor OLE DB admite el conjunto de filas de esquema `CHECK_CONSTRAINTS_BY_TABLE` de OLE DB 2.6. La columna `CHECK_CLAUSE` del conjunto de filas de esquema debe devolver el predicado de la cláusula `CHECK` en una sintaxis compatible con SQL-92. El optimizador de consultas usa la información de restricciones para eliminar o simplificar los predicados que se sabe que son siempre false o siempre true debido a la presencia de una restricción CHECK en la tabla.

### <a name="transaction-management"></a>Administración de transacciones

SQL Server admite el acceso a datos distribuidos basado en transacciones mediante las interfaces OLE DB `ITransactionLocal` (para transacciones locales) e `ITransactionJoin` (para transacciones distribuidas) del proveedor. Mediante el inicio de una transacción local en el proveedor, SQL Server garantiza operaciones de escritura atómicas. Mediante el uso de transacciones distribuidas, SQL Server garantiza que una transacción que implique varios nodos tenga el mismo resultado (confirmación o anulación) en todos los nodos. Si el proveedor no admite las interfaces obligatorias de OLE DB relacionadas con las transacciones, no se permiten las operaciones de actualización en ese proveedor en función del contexto de transacción local.

En la tabla siguiente se describe lo que sucede cuando el usuario ejecuta una consulta distribuida dadas las funciones del proveedor y un contexto de transacción local. Una operación de lectura en un proveedor hace referencia a una instrucción `SELECT` o cuando se lee la tabla remota en el lado de entrada de una instrucción `SELECT INTO`, `INSERT`, `UPDATE` o `DELETE`. Una operación de escritura en un proveedor hace referencia a una instrucción `INSERT`, `UPDATE` o `DELETE` con una tabla remota como tabla de destino.

Resultados de una consulta distribuida en función de las funcionalidades del proveedor y el contexto de la transacción:

|La consulta distribuida se produce|El proveedor no admite `ITransactionLocal`|El proveedor admite `ITransactionLocal` pero no `ITransactionJoin`|El proveedor admite `ITransactionLocal` y `ITransactionJoin`|
|:-----|:-----|:-----|:-----|
| En una transacción por sí misma (sin transacción de usuario).|De forma predeterminada, solo se permiten las operaciones de lectura. Cuando se habilita la opción `Nontransacted Updates` de nivel de proveedor, se permiten las operaciones de escritura. (Cuando se habilita esta opción, SQL Server no puede garantizar la atomicidad y coherencia en los datos del proveedor. Esto puede hacer que los efectos parciales de una operación de escritura se reflejen en el origen de datos remoto sin la capacidad de deshacerlos).| Se permiten todas las instrucciones en los datos remotos. Los cursores de conjunto de claves son de solo lectura. La transacción local se inicia en el proveedor con el nivel de aislamiento de la sesión de SQL Server actual y se confirma al final de la evaluación correcta de la instrucción. (El nivel de aislamiento predeterminado para una sesión SQL Server es `READ COMMITTED` a menos que se modifique con la instrucción `SET TRANSACTION ISOLATION LEVEL`. El proveedor debe admitir el nivel de aislamiento solicitado). | Se permiten todas las instrucciones. Los cursores de conjunto de claves son de solo lectura. La transacción local se inicia en el proveedor con el nivel de aislamiento de la sesión de SQL Server actual y se confirma al final de una evaluación correcta de la instrucción. |
| En una transacción de usuario (es decir, entre `BEGIN TRAN` o `BEGIN DISTRIBUTED TRAN` y `COMMIT`). | Si el nivel de aislamiento de la transacción es `READ COMMITTED` (el valor predeterminado) o inferior, se permiten las operaciones de lectura. Si el nivel de aislamiento es mayor, no se permite ninguna consulta distribuida. | Solo se permiten las operaciones de lectura. Las nuevas transacciones distribuidas se inician en el proveedor con el nivel de aislamiento de la sesión de SQL Server actual. |Se permiten todas las instrucciones. La nueva transacción local se inicia en el proveedor con el nivel de aislamiento de la sesión de SQL Server actual y se confirma al confirmar la transacción de usuario. En el caso de las instrucciones de modificación de datos, SQL Server inicia de forma predeterminada una transacción anidada en la transacción distribuida, de modo que la instrucción de modificación de datos se puede detener en determinadas condiciones de error sin detener la transacción circundante. Si la opción `XACT_ABORT SET` está activada, SQL Server no requiere compatibilidad con transacciones anidadas y detiene la transacción circundante en caso de que se produzcan errores durante la instrucción de modificación de datos. |
|  |  |  |

### <a name="data-type-handling-in-distributed-queries"></a>Control de tipos de datos en consultas distribuidas

Los proveedores OLE DB exponen sus datos en términos de los tipos de datos definidos por OLE DB (que se indican mediante `DBTYPE` en OLE DB). SQL Server procesa los datos externos en el servidor como tipos de SQL Server nativos; esto da como resultado una asignación de tipos de datos de OLE DB a los tipos nativos de SQL Server y viceversa, ya que SQL Server los consume o los exporta, respectivamente. Esta asignación se realiza de forma implícita, a menos que se indique lo contrario.

Los tipos de datos de las consultas distribuidas se administran mediante uno de estos dos métodos de asignación:

- La asignación del lado de consumo asigna tipos de datos de OLE DB a tipos de datos nativos de SQL Server en el lado consumidor, cuando las tablas remotas aparecen en instrucciones `SELECT` y en el lado de entrada de las instrucciones INSERT, UPDATE y DELETE.

- La asignación en el lado de la exportación asigna tipos de datos de SQL Server a tipos de datos de OLE DB en el lado de la exportación, cuando una tabla remota aparece como la tabla de destino de una instrucción `INSERT` o `UPDATE`.

Tabla de asignación de tipos de datos de SQL Server y OLE DB.

| Tipo de OLE DB | `DBCOLUMNFLAG` | Tipos de datos de SQL Server |
|-----|-----|-----|
|`DBTYPE_I1`*| |`numeric(3, 0)`|
|`DBTYPE_I2`| |`smallint`|
|`DBTYPE_I4`| |`int`|
|`DBTYPE_I8`| |`numeric(19,0)`|
|`DBTYPE_UI1`| |`tinyint`|
|`DBTYPE_UI2`*| |`numeric(5,0)`|
|`DBTYPE_UI4`*| |`numeric(10,0)`|
|`DBTYPE_UI8`*| |`numeric(20,0)`|
|`DBTYPE_R4`| |`float`|
|`DBTYPE_R8`| |`real`|
|`DBTYPE_NUMERIC`| |`numeric`|
|`DBTYPE_DECIMAL`| |`decimal`|
|`DBTYPE_CY`| |`money`|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`<br>o bien<br> Longitud máxima > 4000 caracteres|ntext|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`|NCHAR|
|`DBTYPE_BSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|NVARCHAR|
|`DBTYPE_IDISPATCH`| |Error|
|`DBTYPE_ERROR`| |Error|
|`DBTYPE_BOOL`| |`bit`|
|`DBTYPE_VARIANT`*| |NVARCHAR|
|`DBTYPE_IUNKNOWN`| |Error|
|`DBTYPE_GUID`| |`uniqueidentifier`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISLONG=true` <br>o bien<br> Longitud máxima > 8000|`image`|
|`DBTYPE_BYTES`|`DBCOLUMNFLAGS_ISROWVER=true`, `DBCOLUMNFLAGS_ISFIXEDLENGTH=true`, Tamaño de columna = 8 <br>o bien<br> Longitud máxima no indicada. | `timestamp` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `binary` |
|`DBTYPE_BYTES`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varbinary`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `char`|
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` | `varchar` |
|`DBTYPE_STR`| `DBCOLUMNFLAGS_ISLONG=true` <br>o bien<br> Longitud máxima > 8000 caracteres <br>o bien<br>   Longitud máxima no indicada. | `text`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISFIXEDLENGTH=true` |`nchar`|
|`DBTYPE_WSTR` | `DBCOLUMNFLAGS_ISFIXEDLENGTH=false`|`nvarchar`|
|`DBTYPE_WSTR`| `DBCOLUMNFLAGS_ISLONG=true` <br>o bien<br> Longitud máxima > 4000 caracteres <br>o bien<br>   Longitud máxima no indicada. | `ntext`|
|`DBTYPE_UDT`| |Error|
|`DBTYPE_DATE`* | | `datetime` |
|`DBTYPE_DBDATE` | | `datetime` (se requiere conversión explícita)|
|`DBTYPE_DBTIME`| | `datetime` (se requiere conversión explícita)|
|`DBTYPE_DBTIMESTAMP`* | | `datetime`|
|`DBTYPE_ARRAY` | |Error|
|`DBTYPE_BYREF` | | Omitido |
|`DBTYPE_VECTOR` | |Error|
|`DBTYPE_RESERVED`| |Error|

\* Indica algún tipo de traducción a la representación del tipo de SQL Server, ya que no hay un tipo de datos equivalente exacto en SQL Server. Estas conversiones pueden provocar la pérdida de precisión, desbordamiento o subdesbordamiento. Las asignaciones implícitas predeterminadas se pueden cambiar en el futuro si en próximas versiones de SQL Server se admiten los tipos de datos correspondientes.

>[!NOTE]
>`numeric(p,s)` indica el tipo de datos de SQL Server `numeric` con la precisión `p` y la escala `s`. La precisión máxima permitida para `DBTYPE_NUMERIC` y `DBTYPE_DECIMAL` es 38. El proveedor debe admitir el enlace a la columna `DBTYPE_BSTR` como `DBTYPE_WSTR` cuando se crea un descriptor de acceso. Las columnas `DBTYPE_VARIANT` se consumen como cadenas de caracteres Unicode `nvarchar`. Esto requiere compatibilidad con la conversión de `DBTYPE_VARIANT` a `DBTYPE_WSTR` desde el proveedor. Se espera que el proveedor implemente esta conversión tal y como se define en OLE DB. Para más información, vea [Tipos de datos de la especificación OLE DB](#appendixa).

#### <a name="interpreting-data-type-mapping"></a>Interpretación de la asignación de tipos de datos

La asignación a un tipo de datos de SQL Server se determina mediante el tipo de datos de OLE DB y los valores DBCOLUMNFLAGS que describen el valor de columna o escalar. En el caso del conjunto de filas de esquema `COLUMNS`, las columnas `DATA_TYPE` y `COLUMN_FLAGS` representan estos valores. En el caso de la interfaz `IColumnsInfo::GetColumnInfo`, los miembros `wType` y `dwFlags` de la estructura `DBCOLUMNINFO` representan esta información.

Para usar la asignación del lado de consumo para una columna determinada con un valor `DBTYPE` y `DBCOLUMNFLAG` específico, busque el tipo de SQL Server correspondiente en la tabla. Las reglas de tipos para las columnas de tablas remotas en expresiones se pueden describir mediante la siguiente regla simple:

>Un valor de columna remota determinado es válido en una expresión Transact-SQL si el tipo de SQL Server asignado correspondiente en la tabla es válido en el mismo contexto.

La tabla y la regla definen:

- Comparaciones y expresiones.

Por lo general, `X <op> <remote-column>` es una expresión válida si `<op>` es un operador válido en el tipo de datos de `X` y el tipo de datos al que se asigna `<remote-column>`.

- Conversiones explícitas.

Se permite `Convert(X, <remote-column>)` si el valor `DBTYPE` de `<remote-column>` se asigna al tipo de datos nativo `Y` (como se indica en la tabla anterior) y se permite la conversión explícita de `Y` a `X`.

Si los usuarios quieren que los datos remotos se conviertan a un tipo de datos nativo no predeterminado, deben usar una conversión explícita.

Para usar la asignación del lado de la exportación en el caso de las instrucciones `UPDATE` e `INSERT` en tablas remotas, asigne tipos de datos nativos de SQL Server a tipos de datos de OLE DB mediante la misma tabla. Se permite una asignación de un tipo `S1` de SQL Server a un tipo `T` de OLE DB determinado siempre que exista una de estas condiciones:

- La asignación correspondiente se puede encontrar directamente en la tabla de asignación.

- Existe una conversión implícita permitida de `S1` a otro tipo `S2` de SQL Server que asigna `S2` al tipo `T` en la tabla de asignación.

#### <a name="large-object-lob-handling"></a>Manipulación de objetos grandes (LOB)

Como se ha indicado en la tabla de asignación, si las columnas del tipo `DBTYPE_STR`, `DBTYPE_WSTR` o `DBTYPE_BSTR` también notifican `DBCOLUMNFLAGS_ISLONG`, o bien si su longitud máxima supera los 4.000 caracteres (o si no se notifica ninguna longitud máxima), SQL Server los trata como una columna `text` o `ntext`, según corresponda. De forma similar, para las columnas `DBTYPE_BYTES`, si se establece `DBCOLUMNFLAGS_ISLONG` o si la longitud máxima es superior a 8.000 bytes (o si no se proporciona la longitud máxima), las columnas se tratan como columnas `image`. Las columnas `Text`, `ntext` e `image` se denominan columnas LOB.

SQL Server no expone la funcionalidad de texto e imagen completa en los LOB de un proveedor OLE DB. `TEXTPTRS` no se admiten en objetos grandes de un proveedor OLE DB; por tanto, no se admite ninguna de las funciones relacionadas, por ejemplo, la función del sistema `TEXTPTR` y las instrucciones `READTEXT`, `WRITETEXT` y `UPDATETEXT`. Se admiten las instrucciones `SELECT` que recuperan columnas LOB completas, como las instrucciones `UPDATE` e `INSERT` para las columnas de objetos grandes en tablas remotas.

SQL Server usa las interfaces de almacenamiento estructurado en las columnas LOB si el proveedor las admite. Las interfaces de almacenamiento estructurado en orden creciente de preferencia y funcionalidad son las siguientes: `ISequentialStream`, `Istream` o `ILockBytes`. Si se admite una o varias de estas interfaces, el proveedor debe devolver DBPROPVAL_OO_BLOB como el valor de la propiedad `DBPROP_OLEOBJECTS` cuando se consulta a través de la interfaz `IDBProperties`. Además, el proveedor debe indicar en la propiedad `DBPROP_STRUCTUREDSTORAGE` la compatibilidad con las interfaces que admite.

Si el proveedor no es compatible con ninguna de las interfaces de almacenamiento estructurado en columnas LOB, SQL Server materializa esta interfaz por sí misma y todavía las expone como columnas `text`, `ntext` o `image`.

#### <a name="accessing-lob-columns"></a>Acceso a columnas LOB

Si el proveedor admite una de las interfaces de almacenamiento estructurado, SQL Server realiza los pasos siguientes para recuperar las columnas LOB durante la ejecución de la consulta:

1. Antes de abrir el conjunto de filas a través de `IOpenRowset::OpenRowset`, SQL Server solicita compatibilidad para una o varias de las interfaces de almacenamiento estructurado (`ISequentialStream`, `Istream` e `ILockBytes`) en las columnas de objetos grandes. Se requiere la primera interfaz admitida por el proveedor; se solicitan interfaces adicionales como \"estén establecidas sin son asumibles\" mediante el establecimiento del elemento *dwOptions* de la estructura DBPROP correspondiente en DBPROPOPTIONS_SETIFCHEAP. Por ejemplo, si un proveedor admite `ISequentialStream` e `ILockBytes`, se requiere `ISequentialStream` e `ILockBytes` se solicita como \"esté establecida si es asumible\".

4. Una vez abierto el conjunto de filas, SQL Server usa `IRowsetInfo::GetProperties` para identificar las interfaces reales disponibles en el conjunto de filas. Se usa la última interfaz o la más preferible que haya devuelto el proveedor. Cuando SQL Server crea un descriptor de acceso en la columna de objetos grandes, la columna se enlaza como DBTYPE_IUNKNOWN con el elemento *iid* de la estructura DBOBJECT en el enlace establecido con la interfaz.

#### <a name="reading-from-lob-columns"></a>Lectura desde columnas LOB

Use el puntero de interfaz para la interfaz de almacenamiento estructurado solicitada devuelta en el búfer de filas desde `IRowset::GetData` para leer desde la columna de objetos grandes. Si el proveedor no admite varios LOB abiertos al mismo tiempo (es decir, si no admite `DBPROP_MULTIPLE_STORAGEOBJECTS`) y si la fila tiene varias columnas de objetos grandes, SQL Server copia las columnas LOB en una tabla de trabajo local.

#### <a name="update-and-insert-statements-on-lob-columns"></a>Instrucciones `UPDATE` e `INSERT` en columnas LOB

SQL Server pasa al proveedor un puntero a un objeto de almacenamiento nuevo en lugar de usar la interfaz proporcionada por el proveedor para modificar el objeto de almacenamiento. Para cada columna LOB, el valor que se actualiza o inserta en un objeto de almacenamiento se crea con la interfaz de almacenamiento estructurado seleccionada. En función de si se trata de una operación `UPDATE` o `INSERT`, se pasa al proveedor un puntero al objeto de almacenamiento a través de `IRowsetChange::SetData` o `IRowsetChange::InsertRow`, respectivamente.

### <a name="error-handling"></a>Tratamiento de errores

Cuando la invocación de un método específico en un proveedor OLE DB devuelve un código de error, SQL Server busca la información de error extendida del proveedor antes de devolver al usuario información sobre la condición de error.

SQL Server usa el objeto de error de OLE DB, como se haya especificado en OLE DB. Algunos de los pasos generales son los siguientes:

1. Cuando la invocación de un método devuelve un código de error del proveedor, SQL Server busca la interfaz `ISupportErrorInfo`. Si se admite esta interfaz, SQL Server llama a `ISupportErrorInfo::InterfaceSupportsErrorInfo` para comprobar si la interfaz que ha generado el código de error admite objetos de error.

<!-- -->

5. Si la interfaz admite objetos de error, SQL Server llama a la función `GetErrorInfo` para obtener un puntero de interfaz `IErrorInfo` en el objeto de error actual.

6. SQL Server usa la interfaz `IErrorInfo` para obtener un puntero a la interfaz `IErrorRecords`.

7. SQL Server usa `IErrorRecords` para recorrer en bucle todos los registros de error del objeto y obtener el texto del mensaje de error correspondiente a cada registro.

Para más información sobre cómo se usa el objeto de error del proveedor, vea la documentación de OLE DB.

### <a name="security"></a>Seguridad

Cuando un consumidor se conecta a un proveedor OLE DB, el proveedor normalmente requiere un identificador de usuario y una contraseña, a menos que el consumidor se quiera autenticar como un usuario de seguridad integrada. En el caso de las consultas distribuidas, SQL Server actúa como el consumidor del proveedor OLE DB en nombre del inicio de sesión de SQL Server que ejecuta la consulta distribuida. SQL Server asigna el inicio de sesión de SQL Server actual a un identificador y una contraseña de usuario en el servidor vinculado.

El usuario puede especificar estas asignaciones para un servidor vinculado determinado y se pueden configurar y administrar mediante los procedimientos almacenados del sistema `sp_addlinkedsrvlogin` y `sp_droplinkedsrvlogin`. Al establecer las propiedades de grupo de inicialización DBPROP_AUTH_USERID y DBPROP_AUTH_PASSWORD a través de `IDBProperties::SetProperties`, el identificador de usuario y la contraseña que determina la asignación se pasan al proveedor durante el establecimiento de la conexión.

Cuando un cliente se conecta a SQL Server a través de la Autenticación de Windows, y si el inicio de sesión tiene una asignación de `self` configurada mediante `sp_addlinkedsrvlogin`, SQL Server intenta suplantar el contexto de seguridad del cliente y establece la propiedad `DBPROP_AUTH_INTEGRATED` en el proveedor durante el establecimiento de la conexión. Este proceso se denomina *delegación*.

Una vez que se ha determinado el contexto de seguridad que se usa para la conexión, la autenticación de este contexto de seguridad y la comprobación de permisos para ese contexto con respecto a los objetos de datos del origen de datos son totalmente responsabilidad del proveedor OLE DB.

Para más información, vea [`sp_addlinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) y [`sp_droplinkedsrvlogin`](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md).

## <a name="query-execution-scenarios"></a>Escenarios de ejecución de consultas

 Al evaluar una consulta distribuida, SQL Server interactúa con el proveedor OLE DB en uno o varios de estos escenarios:

- Consulta remota

- Acceso indexado

- Recorridos de tabla puros

- Instrucciones `UPDATE` y DELETE

- Instrucción `INSERT`

- Consultas de paso a través

### <a name="remote-query"></a>Remote Query

SQL Server genera una consulta SQL que evalúa una parte de la consulta original que el proveedor puede evaluar en su totalidad. Este escenario solo es posible en los proveedores de comandos SQL. La medida en la que SQL Server inserta las operaciones en el proveedor mediante la generación de una consulta SQL depende de la gramática de SQL que admita el proveedor. El proveedor debe indicar su nivel de compatibilidad con SQL a través de lo siguiente:

1. Indicando la compatibilidad con el nivel de entrada SQL Minimum, ODBC Core o SQL-92 a través de la propiedad `DBPROP_SQLSUPPORT`. SQL Minimum es un nivel de sintaxis nuevo que se admite en SQL Server y que permite a SQL Server enviar consultas remotas a proveedores simples que admiten un subconjunto sencillo de SQL. Este nivel engloba una instrucción `SELECT` básica que no incluye subconsultas, varias tablas en la cláusula `FROM` (y, por tanto, ninguna combinación) y GROUP BY. Para obtener el subconjunto de la gramática de SQL que usa SQL Server para generar consultas remotas en proveedores de cada uno de estos niveles de sintaxis, vea [Subconjunto de SQL que se usa para generar consultas remotas](#appendixb).

1. Admitiendo varias propiedades específicas de SQL Server para indicar la compatibilidad con características individuales de SQL que no se incluyen en el nivel de sintaxis notificado por DBPROP_SQLSUPPORT. La lista de propiedades y cómo se usan en SQL Server se describe más adelante en esta sección.

SQL Server usa la ejecución de consultas con parámetros con un signo de interrogación (?) como marcador de parámetro en la cadena de Transact-SQL. La ejecución de consultas con parámetros se usa en los proveedores OLE DB SQL Server, Microsoft Jet y Oracle. En otros proveedores, se usa la ejecución de consultas con parámetros si el proveedor admite `ICommandWithParameters` en el objeto `Command` y se cumple al menos una de las condiciones siguientes:

- El proveedor indica el nivel de compatibilidad ODBC Core con SQL Server a través de la propiedad `DBPROP_SQLSUPPORT`.

- Para indicar la compatibilidad con el marcador de parámetro de signo de interrogación (?), el proveedor admite la propiedad específica de SQL Server SQLPROP_DYNCMICSQL a través de `IDBPProperties`. Para más información, vea la sección siguiente sobre propiedades de proveedor.

- El administrador establece la opción de proveedor `Dynamic Parameters` en el proveedor para que SQL Server genere consultas con parámetros.

Cuando SQL Server genera el texto SQL que se va a ejecutar de forma remota, los nombres de tabla y de columna se incluyen entre comillas con el carácter de comillas del proveedor como se ha indicado a través del literal `DBLITERAL_QUOTE` de la interfaz `IDBInfo`. Si no se admite este literal, los nombres de tabla y de columna no se incluyen entre comillas.

Si el proveedor admite la ejecución de consultas con parámetros, SQL Server considera una estrategia de ejecución de consultas con parámetros para evaluar una combinación de una tabla remota con una tabla local. La consulta con parámetros se ejecuta repetidamente para los valores de parámetro generados desde cada fila de la tabla local. Esta estrategia reduce el número de filas que se recuperan del proveedor y es beneficiosa cuando una tabla local con un número pequeño de filas se combina con una tabla remota con un gran número de filas. Esta estrategia de combinación remota se puede aplicar mediante la sugerencia de optimizador de combinación `REMOTE`. Para más información sobre las consultas con parámetros, vea [Procedimientos para ejecutar consultas con parámetros](../../connect/php/how-to-perform-parameterized-queries.md).

A continuación se muestran los pasos más generales en el proveedor para el escenario de consultas remotas.

1. SQL Server crea un objeto `Command` a partir del objeto `Session` mediante `IDBCreateCommand::CreateCommand`.

9. Si la opción de configuración del servidor `Remote Query Timeout` se establece en un valor menor que 0, SQL Server establece la propiedad `DBPROP_COMMANDTIMEOUT` del objeto `Command` en el mismo valor mediante `ICommandProperties::SetProperties`; se debe llamar a `ICommand::SetCommandText` para establecer el texto del comando en la cadena de Transact-SQL generada.

10. SQL Server llama a `ICommandPrepare::Prepare` para preparar el comando. Si el proveedor no admite esta interfaz, SQL Server continúa con el paso 4.

11. Si la consulta generada tiene parámetros, SQL Server usa `ICommandWithParameters::SetParameterInfo` para describir los parámetros y `IAccessor::CreateAccessor` para crear descriptores de acceso para los parámetros.

12. SQL Server llama a `ICommand::Execute` para ejecutar el comando y crear el conjunto de filas.

13. SQL Server usa la interfaz `IRowset` para navegar y consumir filas de la tabla. Se usa `IRowset::GetNextRows` para capturar filas, `IRowset::RestartPosition` para cambiar la posición al principio del conjunto de filas y `IRowset::ReleaseRows` para liberar filas.

#### <a name="provider-properties-of-interest-for-remote-query-execution"></a>Propiedades de proveedor de interés para la ejecución de consultas remotas

Si el proveedor admite características de SQL que no se incluyen en el nivel de sintaxis notificado en DBPROP_SQLSUPPORT, las puede indicar mediante diversas propiedades específicas del proveedor.

- SQLPROP_GROUPBY. Esta propiedad es de interés para un proveedor que admita el nivel Minimum (mínimo) de SQL. Esta propiedad indica que el proveedor admite las cláusulas GROUP BY y HAVING en la instrucción `SELECT`. Además, esta propiedad también indica que el proveedor admite las cinco funciones de agregado siguientes: MIN, MAX, SUM, COUNT y AVG. Es posible que el proveedor no admita DISTINCT en el argumento de estas funciones de agregado.

- SQLPROP_SUBQUERIES. esta propiedad es de interés para un proveedor que admita el nivel Minimum de SQL. Indica que el proveedor admite subconsultas, como se especifica en el nivel de entrada SQL-92. Esto incluye las subconsultas de la lista `SELECT` y la cláusula `WHERE` con compatibilidad con las subconsultas correlacionadas, y los operadores `IN`, `EXISTS`, `ALL` y `ANY`.

- SQLPROP_DATELITERALS. Esta propiedad es de interés para cualquier proveedor (incluidos los que admiten el nivel de entrada SQL-92). La compatibilidad con la sintaxis de literales estándar para los literales de fecha y hora no forma parte del nivel de entrada SQL-92. Esta propiedad específica de SQL Server indica que el proveedor admite la sintaxis de literales de fecha y hora, como se especifica en el estándar SQL-92.

- SQLPROP_ANSILIKE. De interés para un proveedor que admita el nivel Minimum de SQL. Esta propiedad indica que el proveedor admite el operador `LIKE` según el nivel de entrada SQL-92 (\'%\' y \'_\' como caracteres comodín). Esto se usará en un proveedor que admita el nivel Minimum de SQL, ya que ese nivel no incluye compatibilidad con `LIKE`.

- SQLPROP_INNERJOIN. esta propiedad es de interés para los proveedores que admiten el nivel Minimum de SQL. Indica compatibilidad con varias tablas en la cláusula `FROM`. Esto se usará en un proveedor que solo admita el nivel Minimum de SQL, ya que ese nivel no incluye compatibilidad con las combinaciones. Esto no indica la compatibilidad con palabras clave JOIN explícitas ni con combinaciones OUTER. Solo indica que se admiten combinaciones implícitas a través de una lista de tablas en la cláusula `FROM`.

- SQLPROP_DYNAMICSQL. Indica compatibilidad con `?` como marcador de parámetro. El marcador de parámetro se debe admitir en lugar de un elemento escalar en una cláusula `WHERE` o en la lista `SELECT`. La compatibilidad con marcadores de parámetros `?` permite a SQL Server enviar consultas con parámetros al proveedor.

- SQLPROP_NESTEDQUERIES. Indica compatibilidad con instrucciones SELECT anidadas en la cláusula `FROM` (por ejemplo, `SELECT * FROM (SELECT * FROM T))`). En muchos casos, SQL Server usa instrucciones `SELECT` anidadas en la cláusula `FROM` de una consulta cuando genera las cadenas de consulta que se van a ejecutar de forma remota. Como la compatibilidad con instancias de `SELECT` anidadas no es necesaria con el nivel de entrada SQL-92, SQL Server no delega al proveedor las consultas con instrucciones `SELECT` anidadas a menos que el proveedor también establezca esta propiedad. Como alternativa, el administrador también puede establecer la opción de proveedor `Nested Queries` para que el proveedor haga que SQL Server genere consultas anidadas en el proveedor.

El proveedor puede admitir estas propiedades mediante un conjunto de propiedades específico de SQL Server denominado `SQLPROPSET_OPTHINTS` y la definición de valores `PROPID`. El conjunto de propiedades `SQLPROPSET_OPTHINTS` y las dos propiedades se definen mediante las constantes siguientes:

```
extern const GUID `SQLPROPSET_OPTHINTS` = { 0x2344480c, 0x33a7, 0x11d1, { 0x9b, 0x1a, 0x0, 0x60, 0x8, 0x26, 0x8b, 0x9e } };
enum SQLPROPERTIES {
SQLPROP_NESTEDQUERIES = 0x4,
SQLPROP_DYNAMICSQL = 0x5,
SQLPROP_GROUPBY = 0x6,
SQLPROP_DATELITERALS = 0x7,
SQLPROP_ANSILIKE = 0x8,
SQLPROP_INNERJOIN = 0x9,
SQLPROP_SUBQUERIES = 0x10
};
```

#### <a name="character-set-and-sort-order-implications"></a>Implicaciones del juego de caracteres y el criterio de ordenación

SQL Server admite la especificación de una intercalación para datos de caracteres en cada nivel de columna. La intercalación incluye el juego de caracteres y la especificación del criterio de ordenación para los datos de caracteres no Unicode (las columnas `char` y `varchar`). Para los datos Unicode (las columnas `nchar` y `nvarchar`), la intercalación solo especifica el criterio de ordenación.

SQL Server delega las comparaciones de cadenas al proveedor solo si el juego de caracteres (para datos que no son Unicode), el criterio de ordenación y la semántica de comparación de cadenas que usa el servidor vinculado son los mismos que los que usa el servidor local.

En el caso de los servidores vinculados de SQL Server, SQL Server determina de forma automática la compatibilidad con la intercalación. Para otros proveedores, el administrador debe indicar a SQL Server la intercalación de datos de caracteres desde un servidor vinculado determinado. En SQL Server, se admite una nueva opción de servidor vinculado denominada `Collation Name`. Si el administrador determina que la semántica de intercalación adoptada por el servidor vinculado es igual que una de las intercalaciones estándar de SQL Server, puede establecer la opción `Collation Name` en ese nombre de intercalación. La opción `Collation Name` se puede habilitar mediante el procedimiento almacenado del sistema `sp_serveroption`. Esta opción solo se debe establecer si se cumplen las dos condiciones siguientes:

- El criterio de ordenación y el juego de caracteres remotos son los mismos que los de la intercalación especificada de SQL Server.

- La semántica de comparación de cadenas que usa el proveedor OLE DB sigue las especificaciones estándar de SQL-92 o equivalentes a la semántica de comparación de SQL Server.

Todavía se admite la opción Compatible con la intercalación admitida en SQL Server 7.0, por razones de compatibilidad con versiones anteriores. Establecerla en true equivale a establecer la opción Nombre de intercalación en la intercalación predeterminada de la base de datos maestra de SQL Server. Las nuevas aplicaciones deben usar la opción Nombre de intercalación en lugar de la opción Compatible con la intercalación.

### <a name="indexed-access"></a>Acceso indizado

SQL Server usa un índice expuesto por el proveedor para evaluar determinados predicados de la consulta distribuida. Este escenario solo es posible en los proveedores de índices y cuando el usuario establece la opción de proveedor `Index as Access Path`. Estos son los principales pasos generales que SQL Server realiza en el proveedor mientras se usa un índice para ejecutar una consulta:

1. Abre el conjunto de filas de índice a través de `IOpenRowset::OpenRowset` con el nombre de tabla completo y el nombre del índice. Los nombres completos de tabla y de índice se generan como se ha descrito antes en el escenario de consultas remotas.

1. Abre el conjunto de filas de tabla base a través de `IOpenRowset::OpenRowset` con el nombre de tabla completo.

1. Establece intervalos en el conjunto de filas de índice en función del predicado de consulta a través de `IRowsetIndex::SetRange`.

1. Examina las filas del conjunto de filas de índice a través de `IRowset` en el conjunto de filas de índice.

1. Usa la columna de marcador de las filas de índice recuperadas para capturar las filas correspondientes del conjunto de filas de la tabla base a través de `IRowsetLocate::GetRowsByBookmark`.

Las propiedades del conjunto de filas `DBPROP_IRowsetLocate` y `DBPROP_BOOKMARKS` son necesarias en el conjunto de filas abierto en la tabla base.

### <a name="pure-table-scans"></a>Recorridos de tabla puros

SQL Server examina la tabla remota completa desde el proveedor y realiza la evaluación de todas las consultas de forma local. El conjunto de filas correspondiente a la tabla se abre mediante una llamada a `IOpenRowset::OpenRowset`. SQL Server crea el nombre de tabla proporcionado a `OPENROWSET` a partir de las partes de nombre de catálogo, esquema y objeto de esta forma:

1. Cada una de las partes del nombre se incluye entre comillas con el carácter de comillas del proveedor (`DBLITERAL_QUOTE`) y, después, se concatenan con el carácter `DBLITERAL_CATALOG_SEPARATOR` insertado entre ellas.

1. Una vez abierto el objeto de conjunto de filas, SQL Server usa la interfaz `IColumnsInfo` para comprobar que los metadatos en tiempo de ejecución son los mismos que los metadatos en tiempo de compilación de la tabla.

1. SQL Server usa la interfaz `IRowset` para navegar y consumir filas de la tabla. Se usa `IRowset::GetNextRows` para capturar filas, `IRowset::RestartPosition` para cambiar la posición al principio del conjunto de filas y `IRowset::ReleaseRows` para liberar filas.

### <a name="update-and-delete-statements"></a>Instrucciones `UPDATE` y `DELETE`

Se deben satisfacer las condiciones siguientes para que una tabla remota se actualice o se elimine de una consulta distribuida de SQL Server:

- El proveedor debe admitir marcadores en el conjunto de filas abierto a través de `IOpenRowset` en la tabla que se va a actualizar o eliminar.

- El proveedor debe admitir las interfaces `IRowsetLocate` y `IRowsetChange` en el conjunto de filas abierto a través de `IOpenRowset` para la tabla que se va a actualizar o eliminar.

- La interfaz `IRowsetChange` debe admitir métodos de actualización (`SetData`) y eliminación (`DeleteRows`).

- Si el proveedor no admite `ITransactionLocal`, las instrucciones `UPDATE` y `DELETE` solo se permiten si la opción `Non-transacted` está establecida para ese proveedor y si la instrucción no está en una transacción de usuario.

- Si el proveedor no admite `ITransactionJoin`, solo se permite una instrucción `UPDATE` o `DELETE` si no se encuentra en una transacción de usuario.

Las propiedades del conjunto de filas siguientes son necesarias en el conjunto de filas abierto en la tabla actualizada: `DBPROP_IRowsetLocate`, `DBPROP_IRowsetChange` y `DBPROP_BOOKMARKS`. La propiedad del conjunto de filas `DBPROP_UPDATABILITY` se establece en `DBPROPVAL_UP_CHANGE` o `DBPROPVAL_UP_DELETE` en función de si la operación realizada es de tipo `UPDATE` o `DELETE`, respectivamente.

Los siguientes pasos generales se realizan en el proveedor para procesar una operación `UPDATE` o `DELETE`:

1. SQL Server abre el conjunto de filas de la tabla base a través de la interfaz `IOpenRowset`. SQL Server requiere las propiedades mencionadas antes en el conjunto de filas.

1. SQL Server determina el conjunto de filas aptas que se van a actualizar o eliminar.

1. SQL Server usa los marcadores para colocar las filas aptas a través de la interfaz `IRowsetLocate`.

1. Se usa `IRowsetChange::SetData` para las operaciones `UPDATE` o `IRowsetChange::DeleteRows` para las operaciones de eliminación para realizar los cambios necesarios en las filas aptas.

### <a name="insert-statement"></a>Instrucción `INSERT`

Las condiciones para admitir instrucciones `INSERT` en una tabla remota son menos estrictas que para las instrucciones `UPDATE` y `DELETE`:

- El proveedor debe admitir `IRowsetChange::InsertRow` en el conjunto de filas abierto en la tabla base en la que se va a insertar.

- Si el proveedor no admite `ITransactionLocal`, las instrucciones `INSERT` solo se permiten si la opción `Non-transacted updates` está establecida para ese servidor vinculado y si la instrucción no está en una transacción de usuario.

- Si el proveedor no admite `ITransactionJoin`, solo se permiten las instrucciones `INSERT` si no se encuentran en una transacción de usuario.

SQL Server usa `IOpenRowset::OpenRowset` para abrir un conjunto de filas en la tabla base y llama a `IRowsetChange::InsertRow` para insertar nuevas filas en el conjunto de filas base.

### <a name="pass-through-queries"></a>Consultas de paso a través

Este escenario es similar al escenario de consultas remotas, a excepción de que el texto del comando proporcionado a `ICommand` es una cadena de comandos enviada por el usuario y no se interpreta mediante SQL Server. SQL Server usa `DBGUID_DEFAULT` como identificador de dialecto cuando llama a `ICommandText::SetCommandText`. `DBGUID_DEFAULT` indica que el proveedor debe usar su dialecto predeterminado. Si el texto de este comando devuelve más de un conjunto de resultados, por ejemplo, si el comando invoca un procedimiento almacenado que devuelve varios conjuntos de resultados, SQL Server solo usaría el primer conjunto de resultados del comando.

Para obtener una lista de todas las interfaces de OLE DB que usa SQL Server, vea [Interfaces de OLE DB que usa SQL Server](#appendixa).

### <a name="conclusion"></a>Conclusión

Microsoft SQL Server ofrece el conjunto de herramientas más sólido para acceder a datos de orígenes de datos heterogéneos. Al comprender las interfaces OLE DB expuestas por SQL Server, los desarrolladores pueden ejercer un alto grado de control y sofisticación en las consultas distribuidas.

## <a name="ole-db-interfaces-consumed-by-sql-server"></a><a name="appendixa"></a> Interfaces de OLE DB que usa SQL Server

En la tabla siguiente se enumeran todas las interfaces de OLE DB que usa SQL Server. En la columna Obligatoria se indica si la interfaz forma parte de la funcionalidad mínima de OLE DB que necesita SQL Server o si es opcional. Si una interfaz determinada no está marcada como obligatoria, SQL Server todavía puede acceder al proveedor, pero algunas funciones u optimizaciones específicas de SQL Server no son posibles en el proveedor.

En el caso de las interfaces opcionales, en la columna Escenarios se indican uno o más de los seis escenarios en los que se usa la interfaz especificada. Por ejemplo, la interfaz `IRowsetChange` de los conjuntos de filas de tabla base es una interfaz opcional; se usa en las instrucciones `UPDATE` y DELETE, y en escenarios con instrucciones `INSERT`. Si no se admite esta interfaz, las instrucciones UPDATE, DELETE e `INSERT` no se admiten en ese proveedor. Algunas de las otras interfaces opcionales están marcadas con \"rendimiento\" en la columna Escenarios, lo que indica que la interfaz da como resultado un mejor rendimiento general. Por ejemplo, si no se admite la interfaz `IDBSchemaRowset`, SQL Server debe abrir el conjunto de filas dos veces: una para sus metadatos y otra para la ejecución de la consulta. Al admitir `IDBSchemaRowset`, se mejora el rendimiento de SQL Server.

|Object|Interfaz|Obligatorio|Comentarios|Escenarios|
|:-----|:-----|:-----|:-----|:-----|
|Objeto de origen de datos|`IDBInitialize`|Sí|Se inicializa y configura el contexto de datos y seguridad.| |
| |`IDBCreateSession`|Sí|Se crea un objeto de sesión de base de datos.| |
| |`IDBProperties`|Sí|Se obtiene información sobre las funciones del proveedor, se establecen propiedades de inicialización, propiedad obligatoria: DBPROP_INIT_TIMEOUT.| |
| |`IDBInfo`|No|Se obtiene el literal de comillas, el catálogo, el nombre, la parte, el separador, el carácter, etc.|Consulta remota.|
|Objeto de sesión de base de datos|`IDBSchemaRowset`|No|Se obtienen los metadatos de tabla o columna. Los conjuntos de filas necesarios `TABLES`, `COLUMNS`, `PROVIDER_TYPES`; otros que se usan si están disponibles: `INDEXES`, `TABLE_STATISTICS`.|Rendimiento, acceso indexado.|
| |`IOpenRowset`|Sí|Se abre un conjunto de filas en una tabla, un índice o un histograma.| |
| |`IGetDataSource`|Sí|Se usa para volver al DSO desde un objeto de sesión de base de datos.| |
| |`IDBCreateCommand`|No|Se usa para crear un objeto de comando (consulta) para los proveedores que admiten consultas.|Consulta remota, consulta de paso a través.|
| |`ITransactionLocal`|No|Se usa para actualizaciones con transacciones.|Instrucciones `UPDATE`, `DELETE` e `INSERT`.|
| |`ITransactionJoin`|No|Se usa para admitir transacciones distribuidas.|`UPDATE` y `DELETE`, instrucciones `INSERT` si está en una transacción de usuario.|
|Objeto de conjunto de filas|IRowset|Sí|Se examinan filas.| |
| |`IAccessor`|Sí|Se establece un enlace con las columnas de un conjunto de filas.| |
| |`IColumnsInfo`|Sí|Se obtiene información sobre las columnas de un conjunto de filas.| |
| |`IRowsetInfo`|Sí|Se obtiene información sobre las propiedades del conjunto de filas.| |
| |`IRowsetLocate`|No|Se necesita para las operaciones `UPDATE`/`DELETE` y para realizar búsquedas basadas en índices; se usa para buscar filas por marcadores.|Acceso indexado, instrucciones `UPDATE` y `DELETE`.|
| |`IRowsetChange`|No|Se necesita para `INSERTS`/`UPDATES`/ `DELETES` en un conjunto de filas. Los conjuntos de filas en tablas base deben admitir esta interfaz para las instrucciones `INSERT`, `UPDATE` y `DELETE`.|Instrucciones `UPDATE`, `DELETE` e `INSERT`.|
| |`IConvertType`|Sí|Se usa para comprobar si el conjunto de filas admite determinadas conversiones de tipos de datos en sus columnas.| |
|Índice|`IRowset`|Sí|Se examinan filas.|Acceso indexado, rendimiento.|
| |`IAccessor`|Sí|Se establece un enlace con las columnas de un conjunto de filas.|Acceso indexado, rendimiento.|
| |`IColumnsInfo`|Sí|Se obtiene información sobre las columnas de un conjunto de filas.|Acceso indexado, rendimiento.|
| |`IRowsetInfo`|Sí|Se obtiene información sobre las propiedades del conjunto de filas.|Acceso indexado, rendimiento.|
| |`IRowsetIndex`|Sí|Solo se necesita para los conjuntos de filas de un índice; se usa para la funcionalidad de indexación (establecimiento de intervalos, búsquedas).|Acceso indexado, rendimiento.|
|Get-Help|`ICommand`|Sí| |Consulta remota, consulta de paso a través.|
| |`ICommandText`|Sí|Se usa para definir el texto de la consulta.|Consulta remota, consulta de paso a través.|
| |`IColumnsInfo`|Sí|Se usa para obtener metadatos de columna para los resultados de la consulta.|Consulta remota, consulta de paso a través.|
| |`ICommandProperties`|Sí|Se usa para especificar propiedades necesarias en conjuntos de filas devueltos por el comando.|Consulta remota, consulta de paso a través.|
| |`ICommandWithParameters`|No|Se usa para la ejecución de consultas con parámetros.|Consulta remota, rendimiento.|
| |`ICommandPrepare`|No|Se usa para preparar un comando con el fin de obtener metadatos (en consultas de paso a través si están disponibles).|Consulta remota, rendimiento.|
|Objeto de error|`IErrorRecords`|Sí|Se usa para obtener un puntero a una interfaz `IErrorInfo` correspondiente a un registro de error único.| |
| |`IErrorInfo`|Sí|Se usa para obtener un puntero a una interfaz `IErrorInfo` correspondiente a un registro de error único.| |
|Cualquier objeto.|`ISupportErrorInfo`|No|Se usa para comprobar si una interfaz determinada admite objetos de error.| |
|  |  |  |  |  |

>[!NOTE]
>Los objetos `Index`, `Command` y `Error` no son obligatorios. Pero si se admiten, las interfaces enumeradas son obligatorias como se especifica en la columna Obligatoria.

## <a name="sql-subset-used-for-generating-remote-queries"></a><a name="appendixb"></a>Subconjunto de SQL usado para generar consultas remotas

El subconjunto de SQL que el procesador de consultas de SQL Server genera en un proveedor de comandos SQL depende del nivel de sintaxis que admita el proveedor, como indica la propiedad `DBPROP_SQLSUPPORT`.

Proveedores de comandos SQL que admiten el nivel de entrada SQL u ODBC Core

SQL Server usa el subconjunto siguiente del lenguaje SQL para las consultas evaluadas por los proveedores de comandos SQL que admiten el nivel de entrada SQL-92 u ODBC Core:

1. Instrucciones `SELECT` con cláusulas `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `UNION`, `UNION ALL`, `ORDER BY DESC`, `ASC` y `HAVING`.

1. `UNION` y `UNION ALL` solo se generan en los proveedores que admiten el nivel de entrada SQL-92, no en los que admiten ODBC Core.

1. Cláusula `SELECT`:

   - Subconsultas escalares en la lista `SELECT`.

   - Alias de columna sin la palabra clave `AS`.

1. Cláusula `FROM`:

   - No se usan palabras clave de combinación explícitas; los nombres de tabla separados por comas se usan para especificar combinaciones internas y las combinaciones externas no se especifican en las consultas remotas.

   - Consultas anidadas con el formato `FROM` ( `<nested query>` ) `<alias>`.

   - Alias de tabla sin la palabra clave AS.

1. La cláusula `WHERE` usa subconsultas con `NOT` `EXISTS`, `ANY`, `ALL`.

1. Expresiones:

   - Funciones de agregado que se usan: `MIN([DISTINCT])`, `MAX([DISTINCT])`, `COUNT([DISTINCT])`, `SUM([DISTINCT])`, `AVG([DISTINCT])` y `COUNT(*)`.

   - Operadores de comparación: `<`, `=`, `<=`, `>`, `<>`, `>=`, `IS NULL` e `IS NOT NULL`.

   - Operadores booleanos: `AND`, `OR` y `NOT`.

 - Operadores aritméticos: `+`, `-`, `*` y `/`.

1. Constantes:

- Los literales numéricos y de moneda siempre se rodean mediante `( )`.

- Los literales de carácter se incluyen entre comillas con `' '`.

Proveedores de comandos SQL que admiten el nivel Minimum de SQL

En los proveedores de comandos SQL que admiten el nivel Minimum de SQL, SQL Server genera SQL mediante la gramática siguiente.

Esta gramática se ha derivado mediante la gramática mínima de SQL descrita en ODBC 3.0. Se resaltan todas las diferencias de esta gramática. Los elementos que se muestran en `*bold italics`* son los que se han agregado a la gramática mínima de SQL descrita en ODBC 3.0. Los elementos que se muestran eliminados en color verde son los que se han quitado de esta gramática.

*select-statement* ::=

`SELECT [ALL | DISTINCT] *select-list* FROM *table-reference-list*[WHERE *search-condition*] [order-by-clause]`

`SELECT` clause

select-list ::= `*` | `select-sublist [, select-sublist]...`

select-sublist ::= expression * [`alias`]*

`*alias ::= user-defined-name`*

`FROM clause`

table-reference-list ::= table-reference

table-identifier ::= user-defined-name

table-name ::= table-identifier

table-reference ::= table-name

`WHERE clause`

search-condition ::= boolean-term \[OR search-condition\]

boolean-term ::= boolean-factor \[AND boolean-term\]

boolean-factor ::= \[NOT\] boolean-primary

boolean-primary ::= comparison-predicate \| ( search-condition )

comparison-predicate ::= expression comparison-operator expression

*\| `expression IS \[NOT\] NULL`*

comparison-operator ::= `< \| >` \| `<= \| >`= \| = \| `<>`

`ORDER BY clause`

order-by-clause ::= ORDER BY sort-specification \[, sort-specification\]\..

sort-specification ::= { \| column-name } \[ASC \| DESC\]

`Common syntactic elements`

expression ::= term \| expression {+\|--} term

term ::= factor \| term {\*\|/} factor

factor ::= \[+\|--\] primary

primary ::= column-name

\| literal

\| ( expression )

column-name ::= \[table-name.\]column-identifier

literal ::= character-string-literal

*\| integer-literal*

*\| exact-numeric-literal*

character-string-literal ::= \'{character}...\'

(character es cualquier carácter del juego de caracteres del controlador u origen de datos. Para incluir un solo carácter de comilla literal [\'] en un literal de cadena de caracteres, use dos caracteres de comilla literales [\'\']).

`*integer-literal ::=* \[*+ \| -*\] *unsigned-integer`*

`*exact-numeric-literal::=* \[*+ \| -*\] *unsigned-integer* \[*period unsigned-integer*\]`

`*\| period unsigned-integer`*

base-table-name ::= base-table-identifier

base-table-identifier ::= user-defined-name

column-identifier ::= user-defined-name

user-defined-name ::= letter\[digit \| letter \| _\]\..

unsigned-integer ::= {digit}...

digit ::= 0 \| 1 \| 2 \| 3 \| 4 \| 5 \| 6 \| 7 \| 8 \| 9

period ::= . 

## <a name="sql-server-specific-properties"></a><a name="appendixc"></a>Propiedades específicas de SQL Server

```
enum SQLPROPERTIES
       {
       SQLPROP_NOHPNEEDED = 0x1,
       SQLPROP_FREETHREADED = 0x2,
       SQLPROP_UMSENABLED = 0x3,
       SQLPROP_NESTEDQUERIES = 0x4,
       SQLPROP_DYNAMICSQL = 0x5,
       SQLPROP_GROUPBY = 0x6,
       SQLPROP_DATELITERALS = 0x7,
       SQLPROP_ANSILIKE = 0x8,
       SQLPROP_INNERJOIN = 0x9,
       SQLPROP_SUBQUERIES = 0x10, 
       SQLPROP_PARALLELSCAN = 0x11,
       SQLPROP_COLUMNCOLLATION = 0x12,
       SQLPROP_CARDINALITY = 0x13,
       SQLPROP_SIMPLEUPDATES = 0x14,
       SQLPROP_SQLLIKE = 0x15,
       SQLPROP_BITREMOTING = 0x16,
       SQLPROP_UNICODELITERALS = 0x17,
       SQLPROP_USELATESTCOLLATIONVERSION = 0x18
       };

```
