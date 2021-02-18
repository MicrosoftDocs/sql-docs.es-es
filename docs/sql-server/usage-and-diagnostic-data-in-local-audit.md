---
title: Auditoría local para la colección de datos de uso y diagnóstico
description: Obtenga información sobre la auditoría local usada que SQL Server usa para recopilar y enviar datos de uso y diagnóstico a Microsoft.
ms.custom: seo-lt-2019
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Local Audit
ms.assetid: a0665916-7789-4f94-9086-879275802cf3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6333f571ea271420b4044f7e3cf562d2f1edcc15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353538"
---
# <a name="local-audit-for-sql-server-usage-and-diagnostic-data-collection-ceip"></a>Auditoría local para la recopilación de datos de uso y diagnóstico de SQL Server (CEIP)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

## <a name="introduction"></a>Introducción

Microsoft SQL Server contiene características habilitadas para Internet que pueden recopilar y enviar información sobre el equipo o dispositivo. Esto se denomina *información estándar del equipo*. El componente de auditoría local de [recopilación de datos de uso y diagnóstico de SQL Server](usage-and-diagnostic-data-configuration-for-sql-server.md) escribe los datos que recopila el servicio en una carpeta designada y que representa los datos (registros) que se enviarán a Microsoft. El propósito de la auditoría local es permitir que los clientes vean todos los datos que Microsoft recopila con esta característica, por motivos de cumplimiento, reglamentarios o por validación de privacidad.  

A partir de SQL Server 2016 CU2 y CU3, la auditoría local se puede configurar en el nivel de instancia para Motor de base de datos de SQL Server y Analysis Services (SSAS). En SQL Server 2016 CU4, 2016 SP1 y versiones posteriores, la auditoría local también está habilitada para SQL Server Integration Services (SSIS). Otros componentes de SQL Server que se instalan durante la configuración y herramientas de SQL Server que se descargan o instalan después de la configuración no cuentan con la funcionalidad de auditoría local para la recopilación de datos de uso y diagnóstico.

## <a name="remarks"></a>Observaciones

 - No se admite la eliminación o deshabilitación del servicio CEIP de SQL. 
 - No se admite la eliminación de los recursos de CEIP de SQL del Grupo de clúster. 

