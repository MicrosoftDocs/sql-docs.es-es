---
title: Notas de la versión de DacFx y SqlPackage
description: Notas de la versión de sqlpackage de Microsoft.
ms.custom: tools|sos
ms.date: 02/02/2019
ms.prod: sql
ms.reviewer: alayu; sstein
ms.prod_service: sql-tools
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 1b93c290596cf9af23e4963912efff652e9c80ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061050"
---
# <a name="release-notes-for-sqlpackageexe"></a>Notas de la versión de SqlPackage.exe

**[Descargar la última versión](sqlpackage-download.md)**

En este artículo se enumeran las características y correcciones que ofrecen las versiones publicadas de SqlPackage.exe.

## <a name="186-sqlpackage"></a>18.6 sqlpackage

|Plataforma|Descargar|Fecha de la versión|Versión|Build
|:---|:---|:---|:---|:---|
|Windows|[Instalador MSI](https://go.microsoft.com/fwlink/?linkid=2143544)|18 de septiembre de 2020|18.6|15.0.4897.1|
|macOS .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2143659)|18 de septiembre de 2020| 18.6|15.0.4897.1|
|Linux .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2143497)|18 de septiembre de 2020| 18.6|15.0.4897.1|
|Windows .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2143496)|18 de septiembre de 2020| 18.6|15.0.4897.1|

### <a name="features"></a>Características
| Característica | Detalles |
| :------ | :------ |
| Plataforma | Se ha actualizado sqlpackage para .NET Core a la versión .NET Core 3.1. |
| Always Encrypted | Se ha agregado compatibilidad con la importación y exportación de enclave seguro para SQL Server 2019. |
| Implementación | Se ha agregado compatibilidad para omitir las tablas habilitadas para la captura de datos modificados al exportar desde Azure SQL Database. |
| Implementación | Se ha agregado compatibilidad con la opción de índice OPTIMIZE_FOR_SEQUENTIAL_KEY en Azure SQL Database. |
| Implementación | Se ha agregado compatibilidad con las columnas de identidad para Azure Synapse Analytics. | 
| Ayuda | Se ha generado la versión de sqlpackage en la ayuda (/?) y la compatibilidad con el parámetro /version. | 

### <a name="fixes"></a>Correcciones
| Característica | Detalles |
| :------ | :------ | 
| Implementación | Se ha corregido un script de implementación incorrecto generado al establecer como destino Azure SQL Managed Instance como un usuario que no es sysadmin.  | 
| Implementación | Se han corregido los colaboradores de implementación de carga al ejecutar acciones de script. | 
| Ayuda | Se ha generado un tiempo transcurrido correcto en sqlpackage cuando la operación tarda más de 1 día. | 
| Implementación | Se ha corregido el registro de dacpac al implementar para .NET Core. | 
| Implementación | Se ha corregido sqlpackage en el control de .NET Core del parámetro /accessToken (/at). | 
| Implementación | Se permiten las instrucciones ALTER TABLE en procedimientos almacenados como instrucciones de nivel no superior. | 
| Implementación | Se ha corregido la validación de las vistas materializadas de Azure Synapse Analytics para que no distinga entre mayúsculas y minúsculas. | 

### <a name="known-issues"></a>Problemas conocidos
| Característica | Detalles |
| :------ | :------ |
| Implementación | Todavía no se admite la característica Administración de cargas de trabajo de Azure Synapse Analytics (Grupos de cargas de trabajo y Clasificadores de cargas de trabajo). | 

## <a name="1851-sqlpackage"></a>sqlpackage 18.5.1

|Plataforma|Descargar|Fecha de la versión|Versión|Build
|:---|:---|:---|:---|:---|
|Windows|[Instalador MSI](https://go.microsoft.com/fwlink/?linkid=2134206)|24 de junio de 2020|18.5.1|15.0.4826.1|
|macOS .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2134312)|24 de junio de 2020| 18.5.1|15.0.4826.1|
|Linux .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2134311)|24 de junio de 2020| 18.5.1|15.0.4826.1|
|Windows .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2134310)|24 de junio de 2020| 18.5.1|15.0.4826.1|

