---
title: Errores de PolyBase y posibles soluciones
description: Referencia de PolyBase para errores y soluciones sugeridas.
ms.date: 02/17/2021
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
dev_langs:
- TSQL
- XML
f1_keywords:
- PolyBase, monitoring
- PolyBase, performance monitoring
helpviewer_keywords:
- PolyBase, troubleshooting
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: ''
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-2016'
ms.openlocfilehash: 463b54aefd36e74318331c90cf2c944734f8a5cc
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873337"
---
# <a name="polybase-errors-and-possible-solutions"></a>Errores de PolyBase y posibles soluciones

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Este artículo contiene escenarios de error comunes y soluciones para PolyBase.

Para más información sobre la supervisión y la solución de problemas de PolyBase, consulte [Supervisión y solución de problemas de PolyBase](polybase-troubleshooting.md).

Para ver las ubicaciones comunes del archivo de registro de PolyBase en Windows y Linux, consulte [Supervisión y solución de problemas de PolyBase](polybase-troubleshooting.md#log-file-locations).


## <a name="error-messages-and-possible-solutions"></a>Mensajes de error y posibles soluciones


### <a name="service-account-change"></a>Cambio de la cuenta de servicio

Ejemplo de mensaje de error:

> 107035;Error de autorización de DMS porque [DOMINIO\usuario] no es miembro del grupo [PdwDataMovementAccess] <BR>
> 107017;Encabezado de control de DMS no válido:

Este error se debe probablemente a la modificación de la cuenta de servicio de PolyBase. Para cambiar las cuentas de servicio de los servicios Motor de PolyBase y Movimiento de datos de PolyBase, desinstale y vuelva a instalar la característica PolyBase.


### <a name="data-movement-service-permissions-errors"></a>Errores de permisos del servicio de movimiento de datos

Para más información sobre cómo solucionar problemas y resolver problemas de permisos con el servicio de movimiento de datos, consulte el artículo sobre [permisos de la cuenta de servicio de PolyBase y errores comunes observados en su ausencia](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711).


### <a name="windows-authentication-failure"></a>Error de autenticación de Windows

Para más información sobre cómo solucionar problemas y resolver problemas de permisos relacionados con un error en la autenticación de Windows, consulte el artículo sobre [permisos de la cuenta de servicio de PolyBase y errores comunes observados en su ausencia](https://techcommunity.microsoft.com/t5/sql-server-support/polybase-service-account-permissions-and-common-errors-observed/ba-p/2112711).


### <a name="cannot-execute-the-query-remote-query"></a>No se puede ejecutar la consulta "Consulta remota" 

Ejemplo de mensaje de error:

> Mensaje 7320, nivel 16, estado 110, línea 14<BR>
> No se puede ejecutar la consulta "Consulta remota" en el proveedor OLE DB "SQLNCLI11" para el servidor vinculado "(NULL)". Consulta anulada: se alcanzó el umbral de rechazo máximo (0 filas) al leer desde un origen externo: 1 fila rechazadas de un total de 1 fila procesada.
(/nation/sensors.ldjson.txt)Índice de columna: 0, tipo de datos esperado: INT, valor incorrecto: {"id":"S2740036465E2B","time":"2016-02-26T16:59:02.9300000Z","temp":23.3,"hum":0.77,"wind":17,"press":1032,"loc":[-76.90914996169623,38.8929314364726]} (error de conversión de columnas), Error: error al convertir el tipo de datos NVARCHAR a INT.

Tenga en cuenta que puede haber derivaciones de este error. El nombre del primer archivo rechazado se muestra en SQL Server Management Studio (SSMS) con valores o tipos de datos incorrectos.

**Posible motivo:**  
el motivo por el que se produce este error es que cada archivo tiene un esquema diferente. Cuando el DDL de tabla externa de PolyBase señala a un directorio, lee de forma recursiva todos los archivos de ese directorio. Cuando se produce un error de coincidencia en un tipo de datos o columna, este error podría aparecer en SSMS.

**Posible solución:**  
si los datos de cada tabla se componen de un archivo, use el nombre de archivo en la sección UBICACIÓN que tiene como prefijo el directorio de los archivos externos. Si hay varios archivos por tabla, coloque cada conjunto de archivos en directorios diferentes en Azure Blob Storage. Haga que UBICACIÓN apunte al directorio en lugar de a un archivo determinado. Esta solución está recomendada.

**Ejemplo**:  

```sqlsyntax
Create External Table foo
(col1 int)WITH (LOCATION='/bar/foobar.txt',DATA_SOURCE…); OR
Create External Table foo
(col1 int) WITH (LOCATION = '/bar/', DATA_SOURCE…);
```


### <a name="kerberos-support"></a>Compatibilidad con Kerberos

SQL Server está configurado para tener acceso a un clúster de Hadoop compatible. No se aplica la seguridad de Kerberos en el clúster de Hadoop.

Al seleccionar en la tabla externa se devuelve el siguiente error:

> Mensaje 105019, nivel 16, estado 1, línea 55<BR>
> No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_Connect: Error [No se puede crear una instancia de LoginClass] al obtener acceso al archivo externo.'<BR>
> Mensaje 7320, nivel 16, estado 110, línea 55<BR>
> No se puede ejecutar la consulta "Consulta remota" en el proveedor OLE DB "SQLNCLI11" para el servidor vinculado "(NULL)". No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_Connect: Error [No se puede crear una instancia de LoginClass] al obtener acceso al archivo externo.'

La interrogación del registro del servidor de DWEngine muestra el siguiente error:

> [Thread:16432] [EngineInstrumentation:EngineQueryErrorEvent] (Error, High):<BR>
> No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_Connect: Error [com.microsoft.polybase.client.KerberosSecureLogin] al obtener acceso al archivo externo.'
Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: no se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_Connect: Error [com.microsoft.polybase.client.KerberosSecureLogin] al obtener acceso al archivo externo.' ---> Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsAccessException: excepción de Java generada en la llamada a HdfsBridge_Connect: Error [com.microsoft.polybase.client.KerberosSecureLogin] al obtener acceso al archivo externo.

**Posible motivo:**  
Kerberos no está habilitado en el clúster de Hadoop, pero la seguridad de Kerberos está habilitada en core-site.xml, yarn-site.xml o el archivo hdfs-site.xml ubicado de forma predeterminada en Archivos de programa\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf. En Linux, los archivos se encuentran de forma predeterminada en /var/opt/mssql/binn/polybase/hadoop/conf/.

**Posible solución:**  
Convierta en comentario la información de seguridad de Kerberos de los archivos mencionados anteriormente.

Para obtener más información sobre la solución de problemas de PolyBase y Kerberos, consulte [Solución de problemas de conectividad de Kerberos con PolyBase](polybase-troubleshoot-connectivity.md).

### <a name="internal-query-processor-error"></a>Error interno del procesador de consultas:

Al consultar una tabla externa se devuelve el siguiente error:

> Mensaje 8680, nivel 17, estado 5, línea 118<BR>
> Error interno del procesador de consultas: error inesperado al procesar una fase de consulta remota.

El registro del servidor de DWEngine contiene los siguientes mensajes:

> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Error, High): ***** El sistema DMS tiene nodos desconectados:<BR>
> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Error, High): ***** El sistema DMS tiene nodos desconectados:<BR>
> [Thread:5216] [ControlNodeMessenger:ErrorEvent] (Error, High): ***** El sistema DMS tiene nodos desconectados:<BR>

