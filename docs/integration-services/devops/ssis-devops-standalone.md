---
title: Herramientas independientes de DevOps para SQL Server Integration Services (SSIS) | Microsoft Docs
description: Aprenda a crear tareas CI/CD en SSIS con las herramientas independientes de DevOps para SSIS.
ms.date: 10/16/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a445e38d796113826f45199cf19b4f59a6e9dc16
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045105"
---
# <a name="standalone-sql-server-integration-service-ssis-devops-tools-preview"></a>Herramientas independientes de DevOps para SQL Server Integration Services (SSIS) (versión preliminar)

Las **herramientas independientes de DevOps para SSIS** proporcionan un conjunto de ejecutables para realizar tareas CI/CD en SSIS. Sin depender de la instalación del runtime de SSIS o Visual Studio, estos ejecutables se pueden integrar fácilmente con cualquier plataforma de CI/CD. Los ejecutables proporcionados son los siguientes:

- SSISBuild.exe: compile proyectos de SSIS en el modelo de implementación de proyectos o en el modelo de implementación de paquetes.
- SSISDeploy.exe: implemente archivos ISPAC en el catálogo de SSIS, o archivos DTSX y sus dependencias en el sistema de archivos.

## <a name="installation"></a>Instalación

Se requiere .NET Framework 4.6.2 o posterior.

