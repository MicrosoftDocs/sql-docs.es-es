---
title: Notas de la versión de (SSMS)
description: Notas de la versión de SQL Server Management Studio (SSMS).
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 3dc76cc1-3b4c-4719-8296-f69ec1b476f9
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/17/2020
ms.openlocfilehash: 050447d6f1f9f13f04b01dec82e1b50ca6efbc69
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354816"
---
# <a name="release-notes-for-sql-server-management-studio-ssms"></a>Notas de la versión de SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

En este artículo, se proporcionan detalles sobre las actualizaciones, mejoras y correcciones de errores de las versiones actuales y anteriores de SSMS.

## <a name="current-ssms-release"></a>Versión actual de SSMS

### <a name="188"></a>18,8

![Descargar](media/download-icon.png) [Descargar SSMS 18.8](download-sql-server-management-studio-ssms.md)

- Número de versión: 18,8
- Número de compilación: 15.0.18369.0
- Fecha de lanzamiento: 17 de diciembre de 2020

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40a)

SSMS 18.8 es la versión de disponibilidad general (GA) más reciente de SSMS. Si necesita una versión anterior de SSMS, consulte [versiones de SSMS anteriores](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-188"></a>Novedades de la versión 18.8

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

| Nuevo elemento | Detalles |
|----------|---------|
| Integración de la instalación de Azure Data Studio | La instalación de SSMS instala Azure Data Studio 1.25.1. |
| Analysis Services | Se ha agregado compatibilidad con las áreas de trabajo de Power BI Premium Gen2. |
| Auditoría | Se ha agregado compatibilidad para EXTERNAL_MONITOR y el "operador audit".  Consulte la [documentación](/azure/azure-sql/managed-instance/auditing-configure) de configuración para más información. |
| Integration Services | Asistente para la creación de IR mejorado, de modo que la creación de SSISDB es opcional al crear IR de SSIS |

#### <a name="bug-fixes-in-188"></a>Correcciones de errores en la versión 18.8

| Nuevo elemento | Detalles |
|----------|---------|
| Accesibilidad | Se han corregido incidencias que afectan al narrador en el cuadro de diálogo ProgressReport |
| Monitor de actividad | Se ha corregido una incidencia por la que el menú contextual "Editar texto de consulta" de la cuadrícula "Consultas costosas activas" no funcionaba |
| Always Encrypted | Se ha corregido una incidencia por la que SSMS podía generar un error en tiempo de ejecución ("Clase no encontrada") |
| Valores altos de PPP/escalado | Se ha mejorado la presentación 4K del cuadro de diálogo ProgressReport |
| Valores altos de PPP/escalado | Se ha corregido una incidencia en el formulario "Nuevo programa: paquete" en Integration Services |
| SSMS general |  Se ha corregido una incidencia por la que SSMS podía generar un error al intentar mostrar las propiedades de una columna que tiene una inicialización de identidad o un incremento de identidad que supera el tamaño de un bigint.
| SSMS general | Se ha corregido un bloqueo en SSMS causado por un elemento de menú "Plantillas de exportación de proyectos..." que no estaba diseñado para mostrarse |
| SSMS general | Se ha corregido una incidencia que podía provocar que SSMS se bloqueara al enumerar las copias de seguridad en una carpeta |
| SSMS general | Se ha solucionado una incidencia que hacía que SSMS perdiera identificadores y memoria al mostrar los resultados de las consultas |
| SSMS general | Se ha corregido una incidencia por la que los usuarios no podían crear un índice de almacén de columnas en clúster para columnas de tipos concretos (UniqueIdentifier en SQL 2014 y versiones posteriores, y VarCharMax, NVarCharMax y VarBinaryMax en SQL 2017 y versiones posteriores) |
| SSMS general | Se ha corregido una incidencia pendiente desde hace mucho tiempo en la que la reordenación de las columnas XML en la "Cuadrícula de resultados" no funcionaba. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/32890930). |
| SSMS general | Se ha corregido una incidencia que hacía que SSMS mostrara de forma incorrecta una advertencia sobre "la incompatibilidad del seguimiento de cambios para una base de datos con objetos con optimización para memoria" |
| Asistente para generar scripts | Se ha corregido un error que impide la enumeración de objetos de base de datos en SQL Server 2014 y versiones anteriores. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). |
| Herramientas de replicación | Se ha corregido una incidencia por la que el monitor de replicación de SQL no respetaba la configuración de autoconexión, por lo que el usuario tiene que volver a conectarse manualmente cada vez. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/40394743). |
| SMO/Generación de Script | Se ha corregido una incidencia al intentar usar "Generar script..." en una vista cifrada en SQL Azure DB |
| Interfaz de usuario de XEvent | Se ha corregido una incidencia que podía provocar que SSMS se bloqueara al cambiar de otras aplicaciones a SSMS. |
| Interfaz de usuario de XEvent | Se ha revertido la mejora de 18.7 que causaba un error en la creación de XEvent debido a comillas adicionales. *Este cambio es temporal y la mejora se devolverá en una versión posterior sin el error que la acompaña.* |

#### <a name="known-issues-188"></a>Incidencias conocidas (18.8)

| Nuevo elemento | Detalles | Solución alternativa |
|----------|---------|------------|
| Analysis Services | Error al conectarse a SSAS a través de msmdpump.dll. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/D |
| Analysis Services | En casos excepcionales, cuando se utiliza la configuración de actualización, puede aparecer un error de objeto no configurado como instancia de un objeto al intentar abrir el editor DAX después de actualizar SSMS. | Desinstale y reinstale SQL Server Management Studio.  Si no se resuelve con la reinstalación, cierre todas las instancias de SSMS, haga una copia de seguridad y, a continuación, quite `%AppData%\Microsoft\SQL Server Management Studio` y `%LocalAppData%\Microsoft\SQL Server Management Studio`. |
| SSMS general | El cuadro de diálogo de nueva especificación de auditoría de servidor puede hacer que SSMS se bloquee con un error de infracción de acceso. | N/D |
| SSMS general | Las extensiones SSMS que usan SMO se deben volver a compilar para que tengan como destino el nuevo paquete v161 de SMO específico de SSMS. Hay una nueva versión preliminar disponible en https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/. </br></br> Las extensiones compiladas con las versiones anteriores a 160 del paquete Microsoft.SqlServer.SqlManagementObjects seguirán funcionando. | N/D |
| Integration Services | Al importar o exportar paquetes en Integration Services o exportar paquetes en Azure-SSIS Integration Runtime, se pierden los scripts para los paquetes que contienen tareas o componentes de script. | Quite la carpeta "C:\Archivos de programa (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". |
| Integration Services | Las conexiones remotas a los servicios de integración pueden generar un error que dice que el servicio especificado no existe como servicio instalado en un sistema operativo más reciente. | Identifique la ubicación del Registro relacionada con los servicios de integración en Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID y, en estos subárboles, cambie el nombre de la clave del Registro llamada "LocalService" por "LocalService_A" correspondiente a la versión específica de los servicios de integración a los que intenta conectarse. |


Puede hacer referencia a [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server) para obtener otras incidencias conocidas y proporcionar comentarios al equipo del producto.

## <a name="previous-ssms-releases"></a>Versiones de SSMS anteriores

[!INCLUDE[ssms-connect-aazure-ad](../includes/ssms-connect-azure-ad.md)]

Para descargar las versiones anteriores de SSMS, seleccione el vínculo de descarga en la sección relacionada.

