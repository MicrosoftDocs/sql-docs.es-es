---
description: Opciones de la línea de comandos en la consola SSMA (SybaseToSQL)
title: Opciones de la línea de comandos en la consola SSMA (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Command Line Options
ms.assetid: 337cbd26-67b7-4c88-9deb-d0a69a3d7714
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 409f5bcfaa779177ca7b7f7c85f4911d3b889d93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063140"
---
# <a name="command-line-options-in-ssma-console-sybasetosql"></a>Opciones de la línea de comandos en la consola SSMA (SybaseToSQL)
Microsoft proporciona un conjunto sólido de opciones de línea de comandos para ejecutar y controlar las actividades de SSMA. Las secciones siguientes detallan el mismo.  
  
## <a name="command-line-options-in-ssma-console"></a>Opciones de la línea de comandos en la consola SSMA  
Aquí se describen las opciones del comando de la consola.  
  
En esta sección, el término ' opción ' también se conoce como ' modificador '.  
  
-   Las opciones no distinguen mayúsculas de minúsculas y pueden comenzar con un **-** carácter ' ' o ' **/** '.  
  
-   Si se especifican opciones, es obligatorio especificar los parámetros de opción correspondientes.  
  
-   Los parámetros de opción deben separarse del carácter de opción mediante un espacio en blanco.  
  
    **Ejemplos de sintaxis:**  
  
    `C:\> SSMAforSybaseConsole.EXE -s scriptfile`  
  
    `C:\> SSMAforSybaseConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\AssessmentReportGenerationSample.xml" -v "C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\VariableValueFileSample.xml" -c "C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ServersConnectionFileSample.xml"`  
  
-   Los nombres de carpeta o archivo que contienen espacios deben especificarse entre comillas dobles.  
  
-   La salida de las entradas de línea de comandos y los mensajes de error se almacenan en STDOUT o en un archivo especificado.  
  
### <a name="script-file-option--sscript"></a>Opción de archivo de script:-s/script  
Un modificador obligatorio, el nombre o la ruta de acceso del archivo de script especifica el script de secuencias de comandos que SSMA debe ejecutar.  
  
**Ejemplos de sintaxis:**  
  
`C:\>SSMAforSybaseConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="variable-value-file-option--vvariable"></a>Opción de archivo de valor de variable:-v/variable  
Este archivo incluye las variables que se usan en el archivo de script. Este es un modificador opcional. Si las variables no se declaran en el archivo de variables y se usan en el archivo de script, la aplicación genera un error y finaliza la ejecución de la consola.  
  
**Ejemplos de sintaxis:**  
  
-   Variables definidas en varios archivos de valores de variable, quizás una con un valor predeterminado y otra con un valor específico de la instancia cuando sea aplicable. El último archivo de variables especificado en los argumentos de la línea de comandos toma la preferencia, en caso de que haya una duplicación de variables:  
  
    `C:\>SSMAforSybaseConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ConversionAndDataMigrationSample.xml" -v c:\migration`  
  
    `projects\global_variablevaluefile.xml -v "c:\migrationprojects\instance_variablevaluefile.xml"`  
  
### <a name="server-connection-file-option--cserverconnection"></a>Opción de archivo de conexión de servidor:-c/serverconnection  
Este archivo contiene información de conexión de servidor para cada servidor. Cada definición de servidor se identifica mediante un identificador de servidor único. Se hace referencia a los identificadores de servidor en el archivo de script para los comandos relacionados con la conexión.  
  
La definición del servidor puede formar parte del archivo de conexión del servidor o del archivo de script. El ID. del servidor en el archivo de script tiene prioridad sobre el archivo de conexión del servidor, en caso de que haya una duplicación del ID. del servidor.  
  
**Ejemplos de sintaxis:**  
  
-   Los identificadores de servidor se usan en el archivo de script y se definen en un archivo de conexión de servidor independiente, el archivo de conexión de servidor usa las variables que se definen en el archivo de valores de variable:  
  
    `C:\>SSMAforSybaseConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -v`  
  
    `c:\SsmaProjects\myvaluefile1.xml -c`  
  
    `c:\SsmaProjects\myserverconnectionsfile1.xml`  
  
-   La definición del servidor se inserta en el archivo de script:  
  
    `C:\>SSMAforSybaseConsole.EXE -s "C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ConversionAndDataMigrationSample.xml"`  
  
### <a name="xml-output-option--xxmloutput-xmloutputfile"></a>Opción de salida XML:-x/xmloutput [xmloutputfile]  
Este comando se usa para generar los mensajes de salida del comando en formato XML, ya sea en la consola o en un archivo XML.  
  
Hay dos opciones disponibles para xmloutput, es decir,...:  
  
-   Si la ruta de acceso se proporciona después del modificador xmloutput, el resultado se redirige al archivo.  
  
    **Ejemplo de sintaxis:**  
  
    `C:\>SSMAforSybaseConsole.EXE -s`  
  
    `"C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -x d:\xmloutput\project1output.xml`  
  
-   Si no se proporciona FilePath después del modificador xmloutput, el xmlout se muestra en la consola.  
  
    **Ejemplo de sintaxis:**  
  
    `C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -xmloutput`  
  
### <a name="log-file-option--llog"></a>Opción de archivo de registro:-l/log  
Todas las operaciones de SSMA en la aplicación de consola se registran en un archivo de registro. Este es un modificador opcional. Si se especifica un archivo de registro y su ruta de acceso en la línea de comandos, el registro se genera en la ubicación especificada. De lo contrario, se genera en su ubicación predeterminada.  
  
**Ejemplo de sintaxis:**  
  
`C:\>SSMAforSybaseConsole.EXE`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -l c:\SsmaProjects\migration1.log`  
  
### <a name="project-environment-folder-option--eprojectenvironment"></a>Opción de carpeta de entorno de proyecto:-e/projectenvironment  
Esto denota la carpeta de configuración del entorno de proyecto para el proyecto SSMA actual. Este modificador es opcional.  
  
**Ejemplo de sintaxis:**  
  
`C:\>SSMAforSybaseConsole.EXE -s`  
  
`"C:\Program Files\Microsoft SQL Server Migration Assistant for Sybase\Sample Console Scripts\ConversionAndDataMigrationSample.xml"  -e c:\SsmaProjects\CommonEnvironment`  
  
### <a name="secure-password-option--psecurepassword"></a>Opción de contraseña segura:-p/securepassword  
Esta opción indica la contraseña cifrada para las conexiones del servidor. Difiere del resto de opciones: la opción no ejecuta ningún script ni ayuda en ninguna actividad relacionada con la migración, pero ayuda a administrar el cifrado de contraseñas para las conexiones de servidor usadas en el proyecto de migración.  
  
No se puede especificar ninguna otra opción o contraseña como parámetro de la línea de comandos. De lo contrario, se producirá un error. Para obtener más información, consulte la sección [Administración de contraseñas](managing-passwords-sybasetosql.md) .  
  
Se admiten las siguientes subopciones para `-p/securepassword` :  
  
-   Para agregar una contraseña al almacenamiento protegido para un identificador de servidor especificado o para todos los identificadores de servidor definidos en el archivo de conexión de servidor. La opción-overwrite, que aparece a continuación, actualiza la contraseña si ya existe:  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}` `-c|-serverconnection <server-connection-file> [-v|variable <variable-value-file>]``[-o|overwrite]`  
  
    `-p|-securepassword -a|add    {"<server_id>[, .n]"|all}``-s|-script <server-connection-file> [-v|variable <variable-value-file>] [-o|overwrite]`  
  
-   Para quitar la contraseña cifrada del almacenamiento protegido del identificador de servidor especificado o para todos los identificadores de servidor:  
  
    `-p/securepassword -r/remove {<server_id> [, ...n] | all}`  
  
-   Para mostrar una lista de los identificadores de servidor para los que se cifra la contraseña:  
  
    `-p/securepassword -l/list`  
  
-   Exportar las contraseñas almacenadas en el almacenamiento protegido a un archivo cifrado. Este archivo está cifrado con la frase de contraseña especificada por el usuario.  
  
    `-p/securepassword -e/export {<server-id> [, ...n] | all} <encrypted-password -file>`  
  
-   El archivo cifrado que se exportó anteriormente se importa en el almacenamiento protegido local mediante la frase de contraseña especificada por el usuario. Una vez descifrado el archivo, se almacena en un nuevo archivo que, a su vez, se cifra en el equipo local.  
  
    `-p/securepassword -i/import {<server-id> [, ...n] | all} <encrypted-password -file>`  
  
    Se pueden especificar varios identificadores de servidor mediante separadores de comas.  
  
### <a name="help-option--help"></a>Opción de ayuda:-?/help  
Muestra el Resumen de la sintaxis de las opciones de la consola de SSMA:  
  
`C:\>SSMAforSybaseConsole.EXE -?`  
  
Para ver una presentación tabular de las opciones de la línea de comandos de la consola de SSMA, consulte [Apéndice-1 &#40;SybaseToSQL&#41;](../../ssma/sybase/appendix-1-sybasetosql.md).  
  
### <a name="securepassword-help-option--securepassword--help"></a>Opción de ayuda de SecurePassword:-SecurePassword-?/Help  
Muestra el Resumen de la sintaxis de las opciones de la consola de SSMA:  
  
`C:\>SSMAforSybaseConsole.EXE -securepassword -?`  
  
Para ver una presentación tabular de las opciones de la línea de comandos de la consola de SSMA, consulte [Apéndice-1 &#40;SybaseToSQL&#41;](../../ssma/sybase/appendix-1-sybasetosql.md)  
  
### <a name="next-step"></a>siguiente paso  
El paso siguiente depende de los requisitos del proyecto:  
  
-   Para especificar una contraseña o exportar o importar contraseñas, consulte [Administración de contraseñas &#40;SybaseToSQL&#41;](../../ssma/sybase/managing-passwords-sybasetosql.md).  
  
-   Para generar informes, consulte [generación de informes &#40;SybaseToSQL&#41;](../../ssma/sybase/generating-reports-sybasetosql.md).  
  
-   Para solucionar problemas de la consola de, consulte solución de problemas de [&#40;SybaseToSQL&#41;](../../ssma/sybase/troubleshooting-sybasetosql.md).  
  