### <a name="fixes"></a>Correcciones
| Característica | Detalles |
| :------ | :------ |
| Implementación | Se ha corregido una regresión que apareció en la versión 18.5 que provocaba un error "Sintaxis incorrecta cerca de "tipo"" al implementar un paquete DAC o importar un bacpac con un usuario con inicio de sesión externo en el entorno local. | 

## <a name="185-sqlpackage"></a>sqlpackage 18.5

|Plataforma|Descargar|Fecha de la versión|Versión|Build
|:---|:---|:---|:---|:---|
|Windows|[Instalador MSI](https://go.microsoft.com/fwlink/?linkid=2128142)|28 de abril de 2020|18.5|15.0.4769.1|
|macOS .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2128145)|28 de abril de 2020| 18.5|15.0.4769.1|
|Linux .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2128144)|28 de abril de 2020| 18.5|15.0.4769.1|
|Windows .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2128143)|28 de abril de 2020| 18.5|15.0.4769.1|

### <a name="features"></a>Características
| Característica | Detalles |
| :------ | :------ |
| Implementación | La clasificación de confidencialidad de datos ahora es compatible con SQL Server 2008 y versiones posteriores, Azure SQL Database y Azure Synapse Analytics |
| Implementación | Adición de compatibilidad de Azure Synapse Analytics para restricciones de tabla |
| Implementación | Adición de compatibilidad de Azure Synapse Analytics para el índice de almacén de columnas agrupado ordenado |
| Implementación | Adición de compatibilidad con el origen de datos externo (para Oracle, Teradata, MongoDB/CosmosDB, ODBC, clúster de macrodatos) y la tabla externa para el clúster de macrodatos de SQL Server 2019 |
| Implementación | Adición de la instancia de SQL Database Edge como edición compatible |
| Implementación | Compatibilidad con nombres de servidor de Instancia administrada con el formato "\<server>.\<dnszone>.base_de_datos.windows.net" |
| Implementación | Adición de compatibilidad con el comando copy en Azure Synapse Analytics |
| Implementación | Adición de la opción de implementación "IgnoreTablePartitionOptions" durante la publicación para evitar que se vuelva a crear la tabla cuando se produzcan cambios en la función de partición en la tabla para Azure Synapse Analytics |
| .NET Core | Adición de compatibilidad con Microsoft.Data.SqlClient en la versión de .NET Core de sqlpackage |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correcciones
| Fix | Detalles |
| :-- | :------ |
| Implementación | Corrección del análisis de la ruta de acceso JSON como expresión |
| Implementación | Corrección de la generación de instrucciones GRANT para los permisos AlterAnyDatabaseScopedConfiguration y AlterAnySensitivityClassification |
| Implementación | Corrección del permiso de script externo que no se reconoce |
| Implementación | Corrección de la propiedad insertada: la adición implícita de la propiedad no debe mostrarse en la diferencia, pero la mención explícita debe mostrarse a través del script |
| Implementación | Resolución de un problema que causaba que, al cambiar una tabla a la que se hacía referencia con una vista materializada (MV), se generasen instrucciones Alter View no admitidas en estas vistas en Azure Synapse Analytics |
| Implementación | Corrección de errores de publicación al agregar una columna a una tabla con datos en Azure Synapse Analytics |
| Implementación | Corrección del script de actualización que debería trasladar los datos a una nueva tabla al cambiar el tipo de columna de distribución (escenario de pérdida de datos) en Azure Synapse Analytics |
| ScriptDom | Corrección del error ScriptDom donde no se podían reconocer las restricciones insertadas definidas después de un índice insertado |
| ScriptDom | Corrección del paréntesis de cierre que faltaba en ScriptDom SYSTEM_TIME en una instrucción por lotes |
| Always Encrypted | Corrección de la tabla #tmpErrors que no se puede quitar si sqlpackage se vuelve a conectar y la tabla temporal ya no existe porque desaparece cuando la conexión termina |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemas conocidos
| Característica | Detalles |
| :------ | :------ |
| Implementación |  Ha aparecido una regresión en la versión 18.5 que provoca un error "Sintaxis incorrecta cerca de "tipo"" al implementar un paquete DAC o importar un bacpac con un usuario con inicio de sesión externo en el entorno local. La solución alternativa consiste en usar sqlpackage 18.4 y se corregirá en la siguiente versión de sqlpackage. | 
| .NET Core | Se produce un "Error irrecuperable de conexión interna" al importar archivos bacpac con la clasificación de confidencialidad debido a este [problema conocido](https://github.com/dotnet/SqlClient/issues/559) en Microsoft.Data.SqlClient. Este problema se solucionará en la próxima versión de sqlpackage. |
| &nbsp; | &nbsp; |

## <a name="1841-sqlpackage"></a>18.4.1 sqlpackage

|Plataforma|Descargar|Fecha de la versión|Versión|Build
|:---|:---|:---|:---|:---|
|Windows|[Instalador MSI](https://go.microsoft.com/fwlink/?linkid=2113703)|13 de diciembre de 2019|18.4.1|15.0.4630.1|
|macOS .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2113705)|13 de diciembre de 2019| 18.4.1|15.0.4630.1|
|Linux .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2113331)|13 de diciembre de 2019| 18.4.1|15.0.4630.1|
|Windows .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2113704)|13 de diciembre de 2019| 18.4.1|15.0.4630.1|

