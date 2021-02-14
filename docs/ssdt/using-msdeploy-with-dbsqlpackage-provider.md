---
title: Usar MSDeploy con el proveedor de dbSqlPackage
description: Obtenga información sobre el proveedor DbSqlPackage obsoleto de MSDeploy. Vea parámetros, ejemplos y herramientas de publicación de bases de datos alternativas de SQL Server y Azure SQL Database.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 213b91ab-03e9-431a-80f0-17eed8335abe
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: e9ceb447b078f7219d2fadec2dee4274a74190e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066830"
---
# <a name="using-msdeploy-with-dbsqlpackage-provider"></a>Usar MSDeploy con el proveedor de dbSqlPackage

**DbSqlPackage** es un proveedor de **MSDeploy** que permite interactuar con bases de datos de SQL Server y Azure SQL Database. **DbSqlPackage** admite las siguientes acciones:  
  
-   **Extract**: crea un archivo de instantáneas de base de datos (.dacpac) a partir de una base de datos activa de SQL Server o Azure SQL Database.  
  
-   **Publicar**: actualiza de forma incremental un esquema de la base de datos para coincidir con el esquema de un archivo .dacpac de origen.  
  
-   **DeployReport**: crea un informe XML de los cambios que realizaría una acción de publicación.  
  
-   **Script**: crea un script de Transact\-SQL equivalente al script ejecutado por la acción de publicación.  
  
