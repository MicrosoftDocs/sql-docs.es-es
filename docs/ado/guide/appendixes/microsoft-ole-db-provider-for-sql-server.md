---
description: Introducción al proveedor de Microsoft OLE DB para SQL Server
title: Proveedor de OLE DB de Microsoft para SQL Server | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- providers [ADO], OLE DB provider for SQL Server
- OLE DB provider for SQL Server [ADO]
- SQLOLEDB [ADO]
ms.assetid: 99bc40c4-9181-4ca1-a06f-9a1a914a0b7b
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b6a62e8946415cc9c1e869a173de43b98ba14f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029259"
---
# <a name="microsoft-ole-db-provider-for-sql-server-overview"></a>Introducción al proveedor de Microsoft OLE DB para SQL Server
El proveedor de OLE DB de Microsoft para SQL Server, SQLOLEDB, permite a ADO tener acceso a Microsoft SQL Server.

> [!IMPORTANT]
> El proveedor de Microsoft OLE DB para SQL Server (SQLOLEDB) sigue en desuso y no se recomienda usarlo para nuevos trabajos de desarrollo. En su lugar, use el nuevo [Microsoft OLE DB Driver for SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), que se actualizará con las características de servidor más recientes.

## <a name="connection-string-parameters"></a>Parámetros de la cadena de conexión
 Para conectarse a este proveedor, establezca el argumento de *proveedor* en la propiedad [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) en:

```vb
SQLOLEDB
```

 Este valor también se puede establecer o leer mediante la propiedad [Provider](../../reference/ado-api/provider-property-ado.md) .

## <a name="typical-connection-string"></a>Cadena de conexión típica
 Una cadena de conexión típica para este proveedor es:

```vb
"Provider=SQLOLEDB;Data Source=serverName;"
Initial Catalog=databaseName;
User ID=MyUserID;Password=MyPassword;"
```

 La cadena consta de estas palabras clave:

|Palabra clave|Descripción|
|-------------|-----------------|
|**Proveedor**|Especifica el proveedor de OLE DB para SQL Server.|
|**Origen de datos** o **servidor**|Especifica el nombre de un servidor.|
|**Catálogo** o **base de datos** inicial|Especifica el nombre de una base de datos en el servidor.|
|**Identificador de usuario** o **UID**|Especifica el nombre de usuario (para la autenticación SQL Server).|
|**Password** o **pwd**|Especifica la contraseña de usuario (para la autenticación SQL Server).|

> [!NOTE]
>  Si se va a conectar a un proveedor de origen de datos que admite la autenticación de Windows, debe especificar **Trusted_Connection = Yes** o **Integrated Security = SSPI** en lugar de la información de identificador de usuario y contraseña en la cadena de conexión.

## <a name="provider-specific-connection-parameters"></a>Provider-Specific de los parámetros de conexión
 El proveedor admite varios parámetros de conexión específicos del proveedor además de los definidos por ADO. Al igual que con las propiedades de conexión ADO, estas propiedades específicas del proveedor se pueden establecer a través de la colección [Properties](../../reference/ado-api/properties-collection-ado.md) de una [conexión](../../reference/ado-api/connection-object-ado.md) o se pueden establecer como parte de **ConnectionString**.

