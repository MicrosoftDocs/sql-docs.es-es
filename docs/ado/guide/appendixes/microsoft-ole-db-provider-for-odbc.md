---
description: Introducción al proveedor de Microsoft OLE DB para ODBC
title: Proveedor de OLE DB de Microsoft para ODBC | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB provider for ODBC [ADO]
- providers [ADO], OLE DB provider for ODBC
ms.assetid: 2dc0372d-e74d-4d0f-9c8c-04e5a168c148
author: rothja
ms.author: jroth
ms.openlocfilehash: 5a6cfdda6072f8ed51d55569ed68b113a4148642
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029269"
---
# <a name="microsoft-ole-db-provider-for-odbc-overview"></a>Introducción al proveedor de Microsoft OLE DB para ODBC
Para un programador de ADO o RDS, un mundo ideal sería aquél en el que cada origen de datos expone una interfaz OLE DB, de modo que ADO podría llamar directamente al origen de datos. Aunque cada vez más proveedores de bases de datos implementan interfaces OLE DB, algunos orígenes de datos todavía no se exponen de esta manera. Sin embargo, se puede tener acceso a la mayoría de los sistemas DBMS en uso hoy a través de ODBC.

 Los controladores ODBC están disponibles para todos los DBMS principales que se usan hoy en día, como Microsoft SQL Server, Microsoft Access (motor de base de datos de Microsoft Jet) y Microsoft FoxPro, además de productos de bases de datos que no son de Microsoft, como Oracle.

 El proveedor ODBC de Microsoft, sin embargo, permite a ADO conectarse a cualquier origen de datos ODBC. El proveedor es de subprocesamiento libre y está habilitado para Unicode.

 El proveedor admite transacciones, aunque los distintos motores DBMS ofrecen diferentes tipos de compatibilidad con las transacciones. Por ejemplo, Microsoft Access admite transacciones anidadas hasta cinco niveles de profundidad.

 Este es el proveedor predeterminado para ADO y se admiten todas las propiedades y métodos ADO dependientes del proveedor.

## <a name="connection-string-parameters"></a>Parámetros de la cadena de conexión
 Para conectarse a este proveedor, establezca el argumento **Provider =** de la propiedad [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) en:

```
MSDASQL
```

 La lectura de la propiedad [Provider](../../reference/ado-api/provider-property-ado.md) también devolverá esta cadena.

## <a name="typical-connection-string"></a>Cadena de conexión típica
 Una cadena de conexión típica para este proveedor es:

```
"Provider=MSDASQL;DSN=dsnName;UID=MyUserID;PWD=MyPassword;"
```

 La cadena consta de estas palabras clave:

|Palabra clave|Descripción|
|-------------|-----------------|
|**Proveedor**|Especifica el proveedor de OLE DB para ODBC.|
|**DSN**|Especifica el nombre del origen de datos.|
|**UID**|Especifica el nombre de usuario.|
|**PWD**|Especifica la contraseña del usuario.|
|**URL**|Especifica la dirección URL de un archivo o directorio publicado en una carpeta Web.|

 Dado que este es el proveedor predeterminado para ADO, si se omite el parámetro **Provider =** de la cadena de conexión, ADO intentará establecer una conexión con este proveedor.

> [!NOTE]
>  Si se va a conectar a un proveedor de origen de datos que admite la autenticación de Windows, debe especificar **Trusted_Connection = Yes** o **Integrated Security = SSPI** en lugar de la información de identificador de usuario y contraseña en la cadena de conexión.

 El proveedor no admite ningún parámetro de conexión específico además de los definidos por ADO. Sin embargo, el proveedor pasará los parámetros de conexión que no sean ADO al administrador de controladores ODBC.

 Dado que puede omitir el parámetro **Provider** , puede crear una cadena de conexión ADO que sea idéntica a una cadena de conexión ODBC para el mismo origen de datos. Utilice los mismos nombres de parámetro (**driver =**, **Database =**, **DSN =**, etc.), los valores y la sintaxis como lo haría al crear una cadena de conexión ODBC. Puede conectarse con o sin un nombre de origen de datos (DSN) predefinido o FileDSN.

## <a name="syntax-with-a-dsn-or-filedsn"></a>Sintaxis con un DSN o FileDSN:

```
"[Provider=MSDASQL;] { DSN=name | FileDSN=filename } ;
[DATABASE=database;] UID=user; PWD=password"
```

