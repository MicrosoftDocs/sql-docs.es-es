---
description: Novedades de Integration Services en SQL Server 2016
title: Novedades de Integration Services en SQL Server 2016 | Microsoft Docs
ms.custom:
- SQL2016_New_Updated
ms.date: 09/28/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Integration Services, what's new
- what's new [Integration Services]
ms.assetid: da6999c7-e5e3-4a59-a284-1da635995af1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aa2736f06ad57ec6fca8ff0af8a07625265e296a
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237263"
---
# <a name="what39s-new-in-integration-services-in-sql-server-2016"></a>Novedades de Integration Services en SQL Server 2016

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



En este tema, se describen las características que se han agregado o actualizado en SQL Server 2016 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. También incluye características agregadas o actualizadas en [Azure Feature Pack para Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md) durante el período de tiempo de SQL Server 2016.  

## <a name="new-for-ssis-in-azure-data-factory"></a>Novedades para SSIS en Azure Data Factory

Con la versión preliminar pública de Azure Data Factory versión 2 de septiembre de 2017, ahora puede hacer lo siguiente:
-   Implementar paquetes en la base de datos del catálogo de SSIS (SSISDB) en Azure SQL Database.
-   Ejecutar paquetes implementados en Azure en Integration Runtime para la integración de SSIS en Azure, un componente de Azure Data Factory versión 2.

Para obtener más información, consulte [Lift and shift SQL Server Integration Services workloads to the cloud](lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md) (Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift).

Estas nuevas funcionalidades requieren SQL Server Data Tools (SSDT) versión 17.2 o posterior, pero no requieren SQL Server 2017 o SQL Server 2016. Al implementar paquetes en Azure, el Asistente para la implementación de paquetes siempre actualiza los paquetes al formato más reciente.

## <a name="2016-improvements-by-category"></a>Mejoras de 2016 por categoría  
  