Para más información sobre DACFx, consulte la documentación sobre la API administrada de DACFx en [https://msdn.microsoft.com/library/microsoft.sqlserver.dac.aspx](/dotnet/api/microsoft.sqlserver.dac) o [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md) (herramienta de línea de comandos de DACFx).  
  
> [!IMPORTANT]  
> La característica de proveedor dbSqlPackage se eliminará de la siguiente versión principal de Visual Studio. Para información acerca de cómo publicar bases de datos con Web Deploy, consulte [Proveedor dbDacFx para la publicación de bases de datos incrementales](https://www.iis.net/learn/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing).  
  
## <a name="command-line-syntax"></a>Sintaxis de la línea de comandos  
**MSDeploy** con el proveedor **dbSqlPackage** usa una línea de comandos de la forma siguiente:  
  
```  
  
MSDeploy -verb: MSDeploy-verb -source:dbSqlPackage="Input"[,dbSqlPackage-source-parameters] -dest:dpSqlPackage="Input"[,dbSqlPackage-target-parameters]  
```  
  
## <a name="ms-deploy-verbs"></a>Verbos de MS-Deploy  
Especifique los verbos de MS-Deploy con el modificador **-verb** en la línea de comandos de MS-Deploy. El proveedor **dbSqlPackage** admite los siguientes verbos de **MSDeploy**:  
  
|Verbo|Descripción|  
|--------|---------------|  
|volcado|Proporciona información, como el nombre, el número de versión y la descripción, acerca de una base de datos de origen contenida en un archivo .dacpac. Especifique la base de datos de origen con el formato siguiente en la línea de comandos:<br /><br />**msdeploy -verb:dump -source:dbSqlPackage="** _ruta_de_acceso_del_archivo_.dacpac_ **"**|  
|sync|Especifique las acciones dbSqlPackage usando el formato siguiente en la línea de comandos:<br /><br />**msdeploy -verb:sync -source:dbSqlPackage**="input" _[,parámetros_de_origen_de_DbSqlPackage] -_ **dest:dbSqlPackage**="input" *[,parámetros_de_destino_de_DbSqlPackage]*<br /><br />Vea las secciones a continuación para conocer los parámetros de origen y de destino para el verbo sync.|  
  
## <a name="dbsqlpackage-source"></a>Origen de dbSqlPackage  
El proveedor de **dbSqlPackage** obtiene una entrada que es una cadena de conexión válida de SQL Server o SQL Azure o bien una ruta de acceso a un archivo .dacpac en el disco.  La sintaxis para especificar el origen de entrada para el proveedor es la siguiente:  
  
|Entrada|Valor predeterminado|Descripción|  
|---------|-----------|---------------|  
|**-source:dbSqlPackage=** {*input*}|**N/D**|*input* es una cadena de conexión válida de SQL Server o SQL Azure o una ruta de acceso a un archivo .dacpac en el disco.<br /><br />**NOTA:** Las únicas propiedades de cadena de conexión que se admiten cuando se usa una cadena de conexión como origen de entrada son *InitialCatalog, DataSource, UserID, Password, IntegratedSecurity, Encrypt, TrustServerCertificate* y *ConnectionTimeout*.|  
  
Si el origen de entrada es una cadena de conexión a una base de datos activa de SQL Server o Azure SQL Database, **dbSqlPackage** extraerá una instantánea de base de datos con formato de archivo .dacpac a partir de Azure SQL Database o SQL Server.  
  
Los parámetros de **Source** son:  
  
|Parámetro|Valor predeterminado|Descripción|  
|-------------|-----------|---------------|  
|**Profile**:{ *string*}|N/D|Especifica la ruta de acceso a un archivo para un perfil de publicación DAC. El perfil define una colección de propiedades y variables que se usarán cuando se genere el archivo .dacpac resultante. El perfil de publicación se pasa al destino y se usa como opciones predeterminadas cuando se lleva a cabo una acción **Publish**, **Script** o **DeployReport**.|  
|**DacApplicationName**={ *string*}|Nombre de la base de datos|Define el nombre de la aplicación que se va a guardar en los metadatos del DACPAC. La cadena predeterminada es el nombre de la base de datos.|  
|**DacMajorVersion** ={*integer*}|**1**|Define la versión principal que se va a guardar en los metadatos del DACPAC.|  
|**DacMinorVersion**={*integer*}|**0**|Define la versión secundaria que se va a guardar en los metadatos del DACPAC.|  
|**DacApplicationDescription**={ *string* }|N/D|Define la descripción de la aplicación que se va a guardar en los metadatos del DACPAC.|  
|**ExtractApplicationScopedObjectsOnly={True &#124; False}**|**True**|Si es **True**, solo extrae los objetos con ámbito de aplicación del origen. Si es **False**, extrae tanto los objetos con ámbito de aplicación como los objetos con ámbito diferente al de aplicación.|  
|**ExtractReferencedServerScopedElements={True &#124; False}**|**True**|Si es **True**, se extraen los objetos de inicio de sesión, de auditoría de servidor y de credencial a los que hacen referencia los objetos de base de datos de origen.|  
|**ExtractIgnorePermissions={True &#124; False}**|**False**|Si es **True**, omite los permisos de extracción de todos los objetos extraídos; si es **False**, no los extrae.|  
|**ExtractStorage={File&#124;Memory}**|**Archivo**|Especifica el tipo de almacenamiento de seguridad para el modelo de esquema que se usa durante la extracción.|  
|**ExtractIgnoreExtendedProperties={True&#124;False}**|**False**|Especifica si se deben omitir las propiedades extendidas.|  
|**VerifyExtraction = {True&#124;False}**|**False**|Especifica si el archivo DACPAC que se extrajo debe comprobarse.|  
  
## <a name="dbsqlpackage-destination"></a>Destino de DbSqlPackage  
El proveedor de **dbSqlPackage** acepta una cadena de conexión válida de SQL Server o SQL Azure o bien una ruta de acceso a un archivo .dacpac en el disco como entrada de destino.  La sintaxis para especificar el destino de entrada para el proveedor es la siguiente:  
  
|Entrada|Valor predeterminado|Descripción|  
|---------|-----------|---------------|  
|-**dest:dbSqlPackage**={*input*}|N/D|*input* es una cadena de conexión válida de SQL Server o SQL Azure o una ruta de acceso completa o parcial a un archivo .dacpac en el disco. Si *input* es una ruta de acceso a un archivo, no se pueden especificar otros parámetros.|  
  
Los parámetros de **Destination** siguientes se encuentran disponibles para todas las operaciones de **dbSqlPackage**:  
  
|Propiedad|Valor predeterminado|Descripción|  
|------------|-----------|---------------|  
|**Action={Publish&#124;DeployReport&#124;Script}**|N/D|Parámetro opcional que especifica la acción que se va a llevar a cabo en **Destination**.|  
|**AllowDropBlockingAssemblies ={True &#124; False}**|**False**|Especifica si la publicación de **SqlClr** quita los ensamblados de bloqueo como parte del plan de implementación. De forma predeterminada, cualquier ensamblado de bloqueo o de referencia bloquea la actualización de un ensamblado si el ensamblado de referencia debe quitarse.|  
|**AllowIncompatiblePlatform={True &#124; False}**|**False**|Especifica si la acción de publicar debe continuar a pesar de las posibles plataformas de SQL Server incompatibles.|  
|**BackupDatabaseBeforeChanges={True &#124; False}**|**False**|Hace una copia de seguridad de la base de datos antes de implementar ningún cambio.|  
|**BlockOnPossibleDataLoss={ True &#124; False}**|**True**|Especifica si el episodio de publicación se termina si la operación de publicación puede ocasionar la pérdida de datos.|  
|**BlockWhenDriftDetected={ True &#124; False}**|**True**|Especifica si bloquear la actualización de una base de datos cuyo esquema ha dejado de corresponderse con su registro o no está registrada.|  
|**CommentOutSetVarDeclarations= {True &#124; False}**|**False**|Especifica si las declaraciones de variable **SETVAR** se incluyen entre comentarios en el script de publicación generado. Puede optar por esta opción si piensa usar una herramienta como **SQLCMD.exe** para especificar los valores de la línea de comandos al publicar.|  
|**CompareUsingTargetCollation={ True &#124; False}**|**False**|Esta configuración determina la forma en que se trata la intercalación de la base de datos durante la implementación; de forma predeterminada, la intercalación de la base de datos de destino se actualizará si no coincide con la especificada por el origen.  Cuando se ha establecido esta opción, se usará la intercalación de la base de datos (o servidor) de destino.|  
|**CreateNewDatabase={ True &#124; False}**|**False**|Especifica si la base de datos de destino debe actualizarse o si se va a quitar para volver a crearse al publicar en una base de datos.|  
|**DeployDatabaseInSingleUserMode={ True &#124; False}**|**False**|Si es **True**, la base de datos se establecerá en modo de usuario único antes de implementarse.|  
|**DisableAndReenableDdlTriggers={True &#124; False}**|**True**|Especifica si los desencadenadores del Lenguaje de definición de datos (DDL) se deshabilitan al principio del proceso de publicación y se vuelven a habilitar al final de la acción de publicación.|  
|**DoNotAlterChangeDataCaptureObjects={ True &#124; False}**|**True**|Si es **True**, los objetos de captura de datos modificados no se verán alterados.|  
|**DoNotAlterReplicatedObjects=( True &#124; False}**|**True**|Especifica si los objetos que se replican se van a identificar durante la comprobación.|  
|**DropConstraintsNotInSource= {True &#124; False}**|**True**|Especifica si la acción de publicación quita las restricciones que no existen en la instantánea de base de datos (.dacpac) de la base de datos de destino al publicar en una base de datos.|  
|**DropDmlTriggersNotInSource= {True &#124; False}**|**True**|Especifica si la acción de publicación quita los desencadenadores del Lenguaje de manipulación de datos (DML) que no existen en la instantánea de base de datos (.dacpac) de la base de datos de destino al publicar en una base de datos.|  
|**DropExtendedPropertiesNotInSource= {True &#124; False}**|**True**|Especifica si la acción de publicación quita las propiedades extendidas que no existen en la instantánea de base de datos (.dacpac) de la base de datos de destino al publicar en una base de datos.|  
|**DropIndexesNotInSource= {True &#124; False}**|**True**|Especifica si la acción de publicación quita los índices que no existen en la instantánea de base de datos (.dacpac) de la base de datos de destino al publicar en una base de datos.|  
|**DropObjectsNotInSource= {True &#124; False}**|**False**|Especifica si los objetos que no existen en el archivo de instantánea de base de datos (.dacpac) se quitarán de la base de datos de destino al publicar en una base de datos.|  
|**DropPermissionsNotInSource= {True &#124; False}**|**False**|Especifica si la acción de publicación quita los permisos de acción que no existen en la instantánea de base de datos (.dacpac) de la base de datos de destino al publicar en una base de datos.|  
|**DropRoleMembersNotInSource= {True &#124; False}**|**False**|Especifica si la acción de publicación quita los miembros del rol que no existen en la instantánea de base de datos (.dacpac) de la base de datos de destino al publicar en una base de datos.|  
|**GenerateSmartDefaults={True &#124; False}**|**False**|Especifica si **SqlPackage.exe** proporciona un valor predeterminado automáticamente cuando actualiza una tabla que contiene datos con una columna que no admite valores NULL.|  
|**IgnoreAnsiNulls= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en la opción **ANSI NULLS** al publicar en una base de datos.|  
|**IgnoreAuthorizer= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el autorizador al publicar en una base de datos.|  
|**IgnoreColumnCollation= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en la intercalación de columnas al publicar en una base de datos.|  
|**IgnoreComments= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el orden de comentarios al publicar en una base de datos.|  
|**IgnoreCryptographicProviderFile= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en la ruta de acceso a los archivos para un proveedor criptográfico al publicar en una base de datos.|  
|**IgnoreDdlTriggerOrder= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el orden de los desencadenadores del Lenguaje de definición de datos (DDL) al publicar en una base de datos.|  
|**IgnoreDdlTriggerState={True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el estado habilitado o deshabilitado de los desencadenadores DDL al publicar en una base de datos.|  
|**IgnoreDefaultSchema={True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el esquema predeterminado al publicar en una base de datos.|  
|**IgnoreDmlTriggerOrder= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el orden de los desencadenadores DML al publicar en una base de datos.|  
|**IgnoreDmlTriggerState= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el estado habilitado o deshabilitado de los desencadenadores DML al publicar en una base de datos.|  
|**IgnoreExtendedProperties= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en las propiedades extendidas al publicar en una base de datos.|  
|**IgnoreFileAndLogFilePath={True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en la ruta de acceso a los archivos y los archivos de registro al publicar en una base de datos.|  
|**IgnoreFilegroupPlacement= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en la colocación de los **FILEGROUP** al publicar en una base de datos.|  
|**IgnoreFileSize= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en los tamaños de los archivos al publicar en una base de datos.|  
|**IgnoreFillFactor= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en los factores de relleno al publicar en una base de datos.|  
  
|Propiedad|Valor predeterminado|Descripción|  
|------------|-----------|---------------|  
|**IgnoreFullTextCatalogFilePath= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en la ruta de acceso a los archivos de índice de texto completo al publicar en una base de datos.|  
|**IgnoreIdentitySeed= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en la inicialización de una columna de identidad al publicar en una base de datos.|  
|**IgnoreIncrement= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el incremento de una columna de identidad al publicar en una base de datos.|  
|**IgnoreIndexOptions ={True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en las opciones de índice al publicar en una base de datos.|  
|**IgnoreIndexPadding= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en el relleno de índice al publicar en una base de datos.|  
|**IgnoreKeywordCasing= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en las mayúsculas y minúsculas de las palabras clave al publicar en una base de datos.|  
|**IgnoreLockHintsOnIndexes= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en las sugerencias de bloqueo o índices al publicar en una base de datos.|  
|**IgnoreLoginSids= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en el identificador de seguridad (SID) al publicar en una base de datos.|  
|**IgnoreNotForReplication= {True &#124; False}**|**False**|Especifica si se omitirá o se actualizará la configuración de not-for-replication al publicar en una base de datos.|  
|**IgnoreObjectPlacementOnPartitionScheme= {True &#124; False}**|**True**|Especifica si se omitirá o se actualizará la colocación de un objeto en un esquema de partición al publicar en una base de datos.|  
|**IgnorePartitionSchemes= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en los esquemas de partición y las funciones al publicar en una base de datos.|  
|**IgnorePermissions= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en los permisos al publicar en una base de datos.|  
|**IgnoreQuotedIdentifiers= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en los valores de identificadores entre comillas al publicar en una base de datos.|  
|**IgnoreRoleMembership={True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en las pertenencias a roles de los inicios de sesión al publicar en una base de datos.|  
|**IgnoreRouteLifetime= {True &#124; False}**|**True**|Especifica si se van a omitir o actualizar las diferencias en las pertenencias a roles de los inicios de sesión al publicar en una base de datos.|  
|**IgnoreSemicolonBetweenState= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en los puntos y coma entre las instrucciones Transact-SQL al publicar en una base de datos.|  
|**IgnoreTableOptions= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en las opciones de tabla al publicar en una base de datos.|  
|**IgnoreUserSettingsObjects= {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en las opciones de usuario al publicar en una base de datos.|  
|**IgnoreWhitespace= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en los espacios en blanco al publicar en una base de datos.|  
|**IgnoreWithNocheckOnCheckConstraints={True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el valor de la cláusula **WITH NOCHECK** para las restricciones CHECK al publicar en una base de datos.|  
|**IgnoreWithNocheckOnForeignKeys={True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en el valor de la cláusula **WITH NOCHECK** para las claves externas al publicar en una base de datos.|  
|**IncludeCompositeObjects= {True &#124; False}**|**False**|Especifica si se incluyen todos los elementos compuestos como parte de una sola operación de publicación.|  
|**IncludeTransactionalScripts={True &#124; False}**|**False**|Especifica si se usan instrucciones transaccionales siempre que sea posible al publicar en una base de datos.|  
|**NoAlterStatementsToChangeClrTypes={True &#124; False}**|**False**|Especifica que la acción de publicación debe quitar y volver a crear siempre un ensamblado en lugar de emitir una instrucción ALTER ASSEMBLY.|  
|**PopulateFilesOnFilegroups= {True &#124; False}**|**True**|Especifica si también se crea un archivo nuevo al crear un nuevo **FileGroup** en la base de datos de destino.|  
|**RegisterDataTierApplication={True &#124; False}**|**False**|Especifica si el esquema está registrado con el servidor de base de datos.|  
|**ScriptDatabaseCollation {True &#124; False}**|**False**|Especifica si se omitirán o se actualizarán las diferencias en la intercalación de la base de datos al publicar en una base de datos.|  
|**ScriptDatabaseCompatibility= {True &#124; False}**|**True**|Especifica si se omitirán o se actualizarán las diferencias en la compatibilidad de la base de datos al publicar en una base de datos.|  
|**ScriptDatabaseOptions= {True &#124; False}**|**True**|Especifica si se establecen o se actualizan las propiedades de la base de datos de destino al publicar en una base de datos.|  
|**ScriptFileSize={True &#124; False}**|**False**|Controla si el tamaño se especifica cuando se agrega un archivo a un grupo de archivos.|  
|**ScriptNewConstraintValidation= {True &#124; False}**|**True**|Especifica si se comprueban todas las restricciones como un conjunto al final de la publicación, evitando los errores en los datos que ocasiona un restricción de clave externa o CHECK a la mitad de una acción de publicación. Si esta opción es **False**, las restricciones se publican sin comprobar los datos correspondientes.|  
|**ScriptDeployStateChecks={True &#124; False}**|**False**|Especifica si se generan instrucciones en el script de publicación para comprobar que los nombres del servidor y de la base de datos coinciden con los especificados en el proyecto de base de datos.|  
|**ScriptRefreshModule= {True &#124; False}**|**True**|Especifica si incluir instrucciones de actualización al final del script de publicación.|  
|**Storage={File&#124;Memory}**|**Memoria**|Especifica la forma en que se almacenan los elementos cuando se genera el modelo de base de datos. Por motivos de rendimiento, el valor predeterminado es **Memory** (Memoria). Cuando se trata de bases de datos grandes, se requiere un almacenamiento de seguridad de archivos.|  
|**TreatVerificationErrorsAsWarnings= {True &#124; False}**|**False**|Especifica si se deben tratar como advertencias los errores que se producen durante la comprobación de la publicación. La comprobación se realiza con el plan de implementación generado antes de que el plan se ejecute con la base de datos de destino. El plan de comprobación detecta problemas, como la pérdida de objetos solo en el destino (por ejemplo, índices) que deben quitarse para hacer un cambio. La comprobación también detecta situaciones en las que existen dependencias (como tablas o vistas) debido a una referencia a un proyecto compuesto, pero no existen en la base de datos de destino. Podría elegir tratar los errores de comprobación como advertencias para obtener una lista completa de los problemas en lugar de permitir que la acción de publicación se detenga cuando se produce el primer error.|  
|**UnmodifiableObjectWarnings= {True &#124; False}**|**True**|Especifica si generar advertencias cuando se encuentren diferencias en los objetos que no se puedan modificar (por ejemplo, si el tamaño de archivo o las rutas de acceso a los archivos son diferentes para un archivo).|  
|**VerifyCollationCompatibility={True &#124; False}**|**True**|Especifica si se comprobó la compatibilidad de intercalación.|  
|**VerifyDeployment={True &#124; False}**|**True**|Especifica si realizar comprobaciones antes de la publicación que detengan la acción de publicación si hay problemas que podrían impedir que la publicación se realizara correctamente. Por ejemplo, su acción de publicación podría detenerse si obtiene errores durante la publicación porque las claves externas en la base de datos de destino no existen en el proyecto de base de datos.|  
  
> [!NOTE]  
> Los parámetros de destino que se especifiquen invalidarán a aquellos especificados en el perfil de publicación de origen.  
  
> [!NOTE]  
> Es preciso especificar las variables y los valores de **SQLCMD** en el parámetro de origen del perfil de publicación, ya que no se pueden especificar como un parámetro de destino.  
  
Los parámetros de **Destination** siguientes solo están disponibles para las operaciones **DeployReport** y **Script**:  
  
|Parámetro|Valor predeterminado|Descripción|  
|-------------|-----------|---------------|  
|**OutputPath**={ *string* }|N/D|Parámetro opcional que indica a **dbSqlPackage** que cree el archivo de resultados DeployReport o el archivo de resultados Script SQL en la ubicación del disco que especifica *string*. Esta acción sobrescribe los scripts que residen actualmente en la ubicación que indica la cadena.|  
  
> [!NOTE]  
> Si no se proporciona el parámetro **OutputPath** para una acción **DeployReport** o **Script**, se devuelven los resultados en forma de mensaje.  
  
## <a name="examples"></a>Ejemplos  
Esta es una sintaxis de ejemplo para una operación **Extract** mediante **dbSqlPackage**:  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source connection string>",<source parameter> -dest:dbSqlPackage="<target dacpac file path>"  
```  
  
Esta es una sintaxis de ejemplo para una operación **Publish** mediante **dbSqlPackage**:  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=Publish,<destination parameters>  
```  
  
Esta es una sintaxis de ejemplo para una operación **DeployReport** mediante **dbSqlPackage**:  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=DeployReport,OutputPath="<path to output XML file>",<destination parameters>  
```  
  
Esta es una sintaxis de ejemplo para una operación **Script** mediante **dbSqlPackage**:  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=Script,OutputPath="<path to output sql script>",<destination parameters>  
```  