## <a name="syntax-without-a-dsn-dsn-less-connection"></a>Sintaxis sin DSN (conexión sin DSN):

```
"[Provider=MSDASQL;] DRIVER=driver; SERVER=server;
DATABASE=database; UID=MyUserID; PWD=MyPassword"
```

## <a name="remarks"></a>Observaciones
 Si utiliza un **DSN** o **FileDSN**, debe definirse a través del administrador de orígenes de datos ODBC en el panel de control de Windows. En Microsoft Windows 2000, el administrador de ODBC se encuentra en herramientas administrativas. En versiones anteriores de Windows, el icono de administrador de ODBC se denomina **ODBC de 32** bits o simplemente **ODBC**.

 Como alternativa a la configuración de un **DSN**, puede especificar el controlador ODBC (**driver =**), como "SQL Server;" el nombre del servidor (**Server =**); y el nombre de la base de datos (**Database =**).

 También puede especificar un nombre de cuenta de usuario (**UID =**) y la contraseña de la cuenta de usuario (**pwd =**) en los parámetros específicos de ODBC o en los parámetros estándar de *usuario* y *contraseña* definidos por ADO.

 Aunque una definición de **DSN** ya especifica una base de datos, puede especificar *un* parámetro de *base de datos* además de un **DSN** para conectarse a una base de datos diferente. Es conveniente incluir siempre *el parámetro de* *base de datos* cuando se usa un **DSN**. Esto garantizará que se conecte a la base de datos correcta si otro usuario cambió el parámetro predeterminado de la base de datos desde que se comprobó por última vez la definición de **DSN** .

## <a name="provider-specific-connection-properties"></a>Propiedades de la conexión Provider-Specific
 El proveedor de OLE DB para ODBC agrega varias propiedades a la colección [Properties](../../reference/ado-api/properties-collection-ado.md) del objeto **Connection** . En la tabla siguiente se enumeran estas propiedades con el nombre de propiedad OLE DB correspondiente entre paréntesis.

|Nombre de la propiedad|Descripción|
|-------------------|-----------------|
|Procedimientos accesibles (KAGPROP_ACCESSIBLEPROCEDURES)|Indica si el usuario tiene acceso a los procedimientos almacenados.|
|Tablas accesibles (KAGPROP_ACCESSIBLETABLES)|Indica si el usuario tiene permiso para ejecutar instrucciones SELECT en las tablas de base de datos.|
|Instrucciones activas (KAGPROP_ACTIVESTATEMENTS)|Indica el número de identificadores que puede admitir un controlador ODBC en una conexión.|
|Nombre del controlador (KAGPROP_DRIVERNAME)|Indica el nombre de archivo del controlador ODBC.|
|Versión ODBC del controlador (KAGPROP_DRIVERODBCVER)|Indica la versión de ODBC que admite este controlador.|
|Uso de archivos (KAGPROP_FILEUSAGE)|Indica cómo el controlador trata un archivo en un origen de datos; como una tabla o como un catálogo.|
|Cláusula escape like (KAGPROP_LIKEESCAPECLAUSE)|Indica si el controlador admite la definición y el uso de un carácter de escape para el carácter de porcentaje (%) y subrayado (_) en el predicado LIKE de una cláusula WHERE.|
|Número máximo de columnas en Group by (KAGPROP_MAXCOLUMNSINGROUPBY)|Indica el número máximo de columnas que se pueden enumerar en la cláusula GROUP BY de una instrucción SELECT.|
|Número máximo de columnas en el índice (KAGPROP_MAXCOLUMNSININDEX)|Indica el número máximo de columnas que pueden incluirse en un índice.|
|Número máximo de columnas por orden (KAGPROP_MAXCOLUMNSINORDERBY)|Indica el número máximo de columnas que se pueden enumerar en la cláusula ORDER BY de una instrucción SELECT.|
|Número máximo de columnas de Select (KAGPROP_MAXCOLUMNSINSELECT)|Indica el número máximo de columnas que se pueden mostrar en la parte SELECT de una instrucción SELECT.|
|Número máximo de columnas de la tabla (KAGPROP_MAXCOLUMNSINTABLE)|Indica el número máximo de columnas permitidas en una tabla.|
|Funciones numéricas (KAGPROP_NUMERICFUNCTIONS)|Indica qué funciones numéricas admite el controlador ODBC. Para obtener una lista de los nombres de función y los valores asociados usados en esta máscara de. vea el [Apéndice E: funciones escalares](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), en la documentación de ODBC.|
|Capacidades de combinación externa (KAGPROP_OJCAPABILITY)|Indica los tipos de combinaciones externas admitidas por el proveedor.|
|Combinaciones externas (KAGPROP_OUTERJOINS)|Indica si el proveedor admite combinaciones externas.|
|Caracteres especiales (KAGPROP_SPECIALCHARACTERS)|Indica qué caracteres tienen un significado especial para el controlador ODBC.|
|Procedimientos almacenados (KAGPROP_PROCEDURES)|Indica si los procedimientos almacenados están disponibles para su uso con este controlador ODBC.|
|Funciones de cadena (KAGPROP_STRINGFUNCTIONS)|Indica qué funciones de cadena son compatibles con el controlador ODBC. Para obtener una lista de los nombres de función y los valores asociados usados en esta máscara de. vea el [Apéndice E: funciones escalares](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), en la documentación de ODBC.|
|Funciones del sistema (KAGPROP_SYSTEMFUNCTIONS)|Indica qué funciones del sistema son compatibles con el controlador ODBC. Para obtener una lista de los nombres de función y los valores asociados usados en esta máscara de. vea el [Apéndice E: funciones escalares](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), en la documentación de ODBC.|
|Funciones de fecha y hora (KAGPROP_TIMEDATEFUNCTIONS)|Indica qué funciones de fecha y hora admite el controlador ODBC. Para obtener una lista de los nombres de función y los valores asociados usados en esta máscara de. vea el [Apéndice E: funciones escalares](../../../odbc/reference/appendixes/appendix-e-scalar-functions.md), en la documentación de ODBC.|
|Compatibilidad con la gramática de SQL (KAGPROP_ODBCSQLCONFORMANCE)|Indica la gramática de SQL que admite el controlador ODBC.|