**Posible motivo:**  
el motivo de este error podría ser que SQL Server no se reinició después de configurar PolyBase.

**Posible solución:**  
Reinicie SQL Server. Compruebe el registro del servidor de DWEngine para confirmar que no hay ninguna desconexión de DMS después del reinicio.


### <a name="user-needed-for-hdfs-access"></a>Usuario necesario para el acceso de HDFS

**Escenario:**  
SQL Server está conectado a un clúster de Hadoop no seguro (Kerberos no está habilitado). PolyBase está configurado para insertar cálculo al clúster de Hadoop.

**Consulta de ejemplo:**  

```sql
select count(*) from foo WITH (FORCE EXTERNALPUSHDOWN);
```

se devuelve un mensaje de error similar al siguiente: 

> Mensaje 105019, nivel 16, estado 1, línea 1<BR>
> No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a JobSubmitter_PollJobStatus: Error [java.net.ConnectException: error de llamada desde big1506sql2016/172.16.1.4 to 0.0.0.0:10020 en la excepción de conexión: java.net ConnectException: conexión rechazada: no hay más información; para obtener información, consulte:  http://wiki.apache.org/hadoop/ConnectionRefused ] al obtener acceso al archivo externo.'<BR>
> El proveedor OLE DB "SQLNCLI11" para el servidor vinculado "(NULL)" devolvió el mensaje "Error no especificado".<BR>
> Mensaje 7421, nivel 16, estado 2, línea 1<BR>
> No se puede capturar el conjunto de filas del proveedor OLE DB "SQLNCLI11" para el servidor vinculado "(NULL)". .<BR>

Hadoop Yarn Log Error:
> Error en la configuración de trabajo: org.apache.hadoop.security.AccessControlException: permiso denegado: user=pdw_user, access=WRITE, inode="/user":hdfs:hdfs:drwxr-xr-x at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkFsPermission(FSPermissionChecker.java:265) at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:251) at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:232) org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:176) at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkPermission(FSNamesystem.java:5525)

