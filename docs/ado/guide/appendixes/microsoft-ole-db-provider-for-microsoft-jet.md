---
description: Introducción al proveedor de Microsoft OLE DB para Microsoft Jet
title: Proveedor de Microsoft OLE DB para Microsoft Jet | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Jet provider for OLE DB [ADO]
- providers [ADO], OLE DB provider for Microsoft Jet
- OLE DB provider for Microsoft Jet [ADO]
ms.assetid: fd956da1-5203-40af-aa7e-fc13a6c6581f
author: rothja
ms.author: jroth
ms.openlocfilehash: 87d9e159a9a97a0a929bec72d03118a927aad56e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029229"
---
# <a name="microsoft-ole-db-provider-for-microsoft-jet-overview"></a>Introducción al proveedor de Microsoft OLE DB para Microsoft Jet
El proveedor de OLE DB para Microsoft Jet permite a ADO obtener acceso a las bases de datos de Microsoft Jet.

## <a name="connection-string-parameters"></a>Parámetros de la cadena de conexión
 Para conectarse a este proveedor, establezca el argumento de *proveedor* de la propiedad [ConnectionString](../../reference/ado-api/connectionstring-property-ado.md) en la siguiente propiedad:

```vb
Microsoft.Jet.OLEDB.4.0
```

 La lectura de la propiedad [Provider](../../reference/ado-api/provider-property-ado.md) también devolverá esta cadena.

## <a name="typical-connection-string"></a>Cadena de conexión típica
 Una cadena de conexión típica para este proveedor es:

```vb
"Provider=Microsoft.Jet.OLEDB.4.0;Data Source=databaseName;User ID=MyUserID;Password=MyPassword;"
```

 La cadena consta de estas palabras clave:

|Palabra clave|Descripción|
|-------------|-----------------|
|**Proveedor**|Especifica el proveedor de OLE DB para Microsoft Jet.|
|**Data Source** (Origen de datos)|Especifica la ruta de acceso de la base de datos y el nombre de archivo (por ejemplo, `c:\Northwind.mdb` ).|
|**Id. de usuario**|Especifica el nombre de usuario. Si no se especifica esta palabra clave, `admin` se utiliza de forma predeterminada la cadena "".|
|**Contraseña**|Especifica la contraseña del usuario. Si no se especifica esta palabra clave, se usa de forma predeterminada la cadena vacía ("").|

> [!NOTE]
>  Si se va a conectar a un proveedor de origen de datos que admite la autenticación de Windows, debe especificar **Trusted_Connection = Yes** o **Integrated Security = SSPI** en lugar de la información de identificador de usuario y contraseña en la cadena de conexión.

## <a name="provider-specific-connection-parameters"></a>Provider-Specific de los parámetros de conexión
 El proveedor de OLE DB para Microsoft Jet admite varias propiedades dinámicas específicas del proveedor además de las definidas por ADO. Como con todos los demás parámetros de **conexión** , se pueden establecer mediante la colección **Properties** del objeto **Connection** o como parte de la cadena de conexión.

 En la tabla siguiente se enumeran estas propiedades junto con el nombre de propiedad OLE DB correspondiente entre paréntesis.