|Parámetro|Descripción|
|---------------|-----------------|
|Trusted_Connection|Indica el modo de autenticación del usuario. Se puede establecer en **sí** o **no**. El valor predeterminado es **No**. Si esta propiedad se establece en **sí**, SQLOLEDB utiliza el modo de autenticación de Microsoft Windows NT para autorizar el acceso del usuario a la base de datos de SQL Server especificada por los valores de la propiedad **Location** y [DataSource](../../reference/ado-api/datasource-property-ado.md) . Si esta propiedad se establece en **no**, SQLOLEDB utiliza el modo mixto para autorizar el acceso del usuario a la base de datos de SQL Server. El inicio de sesión de SQL Server y la contraseña se especifican en las propiedades ID. de **usuario** y **contraseña** .|
|Idioma actual|Indica un nombre de lenguaje de SQL Server. Identifica el lenguaje que se usa para la selección y el formato de mensajes del sistema. El idioma debe instalarse en el SQL Server; de lo contrario, se producirá un error al abrir la conexión.|
|Dirección de red|Indica la dirección de red del SQL Server especificado por la propiedad **Location** .|
|Biblioteca de red|Indica el nombre de la biblioteca de red (DLL) utilizada para comunicarse con el SQL Server. El nombre no debe incluir la ruta de acceso ni la extensión de nombre de archivo .dll. La configuración del cliente de SQL Server proporciona el valor predeterminado.|
|Usar el procedimiento para preparar|Determina si SQL Server crea procedimientos almacenados temporales cuando se preparan los comandos (mediante la propiedad **Prepared** ).|
|Auto Translate|Indica si se convierten los caracteres OEM/ANSI. Esta propiedad se puede establecer en **true** o en **false**. El valor predeterminado es **True**. Si esta propiedad se establece en **true**, SQLOLEDB realiza la conversión de caracteres OEM/ANSI cuando las cadenas de caracteres de varios bytes se recuperan o se envían al SQL Server. Si esta propiedad se establece en **false**, SQLOLEDB no realiza la conversión de caracteres OEM/ANSI en los datos de cadenas de caracteres multibyte.|
|Tamaño del paquete|Indica un tamaño de paquete de red en bytes. El valor de la propiedad tamaño de paquete debe estar entre 512 y 32767. El tamaño de paquete de red SQLOLEDB predeterminado es 4096.|
|Nombre de la aplicación|Indica el nombre de la aplicación cliente.|
|Id. de estación de trabajo|cadena que identifica la estación de trabajo.|

## <a name="command-object-usage"></a>Uso de objetos de comando
 SQLOLEDB acepta una amalgama de ODBC, ANSI y Transact-SQL específico de SQL Server como sintaxis válida. Por ejemplo, la siguiente instrucción SQL utiliza una secuencia de escape de ODBC SQL para especificar la función de cadena LCASE:

```sql
SELECT customerid={fn LCASE(CustomerID)} FROM Customers

```

 LCASE devuelve una cadena de caracteres, convirtiendo todos los caracteres en mayúscula en sus equivalentes en minúscula. La función de cadena ANSI SQL anterior realiza la misma operación, por lo que la siguiente instrucción SQL es un equivalente de ANSI a la instrucción ODBC presentada anteriormente:

```sql
SELECT customerid=LOWER(CustomerID) FROM Customers

```

 SQLOLEDB procesa correctamente cualquier forma de la instrucción cuando se especifica como texto para un comando.

## <a name="stored-procedures"></a>Procedimientos almacenados
 Al ejecutar un SQL Server procedimiento almacenado mediante un comando SQLOLEDB, utilice la secuencia de escape de la llamada al procedimiento ODBC en el texto del comando. SQLOLEDB utiliza el mecanismo de llamada a procedimiento remoto de SQL Server para optimizar el procesamiento de comandos. Por ejemplo, la siguiente instrucción SQL de ODBC es el texto de comando preferido sobre el formulario de Transact-SQL:

## <a name="odbc-sql"></a>ODBC SQL

```vb
{call SalesByCategory('Produce', '1995')}

```

## <a name="transact-sql"></a>Transact-SQL

```sql
EXECUTE SalesByCategory 'Produce', '1995'

```

## <a name="sql-server-features"></a>SQL Server características
 Con SQL Server, ADO puede usar XML para la entrada de **comandos** y recuperar los resultados en formato de secuencia XML en lugar de en objetos de **conjunto de registros** . Para obtener más información, vea [usar secuencias para la entrada de comandos](../data/command-streams.md) y [recuperar conjuntos de información en secuencias](../data/retrieving-resultsets-into-streams.md).