| Versión de SSMS | Número de compilación | Fecha de la versión |
|--------------|--------------|--------------|
| [18.7.1](#1871) | 15.0.18358.0 | 27 de octubre de 2020 |
| [18,7](#187) | 15.0.18357.0 | 20 de octubre de 2020 |
| [18.6](#186) | 15.0.18338.0 | 22 de julio de 2020 |
| [18.5.1](#1851) | 15.0.18333.0 | 9 de junio de 2020 |
| [18.5](#185) | 15.0.18330.0 | 7 de abril de 2020 |
| [18.4](#184) | 15.0.18206.0 | 4 de noviembre de 2019 |
| [18.3.1](#1831) | 15.0.18183.0 | 2 de octubre de 2019 |
| [18.2](#182) | 15.0.18142.0 | 25 de julio de 2019 |
| [18.1](#181) | 15.0.18131.0 | 11 de junio de 2019 |
| [18.0](#180) | 15.0.18118.0 | 24 de abril de 2019 |
| [17.9.1](#1791) | 14.0.17289.0 | 21 de noviembre de 2018 |
| [16.5.3](#1653) | 13.0.16106.4 | 30 de enero de 2017 |

### <a name="1871"></a>18.7.1

![Descargar](media/download-icon.png) [Descargar SSMS 18.7](https://go.microsoft.com/fwlink/?linkid=2147207)

- Número de versión: 18.7.1
- Número de compilación: 15.0.18358.0
- Fecha de lanzamiento: 27 de octubre de 2020

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2147207&clcid=0x40a)

SSMS 18.7 es la versión de disponibilidad general (GA) más reciente de SSMS. Si necesita una versión anterior de SSMS, consulte [versiones de SSMS anteriores](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-1871"></a>Novedades de la versión 18.7.1

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]


#### <a name="bug-fixes-in-1871"></a>Correcciones de errores en la versión 18.7.1

| Nuevo elemento | Detalles |
|----------|---------|
| Almacén de consultas | Se ha corregido un error producido al hacer clic con el botón secundario en el nodo del explorador de objetos de Almacén de consultas. |


#### <a name="known-issues-1871"></a>Incidencias conocidas (18.7.1)

| Nuevo elemento | Detalles | Solución alternativa |
|----------|---------|------------|
| Analysis Services | Error al conectarse a SSAS a través de msmdpump.dll. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/D |
| Analysis Services | En casos excepcionales, cuando se utiliza la configuración de actualización, puede aparecer un error de objeto no configurado como instancia de un objeto al intentar abrir el editor DAX después de actualizar SSMS. | Para solucionar este problema, desinstale y vuelva a instalar SSMS. |
| Editor de consultas MDX | Al abrir el editor de consultas DAX, se produce el error de objeto no configurado como instancia de un objeto. | Desinstale y reinstale SQL Server Management Studio.  Si no se resuelve con la reinstalación, cierre todas las instancias de SSMS, haga una copia de seguridad y, a continuación, quite `%AppData%\Microsoft\SQL Server Management Studio` y `%LocalAppData%\Microsoft\SQL Server Management Studio`. |
| SSMS general | El cuadro de diálogo de nueva especificación de auditoría de servidor puede hacer que SSMS se bloquee con un error de infracción de acceso. | N/D |
| SSMS general | Las extensiones SSMS que usan SMO se deben volver a compilar para que tengan como destino el nuevo paquete v161 de SMO específico de SSMS. Hay una nueva versión preliminar disponible en https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/. </br></br> Las extensiones compiladas con las versiones anteriores a 160 del paquete Microsoft.SqlServer.SqlManagementObjects seguirán funcionando. | N/D |
| Asistente para generar scripts | Se produce un error en el asistente al intentar enumerar objetos de bases de datos en SQL Server 2014 y versiones anteriores. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). | Use SSMS 18.6 para seleccionar objetos en el asistente para generar scripts para SQL Server 2014 y versiones anteriores. |
| Integration Services | Al importar o exportar paquetes en Integration Services o exportar paquetes en Azure-SSIS Integration Runtime, se pierden los scripts para los paquetes que contienen tareas o componentes de script. | Quite la carpeta "C:\Archivos de programa (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". |
| Integration Services | Las conexiones remotas a los servicios de integración pueden generar un error que dice que el servicio especificado no existe como servicio instalado en un sistema operativo más reciente. | Identifique la ubicación del Registro relacionada con los servicios de integración en Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID y, en estos subárboles, cambie el nombre de la clave del Registro llamada "LocalService" por "LocalService_A" correspondiente a la versión específica de los servicios de integración a los que intenta conectarse. |
| Explorador de objetos | Las versiones de SSMS anteriores a 18.7 tienen un cambio importante en el explorador de objetos debido a los cambios del motor relacionados con el [grupo de SQL sin servidor de Azure Synapse Analytics](/azure/synapse-analytics/sql/on-demand-workspace-overview). | Para seguir usando el explorador de objetos en SSMS con el grupo de SQL sin servidor de Azure Synapse Analytics, necesita usar SSMS 18.7 o posterior. |

### <a name="187"></a>18,7

![Descargar](media/download-icon.png) [Descargar SSMS 18.7](https://go.microsoft.com/fwlink/?linkid=2146265)

- Número de versión: 18.7
- Número de compilación: 15.0.18357.0
- Fecha de lanzamiento: 20 de octubre de 2020

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2146265&clcid=0x40a)

SSMS 18.7 es la versión de disponibilidad general (GA) más reciente de SSMS. Si necesita una versión anterior de SSMS, consulte [versiones de SSMS anteriores](release-notes-ssms.md#previous-ssms-releases).

#### <a name="whats-new-in-187"></a>Novedades de la versión 18.7

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

| Nuevo elemento | Detalles |
|----------|---------|
| Integración de la instalación de Azure Data Studio | La instalación de SSMS también instala Azure Data Studio. |
| Always Encrypted | Para reconocer los nuevos puntos de conexión de HSM, debe actualizar SSMS. Para ello, se utiliza el nuevo proveedor de AKV NugetPackage. |
| Importar archivo plano | Se ha mejorado la predicción de los tipos de datos al aprender en 300 líneas de forma predeterminada. |
| Importar archivo plano | Evite que las columnas que deben ser SmallInt se declaren como TinyInt. |
| Importar archivo plano | Se realizó una mejora en la que las tablas DW se limpian correctamente, si hay un error en la importación de datos. |
| regulador de recursos | Compatibilidad agregada para valores decimales. |
| ShowPlan | Se ha agregado el operador PREDICT. |
| Interfaz de usuario de XEvent | Funcionalidad agregada para crear scripts de eventos extendidos con el nombre wait_type. Los usuarios solicitan utilizar el valor de la columna map_value en lugar de map_key en el predicado del filtro wait_type, ya que el valor de la clave está sujeto a cambios durante la actualización de la versión. Solución: Se ha agregado una casilla y se ha dado a los usuarios la opción de elegir entre map_value o map_key para el valor de predicado del filtro wait_type. |

#### <a name="bug-fixes-in-187"></a>Correcciones de errores en 18.7

| Nuevo elemento | Detalles |
|----------|---------|
| Accesibilidad | Se corrigió el orden de tabulación de los botones en la ventana "La siguiente configuración no es compatible con la base de datos" que aparece cuando se consulta en DW. |
| Accesibilidad | Asistente para importación y exportación: el diseño de página no es correcto en el modo de PPP alto. |
| Monitor de actividad | Se corrigió un problema en el que el monitor de actividad entraba en pausa al abrir la pestaña "Procesos". Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37050118). |
| Grupo de disponibilidad Always On | Se solucionó un problema por el cual la conmutación por error del grupo de disponibilidad de escalado de lectura no funcionaba. |
| Analysis Services | Se actualizaron los componentes de PowerQuery a 2.84.982 en escenarios de modelos tabulares de AS. |
| Analysis Services | Se corrigió un problema que podría haber provocado un error al intentar conectarse a SSAS a través de msmdpump.dll. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). |
| Copia de seguridad y restauración | Se solucionó un problema por el cual la selección de "Ver propiedades de conexión" producía un error SMO que causaba la falta de la propiedad HostDistribution para SQL 2016 y versiones anteriores. |
| Diseñador de bases de datos | Se corrigió un problema que hacía que SSMS se bloqueara al administrar números decimales. |
| Diagramas de base de datos | Se ha corregido un problema que podía provocar que SSMS se bloqueara o dejara de responder al usar diagramas de base de datos donde el cuadro de diálogo "Agregar tabla" no se mostraba correctamente. |
| Creación de reflejo de la base de datos | Se corrigió un problema que hacía que se produjera un error en la configuración del reflejo. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897281). |
| SSMS general | Se corrigió un problema al intentar conectarse a una base de datos SQL de Azure, que podía tardar varios segundos (inicio de sesión de SQL en una base de datos de usuario). |
| SSMS general | Se corrigió un problema por el que SSMS no administraba ni mostraba el interbloqueo capturado (archivos .xdl). |
| SSMS general | Se corrigió un problema por el cual el intento de abrir la configuración del registro de errores para SQL Server 2008 R2 y versiones anteriores daba un error acerca de que no se encontraba la propiedad ErrorLogSizeKb. |
| SSMS general | Correcciones generales y mejoras en la compatibilidad del [grupo de SQL sin servidor de Azure Synapse Analytics](/azure/synapse-analytics/sql/on-demand-workspace-overview). |
| Importar archivo plano | Se solucionó un problema por el cual el asistente no detectaba que el archivo podría estar en uso por otra aplicación y en su lugar generaba un error. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/40761574). |
| Importar/exportar aplicación de capa de datos | Se corrigió el nivel de servicio predeterminado para que sea Estándar S0 al importar un BACPAC (igual que Azure Portal y el comportamiento de SqlPackage.exe). |
| Importar archivo plano | Se solucionó un problema por el cual el asistente no detectaba que el archivo podría estar en uso por otra aplicación y en su lugar generaba un error. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/40761574). |
| Integration Services | Se solucionó un problema por el cual los elementos del cuadro combinado de Azure estaban duplicados en el Asistente para la creación de IR y el Asistente de migración de trabajos cuando diferentes suscripciones tenían el mismo nombre. |
| Integration Services | Se corrigió un problema que a veces el botón Conectar no se puede habilitar en el Asistente para la creación de IR. |
| Integration Services | Se solucionó un problema por el cual los elementos del cuadro combinado "Usar variable de entorno" en el cuadro de diálogo "Establecer valor de parámetro" no estaban en orden. |
| Intellisense | Se corrigió un bloqueo en SSMS que podía producirse al ejecutar una consulta. |
| Intellisense | Se corrigió un problema por el que IntelliSense no funciona cuando el usuario está conectado a un grupo de disponibilidad configurado para el enrutamiento de solo lectura. |
| Servidores vinculados | Se corrigió un problema por el cual un usuario con permiso CONTROL SERVER (pero no en el rol de administrador de sistemas) no podía agregar un servidor vinculado. |
| Visor de registros | Se corrigió un problema por el que un usuario con permisos VIEW SERVER STATE no podía ver los registros de errores de SQL Server. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/32899204). |
| Explorador de objetos | Se solucionó un problema por el cual, al seleccionar el menú **Iniciar PowerShell** en algunos nodos del Explorador de objetos (como "Administración de políticas", "Eventos extendidos"), PowerShell no se iniciaba correctamente. |
| Servidores registrados | Se corrigió un problema que hacía que SSMS se bloqueara al intentar registrar un servidor de administración central. |
| Servidores registrados | Se corrigió el problema por el cual faltaban los elementos del menú para iniciar Azure Data Studio desde los servidores registrados. |
| Informes | Se corrigió un problema por el cual en el Panel de rendimiento intentaba navegar a subvínculos (como **Consultas costosas**) sin conseguirlo. Este problema es común en la mayoría de las versiones que no están en inglés de SSMS. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/41454499). |
| ShowPlan | Se corrigió el problema que hacía que SSMS se bloqueara al usar Buscar nodo para buscar texto. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/40421650). |
| ShowPlan | Adición de un sufijo KB en la fila de información sobre herramientas de concesión de memoria |
| Evaluación de vulnerabilidad | Se corrigió un problema que hacía que SSMS produjera un error al intentar establecer líneas de base en la evaluación de vulnerabilidades. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/40578565). |
| Interfaz de usuario de XEvent | Se corrigió el problema que hacía que presionar F1 no se aterrizara en la página correcta de DOCS. |
| Interfaz de usuario de XEvent | Se corrigió el registro en el Visor de XEvent donde la información sobre herramientas no mostraba correctamente el texto que contenía texto codificado con pares suplentes. |

#### <a name="known-issues-187"></a>Problemas conocidos (18.7)

| Nuevo elemento | Detalles | Solución alternativa |
|----------|---------|------------|
| Analysis Services | Error al conectarse a SSAS a través de msmdpump.dll. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/D |
| Analysis Services | En casos excepcionales, cuando se utiliza la configuración de actualización, puede aparecer un error de objeto no configurado como instancia de un objeto al intentar abrir el editor DAX después de actualizar SSMS. | Para solucionar este problema, desinstale y vuelva a instalar SSMS. |
| SSMS general | El cuadro de diálogo de nueva especificación de auditoría de servidor puede hacer que SSMS se bloquee con un error de infracción de acceso. | N/D |
| SSMS general | Las extensiones SSMS que usan SMO se deben volver a compilar para que tengan como destino el nuevo paquete v161 de SMO específico de SSMS. Hay una nueva versión preliminar disponible en https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/. </br></br> Las extensiones compiladas con las versiones anteriores a 160 del paquete Microsoft.SqlServer.SqlManagementObjects seguirán funcionando. | N/D |
| Asistente para generar scripts | Se produce un error en el asistente al intentar enumerar objetos de bases de datos en SQL Server 2014 y versiones anteriores. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/41885587). | Use SSMS 18.6 para seleccionar objetos en el asistente para generar scripts para SQL Server 2014 y versiones anteriores. |
| Integration Services | Al importar o exportar paquetes en Integration Services o exportar paquetes en Azure-SSIS Integration Runtime, se pierden los scripts para los paquetes que contienen tareas o componentes de script. Solución alternativa: Quite la carpeta "C:\Archivos de programa (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". | N/D |
| Integration Services | Las conexiones remotas a los servicios de integración pueden generar un error que dice que el servicio especificado no existe como servicio instalado en un sistema operativo más reciente. Solución alternativa: Identifique la ubicación del Registro relacionada con los servicios de integración en Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID y, en estos subárboles, cambie el nombre de la clave del Registro llamada "LocalService" por "LocalService_A" correspondiente a la versión específica de los servicios de integración a los que intenta conectarse. | No aplicable |
| Explorador de objetos | Las versiones de SSMS anteriores a 18.7 tienen un cambio importante en el explorador de objetos debido a los cambios del motor relacionados con el [grupo de SQL sin servidor de Azure Synapse Analytics](/azure/synapse-analytics/sql/on-demand-workspace-overview). | Para seguir usando el explorador de objetos en SSMS con el grupo de SQL sin servidor de Azure Synapse Analytics, necesita SSMS 18.7 o posterior. |
| Almacén de consultas | El nodo del explorador de objetos en Almacén de consultas produce un error al hacer clic con el botón secundario. | Expanda el nodo y haga clic con el botón secundario en opciones secundarias individuales para acceder directamente a los elementos. |

### <a name="186"></a>18.6

![Descargar](media/download-icon.png) [Descargar SSMS 18.6](https://go.microsoft.com/fwlink/?linkid=2135491)

- Número de versión: 18.6
- Número de compilación: 15.0.18338.0
- Fecha de lanzamiento: 22 de julio de 2020

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2135491&clcid=0x40a)

#### <a name="whats-new-in-186"></a>Novedades de la versión 18.6

| Nuevo elemento | Detalles |
|----------|---------|
| Analysis Services | Se ha actualizado a la versión más reciente de las bibliotecas de cliente de AS. |
| Auditoría | Se ha agregado compatibilidad con el id. de acción SENSITIVE_BATCH_COMPLETED_GROUP (cadena en lugar de un número). |
| Auditoría | Se han agregado los campos siguientes al visor de auditoría: affected_rows, response_rows, connection_id, duration_milliseconds y data_sensitivity_information. |
| Clasificación de datos | Actualice SSMS para admitir la importación y exportación de la directiva exportada a través de cmdlets de PowerShell. |
| Importar archivo plano | Se ha agregado compatibilidad con los archivos de ancho fijo y la detección de tipos de archivo para los archivos .csv o .tsv a fin de garantizar que se analizan como archivos CSV o TSV, respectivamente. |
| Integration Services | Se ha agregado compatibilidad con los trabajos del agente de Azure SQL Managed Instance para ejecutar un paquete SSIS desde el almacén de paquetes en Azure-SSIS IR. |
| SMO/Creación de script | Se ha agregado compatibilidad con el script Enmascaramiento dinámico de datos en [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is) (anteriormente Azure SQL DW). |
| SMO/Creación de script | Se ha agregado compatibilidad con el script Directiva de seguridad en [Azure Synapse Analytics](/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is). |

#### <a name="bug-fixes-in-186"></a>Correcciones de errores en 18.6

| Nuevo elemento | Detalles |
|----------|---------|
| Accesibilidad | Colores del borde ajustados para permitir accesibilidad en la **página de propiedades generales de la base de datos** (el borde alrededor de la cuadrícula y el cuadro del nombre es más oscuro para establecer que el contraste sea > 3:1). |
| Accesibilidad | Control agregado para que la ejecución de la consulta actualice el narrador (requiere NetFx4.8+ instalado en la máquina). |
| Always Encrypted | Se ha corregido una incidencia que provocaba que el cuadro de diálogo *Nueva clave de cifrado de columnas* indicara que la CEK no estuviera habilitada para enclave, aunque la CMK sí lo estuviera. |
| Analysis Services | Se ha corregido una incidencia al ver las particiones de Analysis Services que pueden haber provocado una excepción no controlada. |
| **Diagramas de base de datos** | Se ha corregido una incidencia pendiente con los **diagramas de base de datos**, que provocaba que se produjeran daños en los diagramas existentes y que SSMS se bloqueara. Si ha creado o guardado un diagrama mediante SSMS 18.0 a través de 18.5.1 y ese diagrama incluye una *Anotación de texto*, no podrá abrir ese diagrama en ninguna versión de SSMS. Con esta corrección, SSMS 18.6 puede abrir y guardar un diagrama creado en SSMS 17.9.1 y versiones anteriores. SSMS 17.9.1 y versiones anteriores también pueden abrir el diagrama después de guardarse en SSMS 18.6. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649). |
| Clasificación de datos | Se ha corregido una incidencia que provocaba que el nombre de columna no se mostrara en el panel de recomendaciones del panel clasificación de datos. |
| SSMS general | Se ha corregido una incidencia que provocaba que las propiedades de la base de datos *Tamaño* y *Espacio disponible* tuvieran valores incorrectos para SQL Azure DB (nivel de servicio de hiperescala). |
| SSMS general | Se ha corregido una incidencia que provocaba que las propiedades "Tamaño" de la base de datos mostraran el tamaño máximo en lugar del tamaño real de la base de datos para las bases de datos de SQL Azure. Nota: Para DW, sigue mostrando el tamaño máximo. |
| SSMS general | Se han solucionado tres orígenes comunes de bloqueos en SSMS. |
| SSMS general | Se han corregido algunas incidencias relacionadas con las entradas *olvidar* del cuadro de diálogo de conexión SSMS (servidor/usuario/contraseñas). Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/40256401) y [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/40015519). |
| SSMS general | Se ha corregido una incidencia en el cuadro de diálogo **Propiedades de estadísticas** que provocaba que la selección de la casilla **Actualizar estadísticas de estas columnas** y de **Aceptar** no produjera ningún efecto. Las estadísticas no se actualizan y, al intentar crear un script de la acción, se produce un mensaje *No hay ninguna acción para incluir en el script*. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37799992). |
| SSMS general | Se han solucionado los problemas relacionados con [CVE-2020-1455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2020-1455). | 
| Importar/exportar aplicación de capa de datos | Se ha corregido una incidencia que hacía que SSMS produjera un error al importar un archivo bacpac. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/40229137). |
| Integration Services | Se ha corregido un error que hacía que los clientes no pudieran editar un paso de trabajo del Agente SQL al usar las versiones 18.4 o anteriores de SSMS para ejecutar paquetes SSIS en Azure SQL Managed Instance. |
| Integration Services | Se ha corregido un error que hacía que la opción **Usar motor en tiempo de ejecución de 32 bits** faltara en la pestaña **Opciones de ejecución** para ejecutar un paquete SSIS en un paso de trabajo del Agente SQL para un servidor SQL Server local. |
| Intellisense/Editor | Se ha corregido una incidencia que provocaba que pudiera aparecer un cuadro de diálogo de error al elegir la opción Archivo -> Nuevo -> Consulta de motor de base de datos. |
| Explorador de objetos | Se ha corregido una incidencia que provocaba que la *ventana Propiedades* no estuviera disponible para Azure SQL Database al hacer clic con el botón derecho en un nodo de tabla o índice en el Explorador de objetos. |
| Explorador de objetos | Se ha solucionado una incidencia que hacía que SSMS no pudiera expandir el nodo Bases de datos para la maestra en Azure si había una interrupción del plano de control que afectara a sys.database_service_objectives. |
| Informes | Se han corregido varios informes estándar que se habían dañado en Linux. </br></br> Ejemplo: Había un error en el informe de consumo de memoria similar a "/var/opt/mssql/log/log_116.trc\log.trc' is invalid…"). |
| SMO/Creación de script | Se ha actualizado la lógica para crear bases de datos en Azure SQL Database para usar Gen5_2 como SLO predeterminado. |
| Interfaz de usuario de XEvent | Se ha solucionado una incidencia pendiente desde hace mucho tiempo (introducido en SSMS 18.0) que provocaba que "Guardar en el archivo XEL..." produjera un error. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37695592). |

#### <a name="known-issues-186"></a>Incidencias conocidas (18.6)

| Nuevo elemento | Detalles | Solución alternativa |
|----------|---------|------------|
| Analysis Services | Error al conectarse a SSAS a través de msmdpump.dll. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696). | N/D |
| SSMS general | El cuadro de diálogo de nueva especificación de auditoría de servidor puede hacer que SSMS se bloquee con un error de infracción de acceso. | N/D |
| SSMS general | Las extensiones SSMS que usan SMO se deben volver a compilar para que tengan como destino el nuevo paquete v161 de SMO específico de SSMS. Hay una nueva versión preliminar disponible en https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects.SSMS/. </br></br> Las extensiones compiladas con las versiones anteriores a 160 del paquete Microsoft.SqlServer.SqlManagementObjects seguirán funcionando. | N/D |
| Integration Services | Al importar o exportar paquetes en Integration Services o exportar paquetes en Azure-SSIS Integration Runtime, se pierden los scripts para los paquetes que contienen tareas o componentes de script. Solución alternativa: Quite la carpeta "C:\Archivos de programa (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". | N/D|
| Integration Services | Las conexiones remotas a los servicios de integración pueden generar un error que dice que el servicio especificado no existe como servicio instalado en un sistema operativo más reciente. Solución alternativa: Identifique la ubicación del Registro relacionada con los servicios de integración en Computer\HKEY_CLASSES_ROOT\AppID & Computer\HKEY_CLASSES_ROOT\ WOW6432Node\AppID y, en estos subárboles, cambie el nombre de la clave del Registro llamada "LocalService" por "LocalService_A" correspondiente a la versión específica de los servicios de integración a los que intenta conectarse. | N/D|

### <a name="1851"></a>18.5.1

![Descargar](media/download-icon.png) [Descargar SSMS 18.5.1](https://go.microsoft.com/fwlink/?linkid=2132606)

- Número de versión: 18.5.1
- Número de compilación: 15.0.18333.0
- Fecha de lanzamiento: 9 de junio de 2020

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2132606&clcid=0x40a)

#### <a name="bug-fixes-in-1851"></a>Correcciones de errores en 18.5.1

| Nuevo elemento | Detalles |
|----------|---------|
| Analysis Services | Rendimiento mejorado al expandir la lista de bases de datos mientras están conectados a servidores de AS Azure o Power BI. |
| Analysis Services | Se ha corregido una incidencia que hacía que se produjera un error al intentar abrir el Asistente para sincronizar bases de datos de un servidor de Analysis Services. |
| Analysis Services | Se ha corregido una incidencia que impedía a los usuarios consultar SSAS 2017 y versiones anteriores con permisos de datos de celda. |
| SSMS general | [Diseñador de tablas: se ha corregido un sonido al intentar hacer tabulación en una cuadrícula del Diseñador de tablas.](https://feedback.azure.com/forums/908035/suggestions/40318435) |

#### <a name="known-issues-1851"></a>Problemas conocidos (18.5.1)

| Nuevo elemento | Detalles | Solución alternativa |
|----------|---------|------------|
| SSMS general | Hay un error conocido con el Diseño de diagramas que provoca que los diagramas existentes se dañen. Por ejemplo, puede crear un diseño de diagrama con SSMS 17.9.1, después actualizarlo o guardarlo con SSMS 18.x y, posteriormente, intentar abrirlo con 17.9.1. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) para obtener información detallada. | N/D |
| SSMS general | El cuadro de diálogo de nueva especificación de auditoría de servidor puede hacer que SSMS se bloquee con un error de infracción de acceso. | N/D ||
| SMO/Generación de Script | Las extensiones SSMS que usan SMO se deben volver a compilar para que tengan como destino el nuevo paquete v160 de SMO. | N/D |
| Integration Services | Al importar o exportar paquetes en Integration Services o exportar paquetes en Azure-SSIS Integration Runtime, se pierden los scripts para los paquetes que contienen tareas o componentes de script. Solución: | Quite la carpeta "C:\Archivos de programa (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild". |

### <a name="185"></a>18.5

![Descargar](media/download-icon.png) [Descargar SSMS 18.5](https://go.microsoft.com/fwlink/?linkid=2125901)

- Número de versión: 18.5
- Número de compilación: 15.0.18330.0
- Fecha de lanzamiento: 7 de abril de 2020

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2125901&clcid=0x40a)

#### <a name="whats-new-in-185"></a>Novedades de la versión 18.5