|Parámetro|Descripción|
|---------------|-----------------|
|Jet OLEDB: Compact cantidad de espacio recuperado (DBPROP_JETOLEDB_COMPACTFREESPACESIZE)|Indica una estimación de la cantidad de espacio, en bytes, que se puede recuperar mediante la compactación de la base de datos. Este valor solo es válido después de establecer una conexión de base de datos.|
|Jet OLEDB: control de conexión (DBPROP_JETOLEDB_CONNECTIONCONTROL)|Indica si los usuarios pueden conectarse a la base de datos.|
|Jet OLEDB: crear base de datos del sistema (DBPROP_JETOLEDB_CREATESYSTEMDATABASE)|Indica si se debe crear una base de datos del sistema al crear un nuevo origen de datos.|
|Jet OLEDB: modo de bloqueo de base de datos (DBPROP_JETOLEDB_DATABASELOCKMODE)|Indica el modo de bloqueo de esta base de datos. El primer usuario que abra la base de datos determina el modo usado mientras la base de datos está abierta.|
|Jet OLEDB: contraseña de base de datos (DBPROP_JETOLEDB_DATABASEPASSWORD)|Indica la contraseña de la base de datos.|
|Jet OLEDB: no copiar configuración regional en Compact (DBPROP_JETOLEDB_COMPACT_DONTCOPYLOCALE)|Indica si jet debe copiar información de configuración regional cuando se compacta una base de datos.|
|Jet OLEDB: cifrar base de datos (DBPROP_JETOLEDB_ENCRYPTDATABASE)|Indica si se debe cifrar una base de datos compactada. Si no se establece esta propiedad, la base de datos compactada se cifrará si la base de datos original también se cifró.|
|Jet OLEDB: tipo de motor (DBPROP_JETOLEDB_ENGINE)|Indica el motor de almacenamiento usado para obtener acceso al almacén de datos actual.|
|Jet OLEDB: retraso asincrónico exclusivo (DBPROP_JETOLEDB_EXCLUSIVEASYNCDELAY)|Indica el período de tiempo máximo, en milisegundos, que jet puede retrasar las escrituras asincrónicas en el disco cuando la base de datos se abre exclusivamente.<br /><br /> Esta propiedad se omite a menos que el tiempo de espera de la **transacción jet OleDb: Flush** se establezca en 0.|
|Jet OLEDB: tiempo de espera de transacciones de vaciado (DBPROP_JETOLEDB_FLUSHTRANSACTIONTIMEOUT)|Indica la cantidad de tiempo que hay que esperar antes de que los datos almacenados en una memoria caché para escritura asincrónica se escriban en el disco. Esta configuración invalida los valores de **jet OleDb: retardo asincrónico compartido** y **jet OleDb: Exclusive Async Delay**.|
|Jet OLEDB: transacciones masivas globales (DBPROP_JETOLEDB_GLOBALBULKNOTRANSACTIONS)|Indica si se transaccionan las transacciones masivas de SQL.|
|Jet OLEDB: operaciones masivas parciales globales (DBPROP_JETOLEDB_GLOBALBULKPARTIAL)|Indica la contraseña utilizada para abrir la base de datos.|
|Jet OLEDB: sincronización de confirmación implícita (DBPROP_JETOLEDB_IMPLICITCOMMITSYNC)|Indica si los cambios realizados en transacciones implícitas internas se escriben en modo sincrónico o asincrónico.|
|Jet OLEDB: retraso de bloqueos (DBPROP_JETOLEDB_LOCKDELAY)|Indica el número de milisegundos que hay que esperar antes de intentar adquirir un bloqueo después de que se haya producido un error en un intento anterior.|
|Jet OLEDB: reintento de bloqueo (DBPROP_JETOLEDB_LOCKRETRY)|Indica cuántas veces se repite un intento de obtener acceso a una página bloqueada.|
|Jet OLEDB: tamaño máximo de búfer (DBPROP_JETOLEDB_MAXBUFFERSIZE)|Indica la cantidad máxima de memoria, en kilobytes, que jet puede usar antes de iniciar el vaciado de los cambios en el disco.|
|Jet OLEDB: máximo de bloqueos por archivo (DBPROP_JETOLEDB_MAXLOCKSPERFILE)|Indica el número máximo de bloqueos que jet puede colocar en una base de datos. El valor predeterminado es 9500.|
|Jet OLEDB: nueva contraseña de base de datos (DBPROP_JETOLEDB_NEWDATABASEPASSWORD)|Indica la nueva contraseña que se va a establecer para esta base de datos. La contraseña anterior se almacena en **jet OleDb: Database Password**.|
|Jet OLEDB: tiempo de espera de comando ODBC (DBPROP_JETOLEDB_ODBCCOMMANDTIMEOUT)|Indica el número de milisegundos antes de que se agote el tiempo de espera de una consulta ODBC remota de jet.|
|Jet OLEDB: bloqueos de página en el bloqueo de tabla (DBPROP_JETOLEDB_PAGELOCKSTOTABLELOCK)|Indica el número de páginas que deben bloquearse dentro de una transacción antes de que jet intente promover el bloqueo a un bloqueo de tabla. Si este valor es 0, el bloqueo nunca se promueve.|
|Jet OLEDB: tiempo de espera de página (DBPROP_JETOLEDB_PAGETIMEOUT)|Indica el número de milisegundos que jet esperará antes de comprobar si su memoria caché no está actualizada con el archivo de base de datos.|
|Jet OLEDB: recicle Long-Valued páginas (DBPROP_JETOLEDB_RECYCLELONGVALUEPAGES)|Indica si jet debe intentar reclamar dinámicamente las páginas BLOB cuando se liberan.|
|Jet OLEDB: ruta de acceso del registro (DBPROP_JETOLEDB_REGPATH)|Indica la clave del registro de Windows que contiene valores para el motor de base de datos Jet.|
|Jet OLEDB: RESET ISAM stats (DBPROP_JETOLEDB_RESETISAMSTATS)|Indica si el **conjunto de registros** de esquema DBSCHEMA_JETOLEDB_ISAMSTATS debe restablecer sus contadores de rendimiento después de que devuelva información de rendimiento.|
|Jet OLEDB: retraso de Async compartido (DBPROP_JETOLEDB_SHAREDASYNCDELAY)|Indica la cantidad máxima de tiempo, en milisegundos, que jet puede retrasar las escrituras asincrónicas en el disco cuando la base de datos se abre en modo multiusuario.|
|Jet OLEDB: base de datos del sistema (DBPROP_JETOLEDB_SYSDBPATH)|Indica la ruta de acceso y el nombre de archivo del archivo de información de grupo de trabajo (base de datos del sistema).|
|Jet OLEDB: modo de confirmación de transacción (DBPROP_JETOLEDB_TXNCOMMITMODE)|Indica si jet escribe datos en el disco de forma sincrónica o asincrónica cuando se confirma una transacción.|
|Jet OLEDB: sincronización de confirmación de usuario (DBPROP_JETOLEDB_USERCOMMITSYNC)|Indica si los cambios realizados en las transacciones se escriben en modo sincrónico o asincrónico.|

