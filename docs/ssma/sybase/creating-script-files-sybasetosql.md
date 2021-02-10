---
description: Creación de archivos de script (SybaseToSQL)
title: Crear archivos de script (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Configuring Settings
- Sybase Console,Script Commands
- Sybase Console,Script File Validation
- Sybase Console,Server Connection Parameters
ms.assetid: e6baf106-abbd-4200-b3de-33b4b4f1b294
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b3f417752ef7de6ab5c4fef077eb9771d7454996
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057200"
---
# <a name="creating-script-files-sybasetosql"></a>Creación de archivos de script (SybaseToSQL)
El primer paso antes de iniciar la aplicación de consola SSMA es crear el archivo de script y, si es necesario, crear el archivo de valores de variable y el archivo de conexión de servidor.  
  
El archivo de script se puede dividir en tres secciones, es decir,...:  
  
1.  **Configuración:** Permite al usuario establecer los parámetros de configuración para la aplicación de consola.  
  
2.  **servidores:** Permite al usuario establecer las definiciones de servidor de origen o de destino. También puede encontrarse en un archivo de conexión de servidor independiente.  
  
3.  **script: comandos:** Permite al usuario ejecutar comandos de flujo de trabajo de SSMA.  
  
Cada sección se describe en detalle a continuación:  
  
## <a name="configuring-sybase-console-settings"></a>Configuración de la consola de Sybase  
Las configuraciones de un script se muestran en el archivo de script de la consola.  
  
Si alguno de los elementos se especifica en el nodo de configuración, se establecen como la configuración global, es decir, se aplican a todos los comandos de script. Estos elementos de configuración también se pueden establecer dentro de cada comando en la sección script-Command si el usuario desea invalidar la configuración global.  
  
Las opciones configurables por el usuario incluyen:  
  
1.  **Proveedor de ventana de salida:** Si el atributo Suppress-messages se establece en "true", los mensajes específicos del comando no se muestran en la consola. A continuación se proporciona la descripción de los atributos:  
  
    -   Destination: especifica si la salida debe imprimirse en un archivo o en stdout. De forma predeterminada, es false.  
  
    -   nombre-archivo: la ruta de acceso del archivo (opcional).  
  
    -   Suprimir mensajes: suprime mensajes en la consola. Es "false" de forma predeterminada.  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <output-window  
  
        suppress-messages="<true/false>"   (optional)  
  
        destination="<file/stdout>"        (optional)  
  
        file-name="<file-name>"            (optional)  
  
       />  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <output-window  
  
         suppress-messages="<true/false>"   (optional)  
  
         destination="<file/stdout>"        (optional)  
  
         file-name="<file-name>"            (optional)  
  
       />  
  
    </...All commands...>  
    ```  
  
2.  **Proveedor de conexión de migración de datos:** Esto especifica el servidor de origen o de destino que se va a considerar para la migración de datos.  Source-use-Last-Used indica que el último servidor de origen usado se usa para la migración de datos. Similar de destino-uso-último: indica que el último servidor de destino utilizado se usa para la migración de datos. El usuario también puede especificar el servidor (origen o destino) mediante los atributos Source-Server o Target-Server.  
  
    Solo se puede usar uno u otro atributo especificado, es decir:  
  
    -   Source-use-Last-Used = "true" (valor predeterminado) o Source-Server = "source_servername"  
  
    -   Target-use-Last-Used = "true" (valor predeterminado) o target-server = "target_servername"  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <data-migration-connection   source-use-last-used="true"  
  
                                   target-server="<target-server-unique-name>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <migrate-data>  
  
      <data-migration-connection   source-server="<source-server-unique-name>"  
  
                                   target-use-last-used="true"/>  
  
    </migrate-data>  
    ```  
  
3.  **Elemento emergente de entrada del usuario:** Esto permite controlar los errores, cuando los objetos se cargan desde la base de datos. El usuario proporciona los modos de entrada y, en caso de error, la consola continúa a medida que el usuario especifica.  
  
    Los modos incluyen:  
  
    -   **Ask-usuario-** Solicita al usuario que continúe (' sí ') o un error (' no ').  
  
    -   **error:** La consola muestra un error y detiene la ejecución.  
  
    -   **continuar:** La consola continúa con la ejecución.  
  
    El modo predeterminado es **error**.  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <!-- Connect to target database -->  
  
    <connect-target-database server="<target-server-unique-name>">  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </connect-target-database>  
    ```  
  
4.  **Volver a conectar el proveedor:** Esto permite al usuario establecer la configuración de reconexión en caso de que se produzcan errores de conexión. Se puede establecer para los servidores de origen y de destino.  
  
    Los modos de reconexión son:  
  
    -   reconexión a último uso-servidor: Si la conexión no está activa, intenta volver a conectarse al último servidor usado como máximo 5 veces.  
  
    -   generar un error: Si la conexión no está activa, se genera un error.  
  
    El modo predeterminado es **Generate-an-error**.  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <reconnect-manager  on-source-reconnect="<reconnect-to-last-used-server/generate-an-error>"  
  
                          on-target-reconnect="<reconnect-to-last-used-server/generate-an-error>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <!--synchronization-->  
  
    <synchronize-target>  
  
      <reconnect-manager on-target-reconnect="reconnect-to-last-used-server"/>  
  
    </synchronize-target>  
    ```  
    *or*  
  
    ```xml  
    <!--data migration-->  
  
    <migrate-data server="<target-server-unique-name>">  
  
      <reconnect-manager  
  
        on-source-reconnect="reconnect-to-last-used-server"  
  
        on-target-reconnect="generate-an-error"/>  
  
    </migrate-data>  
    ```  
  
5.  **Proveedor de sobrescritura de convertidor:** Esto permite al usuario controlar los objetos que ya están presentes en la metabase de destino. Entre las posibles acciones se incluyen:  
  
    -   error: la consola muestra un error y detiene la ejecución.  
  
    -   overwrite: sobrescribe los valores de objeto existentes. Esta acción se realiza de forma predeterminada.  
  
    -   omitir: la consola de omite los objetos que ya existen en la base de datos  
  
    -   Ask-User: solicita al usuario una entrada ("Yes"/"no")  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <object-overwrite action="<error/skip/overwrite/ask-user>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <convert-schema object-name="<object-name>">  
  
      <object-overwrite action="<error/skip/overwrite/ask-user>"/>  
  
    </convert-schema>  
    ```  
  
6.  **Error de proveedor de requisitos previos:** Esto permite al usuario controlar los requisitos previos necesarios para procesar un comando. De forma predeterminada, el modo STRICT es ' false '. Si se establece en "true", se genera una excepción para que no cumpla los requisitos previos.  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <prerequisites strict-mode="<true/false>"/>  
  
    </output-providers>  
    ```  
  
7.  **Detener operación:** Durante la operación intermedia, si el usuario desea detener la operación, se puede usar la tecla de lectura rápida **' Ctrl + C '** . SSMA para la consola de Sybase esperará a que se complete la operación y finalizará la ejecución de la consola.  
  
    Si el usuario desea detener la ejecución inmediatamente, se puede volver a presionar la tecla de **"Ctrl + C"** para la finalización brusca de la aplicación de consola SSMA  
  
8.  **Proveedor de progreso:** Informa del progreso de cada comando de la consola. Esta opción está deshabilitada de manera predeterminada. Los atributos de informe de progreso comprenden:  
  
    -   apagado  
  
    -   cada-1%  
  
    -   cada-2%  
  
    -   cada-5%  
  
    -   cada-10%  
  
    -   cada-20%  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <progress-reporting enable="<true/false>"           (optional)  
  
                          report-messages="<true/false>"  (optional)  
  
                          report-progress="every-1%/every-2%/every-5%/every-10%/every-20%/off" (optional)/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <progress-reporting  
  
        enable="<true/false>"          (optional)  
  
        report-messages="<true/false>" (optional)  
  
        report-progress="every-1%/every-2%/every-5%/every-10%/every-20%/off (optional)/>  
  
    </...All commands...>  
    ```  
  
9. Nivel de **detalle del registrador:** Establece el nivel de detalle del registro. Esto corresponde a la opción todas las categorías de la interfaz de usuario. De forma predeterminada, el nivel de detalle del registro es "error".  
  
    Las opciones de nivel de registrador incluyen:  
  
    -   error irrecuperable: solo se registran los mensajes de error irrecuperable.  
  
    -   error: solo se registran los mensajes de error y graves.  
  
    -   ADVERTENCIA: se registran todos los niveles excepto los mensajes de depuración e información.  
  
    -   info: se registran todos los niveles excepto los mensajes de depuración.  
  
    -   depurar: todos los niveles de mensajes registrados.  
  
    > [!NOTE]  
    > Los mensajes obligatorios se registran en cualquier nivel.  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <log-verbosity level="fatal-error/error/warning/info/debug"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <log-verbosity level="fatal-error/error/warning/info/debug"/>  
  
    </...All commands...>  
    ```  
  
10. **Invalidar contraseña cifrada:** Si es "true", la contraseña de texto no cifrado especificada en la sección definición de servidor del archivo de conexión de servidor o en el archivo de script invalida la contraseña cifrada almacenada en el almacenamiento protegido, si existe. Si no se especifica ninguna contraseña en texto no cifrado, se le pedirá al usuario que escriba la contraseña.  
  
    Aquí aparecen dos casos:  
  
    1.  Si la opción de invalidación es **false**, el orden de búsqueda será almacenamiento protegido-archivo de &gt; conexión del servidor de archivos de script: preguntar al &gt; &gt; usuario.  
  
    2.  Si la opción de invalidación es **true**, el orden de búsqueda será archivo de conexión de script de servidor de archivos &gt; : &gt; preguntar al usuario.  
  
    **Ejemplo**:  
  
    ```xml  
    <output-providers>  
  
      <encrypted-password override="<true/false>"/>  
  
    </output-providers>  
    ```  
  
La opción no configurable es:  
  
-   **Número máximo de intentos de reconexión:** Cuando una conexión establecida agota el tiempo de espera o se interrumpe debido a un error de la red, es necesario volver a conectar el servidor. Los intentos de reconexión pueden tener un máximo de **5** reintentos después de lo cual, la consola realiza automáticamente la reconexión. La instalación de la reconexión automática reduce el esfuerzo de reejecutar el script.  
  
## <a name="server-connection-parameters"></a>Parámetros de conexión del servidor  
Los parámetros de conexión de servidor se pueden definir en el archivo de script o en el archivo de conexión de servidor. Consulte la sección [creación de archivos de conexión de servidor &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md) para obtener más detalles.  
  
## <a name="script-commands"></a>Comandos de script  
El archivo de script contiene una secuencia de comandos de flujo de trabajo de migración en formato XML. La aplicación de consola SSMA procesa la migración en el orden de los comandos que aparecen en el archivo de script.  
  
Por ejemplo, una migración de datos típica de una tabla específica en una base de datos de Sybase sigue la jerarquía de: Database- &gt; Schema- &gt; TABLE.  
  
Cuando todos los comandos del archivo de script se ejecutan correctamente, la aplicación de consola SSMA se cierra y devuelve el control al usuario. El contenido de un archivo de script es más o menos estático con información variable contenida en un archivo de [valores de variable](creating-variable-value-files-sybasetosql.md) o, en una sección independiente del archivo de script para valores de variable.  
  
**Ejemplo**:  
  
```xml  
<!--Sample of script file commands -->  
  
<ssma-script-file>  
  
  <script-commands>  
  
    <create-new-project project-folder="<project-folder>"  
  
                        project-name="<project-name>"  
  
                        overwrite-if-exists="<true/false>"/>  
  
    <connect-source-database server="<source-server-unique-name>"/>  
  
    <save-project/>  
  
    <close-project/>  
  
  </script-commands>  
  
</ssma-script-file>  
```  
Las plantillas que constan de 3 archivos de script (para ejecutar varios escenarios), un archivo de valores de variable y un archivo de conexión de servidor se proporcionan en la carpeta scripts de la consola de ejemplo del directorio del producto:  
  
-   AssessmentReportGenerationSample.xml  
  
-   ConversionAndDataMigrationSample.xml  
  
-   SqlStatementConversionSample.xml  
  
-   VariableValueFileSample.xml  
  
-   ServersConnectionFileSample.xml  
  
Puede ejecutar las plantillas (archivos) después de cambiar los parámetros que se muestran en la relevancia.  
  
Puede encontrar una lista completa de los comandos de script en [la ejecución de la consola de SSMA &#40;SybaseToSQL&#41;](../../ssma/sybase/executing-the-ssma-console-sybasetosql.md)  
  
## <a name="script-file-validation"></a>Validación de archivos de script  
El usuario puede validar fácilmente su archivo de script con el archivo **de definición de esquema 2ssconsolescriptschema. xsd '** disponible en la carpeta ' schemas '.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso en el funcionamiento de la consola es la [creación de archivos de valor Variable &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Crear archivos de valores de variable &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md)  
  