### <a name="accessing-sql_variant-data-using-mdac-27-mdac-28-or-windows-dac-60"></a>Obtener acceso a los datos de sql_variant mediante MDAC 2,7, MDAC 2,8 o Windows DAC 6,0
 Microsoft SQL Server tiene un tipo de datos denominado **sql_variant**. De forma similar a la **DBTYPE_VARIANT** de OLE DB, el tipo de datos **sql_variant** puede almacenar datos de varios tipos diferentes. Sin embargo, hay algunas diferencias clave entre **DBTYPE_VARIANT** y **sql_variant**. ADO también controla los datos almacenados como un valor **sql_variant** de forma distinta a como se trata de otros tipos de datos. En la lista siguiente se describen los problemas que se deben tener en cuenta cuando se tiene acceso a SQL Server datos almacenados en columnas de tipo **sql_variant**.

-   En MDAC 2,7, MDAC 2,8 y Windows Data Access Components (Windows DAC) 6,0, el proveedor de OLE DB para SQL Server admite el tipo de **sql_variant** . El proveedor de OLE DB para ODBC no lo hace.

-   El tipo de **sql_variant** no coincide exactamente con el tipo de datos **DBTYPE_VARIANT** .  El tipo de **sql_variant** admite algunos subtipos nuevos no admitidos por **DBTYPE_VARIANT,** incluidas las cadenas de **GUID**, **ANSI** (no Unicode) y **BIGINT**. El uso de subtipos distintos de los mencionados anteriormente funcionará correctamente.

-   El **valor numérico** del subtipo **sql_variant** no coincide con el **DBTYPE_DECIMAL** de tamaño.

-   Si hay varias conversiones de tipos de datos, se producirán tipos que no coinciden. Por ejemplo, la conversión de un **sql_variant** con un subtipo de **GUID** en un **DBTYPE_VARIANT** producirá un subtipo de **SAFEARRAY**(bytes). Si vuelve a convertir este tipo en un **sql_variant** , se producirá un nuevo subtipo de **matriz**(bytes).

-   Los campos de **conjunto de registros** que contienen datos de **sql_variant** se pueden reenviar (calcular las referencias) o conservarse solo si el **sql_variant** contiene subtipos específicos. Si se intentan almacenar datos de forma remota con los siguientes subtipos no admitidos, se producirá un error en tiempo de ejecución (conversión no compatible) del proveedor de persistencia de Microsoft (MSPersist): **VT_VARIANT**, **VT_RECORD**, **VT_ILLEGAL**, **VT_UNKNOWN**, **VT_BSTR** y **VT_DISPATCH.**

-   El proveedor de OLE DB para SQL Server en MDAC 2,7, MDAC 2,8 y Windows DAC 6,0 tiene una propiedad dinámica denominada **permitir variantes nativas** que, como el nombre implica, permite a los desarrolladores tener acceso a la **sql_variant** en su formato nativo en lugar de a una **DBTYPE_VARIANT**. Si se establece esta propiedad y se abre un **conjunto de registros** con el motor de cursor de cliente (**adUseClient**), se producirá un error en la llamada a **Recordset. Open** . Si se establece esta propiedad y se abre un **conjunto de registros** con cursores de servidor (**adUseServer**), la llamada a **Recordset. Open** se realizará correctamente, pero el acceso a las columnas de tipo **sql_variant** producirá un error.

-   En las aplicaciones cliente que usan MDAC 2,5, se pueden usar **sql_variant** datos con consultas en Microsoft SQL Server. Sin embargo, los valores de los datos de **sql_variant** se tratan como cadenas. Estas aplicaciones cliente deben actualizarse a MDAC 2,7, MDAC 2,8 o Windows DAC 6,0.