## <a name="provider-specific-recordset-and-command-properties"></a>Provider-Specific conjunto de registros y propiedades de comando
 El proveedor Jet también admite varias propiedades de **conjunto de registros** y **comandos** específicas del proveedor. Se tiene acceso a estas propiedades y se establecen a través de la colección **Properties** del objeto de **conjunto de registros** o del **comando** . En la tabla se muestra el nombre de la propiedad ADO y su nombre de propiedad OLE DB correspondiente entre paréntesis.

|Nombre de la propiedad|Descripción|
|-------------------|-----------------|
|Jet OLEDB: transacciones masivas (DBPROP_JETOLEDB_BULKNOTRANSACTIONS)|Indica si las operaciones masivas de SQL son transacciones. Es posible que se produzcan errores en operaciones masivas de gran tamaño al realizar transacciones debido a retrasos de recursos.|
|Jet OLEDB: habilitar cursores FAT (DBPROP_JETOLEDB_ENABLEFATCURSOR)|Indica si jet debe almacenar en caché varias filas al rellenar un conjunto de registros para los orígenes de filas remotos.|
|Jet OLEDB: tamaño de la caché de cursores FAT (DBPROP_JETOLEDB_FATCURSORMAXROWS)|Indica el número de filas que se van a almacenar en caché al usar el almacenamiento en caché de filas del almacén de datos remoto. Este valor se omite a menos que **jet OleDb: enable FAT cursor** sea true.|
|Jet OLEDB: incoherente (DBPROP_JETOLEDB_INCONSISTENT)|Indica si los resultados de la consulta permiten actualizaciones incoherentes.|
|Jet OLEDB: granularidad de bloqueo (DBPROP_JETOLEDB_LOCKGRANULARITY)|Indica si se abre una tabla mediante el bloqueo de fila.|
|Jet OLEDB: instrucción Pass-Through de ODBC (DBPROP_JETOLEDB_ODBCPASSTHROUGH)|Indica que jet debe pasar el texto SQL de un objeto de **comando** al back-end sin modificar.|
|Jet OLEDB: operaciones masivas parciales (DBPROP_JETOLEDB_BULKPARTIAL)|Indica el comportamiento de jet cuando se produce un error en las operaciones DML SQL.|
|Jet OLEDB: Bulk-Op de consultas de paso a través (DBPROP_JETOLEDB_PASSTHROUGHBULKOP)|Indica si las consultas que no devuelven un **conjunto de registros** se pasan sin cambios al origen de datos.|
|Jet OLEDB: paso a través de la cadena de conexión de consulta (DBPROP_JETOLEDB_ODBCPASSTHROUGHCONNECTSTRING)|Indica la cadena de conexión de jet utilizada para conectarse a un almacén de datos remoto. Este valor se omite a menos que la **instrucción jet OleDb: ODBC Pass-Through** sea true.|
|Jet OLEDB: consulta almacenada (DBPROP_JETOLEDB_STOREDQUERY)|Indica si el texto del comando debe interpretarse como una consulta almacenada en lugar de como un comando SQL.|
|Jet OLEDB: Validate rules on set (DBPROP_JETOLEDB_VALIDATEONSET)|Indica si las reglas de validación de jet se evalúan cuando se establecen datos de columna o cuando los cambios se confirman en la base de datos.|

 De forma predeterminada, el proveedor de OLE DB para Microsoft Jet abre las bases de datos de Microsoft Jet en modo de lectura y escritura. Para abrir una base de datos en modo de solo lectura, establezca la propiedad [mode](../../reference/ado-api/mode-property-ado.md) en el objeto de **conexión** ADO en **adModeRead**.