**Posible motivo:**  
con Kerberos deshabilitado, PolyBase usará pdw_user como usuario para tener acceso a HDFS y enviar trabajos de MapReduce.

**Posible solución:**  
cree pdw_user en Hadoop y asígnele los permisos necesarios para los directorios usados durante el procesamiento de MapReduce. Asegúrese también de que pdw_user es el propietario del directorio de HDFS /user/pdw_user.

A continuación se muestra un ejemplo de cómo crear un directorio principal y asignar permisos para pdw_user:

```console
sudo -u hdfs hadoop fs -mkdir /user/pdw_user
sudo -u hdfs hadoop fs -chown pdw_user /user/pdw_user
```

Después, asegúrese de que pdw_user tiene permisos de lectura, escritura y ejecución en el directorio /user/pdw_user. Asegúrese de que el directorio /tmp tiene 777 permisos.

Para obtener más información sobre la solución de problemas de PolyBase y Kerberos, consulte [Solución de problemas de conectividad de Kerberos con PolyBase](polybase-troubleshoot-connectivity.md).

### <a name="java-memory-error-due-to-utf-8"></a>Error de memoria de Java debido a UTF-8

**Escenario:**  
PolyBase de SQL Server se configura con un clúster de Hadoop o Azure Blob Storage. Se produce el siguiente error en la consulta de selección:

> Mensaje 106000, nivel 16, estado 1, línea 1<BR>
> Espacio de montón de Java<BR>

**Posible motivo:**  
una entrada no válida puede provocar el error de memoria insuficiente de Java. Puede que el archivo no esté en formato UTF-8. DMS intenta leer el archivo completo como una fila, ya que no puede descodificar el delimitador de filas y se ejecuta en un error de espacio en el montón de Java.

**Posible solución:**  
convierta el archivo al formato UTF-8, ya que PolyBase requiere actualmente el formato UTF-8 para los archivos delimitados por texto.

### <a name="hadoop-connectivity-configuration"></a>Configuración de conectividad de Hadoop

La configuración de PolyBase de SQL Server para conectarse a Azure Blob Storage devuelve el siguiente mensaje de error en SQL Server:

> Mensaje 105019, nivel 16, estado 1, línea 74<BR>
> No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_Connect: Error [Ningún sistema de archivos para el esquema: wasbs] al obtener acceso al archivo externo.'<BR>

**Posible motivo:**  
la conectividad de Hadoop no se establece en el valor de configuración para acceder a Azure Blob Storage.