## <a name="provider-specific-recordset-and-command-properties"></a>Provider-Specific conjunto de registros y propiedades de comando
 El proveedor de OLE DB para ODBC agrega varias propiedades a la colección **Properties** del **conjunto de registros** y los objetos de **comando** . En la tabla siguiente se enumeran estas propiedades con el nombre de propiedad OLE DB correspondiente entre paréntesis.

|Nombre de la propiedad|Descripción|
|-------------------|-----------------|
|Actualizaciones/eliminaciones/inserciones basadas en consultas (KAGPROP_QUERYBASEDUPDATES)|Indica si las actualizaciones, eliminaciones e inserciones se pueden realizar mediante consultas SQL.|
|Tipo de simultaneidad de ODBC (KAGPROP_CONCURRENCY)|Indica el método utilizado para reducir los posibles problemas causados por dos usuarios que intentan obtener acceso a los mismos datos del origen de datos simultáneamente.|
|Accesibilidad de BLOBs en Forward-Only cursor (KAGPROP_BLOBSONFOCURSOR)|Indica si se puede tener acceso a **los campos** BLOB cuando se usa un cursor de solo avance.|
|Incluir SQL_FLOAT, SQL_DOUBLE y SQL_REAL en las cláusulas WHERE de QBU (KAGPROP_INCLUDENONEXACT)|Indica si los valores de SQL_FLOAT, SQL_DOUBLE y SQL_REAL se pueden incluir en una cláusula WHERE de QBU.|
|Posición en la última fila después de la inserción (KAGPROP_POSITIONONNEWROW)|Indica que, una vez insertado un nuevo registro en una tabla, la última fila de la tabla se incluirá en la fila actual.|
|IRowsetChangeExtInfo (KAGPROP_IROWSETCHANGEEXTINFO)|Indica si la interfaz **IRowsetChange** proporciona compatibilidad con la información extendida.|
|Tipo de cursor de ODBC (KAGPROP_CURSOR)|Indica el tipo de cursor utilizado por el **conjunto de registros**.|
|Generar un conjunto de filas cuyas referencias se pueden calcular (KAGPROP_MARSHALLABLE)|Indica que el controlador ODBC genera un conjunto de registros en el que se pueden calcular las referencias|

