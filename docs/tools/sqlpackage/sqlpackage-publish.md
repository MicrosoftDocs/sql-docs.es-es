---
title: Publicación de SqlPackage
description: Obtenga información sobre cómo automatizar las tareas de desarrollo de bases de datos con Publish de SqlPackage.exe. Vea ejemplos y los parámetros, las propiedades y las variables SQLCMD disponibles.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 3/10/2021
ms.openlocfilehash: 364849c03150839ce38c4764c788bb8b813b9d53
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622611"
---
# <a name="sqlpackage-publish-parameters-properties-and-sqlcmd-variables"></a>Parámetros, propiedades y variables SQLCMD de Publish de SqlPackage
La operación Publish de SqlPackage.exe actualiza incrementalmente el esquema de una base de datos de destino para que coincida con la estructura de una de origen. Al publicar un paquete de implementación que contiene datos de usuario de todas las tablas o un subconjunto de ellas, se actualizan los datos de la tabla, además del esquema. La implementación de datos sobrescribe el esquema y los datos de las tablas existentes de la base de datos de destino. La implementación de datos no modificará el esquema ni los datos de la base de datos de destino para las tablas que no se incluyen en el paquete de implementación.  

## <a name="command-line-syntax"></a>Sintaxis de línea de comandos

**SqlPackage.exe** inicia las acciones especificadas usando los parámetros, las propiedades y las variables de SQLCMD especificadas en la línea de comandos.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

> [!NOTE]
> Cuando se extrae una base de datos con credenciales de usuario de autenticación SQL, la contraseña se sustituye por otra diferente con la complejidad adecuada. Se da por hecho que, una vez que se publica el archivo .dacpac, se cambia la contraseña del usuario.


## <a name="parameters-for-the-publish-action"></a>Parámetros de la acción Publish