Descargue el instalador más reciente desde el [Centro de descarga](https://aka.ms/AA9xp65); después, instálelo mediante el asistente o la línea de comandos:

- Instalación a través del asistente

Haga doble clic en el archivo .exe para iniciar la instalación y, a continuación, especifique una carpeta para extraer los ejecutables y los archivos de dependencia.

![Ubicación de la instalación](media/installation-location.png)

- Instalación mediante la línea de comandos

```
SSISDevOpsTools.exe /Q /C /T:<full path>
```

![Instalación mediante la línea de comandos](media/installation-command-line.png)


## <a name="ssisbuildexe"></a>SSISBuild.exe

**Sintaxis**

```
SSISBuild.exe -project|-p:<dtproj file path> [-configuration|-c:<configuration name>] [-projectPassword|-pp:<project password>] [-stripSensitive|-ss] [-output|-o:<output path>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**Parámetros**

|Parámetro|Descripción|
|---------|---------|
|-project \|-p:\<dtproj file path>|Ruta de acceso del archivo dtproj que se va a compilar.|
|-configuration\|-c:\<configuration name>|Nombre de la configuración del proyecto que se va a usar para la compilación. Si no se proporciona, el valor predeterminado es la primera configuración de proyecto definida en el archivo dtproj.|
|-projectPassword\|-pp:\<project password>|Contraseña del proyecto de SSIS y sus paquetes. Este argumento solo es válido cuando el nivel de protección del proyecto y los paquetes de SSIS es EncryptSensitiveWithPassword o EncryptAllWithPassword. En el modelo de implementación de paquetes, todos los paquetes deben compartir la misma contraseña especificada por este argumento.|
|-stripSensitive\|-ss|Convierta el nivel de protección del proyecto de SSIS en DontSaveSensitive. Cuando el nivel de protección es EncryptSensitiveWithPassword o EncryptAllWithPassword, el argumento -projectPassword debe estar correctamente establecido. Esta opción solo es válida para el modelo de implementación de proyectos.|
|-output\|-o:\<output path>|Ruta de acceso de salida del artefacto de compilación. El valor de este argumento sobrescribirá la ruta de acceso de salida predeterminada en la configuración del proyecto.|
|-log\|-l:\<log level>[;\<log path>]|Configuración relacionada con los registros. <li>nivel de registro: solo se escribirán en el archivo de registro los registros cuyo nivel de registro sea igual o superior. Hay cuatro niveles de registro (del más bajo al más alto), que son DIAG, INFO, WRN y ERR. Si no se especifica, el nivel de registro predeterminado es INFO. <li> ruta de acceso a los registros: Ruta de acceso del archivo para guardar los registros. No se generará el archivo de registro si no se especifica la ruta de acceso.|
|-quiet\|-q|No se muestra ningún registro en la salida estándar.|
|-help\|-h\|-?|Muestra información de uso detallada de esta utilidad de línea de comandos.|

**Ejemplos**

- Cree un archivo dtproj con la primera configuración de proyecto definida, sin cifrar, pero con contraseña:    
    ```
    SSISBuild.exe -p:"C:\projects\demo\demo.dtproj"
    ```

- Cree un archivo dtproj con la configuración "DevConfiguration", cifrado con contraseña, y genere los artefactos de compilación en una carpeta específica:
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -o:D:\folder
    ```

- Cree un archivo dtproj con la configuración "DevConfiguration", cifrado con contraseña, seccionando sus datos confidenciales y con el nivel de registro DIAG:
    ```
    SSISBuild.exe -p:C:\projects\demo\demo.dtproj -c:DevConfiguration -pp:encryptionpassword -ss -l:diag
    ```

## <a name="ssisdeployexe"></a>SSISDeploy.exe

**Sintaxis**

```
SSISDeploy.exe -source|-s:<source path> -destination|-d:<type>;<path>[;server] [-authType|-at:<auth type name>] [-connectionStringSuffix|-css:<connection string suffix>] [-projectPassword|-pp:<project password>] [-username|-u:<username>] [-password|-p:<password>] [-log|-l:<log level>[;<log path>]] [-quiet|-q] [-help|-h|-?]
```

**Parámetros**

|Parámetro|Descripción|
|---------|---------|
|-source\|-s:\<source path>|Ruta de acceso del archivo local de los artefactos que se van a implementar. Se permiten archivos ISPAC y DTSX, la ruta de acceso de la carpeta de los archivos DTSX y el archivo SSISDeploymentManifiest.|
|-destination\|-d:\<type>;\<path>[;server]|El tipo de destino, la ruta de acceso de la carpeta de destino y el nombre del servidor del catálogo de SSIS en el que se implementará el archivo de origen. Actualmente, se admiten los dos tipos de destino siguientes: <li> *CATALOG*: implemente uno o varios archivos ISPAC en el catálogo de SSIS especificado. La ruta de acceso del destino CATALOG debe tener el formato siguiente: <br> /SSISDB/\<folder name>[/\<project name>] <br> La parte <project name> opcional solo es válido cuando el origen especifica una única ruta de acceso al archivo ISPAC. Se debe especificar el nombre del servidor para el destino CATALOG. <li> *FILE*: implemente archivos o paquetes de SSIS especificados en uno o varios archivos SSISDeploymentManifest en la ruta de acceso especificada del sistema de archivos. La ruta de acceso del destino FILE puede ser una ruta de acceso de carpeta local o una ruta de acceso de carpeta de red con este formato: <br>\\\\\<machine name>\\\<folder name>[\\\<sub folder name>\...]|
|-authType\|-at:\<auth type name>|Tipo de autenticación para acceder a SQL Server. Obligatorio para el destino CATALOG. Se admiten los tipos siguientes: <li> WIN:  Autenticación de Windows <li> SQL:  Autenticación de SQL Server <li> ADPWD:  Active Directory - Contraseña <li> ADINT:  Active Directory - Integrado|
|-connectionStringSuffix\|-css:\<connection string suffix> |Sufijo de la cadena de conexión, que se utiliza para conectarse al catálogo de SSIS.|
|-projectPassword\|-pp:\<project password> |Contraseña para descifrar los archivos ISPAC o DTSX.|
|-username\|-u:\<username>  |Nombre de usuario para acceder al catálogo de SSIS o al sistema de archivos especificado. Se permite usar un prefijo con nombre de dominio para el acceso al sistema de archivos.|
|-password\|-p:\<password>  |Contraseña para acceder al catálogo de SSIS o al sistema de archivos especificado.|
|-log\|-l:\<log level>[;\<log path>] |Configuración relacionada con los registros para ejecutar esta utilidad. <li> nivel de registro: solo se escribirán en el archivo de registro los registros cuyo nivel de registro sea igual o superior. Hay cuatro niveles de registro (del más bajo al más alto), que son DIAG, INFO, WRN y ERR. Si no se especifica, el nivel de registro predeterminado es INFO. <li> ruta de acceso a los registros: Ruta de acceso del archivo para guardar los registros. No se generará el archivo de registro si no se especifica la ruta de acceso.|
|-quiet\|-q |No se muestran registros en la salida estándar.|
|-help\|-h\|-?  |Muestra información de uso detallada de esta utilidad de línea de comandos.|

**Ejemplos**

- Implemente un único archivo ISPAC sin cifrar con contraseña en el catálogo de SSIS con la autenticación de Windows.
    ```
    SSISDeploy.exe -s:D:\myfolder\demo.ispac -d:catalog;/SSISDB/destfolder;myssisserver -at:win
    ```

- Implemente un único archivo ISPAC cifrado con contraseña en el catálogo de SSIS con la autenticación de SQL y cambie el nombre del proyecto.
    ```
    SSISDeploy.exe -s:D:\myfolder\test.ispac -d:catalog;/SSISDB/folder/testproj;myssisserver -at:sql -u:sqlusername -p:sqlpassword -pp:encryptionpassword
    ```

- Implemente un único archivo SSISDeploymentManifest y sus archivos asociados en el recurso compartido de archivos de Azure.
    ```
    SSISDeploy.exe -s:D:\myfolder\mypackage.SSISDeploymentManifest -d:file;\\myssisshare.file.core.windows.net\destfolder -u:Azure\myssisshare -p:storagekey
    ```

- Implemente una carpeta de archivos DTSX en el sistema de archivos local.
    ```
    SSISDeploy.exe -s:D:\myfolder -d:file;\\myssisshare\destfolder
    ```

## <a name="release-notes"></a>Notas de la versión

### <a name="version-012-preview"></a>Versión preliminar 0.1.2

Fecha de lanzamiento: 14 de enero de 2021

- Se ha corregido un problema que provocaba un error de SSISBuild.exe al compilar el proyecto con la excepción NullReference cuando no coinciden los metadatos de parámetro de paquete en el archivo del proyecto SSIS y el paquete SSIS.
- Se ha corregido un problema que provocaba que el paquete no se ejecutara con un error que empieza por "No se pudo descifrar el nodo XML protegido", aunque el paquete se ha implementado correctamente en SSISDB con SSISDeploy.exe, cuando el proyecto de SSIS que contiene el paquete está cifrado con EncryptSensitiveWithUserKey y el paquete contiene CM con datos confidenciales.

### <a name="version-011-preview"></a>Versión 0.1.1 (versión preliminar)

Fecha de lanzamiento: 11 de noviembre de 2020

- Se ha corregido un problema que provocaba que SSISDeploy.exe no cargase un ensamblado al implementar ISPAC en el catálogo de SSIS.

### <a name="version-010-preview"></a>Versión 0.1.0 (versión preliminar)

Fecha de lanzamiento: 16 de octubre de 2020

Versión preliminar inicial de las herramientas independientes de DevOps para SSIS.

## <a name="next-steps"></a>Pasos siguientes

- Descargue las [herramientas independientes de DevOps para SSIS](https://aka.ms/AA9xp65).
- Si tiene alguna duda, consulte [estas preguntas y respuestas](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools&ssr=false#qna).