## <a name="command-text"></a>Texto de comando
 La forma en que use el objeto de [comando](../../reference/ado-api/command-object-ado.md) en gran medida dependerá del origen de datos y del tipo de instrucción de consulta o comando que aceptará.

 ODBC proporciona una sintaxis específica para llamar a procedimientos almacenados. En el caso de la propiedad [CommandText](../../reference/ado-api/commandtext-property-ado.md) de un objeto **Command** , el argumento *CommandText* para el método **Execute** en un objeto [Connection](../../reference/ado-api/connection-object-ado.md) , o el argumento *source* para el método **Open** en un objeto [Recordset](../../reference/ado-api/recordset-object-ado.md) , pasa una cadena con esta sintaxis:

```
"{ [ ? = ] call procedure [ ( ? [, ? [ , ... ]] ) ] }"
```

 Cada uno **?** hace referencia a un objeto de la colección [Parameters](../../reference/ado-api/parameters-collection-ado.md) . ¿Primero **?** hace referencia a los **parámetros**(0), el siguiente **?** hace referencia a **los parámetros**(1), etc.

 Las referencias de parámetro son opcionales y dependen de la estructura del procedimiento almacenado. Si desea llamar a un procedimiento almacenado que no define ningún parámetro, la cadena tendrá el aspecto siguiente:

```
"{ call procedure }"
```

 Si tiene dos parámetros de consulta, la cadena sería similar a la siguiente:

```
"{ call procedure ( ?, ? ) }"
```

 Si el procedimiento almacenado devuelve un valor, el valor devuelto se trata como otro parámetro. Si no tiene ningún parámetro de consulta pero tiene un valor devuelto, la cadena sería similar a la siguiente:

```
"{ ? = call procedure }"
```

 Por último, si tiene un valor devuelto y dos parámetros de consulta, la cadena sería similar a la siguiente:

```
"{ ? = call procedure ( ?, ? ) }"
```

## <a name="recordset-behavior"></a>Comportamiento del conjunto de registros
 En las tablas siguientes se enumeran los métodos y propiedades de ADO estándar disponibles en un objeto de **conjunto de registros** abierto con este proveedor.

 Para obtener información más detallada sobre el comportamiento del **conjunto de registros** para la configuración del proveedor, ejecute el método [Supports](../../reference/ado-api/supports-method.md) y enumere la colección **Properties** del **conjunto de registros** para determinar si las propiedades dinámicas específicas del proveedor están presentes.

 Disponibilidad de propiedades de **conjunto de registros** de ADO estándar:

|Propiedad|ForwardOnly|Dinámica|Keyset|estática|
|--------------|-----------------|-------------|------------|------------|
|[AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md)|no disponible|no disponible|lectura/escritura|lectura/escritura|
|[AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md)|no disponible|no disponible|lectura/escritura|lectura/escritura|
|[ActiveConnection](../../reference/ado-api/activeconnection-property-ado.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[BOF](../../reference/ado-api/bof-eof-properties-ado.md)|solo lectura|solo lectura|solo lectura|solo lectura|
|[Marcador](../../reference/ado-api/bookmark-property-ado.md)|no disponible|no disponible|lectura/escritura|lectura/escritura|
|[CacheSize](../../reference/ado-api/cachesize-property-ado.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[CursorType](../../reference/ado-api/cursortype-property-ado.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[EditMode](../../reference/ado-api/editmode-property.md)|solo lectura|solo lectura|solo lectura|solo lectura|
|[Filter](../../reference/ado-api/filter-property.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[LockType](../../reference/ado-api/locktype-property-ado.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[MarshalOptions](../../reference/ado-api/marshaloptions-property-ado.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[MaxRecords](../../reference/ado-api/maxrecords-property-ado.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[NúmeroDePáginas](../../reference/ado-api/pagecount-property-ado.md)|lectura/escritura|no disponible|solo lectura|solo lectura|
|[PageSize](../../reference/ado-api/pagesize-property-ado.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[RecordCount](../../reference/ado-api/recordcount-property-ado.md)|lectura/escritura|no disponible|solo lectura|solo lectura|
|[Origen](../../reference/ado-api/source-property-ado-recordset.md)|lectura/escritura|lectura/escritura|lectura/escritura|lectura/escritura|
|[Estado](../../reference/ado-api/state-property-ado.md)|solo lectura|solo lectura|solo lectura|solo lectura|
|[Estado](../../reference/ado-api/status-property-ado-recordset.md)|solo lectura|solo lectura|solo lectura|solo lectura|

 Las propiedades [AbsolutePosition](../../reference/ado-api/absoluteposition-property-ado.md) y [AbsolutePage](../../reference/ado-api/absolutepage-property-ado.md) son de solo escritura cuando ADO se usa con la versión 1,0 del proveedor de Microsoft OLE DB para ODBC.

 Disponibilidad de métodos de **conjunto de registros** de ADO estándar:

|Método|ForwardOnly|Dinámica|Keyset|estática|
|------------|-----------------|-------------|------------|------------|
|[AgregarNuevo](../../reference/ado-api/addnew-method-ado.md)|Sí|Sí|Sí|Sí|
|[Cancelar](../../reference/ado-api/cancel-method-ado.md)|Sí|Sí|Sí|Sí|
|[CancelBatch](../../reference/ado-api/cancelbatch-method-ado.md)|Sí|Sí|Sí|Sí|
|[CancelUpdate](../../reference/ado-api/cancelupdate-method-ado.md)|Sí|Sí|Sí|Sí|
|[Clonar](../../reference/ado-api/clone-method-ado.md)|No|No|Sí|Sí|
|[Cerrar](../../reference/ado-api/close-method-ado.md)|Sí|Sí|Sí|Sí|
|[Eliminar](../../reference/ado-api/delete-method-ado-recordset.md)|Sí|Sí|Sí|Sí|
|[GetRows](../../reference/ado-api/getrows-method-ado.md)|Sí|Sí|Sí|Sí|
|[Mover](../../reference/ado-api/move-method-ado.md)|Sí|Sí|Sí|Sí|
|[MoveFirst](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Sí|Sí|Sí|Sí|
|[MoveLast](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|No|Sí|Sí|Sí|
|[MoveNext](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Sí|Sí|Sí|Sí|
|[MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|No|Sí|Sí|Sí|
|[NextRecordset](../../reference/ado-api/nextrecordset-method-ado.md)*|Sí|Sí|Sí|Sí|
|[Abrir](../../reference/ado-api/open-method-ado-recordset.md)|Sí|Sí|Sí|Sí|
|[Requery](../../reference/ado-api/requery-method.md)|Sí|Sí|Sí|Sí|
|[Resincronizar](../../reference/ado-api/resync-method.md)|No|No|Sí|Sí|
|[Es compatible con](../../reference/ado-api/supports-method.md)|Sí|Sí|Sí|Sí|
|[Actualizar](../../reference/ado-api/update-method.md)|Sí|Sí|Sí|Sí|
|[UpdateBatch](../../reference/ado-api/updatebatch-method.md)|Sí|Sí|Sí|Sí|

 * No se admite para las bases de datos de Microsoft Access.

## <a name="dynamic-properties"></a>Propiedades dinámicas
 El proveedor de Microsoft OLE DB para ODBC inserta varias propiedades dinámicas en la colección de **propiedades** de los objetos de [conexión](../../reference/ado-api/connection-object-ado.md), [conjunto de registros](../../reference/ado-api/recordset-object-ado.md)y [comando](../../reference/ado-api/command-object-ado.md) no abiertos.

 Las tablas siguientes son un índice entre los nombres de ADO y OLE DB para cada propiedad dinámica. La referencia del programador de OLE DB hace referencia a un nombre de propiedad ADO por el término "Descripción". Puede encontrar más información sobre estas propiedades en la referencia del programador de OLE DB. Busque el nombre de la propiedad OLE DB en el índice o vea el [Apéndice C: OLE DB propiedades](/previous-versions/windows/desktop/ms723130(v=vs.85)).

## <a name="connection-dynamic-properties"></a>Propiedades dinámicas de conexión
 Las propiedades siguientes se agregan a la colección de **propiedades** del objeto de **conexión** .

|Nombre de la propiedad ADO|OLE DB nombre de propiedad|
|-----------------------|--------------------------|
|Sesiones activas|DBPROP_ACTIVESESSIONS|
|Anulación de asíncrona|DBPROP_ASYNCTXNABORT|
|Confirmación de asíncrona|DBPROP_ASYNCTNXCOMMIT|
|Niveles de aislamiento de confirmación automática|DBPROP_SESS_AUTOCOMMITISOLEVELS|
|Ubicación del catálogo|DBPROP_CATALOGLOCATION|
|Término de catálogo|DBPROP_CATALOGTERM|
|Definición de columna|DBPROP_COLUMNDEFINITION|
|Tiempo de espera de la conexión|DBPROP_INIT_TIMEOUT|
|Catálogo actual|DBPROP_CURRENTCATALOG|
|Origen de datos|DBPROP_INIT_DATASOURCE|
|Data Source Name|DBPROP_DATASOURCENAME|
|Modelo de subprocesos de objetos de origen de datos|DBPROP_DSOTHREADMODEL|
|Nombre DBMS|DBPROP_DBMSNAME|
|Versión DBMS|DBPROP_DBMSVER|
|Propiedades extendidas|DBPROP_INIT_PROVIDERSTRING|
|AGRUPAr por compatibilidad|DBPROP_GROUPBY|
|Compatibilidad con tablas heterogéneas|DBPROP_HETEROGENEOUSTABLES|
|Distinción de mayúsculas y minúsculas de identificadores|DBPROP_IDENTIFIERCASE|
|Catálogo original|DBPROP_INIT_CATALOG|
|Niveles de aislamiento|DBPROP_SUPPORTEDTXNISOLEVELS|
|Retención de aislamiento|DBPROP_SUPPORTEDTXNISORETAIN|
|Identificador de configuración regional|DBPROP_INIT_LCID|
|Location|DBPROP_INIT_LOCATION|
|Tamaño máximo del índice|DBPROP_MAXINDEXSIZE|
|Tamaño máximo de fila|DBPROP_MAXROWSIZE|
|El tamaño máximo de fila incluye BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|Número máximo de tablas en SELECT|DBPROP_MAXTABLESINSELECT|
|Modo|DBPROP_INIT_MODE|
|Varios conjuntos de parámetros|DBPROP_MULTIPLEPARAMSETS|
|Varios resultados|DBPROP_MULTIPLERESULTS|
|Varios objetos de almacenamiento|DBPROP_MULTIPLESTORAGEOBJECTS|
|Actualización de varias tablas|DBPROP_MULTITABLEUPDATE|
|Orden de intercalación NULL|DBPROP_NULLCOLLATION|
|Comportamiento de concatenación NULL|DBPROP_CONCATNULLBEHAVIOR|
|Servicios OLE DB|DBPROP_INIT_OLEDBSERVICES|
|Versión de OLE DB|DBPROP_PROVIDEROLEDBVER|
|Compatibilidad con objetos OLE|DBPROP_OLEOBJECTS|
|Compatibilidad con abrir conjuntos de filas|DBPROP_OPENROWSETSUPPORT|
|ORDENAR por columnas en la lista de selección|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilidad del parámetro de salida|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Contraseña|DBPROP_AUTH_PASSWORD|
|Descriptores de acceso pass by Ref|DBPROP_BYREFACCESSORS|
|Persist Security Info|DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO|
|Tipo de ID. persistente|DBPROP_PERSISTENTIDTYPE|
|Preparar el comportamiento de anulación|DBPROP_PREPAREABORTBEHAVIOR|
|Preparar el comportamiento de confirmación|DBPROP_PREPARECOMMITBEHAVIOR|
|Término del procedimiento|DBPROP_PROCEDURETERM|
|Solicitud|DBPROP_INIT_PROMPT|
|Nombre descriptivo del proveedor|DBPROP_PROVIDERFRIENDLYNAME|
|Nombre del proveedor|DBPROP_PROVIDERFILENAME|
|Versión del proveedor|DBPROP_PROVIDERVER|
|Read-Only origen de datos|DBPROP_DATASOURCEREADONLY|
|Conversiones del conjunto de filas en el comando|DBPROP_ROWSETCONVERSIONSONCOMMAND|
|Término de esquema|DBPROP_SCHEMATERM|
|Uso de esquemas|DBPROP_SCHEMAUSAGE|
|Soporte técnico de SQL|DBPROP_SQLSUPPORT|
|Almacenamiento estructurado|DBPROP_STRUCTUREDSTORAGE|
|Compatibilidad con subconsultas|DBPROP_SUBQUERIES|
|Término de tabla|DBPROP_TABLETERM|
|DDL de transacción|DBPROP_SUPPORTEDTXNDDL|
|Id. de usuario|DBPROP_AUTH_USERID|
|Nombre de usuario|DBPROP_USERNAME|
|Identificador de ventana|DBPROP_INIT_HWND|

## <a name="recordset-dynamic-properties"></a>Propiedades dinámicas del conjunto de registros
 Las propiedades siguientes se agregan a la colección de **propiedades** del objeto de **conjunto de registros** .

|Nombre de la propiedad ADO|OLE DB nombre de propiedad|
|-----------------------|--------------------------|
|Orden de acceso|DBPROP_ACCESSORDER|
|Bloquear objetos de almacenamiento|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo de marcador|DBPROP_BOOKMARKTYPE|
|Marcador|DBPROP_IROWSETLOCATE|
|Cambiar filas insertadas|DBPROP_CHANGEINSERTEDROWS|
|Privilegios de columna|DBPROP_COLUMNRESTRICT|
|Notificación de conjunto de columnas|DBPROP_NOTIFYCOLUMNSET|
|Retraso de las actualizaciones de objetos de almacenamiento|DBPROP_DELAYSTORAGEOBJECTS|
|Capturar hacia atrás|DBPROP_CANFETCHBACKWARDS|
|Almacenar filas|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|Filas immóviles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Marcadores literales|DBPROP_LITERALBOOKMARKS|
|Identidad de fila literal|DBPROP_LITERALIDENTITY|
|Número máximo de filas abiertas|DBPROP_MAXOPENROWS|
|Número máximo de filas pendientes|DBPROP_MAXPENDINGROWS|
|Número máximo de filas|DBPROP_MAXROWS|
|Granularidad de notificaciones|DBPROP_NOTIFICATIONGRANULARITY|
|Fases de notificación|DBPROP_NOTIFICATIONPHASES|
|Objetos con transacciones|DBPROP_TRANSACTEDOBJECT|
|Propios cambios visibles|DBPROP_OWNUPDATEDELETE|
|Propias inserciones visibles|DBPROP_OWNINSERT|
|Conservar al anular|DBPROP_ABORTPRESERVE|
|Conservar al confirmar|DBPROP_COMMITPRESERVE|
|Reinicio rápido|DBPROP_QUICKRESTART|
|Eventos reentrantes|DBPROP_REENTRANTEVENTS|
|Quitar filas eliminadas|DBPROP_REMOVEDELETED|
|Informar de varios cambios|DBPROP_REPORTMULTIPLECHANGES|
|Devolver inserciones pendientes|DBPROP_RETURNPENDINGINSERTS|
|Notificación de eliminación de fila|DBPROP_NOTIFYROWDELETE|
|Notificación de cambio de primera fila|DBPROP_NOTIFYROWFIRSTCHANGE|
|Notificación de inserción de fila|DBPROP_NOTIFYROWINSERT|
|Privilegios de fila|DBPROP_ROWRESTRICT|
|Notificación de resincronización de filas|DBPROP_NOTIFYROWRESYNCH|
|Modelo de subprocesos de filas|DBPROP_ROWTHREADMODEL|
|Notificación de cambio de deshacer filas|DBPROP_NOTIFYROWUNDOCHANGE|
|Notificación de eliminación de fila deshacer|DBPROP_NOTIFYROWUNDODELETE|
|Anular la notificación de inserción de fila|DBPROP_NOTIFYROWUNDOINSERT|
|Notificación de actualización de fila|DBPROP_NOTIFYROWUPDATE|
|Notificación de cambio de posición de captura de conjunto de filas|DBPROP_NOTIFYROWSETFETCHPOSISIONCHANGE|
|Notificación de versión del conjunto de filas|DBPROP_NOTIFYROWSETRELEASE|
|Desplazarse hacia atrás|DBPROP_CANSCROLLBACKWARDS|
|Omitir marcadores eliminados|DBPROP_BOOKMARKSKIPPED|
|Identidad de fila segura|DBPROP_STRONGITDENTITY|
|Filas únicas|DBPROP_UNIQUEROWS|
|Actualización|DBPROP_UPDATABILITY|
|Usar marcadores|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propiedades dinámicas de comandos
 Las propiedades siguientes se agregan a la colección de **propiedades** del objeto de **comando** .

|Nombre de la propiedad ADO|OLE DB nombre de propiedad|
|-----------------------|--------------------------|
|Orden de acceso|DBPROP_ACCESSORDER|
|Bloquear objetos de almacenamiento|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo de marcador|DBPROP_BOOKMARKTYPE|
|Marcador|DBPROP_IROWSETLOCATE|
|Cambiar filas insertadas|DBPROP_CHANGEINSERTEDROWS|
|Privilegios de columna|DBPROP_COLUMNRESTRICT|
|Notificación de conjunto de columnas|DBPROP_NOTIFYCOLUMNSET|
|Retraso de las actualizaciones de objetos de almacenamiento|DBPROP_DELAYSTORAGEOBJECTS|
|Capturar hacia atrás|DBPROP_CANFETCHBACKWARDS|
|Almacenar filas|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|Filas immóviles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Marcadores literales|DBPROP_LITERALBOOKMARKS|
|Identidad de fila literal|DBPROP_LITERALIDENTITY|
|Número máximo de filas abiertas|DBPROP_MAXOPENROWS|
|Número máximo de filas pendientes|DBPROP_MAXPENDINGROWS|
|Número máximo de filas|DBPROP_MAXROWS|
|Granularidad de notificaciones|DBPROP_NOTIFICATIONGRANULARITY|
|Fases de notificación|DBPROP_NOTIFICATIONPHASES|
|Objetos con transacciones|DBPROP_TRANSACTEDOBJECT|
|Propios cambios visibles|DBPROP_OWNUPDATEDELETE|
|Propias inserciones visibles|DBPROP_OWNINSERT|
|Conservar al anular|DBPROP_ABORTPRESERVE|
|Conservar al confirmar|DBPROP_COMMITPRESERVE|
|Reinicio rápido|DBPROP_QUICKRESTART|
|Eventos reentrantes|DBPROP_REENTRANTEVENTS|
|Quitar filas eliminadas|DBPROP_REMOVEDELETED|
|Informar de varios cambios|DBPROP_REPORTMULTIPLECHANGES|
|Devolver inserciones pendientes|DBPROP_RETURNPENDINGINSERTS|
|Notificación de eliminación de fila|DBPROP_NOTIFYROWDELETE|
|Notificación de cambio de primera fila|DBPROP_NOTIFYROWFIRSTCHANGE|
|Notificación de inserción de fila|DBPROP_NOTIFYROWINSERT|
|Privilegios de fila|DBPROP_ROWRESTRICT|
|Notificación de resincronización de filas|DBPROP_NOTIFYROWRESYNCH|
|Modelo de subprocesos de filas|DBPROP_ROWTHREADMODEL|
|Notificación de cambio de deshacer filas|DBPROP_NOTIFYROWUNDOCHANGE|
|Notificación de eliminación de fila deshacer|DBPROP_NOTIFYROWUNDODELETE|
|Anular la notificación de inserción de fila|DBPROP_NOTIFYROWUNDOINSERT|
|Notificación de actualización de fila|DBPROP_NOTIFYROWUPDATE|
|Notificación de cambio de posición de captura de conjunto de filas|DBPROP_NOTIFYROWSETFETCHPOSITIONCHANGE|
|Notificación de versión del conjunto de filas|DBPROP_NOTIFYROWSETRELEASE|
|Desplazarse hacia atrás|DBPROP_CANSCROLLBACKWARDS|
|Omitir marcadores eliminados|DBPROP_BOOKMARKSKIP|
|Identidad de fila segura|DBPROP_STRONGIDENTITY|
|Actualización|DBPROP_UPDATABILITY|
|Usar marcadores|DBPROP_BOOKMARKS|

 Para obtener más información sobre la implementación específica y la información funcional sobre el proveedor de Microsoft OLE DB para ODBC, vea la [Referencia del programador de OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85)) o visite el sitio web del centro para desarrolladores de acceso a datos y almacenamiento en MSDN.

## <a name="see-also"></a>Consulte también
 [Objeto de comando (ADO)](../../reference/ado-api/command-object-ado.md) [propiedad CommandText (](../../reference/ado-api/commandtext-property-ado.md) ADO) [objeto de conexión](../../reference/ado-api/connection-object-ado.md) (ADO) ConnectionString (propiedad) ( [ADO](../../reference/ado-api/connectionstring-property-ado.md) ) [método execute (comando de ADO](../../reference/ado-api/execute-method-ado-command.md) ) [Open Method (ADO Recordset)](../../reference/ado-api/open-method-ado-recordset.md) [Parameters](../../reference/ado-api/parameters-collection-ado.md) (ADO) [Properties](../../reference/ado-api/properties-collection-ado.md) (colección) (ADO) [propiedad Provider](../../reference/ado-api/provider-property-ado.md) (ADO) [Recordset Object (ADO)](../../reference/ado-api/recordset-object-ado.md) [admite el método](../../reference/ado-api/supports-method.md)