### <a name="fixes"></a>Correcciones
| Fix | Detalles |
| :-- | :------ |
| ScriptDom |  Se introdujo una regresión de análisis de ScriptDom en 18.3.1 donde "RENAME" se trata incorrectamente como un token de nivel superior, lo que provoca un error en el análisis.
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemas conocidos 

| Característica | Detalles |
| :------ | :------ |
| Implementación |  En la versión 18.4.1 se ha presentado una regresión que provoca que haya una "referencia a objeto no establecida en una instancia de un objeto". Error al implementar dacpac o al importar bacpac con un usuario con inicio de sesión externo. La solución alternativa consiste en usar sqlpackage 18.4 y se corregirá en la siguiente versión de sqlpackage. | 
| &nbsp; | &nbsp; |

## <a name="184-sqlpackage"></a>18.4 sqlpackage

|Plataforma|Descargar|Fecha de la versión|Versión|Build
|:---|:---|:---|:---|:---|
|Windows|[Instalador MSI](https://go.microsoft.com/fwlink/?linkid=2108813)|29 de octubre de 2019|18.4|15.0.4573.2|
|macOS .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2108815)|29 de octubre de 2019| 18.4|15.0.4573.2|
|Linux .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2108814)|29 de octubre de 2019| 18.4|15.0.4573.2|
|Windows .NET Core |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2109019)|29 de octubre de 2019| 18.4|15.0.4573.2|

### <a name="features"></a>Características

| Característica | Detalles |
| :------ | :------ |
| Implementación | Se agregó compatibilidad para implementar en Azure Synapse Analytics (GA). | 
| Plataforma | sqlpackage .NET Core en disponibilidad general para macOS, Linux y Windows. | 
| Seguridad | Se quitó la firma de código SHA1. |
| Implementación | Se agregó compatibilidad para las nuevas ediciones de base de datos de Azure: GeneralPurpose, BusinessCritical, Hyperscale |
| Implementación | Se agrega compatibilidad con Instancia administrada para el usuario y los grupos de Azure Active Directory. |
| Implementación | Compatibilidad con el parámetro/AccessToken para sqlpackage en .NET Core. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemas conocidos 

| Característica | Detalles |
| :------ | :------ |
| ScriptDom |  Se introdujo una regresión de análisis de ScriptDom en 18.3.1 donde "RENAME" se trata incorrectamente como un token de nivel superior, lo que provoca un error en el análisis. Este problema se solucionará en la próxima versión de sqlpackage. | 
| &nbsp; | &nbsp; |

### <a name="known-issues-for-net-core"></a>Problemas conocidos de .NET Core

