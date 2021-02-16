---
title: Introducción a SQL Server Integration Services DevOps | Microsoft Docs
description: Aprenda a crear CICD en SSIS con las herramientas de DevOps de SSIS.
ms.date: 12/06/2019
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f44c8c3677f76ebc96b6e0385aa839a9e20aa2f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030613"
---
# <a name="sql-server-integration-services-ssis-devops-tools-azure-devops-extension"></a>Extensión de Azure DevOps para las herramientas de DevOps para SQL Server Integration Services (SSIS)

La extensión [SSIS DevOps Tools](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools) está disponible en el marketplace de **Azure DevOps**.

Si no dispone de ninguna organización de **Azure DevOps**, primero debe registrarse en [Azure Pipelines](/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops&preserve-view=true) y, después, agregar la extensión **SSIS DevOps Tools** siguiendo [estos pasos](/azure/devops/marketplace/overview?tabs=browser&view=azure-devops&preserve-view=true#add-an-extension).

Las **herramientas de DevOps de SSIS** incluyen la tarea **de compilación de SSIS**, la tarea de la versión **de implementación de SSIS** y la **tarea de configuración de catálogo de SSIS**.

- La tarea **[de compilación de SSIS](#ssis-build-task)** admite la generación de archivos dtproj en el modelo de implementación de proyectos o en el modelo de implementación de paquetes.

- La tarea **[de implementación de SSIS](#ssis-deploy-task)** admite la implementación de archivos ispac individuales o múltiples en el catálogo de SSIS local y en Azure-SSIS IR, o de archivos SSISDeploymentManifest y sus archivos asociados en un recurso compartido de archivos locales o de Azure.

- La tarea **[de configuración del catálogo de SSIS](#ssis-catalog-configuration-task)** permite configurar la carpeta, proyecto o entorno del catálogo de SSIS con un archivo de configuración en formato JSON. Esta tarea admite los escenarios siguientes:
    - Carpeta
        - Crear una carpeta.
        - Actualizar la descripción de la carpeta.
    - proyecto
        - Configurar el valor de los parámetros, se admiten tanto el valor literal como el valor al que se hace referencia.
        - Agregar referencias del entorno.
    - Entorno
        - Crear entorno.
        - Actualizar la descripción del entorno.
        - Crear o actualizar la variable de entorno.

## <a name="ssis-build-task"></a>Tarea de compilación de SSIS

![tarea de compilación](media/ssis-build-task.png)

### <a name="properties"></a>Propiedades

#### <a name="project-path"></a>Ruta de acceso del proyecto

Ruta de acceso de la carpeta o el archivo del proyecto que se va a compilar. Si se especifica una ruta de acceso de carpeta, la tarea de compilación de SSIS buscará todos los archivos dtproj de forma recursiva en esta carpeta y los compilará todos.

La ruta de acceso del proyecto no puede estar *vacía*; establézcala como **.** para realizar la compilación desde la carpeta raíz del repositorio.

#### <a name="project-configuration"></a>Configuración de proyecto

Nombre de la configuración del proyecto que se va a usar para la compilación. Si no se proporciona, el valor predeterminado es la primera configuración de proyecto definida en cada archivo dtproj.

#### <a name="output-path"></a>Ruta de acceso de resultados

Ruta de acceso de una carpeta independiente para guardar los resultados de la compilación, que se pueden publicar como artefactos de compilación a través de una [tarea de publicación de artefactos de compilación](/azure/devops/pipelines/tasks/utility/publish-build-artifacts?view=azure-devops&preserve-view=true).

### <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos

- La tarea de compilación de SSIS se basa en Visual Studio y en el diseñador de SSIS, que es obligatorio en los agentes de compilación. Por lo tanto, para ejecutar la tarea de compilación de SSIS en la canalización, debe elegir **vs2017-win2016** para los agentes hospedados en Microsoft o bien instalar Visual Studio y el diseñador de SSIS (ya sea VS2017 + SSDT2017 o VS2019 + la extensión de proyectos de SSIS) en agentes autohospedados.

- Para compilar proyectos de SSIS mediante el uso de componentes integrados (incluido Azure Feature Pack de SSIS y otros componentes de terceros), los componentes integrados deben instalarse en el equipo en el que se ejecuta el agente de canalización.  Para el agente hospedado por Microsoft, el usuario puede agregar una [tarea PowerShell Script](/azure/devops/pipelines/tasks/utility/powershell?view=azure-devops&preserve-view=true) o una [Command Line Script](/azure/devops/pipelines/tasks/utility/command-line?view=azure-devops&preserve-view=true) para descargar e instalar los componentes antes de que se ejecute la tarea de compilación de SSIS. A continuación se muestra el script de PowerShell de ejemplo para instalar Azure Feature Pack: 

```powershell
wget -Uri https://download.microsoft.com/download/E/E/0/EE0CB6A0-4105-466D-A7CA-5E39FA9AB128/SsisAzureFeaturePack_2017_x86.msi -OutFile AFP.msi

start -Wait -FilePath msiexec -Args "/i AFP.msi /quiet /l* log.txt"

cat log.txt
```

- En la tarea de compilación de SSIS no se admiten los niveles de protección **EncryptSensitiveWithPassword** y **EncryptAllWithPassword**. Asegúrese de que todos los proyectos de SSIS del código base no usan ninguno de estos dos niveles de protección, o la tarea de compilación de SSIS dejará de responder y agotará el tiempo de espera durante la ejecución.

## <a name="ssis-deploy-task"></a>Tarea de implementación de SSIS

![tarea de implementación](media/ssis-deploy-task.png)

### <a name="properties"></a>Propiedades

#### <a name="source-path"></a>Ruta de origen

Ruta de acceso de los archivos de origen ISPAC o SSISDeploymentManifest que quiere implementar. Esta ruta de acceso puede ser una ruta de acceso a una carpeta o a un archivo.

#### <a name="destination-type"></a>Tipo de destino

Tipo de destino. Actualmente, la tarea de implementación de SSIS admite dos tipos de destino:

- *Sistema de archivos*: los archivos SSISDeploymentManifest y sus archivos asociados se implementan en un sistema de archivos especificado. Se admiten recursos compartidos de archivos tanto locales como en Azure.
- *SSISDB*: los archivos ISPAC se implementan en un catálogo de SSIS específico, que se puede hospedar localmente en SQL Server o en Azure-SSIS Integration Runtime.

#### <a name="destination-server"></a>Servidor de destino

Nombre del servidor de SQL de destino. Puede ser el nombre de una instancia administrada de SQL Server, Azure SQL Database o Azure SQL Managed Instance local. Esta propiedad solo es visible cuando el tipo de destino es SSISDB.

#### <a name="destination-path"></a>Ruta de acceso de destino

Ruta de acceso de la carpeta de destino en la que se implementará el archivo de código fuente. Por ejemplo:

- /SSISDB/\<folderName\>
- \\\\\<machineName\>\\\<shareFolderName\>\\\<optionalSubfolderName\>

La tarea de implementación de SSIS creará la carpeta y la subcarpeta, si no existen.

#### <a name="authentication-type"></a>Tipo de autenticación

Tipo de autenticación para acceder al servidor de destino especificado. Esta propiedad solo es visible cuando el tipo de destino es SSISDB. En general, se admiten los tipos de autenticación siguientes:

- Autenticación de Windows
- Autenticación de SQL Server
- Active Directory - Contraseña
- Active Directory - Integrado

Pero el hecho de que se admita un tipo de autenticación concreto depende del tipo de servidor y del tipo de agente de destino. En la tabla siguiente se muestra la matriz de compatibilidad detallada.

|Tipo de servidor de destino|Agente hospedado por Microsoft|Agente autohospedado|
|---------|---------|---------|
|SQL Server local o máquina virtual |N/D|Autenticación de Windows|
|Azure SQL|Autenticación de SQL Server <br> Active Directory - Contraseña|Autenticación de SQL Server <br> Active Directory - Contraseña <br> Active Directory - Integrado|

#### <a name="domain-name"></a>Nombre de dominio

Nombre de dominio para acceder al sistema de archivos especificado. Esta propiedad solo es visible cuando el tipo de destino es Sistema de archivos.
Puede dejarla vacía cuando la cuenta de usuario que va a ejecutar el agente autohospedado haya obtenido acceso de lectura/escritura a la ruta de acceso al destino especificado.

#### <a name="username"></a>Nombre de usuario

Nombre de usuario para acceder al sistema de archivos especificado o a SSISDB. Esta propiedad está visible cuando el tipo de destino es el sistema de archivos o el tipo de autenticación es Autenticación de SQL Server o Active Directory - Contraseña.
Puede dejarla vacía cuando el tipo de destino sea el sistema de archivos y la cuenta de usuario que va a ejecutar el agente autohospedado haya obtenido acceso de lectura/escritura a la ruta de acceso al destino especificado.

#### <a name="password"></a>Contraseña

Contraseña para acceder al sistema de archivos especificado o a SSISDB. Esta propiedad está visible cuando el tipo de destino es el sistema de archivos o el tipo de autenticación es Autenticación de SQL Server o Active Directory - Contraseña.
Puede dejarla vacía cuando el tipo de destino sea el sistema de archivos y la cuenta de usuario que va a ejecutar el agente autohospedado haya obtenido acceso de lectura/escritura a la ruta de acceso al destino especificado.

#### <a name="overwrite-existing-projects-or-ssisdeploymentmanifest-files-of-the-same-names"></a>Sobreescritura de proyectos existentes o archivos SSISDeploymentManifest con los mismos nombres

Especifica si se sobrescriben los proyectos existentes o archivos SSISDeploymentManifest con los mismos nombres. En caso negativo, la tarea de implementación de SSIS omite la implementación de esos proyectos o archivos.

#### <a name="continue-deployment-when-error-occurs"></a>Implementación continua al producirse un error

Especifica si se continúa con la implementación de los proyectos o archivos restantes cuando se produce un error. En caso negativo, la tarea de implementación de SSIS se detiene inmediatamente cuando se produce un error.

### <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos

Actualmente, la tarea de implementación de SSIS no admite los escenarios siguientes:

- Configuración del entorno en el catálogo de SSIS.
- Implemente ISPAC en Azure SQL Server o Instancia administrada de Azure SQL, que solo admite la autenticación multifactor (MFA).
- Implementación de paquetes en MSDB o en el almacén de paquetes de SSIS.

## <a name="ssis-catalog-configuration-task"></a>Tarea de configuración del catálogo de SSIS

![Tarea de configuración del catálogo](media/ssis-catalog-configuation-task.png)

### <a name="properties"></a>Propiedades

#### <a name="configuration-file-source"></a>Origen del archivo de configuración

Origen del archivo JSON de configuración del catálogo de SSIS. Puede ser "Ruta de acceso al archivo" o "Insertado".

Consulte los detalles sobre cómo [definir el archivo JSON de configuración](#define-configuration-json):

- Consulte [un ejemplo de archivo JSON de configuración insertado](#a-sample-inline-configuration-json).
- Consulte [Esquema JSON](#json-schema).

#### <a name="configuration-json-file-path"></a>Ruta de acceso al archivo JSON de configuración

Ruta de acceso del archivo JSON de configuración del catálogo de SSIS. Esta propiedad solo es visible cuando se selecciona "Ruta de acceso al archivo" como origen del archivo de configuración.

Para usar [variables de canalización](/azure/devops/pipelines/process/variables) en el archivo JSON de configuración, debe agregar una [tarea de transformación de archivo](/azure/devops/pipelines/tasks/utility/file-transform?view=azure-devops&preserve-view=true) antes de esta para reemplazar los valores de configuración con variables de canalización. Para obtener más información, vea [Sustitución de variables JSON](/azure/devops/pipelines/tasks/transforms-variable-substitution?tabs=Classic&view=azure-devops&preserve-view=true#json-variable-substitution).

#### <a name="inline-configuration-json"></a>Archivo JSON de configuración insertado

Archivo JSON de configuración insertado del catálogo de SSIS. Esta propiedad solo es visible cuando se selecciona "Insertado" como origen del archivo de configuración. Las variables de canalización se pueden usar directamente.

#### <a name="roll-back-configuration-when-error-occurs"></a>Revertir la configuración cuando se produce un error

Indica si se va a revertir la configuración realizada por esta tarea cuando se produce un error.

#### <a name="target-server"></a>Servidor de destino

Nombre del servidor SQL de destino. Puede ser el nombre de una instancia administrada de SQL Server, Azure SQL Database o Azure SQL Managed Instance local.

#### <a name="authentication-type"></a>Tipo de autenticación

Tipo de autenticación para acceder al servidor de destino especificado. En general, se admiten los tipos de autenticación siguientes:

- Autenticación de Windows
- Autenticación de SQL Server
- Active Directory - Contraseña
- Active Directory - Integrado

Pero el hecho de que se admita un tipo de autenticación concreto depende del tipo de servidor y del tipo de agente de destino. En la tabla siguiente se muestra la matriz de compatibilidad detallada.

|Tipo de servidor de destino|Agente hospedado por Microsoft|Agente autohospedado|
|---------|---------|---------|
|SQL Server local o máquina virtual |N/D|Autenticación de Windows|
|Azure SQL|Autenticación de SQL Server <br> Active Directory - Contraseña|Autenticación de SQL Server <br> Active Directory - Contraseña <br> Active Directory - Integrado|

#### <a name="username"></a>Nombre de usuario

Nombre de usuario para acceder a la instancia de SQL Server de destino. Esta propiedad solo es visible cuando el tipo de autenticación es Autenticación de SQL Server o Active Directory - Contraseña.

#### <a name="password"></a>Contraseña

Contraseña para acceder a la instancia de SQL Server de destino. Esta propiedad solo es visible cuando el tipo de autenticación es Autenticación de SQL Server o Active Directory - Contraseña.

### <a name="define-configuration-json"></a>Definición del archivo JSON de configuración

El esquema JSON de configuración tiene tres niveles:

- catalog
- folder
- proyecto y entorno

![Esquema de configuración del catálogo](media/catalog-configuration-schema.png)

#### <a name="a-sample-inline-configuration-json"></a>Un ejemplo de archivo JSON de configuración insertado

```json
{
  "folders": [
    {
      "name": "devopsdemo",
      "description": "devops demo folder",
      "projects": [
        {
          "name": "catalog devops",
          "parameters": [
            {
              "name": "password",
              "container": "Package.dtsx",
              "value": "passwd",
              "valueType": "referenced"
            },
            {
              "name": "serverName",
              "container": "catalog devops",
              "value": "localhost",
              "valueType": "literal"
            }
          ],
          "references": [
            {
              "environmentName": "test",
              "environmentFolder": "devopsdemo"
            },
            {
              "environmentName": "test",
              "environmentFolder": "."
            }
          ]
        }
      ],
      "environments": [
        {
          "name": "test",
          "description": "test",
          "variables": [
            {
              "name": "passwd",
              "type": "string",
              "description": "",
              "value": "$(SSISDBServerAdminPassword)",
              "sensitive": true
            },
            {
              "name": "serverName",
              "type": "string",
              "description": "",
              "value": "$(TargetServerName)",
              "sensitive": false
            }
          ]
        }
      ]
    }
  ]
}
```

#### <a name="json-schema"></a>Esquema JSON

##### <a name="catalog-attributes"></a>Atributos del catálogo

|Propiedad  |Descripción  |Notas  |
|---------|---------|---------|
|carpetas  |Matriz de objetos de carpeta. Cada objeto contiene información de configuración de una carpeta de catálogo.|Vea *Atributos de carpeta* para obtener el esquema de un objeto de carpeta.|

##### <a name="folder-attributes"></a>Atributos de carpeta

|Propiedad.  |Descripción  |Notas  |
|---------|---------|---------|
|name  |Nombre de la carpeta del catálogo.|La carpeta se creará si no existe.|
|description|Descripción de la carpeta del catálogo.|Se omitirá el valor de *null*.|
|projects|Matriz de objetos de proyecto. Cada objeto contiene información de configuración de un proyecto.|Vea *Atributos del proyecto* para obtener el esquema de un objeto de proyecto.|
|environments|Matriz de objetos de entorno. Cada objeto contiene información de configuración de un entorno.|Vea *Atributos de entorno* para obtener el esquema de un objeto de entorno.|

##### <a name="project-attributes"></a>Atributos de proyecto

|Propiedad.  |Descripción  |Notas  |
|---------|---------|---------|
|name|Nombre del proyecto. |El objeto de proyecto se omitirá si el proyecto no existe en la carpeta principal.|
|parámetros|Matriz de objetos de parámetro. Cada objeto contiene información de configuración de un parámetro.|Vea *Atributos de parámetro* para obtener el esquema de un objeto de parámetro.|
|references|Matriz de objetos de referencia. Cada objeto representa una referencia de entorno al proyecto de destino.|Vea *Atributos de referencia* para obtener el esquema de un objeto de referencia.|

##### <a name="parameter-attributes"></a>Atributos de parámetro

|Propiedad.  |Descripción  |Notas  |
|---------|---------|---------|
|name|Nombre del parámetro.|<li>El parámetro puede ser un parámetro de proyecto o un parámetro de paquete. <li>El parámetro se omite si no existe. <li>Si el parámetro es una propiedad del administrador de conexiones, el nombre debe estar con el formato **CM.\<Connection Manager Name>.\<Property Name>** . |
|contenedor|Contenedor del parámetro.|<li>Si el parámetro es un parámetro de proyecto, *container* debe ser el nombre del proyecto. <li>Si es un parámetro de paquete, *container* debe ser el nombre del paquete con la extensión **.dtsx**.|
|value|Valor del parámetro.|<li>Cuando *valueType* es *referenced*: el valor es una referencia a una variable de entorno con el tipo de *cadena*. <li> Cuando *valueType* es *literal*: este atributo admite cualquier valor JSON *booleano*, de *número* y de *cadena* válido. <li> El valor se convertirá al tipo de parámetro de destino. Se producirá un error si no se puede convertir.<li> El valor de *null* no es válido. La tarea omitirá este objeto de parámetro y mostrará una advertencia.|
|valueType|Tipo del valor del parámetro.|Los tipos válidos son los siguientes: <br> *literal*: el atributo *value* representa un valor literal. <br> *referenced*: el atributo *value* es una referencia a una variable de entorno.|

##### <a name="reference-attributes"></a>Atributos de referencia

|Propiedad.  |Descripción  |Notas  |
|---------|---------|---------|
|environmentFolder|Nombre de carpeta del entorno.|La carpeta se creará si no existe. <br> El valor puede ser ".", que representa la carpeta principal del proyecto, que hace referencia al entorno.|
|environmentName|Nombre del entorno al que se hace referencia.|Si el entorno especificado no existe, se creará.|

##### <a name="environment-attributes"></a>Atributos de entorno

|Propiedad.  |Descripción  |Notas  |
|---------|---------|---------|
|name|Nombre del entorno.|Si el entorno no existe, se creará.|
|description|Descripción del entorno.|Se omitirá el valor de *null*.|
|variables|Matriz de objetos de variable.|Cada objeto contiene información de configuración para una variable de entorno. Vea *Atributos de variables* para obtener el esquema de un objeto de variable.|

##### <a name="variable-attributes"></a>Atributos de variables

|Propiedad  |Descripción  |Notas  |
|---------|---------|---------|
|name|Nombre de la variable de entorno.|Si la variable de entorno no existe, se creará.|
|type|Tipo de datos de la variable de entorno.|Los tipos válidos son los siguientes: <br> *boolean* <br> *byte* <br> *datetime* <br> Decimal <br> *double* <br> *int16* <br> *int32* <br> *int64* <br> *sbyte* <br> *single* <br> *string* <br> *uint32* <br> *uint64*|
|description|Descripción de la variable de entorno.|Se omitirá el valor de *null*.|
|value|Valor de la variable de entorno.|Este atributo admite cualquier valor JSON booleano, de número y de cadena válido.<br> El valor se convertirá al tipo especificado por el atributo **type**. Se producirá un error si no se puede realizar la conversión.<br>El valor de *null* no es válido. La tarea omitirá este objeto de variable de entorno y mostrará una advertencia.|
|sensitive|Si el valor de la variable de entorno es confidencial.|Las entradas válidas son: <br> *true* <br> *false*|

## <a name="release-notes"></a>Notas de la versión

### <a name="version-103"></a>Versión 1.0.3

Fecha de lanzamiento: 21 de octubre de 2020

- Permite especificar el sufijo de la cadena de conexión para la tarea de implementación de SSIS y la tarea de configuración del catálogo de SSIS.

### <a name="version-102"></a>Versión 1.0.2

Fecha de lanzamiento: 26 de mayo de 2020

- Se ha corregido un problema que hacía que se produjera un error en la tarea de configuración del catálogo de SSIS en algún caso después de realizar la configuración.

### <a name="version-101"></a>Versión 1.0.1

Fecha de lanzamiento: 9 de mayo de 2020

- Se corrigió un problema que haría que la tarea de compilación de SSIS compilara siempre toda la solución incluso si solo se especifica un único archivo dtproj como ruta de acceso del proyecto.

### <a name="version-100"></a>Versión 1.0.0

Fecha de lanzamiento: 8 de mayo de 2020

- Versión de disponibilidad general (GA).
- Se ha agregado una restricción de versión mínima de .NET Framework en el agente. La versión mínima requerida de .NET Framework es 4.6.2 actualmente.
- Se ha mejorado la descripción de las tareas de compilación de SSIS y de implementación de SSIS.

### <a name="version-020-preview"></a>Versión 0.2.0 Versión preliminar

Fecha de lanzamiento: 31 de marzo de 2020

- Se ha agregado la tarea de configuración del catálogo de SSIS.

### <a name="version-013-preview"></a>Versión 0.1.3 (versión preliminar)

Fecha de lanzamiento: 19 de enero de 2020

- Se corrigió un problema que impedía la implementación de ISPAC si se cambiaba el nombre de archivo original.

### <a name="version-012-preview"></a>Versión preliminar 0.1.2

Fecha de lanzamiento: 13 de enero de 2020

- Se ha agregado información más detallada sobre las excepciones en el registro de tareas de implementación de SSIS cuando el tipo de destino es SSISDB.
- Se ha corregido la ruta de destino de ejemplo en el texto de ayuda de la ruta de destino de la propiedad de la tarea de implementación de SSIS.

### <a name="version-011-preview"></a>Versión 0.1.1 (versión preliminar)

Fecha de lanzamiento: 6 de enero de 2020

- Se ha agregado una restricción de requisito de versión mínima del agente. Actualmente, la versión mínima del agente de este producto es 2.144.0.
- Se han corregido algunos textos de visualización incorrectos para la tarea de implementación de SSIS.
- Se han modificado algunos mensajes de error.

### <a name="version-010-preview"></a>Versión 0.1.0 (versión preliminar)

Fecha de lanzamiento: 5 de diciembre de 2019

Versión inicial de las herramientas de DevOps para SSIS. Se trata de una versión preliminar.

## <a name="next-steps"></a>Pasos siguientes

- Obtener la [extensión SSIS DevOps](https://marketplace.visualstudio.com/items?itemName=SSIS.ssis-devops-tools)