**Posible solución:**  
establezca la conectividad de Hadoop en un valor (preferiblemente 7) que admita Azure Blob Storage y reinicie SQL Server. Para obtener una lista de valores de conectividad y tipos admitidos, consulte [Configuración de conectividad de PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#arguments).


### <a name="create-table-as-select-error"></a>Error CREATE TABLE AS SELECT

**Escenario:**  
al intentar exportar datos al sistema de archivos de Hadoop y Azure Blob Storage mediante PolyBase con la sintaxis CREATE EXTERNAL TABLE AS SELECT (CETAS) de SQL Server se produce un error con el mensaje de error siguiente:

> Mensaje 156, nivel 15, estado 1, línea 177<BR>
> Sintaxis incorrecta junto a la palabra clave 'WITH'.<BR>
> Mensaje 319, nivel 15, estado 1, línea 177<BR>
> Sintaxis incorrecta junto a la palabra clave 'with'. Si esta instrucción es una expresión de tabla común, una cláusula xmlnamespaces o una cláusula de contexto de seguimiento de cambios, la instrucción anterior debe terminarse con punto y coma.<BR>

**Posible motivo:**  
Al exportar datos a Hadoop o Azure Blob Storage mediante PolyBase, solo se exportan los datos, y no los nombres de columna (metadatos), tal y como se define en el comando CREATE EXTERNAL TABLE.

**Posible solución:**  
cree primero la tabla externa y, a continuación, utilice INSERT INTO SELECT para exportar a la ubicación externa. Para obtener un ejemplo de código, consulte [Escenarios de consulta de PolyBase](polybase-queries.md#export-data).


### <a name="create-external-table-from-azure-blob-storage-fails"></a>Se produce un error al crear una tabla externa desde Azure Blob Storage

**Escenario:**  
SQL DW está configurado para importar datos desde Azure Blob Storage. Al crear una tabla externa se produce un error con el mensaje siguiente.

> Mensaje 105019, nivel 16, estado 1, línea 34<BR>
> No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_IsDirExist. Mensaje de error de Java: com.microsoft.azure.storage.StorageException: el servidor no pudo autenticar la solicitud. Asegúrese de que el valor del encabezado de autenticación tenga el formato correcto: Error [com.microsoft.azure.storage.StorageException: el servidor no pudo autenticar la solicitud. Asegúrese de que el valor del encabezado de autorización tenga el formato correcto, incluida la firma.] al obtener acceso al archivo externo.'<BR>

**Posible motivo:**  
se usó una clave de Azure Storage incorrecta para crear la credencial con ámbito de base de datos.

**Posible solución:**  
quite todos los objetos relacionados (es decir, el origen de datos, el formato de archivo) y, a continuación, quite y vuelva a crear la credencial con ámbito de base de datos con la clave de almacenamiento correcta.


### <a name="kerberos-configuration-capitalization"></a>Capitalización de configuración de Kerberos

**Escenario:**  
SQL Server se configura con el clúster de Cloudera habilitado para Kerberos. SQL Server se ha reiniciado después de todos los cambios de configuración. Los servicios Motor de PolyBase y Movimiento de datos de PolyBase se ejecutan después del reinicio. Se devuelven los mensajes de error siguientes:

Origen de datos configurado sin ubicación de seguimiento de trabajo:  
> org.apache.hadoop.fs.FileSystem: no se pudo crear una instancia del proveedor org.apache.hadoop.fs.viewfs.ViewFileSystem

Origen de datos configurado con ubicación de seguimiento de trabajo:  
> Error [No se puede obtener el dominio Kerberos] al obtener acceso al archivo externo

**Posible motivo:**  
el valor de la propiedad "hadoop.security.authentication" indica kerberos en el archivo Coresite.xml.

**Posible solución:**  
La propiedad "hadoop.security.authentication" del archivo Coresite.xml debe ser KERBEROS (todo en mayúsculas) como valor. 

Para obtener más información sobre la solución de problemas de PolyBase y Kerberos, consulte [Solución de problemas de conectividad de Kerberos con PolyBase](polybase-troubleshoot-connectivity.md).

### <a name="mapred-sitexml-missing-needed-values"></a>Ausencia de valores necesarios en mapred-site.xml

**Escenario:**  
SQL Server o APS se configura con un clúster HDP compatible. Las consultas que no requieren delegación funcionan, pero se produce un error en ellas con el mensaje siguiente cuando se usa la sugerencia 'FORCE PUSHDOWN' con los siguientes mensajes de error:

> Mensaje 7320, nivel 16, estado 110, línea 35<BR>
> No se puede ejecutar la consulta "Consulta remota" en el proveedor OLE DB "SQLNCLI11" para el servidor vinculado "(NULL)". No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a JobSubmitter_PollJobStatus: Error [org.apache.hadoop.ipc.RemoteException(java.lang.NullPointerException): java.lang.NullPointerException<BR>
> at org.apache.hadoop.mapreduce.v2.hs.HistoryClientService$HSClientProtocolHandler.getTaskAttemptCompletionEvents(HistoryClientService.java:277)<BR>
> at org.apache.hadoop.mapreduce.v2.api.impl.pb.service.MRClientProtocolPBServiceImpl.getTaskAttemptCompletionEvents(MRClientProtocolPBServiceImpl.java:173)<BR>
> at org.apache.hadoop.yarn.proto.MRClientProtocol$MRClientProtocolService$2.callBlockingMethod(MRClientProtocol.java:283)<BR>
> at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:619)<BR>
> at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:962)<BR>
> at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2127)<BR>
> at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2123)<BR>
> at java.security.AccessController.doPrivileged(Native Method)<BR>
> at javax.security.auth.Subject.doAs(Subject.java:415)<BR>
> at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1671)<BR>
> at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2121)<BR>
> ] al obtener acceso al archivo externo.'<BR>