## <a name="recordset-behavior"></a>Comportamiento del conjunto de registros
 SQLOLEDB no puede usar SQL Server cursores para admitir el resultado múltiple generado por muchos comandos. Si un consumidor solicita un conjunto de registros que requiere SQL Server compatibilidad con cursores, se produce un error si el texto del comando utilizado genera más de un conjunto de registros como resultado.

 Los objetos de conjunto de registros SQLOLEDB desplazables son compatibles con los cursores SQL Server. SQL Server impone limitaciones en los cursores que son sensibles a los cambios realizados por otros usuarios de la base de datos. En concreto, no se pueden ordenar las filas de algunos cursores y se puede producir un error al intentar crear un conjunto de registros con un comando que contenga una cláusula ORDER BY de SQL.

## <a name="dynamic-properties"></a>Propiedades dinámicas
 El proveedor de OLE DB de Microsoft para SQL Server inserta varias propiedades dinámicas en la colección de **propiedades** de los objetos de [conexión](../../reference/ado-api/connection-object-ado.md), [conjunto de registros](../../reference/ado-api/recordset-object-ado.md)y [comando](../../reference/ado-api/command-object-ado.md) no abiertos.

 Las tablas siguientes son un índice entre los nombres de ADO y OLE DB para cada propiedad dinámica. La referencia del programador de OLE DB hace referencia a un nombre de propiedad ADO por el término "Descripción". Puede encontrar más información sobre estas propiedades en la referencia del programador de OLE DB. Busque el nombre de la propiedad OLE DB en el índice o vea el [Apéndice C: OLE DB propiedades](/previous-versions/windows/desktop/ms723130(v=vs.85)).

## <a name="connection-dynamic-properties"></a>Propiedades dinámicas de conexión
 Las propiedades siguientes se agregan a la colección **Properties** del objeto **Connection** .

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
|Tamaño máximo del índice|DBPROP_MAXINDEXSIZE|
|Tamaño máximo de fila|DBPROP_MAXROWSIZE|
|El tamaño máximo de fila incluye BLOB|DBPROP_MAXROWSIZEINCLUDESBLOB|
|Número máximo de tablas en SELECT|DBPROP_MAXTABLESINSELECT|
|Varios conjuntos de parámetros|DBPROP_MULTIPLEPARAMSETS|
|Varios resultados|DBPROP_MULTIPLERESULTS|
|Varios objetos de almacenamiento|DBPROP_MULTIPLESTORAGEOBJECTS|
|Actualización de varias tablas|DBPROP_MULTITABLEUPDATE|
|Orden de intercalación NULL|DBPROP_NULLCOLLATION|
|Comportamiento de concatenación NULL|DBPROP_CONCATNULLBEHAVIOR|
|Versión de OLE DB|DBPROP_PROVIDEROLEDBVER|
|Compatibilidad con objetos OLE|DBPROP_OLEOBJECTS|
|Compatibilidad con abrir conjuntos de filas|DBPROP_OPENROWSETSUPPORT|
|ORDENAR por columnas en la lista de selección|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilidad del parámetro de salida|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Descriptores de acceso pass by Ref|DBPROP_BYREFACCESSORS|
|Contraseña|DBPROP_AUTH_PASSWORD|
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
 Las propiedades siguientes se agregan a la colección **Properties** del objeto **Recordset** .