Para no participar en la recopilación de datos, vea [Activar o desactivar la auditoría local](#turning-local-audit-on-or-off).

## <a name="prerequisites"></a>Prerrequisitos 

A continuación, aparecen los requisitos previos para habilitar la auditoría local en cada instancia de SQL Server: 

1. La instancia aplica una revisión a SQL Server 2016 RTM CU2 o a una versión posterior. En el caso de Integration Services, la revisión de la instancia se aplica a SQL 2016 RTM CU4, SQL 2016 SP1 o versiones posteriores.

1. El usuario debe ser un administrador del sistema o debe tener un rol con acceso para agregar y modificar una clave del Registro, crear carpetas, administrar la seguridad de las carpetas y detener e iniciar un servicio de Windows.  

## <a name="pre-configuration-steps-prior-to-turning-on-local-audit"></a>Pasos de configuración previa antes de activar la auditoría local

Antes de activar la auditoría local, un administrador del sistema debe:

1. Saber el nombre de la instancia de SQL Server y la cuenta de inicio de sesión del servicio CEIP de SQL Server. 

1. Configure una carpeta nueva para los archivos de auditoría local.

1. Conceda permisos a la cuenta de inicio de sesión del servicio CEIP de SQL Server.

1. Cree una configuración de clave del Registro para configurar el directorio de destino de auditoría local. 


### <a name="get-the-sql-server-ceip-service-logon-account"></a>Obtención de la cuenta de inicio de sesión del servicio CEIP de SQL Server 

Complete los pasos siguientes para obtener la cuenta de inicio de sesión del servicio CEIP de SQL Server
 
1. Abra la consola **Servicios**. Para ello, presione la **tecla Windows + R** en el teclado para abrir el cuadro de diálogo **Ejecutar**. Después, escriba *services.msc* en el campo de texto y seleccione **Aceptar** para abrir la consola **Servicios**.  

2. Vaya al servicio correspondiente. Por ejemplo, para el motor de base de datos, ubique **Servicio CEIP de SQL Server**  **(*Nombre-Instancia*)** . En el caso de Analysis Services, busque **CEIP de SQL Server Analysis Services**  **(*Nombre-Instancia*)** . En el caso de Integration Services, busque **Servicio CEIP para SQL Server Integration Services**.

3. Haga clic con el botón derecho en el servicio y elija **Propiedades**. 

4. Seleccione la pestaña **Iniciar sesión**. La cuenta de inicio de sesión aparece en **Esta cuenta**. 

### <a name="configure-a-new-folder-for-the-local-audit-files"></a>Configure una carpeta nueva para los archivos de auditoría local.    

Cree una carpeta nueva (directorio de auditoría local) donde la uditoría local escribirá los registros. Por ejemplo, la ruta de acceso completa al directorio de auditoría local para una instancia predeterminada del motor de la base de datos sería la siguiente: *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* . 
 
  >[!NOTE] 
  >Configure la ruta de acceso al directorio para auditoría local fuera de la ruta de instalación de SQL Server para evitar que el hecho de permitir la funcionalidad de auditoría y de aplicar revisiones provoque problemas potenciales con SQL Server.

|Decisión de diseño|Recomendación|  
|-----------------|----------|  
|Disponibilidad de espacio |En una carga de trabajo moderada con alrededor de 10 bases de datos, disponga de unos 2 MB de espacio en disco por base de datos por instancia.|  
|Directorios independientes | Cree un directorio para cada instancia. Por ejemplo, use *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* para una instancia de SQL Server denominada `MSSQLSERVER`. Esto simplifica la administración de archivos.
|Carpetas independientes |Use una carpeta específica para cada servicio. Por ejemplo, para un nombre de instancia determinado, debe tener una carpeta para el motor de base de datos. Si una instancia de Analysis Services usa el mismo nombre de instancia, cree una carpeta independiente para Analysis Services. Si tiene las instancias de Motor de base de datos y de Analysis Services configuradas en la misma carpeta, la auditoría local escribirá todo en el mismo archivo de registro desde ambas instancias.| 
|Concesión de permisos a la cuenta de inicio de sesión del servicio de CEIP de SQL Server|Habilite **Mostrar el contenido de la carpeta**, el acceso de **lectura** y **escritura** a la cuenta de inicio de sesión del servicio CEIP de SQL Server.|


### <a name="grant-permissions-to-the-sql-server-ceip-service-logon-account"></a>Concesión de permisos a la cuenta de inicio de sesión del servicio de CEIP de SQL Server
  
1. En **Explorador de archivos**, vaya a la ubicación donde se encuentra la carpeta nueva.

1. Haga clic con el botón derecho en la carpeta nueva y elija **Propiedades**. 

1. En la **pestaña Seguridad**, seleccione **Editar** permiso Administrar.

1. Seleccione **Agregar** y escriba las credenciales del servicio CEIP de SQL Server. Por ejemplo, `NT Service\SQLTELEMETRY`.

1. Seleccione **Comprobar nombres** para validar el nombre que se proporcionó y, después, seleccione **Aceptar**.

1. En el cuadro de diálogo **Permiso**, elija la cuenta de inicio de sesión en el servicio CEIP de SQL Server y seleccione **Mostrar el contenido de la carpeta**, **Lectura** y **Escritura**.

1. Seleccione **Aceptar** para aplicar de inmediato los cambios en los permisos. 
  
### <a name="create-a-registry-key-setting-to-configure-local-audit-target-directory"></a>Creación de una configuración de clave del Registro para configurar el directorio de destino de auditoría local

1. Inicie regedit.

1. Vaya a la ruta de CPE correspondiente:

   | Versión | ***Motor de base de datos***: Clave del Registro |
   | :------ | :----------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **13**.*Nombre-Instancia*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **14**.*Nombre-Instancia*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **15**.*Nombre-Instancia*\\CPE |
   | &nbsp; | &nbsp; |

   | Versión | ***Analysis Services***: Clave del Registro |
   | :------ | :------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **13**.*Nombre-Instancia*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **14**.*Nombre-Instancia*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **15**.*Nombre-Instancia*\\CPE |  
   | &nbsp; | &nbsp; |

   | Versión | ***Integration Services***: Clave del Registro |
   | :------ | :---------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**130** |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**140** |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**150** |
   | &nbsp; | &nbsp; |

1. Haga clic con el botón derecho en la ruta de CPE y elija **Nuevo**. Seleccione **Valor de cadena**.

1. Ponga un nombre a la nueva clave de registro `UserRequestedLocalAuditDirectory`. 
 
## <a name="turning-local-audit-on-or-off"></a>Activación o desactivación de la auditoría local

Una vez que haya seguido los pasos de configuración previa, puede activar la auditoría local. Para ello, use una cuenta de administrador del sistema o un rol similar con acceso para modificar las claves del Registro a fin de activar o desactivar la auditoría local con los pasos siguientes. 

1. Inicie **regedit**.  

1. Vaya a la [ruta](#create-a-registry-key-setting-to-configure-local-audit-target-directory) de CPE correspondiente. 

1. Haga clic con el botón derecho en **UserRequestedLocalAuditDirectory** y seleccione *Modificar*. 

1. Para activar la auditoría local, escriba la ruta de acceso a la auditoría local; por ejemplo, *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* .
 
    Para desactivar la auditoría local, vacíe el valor de **UserRequestedLocalAuditDirectory**.

1. Cierre **regedit**. 

CEIP de SQL Server debe reconocer inmediatamente la configuración de auditoría local si el servicio ya está en ejecución. Para iniciar el servicio CEIP de SQL Server, un administrador del sistema o alguna persona con acceso para iniciar o detener los servicios de Windows puede seguir estos pasos: 

1. Abra la consola **Servicios**. Para ello, presione la **tecla Windows + R** en el teclado para abrir el cuadro de diálogo **Ejecutar**. Después, escriba *services.msc* en el campo de texto y seleccione **Aceptar** para abrir la consola **Servicios**.  

1. Vaya al servicio correspondiente. 

    - Para Motor de base de datos, use **Servicio CEIP de SQL Server (*Nombre-Instancia*)**.     
    - Para Analysis Services, use **CEIP de SQL Server Analysis Services (*Nombre-Instancia*)**.
    - Para Integration Services, 
        - Para SQL 2016, use *Servicio CEIP para SQL Server Integration Services 13.0*.
        - Para SQL 2017, use *Servicio CEIP para SQL Server Integration Services 14.0*.
    - Para SQL 2019, use *Servicio CEIP para SQL Server Integration Services 15.0*.

1. Haga clic con el botón derecho en el servicio y elija Reiniciar. 

1. Compruebe que el estado del servicio sea **En ejecución**. 

La auditoría local generará un archivo de registro al día. Los archivos de registro tendrán este formato: `<YYYY-MM-DD>.json`. Por ejemplo, *2016-07-12.json*. Si ya hay un archivo correspondiente al día en cuestión en el directorio designado, la auditoría local lo anexará a este. En caso contrario, se creará un archivo nuevo correspondiente al día. 

  >[!NOTE]
  > Después de habilitar la auditoría local, pueden pasar hasta cinco minutos hasta que se escriba por primera vez el archivo de registro. 

## <a name="maintenance"></a>Mantenimiento 

1. Para limitar el uso del espacio en disco que ocupan los archivos que escribe la auditoría local, configure una directiva o un trabajo periódico para limpiar el directorio de Auditoría local y quitar los archivos antiguos e innecesarios.  

2. Proteja la ruta de acceso al directorio de la auditoría local para que solo las personas adecuadas puedan tener acceso a él. Tenga en cuenta que los archivos de registro contienen información según lo descrito en [Cómo configurar SQL Server 2016 para enviar comentarios a Microsoft](https://support.microsoft.com/kb/3153756). El acceso a este archivo debe evitar que la mayoría de los miembros de su organización pueda leerlo.  

## <a name="data-dictionary-of-local-audit-output-data-structure"></a>Diccionario de datos de la estructura de datos de salida de la auditoría local 

- Los archivos de registro de auditoría local tienen el formato JSON y contienen un conjunto de objetos (filas) que representan puntos de datos que se envían de vuelta a Microsoft en **emitTime**.
- Cada fila sigue un esquema específico que se identifica por **schemaVersion**.
- Cada fila es una salida de una sesión de servicio de SQLCEIP que se identifica como **sessionID**.
- Las filas se emiten en una secuencia que se identifica por **sequence**.
- Cada fila de punto de datos contiene la salida de un **queryIdentifier**, que puede ser una consulta T-SQL, una sesión de XE o un mensaje relacionado con un tipo de seguimiento, identificado como **traceName**.
- Los valores **queryIdentifiers** están agrupados y tienen versiones junto con **querySetVersion**.
- Los valores **data** contienen la salida de la ejecución de consulta correspondiente, que usó **queryTimeInTicks**.
- Los valores **queryIdentifiers** para consultas T-SQL tienen la definición de consulta T-SQL almacenada en la consulta.

| Jerarquía de información de la auditoría local lógica | Columnas relacionadas |
| ------ | -------|
| Encabezado | emitTime, schemaVersion 
| Máquina | operatingSystem 
| Instancia | instanceUniqueID, correlationID y clientVersion 
| Sesión | sessionID, traceName 
| Consultar | sequence, querySetVersion, queryIdentifier, query, queryTimeInTicks 
| data |  datos 

### <a name="namevalue-pairs-definition-and-examples"></a>Definición y ejemplos de pares nombre-valor 

Las columnas siguientes representan el orden de la salida de archivo de auditoría local. Se usa un hash unidireccional con SHA 256 para anonimizar los valores para varias de las columnas siguientes.  

| NOMBRE | Descripción | Valores de ejemplo
|-------|--------| ----------|
|instanceUniqueID| Identificador de instancia anonimizada | 888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD 
|schemaVersion| La versión del esquema de SQLCEIP |  3 
|emitTime |Hora de emisión del punto de datos en hora UTC | 2016-09-08T17:20:22.1124269Z 
|sessionID | El identificador de la sesión del servicio SQLCEIP | 89decf9a-ad11-485c-94a7-fefb3a02ed86 
|correlationId | El marcador de posición para un identificador adicional | 0 
|sequence | El número de secuencia de los puntos de datos enviados dentro de la sesión | 15 
|clientVersion | Versión de la instancia de SQL Server | 13.0.2161.3 ((SQL16_RTM_QFE-CU).160907-1223) 
|operatingSystem | La versión del sistema operativo donde está instalada la instancia de SQL Server | Microsoft Windows Server 2012 R2 Datacenter 
|querySetVersion | La versión de un grupo de definiciones de consulta | 1.0.0.0 
|traceName | Categorías de seguimientos: (SQLServerXeQueries, SQLServerPeriodicQueries, SQLServerOneSettingsException) | SQLServerPeriodicQueries 
|queryIdentifier | Un identificador de la consulta | SQLServerProperties.002 
|datos   | El resultado de la información recopilada en queryIdentifier como salida de la consulta T-SQL, la sesión de XE o la aplicación |  [{"Collation": "SQL_Latin1_General_CP1_CI_AS","SqlFTinstalled": "0" "SqlIntSec": "1","IsSingleUser": "0","SqlFilestreamMode": "0","SqlPbInstalled": "0","SqlPbNodeRole": "","SqlVersionMajor": "13","SqlVersionMinor": "0","SqlVersionBuild": "2161","ProductBuildType": "","ProductLevel": "RTM","ProductUpdateLevel": "CU2","ProductUpdateReference": "KB3182270","ProductRevision": "3","SQLEditionId": "-1534726760","IsClustered": "0","IsHadrEnabled": "0","SqlAdvAInstalled": "0","PacketReceived": "1210","Version": "Microsoft SQL Server 2016 (RTM-CU2) (KB3182270) - 13.0.2161.3 (X64) \n\tSep  7 2016 14:24:16 \n\tCopyright (c) Microsoft Corporation\n\tStandard Edition (64-bit) on Windows Server 2012 R2 Datacenter 6.3 \u003cX64\u003e (Build 9600: ) (Hypervisor)\n"}],
|Query| Si corresponde, la definición de consulta T-SQL relacionada con el identificador queryIdentifier que genera los datos.        El servicio CEIP de SQL Server no carga este componente. Solo se incluye en la auditoría local como referencia para los clientes.| SELECT\n      SERVERPROPERTY(\u0027Collation\u0027) AS [Collation],\n      SERVERPROPERTY(\u0027IsFullTextInstalled\u0027) AS [SqlFTinstalled],\n      SERVERPROPERTY(\u0027IsIntegratedSecurityOnly\u0027) AS [SqlIntSec],\n      SERVERPROPERTY(\u0027IsSingleUser\u0027) AS [IsSingleUser],\n      SERVERPROPERTY (\u0027FileStreamEffectiveLevel\u0027) AS [SqlFilestreamMode],\n      SERVERPROPERTY(\u0027IsPolyBaseInstalled\u0027) AS [SqlPbInstalled],\n      SERVERPROPERTY(\u0027PolyBaseRole\u0027) AS [SqlPbNodeRole],\n      SERVERPROPERTY(\u0027ProductMajorVersion\u0027) AS [SqlVersionMajor],\n      SERVERPROPERTY(\u0027ProductMinorVersion\u0027) AS [SqlVersionMinor],\n      SERVERPROPERTY(\u0027ProductBuild\u0027) AS [SqlVersionBuild],\n      SERVERPROPERTY(\u0027ProductBuildType\u0027) AS ProductBuildType,\n      SERVERPROPERTY(\u0027ProductLevel\u0027) AS ProductLevel,\n      SERVERPROPERTY(\u0027ProductUpdateLevel\u0027) AS ProductUpdateLevel,\n      SERVERPROPERTY(\u0027ProductUpdateReference\u0027) AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)),CHARINDEX(\u0027.\u0027, REVERSE(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY(\u0027EditionID\u0027) AS SQLEditionId,\n      SERVERPROPERTY(\u0027IsClustered\u0027) AS IsClustered,\n      SERVERPROPERTY(\u0027IsHadrEnabled\u0027) AS IsHadrEnabled,\n      SERVERPROPERTY(\u0027IsAdvancedAnalyticsInstalled\u0027) AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version
|queryTimeInTicks | La duración necesaria para que se ejecute la consulta con la siguiente categoría de seguimiento: (SQLServerXeQueries, SQLServerPeriodicQueries) |  0 
 
### <a name="trace-categories"></a>Categorías de seguimiento 
Actualmente recopilamos las siguientes categorías de seguimiento: 

- **SQLServerXeQueries**: contiene puntos de datos recopilados a través de una sesión de eventos extendidos.
- **SQLServerPeriodicQueries**: contiene puntos de datos recopilados a través de consultas periódicas ejecutadas en una instancia de SQL Server.
- **SQLServerPerDBPeriodicQueries**: contiene puntos de datos recopilados a través de consultas periódicas ejecutadas en hasta 30 bases de datos en una instancia de SQL Server.
- **SQLServerOneSettingsException**: contiene mensajes de excepción relacionados con la actualización de un esquema o un conjunto de consultas.
- **DigitalProductID**: contiene puntos de datos para agregar el id. de producto digital con hash anónimo (SHA-256) de instancias de SQL Server. 

### <a name="local-audit-file-examples"></a>Ejemplos de archivos de auditoría local



A continuación, aparece un fragmento de una salida de archivo JSON de auditoría local.

```JSON
[
  {
    "instanceUniqueId": "888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:27:59.7031518Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 18,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "SQLServerProperties.002",
    "data": [
      {
        "Collation": "SQL_Latin1_General_CP1_CI_AS",
        "SqlFTinstalled": "0",
        "SqlIntSec": "1",
        "IsSingleUser": "0",
        "SqlFilestreamMode": "2",
        "SqlPbInstalled": "1",
        "SqlPbNodeRole": "Head",
        "SqlVersionMajor": "14",
        "SqlVersionMinor": "0",
        "SqlVersionBuild": "3025",
        "ProductBuildType": "",
        "ProductLevel": "RTM",
        "ProductUpdateLevel": "CU6",
        "ProductUpdateReference": "KB4101464",
        "ProductRevision": "34",
        "SQLEditionId": "1872460670",
        "IsClustered": "0",
        "IsHadrEnabled": "0",
        "SqlAdvAInstalled": "1",
        "PacketReceived": "422",
        "Version": "Microsoft SQL Server 2017 (RTM-CU6) (KB4101464) - 14.0.3025.34 (X64) \n\tApr  9 2018 18:00:41 \n\tCopyright (C) 2017 Microsoft Corporation\n\tEnterprise Edition: Core-based Licensing (64-bit) on Windows 10 Enterprise 10.0 <X64> (Build 16299: )\n"
      }
    ],
    "query": "SELECT\n      SERVERPROPERTY('Collation') AS [Collation],\n      SERVERPROPERTY('IsFullTextInstalled') AS [SqlFTinstalled],\n      SERVERPROPERTY('IsIntegratedSecurityOnly') AS [SqlIntSec],\n      SERVERPROPERTY('IsSingleUser') AS [IsSingleUser],\n      SERVERPROPERTY ('FileStreamEffectiveLevel') AS [SqlFilestreamMode],\n      SERVERPROPERTY('IsPolyBaseInstalled') AS [SqlPbInstalled],\n      SERVERPROPERTY('PolyBaseRole') AS [SqlPbNodeRole],\n      SERVERPROPERTY('ProductMajorVersion') AS [SqlVersionMajor],\n      SERVERPROPERTY('ProductMinorVersion') AS [SqlVersionMinor],\n      SERVERPROPERTY('ProductBuild') AS [SqlVersionBuild],\n      SERVERPROPERTY('ProductBuildType') AS ProductBuildType,\n      SERVERPROPERTY('ProductLevel') AS ProductLevel,\n      SERVERPROPERTY('ProductUpdateLevel') AS ProductUpdateLevel,\n      SERVERPROPERTY('ProductUpdateReference') AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)),CHARINDEX('.', REVERSE(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY('EditionID') AS SQLEditionId,\n      SERVERPROPERTY('IsClustered') AS IsClustered,\n      SERVERPROPERTY('IsHadrEnabled') AS IsHadrEnabled,\n      SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version",
    "queryTimeInTicks": 0
  },
  {
    "instanceUniqueId": "8884F770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:28:00.9025999Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 23,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "OsSysInfo.003",
    "data": [
      {
        "LogicalCPUCount": "8",
        "HyperthreadRatio": "8",
        "PhysicalMemoryMB": "32710.902343",
        "SQLServerStartTime": "05/04/2018 08:22:30",
        "AffinityTypeDesc": "AUTO",
        "VirtualMachineType": "0",
        "SocketCount": "1",
        "CoresPerSocket": "4",
        "NumaNodeCount": "1",
        "ContainerType": "0",
        "ContainerDescription": "NONE"
      }
    ],
    "query": "SELECT\n      cpu_count AS LogicalCPUCount,\n      hyperthread_ratio AS HyperthreadRatio,\n      physical_memory_kb/1024.0 AS PhysicalMemoryMB,\n      sqlserver_start_time AS SQLServerStartTime,\n      affinity_type_desc AS AffinityTypeDesc,\n      virtual_machine_type AS VirtualMachineType,\n      socket_count as SocketCount,\n      cores_per_socket as CoresPerSocket,\n      numa_node_count as NumaNodeCount,\n      container_type as ContainerType,\n      container_type_desc as ContainerDescription\n      FROM sys.dm_os_sys_info WITH(nolock)",
    "queryTimeInTicks": 0
  }
]
```
## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

**¿De qué manera los administradores de bases de datos leen los archivos de registro de auditoría local?**
Estos archivos de registro están escritos en formato JSON. Cada línea será un objeto JSON que representa una parte de los datos de uso o diagnóstico que se cargaron en Microsoft. Los nombres de los campos deben ser claros.

**¿Qué ocurre si el administrador de base de datos deshabilita la recopilación de datos de uso y diagnóstico?**
No se escribirá ningún archivo de auditoría local.

**¿Qué ocurre si no hay conectividad a Internet o si la máquina está detrás de un firewall?**
No se enviarán datos de uso y diagnóstico de SQL Server a Microsoft. De todos modos, se intentarían escribir los registros de Auditoría local si la configuración es correcta.

**¿De qué manera los administradores de bases de datos deshabilitan la auditoría local?**
Deshabilitan la entrada de la clave del Registro UserRequestedLocalAuditDirectory.

**¿Quiénes pueden leer los archivos de registro de auditoría local?**
Cualquier persona de la organización que cuente con acceso al directorio de auditoría local.

**¿De qué manera los administradores de bases de datos administran los archivos de registro escritos en el directorio designado?**
Los administradores de bases de datos deberán administrar automáticamente la limpieza de los archivos del directorio para evitar que se use demasiado espacio en disco.

**¿Existe algún cliente o herramienta que se pueda usar para leer esta salida JSON?**
Es posible leer la entrada con el Bloc de notas, Visual Studio o cualquier lector de JSON de su preferencia.
Como alternativa, puede leer el archivo JSON y analizar los datos de una instancia de SQL Server como se muestra a continuación. Para más detalles sobre cómo leer un archivo JSON en SQL Server, visite [Importing JSON files into SQL Server using OPENROWSET (BULK) and OPENJSON (Transact-SQL)](/archive/blogs/sqlserverstorageengine/bulk-importing-json-files-into-sql-server)(Importación de archivos JSON a SQL Server con OPENROWSET [BULK] y OPENJSON [Transact-SQL]).

```Transact-SQL
DECLARE @JSONFile AS VARCHAR(MAX)

-- Read the JSON file into variable 
SELECT @JSONFile = BulkColumn 
FROM OPENROWSET (BULK 'C:\SQLCEIPAudit\MSSQLSERVER\2016-09-08.json', SINGLE_CLOB) MyFile 

-- Check if the JSON file has been read properly and if it's in a JSON format
SELECT 
    @JSONFile LocalAuditOutput, 
    ISJSON(@JSONFile) IsFileInJSONFormat

-- Get the query identifier, query and the data (output of the query)   
SELECT 
    sequence,
    queryIdentifier,
    query,
    data
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON)
-- Get specific details about the output of "DatabaseProperties.001" query  
SELECT 
    QueryIdentifier,
    DatabaseID,
    CompatibilityLevel,
    IsQueryStoreOn
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON) 
    CROSS APPLY OPENJSON(data) 
        WITH (   DatabaseID varchar(128) '$.database_id'
                ,CompatibilityLevel varchar(128) '$.compatibility_level'
                ,IsQueryStoreOn varchar(128) '$.QS'
             )
WHERE queryIdentifier = 'DatabaseProperties.001'
```

## <a name="see-also"></a>Consulte también
[Auditoría local para la recopilación de datos de uso y diagnóstico de SSMS](../ssms/sql-server-management-studio-telemetry-ssms.md)