| Característica | Detalles |
| :------ | :------ |
| Importar |  En el caso de los archivos. bacpac con archivos comprimidos con un tamaño superior a 4 GB, puede que tenga que usar la versión de .NET Core de sqlpackage para realizar la importación.  Este comportamiento se debe a que .NET Core genera encabezados zip que, aunque son válidos, no se pueden leer con la versión completa de .NET Framework de sqlpackage. | 
| Implementación | No se admite el parámetro /p: Storage=File. Solo se admite la memoria en .NET Core. | 
| Always Encrypted | sqlpackage .NET Core no admite columnas Always Encrypted. | 
| Seguridad | sqlpackage .NET Core no admite el parámetro /ua para autenticación multifactor. | 
| Implementación | No se admiten archivos .dacpac y .bacpac antiguos que utilizan la serialización de datos de json. |
| &nbsp; | &nbsp; |

## <a name="1831-sqlpackage"></a>18.3.1 sqlpackage

|Plataforma|Descargar|Fecha de la versión|Versión|Build
|:---|:---|:---|:---|:---|
|Windows|[Instalador MSI](https://go.microsoft.com/fwlink/?linkid=2102893)|13 de septiembre de 2019|18.3.1|15.0.4538.1|
|macOS .NET Core (versión preliminar)|[archivo zip](https://go.microsoft.com/fwlink/?linkid=2102894)|13 de septiembre de 2019| 18.3.1|15.0.4538.1|
|Linux .NET Core (versión preliminar)|[archivo zip](https://go.microsoft.com/fwlink/?linkid=2102978)|13 de septiembre de 2019| 18.3.1|15.0.4538.1|
|Windows .NET Core (versión preliminar)|[archivo zip](https://go.microsoft.com/fwlink/?linkid=2102979)|13 de septiembre de 2019| 18.13.1|15.0.4538.1|

### <a name="features"></a>Características

| Característica | Detalles |
| :------ | :------ |
| Implementación | Se agregó compatibilidad para implementar en Azure Synapse Analytics (versión preliminar). | 
| Implementación | Se agregó el parámetro /p:DatabaseLockTimeout=(INT32 '60') a sqlpackage. | 
| Implementación | Se agregó el parámetro /p:LongRunningCommandTimeout=(INT32) a sqlpackage. |
| Exportar/Extraer | Se agregó el parámetro /p:TempDirectoryForTableData=(STRING) a sqlpackage. |
| Implementación | Se permite que los colaboradores de implementación se carguen desde ubicaciones adicionales. Los colaboradores de implementación se cargarán desde el mismo directorio que el archivo. dacpac de destino que se está implementando, el directorio de extensiones relacionado con el binario sqlpackage.exe y el parámetro /p:AdditionalDeploymentContributorPaths=(STRING) agregado a sqlpackage donde se pueden especificar ubicaciones de directorio adicionales. |
| Implementación | Se agregó compatibilidad para OPTIMIZE_FOR_SEQUENTIAL_KEY. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correcciones

| Fix | Detalles |
| :-- | :------ |
| Implementación | Corrección para omitir los índices automáticos de modo que no se quiten en la implementación. | 
| Always Encrypted | Corrección para el control de las columnas VARCHAR de Always Encrypted. | 
| Compilación/Implementación | Corrección para resolver el método nodes() para los conjuntos de columnas XML.| 
| ScriptDom | Se corrigieron casos adicionales donde la cadena "URL" se interpretaba como un token de nivel superior. | 
| Grafo | Corrección del valor de TSQL generado para las referencias a pseudocolumnas en restricciones.  | 
| Exportación | Se generaron contraseñas aleatorias que cumplen los requisitos de complejidad. | 
| Implementación | Se corrigió la adhesión a los tiempos de espera de comandos al recuperar restricciones. | 
| .NET Core (versión preliminar) | Se corrigió el registro de diagnóstico en un archivo. | 
| .NET Core (versión preliminar) | Uso de streaming para exportar datos de tablas para admitir tablas grandes. | 
| &nbsp; | &nbsp; |

## <a name="182-sqlpackage"></a>sqlpackage 18.2

|Plataforma|Descargar|Fecha de la versión|Versión|Build
|:---|:---|:---|:---|:---|
|Windows|[Instalador MSI](https://go.microsoft.com/fwlink/?linkid=2087429)|15 de abril de 2019|18.2|15.0.4384.2|
|macOS .NET Core (versión preliminar)|[archivo zip](https://go.microsoft.com/fwlink/?linkid=2087247)|15 de abril de 2019 | 18.2 |15.0.4384.2|
|Linux .NET Core (versión preliminar)|[archivo zip](https://go.microsoft.com/fwlink/?linkid=2087431)|15 de abril de 2019 | 18.2 |15.0.4384.2|

### <a name="features"></a>Características

| Característica | Detalles |
| :------ | :------ |
| Grafo | Agregue compatibilidad con la tabla de grafos para las restricciones perimetrales y las cláusulas de restricciones perimetrales. |
| Implementación | Regla de validación de modelo habilitada para admitir 32 columnas de claves de índice para SQL Server 2016 y superiores. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correcciones

| Fix | Detalles |
| :-- | :------ |
| Implementación | Se corrigió la ingeniería inversa de una base de datos RTM de SQL Server 2016 debido a una sugerencia de consulta no compatible que se está utilizando. |
| Implementación | Se corrigió el orden de implementación de las instrucciones de alteración de cierre automático para que se produzcan antes de crear instrucciones de grupo de archivos. |
| ScriptDom | Se corrigió la regresión de análisis de ScriptDom donde la cadena "URL" se interpretaba como un token de nivel superior. |
| Implementación | Se corrigió una excepción de referencia nula al analizar una instrucción de índice de adición de tabla de alteración. | 
| Comparación de esquemas | Se corrigió la comparación de esquemas para las columnas calculadas persistentes que aceptan valores NULL que siempre se muestran diferentes.|
| &nbsp; | &nbsp; |

## <a name="181-sqlpackage"></a>18.1 sqlpackage

Fecha de lanzamiento: &nbsp; 1 de febrero de 2019  
Compilación: &nbsp; 15.0.4316.1  
Versión preliminar.

### <a name="features"></a>Características

| Característica | Detalles |
| :------ | :------ |
| Implementación | Se agregó compatibilidad con intercalaciones UTF8. |
| Implementación | Se habilitaron los índices de almacén de columnas no en clúster en una vista indexada. |
| Plataforma | Se movió a .NET Core 2.2. | 
| Comparación de esquemas | Utilice el almacenamiento con copia de seguridad de memoria para la comparación de esquemas en .NET Core. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correcciones

| Fix | Detalles |
| :-- | :------ |
| Rendimiento | Corrección de rendimiento para utilizar el estimador de cardinalidad heredado para consultas de ingeniería inversa. | 
| Rendimiento | Se corrigió un problema de rendimiento de comparación de esquema significativo al generar un script. | 
| Comparación de esquemas | Se corrigió la lógica de detección de desviación del esquema para ignorar ciertas sesiones de evento extendido (xevent). |
| Grafo | Se corrigió el orden de importación para las tablas de grafos. | 
| Exportación | Se corrigió la exportación de tablas externas con permisos de objeto. |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Problemas conocidos

Esta versión incluye las compilaciones de versión preliminar multiplataforma de sqlpackage que tienen como destino .NET Core 2.2. El sqlpackage puede ejecutarse en macOS y Linux.

| Problema conocido | Detalles |
| :---------- | :------ |
| Implementación | En .NET Core, los colaboradores de compilación y de implementación no se admiten. | 
| Implementación | En .NET Core, no se admiten archivos .dacpac y .bacpac antiguos que utilizan la serialización de datos de json. | 
| Implementación | En .NET Core, es posible que los archivos .dacpac a los que se hace referencia (por ejemplo, master.dacpac) no se resuelvan debido a problemas con sistemas de archivos que distinguen entre mayúsculas y minúsculas. | Una solución consiste en poner en mayúscula el nombre del archivo de referencia (por ejemplo MASTER.BACPAC). |
| &nbsp; | &nbsp; |

## <a name="180-sqlpackage"></a>18.0 sqlpackage

Fecha de lanzamiento: &nbsp; 24 de octubre de 2018  
Compilación: &nbsp; 15.0.4200.1

### <a name="features"></a>Características

| Característica | Detalles |
| :------ | :------ |
| Implementación | Se agregó compatibilidad para el nivel de compatibilidad de la base de datos 150. | 
| Implementación | Se agregó compatibilidad para instancias administradas. | 
| Rendimiento | Se agregó el parámetro de línea de comandos MaxParallelism para especificar el grado de paralelismo para las operaciones de base de datos. | 
| Seguridad | Se agregó el parámetro de línea de comandos AccessToken para especificar un token de autenticación al conectarse a SQL Server. | 
| Importar | Se agregó compatibilidad para tipos de datos BLOB y CLOB en secuencia para las importaciones. | 
| Implementación | Se agregó compatibilidad con la opción escalar "INLINE" de UDF. | 
| Grafo | Se agregó compatibilidad con la sintaxis "MERGE" de la tabla de grafos. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correcciones

| Fix | Detalles |
| :-- | :------ |
| Grafo | Se corrigió la pseudocolumna no resuelta para las tablas de grafos. |
| Implementación | Se corrigió la creación de una base de datos con grupos de archivos optimizados de memoria cuando se usan tablas optimizadas de memoria. |
| Implementación | Se corrigió la inclusión de propiedades extendidas en tablas externas. |
| &nbsp; | &nbsp; |

## <a name="178-sqlpackage"></a>17.8 sqlpackage

Fecha de lanzamiento: &nbsp; 22 de junio de 2018  
Compilación: &nbsp; 14.0.4079.2

### <a name="features"></a>Características

| Característica | Detalles |
| :------ | :------ |
| Diagnóstico | Se mejoraron los mensajes de error para errores de conexión, incluido el mensaje de excepción SqlClient. |
| Implementación | Compatibilidad con compresión de índices en índices de partición única para importación/exportación. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correcciones

| Fix | Detalles |
| :-- | :------ |
| Implementación | Se corrigió un problema de ingeniería inversa para conjuntos de columnas XML con SQL 2017 y versiones posteriores. | 
| Implementación | Se corrigió un problema donde se omitió el scripting en el nivel de compatibilidad de base de datos 140 para Azure SQL Database. |
| &nbsp; | &nbsp; |

## <a name="1741-sqlpackage"></a>17.4.1 sqlpackage

Fecha de lanzamiento: &nbsp; 25 de enero de 2018  
Compilación: &nbsp; 14.0.3917.1

### <a name="features"></a>Características

| Característica | Detalles |
| :------ | :------ |
| Import/Export | Se agregó el parámetro de línea de comandos ThreadMaxStackSize para analizar Transact-SQL con una gran cantidad de instrucciones anidadas. |
| Implementación | Compatibilidad con intercalación del catálogo de base de datos. | 
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correcciones

| Fix | Detalles |
| :-- | :------ |
| Importar | Al importar un archivo .bacpac de Azure SQL Database en una instancia local, se corrigieron los errores debidos a que las _claves maestras de base de datos sin la contraseña no se admiten en esta versión de SQL Server_. |
| Grafo | Se corrigió el error de pseudocolumna no resuelta para las tablas de grafos. |
| Comparación de esquemas | Se corrigió la autenticación de SQL para comparar esquemas. | 
| &nbsp; | &nbsp; |

## <a name="1740-sqlpackage"></a>17.4.0 sqlpackage

Fecha de lanzamiento: &nbsp; 12 de diciembre de 2017  
Compilación: &nbsp; 14.0.3881.1

### <a name="features"></a>Características

| Característica | Detalles |
| :------ | :------ |
| Implementación |  Se agregó soporte para la _directiva de retención temporal_ en SQL 2017+ y Azure SQL Database. | 
| Diagnóstico | Se agregó el parámetro de línea de comandos /DiagnosticsFile:"C:\Temp\sqlpackage.log "para especificar una ruta de archivo para guardar la información de diagnóstico. | 
| Diagnóstico | Se agregó el parámetro de línea de comandos /Diagnostics para registrar información de diagnóstico en la consola. |
| &nbsp; | &nbsp; |

### <a name="fixes"></a>Correcciones

| Fix | Detalles |
| :-- | :------ |
| Implementación | No bloquear cuando se encuentre un nivel de compatibilidad de base de datos que no entienda. En su lugar, se asumirá la versión más reciente de Azure SQL Database o la plataforma local. |
| &nbsp; | &nbsp; |
