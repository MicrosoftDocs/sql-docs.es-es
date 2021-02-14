---
title: Importación de SqlPackage
description: Obtenga información sobre cómo automatizar las tareas de desarrollo de bases de datos con Import de SqlPackage.exe. Vea ejemplos y los parámetros, las propiedades y las variables SQLCMD disponibles.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: cecc6de89a9e8f82a64942acb08af7ddee48c5be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081466"
---
# <a name="sqlpackage-import-parameters-and-properties"></a>Parámetros y propiedades de Import de SqlPackage
La acción Import de SqlPackage.exe importa los datos de esquemas y tablas de un paquete BACPAC (archivo .bacpac) en una base de datos nueva o vacía en SQL Server o Azure SQL Database. En el momento de la operación de importación a una base de datos existente, la base de datos de destino no puede contener ningún objeto de esquema definido por el usuario.  

## <a name="command-line-syntax"></a>Sintaxis de línea de comandos

**SqlPackage.exe** inicia las acciones especificadas usando los parámetros, las propiedades y las variables de SQLCMD especificadas en la línea de comandos.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-import-action"></a>Parámetros de la acción Import

|Parámetro|Forma corta|Value|Descripción|
|---|---|---|---|
|**/Action:**|**/a**|Importar|Especifica la acción que se va a realizar. |
|**/AccessToken:**|**/at**|{string}| Especifica el token de acceso de autenticación basada en tokens que se usará al conectarse a la base de datos de destino. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Especifica si la salida del registro de diagnóstico es la consola. El valor predeterminado es False. |
|**/ DiagnosticsFile:**|**/DF**|{string}|Especifica un archivo para almacenar los registros de diagnóstico. |
|**/MaxParallelism:**|**/mp**|{int}| Especifica el nivel de paralelismo para las operaciones simultáneas que se ejecutan en una base de datos. El valor predeterminado es 8. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Especifica un par de nombre y valor para una propiedad específica de acción; {PropertyName}={Value}. Remítase a la ayuda de una acción determinada para ver los nombres de propiedad de esa acción. Ejemplo: sqlpackage.exe /Action:Import /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Especifica si se suprimen los comentarios detallados. El valor predeterminado es False.|
|**/SourceFile:**|**/sf**|{string}|Especifica un archivo de origen que se va a usar como origen de la acción. Si se usa este parámetro, el resto de parámetros de origen no serán válidos. |
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

## <a name="properties-specific-to-the-import-action"></a>Propiedades específicas de la acción Import

|Propiedad|Value|Descripción|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Especifica el tiempo de espera de comando en segundos cuando se ejecutan consultas en SQL Server.|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Define la edición de la instancia de Azure SQL Database.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Especifica el tiempo de expiración de bloqueo de la base de datos en segundos al ejecutar consultas en SQLServer. Use -1 para esperar indefinidamente.|
|**/p:**|DatabaseMaximumSize=(INT32)|Define el tamaño máximo en GB de una instancia de Azure SQL Database.|
|**/p:**|DatabaseServiceObjective=(STRING)|Define el nivel de rendimiento de una instancia de Azure SQL Database, como "P0" o "S1".|
|**/p:**|ImportContributorArguments=(STRING)|Especifica los argumentos de colaborador de implementación para los colaboradores de implementación. Debe ser una lista de valores delimitada por punto y coma.|
|**/p:**|ImportContributors=(STRING)|Especifica los colaboradores de implementación que se deben ejecutar cuando se importa el dacpac. Debe ser una lista delimitada por punto y coma de nombres completos o identificadores de los colaboradores de compilación.|
|**/p:**|ImportContributorPaths=(STRING)|Especifica las rutas de acceso para cargar colaboradores de implementación adicionales. Debe ser una lista de valores delimitada por punto y coma. |
|**/p:**|LongRunningCommandTimeout=(INT32)| Especifica el tiempo de espera del comando de larga duración en segundos al ejecutar consultas en SQL Server. Use 0 para esperar indefinidamente.|
|**/p:**|Storage=({File&#124;Memory})|Especifica la forma en que se almacenan los elementos cuando se genera el modelo de base de datos. Por motivos de rendimiento, el valor predeterminado es InMemory. Cuando se trata de bases de datos grandes, se requiere almacenamiento respaldado por archivos.|
  

## <a name="next-steps"></a>Pasos siguientes

- Mas información sobre [sqlpackage](sqlpackage.md)