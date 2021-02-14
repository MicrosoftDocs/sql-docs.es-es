---
title: Exportación de SqlPackage
description: Obtenga información sobre cómo automatizar las tareas de desarrollo de bases de datos con Export de SqlPackage.exe. Vea ejemplos y los parámetros, las propiedades y las variables SQLCMD disponibles.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: 942d0c84e697654e67d48ce708792eb8bcf770aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081486"
---
# <a name="sqlpackage-export-parameters-and-properties"></a>Parámetros y propiedades de Export de SqlPackage
La acción Export de SqlPackage.exe exporta una base de datos conectada a un archivo BACPAC (.bacpac). De forma predeterminada, los datos de todas las tablas se incluirán en el archivo .bacpac. Si lo desea, puede especificar solo un subconjunto de las tablas para exportar los datos. La validación de la acción de exportación garantiza la compatibilidad de Azure SQL Database para toda la base de datos de destino, incluso si se especifica un subconjunto de tablas para la exportación. 

## <a name="command-line-syntax"></a>Sintaxis de línea de comandos

**SqlPackage.exe** inicia las acciones especificadas usando los parámetros, las propiedades y las variables de SQLCMD especificadas en la línea de comandos.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-export-action"></a>Parámetros de la acción Export

|Parámetro|Forma corta|Value|Descripción|
|---|---|---|---|
|**/Action:**|**/a**|Exportación|Especifica la acción que se va a realizar. |
|**/AccessToken:**|**/at**|{string}| Especifica el token de acceso de autenticación basada en tokens que se usará al conectarse a la base de datos de destino. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Especifica si la salida del registro de diagnóstico es la consola. El valor predeterminado es False. |
|**/ DiagnosticsFile:**|**/DF**|{string}|Especifica un archivo para almacenar los registros de diagnóstico. |
|**/MaxParallelism:**|**/mp**|{int}| Especifica el nivel de paralelismo para las operaciones simultáneas que se ejecutan en una base de datos. El valor predeterminado es 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Especifica si sqlpackage.exe debe sobrescribir los archivos existentes. Si se especifica False, sqlpackage.exe anula la acción si se encuentra un archivo existente. El valor predeterminado es True. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Especifica un par de nombre y valor para una propiedad específica de acción; {PropertyName}={Value}. Remítase a la ayuda de una acción determinada para ver los nombres de propiedad de esa acción. Ejemplo: sqlpackage.exe /Action:Export /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Especifica si se suprimen los comentarios detallados. El valor predeterminado es False.|
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
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Especifica si se debe usar la autenticación universal. Cuando se establece en true, el protocolo de autenticación interactiva se activa y admite MFA. Esta opción también se puede usar para la autenticación de Azure AD sin MFA, mediante un protocolo interactivo que requiere que el usuario escriba su nombre de usuario y contraseña o autenticación integrada (credenciales de Windows). Cuando/UniversalAuthentication se establece en True, no se puede especificar ninguna autenticación de Azure AD en SourceConnectionString (/scs). Cuando/UniversalAuthentication se establece en False, se debe especificar la autenticación de Azure AD en SourceConnectionString (/scs). <br/> Para más información sobre la autenticación universal de Active Directory, vea [Autenticación universal con SQL Database y Azure Synapse Analytics (compatibilidad de SSMS con MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

## <a name="properties-specific-to-the-export-action"></a>Propiedades específicas de la acción Export

|Propiedad|Value|Descripción|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Especifica el tiempo de espera de comando en segundos cuando se ejecutan consultas en SQL Server.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Especifica el tiempo de expiración de bloqueo de la base de datos en segundos al ejecutar consultas en SQLServer. Use -1 para esperar indefinidamente.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Especifica el tiempo de espera del comando de larga duración en segundos al ejecutar consultas en SQL Server. Use 0 para esperar indefinidamente.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Especifica el tipo de almacenamiento de seguridad para el modelo de esquema que se usa durante la extracción.|
|**/p:**|TableData=(STRING)|Indica la tabla de la que se extraerán los datos. Especifique el nombre de tabla con o sin corchetes en ambas partes del nombre en el siguiente formato: nombre_esquema.identificador_tabla. Esta opción se puede especificar varias veces.|
|**/p:**|TargetEngineVersion=({Default&#124;Latest&#124;V11&#124;V12} 'Latest')|Especifica la versión del motor de destino que se espera. Esto afecta a si se permiten objetos compatibles con los servidores de Azure SQL Database con capacidades de V12, como las tablas optimizadas para memoria, en el bacpac generado.|
|**/p:**|TempDirectoryForTableData=(STRING)|Especifica el directorio temporal que se usará para almacenar en búfer los datos de la tabla antes de escribirlos en el archivo de paquete.|
|**/p:**|VerifyFullTextDocumentTypesSupported=(BOOLEAN)|Especifica si los tipos de documentos de texto completo admitidos para Microsoft Azure SQL Database v12 deben comprobarse.|

## <a name="next-steps"></a>Pasos siguientes

- Mas información sobre [sqlpackage](sqlpackage.md)