| Nuevo elemento | Detalles |
|----------|---------|
| Analysis Services | Se ha agregado compatibilidad para el punto de conexión de Power BI en Analysis Services, funcionalidad coincidente de Azure Analysis Services. |
| Analysis Services | Profiler: se ha agregado compatibilidad con la definición de seguimiento 15.1 de Analysis Services. |
| Clasificación de datos | Se ha agregado un botón a la vista de resultados del examen de evaluación de vulnerabilidades con el fin de corregir la regla de clasificación de datos yendo al panel de clasificación de datos. |
| Clasificación de datos | Se ha agregado compatibilidad con el rango de confidencialidad en la clasificación de datos. |
| Hiperescala | Se ha agregado compatibilidad para la *importación de la aplicación de capa de datos* (.bacpac) con la hiperescala de SQL Azure. |
| Integration Services | Se ha agregado compatibilidad con la ejecución de un paquete SSIS desde el sistema de archivos en el trabajo del agente de MI. |
| Integration Services | Se han hecho mejoras para facilitar el uso de la configuración de DTExec habilitado para Azure para invocar ejecuciones de paquetes SSIS en Azure-SSIS Integration Runtime.
| Integration Services | Se ha agregado compatibilidad con la conexión de Azure-SSIS Integration Runtime y la administración o la ejecución de paquetes SSI en almacenes de paquetes.
| Integration Services | Se ha agregado compatibilidad con la migración de trabajos del agente de SSIS local a canalizaciones y desencadenadores de ADF.
| Integration Services | Se ha mejorado la experiencia del usuario para la exportación de proyectos de SSIS desde una base de datos de SSIS. En comparación con la anterior exportación, que cargaba y actualizaba los paquetes en el proyecto de SSIS, la nueva exportación independiente de la versión no cargará ni actualizará los paquetes en el proyecto de SSIS. En su lugar, mantiene los paquetes en los proyectos tal como están en la base de SSIS, excepto el cambio de nivel de protección a EncryptSensitiveWithUserKey. |
| SMO/Generación de Script | Se ha agregado la nueva propiedad DwMaterializedViewDistribution al objeto de vista. |
| SMO/Generación de Script | Se ha quitado la compatibilidad con la *restricción de características* (esta característica en versión preliminar se ha quitado de SQL Azure y SQL local). |
| SMO/Generación de Script | Se ha agregado el *cuaderno* como destino para el Asistente para generar scripts. |
| SMO/Generación de Script | Se ha agregado compatibilidad para el *grupo de SQL sin servidor de Azure Synapse Analytics*. |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): los campos Platform, Name y engineEdition ahora pueden contener listas separadas por comas habituales (*plataforma*: \[*Windows*, *Linux*\]), no solo expresiones regulares (*plataforma*: *\/Windows\|Linux\/* )
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se han agregado 13 reglas de evaluación. Para más información, consulte [GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-assessment-api). |

#### <a name="bug-fixes-in-185"></a>Correcciones de errores en 18.5

| Nuevo elemento | Detalles |
|----------|---------|
| Accesibilidad | SSIS ADF/nueva programación: se ha corregido un problema por el que el orden de enfoque no es lógico en el modo de recorrido del narrador en el Asistente para *nueva programación*. |
| Accesibilidad | Asistente para base de datos de extensión: se ha corregido un problema por el que el lector de pantalla no informa sobre el nombre de la tabla de consulta al proporcionar información sobre la tabla. |
| Analysis Services | Se ha corregido la conexión almacenada en caché al generar scripts en AS con la conexión de Azure AD. |
| Always On | Se ha corregido un problema por el que la primera base de datos agregada al grupo de disponibilidad AlwaysOn no se une correctamente.
| Always On | Se ha corregido un problema por el que se mostraba un error al intentar mostrar el panel al conectarse a un punto de conexión de clúster de macrodatos. |
| Auditoría | Se ha corregido un problema por el que la ventana de combinación de registros de auditoría se bloquea cuando hay una carpeta con un nombre vacío en la carpeta raíz de la cuenta de almacenamiento. |
| Auditoría | Se ha corregido un problema por el que la ventana de combinación de registros de auditoría no muestra todos los servidores cuando hay demasiados elementos en la raíz del contenedor. |
| Clasificación de datos | Se ha corregido un problema por el que el Asistente para la *clasificación de datos* no se abrirá para las bases de datos con un gran número de tablas. |
| Clasificación de datos | Ahora se aplican diferentes GUID para cada estructura label/infoType y GUID en el proceso de validación. |
| Clasificación de datos | Se ha quitado el proceso de clasificación en SqlServer2019. |
| Clasificación de datos | Se van a corregir las pruebas de validación anteriores (agregar rangos, quitar la propiedad no válida *InformationTypes*) y se van a agregar otras nuevas para los dos primeros puntos. |
| Clasificación de datos | El botón situado justo encima de la tabla de columnas clasificadas ahora minimiza el panel de recomendaciones, tal como se indica. |
| SSMS general | Se va a actualizar la versión de los controladores MSODBC y MSOLEDB. |
| SSMS general | Se ha corregido el hecho de que al menos dos orígenes comunes se bloquean en SSMS. |
| SSMS general | Se ha solucionado un caso más en el que el *cuadro de diálogo de restauración* se bloquea al seleccionar el botón Examinar. |
| SSMS general | Se ha corregido la *nueva interfaz gráfica de usuario de base de datos* para el grupo de SQL sin servidor de Azure Synapse Analytics. |
| SSMS general | Se han corregido las plantillas *Nueva tabla externa...* y *Nuevo origen de datos externo...* para el grupo de SQL sin servidor de Azure Synapse Analytics. |
| SSMS general | Se han corregido propiedades de base de datos y propiedades de conexión, se van a ocultar informes y se va a cambiar el nombre del grupo de SQL sin servidor de Azure Synapse Analytics. |
| SSMS general | Always Encrypted: Se ha corregido un problema por el que la lista desplegable de nombres de clave se hace de solo lectura al seleccionar la nueva clave habilitada para enclave. |
| SSMS general | Se ha limpiado la cuadrícula de *opciones de propiedad de base de datos*, que mostraba dos *categorías varias*. |
| SSMS general | Se ha corregido un problema por el que la barra de desplazamiento comenzaba desde el centro en la cuadrícula de "opciones de propiedad de base de datos". |
| SSMS general | Se ha corregido un problema que hacía que SSMS se bloqueara al abrir el archivo .sql mientras estaba conectado al servidor de Analysis Services. |
| SSMS general | Cuadro de diálogo de conexión: se ha corregido un problema por el que la desactivación de "Recordar contraseña" no funciona. |
| SSMS general | Se ha corregido un problema por el que siempre se recuerdan las credenciales asociadas al servidor o a los usuarios. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37875172). |
| SSMS general | Se ha corregido un problema por el que algunas ventanas del editor no se actualizaban correctamente. Esto se consigue deshabilitando la aceleración de hardware en *Herramientas > Opciones > Entorno*. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). |
| SSMS general | Se ha corregido un problema por el que la autenticación de Azure Active Directory no funcionaba a través de un proxy. |
| Valores altos de PPP/escalado | Se ha corregido un problema que hacía que los controles de las *propiedades de índice* se representaran incorrectamente (botones superpuestos a la cuadrícula). Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/36030424). |
| Valores altos de PPP/escalado | Se han corregido varios problemas en el cuadro de diálogo de *propiedades de la base de datos*, que podría mostrar controles recortados en pantallas de 4K. |
| Valores altos de PPP/escalado | Se han corregido los asistentes para publicación y suscripción en pantallas de 4K. |
| Valores altos de PPP/escalado | Corrección secundaria en la página de nueva especificación de auditoría de servidor. |
| Valores altos de PPP/escalado | Se ha corregido un problema de visualización de 4K en el Asistente para alta disponibilidad. |
| Valores altos de PPP/escalado | Se ha corregido un problema que hacía que el usuario no pudiera agregar un destino en una ventana de nueva sesión de XEvent + establecer filtros de eventos de sesión en el Asistente para sesiones de XEvent al escalar la pantalla al 125 %. |
| Valores altos de PPP/escalado | Se va a corregir un problema por el que los controles de la interfaz de usuario de la *copia de seguridad de la base de datos en la dirección URL* se quedaban ocultos en un escalado superior al 100 %. |
|Importación de archivo plano | Se ha actualizado el Asistente para la importación de archivos planos para permitir comprobar todo en la columna Permitir NULL. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/38027137). |
| Explorador de objetos | Se ha corregido un problema por el que Explorador de objetos podía mostrar información incorrecta cuando se usan cadenas de conexión para conectarse en el cuadro de diálogo de conexión. |
| Explorador de objetos | Se ha corregido un problema por el que OE tardaba en expandir tablas para bases de datos con varios miles de tablas (+ de 20 000). |
| Interfaz de usuario de Almacén de consultas | Se ha corregido el hecho de que el informe de TRC calculase el recuento de ejecuciones (para la métrica *tiempo de espera*) como la suma de los recuentos de ejecución de cada categoría de espera individual, lo cual es incorrecto. Pero para una sola ejecución de consulta, se registrará para cada una de las categorías de espera que esperaba la consulta. Por lo tanto, si TRC hace la suma a través de la categoría de espera, sobrecargará el recuento de ejecuciones. De hecho, debe ser el máximo en wait_category. |
| Interfaz de usuario de Almacén de consultas | Se ha corregido que la vista detallada de TRC devolviera datos incorrectos cuando se filtra el conjunto de resultados en la x superior. Esto sucede porque la consulta utiliza varias expresiones de tabla comunes, que después se unen para crear el conjunto de resultados final. Si la x superior se inserta en CTE, a veces puede filtrar las filas necesarias. En ocasiones, esto puede hacer que el conjunto de resultados sea no determinista. La solución consiste en no insertar la cláusula de la x superior en CTE. |
| Interfaz de usuario de Almacén de consultas | Se ha corregido que el resumen del plan en ambas vistas (cuadrícula y gráfico) requiriesen el último tiempo de espera de ejecución de la consulta. La ausencia de esta columna está interrumpiendo la consulta. Este conjunto de cambios agregará esta columna a la CTE de estadísticas de espera. |
| ShowPlan | Se ha mejorado la forma en que SSMS muestra recuentos de filas estimados para operadores con varias ejecuciones: (1) Se ha modificado el *número estimado de filas* en SSMS a "Número estimado de filas por ejecución"; (2) Se ha agregado una nueva propiedad de *número estimado de filas para todas las ejecuciones*; (3) Se ha modificado la propiedad de *número real de filas* a otra de *número real de filas para todas las ejecuciones*. |
| Agente SQL | Se ha corregido un problema por el que al intentar editar un paso de trabajo del Agente SQL se podría haber provocado el bloqueo de la interfaz de usuario de SSMS. SSMS ahora permite ver (botón *Ver*) un archivo output_file cuyo nombre se ha convertido en token (por lo menos para las macros o tokens simples admitidos por el Agente SQL que no se determinan en el entorno de ejecución). Además, SSMS no deshabilita el botón "Ver" cuando el usuario no tiene acceso al archivo (en lo que se refiere a los permisos de SQL). Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/39063124). |
| Agente SQL | Se ha corregido el orden de tabulación en la página de paso de trabajo. |
| Agente SQL | Se ha invertido la posición de los botones "Siguiente" y "Anterior" en la página de paso de trabajo para colocarlos en un orden lógico. |
| Agente SQL | Se ha ajustado la ventana de programación del trabajo para que no recorte la interfaz de usuario. |
| SMO/Generación de Script | Se ha corregido la generación del script de base de datos para el grupo de SQL sin servidor de Azure Synapse Analytics. |
| SMO/Generación de Script | Se va a quitar la conversión SQLVARIANT explícita (sintaxis T-SQL no válida para SqlOnDemand) que corrige el scripting para SqlOnDemand. |
| SMO/Generación de Script | Se ha corregido un problema en el que se omitió FILLFACTOR en los índices de SQL Azure. |
| SMO/Generación de Script | Se ha corregido un problema relacionado con el scripting de objetos externos. |
| SMO/Generación de Script | Se ha corregido un problema por el que *Generar scripts* no permitía elegir la opción de scripting para las propiedades extendidas en SQL Database. Además, se ha corregido el scripting de dichas propiedades extendidas. |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): vínculo de ayuda incorrecto en la regla de XTPHashAvgChainBuckets. |
| Interfaz de usuario de XEvent | Se ha corregido un problema por el que los elementos de la cuadrícula se seleccionaban al mantener el puntero. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/38262124) y [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/37873921). |

#### <a name="known-issues-185"></a>Problemas conocidos (18.5)

| Nuevo elemento | Detalles | Solución alternativa |
|----------|---------|------------|

- El diagrama de base de datos creado a partir de un conjunto SSMS que se ejecuta en la máquina A no se puede modificar desde la máquina B (bloquearía SSMS). Vea [Comentarios del usuario 37992649 de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) para obtener información detallada.

- Al importar o exportar paquetes en Integration Services o exportar paquetes en Azure-SSIS Integration Runtime, se pierden los scripts para los paquetes que contienen tareas o componentes de script. Una solución alternativa es quitar la carpeta *C:\Program Files (x86)\Microsoft SQL Server Management Studio 18\Common7\IDE\CommonExtensions\MSBuild*.

- El cuadro de diálogo de nueva especificación de auditoría de servidor puede hacer que SSMS se bloquee con un error de infracción de acceso.

- Es necesario volver a compilar las extensiones de SSMS con SMO para que tengan como destino el nuevo SMO v160 (el paquete estará disponible en nuget.org justo después de la publicación de SSMS 18.5).

- [Error al conectarse a SSAS a través de msmdpump.dll en SSMS](https://feedback.azure.com/forums/908035-sql-server/suggestions/40144696-error-when-connecting-to-ssas-via-msmdpump-dll-in).

### <a name="184"></a>18.4

![descargar](media/download-icon.png) [Descargar SSMS 18.4](https://go.microsoft.com/fwlink/?linkid=2108895)

- Número de versión: 18.4
- Número de compilación: 15.0.18206.0
- Fecha de lanzamiento: 4 de noviembre de 2019

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2108895&clcid=0x40a)

| Nuevo elemento | Detalles |
|----------|---------|
| Clasificación de datos | Se ha agregado compatibilidad con la directiva de protección de la información personalizada para la clasificación de datos. |
| Almacén de consultas | Se ha agregado el valor *Planes máximos por consulta* en las propiedades del cuadro de diálogo. |
| Almacén de consultas | Se ha agregado compatibilidad para las nuevas directivas de captura personalizadas. |
| Almacén de consultas | Se agregó el **modo de captura de las estadísticas de espera** a las opciones **Almacén de consultas** **Propiedades de la base de datos**. |
| SMO/Generación de Script | Script de compatibilidad de la vista materializada en SQL DW. |
| SMO/Generación de Script | Se ha agregado compatibilidad para el *grupo de SQL sin servidor de Azure Synapse Analytics*. |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se han agregado 50 reglas de evaluación (consulte los detalles en GitHub). |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se han agregado expresiones matemáticas básicas y comparaciones a las condiciones de las reglas. |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se ha agregado compatibilidad con el objeto RegisteredServer. |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se ha actualizado la forma en que se almacenan las reglas en formato JSON y también el mecanismo de aplicación de invalidaciones o personalizaciones. |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se han actualizado las reglas para incluir compatibilidad de SQL en Linux. |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se ha actualizado el formato JSON del conjunto de reglas y se ha agregado la versión de SCHEMA. |
| SMO/Generación de Script | [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se ha actualizado la salida de los cmdlets para mejorar la legibilidad de las recomendaciones. |
| Generador de perfiles XEvent | Se ha agregado el evento *error_reported* a las sesiones del generador de perfiles XEvent. |

#### <a name="bug-fixes-in-184"></a>Correcciones de errores en 18.4

| Nuevo elemento | Detalles |
|----------|---------|
| Analysis Services | Se ha corregido un problema por el que el editor de scripts DAX para bases de datos multidimensionales no mostraba tablas en IntelliSense. |
| Analysis Services | Use el analizador DAX para efectuar una conversión a una cadena de motor. Sirve para los separadores internacionales, decimales y espacios en blanco. |
| Always Encrypted | Se ha corregido un problema por el que la *validación de notificaciones* no *distinguía mayúsculas de minúsculas*. |
| Always Encrypted | Se ha corregido un problema por el que los informes de errores o advertencias no funcionaban correctamente. |
| Asistente para copiar bases de datos | Se han corregido varios problemas de truncamiento y diseño en la representación de este cuadro de diálogo. |
| SSMS general | Se ha corregido un problema pendiente desde hace tiempo por el que SSMS no respetaba la información de conexión que se pasaba en la línea de comandos cuando también se especificaban archivos SQL. |
| SSMS general | Se ha corregido un bloqueo en SSMS al intentar mostrar elementos protegibles en los objetos "Filtro de replicación". |
| SSMS general | Se ha mitigado la eliminación de la opción de la línea de comandos -P haciendo que SSMS examine su caché de credenciales: si se encuentra la credencial necesaria, se establecerá la conexión con ella. |
| Importación de archivo plano | Se ha corregido un problema por el que la funcionalidad *Importar archivo sin formato* no controlaba correctamente los calificadores de texto. |
| Explorador de objetos | Se ha corregido un problema por el que, al quitar una base de datos de Azure SQL Database en el Explorador de objetos, se mostraba un mensaje incorrecto. |
| Resultados de la consulta | Se ha corregido un problema que apareció en SSMS 18.3.1 por el que las cuadrículas se dibujaban demasiado estrechas y mostraban *...* al final de la cadena más larga de cada columna. |
| Herramientas de replicación | Se ha corregido un problema que provocaba que la aplicación generara un error ("No se pudo cargar un archivo o ensamblado...") al intentar editar trabajos del Agente SQL. |
| SMO/Generación de Script | Se ha corregido un problema cuando no funcionaba *Generar script de tabla como…* para SQL DW cuya intercalación es Japanese_BIN2.|
| SMO/Generación de Script | Se ha corregido un problema que hacía que ScriptAlter() terminara de ejecutar las instrucciones en el servidor.|
| Agente SQL | Se ha corregido un problema por el que la interfaz de usuario del operador del agente no actualizaba el nombre del operador cuando se cambiaba en la interfaz de usuario, ni se generaba el script correspondiente. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/32897647) para obtener información detallada.|

#### <a name="known-issues-184"></a>Problemas conocidos (18.4)

- El diagrama de base de datos creado a partir de un conjunto SSMS que se ejecuta en la máquina A no se puede modificar desde la máquina B (bloquearía SSMS). Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) para obtener información detallada.

- Existen incidencias de rediseño al cambiar entre varias ventanas de consulta. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042) para obtener información detallada. Una solución para esta incidencia es deshabilitar la aceleración de hardware en *Herramientas > Opciones*.

Puede hacer referencia a [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server) para obtener otras incidencias conocidas y proporcionar comentarios al equipo del producto.

### <a name="1831"></a>18.3.1

![descargar](media/download-icon.png) [Descargar SSMS 18.3.1](https://go.microsoft.com/fwlink/?linkid=2105412)

- Número de versión: 18.3.1
- Número de compilación: 15.0.18183.0
- Fecha de lanzamiento: 2 de octubre de 2019

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2105412&clcid=0x40a)

#### <a name="whats-new-in-1831"></a>Novedades de 18.3.1

| Nuevo elemento | Detalles |
|----------|---------|
| Clasificación de datos | Agregue información de clasificación de datos a la interfaz de usuario de propiedades de columna (*Tipo de información*, *Id. de tipo de información*, *Etiqueta de confidencialidad* e *Id. de etiqueta de confidencialidad* no se exponen en la interfaz de usuario de SSMS). |
| Intellisense/Editor | Se ha actualizado la compatibilidad con las características agregadas recientemente a SQL Server 2019 (por ejemplo, *ALTER SERVER CONFIGURATION*). |
| Integration Services | Se ha agregado un nuevo elemento de menú de selección `Tools > Migrate to Azure > Configure Azure-enabled DTExec` que invoca las ejecuciones de paquetes SSIS en Azure-SSIS Integration Runtime como actividades de Ejecutar paquetes SSIS en canalizaciones de ADF. |
| SMO/Generación de Script | Se ha agregado compatibilidad con la generación de script de la restricción UNIQUE de Azure SQL Data Warehouse. |
| SMO/Generación de Script | Clasificación de datos </br> - Se ha agregado compatibilidad con la versión 10 de SQL (SQL 2008) y versiones posteriores. </br> - Se ha agregado el nuevo atributo de sensibilidad "rank" para la versión 15 de SQL (SQL 2019) y versiones posteriores y para Azure SQL Database. |
| SMO/Generación de Script | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se ha agregado el control de versiones al formato ruleset. |
| SMO/Generación de Script | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se han agregado nuevas comprobaciones. |
| SMO/Generación de Script | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): se ha agregado compatibilidad con Azure SQL Managed Instance. |
| SMO/Generación de Script | [API SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md): vista predeterminada actualizada de los cmdlets para mostrar los resultados como una tabla. |

#### <a name="bug-fixes-in-1831"></a>Correcciones de errores en 18.3.1

| Nuevo elemento | Detalles |
|----------|---------|
| Analysis Services | Se ha corregido la incidencia de escalado en el Editor de consultas MDX.|
| Analysis Services | Se ha corregido una incidencia en la interfaz de usuario de XEvent que impedía que los usuarios pudieran crear una nueva sesión. |
| Implementación de base de datos en SQL Azure | Se ha corregido una incidencia (en DacFx) que provocaba que esta característica no funcionara.|
| SSMS general | Se ha corregido una incidencia que hacía que SSMS se bloqueara al usar la característica de ordenación en el visor de XEvent. |
| SSMS general | Se han corregido incidencias pendientes que provocaban que fuera posible que la base de datos de restauración de SSMS no respondiera indefinidamente. </br></br> Vea los elementos de los Comentarios del usuario de SQL Server para obtener información detallada: </br> [Restaurar base de datos: seleccione los dispositivos de copia de seguridad lentos que se van a cargar](https://feedback.azure.com/forums/908035/suggestions/32899099/).  </br> [SSMS 2016 es muy lento en los cuadros de diálogo de restauración de bases de datos](https://feedback.azure.com/forums/908035/suggestions/32900767/). </br> [La restauración de la base de datos es lenta](https://feedback.azure.com/forums/908035/suggestions/32900224/).  </br> [La restauración de la base de datos desde el dispositivo NO RESPONDE al hacer clic en "..."](https://feedback.azure.com/forums/908035/suggestions/34281658/).  |
| SSMS general | Se ha corregido una incidencia por la que el idioma predeterminado que se mostraba en todos los inicios de sesión era el árabe. </br></br> Vea el elemento de los Comentarios del usuario de SQL Server para obtener información detallada: [Error de visualización de idioma predeterminado en SSMS 18.2](https://feedback.azure.com/forums/908035/suggestions/38236363). |
| SSMS general | Se ha corregido la dificultad que había al ver el cuadro de diálogo *Opciones de consulta* (cuando el usuario hacía clic con el botón derecho en la ventana del editor de T-SQL) y ahora se puede ajustar.|
| SSMS general | El mensaje *Hora de finalización* visible en la cuadrícula o el archivo de resultados (introducido en SSMS 18.2) ahora es configurable en Herramientas > Opciones > Ejecución de consultas > SQL Server > Avanzadas > Mostrar la hora de finalización. |
| SSMS general | En el cuadro de diálogo de conexión, se ha reemplazado *Active Directory: contraseña* y *Active Directory: integrada* por *Azure Active Directory: contraseña* y *Azure Active Directory: integrada*, respectivamente. |
| SSMS general | Se ha corregido una incidencia que impedía que los usuarios pudieran usar SSMS para configurar la auditoría en instancias administradas de SQL Azure cuando se encontraban en una ZH con diferencia horaria con UTC negativa. |
| SSMS general | Se ha solucionado una incidencia en la interfaz de usuario de XEvent que hacía que mantener el ratón sobre la cuadrícula provocara la selección de filas. </br></br> Vea el elemento de los Comentarios del usuario de SQL Server para obtener información detallada: [La interfaz de usuario de eventos extendidos de SSMS selecciona acciones al mantener el puntero](https://feedback.azure.com/forums/908035/suggestions/38262124). |
| Importación de archivo plano | Se ha corregido el problema por el que Importar archivo plano no importaba todos los datos al permitir que el usuario eligiera una detección de tipos de datos simple o enriquecida.</br></br> Vea el elemento de los Comentarios del usuario de SQL Server para obtener información detallada: [La importación del archivo plano de SSMS no puede importar todos los datos](https://feedback.azure.com/forums/908035/suggestions/38096989). |
| Integration Services | Agregue un nuevo tipo de operación *StartNonCatalogExecution* para el informe de operación de SSIS.|
| Integration Services | Se ha corregido una incidencia en las canalizaciones de Azure Data Factory generadas por la utilidad `DTExec` habilitada para Azure para usar el tipo de parámetro correcto. (explícito para 18.3.1) |
| SMO/Generación de Script | Se ha corregido una incidencia que hacía que los SMO produjeran errores al capturar propiedades cuando se usaba **SMO.Server.SetDefaultInitFields(true)** .|
| Interfaz de usuario de Almacén de consultas | Se ha corregido una incidencia que provocaba que el eje Y no escalara cuando se seleccionaba la métrica *Recuento de ejecuciones* en la vista *Consulta seguida*. |
| Evaluación de vulnerabilidad | Se han deshabilitado las opciones de eliminación y aprobación de la línea de base de Azure SQL Database.|

#### <a name="known-issues-1831"></a>Problemas conocidos (18.3.1)

- El diagrama de base de datos creado a partir de un conjunto SSMS que se ejecuta en la máquina A no se puede modificar desde la máquina B (bloquearía SSMS). Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) para obtener información detallada.

- Existen incidencias de rediseño al cambiar entre varias ventanas de consulta. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042) para obtener información detallada. Una solución para esta incidencia es deshabilitar la aceleración de hardware en Herramientas > Opciones.

Puede hacer referencia a [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server) para obtener otras incidencias conocidas y proporcionar comentarios al equipo del producto.

### <a name="182"></a>18.2

![descargar](media/download-icon.png) [Descargar SSMS 18.2](https://go.microsoft.com/fwlink/?linkid=2099720)

- Número de versión: 18.2
- Número de compilación: 15.0.18142.0
- Fecha de lanzamiento: 25 de julio de 2019

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2099720&clcid=0x40a)

#### <a name="whats-new-in-182"></a>Novedades de 18.2

| Nuevo elemento | Detalles |
|----------|---------|
| Integration Services (SSIS) | Optimización del rendimiento del programador de paquetes SSIS en Azure. |
| Intellisense/Editor | Se ha agregado compatibilidad con la clasificación de datos. |
| OPTIMIZE_FOR_SEQUENTIAL_KEY | Se ha agregado compatibilidad con IntelliSense. |
| OPTIMIZE_FOR_SEQUENTIAL_KEY | Activa una optimización en el motor de base de datos que ayuda a mejorar el rendimiento de las inserciones de alta simultaneidad en el índice. Esta opción está diseñada para los índices que son propensos a la contención de inserción de la última página, que suele darse con índices que tienen una clave secuencial, como una columna de identidad, de secuencia o de fecha y hora. Para obtener más detalles, vea [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md#sequential-keys). |
| Ejecución o resultados de la consulta | Se ha agregado una *hora de finalización* en los mensajes en los que se realiza el seguimiento cuando una consulta determinada ha completado su ejecución. |
| Ejecución o resultados de la consulta | Permite que se muestren más datos (resultado a texto) y se almacenen en celdas (resultado a cuadrícula). Ahora SSMS admite hasta 2 M de caracteres para ambos (hasta 256 K y 64 K, respectivamente). Esto también ha solucionado la incidencia por la que los usuarios no podían obtener más de 43 680 caracteres de las celdas de la cuadrícula. |
| ShowPlan | Se ha agregado un nuevo atributo en QueryPlan cuando la [característica escalar en línea UDF](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining) está habilitada (ContainsInlineScalarTsqludfs). |
| SMO | Se agregó soporte técnico para la *API de SQL Assessment*. Para obtener más información, consulte la [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md). |
|  |  |

#### <a name="bug-fixes-in-182"></a>Correcciones de errores en 18.2

| Nuevo elemento | Detalles |
|------------|-------|
| Accesibilidad | Se ha actualizado la interfaz de usuario de XEvent (la cuadrícula) para que se pueda ordenar al presionar F3. |
| Always On | Se ha corregido un problema que hacía que SSMS produjera un error al intentar eliminar un grupo de disponibilidad (AG). |
| Always On | Se ha corregido un problema que hacía que SSMS presentase un asistente de conmutación por error incorrecto, cuando las réplicas se configuraban como sincrónicas, al usar las escalas de lectura AG (tipo de clúster = NONE). Ahora, SSMS presenta el asistente para la opción Force_Failover_Allow_Data_Loss, que es el único que se permite para la disponibilidad de tipo de clúster NONE. |
| Always On | Se ha corregido una incidencia por la que el asistente restringía el número de sincronizaciones permitidas a tres. |
| Clasificación de datos | Se ha corregido una incidencia que hacía que SSMS produjera un error *Índice (basado en cero) debe ser mayor o igual que cero* al intentar ver informes de clasificación de datos en bases de datos con un valor de CompatLevel inferior a 150. |
| SSMS general | Se ha corregido una incidencia por la que el usuario no podía desplazar horizontalmente el Panel de resultados a través de la rueda del mouse. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/34145641) para obtener información detallada. |
| SSMS general | Se ha actualizado *Monitor de actividad* para omitir los tipos de espera benignos SQLTRACE_WAIT_ENTRIES. |
| SSMS general | Se ha corregido una incidencia por la que algunas opciones de color *(Editor de texto > Pestaña editor y barra de estado)* no se conservaban. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37924165).
| SSMS general | En el cuadro de diálogo de conexión, se ha reemplazado *Azure Active Directory: universal compatible con MFA* por *Azure Active Directory: universal con MFA* (la funcionalidad es la misma, pero probablemente sea menos confusa). |
| SSMS general | Se ha actualizado SSMS para usar los valores predeterminados correctos al crear una base de datos de Azure SQL Database. |
| SSMS general | Se ha corregido un problema que provocaba que el usuario no pudiera *Iniciar PowerShell* desde un nodo de [Registrar servidores](register-servers/register-servers.md) cuando el servidor era un [contenedor de SQL Linux](../linux/quickstart-install-connect-docker.md). |
| Importar archivo plano | Se ha corregido un problema que provocaba que *Importar un archivo plano* no funcionara después de la actualización de SSMS 18.0 a 18.1. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37912636). |
| Importar archivo plano | Se ha corregido una incidencia por la que el *Asistente para la importación de archivos planos estaba notificando una columna duplicada o no válida* en un archivo .csv con encabezados con caracteres Unicode. |
| Explorador de objetos | Se ha corregido una incidencia que hacía que algunos elementos de menú (por ejemplo, el *Asistente para importación y exportación* de SQL Server) no estuvieran presentes o estuvieran deshabilitados cuando se conectaban a SQL Express. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37500016) para obtener información detallada. |
| Explorador de objetos | Se ha corregido una incidencia que provocaba que SSMS se bloqueara cuando se arrastraba un objeto desde el Explorador de objetos al editor. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37887988) para obtener información detallada. |
| Explorador de objetos | Se ha corregido una incidencia por la que el cambio de nombre de las bases de datos hacía que se mostraran nombres de la base de datos incorrectos en el Explorador de objetos. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37638472) para obtener información detallada. |
| Explorador de objetos | Se ha corregido una incidencia pendiente durante mucho tiempo por la que, al intentar expandir el nodo *Tablas* en el Explorador de objetos de una base de datos que estuviera configurada para usar una intercalación que Windows ya no admitiera, se desencadenara un error (y el usuario no pudiera expandir las tablas). Un ejemplo de este tipo de intercalación sería Korean_Wansung_Unicode_CI_AS. |
| [Registrar servidores](register-servers/register-servers.md) | Se ha corregido una incidencia que hacía que, cuando el Servidor registrado usaba *Active Directory: integrado* o *Azure Active Directory: universal con MFA*, al intentar emitir una consulta en varios servidores (en un *Grupo* en Servidores registrados) no funcionara debido a que SSMS producía un error al conectarse. |
| [Registrar servidores](register-servers/register-servers.md) | Se ha corregido una incidencia que hacía que, cuando el Servidor registrado usaba *Active Directory: contraseña* o *SQL Auth* y el usuario no recordaba la contraseña y marcaba esta opción, al intentar emitir una consulta en varios servidores (en un *Grupo* en Servidores registrados) provocara el bloqueo de SSMS. |
| Informes | Se ha corregido una incidencia, en los informes de *Utilización de disco*, por la que se producía un error en el informe cuando los archivos de datos tenían un gran número de extensiones. |
| Herramientas de replicación | Se ha corregido un problema que provocaba que el Monitor de replicación no funcionara con la base de datos del publicador en AG ni con el distribuidor en AG (esto se ha corregido antes en SSMS 17.x). |
| Agente SQL | Se ha corregido una incidencia que hacía que, al agregar, insertar, editar o quitar pasos de trabajo, el foco se restableciese en la primera fila en lugar de en la fila activa. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/38070892) para obtener información detallada. |
| SMO/Generación de Script | Se ha corregido un problema que provocaba que la instrucción *CREATE o ALTER* no contuviera objetos de scripting que tuvieran propiedades extendidas. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/37236748) para obtener información detallada. |
| SMO/Generación de Script | Se ha corregido una incidencia por la que SSMS no podía crear un script CREATE EXTERNAL LIBRARY correctamente. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37868089) para obtener información detallada. |
| SMO/Generación de Script | Se ha corregido una incidencia por la que, al intentar ejecutar *Generar scripts* en una base de datos con unas cuantas miles de tablas, provocara que el cuadro de diálogo de progreso pareciera bloquearse. |
| SMO/Generación de Script | Se ha corregido una incidencia por la que el scripting de *Tabla externa* en SQL 2019 no funcionaba. |
| SMO/Generación de Script | Se ha corregido una incidencia por la que el scripting de *Origen de datos externos* en SQL 2019 no funcionaba. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/34295080) para obtener información detallada. |
| SMO/Generación de Script | Se ha corregido una incidencia por la que las *propiedades extendidas* de las columnas no se incluían en el script cuando el destino era Azure SQL Database. Vea [stackoverflow](https://stackoverflow.com/questions/56952337/how-can-i-script-the-descriptions-of-columns-in-ms-sql-server-management-studio) para obtener más detalles. |
| SMO/Generación de Script | Inserción de la última página: SMO: agregar propiedad *Index.IsOptimizedForSequentialKey* |
|**Programa de instalación de SSMS**| **Se ha mitigado una incidencia por la que el programa de instalación de SSMS bloqueaba de forma incorrecta la instalación de los idiomas que no coincidían con los informes de SSMS. Esto podría haber sido una incidencia en algunas situaciones anómalas, como una instalación anulada o una desinstalación incorrecta de una versión anterior de SSMS. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37483594/) para obtener información detallada.** |
| Generador de perfiles XEvent | Se ha corregido un bloqueo que se producía cuando se cerraba el visor. |

#### <a name="known-issues-182"></a>Incidencias conocidas (18.2)

- El Diagrama de base de datos creado a partir de un conjunto SSMS que se ejecuta en la máquina A no se puede modificar desde la máquina B (bloquearía SSMS). Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) para obtener información detallada.

- Existen incidencias de rediseño al cambiar entre varias ventanas de consulta. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042). Una solución para esta incidencia es deshabilitar la aceleración de hardware en **Herramientas** > **Opciones**.

- Existe una limitación del tamaño de los datos que ve en los resultados de SSMS que se muestran en la cuadrícula, el texto o el archivo.

- Hay un problema con la recepción de un error al eliminar una instancia de Azure SQL Database en el Explorador de objetos, pero en realidad se realiza correctamente.

- El idioma predeterminado para los inicios de sesión de SQL puede mostrarse como Árabe en el cuadro de diálogo Propiedades de inicio de sesión, con independencia del idioma predeterminado real establecido para el inicio de sesión. Para ver el idioma predeterminado real para un inicio de sesión determinado, use T-SQL para seleccionar el valor **default_language_name** del inicio de sesión en **master.sys.server_principles**.

### <a name="181"></a>18.1

![descargar](media/download-icon.png) [Descargar SSMS 18.1](https://go.microsoft.com/fwlink/?linkid=2094583)

- Número de versión: 18.1
- Número de compilación: 15.0.18131.0
- Fecha de lanzamiento: 11 de junio de 2019

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2094583&clcid=0x40a)

#### <a name="whats-new-in-181"></a>Novedades de 18.1

| Nuevo elemento | Detalles |
| :-------| :------|
| Diagramas de base de datos | [Los diagramas de base de datos se han vuelto a agregar a SSMS](https://feedback.azure.com/forums/908035/suggestions/37507828).
| SSBDIAGNOSE.EXE |La herramienta Diagnose de SQL Server (herramienta de línea de comandos) se ha vuelto a agregar al paquete SSMS.|
| Integration Services (SSIS) | Compatibilidad para la programación del paquete SSIS, ubicado en el catálogo de SSIS en Azure o el sistema de archivos de Azure. Existen tres entradas para iniciar el cuadro de diálogo Nueva programación, el elemento de menú *Nueva programación…* que se muestra al hacer clic con el botón derecho en el paquete SSIS en el catálogo de SSIS en Azure, el elemento de menú *Paquete SSIS de programación en Azure* que se encuentra en el elemento de menú *Migrar a Azure* en el elemento de menú *Herramientas* y "Programar SSIS en Azure", que se muestra al hacer clic con el botón derecho en la carpeta Trabajos del Agente de SQL Server de Azure SQL Managed Instance.|

#### <a name="bug-fixes-in-181"></a>Correcciones de errores en 18.1

| Nuevo elemento | Detalles |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Accesibilidad | Mejora de la accesibilidad de la interfaz de usuario de trabajo del agente. |'
| Accesibilidad | Se ha mejorado la accesibilidad en la página de supervisión de Stretch al agregar un nombre accesible para el botón *Actualizar automáticamente*; además, se agregó un nombre accesible inteligente que va a ayudar a los usuarios a saber no solo en qué botón se encuentran, sino también el efecto que tendrá presionarlo. |
| Integración de ADS| Se ha corregido un posible bloqueo en SSMS al intentar usar los servidores registrados de ADS.|
| Diseñador de bases de datos | Se ha agregado compatibilidad con la intercalación Latin1_General_100_BIN2_UTF8 (disponible en SQL Server 2019 CTP 3.0) |
| Clasificación de datos | Se evita la adición de etiquetas de confidencialidad a las columnas de la tabla de historial, que no se admite. |
| Clasificación de datos | Se ha solucionado un problema relacionado con el tratamiento incorrecto del nivel de compatibilidad (servidor frente a base de datos). |
| Diseñador de bases de datos | Se ha agregado compatibilidad con la intercalación Latin1_General_100_BIN2_UTF8 (disponible en SQL2019 CTP 3.0). |
| SSMS general | Se ha corregido un problema consistente en que el scripting de pseudocolumnas de un índice era incorrecto. |
| SSMS general | Se ha corregido un problema en la página Propiedades de inicio de sesión consistente en que al hacer clic en el botón *Agregar credencial* se podía producir una excepción de referencia nula. |
| SSMS general | Se ha corregido la visualización del tamaño de la columna "money" en la página de propiedades del índice. |
| SSMS general | Se ha corregido un problema consistente en que SSMS no respetaba la configuración de Intellisense de *Herramientas/Opciones* en la ventana del editor SQL. |
| SSMS general | Se ha corregido una incidencia en la que SSMS no respetaba la configuración de la ayuda ("en línea" frente a "sin conexión"). |
| Configuración elevada de ppp | Se ha corregido el diseño de los controles de los cuadros de diálogo de error de las opciones de consulta no admitidas. |
| Configuración elevada de ppp | Se ha corregido el diseño de los controles de la página *Nuevo grupo de disponibilidad*, que se encuentra en alguna versión localizada de SSMS. |
| Configuración elevada de ppp | Se ha corregido el diseño de la página *Nueva programación de trabajo*. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37632094) para obtener información detallada. |
| Importación de archivo plano | Se ha corregido una incidencia en la que las filas pueden perderse silenciosamente durante la importación.|
| Intellisense/editor | Se ha reducido el tráfico de las consultas basadas en SMO a Azure SQL Database para IntelliSense. |
| Intellisense/editor | Se ha corregido un error gramatical en la información sobre herramientas mostrada al escribir T-SQL para crear un usuario. Además, se ha corregido el mensaje de error para eliminar la ambigüedad entre usuarios e inicios de sesión. |
| Visor de registros | Se ha corregido una incidencia en la que SSMS siempre abre el registro del servidor (o agente) actual, incluso si se hace doble clic en un inicio de sesión de archivo anterior en el Explorador de objetos. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37633648) para obtener información detallada. |
| Programa de instalación de SSMS | Se ha corregido el problema que hacía que el programa de instalación SSMS generara un error cuando la ruta de acceso del registro de instalación contenía espacios. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37496110) para obtener información detallada. |
| Programa de instalación de SSMS | Se ha corregido un problema consistente en que SSMS salía inmediatamente después de mostrar la pantalla de presentación. </br> Para obtener más detalles, vea estos sitios: [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37502512), [SSMS no se inicia](https://dba.stackexchange.com/questions/238609/ssms-refuses-to-start) y [Administradores de base de datos](https://dba.stackexchange.com/questions/237086/sql-server-management-studio-18-wont-open-only-splash-screen-pops-up). |
| Explorador de objetos | Se ha eliminado la restricción sobre la habilitación de *iniciar PowerShell* al conectarse a SQL en Linux. |
| Explorador de objetos | Se ha corregido un problema que hacía que SSMS se bloquease al intentar expandir el nodo Polybase/Grupo de escalado horizontal (al conectarse a un nodo de proceso). |
| Explorador de objetos | Se ha corregido un problema consistente en que el elemento de menú *Deshabilitado* todavía estaba habilitado incluso después de deshabilitar un índice determinado. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37735375) para obtener información detallada. |
| Informes | Se ha corregido un informe para mostrar realmente GrantedQueryMemory en KB (informe del panel de rendimiento de SQL). Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37167289) para obtener información detallada. |
| Informes | Se ha mejorado el seguimiento del bloque de registro en escenarios de Always-On. |
| ShowPlan | Se ha agregado un nuevo elemento de plan de presentación *SpillOccurred* al esquema de plan de presentación. |
| ShowPlan | Se agregaron lecturas remotas (*ActualPageServerReads*, *ActualPageServerReadAheads*, *ActualLobPageServerReads*, *ActualLobPageServerReadAheads*) al esquema del plan de presentación. |
| SMO/scripting | Se evita la consulta de restricciones perimetrales durante el scripting de tablas que no son de grafos. |
| SMO/scripting | Se ha eliminado la restricción sobre la clasificación de confidencialidad al generar scripts de columnas con *Clasificación de datos*. |
| SMO/scripting | Se ha corregido una incidencia en la que la opción "Generar script" de una tabla de grafos producía un error al generar datos. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898466) para obtener información detallada. |
| SMO/scripting | Se ha corregido una incidencia en la que el método EnumObjects() no capturaba el nombre de esquema de un sinónimo. |
| SMO/scripting | Se ha corregido un problema en UIConnectionInfo.LoadFromStream() consistente en que la sección *AdvancedOptions* no se leía (cuando no se especificaba una contraseña). |
| Agente SQL | Se ha corregido un problema que hacía que SSMS se bloqueara al trabajar con una ventana Propiedades del trabajo. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37164244) para obtener información detallada. |
| Agente SQL | Se ha corregido un problema consistente en que el botón "Ver" de *Propiedades de paso de trabajo* no siempre estaba habilitado, lo que evitaba la visualización de la salida de un paso de trabajo determinado. |
| Interfaz de usuario de XEvent | Se ha agregado una columna "Package" a la lista de XEvents para eliminar la ambigüedad de los eventos con nombres idénticos. |
| Interfaz de usuario de XEvent | Se ha agregado una asignación de tipo de clase "EXTERNAL LIBRARY" que faltaba a XEventUI. |

#### <a name="known-issues-181"></a>Problemas conocidos (18.1)

- Es posible que se muestre un error a los usuarios al arrastrar un objeto de tabla del Explorador de objetos al Editor de consultas. Somos conscientes del problema y la corrección está prevista para la próxima versión.

- Las opciones de color *Conexiones de grupo* y *Conexiones de servidor único* en Opciones -> Editor de texto -> Pestaña del editor y barra de estado -> Diseño y colores de la barra de estado, no se conservan después de cerrar SSMS 18.1. Al volver a abrir SSMS, la opción Diseño y colores de la barra de estado vuelve al valor predeterminado (blanco).

- Existe una limitación en el tamaño de los datos que ve en los resultados de SSMS que se muestran en la cuadrícula, el texto o el archivo.

- El diagrama de base de datos creado a partir de un conjunto SSMS que se ejecuta en la máquina A no se puede modificar desde la máquina B (bloquearía SSMS). Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37992649) para obtener información detallada.

### <a name="180"></a>18.0

![descargar](media/download-icon.png) [Descargar SSMS 18.0](https://go.microsoft.com/fwlink/?linkid=2088649)

- Número de versión: 18.0  
- Número de compilación: 15.0.18118.0  
- Fecha de lanzamiento: 24 de abril de 2019

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x804)| [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x404)| [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x409)| [Francés](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x40c)| [Alemán](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x407)| [Italiano](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x410)| [Japonés](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x411)| [Coreano](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x412)| [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x416)| [Ruso](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x419)| [Español](https://go.microsoft.com/fwlink/?linkid=2088649&clcid=0x40a)

#### <a name="whats-new-in-180"></a>Novedades de 18.0

| Nuevo elemento| Detalles|
| :-------| :------|
|Compatibilidad con SQL Server 2019|SSMS 18.0 es la primera versión de este producto que es totalmente *compatible* con SQL Server 2019 (compatLevel 150).|
|Compatibilidad con SQL Server 2019|Es compatible con "BATCH_STARTED_GROUP" y "BATCH_COMPLETED_GROUP" en SQL Server 2019 e Instancia administrada de SQL Database.|
|Compatibilidad con SQL Server 2019|SMO: se añade compatibilidad para la Inserción de UDF.|
|Compatibilidad con SQL Server 2019|GraphDB: se agrega una marca en el plan de presentación para Graph TC Sequence.|
|Compatibilidad con SQL Server 2019|Always Encrypted: se ha agregado compatibilidad para AEv2 / Enclave.|
|Compatibilidad con SQL Server 2019|Always Encrypted: el cuadro de diálogo de conexión presenta una nueva pestaña "Always Encrypted" cuando el usuario hace clic en el botón "Opciones" para habilitar o configurar la compatibilidad de enclave.|
|Tamaño de descarga SSMS más pequeño| El tamaño actual es de unos 500 MB, aproximadamente la mitad del conjunto de SSMS 17.x.
|SSMS Se basa en el shell aislado de Visual Studio 2017|El nuevo shell (SSMS se basa en Visual Studio 2017 15.9.11) desbloquea todas las correcciones de accesibilidad que se han incluido en SSMS y Visual Studio e incluye las correcciones de seguridad más recientes.|
|Mejoras de accesibilidad de SSMS| Se ha realizado un gran esfuerzo para resolver las incidencias de accesibilidad en todas las herramientas (SSMS, DTA y Profiler).|
|Ahora puede instalarse SSMS en una carpeta personalizada.| Esta opción está disponible desde la línea de comandos (útil para la instalación desatendida) y la configuración de la interfaz de usuario. Desde la línea de comandos, pase este argumento adicional a SSMS-Setup-ENU.exe:   SSMSInstallRoot=C:\MySSMS18 De forma predeterminada, la nueva ubicación de instalación para SSMS es la siguiente: %ProgramFiles(x86)%\Microsoft SQL Server Management Studio 18\Common7\IDE\ssms.exe.  Esto no significa que SSMS sea de instancias múltiples.|
|SSMS permite instalar en un idioma distinto del sistema operativo.|Se ha suprimido el bloque en la configuración de idiomas mixtos. Por ejemplo, puede instalar SSMS en alemán en un Windows en francés. Si el idioma del sistema operativo no coincide con el idioma de SSMS, el usuario debe cambiar el idioma en **Herramientas** > **Opciones** > **Configuración internacional**. En caso contrario, SSMS mostrará la interfaz de usuario en inglés.|
|SSMS ya no comparte componentes con el motor SQL.|Se ha realizado un gran esfuerzo para evitar compartir componentes con el motor SQL, lo que a menudo daba lugar a incidencias de servicio (uno sobrescribía los archivos instalados por el otro).|
|SSMS requiere NetFx 4.7.2 o posterior|Hemos actualizado nuestros requisitos mínimos de NetFx4.6.1 a NetFx4.7.2: esto nos permite aprovechar las ventajas de la nueva funcionalidad expuesta en el nuevo marco.|
|Capacidad de migrar la configuración de SSMS| Cuando SSMS 18 se inicia por primera vez, se le pide al usuario que migre la configuración de la versión 17.x. Los archivos de configuración de usuario ahora se almacenan como un archivo XML sin formato, de este modo mejora la portabilidad y, posiblemente, permite la edición.|
|Compatibilidad con la configuración elevada de PPP| La configuración elevada de PPP está habilitada de manera predeterminada.|
|SSMS se entrega con el controlador OLE DB de Microsoft.| Para obtener más detalles, consulte [Descarga del controlador Microsoft OLE DB para SQL Server](../connect/oledb/download-oledb-driver-for-sql-server.md).|
|SSMS no se admite en Windows 8. Windows 10 y Windows Server 2016 requieren la versión 1607 (10.0.14393) o posterior.|Debido a la nueva dependencia de NetFx 4.7.2, SSMS 18.0 no se instala en Windows 8 ni en las versiones antiguas de Windows 10 y Windows Server 2016. La configuración de SSMS bloquea esos sistemas. Windows 8.1 sigue siendo compatible.|
|SSMS ya no se agrega a la variable de entorno PATH.|La ruta de acceso a SSMS.EXE (y las herramientas en general) ya no se agrega más a la ruta de acceso. Los usuarios pueden agregarla de forma manual o, si se encuentran en un equipo moderno de Windows, usar el menú Inicio.|
|Los identificadores de paquete ya no tienen que desarrollar extensiones de SSMS.| En el pasado, SSMS cargaba selectivamente solo los paquetes conocidos, lo que requería que los desarrolladores registrasen sus propios paquetes. Este ya no es el caso.|
|SSMS general|Se expone la opción de configuración AUTOGROW_ALL_FILES para grupos de archivos en SSMS.|
|SSMS general|Se han eliminado las opciones con riesgo "agrupación ligera" y "aumento de prioridad" de la interfaz gráfica de usuario de SSMS. Para obtener más información, consulte [Detalles de aumento de prioridad y por qué no se recomienda](https://deep.data.blog/2010/01/26/priority-boost-details-and-why-its-not-recommended/).
|SSMS general|Nuevos enlaces de teclado y menú para crear archivos **CTRL+ALT+N**. Se mantiene **CTRL+N** para crear una consulta.|
|SSMS general|Ahora el cuadro de diálogo **Nueva regla de firewall** permite al usuario especificar un nombre de regla, en lugar de generar uno de forma automática en nombre del usuario.|
|SSMS general|Mejora de IntelliSense en el editor, especialmente para la versión v140 de T-SQL y versiones posteriores.|
|SSMS general|Se ha agregado compatibilidad de la interfaz de usuario de SSMS con UTF-8 en el diálogo de intercalación.|
|SSMS general|Se ha cambiado al "Administrador de credenciales de Windows" para las contraseñas utilizadas recientemente en el cuadro de diálogo de conexión. Esto soluciona un problema pendiente desde hace mucho tiempo consistente en que la persistencia de las contraseñas no siempre era fiable.|
|SSMS general|Compatibilidad mejorada con sistemas de varios monitores al asegurarnos de que aparezcan cada vez más diálogos y ventanas en el monitor esperado.|
|SSMS general|Se ha expuesto la configuración de servidor "valor predeterminado de la suma de comprobación de copia de seguridad" en la nueva página de configuración de la base de datos del diálogo de propiedades del servidor. Para obtener detalles, vea [https://feedback.azure.com/forums/08035-sql-server/suggestions/34634974](https://feedback.azure.com/forums/08035-sql-server/suggestions/34634974).|
|SSMS general|Se expone el "tamaño máximo de los archivos de registro de errores" en "Configurar registros de errores de SQL Server". Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/33624115](https://feedback.azure.com/forums/908035/suggestions/33624115).|
|SSMS general|Se ha agregado "Migrar a Azure" al menú Herramientas: se ha integrado Database Migration Assistant y Azure Database Migration Service con el fin de proporcionar acceso rápido y sencillo para ayudar a acelerar las migraciones a Azure.|
|SSMS general|Se ha agregado lógica para pedirle al usuario que confirme las transacciones abiertas cuando se usa "Cambiar la conexión".|
|Integración de Azure Data Studio|Se ha agregado un elemento de menú para iniciar o descargar Azure Data Studio.|
|Integración de Azure Data Studio|Se ha agregado el elemento de menú "Iniciar Azure Data Studio" al Explorador de objetos.|
|Integración de Azure Data Studio|Cuando se hace clic con el botón secundario en un nodo de base de datos en OE, al usuario se le presentan menús contextuales para ejecutar una consulta o crear un nuevo cuaderno en Azure Data Studio.|
|Compatibilidad con Azure SQL| Las propiedades de base de datos SLO/edición/MaxSize ahora aceptan nombres personalizados, lo que facilita la compatibilidad con ediciones futuras de Azure SQL Database.|
|Compatibilidad con Azure SQL| Se ha agregado compatibilidad con las SKU de los núcleos virtuales (De uso general y Crítico para la empresa): Gen4_24 y la serie Gen5 completa.|
|Azure SQL Managed Instance|Se ha agregado la opción "Inicios de sesión de Azure AD" como nuevo tipo de inicio de sesión en SMO y SSMS al conectarse a Azure SQL Managed Instance.|
|Always On|Se recombinan RTO (tiempo de recuperación estimado) y RPO (pérdida de datos estimada) en el panel Always On de SSMS. Vea la documentación actualizada en [https://docs.microsoft.com/sql/database-engine/availability-groups/windows/monitor-performance-for-always-on-availability-groups](../database-engine/availability-groups/windows/monitor-performance-for-always-on-availability-groups.md).|
|Always Encrypted| La casilla Habilitar Always Encrypted de la pestaña Always Encrypted del cuadro de diálogo Conectar con el servidor ofrece ahora una manera fácil de habilitar y deshabilitar Always Encrypted para una conexión de base de datos.|
|Always Encrypted con enclaves seguros| Se han realizado varias mejoras para admitir Always Encrypted con enclaves seguros en SQL Server 2019:  Un campo de texto para especificar la dirección URL de atestación de enclave en el cuadro de diálogo Conectar con el servidor (nueva pestaña Always Encrypted).  La nueva casilla del cuadro de diálogo Nueva clave maestra de columna para controlar si una nueva clave maestra de columna permite cálculos de enclave.  Otros cuadros de diálogo de administración de claves de Always Encrypted exponen ahora la información sobre qué claves maestras de columna permiten cálculos de enclave.|
|Archivos de auditoría|Se ha cambiado el método de autenticación basada en Clave de cuenta de almacenamiento a autenticación basada en AD Azure.|
|Clasificación de datos| Se ha reorganizado el menú de tareas de clasificación de datos: se ha agregado un submenú al menú de tareas de base de datos y una opción para abrir el informe desde el menú sin tener que abrir primero la ventana de clasificación de datos.|
|Clasificación de datos|Se ha agregado la nueva característica "Clasificación de datos" a SMO. El objeto de columna expone propiedades nuevas: SensitivityLabelName, SensitivityLabelId, SensitivityInformationTypeName, SensitivityInformationTypeId e IsClassified (de solo lectura). Para obtener más información, consulte [ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../t-sql/statements/add-sensitivity-classification-transact-sql.md)|
|Clasificación de datos|Se ha agregado el elemento de menú "Informe de clasificación" al control flotante "Clasificación de datos".|
|Clasificación de datos| Recomendaciones actualizadas.|
|Actualización del nivel de compatibilidad de la base de datos|Se ha agregado una nueva opción en ***Nombre de base de datos** _ > _*_Tareas_*_ > _*_Actualizar base de datos_*_. Esta opción iniciará el nuevo _ *Asistente para la optimización de consultas (QTA)* * que guía a los usuarios por los procesos siguientes: Recopilación de una línea de base de rendimiento antes de actualizar el nivel de compatibilidad de la base de datos. Actualización al nivel de compatibilidad de la base de datos deseado.  Recopilación de un segundo paso de datos de rendimiento a través de la misma carga de trabajo. Detección de regresiones de carga de trabajo y proporcionar recomendaciones comprobadas para mejorar el rendimiento de la carga de trabajo.  Esto es similar al proceso de actualización de base de datos documentado en [escenarios de uso del almacén de consultas](../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade), excepto el último paso donde QTA no se basa en un estado correcto conocido previamente para generar las recomendaciones.|
|Asistente para aplicaciones de capa de datos|Se ha agregado compatibilidad para importar o exportar la aplicación de capa de datos con tablas de grafos.|
|Asistente para la importación de archivos planos|Se ha agregado lógica para notificar al usuario que una importación puede haber producido un cambio de nombre de las columnas.|
|Integration Services (SSIS)|Se ha agregado compatibilidad para permitir que los clientes programen paquetes SSIS en los Azure-SSIS IR que están en la nube de Azure Government.|
|Integration Services (SSIS)|Cuando se usa el Agente SQL de Instancia administrada de Azure SQL mediante SSMS, se puede configurar el administrador de conexiones y parámetros en el paso de trabajo de agente SSIS.|
|Integration Services (SSIS)|Al conectarse a Azure SQL Database o Azure SQL Managed Instance, lo puede hacer con la opción *predeterminada* como base de datos inicial.|
|Integration Services (SSIS)|Se ha agregado un nuevo elemento de entrada **Probar SSIS en Azure Data Factory** en el nodo "Catálogos de Integration Services", que se puede usar para iniciar el "Asistente para la creación de un entorno de ejecución de integración" y crear de forma rápida "Azure-SSIS Integration Runtime".
|Integration Services (SSIS)|Se ha agregado un botón **Create SSIS IR** (Crear entorno de ejecución de integración de SSI) a "Catalog Creation Wizard" (Asistente para crear catálogos), que se puede usar para iniciar el "Asistente para la creación de un entorno de ejecución de integración" y crear de forma rápida "Azure-SSIS Integration Runtime".|
|Integration Services (SSIS)|ISDeploymentWizard ahora admite autenticación SQL, autenticación integrada de Azure Active Directory y autenticación de contraseña de Azure Active Directory en el modo de línea de comandos.|
|Integration Services (SSIS)|El Asistente para la implementación ahora admite la creación y la implementación en SSIS Integration Runtime en Azure Data Factory.|
|Scripting de objetos|Se han agregado elementos de menú nuevos para "CREATE OR ALTER" al generar scripts de objetos.|
|Almacén de consultas|Se ha mejorado la facilidad de uso de algunos informes (Consumos de recursos globales) mediante la adición del separador de miles a los números que aparecen en el eje Y de los gráficos.|
|Almacén de consultas|Se ha agregado un nuevo informe de estadísticas de espera de consulta.|
|Almacén de consultas|Se ha agregado la métrica "Recuento de ejecuciones" a la vista "Consulta con seguimiento".|
|Herramientas de replicación|Se ha agregado compatibilidad para la característica de especificación de puerto no predeterminado en el Monitor de replicación y SSMS.|
|ShowPlan|Se han agregado filas estimadas frente a reales en el tiempo real transcurrido en el nodo de operadores de plan de presentación, si están disponibles. Esta opción hace que el plan real tenga una apariencia coherente con el plan de estadísticas de consulta activa.|
|ShowPlan|Se ha modificado la información sobre herramientas y se ha agregado un comentario al hacer clic en el botón Editar consulta para un plan de presentación, que indica al usuario que puede que el motor SQL trunque el plan de presentación si la consulta es más de 4000 caracteres.|
|ShowPlan|Se ha agregado lógica para mostrar el "operador Materializer (instrucción Select externa)".|
|ShowPlan|Se ha agregado el nuevo atributo de plan de presentación BatchModeOnRowStoreUsed para identificar fácilmente las consultas en las que se usa la característica de "examen de modo por lotes en almacenes de filas". Cada vez que una consulta realiza el examen de modo por lotes en almacenes de filas, se agrega un nuevo atributo (BatchModeOnRowStoreUsed = "true") al elemento StmtSimple.|
|ShowPlan|Se ha agregado compatibilidad de plan de presentación al elemento RelOp LocalCube para DW ROLLUP y CUBE.|
|ShowPlan|Nuevo operador LocalCube para la nueva característica de agregación ROLLUP y CUBE en Azure Synapse Analytics.|
|SMO| Se amplía la compatibilidad SMO con la creación de índices reanudables.|
|SMO| Se agregó un nuevo evento en los objetos SMO ("PropertyMissing") para que los creadores de aplicaciones detecten antes los problemas de rendimiento SMO.|
|SMO| Se ha expuesto la nueva propiedad DefaultBackupChecksum en el objeto de configuración que se asigna a la configuración de servidor "Valor predeterminado de la suma de comprobación de copia de seguridad".|
|SMO| Se expone la nueva propiedad ProductUpdateLevel en el objeto de servidor, que se asigna al nivel de servicio para la versión de SQL en uso (por ejemplo, CU12, RTM).|
|SMO| Se expone la propiedad nueva LastGoodCheckDbTime en el objeto de base de datos, que se asigna a la propiedad de base de datos "lastgoodcheckdbtime". Si esta propiedad no está disponible, se devuelve un valor predeterminado de 1/1/1900 12:00:00.|
|SMO|Se ha movido la ubicación del archivo RegSrvr.xml (archivo de configuración de servidor registrado) a "%AppData%\Microsoft\SQL Server Management Studio" (sin versión, para que se pueda compartir entre las versiones de SSMS).|
|SMO|Se ha agregado "Testigo de la nube" como un tipo de cuórum nuevo y como un tipo de recurso nuevo.|
|SMO|Se ha agregado compatibilidad con las "restricciones perimetrales" en SMO y SSMS.|
|SMO|Se ha agregado compatibilidad con la eliminación en cascada a las "restricciones perimetrales" en SMO y SSMS.|
|SMO|Se ha agregado compatibilidad para los permisos de "lectura-escritura" de la clasificación de datos.|
|Evaluación de vulnerabilidad| Se ha habilitado el menú de tareas Evaluación de vulnerabilidad en Azure SQL Data Warehouse.|
|Evaluación de vulnerabilidad|Se ha cambiado el conjunto de reglas de evaluación de vulnerabilidades que se ejecuta en Azure SQL Managed Instance para que los resultados del examen de "evaluación de vulnerabilidades" puedan ser coherentes con los de Azure SQL DB.|
|Evaluación de vulnerabilidad| "Evaluación de vulnerabilidades de SQL" ahora es compatible con Azure SQL Data Warehouse.|
|Evaluación de vulnerabilidad|Se ha agregado una nueva característica de exportación para exportar los resultados del examen de evaluación de vulnerabilidades a Excel.|
|Visor de XEvent|En el Visor de XEvent se ha habilitado la ventana de plan de presentación para más elementos XEvent.|

#### <a name="bug-fixes-in-180"></a>Correcciones de errores en 18.0

| Nuevo elemento | Detalles|
|----------|--------|
|Bloqueos|Se ha corregido una fuente frecuente de bloqueos de SSMS relacionados con objetos GDI.|
|Bloqueos|Se ha corregido una fuente frecuente de errores y rendimiento deficiente cuando se selecciona "Script como Crear/Actualizar/Colocar" (se han eliminado capturas innecesarias de objetos SMO).|
|Bloqueos|Se ha corregido un problema que hacía que el sistema dejara de responder al conectarse a una instancia de Azure SQL Database con MFA mientras los seguimientos de ADAL estaban habilitados.|
|Bloqueos|Se ha corregido una incidencia que provocaba que el sistema dejara de responder (o se percibía un bloqueo) en Estadísticas de consultas dinámicas cuando se invocaba desde el Monitor de actividad (la incidencia se manifestaba cuando se utilizaba la autenticación de SQL Server sin haber establecido "información de seguridad persistente").|
|Bloqueos|Se ha corregido una incidencia que provocaba que el sistema dejara de responder al seleccionar "Informes" en el Explorador de objetos que se podía manifestar en conexiones de alta latencia o en la opción de no accesibilidad temporal de los recursos.|
|Bloqueos|Se ha corregido un bloqueo en SSMS cuando se intenta usar el Servidor de administración central y los servidores de Azure SQL. Para obtener más información, consulte [Error y bloqueo de la aplicación SSMS 17.5 cuando se usa el Servidor de administración central](https://feedback.azure.com/forums/908035/suggestions/33374884).|
|Bloqueos|Se ha corregido una incidencia que provocaba que el sistema dejara de responder en el Explorador de objetos mediante la optimización de la manera de recuperar la propiedad IsFullTextEnabled.|
|Bloqueos|Se ha corregido una incidencia que provocaba que el sistema dejara de responder en el "Asistente para copiar bases de datos" al evitar la compilación de consultas innecesarias para recuperar propiedades de la base de datos.|
|Bloqueos|Se ha corregido una incidencia que provocaba que SSMS dejara de responder o se bloqueara al editar T-SQL.|
|Bloqueos|Se ha mitigado una incidencia que hacía que SSMS dejara de responder al editar scripts de T-SQL grandes.|
|Bloqueos|Se ha corregido una incidencia que provocaba que SSMS se quedara sin memoria cuando se administraban los grandes conjuntos de datos que devolvían las consultas.|
|SSMS general|Se ha corregido un problema consistente en que "ApplicationIntent" no se pasaba en las conexiones en "Servidores registrados".|
|SSMS general|Se ha corregido una incidencia en la que el formulario "Nueva interfaz de usuario del asistente de sesión de XEvent" no se representaba correctamente en monitores con una configuración elevada de ppp.|
|SSMS general|Se ha corregido una incidencia que hacía que se intentara importar un archivo bacpac.|
|SSMS general|Se ha corregido una incidencia que provocaba que al intentar mostrar las propiedades de una base de datos (con FILEGROWTH > 2048 GB) se produjera un error de desbordamiento aritmético.|
|SSMS general|Se ha corregido un problema por el que el informe del panel de rendimiento notificaba esperas PAGELATCH y PAGEIOLATCH que no se podían encontrar en los subinformes.|
|SSMS general|Se han realizado otra serie de correcciones para hacer que SSMS sea más compatible con varios monitores al tener un diálogo abierto en el monitor correcto.|
|Analysis Services (AS)|Se ha corregido una incidencia por la que se recortaba la opción "Configuración avanzada" en la interfaz de usuario de XEvent de sistema autónomo (AS).|
|Analysis Services (AS)|Se ha corregido un problema en el que el análisis de DAX inicia una excepción "No se encontró el archivo".|
|Azure SQL Database|Se ha corregido un problema que hacía que la lista de bases de datos no se rellenara correctamente en la ventana de consulta de Azure SQL Database cuando se conectaba a una base de datos de usuario de Azure SQL Database en lugar de a una base de datos maestra.|
|Azure SQL Database|Se ha corregido un problema consistente en que no era posible agregar una "tabla temporal" a una base de datos de Azure SQL.|
|Azure SQL Database|Se ha habilitado la opción de submenú de propiedades de Estadísticas en el menú Estadísticas de Azure, porque desde hace bastante tiempo se admite de forma completa.|
|Azure SQL: compatibilidad general|Se han corregido problemas en el control común de la interfaz de usuario de Azure que impedían al usuario mostrar suscripciones de Azure (si había más de 50). Además, se ha cambiado la ordenación para que sea por nombre en lugar de por identificador de suscripción. Por ejemplo, el usuario podría encontrarse este error al intentar restaurar una copia de seguridad desde una dirección URL.|
|Azure SQL: compatibilidad general|Se ha corregido una incidencia en el control común de la interfaz de usuario de Azure al enumerar suscripciones que podían generar un error de tipo "El índice estaba fuera del intervalo. Debe ser un valor no negativo e inferior al tamaño de la colección." cuando el usuario no tenía suscripciones en algunos inquilinos. Por ejemplo, el usuario podría encontrarse este error al intentar restaurar una copia de seguridad desde una dirección URL.|
|Azure SQL: compatibilidad general|Se ha corregido una incidencia por la que los objetivos de nivel de servicio estaban codificados, lo que dificultaba la compatibilidad de SSMS con nuevos SLO de Azure SQL. Ahora el usuario puede iniciar sesión en Azure y permitir a SSMS recuperar todos los datos de SLO aplicables (edición y tamaño máximo).|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha mejorado y perfeccionado la compatibilidad con instancias administradas: se han deshabilitado opciones no admitidas en la interfaz de usuario y se proporciona una corrección para la opción Ver registros de auditoría para controlar la dirección URL de destino de auditoría.|
|Compatibilidad con la Instancia administrada de Azure SQL|El asistente para "generar y publicar scripts" crea scripts de cláusulas CREATE DATABASE no admitidas.|
|Compatibilidad con la Instancia administrada de Azure SQL|Habilite las estadísticas de consultas activas para instancias administradas.|
|Compatibilidad con la Instancia administrada de Azure SQL|Propiedades de la base de datos -> Archivos incluía un scripting ALTER DB ADD FILE incorrecto.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha corregido la regresión con el programador del servicio Agente SQL por la que se elegía la programación ONIDLE aunque se hubiera elegido algún otro tipo de programación.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se han ajustado MAXTRANSFERRATE y MAXBLOCKSIZE para realizar copias de seguridad en Azure Storage.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha solucionado el problema por el que el script de la copia de seguridad de registros final se creaba antes de la operación RESTORE (esto no se admite en CL).|
|Compatibilidad con la Instancia administrada de Azure SQL|El asistente para crear bases de datos no creaba correctamente el scripting de la instrucción CREATE DATABASE.|
|Compatibilidad con la Instancia administrada de Azure SQL|Tratamiento especial de paquetes SSIS en SSMS cuando se conecta a instancias administradas.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha corregido una incidencia en la que se mostraba un error al intentar usar el "Monitor de actividades" cuando estaba conectado a instancias administradas.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha mejorado la compatibilidad con los inicios de sesión de Azure AD (en el Explorador de SSMS).|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha mejorado el scripting de los objetos de grupos de archivos de SMO.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha mejorado la interfaz de usuario para las credenciales.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha agregado compatibilidad para la replicación lógica.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha corregido una incidencia que provocaba un error al hacer clic con el botón derecho en una base de datos y al seleccionar "Importar la aplicación de capa de datos".|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha corregido una incidencia que provocaba que se mostraran errores al hacer clic con el botón derecho en un valor "TempDB".|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha corregido una incidencia que provocaba que al intentar el scripting de la instrucción ALTER DB ADD FILE de SMO, se generara un script T-SQL vacío.|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha mejorado la visualización de las propiedades específicas del servidor de Instancia administrada (generación de hardware, nivel de servicio, almacenamiento usado y reservado).|
|Compatibilidad con la Instancia administrada de Azure SQL|Se ha corregido un problema que provocaba que el scripting de una base de datos ("Crear script como CREATE...") no generara scripts de grupos de archivos y archivos adicionales. Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/37326799](https://feedback.azure.com/forums/908035/suggestions/37326799). |
|Realizar copia de seguridad/restaurar/adjuntar/desasociar una base de datos|Se ha corregido un problema por el que el usuario no podía adjuntar una base de datos cuando el nombre de archivo físico del archivo .mdf no coincide con el nombre de archivo original.|
|Realizar copia de seguridad/restaurar/adjuntar/desasociar una base de datos|Se ha corregido una incidencia debido a la cual era posible que SSMS no encontrase un plan de restauración válido o encontrase uno que fuera poco óptimo. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897752](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897752). |
|Realizar copia de seguridad/restaurar/adjuntar/desasociar una base de datos|Se ha corregido un problema que provocaba que el asistente "Adjuntar base de datos" no mostrara los archivos secundarios cuyo nombre se hubiera cambiado. En este momento, se muestra el archivo y se agrega un comentario sobre él (por ejemplo "No se ha encontrado"). Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/32897434](https://feedback.azure.com/forums/908035/suggestions/32897434). |
|Asistente para copiar bases de datos|No se fuerza la activación de ansi_padding cuando los asistentes para copiar bases de datos, generar scripts y realizar transferencias intentan crear una tabla con una tabla en memoria.|
|Asistente para copiar bases de datos|Los asistentes para copiar bases de datos y para la tarea Transferir bases de datos se han interrumpido en SQL Server 2017 y SQL Server 2019.|""
|Asistente para copiar bases de datos|Los asistentes para copiar bases de datos, para generar scripts y para transferencias incluyen en un script la creación de la tabla antes de crear el origen de datos externo asociado.|
|Cuadro de diálogo de conexión|Se ha habilitado la eliminación de nombres de usuario de la lista anterior de nombres de usuario al presionar la tecla SUPR. Para obtener más información, consulte [Permitir la eliminación de usuarios de la ventana de inicio de sesión de SSMS](https://feedback.azure.com/forums/908035/suggestions/32897632).|
|Asistente para importación de DAC|Se ha corregido un problema que provocaba que el Asistente para importación de DAC no funcionara al conectarse por medio de Azure Active Directory (Azure AD).|
|Clasificación de datos|Se ha corregido un problema que se producía al guardar clasificaciones en el panel de clasificación de datos mientras hay otros paneles de clasificación de datos abiertos en otras bases de datos.|
|Asistente para aplicaciones de capa de datos|Se ha corregido una incidencia debido a la cual el usuario no podía importar una aplicación de capa de datos (.dacpac) debido al acceso limitado al servidor (por ejemplo, falta de acceso a todas las bases de datos del mismo servidor).|
|Asistente para aplicaciones de capa de datos|Se ha corregido una incidencia que hacía que la importación fuera muy lenta si muchas bases de datos estaban hospedadas en el mismo servidor SQL Azure.|
|Tablas externas|Se ha agregado compatibilidad con Rejected_Row_Location de plantilla, SMO, IntelliSense y cuadrícula de propiedades.|
|Asistente para la importación de archivos planos|Se ha corregido un problema por el que el "Asistente para la importación de archivos planos" no trataba correctamente las comillas dobles (escape). Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/32897998](https://feedback.azure.com/forums/908035/suggestions/32897998). |
|Asistente para la importación de archivos planos|Se ha corregido una incidencia relacionada con el tratamiento incorrecto de tipos de número de punto flotante (en configuraciones regionales que usan un delimitador distinto en números de punto flotante).|
|Asistente para la importación de archivos planos|Se ha corregido un problema relacionado con la importación de bits cuando los valores son 0 o 1. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898535](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898535). |
|Asistente para la importación de archivos planos|Se ha corregido un problema por el que el tipo de datos *float* se introducía como *nulls*.|
|Asistente para la importación de archivos planos|Se ha corregido un problema que evitaba que el Asistente para importación procesara valores decimales negativos.|
|Asistente para la importación de archivos planos|Se ha corregido un problema que evitaba que el asistente importara desde archivos CSV de una sola columna.|
|Asistente para la importación de archivos planos|Se ha corregido un problema por el que la importación de archivos planos no permitía cambiar la tabla de destino cuando la tabla ya existe. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32896186](https://feedback.azure.com/forums/908035-sql-server/suggestions/32896186). |
|Visor de Ayuda|Lógica mejorada en torno al respeto de los modos en línea o sin conexión (aún puede haber algunas incidencias que deban solucionarse).|
|Visor de Ayuda|Se ha corregido la opción "Ver Ayuda" para que se respete la configuración de en línea/sin conexión. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897791](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897791). |
|Alta disponibilidad y recuperación ante desastres<BR> Grupos de disponibilidad|Se ha corregido una incidencia que provocaba que los roles del "Asistente de Grupos de disponibilidad de conmutación por error" siempre se mostraran como "Resolviendo".|
|Alta disponibilidad y recuperación ante desastres<BR> Grupos de disponibilidad|Se ha corregido un problema que provocaba que SSMS mostrara advertencias truncadas en el "Panel de grupos de disponibilidad".|
|Integration Services (IS)|Se ha corregido un problema en paralelo por el que el Asistente para la implementación no se puede conectar a SQL Server si SQL Server 2019 y SSMS 18.0 están instalados en el mismo equipo.|
|Integration Services (IS)|Se ha corregido un problema que impedía editar una tarea de plan de mantenimiento al diseñar el plan de mantenimiento.|
|Integration Services (IS)|Se ha corregido una incidencia que provocaba el bloqueo del asistente para la implementación al cambiar el nombre del proyecto en la implementación.|
|Integration Services (IS)|Se ha habilitado la configuración del entorno en la característica de programación de Azure-SSIS IR.|
|Integration Services (IS)|Se ha corregido un problema que provocaba que el Asistente para la creación de un entorno de ejecución de integración de SSIS dejara de responder cuando la cuenta del cliente pertenecía a más de un inquilino.|
|Monitor de actividad de trabajo|Se ha corregido un bloqueo al usar el Monitor de actividad (con filtros).|
|Explorador de objetos|Se ha corregido una incidencia por la que SSMS generaba una excepción "No se puede convertir el objeto del tipo DBNull a otros tipos" al intentar expandir el nodo "Administración" del Explorador de objetos (configuración errónea de DataCollector).|
|Explorador de objetos|Se ha corregido un problema por el que el Explorador de objetos no incluía comillas de escape antes de invocar a "Editar las primeras...", lo que provocaba confusión en el diseñador.|
|Explorador de objetos|Se ha corregido un problema donde el asistente "Importar la aplicación de capa de datos" no se iniciaba desde el árbol de Azure Storage.|
|Explorador de objetos|Se ha corregido un problema en "Configuración de Correo electrónico de base de datos" que provocaba que no se conservara el estado de la casilla TLS/SSL. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32895541](https://feedback.azure.com/forums/908035-sql-server/suggestions/32895541). |
|Explorador de objetos|Se ha corregido una incidencia por la que la opción de SSMS para cerrar las conexiones existentes se atenuaba al intentar restaurar la base de datos con is_auto_update_stats_async_on.|
|Explorador de objetos|Se ha corregido una incidencia en la que al hacer clic con el botón derecho en los nodos del Explorador de objetos (por ejemplo "Tablas") y querer realizar una acción como filtrar tablas mediante Filtro > Configuración de filtro, el formulario de configuración de filtro puede aparecer en la otra pantalla y no en la que SSMS está activo actualmente. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/34284106](https://feedback.azure.com/forums/908035-sql-server/suggestions/34284106). |
|Explorador de objetos|Se ha corregido un problema pendiente desde hace mucho tiempo por el que la tecla SUPR no funcionaba en el Explorador de objetos al intentar cambiar el nombre de un objeto. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/33073510](https://feedback.azure.com/forums/908035-sql-server/suggestions/33073510), [https://feedback.azure.com/forums/908035/suggestions/32910247](https://feedback.azure.com/forums/908035/suggestions/32910247) y otros elementos duplicados.|
|Explorador de objetos|Cuando se muestran las propiedades de los archivos de base de datos existentes, el tamaño aparece en una columna "Tamaño (MB)" en lugar de "Tamaño inicial (MB)", que es lo que se muestra al crear una base de datos. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32629024](https://feedback.azure.com/forums/908035-sql-server/suggestions/32629024). |
|Explorador de objetos|Se ha deshabilitado el elemento de menú contextual "Diseño" en "Tablas de Graph" ya que no existe ninguna compatibilidad con ese tipo de tablas en la versión actual de SSMS.|
|Explorador de objetos|Se ha corregido un problema que hacía que el cuadro de diálogo "Nueva programación de trabajo" no se representara correctamente en monitores con una configuración elevada de ppp. Para obtener detalles, vea [https://feedback.azure.com/admin/v3/suggestions/35541262](https://feedback.azure.com/admin/v3/suggestions/35541262). |
|Explorador de objetos|Se ha corregido y mejorado la forma de mostrar una incidencia de tamaño de base de datos ("Tamaño (MB)") en los detalles del Explorador de objetos: solo con dos dígitos decimales y formato con el separador de miles. Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/34379308](https://feedback.azure.com/forums/908035/suggestions/34379308).|
|Explorador de objetos|Se ha corregido una incidencia que hacía que la creación de un "índice espacial" produjera un error del tipo "Para realizar esta acción, establezca la propiedad PartitionScheme".|
|Explorador de objetos|Mejoras de rendimiento menores en el Explorador de objetos para evitar la emisión de consultas adicionales, siempre que sea posible.|
|Explorador de objetos|Se ha extendido la lógica para solicitar confirmación al cambiar el nombre de una base de datos a todos los objetos de esquema (se puede configurar el valor).|
|Explorador de objetos|Se han agregado secuencias de escape adecuadas en el filtrado del Explorador de objetos. Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/36678803](https://feedback.azure.com/forums/908035/suggestions/36678803). |
|Explorador de objetos|Se ha corregido y mejorado la vista de detalles del Explorador de objetos para mostrar los números con separadores adecuados. Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/32900944](https://feedback.azure.com/forums/908035/suggestions/32900944). |
|Explorador de objetos|Se ha corregido el menú contextual en el nodo de "Tablas" cuando se conecta a SQL Express, que provocaba que faltara el control flotante "Nuevo", las tablas de grafos se mostraran de forma incorrecta y faltara la tabla con versiones del sistema. Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/37245529](https://feedback.azure.com/forums/908035/suggestions/37245529). |
|Scripting de objetos|Mejoras de rendimiento general: se tarda la mitad de tiempo en generar scripts de WideWorldImporters en comparación con SSMS 17.7.|
|Scripting de objetos|Al crear objetos de scripting, se omite la configuración del ámbito de DB que tiene valores predeterminados.|
|Scripting de objetos|No se genera T-SQL dinámico al crear un scripting. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898391](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898391). |
|Scripting de objetos|Se omite la sintaxis de gráfico "como borde" y "como nodo" en el scripting de una tabla en SQL Server 2016 y versiones anteriores.|
|Scripting de objetos|Se ha corregido un problema que hacía que se produjera un error de scripting de objetos de base de datos al conectarse a una instancia de Azure SQL Database mediante Azure AD con MFA.|
|Scripting de objetos|Se ha corregido un problema que iniciaba un error al intentar incluir en un script un índice espacial con GEOMETRY_AUTO_GRID o GEOGRAPHY_AUTO_GRID en una base de datos de SQL Azure.|
|Scripting de objetos|Se ha corregido un problema que provocaba que el scripting de base de datos (una base de datos de Azure SQL) tuviera siempre como destino una instancia de SQL Server local, incluso si la configuración de scripting del "Explorador de objetos" se ha establecido para coincidir con el origen.|
|Scripting de objetos|Se ha corregido una incidencia que provocaba que, al intentar incluir en un script una tabla de una base de datos de SQL Data Warehouse con índices agrupados y no agrupados, se generaran instrucciones T-SQL incorrectas.|
|Scripting de objetos|Se ha corregido una incidencia que provocaba que, al intentar incluir en un script una tabla de una base de datos de SQL Data Warehouse con "Índices de almacén de columnas agrupados" e "Índices agrupados", se generaran instrucciones T-SQL incorrectas (duplicadas).|
|Scripting de objetos|Se ha corregido el scripting de tablas con particiones sin valores de rango (bases de datos de almacenamiento de datos de SQL).|
|Scripting de objetos|Se ha corregido una incidencia que provocaba que el usuario no pudiera incluir en un script una auditoría o una especificación de auditoría SERVER_PERMISSION_CHANGE_GROUP.|
|Scripting de objetos|Se corrige una incidencia que hacía que el usuario no pudiera incluir en un script estadísticas de SQL Data Warehouse. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32897296](https://feedback.azure.com/forums/908035-sql-server/suggestions/32897296). |
|Scripting de objetos|Se ha corregido una incidencia que provocaba que el "Asistente para generar scripts" mostrara la tabla incorrecta con un error de scripting cuando "Continuar scripting en caso de Error" se establece en false.|
|Scripting de objetos|Generación de scripts mejorada en SQL Server 2019.|
|Generador de perfiles|Se ha agregado el evento "Consulta de reescritura de tabla de agregado" a los eventos del generador de perfiles.|
|Almacén de datos de consultas|Se ha corregido un problema que producía que se pudiera iniciar una excepción "DocumentFrame (SQLEditors)".|
|Almacén de datos de consultas|Se ha corregido un problema por el que al intentar establecer un intervalo de tiempo personalizado en los informes de Almacén de consultas integrados, el usuario no podía seleccionar AM o PM en el intervalo de inicio o fin.|
|Cuadrícula de resultados|Se ha corregido un problema que se provocaba en el modo de contraste alto (números de línea seleccionados no visibles).|
|Cuadrícula de resultados|Se ha corregido una incidencia que provocaba una excepción de tipo "Índice fuera de rango" al hacer clic en la cuadrícula.|
|Cuadrícula de resultados|Se ha corregido un problema por el cual se omitía el color de fondo del resultado de la cuadrícula. Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/32895916](https://feedback.azure.com/forums/908035/suggestions/32895916). |
|ShowPlan|Las nuevas propiedades del operador de concesión de memoria no se muestran correctamente cuando hay más de un subproceso.|
|ShowPlan|Se han agregado los siguientes cuatro atributos en RunTimeCountersPerThread del plan XML de ejecución real: HpcRowCount (número de filas procesadas por un dispositivo *hpc*), HpcKernelElapsedUs (espera de tiempo transcurrida para la ejecución del kernel en uso), HpcHostToDeviceBytes (bytes transferidos desde el host al dispositivo) y HpcDeviceToHostBytes (bytes transferidos desde el dispositivo al host).|
|ShowPlan|Se ha corregido una incidencia que provocaba que los nodos de plan parecidos se resaltaran en una posición incorrecta.|
|SMO|Se ha corregido un problema por el que SMO/ServerConnection no controlaba correctamente las conexiones basadas en SqlCredential. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/33698941](https://feedback.azure.com/forums/908035-sql-server/suggestions/33698941). |
|SMO|Se ha corregido un problema por el que una aplicación escrita con SMO podía encontrar un error si intentaba enumerar las bases de datos desde el mismo servidor en varios subprocesos, aunque usara instancias de SqlConnection independientes en cada uno.|
|SMO|Se ha corregido la regresión de rendimiento en la transferencia desde tablas externas.|
|SMO|Se ha corregido una incidencia en la seguridad para subprocesos de ServerConnection que hacía que SMO perdiera instancias de SqlConnection cuando tiene como destino Azure.|
|SMO|Se ha corregido una incidencia que provocaba un error StringBuilder.FormatError al intentar restaurar una base de datos que tenía llaves en su nombre.|
|SMO|Se ha corregido una incidencia por la cual las bases de datos de Azure en SMO aplicaban, de forma predeterminada, la intercalación sin distinguir entre mayúsculas y minúsculas para todas las comparaciones de cadenas en lugar de usar la intercalación especificada para la base de datos.|
|Editor de SSMS|Se ha corregido una incidencia en la que la "Tabla del sistema de SQL" al restaurar los colores predeterminados cambiaba el color a verde lima, y no al color verde predeterminado, lo que hacía difícil leerla sobre un fondo blanco. Para obtener más información, consulte [Restaurar el color predeterminado incorrecto para la Tabla del sistema de SQL](https://feedback.azure.com/forums/908035-sql-server/suggestions/32896906). La incidencia todavía persiste en versiones no inglesas de SSMS.|
|Editor de SSMS|Se ha corregido un problema por el que IntelliSense no funcionaba cuando se conectaba a Azure SQLDW con autenticación de Azure Active Directory (Azure AD).|
|Editor de SSMS|Se ha corregido IntelliSense en Azure cuando el usuario carece de acceso a la base de datos **maestra**.|
|Editor de SSMS|Se han corregido fragmentos de código para crear "tablas temporales", que se interrumpieron cuando la intercalación de la base de datos de destino distinguía mayúsculas y minúsculas.|
|Editor de SSMS|Ahora IntelliSense reconoce la nueva función TRANSLATE. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898430](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898430). |
|Editor de SSMS|Se ha mejorado IntelliSense en la función integrada FORMAT. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898676](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898676). |
|Editor de SSMS|LAG y LEAD ahora se reconocen como funciones integradas. Para obtener detalles, vea [https://feedback.azure.com/forums/908035-sql-server/suggestions/32898757](https://feedback.azure.com/forums/908035-sql-server/suggestions/32898757). |
|Editor de SSMS|Se ha corregido una incidencia que hacía que IntelliSense generara una advertencia al usar "ALTER TABLE...ADD CONSTRAINT...WITH(ONLINE=ON)".|
|Editor de SSMS|Se corrigió una incidencia debido a la cual varias vistas del sistema y funciones de valores de tabla no se coloreaban correctamente.|
|Editor de SSMS|Se ha corregido una incidencia que podía causar que al hacer clic en las pestañas del editor la ficha se cerrara en lugar de obtener el foco. Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/37291114](https://feedback.azure.com/forums/908035/suggestions/37291114). |
|Opciones de SSMS|Se ha corregido un problema por el que la página **Herramientas** > **Opciones** > **Explorador de objetos de SQL Server** > **Comandos** no cambiaba de tamaño correctamente.|
|Opciones de SSMS|Ahora, SSMS deshabilita de forma predeterminada la descarga automática de DTD en el editor de XMLA; el editor de scripts XMLA (que usa el servicio de lenguaje XML) evita ahora de forma predeterminada que se descargue automáticamente la DTD para archivos xmla potencialmente malintencionados. Esto se controla mediante la desactivación del valor "Descargar automáticamente DTD y esquemas" en **Herramientas** > **Opciones** > **Entorno** > **Editor de texto** > **XML** > **Varios**.|
|Opciones de SSMS|Se ha restaurado **CTRL+D** para que sea el acceso directo como lo era en versiones anteriores de SSMS. Para obtener detalles, vea [https://feedback.azure.com/forums/908035/suggestions/35544754](https://feedback.azure.com/forums/908035/suggestions/35544754). |
|Diseñador de tablas|Se ha corregido un bloqueo al "Editar 200 filas".|
|Diseñador de tablas|Se ha corregido un problema por el que se permitía al diseñador agregar una tabla cuando se estaba conectado a una instancia de Azure SQL Database.|
|Evaluación de vulnerabilidad|Se ha corregido una incidencia que provocaba que los resultados del análisis no se cargaran correctamente.|
|XEvent|Se han agregado dos columnas, "action_name" y "class_type_desc", que muestran los campos de identificador de acción y tipo de clase como cadenas legibles.|
|XEvent|Se ha quitado el límite de 1 000 000 eventos del Visor de XEvent.|
|Generador de perfiles XEvent|Se ha corregido un problema por el que el generador de perfiles XEvent no se podía iniciar cuando se estaba conectado a una instancia de SQL Server de 96 núcleos.|
|Visor de XEvent|Se ha corregido una incidencia que provocaba que el Visor de XEvent se bloqueara al intentar agrupar los eventos mediante las "Opciones de la barra de herramientas de eventos extendidos".|

#### <a name="deprecated-and-removed-features-in-180"></a>Características en desuso y eliminadas en 18.0

Estas son las características que están en desuso y se han quitado de la versión 18.0 de SSMS.

- Depurador de T-SQL
- Diagramas de base de datos
- Las siguientes herramientas ya no se instalan con SSMS:
  - OSQL.EXE
  - DReplay.exe
  - SQLdiag.exe
  - SSBDiagnose.exe
  - bcp.exe
  - sqlcmd.exe
- Herramientas de Configuration Manager:
  - SQL Server Configuration Manager y Reporting Server Configuration Manager ya no forman parte del programa de instalación de SSMS.
- Directivas estándar de DMF
  - Las directivas ya no se instalan con SSMS. Se han cambiado a Git. Si quieren, los usuarios pueden colaborar y descargarlas o instalarlas.
- Se ha quitado la opción -P de la línea de comandos de SSMS
  - Por motivos de seguridad, se ha quitado la opción que permitía especificar contraseñas de texto no cifrado en la línea de comandos.
- Se ha eliminado Generar Scripts > Publicar en servicio web.
  - Esta característica en desuso se ha eliminado de la interfaz de usuario de SSMS.
- Se ha eliminado el nodo "Mantenimiento > Heredado" del Explorador de objetos.
  - Ya no se podrá acceder a los nodos antiguos "Plan de mantenimiento de bases de datos" y "SQL Mail". Los nodos modernos "Correo electrónico de base de datos" y "Planes de mantenimiento" siguen funcionando con normalidad.

#### <a name="known-issues-180"></a>Problemas conocidos (18.0)

- Podría detectar un problema al instalar la versión 18.0 consistente en la imposibilidad de ejecutar SQL Server Management Studio. Si detecta este problema, siga los pasos del artículo [SSMS2018 está instalado, pero no se ejecuta](https://feedback.azure.com/forums/908035-sql-server/suggestions/37502512-ssms2018-installed-but-will-not-run).

- Existe una limitación en el tamaño de los datos que ve en los resultados de SSMS que se muestran en la cuadrícula, el texto o el archivo.

- Existen incidencias de rediseño al cambiar entre varias ventanas de consulta. Vea [Comentarios del usuario de SQL Server](https://feedback.azure.com/forums/908035/suggestions/37474042) para obtener información detallada. Una solución para esta incidencia es deshabilitar la aceleración de hardware en Herramientas > Opciones.

### <a name="1791"></a>17.9.1

![descargar](media/download-icon.png) [Descargar SSMS 17.9.1](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)

- Número de versión: 17.9.1
- Número de compilación: 14.0.17289.0
- Fecha de lanzamiento: 21 de noviembre de 2018

#### <a name="bug-fixes-in-1791"></a>Correcciones de errores en 17.9.1

- Se ha corregido una incidencia que hace que la conexión de los usuarios se cierre y se vuelva a abrir con cada invocación de consulta cuando se usa la autenticación "Azure Active Directory: universal compatible con MFA" con el editor de consultas SQL. Entre los efectos secundarios del cierre de la conexión se incluyen tablas temporales globales que se quitan de forma inesperada y, a veces, la asignación de un SPID nuevo a la conexión.
- Se ha corregido un problema pendiente desde hace mucho tiempo por el que se producía un error al buscar un plan de restauración, o bien se generaba un plan de restauración ineficaz en determinadas condiciones.
- Se ha corregido un problema en el asistente "para importar aplicaciones de capa de datos" que podía producir un error cuando se conectaba a una base de datos de Azure SQL.

> [!NOTE]
> Las versiones localizadas de SSMS 17.x en idiomas diferentes al inglés requieren el [paquete de actualización de seguridad KB 2862966](https://support.microsoft.com/kb/2862966) cuando se instalan en: Windows 8, Windows 7, Windows Server 2012 y Windows Server 2008 R2.

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x804)| [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x404)| [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x409)| [Francés](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40c)| [Alemán](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x407)| [Italiano](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x410)| [Japonés](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x411)| [Coreano](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x412)| [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x416)| [Ruso](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x419)| [Español](https://go.microsoft.com/fwlink/?linkid=2043154&clcid=0x40a)

#### <a name="uninstall-and-reinstall-ssms-17x"></a>Desinstalación y reinstalación de SSMS 17.x

Si tiene algún problema con la instalación de SSMS que no se resuelve al instalarlo y volverlo a instalar, primero puede probar a [reparar](https://support.microsoft.com/help/4028054/windows-10-repair-or-remove-programs) Visual Studio 2015 IsoShell. Si el problema tampoco se resuelve al reparar Visual Studio 2015 IsoShell, pruebe los pasos que se indican a continuación, con los que se han podido resolver muchos problemas aleatorios:

1. Desinstale SSMS de la misma forma en que desinstalaría cualquier aplicación (con las opciones *Aplicaciones y características* o *Programas y características* en función de su versión de Windows).

2. Desinstale Visual Studio 2015 IsoShell **desde un símbolo del sistema con privilegios elevados**:

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```

    ```vs_isoshell.exe /Uninstall /Force /PromptRestart```

3. Desinstale Microsoft Visual C++ 2015 Redistributable de la misma forma en que desinstalaría cualquier aplicación. Si están en el equipo, desinstale x86 y x64.

4. Vuelva a instalar Visual Studio 2015 IsoShell **desde un símbolo del sistema con privilegios elevados**:  

    ```PUSHD "C:\ProgramData\Package Cache\FE948F0DAB52EB8CB5A740A77D8934B9E1A8E301\redist"```  

    ```vs_isoshell.exe /PromptRestart```

5. Vuelva a instalar SSMS.

6. Actualice a la [versión más reciente de Visual C++ 2015 Redistributable](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) si todavía no lo ha hecho.

### <a name="1653"></a>16.5.3

![descargar](media/download-icon.png) [Descargar SSMS 16.5.3](https://go.microsoft.com/fwlink/?LinkID=840946)

- Número de versión: 16.5.3  
- Número de compilación: 13.0.16106.4  
- Fecha de lanzamiento: 30 de enero de 2017

[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x804)| [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x404)| [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x409)| [Francés](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40c)| [Alemán](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x407)| [Italiano](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x410)| [Japonés](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x411)| [Coreano](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x412)| [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x416)| [Ruso](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x419)| [Español](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40a)

#### <a name="bug-fixes-in-1653"></a>Correcciones de errores en 16.5.3

- Se ha corregido una incidencia detectada en SSMS 16.5.2 que provocaba la expansión del nodo "Table" cuando la tabla tenía más de una columna dispersa.

- Los usuarios pueden implementar paquetes SSIS que contienen el Administrador de conexiones OData que se conectan a un recurso de Microsoft Dynamics AX/CRM Online en el catálogo de SSIS. Para más información, vea [Administrador de conexiones OData](../integration-services/connection-manager/odata-connection-manager.md).

- La configuración de Always Encrypted en una tabla existente produce errores en los objetos relacionados. [Id. de Connect 3103181](https://connect.microsoft.com/SQLServer/feedback/details/3103181/setting-up-always-encrypted-on-an-existing-table-fails-with-errors-on-unrelated-objects)

- La Configuración de Always Encrypted para una base de datos existente con varios esquemas no funciona. [Id. de Connect 3109591](https://connect.microsoft.com/SQLServer/feedback/details/3109591/sql-server-2016-always-encrypted-against-existing-database-with-multiple-schemas-doesnt-work)

- El Asistente para columnas cifradas de Always Encrypted produce un error debido a la base de datos que contiene vistas que hacen referencia a vistas del sistema. [Id. de Connect 3111925](https://connect.microsoft.com/SQLServer/feedback/details/3111925/sql-server-2016-always-encrypted-encrypted-column-wizard-failed-task-failed-due-to-following-error-cannot-save-package-to-file-the-model-has-build-blocking-errors)

- Cuando se cifra mediante Always Encrypted, los errores al actualizar los módulos después del cifrado se tratan incorrectamente.

- El menú *Abrir recientes* no muestra los archivos guardados recientemente. [Id. de Connect 3113288](https://connect.microsoft.com/SQLServer/feedback/details/3113288/ssms-2016-open-recent-menu-doesnt-show-recently-saved-files)

- SSMS funciona con lentitud cuando se hace clic con el botón derecho en el índice de una tabla (a través de una conexión remota de Internet). [Id. de Connect 3114074](https://connect.microsoft.com/SQLServer/feedback/details/3114074/ssms-slow-when-right-clicking-an-index-for-a-table-over-a-remote-internet-connection)

- Se ha corregido un problema de la barra de desplazamiento del Diseñador de SQL. [Id. de Connect 3114856](https://connect.microsoft.com/SQLServer/feedback/details/3114856/bug-in-scrollbar-on-sql-desginer-in-ssms-2016)

- El menú contextual de las tablas deja de responder momentáneamente.

- En algunas ocasiones, SSMS produce excepciones en el Monitor de actividad y se bloquea. [Id. de Connect 697527](https://connect.microsoft.com/SQLServer/feedback/details/697527/)

- SSMS 2016 se bloquea con error "El proceso terminó debido a un error interno en el runtime de .NET en la dirección IP 71AF8579 (71AE0000) con el código de salida 80131506".

## <a name="additional-downloads"></a>Descargas adicionales

Para obtener una lista de todas las descargas de SQL Server Management Studio, consulte el [Centro de descargas de Microsoft](https://www.microsoft.com/download/search.aspx?q=sql%20server%20management%20studio&p=0&r=10&t=&s=Relevancy~Descending).  
  
Para obtener la versión más reciente de SQL Server Management Studio, vea [Descargar SQL Server Management Studio &#40;SSMS&#41;](../ssms/download-sql-server-management-studio-ssms.md).