**Posible motivo:**  
en mapred-side.xml faltan algunos valores necesarios que buscan resultados intermedios y finales.

**Posible solución:**  
agregue las siguientes propiedades y asocie los valores correctos tal como se muestra en Ambari en el archivo mapred-site.xml de SQL Server. 

```xml
<property>
<name>yarn.app.mapreduce.am.staging-dir</name>
<value>/user</value>
</property>
<property>
<name>mapreduce.jobhistory.done-dir</name>
<value>/mr-history/done</value>
</property>
<property>
<name>mapreduce.jobhistory.intermediate-done-dir</name>
<value>/mr-history/tmp</value>
</property>
```

### <a name="configuring-access-by-hostname"></a>Configuración del acceso por nombre de host 

**Escenario:**  
SQL Server está configurado para tener acceso a un clúster de Hadoop compatible. Al crear una tabla externa se devuelve uno de los siguientes errores:

> No se puede ejecutar la consulta "Consulta remota" en el proveedor OLE DB "SQLNCLI11" para el servidor vinculado "(NULL)". 110802;Se produjo un error de DMS interno que provocó un error en esta operación. Detalles: excepción Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DmsSqlNativeException, mensaje: SqlNativeBufferReader.Run, error en OdbcExecuteQuery: SqlState: 42000, NativeError: 8680, 'error al llamar a: SQLExecDirect(this->GetHstmt(), (SQLWCHAR *)statementText, SQL_NTS), código de retorno de SQL: -1 | Información de error de SQL: SrvrMsgState: 26, SrvrSeverity: 17,  error <1>: ErrorMsg: [Microsoft][controlador ODBC 13 para SQL Server][SQL Server]error interno del procesador de consultas: error inesperado durante el procesamiento de una fase de consulta remota. | Error al llamar: pReadConn->ExecuteQuery(statementText, bufferFormat) | Estado: FFFF, número: 24, conexiones activas: 8', cadena de conexión: controlador = {pdwodbc}; APP = RCSmall-DmsNativeReader:WAD1D16HD2001\mpdwsvc (3600)-ODBC-PoolId1433;Trusted_Connection = sí;AutoTranslate = no;Server = \\.\pipe\sql\query<BR>

> [Thread:30544] [AbstractReaderWorker:ErrorEvent] (Error, High): QueryId QID2433 PlanId 6c3a4551-e54c-4c06-a5ed-a8733edac691 StepId 7:<BR>
> No se pudo obtener el bloque: BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: No se pudo obtener el bloque: BP-1726738607-192.168.225.121-1443123675290:blk_1159687047_86196509 file=/user/hive/warehouse/u_data/000000_0<BR>
> at Microsoft.SqlServer.DataWarehouse.DataMovement.Common.ExternalAccess.HdfsBridgeReadAccess.Read(MemoryBuffer buffer, Boolean& isDone)<BR>
> at Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.DataReader.ExternalMoveBufferReader.Read()<BR>
> at Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.ReadAndSendData()<BR>
> at Microsoft.SqlServer.DataWarehouse.DataMovement.Workers.ExternalMoveReaderWorker.Execute(Object status)<BR>

**Posible motivo:**  
este mensaje de error puede aparecer cuando el clúster de Hadoop se configura en una configuración en la que solo se puede acceder a los nodos de datos fuera del clúster mediante el nombre de host y no la dirección IP.

