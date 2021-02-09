---
title: Extracción de SqlPackage
description: Obtenga información sobre cómo automatizar las tareas de desarrollo de bases de datos con Extract de SqlPackage.exe. Vea ejemplos y los parámetros, las propiedades y las variables SQLCMD disponibles.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: c4a9947520ef3914a2ccb34aba5ffaacc1bc6bb2
ms.sourcegitcommit: 5ceafd29b8f22edb800cec150f0ccddea43313e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983648"
---
# <a name="sqlpackage-extract-parameters-and-properties"></a>Parámetros y propiedades de Extract de SqlPackage
La acción Extract de SqlPackage.exe crea un esquema de una base de datos conectada en un archivo DACPAC (.dacpac). De forma predeterminada, los datos no se incluyen en el archivo. dacpac. Para incluir datos, use la [acción Export](sqlpackage-export.md) o las propiedades Extract *ExtractAllTableData*/*TableData*. 

## <a name="command-line-syntax"></a>Sintaxis de línea de comandos

**SqlPackage.exe** inicia las acciones especificadas usando los parámetros, las propiedades y las variables de SQLCMD especificadas en la línea de comandos.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

> [!NOTE]
> Cuando se extrae una base de datos con credenciales de contraseña (por ejemplo, un usuario de autenticación SQL), la contraseña se sustituye por otra diferente con la complejidad adecuada. Los usuarios de SqlPackage o DacFx deben cambiar la contraseña una vez que se haya publicado el archivo .dacpac.

## <a name="parameters-for-the-extract-action"></a>Parámetros de la acción Extract

|Parámetro|Forma corta|Value|Descripción|
|---|---|---|---|
|**/Action:**|**/a**|Extract|Especifica la acción que se va a realizar. |
|**/AccessToken:**|**/at**|{string}| Especifica el token de acceso de autenticación basada en tokens que se usará al conectarse a la base de datos de destino. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Especifica si la salida del registro de diagnóstico es la consola. El valor predeterminado es False. |
|**/ DiagnosticsFile:**|**/DF**|{string}|Especifica un archivo para almacenar los registros de diagnóstico. |
|**/MaxParallelism:**|**/mp**|{int}| Especifica el nivel de paralelismo para las operaciones simultáneas que se ejecutan en una base de datos. El valor predeterminado es 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Especifica si sqlpackage.exe debe sobrescribir los archivos existentes. Si se especifica False, sqlpackage.exe anula la acción si se encuentra un archivo existente. El valor predeterminado es True. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Especifica un par de nombre y valor para una propiedad específica de acción; {PropertyName}={Value}. Remítase a la ayuda de una acción determinada para ver los nombres de propiedad de esa acción. Ejemplo: sqlpackage.exe /Action:Extract /?. |
|**/Quiet:**|**/q**|{True&#124;False}|Especifica si se suprimen los comentarios detallados. El valor predeterminado es False. |
|**/SourceConnectionString:**|**/SCS**|{string}|Especifica una cadena de conexión válida de SQL Server o SQL Azure para la base de datos de origen. Si se especifica este parámetro, lo usan exclusivamente los demás parámetros de origen. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Define el nombre de la base de datos de origen. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Especifica si se debe usar cifrado SQL para la conexión de base de datos de origen. |
|**/SourcePassword:**|**/sp**|{string}|En escenarios de autenticación de SQL Server, define la palabra clave que se va a usar para acceder a la base de datos de origen. |
|**/SourceServerName:**|**/ssn**|{string}|Define el nombre del servidor que hospeda la base de datos de origen. |
|**/SourceTimeout:**|**/st**|{int}|Especifica el tiempo de espera para establecer una conexión con la base de datos de origen, en segundos. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Especifica si se usará TLS para cifrar la conexión con la base de datos de origen y no tener que recorrer la cadena de certificados para validar la confianza. |
|**/SourceUser:**|**/su**|{string}|En escenarios de autenticación de SQL Server, define el usuario de SQL Server que se va a usar para acceder a la base de datos de origen. |
|**/TargetFile:**|**/tf**|{string}| Especifica un archivo de destino (es decir, un archivo. dacpac) que se va a usar como destino de la acción en lugar de una base de datos. Si se usa este parámetro, el resto de parámetros de destino no serán válidos. Este parámetro no será válido para acciones que solo admitan destinos de base de datos.| 
|**/TenantId:**|**/tid**|{string}|Representa el identificador de inquilino de Azure AD o el nombre de dominio. Esta opción se requiere para admitir usuarios de Azure AD invitados o importados, así como cuentas Microsoft (MSA) como outlook.com, hotmail.com o live.com. Si se omite este parámetro, se usará el identificador de inquilino predeterminado para Azure AD, suponiendo que el usuario autenticado sea un usuario nativo para esta instancia de AD. Sin embargo, en este caso no se admiten usuarios invitados o importados ni cuentas de Microsoft hospedadas en este directorio de Azure AD y se producirá un error en la operación. <br/> Para más información sobre la autenticación universal de Active Directory, vea [Autenticación universal con SQL Database y Azure Synapse Analytics (compatibilidad de SSMS con MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Especifica si se debe usar la autenticación universal. Cuando se establece en true, el protocolo de autenticación interactiva se activa y admite MFA. Esta opción también se puede usar para la autenticación de Azure AD sin MFA, mediante un protocolo interactivo que requiere que el usuario escriba su nombre de usuario y contraseña o autenticación integrada (credenciales de Windows). Cuando/UniversalAuthentication se establece en True, no se puede especificar ninguna autenticación de Azure AD en SourceConnectionString (/scs). Cuando/UniversalAuthentication se establece en False, se debe especificar la autenticación de Azure AD en SourceConnectionString (/scs). <br/> Para más información sobre la autenticación universal de Active Directory, vea [Autenticación universal con SQL Database y Azure Synapse Analytics (compatibilidad de SSMS con MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

## <a name="properties-specific-to-the-extract-action"></a>Propiedades específicas de la acción Extract

|Propiedad|Value|Descripción|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Especifica el tiempo de espera de comando en segundos cuando se ejecutan consultas en SQL Server.|
|**/p:**|DacApplicationDescription=(STRING)|Define la descripción de la aplicación que se va a guardar en los metadatos del DACPAC.|
|**/p:**|DacApplicationName=(STRING)|Define el nombre de la aplicación que se va a guardar en los metadatos del DACPAC. El valor predeterminado es el nombre de la base de datos.|
|**/p:**|DacMajorVersion=(INT32 '1')|Define la versión principal que se va a guardar en los metadatos del DACPAC.|
|**/p:**|DacMinorVersion=(INT32 '0')|Define la versión secundaria que se va a guardar en los metadatos del DACPAC.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Especifica el tiempo de expiración de bloqueo de la base de datos en segundos al ejecutar consultas en SQLServer. Use -1 para esperar indefinidamente.|
|**/p:**|ExtractAllTableData=(BOOLEAN)|Indica si se extraen los datos de todas las tablas de usuario. Si es "true", se extraen los datos de todas las tablas de usuario y no se pueden especificar tablas de usuario individuales para la extracción de datos. Si es "false", especifique una o varias tablas de usuario de las que extraer datos.|
|**/p:**|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')|Si se establece en true, solo se extraen los objetos con ámbito de aplicación para el origen especificado. Si se establece en false, se extraen todos los objetos para el origen especificado.|
|**/p:**|ExtractReferencedServerScopedElements=(BOOLEAN 'True')|Si se establece en true, se extraen los objetos de inicio de sesión, de auditoría de servidor y de credencial a los que hacen referencia los objetos de base de datos de origen.|
|**/p:**|ExtractUsageProperties=(BOOLEAN)|Especifica si las propiedades de uso, como el número de filas de tabla o el tamaño del índice, se extraerán de la base de datos.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Especifica si se deben omitir las propiedades extendidas.|
|**/p:**|IgnorePermissions=(BOOLEAN 'True')|Especifica si se deben omitir los permisos.|
|**/p:**|IgnoreUserLoginMappings=(BOOLEAN)|Especifica si se omiten las relaciones entre usuarios e inicios de sesión.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Especifica el tiempo de espera del comando de larga duración en segundos al ejecutar consultas en SQL Server. Use 0 para esperar indefinidamente.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Especifica el tipo de almacenamiento de seguridad para el modelo de esquema que se usa durante la extracción.|
|**/p:**|TableData=(STRING)|Indica la tabla de la que se extraerán los datos. Especifique el nombre de tabla con o sin corchetes en ambas partes del nombre en el siguiente formato: nombre_esquema.identificador_tabla. Esta opción se puede especificar varias veces.|
|**/p:**| TempDirectoryForTableData=(STRING)|Especifica el directorio temporal que se usará para almacenar en búfer los datos de la tabla antes de escribirlos en el archivo de paquete.|
|**/p:**|VerifyExtraction=(BOOLEAN)|Especifica si el archivo DACPAC que se extrajo debe comprobarse.|

## <a name="next-steps"></a>Pasos siguientes

- Mas información sobre [sqlpackage](sqlpackage.md)