-   **Facilidad de uso**  
  
    -   Mejor implementación  
  
        -   [Asistente para actualización de SSISDB](#ssisdbupgrwiz)  
  
        -   [Compatibilidad con Always On en el catálogo de SSIS](#AlwaysOn)  
  
        -   [Implementación incremental de paquetes](#IncrementalDeployment)  
  
        -   [Compatibilidad con Always Encrypted en el catálogo de SSIS](#encrypted)  
  
    -   Mejor depuración  
  
        -   [Nuevo rol de base de datos ssis_logreader en el catálogo de SSIS](#LogReader)  
  
        -   [Nuevo nivel de registro RuntimeLineage en el catálogo de SSIS](#RuntimeLineage)  
  
        -   [Nuevo nivel de registro personalizado en el catálogo de SSIS](#CustomLogging)  
  
        -   [Nombres de columna para errores del flujo de datos](#ErrorColumn)  
  
        -   [Compatibilidad ampliada con nombres de columna de error](#getidstring)  
  
        -   [Compatibilidad con el nivel de registro predeterminado de todo el servidor](#ServerLogLevel)  
  
        -   [Nueva interfaz IDTSComponentMetaData130 de la API](#CMD130)  
  
    -   Mejor administración de paquetes  
  
        -   [Experiencia mejorada de actualización de proyectos](#ProjectUpgrade)  
  
        -   [Propiedad AutoAdjustBufferSize calcula automáticamente el tamaño del búfer del flujo de datos](#BufferSize)  
  
        -   [Plantillas de flujo de control reutilizables](#Templates)  
  
        -   [Cambio del nombre de nuevas plantillas por elementos](#Parts)  
  
-   **Conectividad**  
  
    -   Conectividad ampliada en entornos locales  
  
        -   [Compatibilidad con orígenes de datos OData v4](#ODatav4)  
  
        -   [Compatibilidad explícita con orígenes de datos de Excel 2013](#Excel2013)  
  
        -   [Compatibilidad con el sistema de archivos Hadoop (HDFS)](#HDFS)  
  
        -   [Compatibilidad ampliada con Hadoop y HDFS](#more_hadoop)  
  
        -   [Destino de archivo HDFS admite ahora el formato de archivo ORC](#hdfsORC)  
  
        -   [Componentes de ODBC actualizados para SQL Server 2016](#odbc2016)  
  
        -   [Compatibilidad explícita con orígenes de datos de Excel 2016](#Excel2016)  
  
        -   [Connector for SAP BW para SQL Server 2016 publicado](#SAPBW)
        
        -   [Conectores v4.0 para Oracle y Teradata publicados](#oracleteradata)
        
        -   [Conectores para la actualización 5 de la aplicación Analytics Platform System (PDW) publicados](#pdwau5)
  
    -   Conectividad ampliada en la nube  
  
        -   Conectores de Almacenamiento de Azure y tareas de Hive y Pig de HDInsight: [Feature Pack de Azure para SSIS publicado para SQL Server 2016](#AFP2016)
        
        -   [Compatibilidad con recursos de Microsoft Dynamics Online publicados en el Service Pack 1](#dynamics)
        
        -   [Se lanzó la compatibilidad con Azure Data Lake Store](#datalakestore)
        
        -   [Compatibilidad con Azure Synapse Analytics publicada](#sqldwupload)
  
-   **Facilidad de uso y productividad**  
  
    -   Mejor experiencia de instalación  
  
        -   [Actualización bloqueada cuando SSISDB pertenece a un grupo de disponibilidad](#Upgrade)  
  
    -   Mejor experiencia de diseño  
  
        -   [El Diseñador SSIS crea y mantiene los paquetes de SQL Server 2016, 2014 o 2012](#OneDesigner)  
  
        -   Varias mejoras del diseñador y correcciones de errores.  
  
    -   Mejor experiencia de administración en SQL Server Management Studio
  
        -   [Rendimiento mejorado para las vistas de catálogo de SSIS](#CatViews)  
  
    -   Otras mejoras  
  
        -   [La transformación Distribuidor de datos equilibrado forma parte ahora de SSIS](#BDDinbox)  
  
        -   [Componentes de publicación de fuente de distribución de datos forma parte ahora de SSIS](#ComplexFeedinbox)  
  
        -   [Compatibilidad con Azure Blog Storage en el Asistente para importación y exportación de SQL Server](#AzureBlob)  
  
        -   [Change Data Capture Designer y Service para Oracle para Microsoft SQL Server 2016 publicado](#CDCOracle)  
  
        -   [Componentes de CDC actualizados para SQL Server 2016](#cdc2016)  
  
        -   [Tarea Ejecutar DDL de Analysis Services actualizada](#ASDDL)  
  
        -   [Tareas de Analysis Services admiten modelos tabulares](#ssasrc0)  
  
        -   [Compatibilidad con R Services integrado](#builtinR)  
  
        -   [Salida enriquecida de validación de XML en la tarea XML](#ValidateXML)  
  
## <a name="manageability"></a>Facilidad de uso  

### <a name="better-deployment"></a>Mejor implementación

####  <a name="ssisdb-upgrade-wizard"></a><a name="ssisdbupgrwiz"></a> Asistente para actualización de SSISDB  
 Ejecute el Asistente para actualización de SSISDB para actualizar la base de datos del catálogo de SSIS, SSISDB, cuando esta sea anterior a la versión actual de la instancia de SQL Server. Tiene lugar cuando se presenta alguna de las siguientes condiciones.  
  
-   Restauró la base de datos de una versión anterior de SQL Server.  
  
-   No quitó la base de datos de un grupo de disponibilidad AlwaysOn antes de actualizar la instancia de SQL Server. Esto evita la actualización automática de la base de datos. Para obtener más información, vea [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade).  
  
 Para obtener más información, vea [Catálogo de SSIS &#40;SSISDB&#41;](../integration-services/catalog/ssis-catalog.md). 

####  <a name="support-for-always-on-in-the-ssis-catalog"></a><a name="AlwaysOn"></a> Compatibilidad con Always On en el catálogo de SSIS  
 La característica Grupos de disponibilidad AlwaysOn es una solución de alta disponibilidad y de recuperación ante desastres que proporciona una alternativa empresarial a la creación de reflejo de la base de datos. Un grupo de disponibilidad admite un entorno de conmutación por error para un conjunto discreto de bases de datos de usuario, conocido como bases de datos de disponibilidad, que realizan la conmutación por error conjuntamente. Para obtener más información, vea [Grupos de disponibilidad Always On](../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md).  
  
 En SQL Server 2016, SSIS incluye nuevas funcionalidades que le permiten implementar fácilmente en un catálogo de SSIS centralizado (es decir, la base de datos de usuario de SSISDB). Para proporcionar alta disponibilidad de la base de datos SSISDB y su contenido, proyectos, paquetes, registros de ejecución, etc., puede agregar la base de datos SSISDB a un grupo de disponibilidad Always On, como en cualquier otra base de datos de usuario. Cuando se produce una conmutación por error, uno de los nodos secundarios se convierte automáticamente en el nuevo nodo principal.  
  
 Para obtener información detallada e instrucciones paso a paso sobre cómo habilitar Always On para SSISDB, vea [SSIS Catalog](../integration-services/catalog/ssis-catalog.md) (Catálogo de SSIS).  

####  <a name="incremental-package-deployment"></a><a name="IncrementalDeployment"></a> Implementación incremental de paquetes  
La característica Implementación incremental de paquetes le permite implementar uno o varios paquetes en un proyecto nuevo o existente sin implementar todo el proyecto. Puede implementar paquetes de forma incremental con las herramientas siguientes.  
  
-   Asistente para la implementación  
  
-   SQL Server Management Studio (que usa el Asistente para implementación)  
  
-   SQL Server Data Tools (Visual Studio) (que también usa el Asistente para implementación)  
  
-   Procedimientos almacenados  
  
-   API del modelo de objetos de administración (MOM)  
  
 Para más información, vea [Implementación de proyectos y paquetes de Integration Services (SSIS)](../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md).  

####  <a name="support-for-always-encrypted-in-the-ssis-catalog"></a><a name="encrypted"></a> Compatibilidad con Always Encrypted en el catálogo de SSIS  
 SSIS ya es compatible con la característica Always Encrypted en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Para obtener más información, vea las entradas de blog siguientes.  
  
-   [SSIS con Always Encrypted](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ssis-with-always-encrypted/ba-p/388272)  
  
-   [Transformación Búsqueda con Always Encrypted](https://techcommunity.microsoft.com/t5/sql-server-integration-services/lookup-transformation-with-always-encrypted/ba-p/388282)  

### <a name="better-debugging"></a>Mejor depuración

####  <a name="new-ssis_logreader-database-level-role-in-the-ssis-catalog"></a><a name="LogReader"></a> Nuevo rol de base de datos ssis_logreader en el catálogo de SSIS  
 En versiones anteriores del catálogo de SSIS, solo los usuarios con el rol **ssis_admin** pueden tener acceso a las vistas que contienen la salida del registro. Ahora hay un nuevo rol de base de datos **ssis_logreader** que puede usar para conceder permisos de acceso a las vistas que contienen la salida de registro a usuarios que no sean administradores.  
  
 También hay un nuevo rol **ssis_monitor** . Este rol admite Always On y es exclusivamente para uso interno del catálogo de SSIS.  

####  <a name="new-runtimelineage-logging-level-in-the-ssis-catalog"></a><a name="RuntimeLineage"></a> Nuevo nivel de registro RuntimeLineage en el catálogo de SSIS  
 El nuevo nivel de registro **RuntimeLineage** del catálogo de SSIS recopila los datos necesarios para realizar un seguimiento de la información de linaje en el flujo de datos. Puede analizar esta información de linaje para asignar la relación de linaje entre las tareas. Los ISV y los desarrolladores pueden generar herramientas de asignación de linaje personalizadas con esta información. 

####  <a name="new-custom-logging-level-in-the-ssis-catalog"></a><a name="CustomLogging"></a> Nuevo nivel de registro personalizado en el catálogo de SSIS  
 Las versiones anteriores del catálogo de SSIS le permiten elegir entre cuatro niveles de registro integrados al ejecutar un paquete: **ninguno, básico, rendimiento o detallado**. SQL Server 2016 agrega el nivel de registro **RuntimeLineage**. Además, ahora puede crear varios niveles de registro personalizado y guardarlos en el catálogo de SSIS, y elegir el nivel de registro que va a usar cada vez que ejecute un paquete. Para cada nivel de registro personalizado, seleccione solo las estadísticas y los eventos que quiera capturar. También puede incluir el contexto del evento para ver los valores de las variables, las cadenas de conexión y las propiedades de la tarea. Para obtener más información, vea [Habilitar el registro para la ejecución de paquetes en el servidor SSIS](../integration-services/performance/integration-services-ssis-logging.md#server_logging). 

####  <a name="column-names-for-errors-in-the-data-flow"></a><a name="ErrorColumn"></a> Nombres de columna para errores del flujo de datos  
 Al redirigir las filas en el flujo de datos que contienen errores a una salida de error, el resultado contiene un identificador numérico para la columna en la que se produjo el error, pero no muestra el nombre de la columna. Ahora existen varias formas de buscar o mostrar el nombre de la columna en la que se produjo el error.  
  
-   Al configurar el registro, seleccione el evento **DiagnosticEx** para el registro. Este evento escribe un mapa de columnas de flujo de datos en el registro. Luego puede buscar el nombre de columna en este mapa de columnas mediante el identificador de columna que captura una salida de error. Para obtener más información, vea [Error Handling in Data](../integration-services/data-flow/error-handling-in-data.md) (Control de errores en los datos).  
  
-   En el Editor avanzado, puede ver el nombre de columna de la columna de nivel superior al ver las propiedades de una columna de entrada o de salida de un componente de flujo de datos.  
  
-   Para ver los nombres de las columnas en las que se produjo el error, adjunte un Visor de datos a una salida de error.  Ahora, el Visor de datos muestra la descripción del error y el nombre de la columna en la que se produjo el error.  
  
-   En el Componente de script o en un componente de flujo de datos personalizado, llame al nuevo método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> de la interfaz IDTSComponentMetadata100.  
  
 Para más info sobre esta mejora, vea el siguiente blog publicado por el desarrollador de SSIS Bo Fan: [Error Column Improvements for SSIS Data Flow](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)(Mejoras de la columna de error para el flujo de datos de SSIS).  
  
> [!NOTE]  
>  (Esta compatibilidad se amplió en versiones posteriores. Para obtener más información, vea [Compatibilidad ampliada con nombres de columna de error](#getidstring) y [Nueva interfaz IDTSComponentMetaData130 de la API](#CMD130)).  

####  <a name="expanded-support-for-error-column-names"></a><a name="getidstring"></a> Compatibilidad ampliada con nombres de columna de error  
 El evento **DiagnosticEx** registra ahora información de columna de todas las columnas de entrada y de salida, no solo las columnas de linaje. Por consiguiente, ahora a la salida la llamamos mapa de columnas de la canalización en lugar de mapa de linaje de la canalización.  
  
 El método GetIdentificationStringByLineageID ha cambiado a <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A>. Para obtener más información, vea [Nombres de columna para errores del flujo de datos](#ErrorColumn).  
  
 Para obtener más información sobre este cambio y la mejora de la columna de error, vea la siguiente entrada de blog actualizada. [Error Column Improvements for SSIS Data Flow (Updated for CTP3.3) (Mejoras de la columna de error para el flujo de datos SSIS (actualizada para CTP3.3))](https://techcommunity.microsoft.com/t5/sql-server-integration-services/error-column-improvements-for-ssis-data-flow-updated-for-rc2/ba-p/388253)  
  
> [!NOTE]  
>  (En RC0, este método se ha movido a la nueva interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> . Para obtener más información, vea [Nueva interfaz IDTSComponentMetaData130 de la API](#CMD130)).  

####  <a name="support-for-server-wide-default-logging-level"></a><a name="ServerLogLevel"></a> Compatibilidad con el nivel de registro predeterminado de todo el servidor  
 En **Propiedades del servidor** de SQL Server, en la propiedad **Nivel de registro de servidor** , ahora puede seleccionar un nivel de registro predeterminado de todo el servidor. Puede elegir entre uno de los niveles de registro integrados (básico, ninguno, detallado, rendimiento o linaje en tiempo de ejecución) o puede elegir un nivel de registro personalizado existente. El nivel de registro seleccionado se aplica a todos los paquetes que se implementen en el catálogo de SSIS. También se aplica de forma predeterminada a un paso de trabajo del Agente SQL que ejecuta un paquete SSIS.  

####  <a name="new-idtscomponentmetadata130-interface-in-the-api"></a><a name="CMD130"></a> Nueva interfaz IDTSComponentMetaData130 de la API  
 El nuevo nivel de registro <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130> agrega nueva funcionalidad de SQL Server 2016 a la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> existente, especialmente el método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> . (El método **GetIdentificationStringByID** pasa a la nueva interfaz desde la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> ). También hay nuevas interfaces, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInputColumn130> y <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn130> , que ofrecen la propiedad **LineageIdentificationString** . Para obtener más información, vea [Nombres de columna para errores del flujo de datos](#ErrorColumn).  

### <a name="better-package-management"></a>Mejor administración de paquetes

####  <a name="improved-experience-for-project-upgrade"></a><a name="ProjectUpgrade"></a> Experiencia mejorada de actualización de proyectos  
 Al actualizar proyectos de SSIS de versiones anteriores a la versión actual, los administradores de conexiones de nivel de proyecto seguirán funcionando según lo previsto y el diseño del paquete y las anotaciones se conservan.  

####  <a name="autoadjustbuffersize-property-automatically-calculates-buffer-size-for-data-flow"></a><a name="BufferSize"></a> Propiedad AutoAdjustBufferSize calcula automáticamente el tamaño del búfer del flujo de datos  
 Al establecer el valor de la nueva propiedad **AutoAdjustBufferSize** en **true**, el motor de flujo de datos calcula automáticamente el tamaño del búfer del flujo de datos. Para obtener más información, vea [Data Flow Performance Features](../integration-services/data-flow/data-flow-performance-features.md).  

####  <a name="reusable-control-flow-templates"></a><a name="Templates"></a> Plantillas de flujo de control reutilizables  
 Guarde un contenedor o una tarea de flujo de control que use habitualmente en un archivo de plantilla independiente y reutílicelo varias veces en uno o más paquetes de un proyecto con plantillas de flujo de control. Esta reusabilidad facilita el diseño y el mantenimiento de paquetes SSIS. Para obtener más información, vea [Reutilización del flujo de control en paquetes mediante el uso de elementos del paquete de flujo de control](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

####  <a name="new-templates-renamed-as-parts"></a><a name="Parts"></a> Cambio del nombre de nuevas plantillas por elementos  
 Se ha cambiado el nombre de las nuevas plantillas de flujo de control reutilizables que se publicaron en CTP 3.0 por elementos de flujo de control o elementos de paquete. Para obtener más información sobre esta característica, vea [Reutilización del flujo de control en paquetes mediante el uso de elementos del paquete de flujo de control](../integration-services/reuse-control-flow-across-packages-by-using-control-flow-package-parts.md).  

## <a name="connectivity"></a>Conectividad  

### <a name="expanded-connectivity-on-premises"></a>Conectividad ampliada en entornos locales

####  <a name="support-for-odata-v4-data-sources"></a><a name="ODatav4"></a> Compatibilidad con orígenes de datos OData v4  
 El origen OData y el Administrador de conexiones de OData admiten ahora los protocolos OData v3 y v4.  
  
-   Para el protocolo OData V3, el componente admite los formatos de datos ATOM y JSON.  
  
-   Protocolo OData V4, el componente admite el formato de datos JSON.  
  
 Para obtener más información, vea [OData Source](../integration-services/data-flow/odata-source.md).  

####  <a name="explicit-support-for-excel-2013-data-sources"></a><a name="Excel2013"></a> Compatibilidad explícita con orígenes de datos de Excel 2013  
 El Administrador de conexiones con Excel, el Origen de Excel y el Destino de Excel, y el Asistente para importación y exportación de SQL Server ofrecen ahora compatibilidad explícita con orígenes de datos de Excel 2013. 

####  <a name="support-for-the-hadoop-file-system-hdfs"></a><a name="HDFS"></a> Compatibilidad con el sistema de archivos Hadoop (HDFS)  
 La compatibilidad con HDFS contiene administradores de conexiones para conectarse a clústeres de Hadoop y tareas para realizar operaciones comunes de HDFS. Para obtener más información, vea [Compatibilidad de Hadoop y HDFS en Integration Services &#40;SSIS&#41;](../integration-services/hadoop-and-hdfs-support-in-integration-services-ssis.md).  

####  <a name="expanded-support-for-hadoop-and-hdfs"></a><a name="more_hadoop"></a> Compatibilidad ampliada con Hadoop y HDFS  
  
-   El Administrador de conexiones de Hadoop admite ahora autenticación básica y Kerberos. Para obtener más información, vea [Hadoop Connection Manager](../integration-services/connection-manager/hadoop-connection-manager.md).  
  
-   El origen de archivo HDFS y el destino de archivo HDFS admiten ahora formato de texto y Avro. Para obtener más información, vea  [HDFS File Source](../integration-services/data-flow/hdfs-file-source.md) y  [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  
  
-   La tarea Sistema de archivos Hadoop admite ahora la opción CopyWithinHadoop, además de las opciones CopyToHadoop y CopyFromHadoop. Para obtener más información, vea [Hadoop File System Task](../integration-services/control-flow/hadoop-file-system-task.md).  

####  <a name="hdfs-file-destination-now-supports-orc-file-format"></a><a name="hdfsORC"></a> Destino de archivo HDFS admite ahora el formato de archivo ORC  
 El destino de archivo HDFS admite ahora el formato de archivo ORC, además de texto y Avro. (El origen de archivo HDFS solo admite texto y Avro). Para obtener más información sobre este componente, vea [HDFS File Destination](../integration-services/data-flow/hdfs-file-destination.md).  

####  <a name="odbc-components-updated-for-sql-server-2016"></a><a name="odbc2016"></a> Componentes de ODBC actualizados para SQL Server 2016  
 Los componentes de origen y de destino de ODBC se actualizan para ofrecer compatibilidad completa con SQL Server 2016. No hay ninguna funcionalidad nueva ni ningún cambio de comportamiento.  

####  <a name="explicit-support-for-excel-2016-data-sources"></a><a name="Excel2016"></a> Compatibilidad explícita con orígenes de datos de Excel 2016  
 El Administrador de conexiones con Excel, el Origen de Excel y el Destino de Excel ofrecen ahora compatibilidad explícita con orígenes de datos de Excel 2016.  

####  <a name="connector-for-sap-bw-for-sql-server-2016-released"></a><a name="SAPBW"></a> Connector for SAP BW para SQL Server 2016 publicado  
 Microsoft Connector for SAP BW para Microsoft SQL Server 2016 se publica como parte de SQL Server 2016 Feature Pack. Para descargar los componentes del Feature Pack, consulte [SQL Server 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833).
 
#### <a name="connectors-v40-for-oracle-and-teradata-released"></a><a name="oracleteradata"></a> Conectores v4.0 para Oracle y Teradata publicados
Se han publicado conectores v4.0 de Microsoft para Oracle y Teradata. Para descargar los conectores, vea [Microsoft Connectors v4.0 for Oracle and Teradata](https://www.microsoft.com/download/details.aspx?id=52950)(Conectores v4.0 de Microsoft para Oracle y Teradata).

### <a name="connectors-for-analytics-platform-system-pdw-appliance-update-5-released"></a><a name="pdwau5"></a> Conectores para la actualización 5 de la aplicación Analytics Platform System (PDW) publicados
Se han publicado los adaptadores de destino para cargar datos en PDW con AU5. Para descargar los adaptadores, vea [Documentación y herramientas de cliente de la actualización 5 de la aplicación Analytics Platform System](https://www.microsoft.com/download/details.aspx?id=51610).

### <a name="expanded-connectivity-to-the-cloud"></a>Conectividad ampliada en la nube

####  <a name="azure-feature-pack-for-ssis-released-for-sql-server-2016"></a><a name="AFP2016"></a> Feature Pack de Azure para SSIS publicado para SQL Server 2016  
 Se ha publicado Azure Feature Pack para Integration Services para SQL Server 2016. El Feature Pack contiene administradores de conexiones para conectarse a orígenes de datos de Azure y tareas para realizar operaciones comunes de Azure. Para obtener más información, vea [Azure Feature Pack para Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md).  

#### <a name="support-for-microsoft-dynamics-online-resources-released-in-service-pack-1"></a><a name="dynamics"></a> Compatibilidad con recursos de Microsoft Dynamics Online publicados en el Service Pack 1

Con SQL Server 2016 Service Pack 1 instalado, el origen OData y el administrador de conexiones OData ahora admiten la conexión a fuentes de OData de Microsoft Dynamics AX Online y Microsoft Dynamics CRM Online.

#### <a name="support-for-azure-data-lake-store-released"></a><a name="datalakestore"></a> Se lanzó la compatibilidad con Azure Data Lake Store

La versión más reciente de Azure Feature Pack incluye un administrador, origen y destino de conexiones para mover los datos hacia y desde Azure Data Lake Store. Para más información, consulte [Azure Feature Pack para Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md)

#### <a name="support-for-azure-synapse-analytics-released"></a><a name="sqldwupload"></a> Compatibilidad con Azure Synapse Analytics publicada

La versión más reciente de Azure Feature Pack incluye la tarea de carga de Azure SQL DW para rellenar con datos Azure Synapse Analytics. Para más información, consulte [Azure Feature Pack para Integration Services &#40;SSIS&#41;](../integration-services/azure-feature-pack-for-integration-services-ssis.md)

## <a name="usability-and-productivity"></a>Facilidad de uso y productividad  
 
### <a name="better-install-experience"></a>Mejor experiencia de instalación

####  <a name="upgrade-blocked-when-ssisdb-belongs-to-an-availability-group"></a><a name="Upgrade"></a> Actualización bloqueada cuando SSISDB pertenece a un grupo de disponibilidad  
 Si la base de datos del catálogo de SSIS (SSISDB) pertenece a un grupo de disponibilidad AlwaysOn, tiene que quitar SSISDB del grupo de disponibilidad, actualizar SQL Server y volver a agregar SSISDB al grupo de disponibilidad. Para obtener más información, vea [Upgrading SSISDB in an availability group](../integration-services/catalog/ssis-catalog.md#Upgrade).  

### <a name="better-design-experience"></a>Mejor experiencia de diseño

####  <a name="multi-targeting-and-multi-version-support-in-ssis-designer"></a><a name="OneDesigner"></a> Compatibilidad con varias plataformas y versiones en el Diseñador SSIS  
 Ahora puede usar el Diseñador SSIS en SQL Server Data Tools (SSDT) para Visual Studio 2015 a fin de crear, mantener y ejecutar paquetes que se destinen a SQL Server 2016, SQL Server 2014 o SQL Server 2012. Para obtener SSDT, consulte [Descarga de las SQL Server Data Tools más recientes](../ssdt/download-sql-server-data-tools-ssdt.md). 

 En el Explorador de soluciones, haga clic con el botón derecho en un proyecto de Integration Services y seleccione **Propiedades** para abrir las páginas de propiedades del proyecto. En la pestaña **General** de **Propiedades de configuración**, seleccione la propiedad **TargetServerVersion** y luego elija SQL Server 2016, SQL Server 2014 o SQL Server 2012.  
   
 ![Propiedad TargetServerVersion en el cuadro de diálogo Propiedades del proyecto](../integration-services/media/targetserverversion2.png "Propiedad TargetServerVersion en el cuadro de diálogo Propiedades del proyecto")  

> [!IMPORTANT]
> Si desarrolla extensiones personalizadas para SSIS, vea [Admitir varias versiones en los componentes personalizados](../integration-services/extending-packages-custom-objects/support-multi-targeting-in-your-custom-components.md) y [Conseguir que la compatibilidad con varias versiones de SSDT 2015 para SQL Server 2016 admita extensiones personalizadas de SSIS](https://blogs.msdn.microsoft.com/ssis/2016/04/19/getting-your-ssis-custom-extensions-to-be-supported-by-the-multi-version-support-of-ssdt-2015-for-sql-server-2016/).  

### <a name="better-management-experience-in-sql-server-management-studio"></a>Mejor experiencia de administración en SQL Server Management Studio

####  <a name="improved-performance-for-ssis-catalog-views"></a><a name="CatViews"></a> Rendimiento mejorado para las vistas de catálogo de SSIS  
 La mayoría de las vistas de catálogo de SSIS funcionan mejor ahora cuando las ejecuta un usuario que no es miembro del rol ssis_admin.  

### <a name="other-enhancements"></a>Otras mejoras

####  <a name="balanced-data-distributor-transformation-is-now-part-of-ssis"></a><a name="BDDinbox"></a> La transformación Distribuidor de datos equilibrado forma parte ahora de SSIS  
 La transformación Distribuidor de datos equilibrado, que requiere una descarga independiente en versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], ahora se instala al instalar [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Para obtener más información, vea [Balanced Data Distributor Transformation](../integration-services/data-flow/transformations/balanced-data-distributor-transformation.md).  
  
####  <a name="data-feed-publishing-components-are-now-part-of-ssis"></a><a name="ComplexFeedinbox"></a> Componentes de publicación de fuente de distribución de datos forma parte ahora de SSIS  
 Componentes de publicación de fuente de distribución de datos, que requiere una descarga independiente en versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], ahora se instala al instalar [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Para obtener más información, vea [Data Streaming Destination](../integration-services/data-flow/data-streaming-destination.md).  

####  <a name="support-for-azure-blob-storage-in-the-sql-server-import-and-export-wizard"></a><a name="AzureBlob"></a> Compatibilidad con Azure Blob Storage en el Asistente para importación y exportación de SQL Server  
 El Asistente para importación y exportación de SQL Server puede ahora importar datos del Almacenamiento de blobs de Azure y guardar datos en él. Para obtener más información, vea [Elegir un origen de datos &#40;Asistente para importación y exportación de SQL Server&#41;](../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md) y [Elegir un destino &#40;Asistente para importación y exportación de SQL Server&#41;](../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md). 

####  <a name="change-data-capture-designer-and-service-for-oracle-for-microsoft-sql-server-2016-released"></a><a name="CDCOracle"></a> Diseñador y servicio de captura de datos modificados para Oracle para Microsoft SQL Server 2016 publicado  
 El servicio y el diseñador de captura de datos modificados para Oracle de Attunity de Microsoft para Microsoft SQL Server 2016 se publican como parte de SQL Server 2016 Feature Pack.  Estos componentes admiten ahora Oracle 12c en la instalación clásica. (No se admite la instalación multiinquilino) Para descargar los componentes del Feature Pack, consulte [SQL Server 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833).  
  
####  <a name="cdc-components-updated-for-sql-server-2016"></a><a name="cdc2016"></a> Componentes de CDC actualizados para SQL Server 2016  
 Los componentes Tarea Control, Origen y Transformación Divisor de CDC (Captura de datos modificados) se actualizan para ofrecer compatibilidad completa con SQL Server 2016. No hay ninguna funcionalidad nueva ni ningún cambio de comportamiento.  
  
####  <a name="analysis-services-execute-ddl-task-updated"></a><a name="ASDDL"></a> Tarea Ejecutar DDL de Analysis Services actualizada  
 La Tarea Ejecutar DDL de Analysis Services se actualiza para aceptar comandos de Tabular Model Scripting Language.

####  <a name="analysis-services-tasks-support-tabular-models"></a><a name="ssasrc0"></a> Tareas de Analysis Services admiten modelos tabulares  
 Ahora puede usar todas las tareas SSIS y los destinos que admiten SQL Server Analysis Services (SSAS) con los modelos tabulares de SQL Server 2016. Las tareas SSIS se han actualizado para representar objetos tabulares en lugar de objetos multidimensionales. Por ejemplo, cuando se seleccionan objetos para procesar, la tarea de procesamiento de Analysis Services detecta automáticamente un modelo tabular y muestra una lista de objetos tabulares en lugar de mostrar grupos de medida y dimensiones. El destino de procesamiento de particiones ahora también muestra objetos tabulares y admite la inserción de datos en una partición.  
  
 El destino de procesamiento de dimensiones no funciona con modelos tabulares con el nivel de compatibilidad de SQL 2016.  La tarea de procesamiento de Analysis Services y el destino de procesamiento de particiones son todo lo que necesita para su procesamiento tabular. 

####  <a name="support-for-built-in-r-services"></a><a name="builtinR"></a> Compatibilidad con R Services integrado  
 SSIS ya es compatible con R Services integrado en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Puede usar SSIS no solo para extraer datos y cargar la salida del análisis, sino para generar, ejecutar y periódicamente volver a entrenar modelos de R. Para obtener más información, vea la entrada de blog siguiente. [Incorporación de operatividad a su proyecto de aprendizaje automático mediante SQL Server 2016 SSIS y R Services](https://techcommunity.microsoft.com/t5/sql-server-integration-services/operationalize-your-machine-learning-project-using-sql-server/ba-p/388296). 

####  <a name="rich-xml-validation-output-in-the-xml-task"></a><a name="ValidateXML"></a> Salida enriquecida de validación de XML en la tarea XML  
 Valide documentos XML y obtenga una salida de error enriquecida habilitando la propiedad **ValidationDetails** de la tarea XML. Antes de que la propiedad **ValidationDetails** estuviera disponible, la validación de XML efectuada mediante la tarea XML solo devolvía un resultado true o false, sin información sobre errores o sus ubicaciones. Ahora, al establecer **ValidationDetails** como true, el archivo de salida contiene información detallada sobre cada uno de los errores, incluido el número de línea y su posición. Puede usar esta información para comprender, buscar y corregir errores en documentos XML. Para obtener más información, vea [Validate XML with the XML Task](../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 [!INCLUDE[ssIS](../includes/ssis-md.md)] ha introducido la propiedad **ValidationDetails** en [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] Service Pack 2. Esta nueva propiedad no se anunció ni documentó en su momento. La propiedad **ValidationDetails** también está disponible en [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] y en [!INCLUDE[sssql15-md](../includes/sssql16-md.md)].   

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

## <a name="see-also"></a>Consulte también  
 [Novedades de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)   
 [Características compatibles con las ediciones de SQL Server 2016](../sql-server/editions-and-components-of-sql-server-2016.md)