|Parámetro|Forma corta|Value|Descripción|
|---|---|---|---|
|**/Action:**|**/a**|Publicar|Especifica la acción que se va a realizar. |
|**/AccessToken:**|**/at**|{string}| Especifica el token de acceso de autenticación basada en tokens que se usará al conectarse a la base de datos de destino. |
|**/AzureKeyVaultAuthMethod:**|**/akv**|{Interactive&#124;ClientIdSecret}|Especifica el método de autenticación que se usa para acceder a Azure KeyVault si una operación de publicación incluye modificaciones en una tabla o columna cifrada. |
|**/ClientId:**|**/cid**|{string}|Especifica el id. de cliente que se usará en la autenticación con Azure Key Vault, cuando sea necesario. |
|**/DeployScriptPath:**|**/dsp**|{string}|Permite especificar una ruta de acceso al archivo opcional para generar el script de implementación. Para implementaciones de Azure, si hubiera comandos TSQL para crear o modificar la base de datos maestra, se escribirá un script en la misma ruta, pero con "Filename_Master.sql" como el nombre del archivo de salida. |
|**/DeployReportPath:**|**/drp**|{string}|Permite especificar una ruta de acceso al archivo opcional para generar el archivo XML del informe de la implementación. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Especifica si la salida del registro de diagnóstico es la consola. El valor predeterminado es False. |
|**/ DiagnosticsFile:**|**/DF**|{string}|Especifica un archivo para almacenar los registros de diagnóstico. |
|**/MaxParallelism:**|**/mp**|{int}| Especifica el nivel de paralelismo para las operaciones simultáneas que se ejecutan en una base de datos. El valor predeterminado es 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Especifica si sqlpackage.exe debe sobrescribir los archivos existentes. Si se especifica False, sqlpackage.exe anula la acción si se encuentra un archivo existente. El valor predeterminado es True. |
|**/Profile:**|**/pr**|{string}|Especifica la ruta de acceso a un archivo para un perfil de publicación DAC. El perfil define una colección de propiedades y variables que se usarán cuando se generen resultados.|
|**/Properties:**|**/p**|{PropertyName}={Value}|Especifica un par de nombre y valor para una propiedad específica de acción; {PropertyName}={Value}. Remítase a la ayuda de una acción determinada para ver los nombres de propiedad de esa acción. Ejemplo: sqlpackage.exe /Action:Publish /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Especifica si se suprimen los comentarios detallados. El valor predeterminado es False.|
|**/Secret:**|**/secr**|{string}|Especifica el secreto de cliente que se usará en la autenticación con Azure Key Vault, cuando sea necesario. |
|**/SourceConnectionString:**|**/SCS**|{string}|Especifica una cadena de conexión válida de SQL Server o SQL Azure para la base de datos de origen. Si se especifica este parámetro, lo usan exclusivamente los demás parámetros de origen. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Define el nombre de la base de datos de origen. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Especifica si se debe usar cifrado SQL para la conexión de base de datos de origen. |
|**/SourceFile:**|**/sf**|{string}|Especifica un archivo de origen que se usará como origen de la acción en lugar de una base de datos. Si se usa este parámetro, el resto de parámetros de origen no serán válidos. |
|**/SourcePassword:**|**/sp**|{string}|En escenarios de autenticación de SQL Server, define la palabra clave que se va a usar para acceder a la base de datos de origen. |
|**/SourceServerName:**|**/ssn**|{string}|Define el nombre del servidor que hospeda la base de datos de origen. |
|**/SourceTimeout:**|**/st**|{int}|Especifica el tiempo de espera para establecer una conexión con la base de datos de origen, en segundos. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Especifica si se usará TLS para cifrar la conexión con la base de datos de origen y no tener que recorrer la cadena de certificados para validar la confianza. |
|**/SourceUser:**|**/su**|{string}|En escenarios de autenticación de SQL Server, define el usuario de SQL Server que se va a usar para acceder a la base de datos de origen. |
|**/TargetConnectionString:**|**/tcs**|{string}|Especifica una cadena de conexión válida de SQL Server o SQL Azure para la base de datos de destino. Si se especifica este parámetro, lo usan exclusivamente los demás parámetros de destino. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Especifica una invalidación del nombre de la base de datos que es el destino de la acción sqlpackage.exe. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Especifica si se debe usar el cifrado de SQL para la conexión de base de datos de destino. |
|**/TargetPassword:**|**/tp**|{string}|En escenarios de autenticación de SQL Server, define la palabra clave que se va a usar para acceder a la base de datos de destino. |
|**/TargetServerName:**|**/tsn**|{string}|Define el nombre del servidor que hospeda la base de datos de destino. |
|**/TargetTimeout:**|**/tt**|{int}|Especifica el tiempo de espera para establecer una conexión con la base de datos de destino, en segundos. Para Azure AD, se recomienda que este valor sea mayor o igual que 30 segundos.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Especifica si se usará TLS para cifrar la conexión con la base de datos de destino y no tener que recorrer la cadena de certificados para validar la confianza. |
|**/TargetUser:**|**/tu**|{string}|En escenarios de autenticación de SQL Server, define el usuario de SQL Server que se va a usar para acceder a la base de datos de destino. |
|**/TenantId:**|**/tid**|{string}|Representa el identificador de inquilino de Azure AD o el nombre de dominio. Esta opción se requiere para admitir usuarios de Azure AD invitados o importados, así como cuentas Microsoft (MSA) como outlook.com, hotmail.com o live.com. Si se omite este parámetro, se usará el identificador de inquilino predeterminado para Azure AD, suponiendo que el usuario autenticado sea un usuario nativo para esta instancia de AD. Sin embargo, en este caso no se admiten usuarios invitados o importados ni cuentas de Microsoft hospedadas en este directorio de Azure AD y se producirá un error en la operación. <br/> Para más información sobre la autenticación universal de Active Directory, vea [Autenticación universal con SQL Database y Azure Synapse Analytics (compatibilidad de SSMS con MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Especifica si se debe usar la autenticación universal. Cuando se establece en true, el protocolo de autenticación interactiva se activa y admite MFA. Esta opción también se puede usar para la autenticación de Azure AD sin MFA, mediante un protocolo interactivo que requiere que el usuario escriba su nombre de usuario y contraseña o autenticación integrada (credenciales de Windows). Cuando/UniversalAuthentication se establece en True, no se puede especificar ninguna autenticación de Azure AD en SourceConnectionString (/scs). Cuando/UniversalAuthentication se establece en False, se debe especificar la autenticación de Azure AD en SourceConnectionString (/scs). <br/> Para más información sobre la autenticación universal de Active Directory, vea [Autenticación universal con SQL Database y Azure Synapse Analytics (compatibilidad de SSMS con MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/Variables:**|**/v**|{PropertyName}={Value}|Especifica un par de nombre y valor para una variable específica de acción; {VariableName}={Value}. El archivo DACPAC contiene la lista de variables SQLCMD válidas. Se produce un error si no se facilita un valor para cada variable. |

## <a name="properties-specific-to-the-publish-action"></a>Propiedades específicas de la acción Publish

|Propiedad|Valor|Descripción|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Especifica los argumentos de colaborador de implementación adicionales para los colaboradores de implementación. Debe ser una lista de valores delimitada por punto y coma.|
|**/p:**|AdditionalDeploymentContributors=(STRING)|Especifica colaboradores de implementación adicionales que se deben ejecutar cuando se implementa el dacpac. Debe ser una lista delimitada por punto y coma de nombres completos o identificadores de los colaboradores de compilación.|
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Especifica las rutas de acceso para cargar colaboradores de implementación adicionales. Debe ser una lista de valores delimitada por punto y coma. | 
|**/p:**|AllowDropBlockingAssemblies=(BOOLEAN)|Esta propiedad se usa en la implementación de SqlClr para hacer que cualquier ensamblado de bloqueo se quite como parte del plan de implementación. De forma predeterminada, cualquier ensamblado de bloqueo o de referencia bloqueará la actualización de un ensamblado si el ensamblado de referencia tiene que quitarse.|
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Especifica si se intenta la acción a pesar de las posibles plataformas de SQL Server incompatibles.|
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|No bloquee la transferencia de datos en una tabla con Seguridad de nivel de fila si esta propiedad está establecida en true. El valor predeterminado es False.|
|**/p:**|AzureSharedAccessSignatureToken=(STRING)|Token de Firma de acceso compartido (SAS) de Azure. Consulte [SqlPackage para Azure Synapse Analytics](sqlpackage-for-azure-synapse-analytics.md#publish).|
|**/p:**|AzureStorageBlobEndpoint=(STRING)|Punto de conexión de Azure Blob Storage. Consulte [SqlPackage para Azure Synapse Analytics](sqlpackage-for-azure-synapse-analytics.md#publish).|
|**/p:**|AzureStorageContainer=(STRING)|Contenedor de Azure Blob Storage. Consulte [SqlPackage para Azure Synapse Analytics](sqlpackage-for-azure-synapse-analytics.md#publish).|
|**/p:**|AzureStorageKey=(STRING)|Clave de la cuenta de almacenamiento de Azure. Consulte [SqlPackage para Azure Synapse Analytics](sqlpackage-for-azure-synapse-analytics.md#publish).|
|**/p:**|AzureStorageRootPath=(STRING)|Ruta de acceso raíz de almacenamiento dentro del contenedor. Sin esta propiedad, el valor predeterminado de la ruta de acceso es `servername/databasename/timestamp/`. Consulte [SqlPackage para Azure Synapse Analytics](sqlpackage-for-azure-synapse-analytics.md#publish).|
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Hace una copia de seguridad de la base de datos antes de implementar ningún cambio.|
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')| Especifica que la operación se terminará durante el paso de validación del esquema si los cambios de esquema resultantes pueden incurrir en una pérdida de datos, incluso debido a la reducción de la precisión de los datos o a un cambio de tipo de datos que necesite una operación de conversión. El valor predeterminado (`True`) hace que la operación finalice independientemente de si la base de datos de destino contiene datos.  Todavía se puede producir un error en la ejecución con un valor `False` para BlockOnPossibleDataLoss durante la ejecución del plan de implementación si hay datos en el destino que no se pueden convertir al nuevo tipo de columna. |
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|Especifica si bloquear la actualización de una base de datos cuyo esquema ha dejado de corresponderse con su registro o no está registrada.|
|**/p:**|CommandTimeout=(INT32 '60')|Especifica el tiempo de espera de comando en segundos cuando se ejecutan consultas en SQL Server.|
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|Especifica si la declaración de las variables SETVAR se incluyen entre comentarios en el script de publicación generado. Puede optar por esta opción si planea usar una herramienta como SQLCMD.EXE para especificar los valores de la línea de comandos al publicar.|
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|Esta configuración determina la forma en que se trata la intercalación de la base de datos durante la implementación; de forma predeterminada, la intercalación de la base de datos de destino se actualizará si no coincide con la especificada por el origen. Cuando se ha establecido esta opción, se usará la intercalación de la base de datos (o servidor) de destino.|
|**/p:**|CreateNewDatabase=(BOOLEAN)|Especifica si la base de datos de destino debe actualizarse o si se va a quitar para volver a crearse al publicar en una base de datos.|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Define la edición de la instancia de Azure SQL Database.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')|Especifica el tiempo de expiración de bloqueo de la base de datos en segundos al ejecutar consultas en SQLServer. Use -1 para esperar indefinidamente.|
|**/p:**|DatabaseMaximumSize=(INT32)|Define el tamaño máximo en GB de una instancia de Azure SQL Database.|
|**/p:**|DatabaseServiceObjective=(STRING)|Define el nivel de rendimiento de una instancia de Azure SQL Database, como "P0" o "S1".|
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|Si se establece en True, la base de datos se establecerá en modo de usuario único antes de implementarse.|
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')|Especifica si los desencadenadores del Lenguaje de definición de datos (DDL) se deshabilitan al principio del proceso de publicación y se vuelven a habilitar al final de la acción de publicación.|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|Si se establece en true, los objetos de captura de datos modificados no se verán alterados.|
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|Especifica si los objetos que se replican se van a identificar durante la comprobación.|
|**/p:**|DoNotDropObjectType=(STRING)|Un tipo de objeto que no debería quitarse si DropObjectsNotInSource es True. Los nombres de tipo de objeto válidos son: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|DoNotDropObjectTypes=(STRING)|Lista delimitada por puntos y coma de tipos de objetos que no deberían quitarse si DropObjectsNotInSource es true. Los nombres de tipo de objeto válidos son: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|Especifica si las restricciones que no existen en el archivo de la instantánea de base de datos (.dacpac) se van a quitar de la base de datos de destino al publicar en una base de datos.|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|Especifica si los desencadenadores DML que no existen en el archivo de la instantánea de base de datos (.dacpac) se van a quitar de la base de datos de destino al publicar en una base de datos.|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|Especifica si las propiedades extendidas que no existen en el archivo de instantánea de base de datos (.dacpac) se quitarán de la base de datos de destino al publicar en una base de datos.|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|Especifica si los índices que no existen en el archivo de la instantánea de base de datos (.dacpac) se van a quitar de la base de datos de destino al publicar en una base de datos.|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|Especifica si los objetos que no existen en el archivo de instantánea de base de datos (.dacpac) se quitarán de la base de datos de destino al publicar en una base de datos. Este valor tiene prioridad sobre DropExtendedProperties.|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|Especifica si los permisos que no existen en el archivo de la instantánea de base de datos (.dacpac) se van a quitar de la base de datos de destino al publicar actualizaciones en una base de datos.|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|Especifica si los miembros de rol que no se definieron en el archivo de la instantánea de base de datos (.dacpac) se van a quitar de la base de datos de destino al publicar actualizaciones en una base de datos.|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|Especifica si las estadísticas que no existen en el archivo de instantánea de base de datos (.dacpac) se quitarán de la base de datos de destino al publicar en una base de datos.|
|**/p:**|ExcludeObjectType=(STRING)|Un tipo de objeto que se debe omitir durante la implementación. Los nombres de tipo de objeto válidos son: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|ExcludeObjectTypes=(STRING)|Lista delimitada por puntos y coma de tipos de objetos que se deben omitir durante la implementación. Los nombres de tipo de objeto válidos son: Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|Proporciona automáticamente un valor predeterminado cuando se actualiza una tabla que contiene datos con una columna que no admite valores NULL.|
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|Especifica si las diferencias en la configuración ANSI NULLS se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|Especifica si las diferencias en el autorizador se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|Especifica si las diferencias en las intercalaciones de columnas se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|Especifica si hay que omitir las diferencias en el orden de las columnas de una tabla o bien hay que actualizar al publicar en una base de datos.|
|**/p:**|IgnoreComments=(BOOLEAN)|Especifica si las diferencias en los comentarios se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|Especifica si las diferencias en la ruta de acceso del archivo del proveedor de servicios criptográficos se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|Especifica si las diferencias en el orden de los desencadenadores de Data Definition Language (DDL) se deben ignorar o actualizar al publicar en una base de datos o en un servidor.|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|Especifica si las diferencias en el estado habilitado o deshabilitado de los desencadenadores de Data Definition Language (DDL) se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|Especifica si las diferencias en el esquema predeterminado se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|Especifica si las diferencias en el orden de los desencadenadores del lenguaje de manipulación de datos (DML) se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|Especifica si las diferencias en el estado habilitado o deshabilitado de los desencadenadores de DML se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Especifica si las diferencias en las propiedades extendidas se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|Especifica si las diferencias en las rutas de acceso de los archivos y archivos de registro se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|Especifica si las diferencias en la colocación de objetos en FILEGROUPs se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|Especifica si las diferencias en los tamaños de archivo se deben ignorar o si debe generarse una advertencia al publicar en una base de datos.|
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|Especifica si las diferencias en el factor de relleno del almacenamiento de índices se deben ignorar o si debe generarse una advertencia al publicar en una base de datos.|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|Especifica si las diferencias en la ruta de acceso al archivo del catálogo de texto completo se deben ignorar o si debe generarse una advertencia al publicar en una base de datos.|
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|Especifica si las diferencias en el valor de inicialización de una columna de identidad se deben ignorar o actualizar al publicar actualizaciones en una base de datos.|
|**/p:**|IgnoreIncrement=(BOOLEAN)|Especifica si las diferencias en el incremento de una columna de identidad se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|Especifica si las diferencias en las opciones de índice se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|Especifica si las diferencias en el relleno de índice se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|Especifica si las diferencias en el uso de mayúsculas y minúsculas en palabras clave se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|Especifica si las diferencias en las sugerencias de bloqueo en los índices se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')|Especifica si las diferencias en el número de identificación de seguridad (SID) se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|Especifica si la configuración de no replicación se debe ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|Especifica si la colocación de un objeto en un esquema de partición se debe ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnorePartitionSchemes=(BOOLEAN)|Especifica si las diferencias en las funciones y los esquemas de particiones se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnorePermissions=(BOOLEAN)|Especifica si las diferencias en los permisos se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|Especifica si las diferencias en la configuración de identificadores entre comillas se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|Especifica si se omitirán o se actualizarán las diferencias en las pertenencias a roles de los inicios de sesión al publicar en una base de datos.|
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|Especifica si las diferencias en cuanto al periodo durante el cual SQL Server conserva la ruta en la tabla de enrutamiento se deben omitir o si hay que actualizar al publicar en una base de datos.|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|Especifica si las diferencias en los caracteres de punto y coma entre las instrucciones T-SQL se ignorarán o se actualizarán al publicar en una base de datos.|
|**/p:**|IgnoreTableOptions=(BOOLEAN)|Especifica si las diferencias en las opciones de tabla se ignorarán o se actualizarán al publicar en una base de datos.|
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|Especifica si las diferencias en las opciones de partición de tabla se ignorarán o se actualizarán al publicar en una base de datos.  Esta opción solo se aplica a bases de datos del grupo de SQL dedicado de Azure Synapse Analytics.|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|Especifica si las diferencias en los objetos de configuración de usuario se ignorarán o se actualizarán al publicar en una base de datos.|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|Especifica si las diferencias en los espacios en blanco se ignorarán o se actualizarán al publicar en una base de datos.|
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|Especifica si las diferencias en el valor de la cláusula WITH NOCHECK de las restricciones CHECK se omiten o hay que actualizar al publicar.|
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|Especifica si las diferencias en el valor de la cláusula WITH NOCHECK para las claves externas se ignorarán o se actualizarán al publicar en una base de datos.|
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|Incluir todos los elementos compuestos como parte de una única operación de publicación.|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|Especifica si las instrucciones transaccionales se deben usar siempre que sea posible al publicar en una base de datos.|
|**/p:**|LongRunningCommandTimeout=(INT32)|Especifica el tiempo de espera del comando de larga duración en segundos al ejecutar consultas en SQL Server. Use 0 para esperar indefinidamente.|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|Especifica que la acción de publicación debe quitar y volver a crear siempre un ensamblado en lugar de emitir una instrucción ALTER ASSEMBLY.|
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|Especifica si se crea también un nuevo archivo cuando se crea un nuevo FileGroup en la base de datos de destino.|
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|Especifica si el esquema está registrado con el servidor de base de datos.|
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|Especifica si los colaboradores DeploymentPlanExecutor deben ejecutarse cuando se ejecutan otras operaciones.|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|Especifica si las diferencias en la intercalación de la base de datos se deben omitir o actualizar al publicar en una base de datos.|
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|Especifica si las diferencias en la compatibilidad de la base de datos se deben ignorar o actualizar al publicar en una base de datos.|
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|Especifica si las propiedades de la base de datos deben establecerse o actualizarse como parte de la acción de publicación.|
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|Especifica si se generan instrucciones en el script de publicación para comprobar que el nombre de la base de datos y el nombre del servidor coinciden con los nombres especificados en el proyecto de base de datos.|
|**/p:**|ScriptFileSize=(BOOLEAN)|Controla si el tamaño se especifica cuando se agrega un archivo a un grupo de archivos.|
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|Al final de la publicación, se comprobarán todas las restricciones como un conjunto, evitando los errores en los datos que ocasiona una restricción de comprobación o clave de referencia en medio de una acción de publicación. Si esta opción es False, las restricciones se publican sin comprobar los datos correspondientes.|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|Incluye instrucciones de actualización al final del script de publicación.|
|**/p:**|Storage=({File&#124;Memory})|Especifica la forma en que se almacenan los elementos cuando se genera el modelo de base de datos. Por motivos de rendimiento, el valor predeterminado es InMemory. Cuando se trata de bases de datos grandes, se requiere almacenamiento respaldado por archivos.|
|**/p:**|TreatVerificationErrorsAsWarnings=(BOOLEAN)|Especifica si los errores detectados durante la comprobación de la publicación se deben tratar como advertencias. La comprobación se realiza con el plan de implementación generado antes de que el plan se ejecute con la base de datos de destino. El plan de comprobación detecta problemas, como la pérdida de objetos solo en el destino (como los índices) que deben quitarse para hacer un cambio. La comprobación también detecta situaciones en las que existen dependencias (como una tabla o vista) debido a una referencia a un proyecto compuesto, pero no existen en la base de datos de destino. Puede optar por hacer esto para obtener una lista completa de todos los problemas, en lugar de que la acción de publicación se detenga en el primer error.
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|Especifica si generar advertencias cuando se encuentren diferencias en los objetos que no se puedan modificar, por ejemplo, si el tamaño de archivo o las rutas de acceso a los archivos son diferentes para un archivo.|
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|Especifica si se comprobó la compatibilidad de intercalación.|
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|Especifica si realizar comprobaciones antes de la publicación que detengan la acción de publicación si hay problemas que pudieran impedir que la publicación se realizara correctamente. Por ejemplo, la acción de publicación podría detenerse si tiene claves externas en la base de datos de destino que no existan en el proyecto de base de datos, y eso provoca errores al publicar.|
|

## <a name="sqlcmd-variables"></a>Variables SQLCMD

En la tabla siguiente se describe el formato de la opción que puede usar para invalidar el valor de una variable de comando SQL (**sqlcmd**) que se usa durante una acción de publicación. Los valores de variable especificados en la línea de comandos invalidan otros valores asignados a la variable (por ejemplo, un perfil de publicación).  
  
|Parámetro|Valor predeterminado|Descripción|  
|-------------|-----------|---------------|  
|**/Variables:{PropertyName}={Value}**||Especifica un par de nombre y valor para una variable específica de acción; {VariableName}={Value}. El archivo DACPAC contiene la lista de variables SQLCMD válidas. Se produce un error si no se facilita un valor para cada variable.|  

## <a name="next-steps"></a>Pasos siguientes

- Mas información sobre [sqlpackage](sqlpackage.md)