**Posible solución:**  
agregue lo siguiente al archivo hdfs-site.xml en el lado cliente (SQL Server). Esta configuración hará que el nodo de nombre devuelva un URI para los nodos de datos con el nombre de host en lugar de la dirección IP interna.

```xml
<property>
<name>dfs.client.use.datanode.hostname</name>
<value>true</value>
</property>
```

### <a name="folder-organization-forces-excess-memory-overhead"></a>La organización de carpetas fuerza la sobrecarga de la memoria excesiva

**Escenario:**  
SQL Server ejecuta una consulta de PolyBase en un directorio con un gran número de archivos (más de 30 000 archivos en la ruta de acceso del directorio de forma recursiva) y se devuelve uno de los siguientes mensajes de error:

> Mensaje 105019, nivel 16, estado 1, línea 1<BR>
> No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_GetFileNameByIndex. Mensaje de excepción de Java: límite de sobrecarga de GC superado: Error [límite de sobrecarga de GC superado] al obtener acceso al archivo externo.'<BR>

> Mensaje 105019, nivel 16, estado 1, línea 1<BR>
> No se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_GetDirectoryFiles. Mensaje de excepción de Java: espacio de montón de Java: Error [espacio de montón de Java] al obtener acceso al archivo externo.'

**Posible motivo:**  
al procesar una ruta de acceso, PolyBase enumera todos los archivos de esa ruta de acceso y hay una sobrecarga de memoria fija asociada a la estructura de datos que se usa para representar los archivos. Con un gran número de archivos, esta sobrecarga adquiere notabilidad y puede consumir finalmente toda la memoria disponible para la JVM.

**Posible solución:**  
reorganice los datos en varios directorios para que cada directorio contenga un subconjunto de archivos y, a continuación, divida la consulta en varias que operen en una parte de la ruta de acceso original a la vez y materialice las tablas como tablas de SQL Server (antes de combinarlas).

Ejemplo: Supongamos que los datos de la tabla externa están en la siguiente ubicación: Orders/file1.txt,...,file30K.txt.

Cambie el diseño para que los datos se almacenen en una estructura de partición de archivos convencional en Orders/*aaaa*/*mm*/*dd*/file1.txt.
Haga que la tabla externa apunte a una ruta de acceso de directorio inferior como mes (mm) o día (dd) e importe los archivos en tablas de SQL Server por partes y, a continuación, agréguelos como parte de una tabla.
Incluso si tenía la estructura de directorios correcta con la que comenzar, siga el paso n.º 2 para poder trabajar con los numerosos archivos ya mencionados sin quedarse sin memoria de JVM.


### <a name="unexpected-characters-in-configuration-files"></a>Caracteres inesperados en archivos de configuración

**Escenario:**  
configuración de SQL Server o APS con un clúster de Hadoop que implique modificar yarn-site.xml, hdfs-site.xml y otros archivos de configuración. Se observa el siguiente mensaje de error de SQL Server:

> Mensaje 105019, nivel 16, estado 1, línea 1<BR>
> Microsoft.SqlServer.DataWarehouse.Common.ErrorHandling.MppSqlException: no se pudo acceder a EXTERNAL TABLE debido a un error interno: 'Excepción de Java generada en la llamada a HdfsBridge_Connect. Mensaje de excepción de Java:com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: byte 1 no válido de la secuencia UTF-8 de 1 byte.: Error [com.sun.org.apache.xerces.internal.impl.io.MalformedByteSequenceException: byte 1 no válido de la secuencia UTF-8 de 1 byte.] al obtener acceso al archivo externo.' ---><BR>

**Posible motivo:**  
esto puede ocurrir si ha copiado y pegado texto en archivos de configuración desde un sitio web o una ventana de chat. Es posible que haya caracteres no deseados o no imprimibles en los archivos de configuración.

**Posible solución:**  
abra los archivos en un editor de texto distinto (que no sea el Bloc de notas) y busque estos caracteres y elimínelos. Reinicie los servicios necesarios. 


## <a name="see-also"></a>Consulte también

[Supervisión y solución de problemas de PolyBase](polybase-troubleshooting.md)  
[Solución de problemas de conectividad de Kerberos con PolyBase](polybase-troubleshoot-connectivity.md)  