## <a name="command-object-usage"></a>Uso de objetos de comando
 El texto del comando en el objeto [Command](../../reference/ado-api/command-object-ado.md) usa el dialecto SQL de Microsoft Jet. Puede especificar consultas que devuelven filas, consultas de acción y nombres de tabla en el texto del comando. sin embargo, no se admiten los procedimientos almacenados y no se deben especificar.

## <a name="recordset-behavior"></a>Comportamiento del conjunto de registros
 El motor de base de datos de Microsoft Jet no es compatible con cursores dinámicos. Por lo tanto, el proveedor de OLE DB para Microsoft Jet no admite el tipo de cursor **adLockDynamic** . Cuando se solicita un cursor dinámico, el proveedor devolverá un cursor de conjunto de claves y restablecerá la propiedad [CursorType](../../reference/ado-api/cursortype-property-ado.md) para indicar el tipo de [conjunto de registros](../../reference/ado-api/recordset-object-ado.md) que se devuelve. Además, si se solicita un **conjunto de registros** actualizable (**LockType** es **adLockOptimistic**, **adLockBatchOptimistic** o **adLockPessimistic**), el proveedor también devolverá un cursor de conjunto de claves y restablecerá la propiedad **CursorType** .

## <a name="dynamic-properties"></a>Propiedades dinámicas
 El proveedor de OLE DB para Microsoft Jet inserta varias propiedades dinámicas en la colección de **propiedades** de los objetos de [conexión](../../reference/ado-api/connection-object-ado.md), [conjunto de registros](../../reference/ado-api/recordset-object-ado.md)y [comando](../../reference/ado-api/command-object-ado.md) no abiertos.

 Las tablas siguientes son un índice entre los nombres de ADO y OLE DB para cada propiedad dinámica. La referencia del programador de OLE DB hace referencia a un nombre de propiedad ADO por el término "Descripción". Puede encontrar más información sobre estas propiedades en la referencia del programador de OLE DB.

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
|Catálogo actual|DBPROP_CURRENTCATALOG|
|Origen de datos|DBPROP_INIT_DATASOURCE|
|Data Source Name|DBPROP_DATASOURCENAME|
|Modelo de subprocesos de objetos de origen de datos|DBPROP_DSOTHREADMODEL|
|Nombre DBMS|DBPROP_DBMSNAME|
|Versión DBMS|DBPROP_DBMSVER|
|AGRUPAr por compatibilidad|DBPROP_GROUPBY|
|Compatibilidad con tablas heterogéneas|DBPROP_HETEROGENEOUSTABLES|
|Distinción de mayúsculas y minúsculas de identificadores|DBPROP_IDENTIFIERCASE|
|Niveles de aislamiento|DBPROP_SUPPORTEDTXNISOLEVELS|
|Retención de aislamiento|DBPROP_SUPPORTEDTXNISORETAIN|
|Identificador de configuración regional|DBPROP_INIT_LCID|
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
|Versión de OLE DB|DBPROP_PROVIDEROLEDBVER|
|Compatibilidad con objetos OLE|DBPROP_OLEOBJECTS|
|Compatibilidad con abrir conjuntos de filas|DBPROP_OPENROWSETSUPPORT|
|ORDENAR por columnas en la lista de selección|DBPROP_ORDERBYCOLUMNSINSELECT|
|Disponibilidad del parámetro de salida|DBPROP_OUTPUTPARAMETERAVAILABILITY|
|Descriptores de acceso pass by Ref|DBPROP_BYREFACCESSORS|
|Contraseña|DBPROP_AUTH_PASSWORD|
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
|Conjunto de filas Append-Only|DBPROP_APPENDONLY|
|Bloquear objetos de almacenamiento|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo de marcador|DBPROP_BOOKMARKTYPE|
|Marcador|DBPROP_IROWSETLOCATE|
|Marcadores ordenados|DBPROP_ORDEREDBOOKMARKS|
|Columnas diferidas de caché|DBPROP_CACHEDEFERRED|
|Cambiar filas insertadas|DBPROP_CHANGEINSERTEDROWS|
|Privilegios de columna|DBPROP_COLUMNRESTRICT|
|Notificación de conjunto de columnas|DBPROP_NOTIFYCOLUMNSET|
|Columna grabable|DBPROP_MAYWRITECOLUMN|
|Diferir columna|DBPROP_DEFERRED|
|Retraso de las actualizaciones de objetos de almacenamiento|DBPROP_DELAYSTORAGEOBJECTS|
|Capturar hacia atrás|DBPROP_CANFETCHBACKWARDS|
|Almacenar filas|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|Filas immóviles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsestLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
|ISupportErrorInfo|DBPROP_ISupportErrorInfo|
|Marcadores literales|DBPROP_LITERALBOOKMARKS|
|Identidad de fila literal|DBPROP_LITERALIDENTITY|
|Número máximo de filas abiertas|DBPROP_MAXOPENROWS|
|Número máximo de filas pendientes|DBPROP_MAXPENDINGROWS|
|Número máximo de filas|DBPROP_MAXROWS|
|Uso de la memoria|DBPROP_MEMORYUSAGE|
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
|Omitir marcadores eliminados|DBPROP_BOOKMARKSKIPPED|
|Identidad de fila segura|DBPROP_STRONGITDENTITY|
|Actualización|DBPROP_UPDATABILITY|
|Usar marcadores|DBPROP_BOOKMARKS|