|Nombre de la propiedad ADO|OLE DB nombre de propiedad|
|-----------------------|--------------------------|
|Orden de acceso|DBPROP_ACCESSORDER|
|Bloquear objetos de almacenamiento|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo de marcador|DBPROP_BOOKMARKTYPE|
|Marcador|DBPROP_IROWSETLOCATE|
|Cambiar filas insertadas|DBPROP_CHANGEINSERTEDROWS|
|Privilegios de columna|DBPROP_COLUMNRESTRICT|
|Notificación de conjunto de columnas|DBPROP_NOTIFYCOLUMNSET|
|Tiempo de espera del comando|DBPROP_COMMANDTIMEOUT|
|Diferir columna|DBPROP_DEFERRED|
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
|IRowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
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
|Cambios visibles en otros usuarios|DBPROP_OTHERUPDATEDELETE|
|Inserciones visibles de otros usuarios|DBPROP_OTHERINSERT|
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
|Cursor de servidor|DBPROP_SERVERCURSOR|
|Omitir marcadores eliminados|DBPROP_BOOKMARKSKIPPED|
|Identidad de fila segura|DBPROP_STRONGITDENTITY|
|Filas únicas|DBPROP_UNIQUEROWS|
|Actualización|DBPROP_UPDATABILITY|
|Usar marcadores|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propiedades dinámicas de comandos
 Las propiedades siguientes se agregan a la colección **Properties** del objeto **Command** .

|Nombre de la propiedad ADO|OLE DB nombre de propiedad|
|-----------------------|--------------------------|
|Orden de acceso|DBPROP_ACCESSORDER|
|Ruta de base|SSPROP_STREAM_BASEPATH|
|Bloquear objetos de almacenamiento|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo de marcador|DBPROP_BOOKMARKTYPE|
|Marcador|DBPROP_IROWSETLOCATE|
|Cambiar filas insertadas|DBPROP_CHANGEINSERTEDROWS|
|Privilegios de columna|DBPROP_COLUMNRESTRICT|
|Notificación de conjunto de columnas|DBPROP_NOTIFYCOLUMNSET|
|Tipo de contenido|SSPROP_STREAM_CONTENTTYPE|
|Captura automática del cursor|SSPROP_CURSORAUTOFETCH|
|Diferir columna|DBPROP_DEFERRED|
|Preparación diferida|SSPROP_DEFERPREPARE|
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
|IRowsetResynch|DBPROP_IRowsetResynch|
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Marcadores literales|DBPROP_LITERALBOOKMARKS|
|Identidad de fila literal|DBPROP_LITERALIDENTITY|
|Modo de bloqueo|DBPROP_LOCKMODE|
|Número máximo de filas abiertas|DBPROP_MAXOPENROWS|
|Número máximo de filas pendientes|DBPROP_MAXPENDINGROWS|
|Número máximo de filas|DBPROP_MAXROWS|
|Granularidad de notificaciones|DBPROP_NOTIFICATIONGRANULARITY|
|Fases de notificación|DBPROP_NOTIFICATIONPHASES|
|Objetos con transacciones|DBPROP_TRANSACTEDOBJECT|
|Cambios visibles en otros usuarios|DBPROP_OTHERUPDATEDELETE|
|Inserciones visibles de otros usuarios|DBPROP_OTHERINSERT|
|Propiedad de codificación de salida|DBPROP_OUTPUTENCODING|
|Propiedad de flujo de salida|DBPROP_OUTPUTSTREAM|
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
|Cursor de servidor|DBPROP_SERVERCURSOR|
|Datos de servidor al insertar|DBPROP_SERVERDATAONINSERT|
|Omitir marcadores eliminados|DBPROP_BOOKMARKSKIP|
|Identidad de fila segura|DBPROP_STRONGIDENTITY|
|Actualización|DBPROP_UPDATABILITY|
|Usar marcadores|DBPROP_BOOKMARKS|
|Raíz XML|SSPROP_STREAM_XMLROOT|
|XSL|SSPROP_STREAM_XSL|

 Para obtener detalles de implementación específicos e información funcional sobre el proveedor de OLE DB de Microsoft SQL Server, vea el [proveedor de SQL Server](/previous-versions/windows/desktop/ms720897(v=vs.85)).

## <a name="see-also"></a>Consulte también
 [Propiedad ConnectionString (ADO)](../../reference/ado-api/connectionstring-property-ado.md) [Provider (propiedad](../../reference/ado-api/provider-property-ado.md) , ADO) [RECORDSET (objeto) (ADO)](../../reference/ado-api/recordset-object-ado.md)