## <a name="command-dynamic-properties"></a>Propiedades dinámicas de comandos
 Las propiedades siguientes se agregan a la colección **Properties** del objeto **Command** .

|Nombre de la propiedad ADO|OLE DB nombre de propiedad|
|-----------------------|--------------------------|
|Orden de acceso|DBPROP_ACCESSORDER|
|Conjunto de filas Append-Only|DBPROP_APPENDONLY|
|Bloquear objetos de almacenamiento|DBPROP_BLOCKINGSTORAGEOBJECTS|
|Tipo de marcador|DBPROP_BOOKMARKTYPE|
|Marcador|DBPROP_IROWSETLOCATE|
|Cambiar filas insertadas|DBPROP_CHANGEINSERTEDROWS|
|Privilegios de columna|DBPROP_COLUMNRESTRICT|
|Notificación de conjunto de columnas|DBPROP_NOTIFYCOLUMNSET|
|Diferir columna|DBPROP_DEFERRED|
|Retraso de las actualizaciones de objetos de almacenamiento|DBPROP_DELAYSTORAGEOBJECTS|
|Capturar hacia atrás|DBPROP_CANFETCHBACKWARDS|
|Almacenar filas|DBPROP_CANHOLDROWS|
|IAccessor|DBPROP_IAccessor|
|IColumnsInfo|DBPROP_IColumnsInfo|
|IColumnsRowset|DBPROP_IColumnsRowset|
|IConnectionPointContainer|DBPROP_IConnectionPointContainer|
|IConvertType|DBPROP_IConvertType|
|ILockBytes|DBPROP_ILockBytes|
|Filas immóviles|DBPROP_IMMOBILEROWS|
|IRowset|DBPROP_IRowset|
|IRowsetChange|DBPROP_IRowsetChange|
|IRowsetIdentity|DBPROP_IRowsetIdentity|
|IRowsetIndex|DBPROP_IRowsetIndex|
|IRowsetInfo|DBPROP_IRowsetInfo|
|IRowsetLocate|DBPROP_IRowsetLocate|
|IRowsetResynch||
|IRowsetScroll|DBPROP_IRowsetScroll|
|IRowsetUpdate|DBPROP_IRowsetUpdate|
|ISequentialStream|DBPROP_ISequentialStream|
|IStorage|DBPROP_IStorage|
|IStream|DBPROP_IStream|
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
|Datos de servidor al insertar|DBPROP_SERVERDATAONINSERT|
|Omitir marcadores eliminados|DBPROP_BOOKMARKSKIP|
|Identidad de fila segura|DBPROP_STRONGIDENTITY|
|Actualización|DBPROP_UPDATABILITY|
|Usar marcadores|DBPROP_BOOKMARKS|

 Para obtener detalles de implementación específicos e información funcional sobre el proveedor de OLE DB para Microsoft Jet, consulte [proveedor de jet](/previous-versions/windows/desktop/ms722791(v=vs.85)) en la documentación de OLE DB.
