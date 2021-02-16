---
description: Ediciones y características admitidas de SQL Server 2019 en Linux
title: Ediciones y características admitidas de SQL Server 2019 en Linux
ms.date: 01/08/2020
ms.prod: sql
ms.technology: linux
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- default components
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.openlocfilehash: 933ce305104358f56246b2c31fd8fe52a07b03ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348149"
---
# <a name="editions-and-supported-features-of-sql-server-2019-on-linux"></a>Ediciones y características admitidas de SQL Server 2019 en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se proporcionan detalles de las características admitidas en las diversas ediciones de SQL Server 2019 en Linux. Para conocer las ediciones y las características admitidas de SQL Server en Windows, vea [SQL Server 2019 en Windows](../sql-server/editions-and-components-of-sql-server-version-15.md).  
  
Los requisitos de instalación varían según las necesidades de las aplicaciones. Las distintas ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] han sido diseñadas para satisfacer los requisitos de rendimiento, tiempo de ejecución y precio propios de cada organización y cada persona. Los componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que instale también dependen de sus necesidades concretas. Las secciones siguientes le servirán de ayuda para elegir la mejor opción entre las ediciones y los componentes disponibles en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

Para leer las notas de la versión más reciente e información sobre las novedades, vea lo siguiente:
- [Notas de la versión de SQL Server 2019 en Linux](sql-server-linux-release-notes-2019.md)
- [Novedades de SQL Server 2019 en Linux](sql-server-linux-whats-new-2019.md)

Para obtener una lista de las características de SQL Server no disponibles en Linux, vea [Características y servicios no admitidos](#Unsupported).

### <a name="try-sql-server"></a>Pruebe SQL Server.    
    
[Descarga de SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)

## <a name="ssnoversion-editions"></a>Ediciones de[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]  
 En la siguiente tabla se describen las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 
  
|Edición de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Definición|  
|---------------------------------------|----------------|  
|Enterprise|La oferta premium, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition ofrece completas capacidades de centro de datos de tecnología avanzada con un rendimiento ultrarrápido que permiten altos niveles de servicio para cargas de trabajo críticas.|  
|Estándar|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard Edition proporciona administración básica de datos para que los departamentos y las pequeñas organizaciones ejecuten sus aplicaciones y admite herramientas de desarrollo comunes, tanto locales como en la nube, que habilitan la administración eficaz de bases de datos con recursos de TI mínimos.|  
|Web|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition es una opción con un costo total de propiedad bajo para los hosts de Web y los VAP de Web que proporciona capacidades asequibles de administración y escalabilidad para propiedades web, tanto de pequeña como de gran escala.|  
|Desarrollador|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer Edition permite a los desarrolladores compilar cualquier tipo de aplicación en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Incluye toda la funcionalidad de la edición Enterprise, pero tiene licencias para usarse como sistema de prueba y desarrollo, no como un servidor de producción. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer es una opción ideal para las personas que compilan y prueban aplicaciones.|  
|Express edition|Express Edition es una base de datos gratuita para principiantes y es ideal para aprender a compilar pequeñas aplicaciones de servidor y de escritorio orientadas a datos. Es la mejor opción para los fabricantes de software independientes, los desarrolladores y los aficionados que compilan aplicaciones cliente. Si necesita características de base de datos más avanzadas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express se puede actualizar sin problemas a otras versiones superiores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Uso de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con aplicaciones cliente/servidor  

Puede instalar solo los componentes de cliente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un equipo en el que se ejecuten aplicaciones cliente/servidor conectadas directamente a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Una instalación de componentes de cliente también es una buena opción si administra una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un servidor de bases de datos, o si tiene pensado desarrollar aplicaciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
## <a name="ssnoversion-components"></a>Componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  

SQL Server 2019 en Linux es compatible con el motor de base de datos de SQL Server. En la tabla siguiente se indican las características del motor de base de datos.   
  
|Componentes de servidor|Descripción|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|El [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] incluye el [!INCLUDE[ssDE](../includes/ssde-md.md)], el servicio principal para almacenar, procesar y proteger datos; también incluye replicación, búsqueda de texto completo, herramientas para administrar datos XML y relacionales e integración analítica en base de datos.|  

**Ediciones Developer, Enterprise Core y Evaluation**  
Para conocer las características admitidas en las ediciones Developer, Enterprise Core y Evaluation, vea las características indicadas para SQL Server Enterprise Edition en las tablas siguientes.

La edición Developer sigue siendo compatible con solo un cliente de [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Límites de escala  
  
|Característica|Enterprise|Estándar|Web|Express| 
|-------------|----------------|--------------|---------|------------------------|
|Capacidad máxima de cálculo que usa una sola instancia: [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Sistema operativo máximo|Limitada a menos de 4 sockets o 24 núcleos|Limitada a menos de 4 sockets o 16 núcleos|Limitada a menos de 1 socket o 4 núcleos| 
|Capacidad máxima de cálculo que usa una sola instancia: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] o [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Sistema operativo máximo|Limitada a menos de 4 sockets o 24 núcleos|Limitada a menos de 4 sockets o 16 núcleos|Limitada a menos de 1 socket o 4 núcleos|
|Memoria máxima para el grupo de búferes por cada instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Sistema operativo máximo|128 GB|64 GB|1410 MB|
|Cantidad máxima de memoria para la caché de segmento del almacén de columnas por cada instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria ilimitada| 32 GB| 16 GB| 352 MB|  
|Tamaño máximo de datos optimizados para memoria por base de datos en [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria ilimitada| 32 GB| 16 GB| 352 MB|
|Tamaño máximo de la base de datos relacional|524 PB|524 PB|524 PB|10 GB|  
  
<sup>1</sup> La licencia basada en Enterprise Edition con licencia de servidor y acceso de cliente (CAL) (no disponible para nuevos contratos) está limitada a un máximo de 20 núcleos por instancia de SQL Server. No hay ningún límite en el modelo de licencias de servidor basado en núcleos. Para obtener más información, vea [Límites de la capacidad de cálculo de cada edición de SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
 
##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> Alta disponibilidad de RDBMS  
  
|Característica|Enterprise|Estándar|Web|Express|  
|-------------|----------------|--------------|---------|------------------------|  
|Trasvase de registros|Sí|Sí|Sí|No|  
|Compresión de copia de seguridad|Sí|Sí|No|No| 
|Instantáneas de base de datos|Sí|Sí|No|No|
|Instancia de clúster de conmutación por error AlwaysOn<sup>1</sup>|Sí|Sí|No|No| 
|Grupos de disponibilidad AlwaysOn<sup>2</sup>|Sí|No|No|No|
|Grupos de disponibilidad básica<sup>3</sup>|No|Sí|No|No|
|Grupo de disponibilidad de confirmación de réplica mínima|Sí|Sí|No|No|
|Grupo de disponibilidad sin clúster|Sí|Sí|No|No|
|Restauración de archivos y páginas en línea|Sí|No|No|No|
|Índices en línea|Sí|No|No|No|
|Recompilaciones de índices en línea reanudables|Sí|No|No|No|
|Cambio de esquema en línea|Sí|No|No|No|
|Recuperación rápida|Sí|No|No|No|
|Copias de seguridad reflejadas|Sí|No|No|No|
|Agregar memoria y CPU sin interrupción|Sí|No|No|No|
|Copia de seguridad cifrada|Sí|Sí|No|No|
|Copia de seguridad híbrida en Azure (copia de seguridad en dirección URL)|Sí|Sí|No|No|
  
<sup>1</sup> En Enterprise Edition, el número de nodos es el máximo del sistema operativo. En Standard Edition hay compatibilidad con dos nodos. 

<sup>2</sup> En Enterprise Edition, proporciona compatibilidad con hasta 8 réplicas secundarias, incluidas 2 réplicas secundarias sincrónicas. 

<sup>3</sup> Standard Edition admite grupos de disponibilidad básica. Un grupo de disponibilidad básica admite dos réplicas, con una base de datos. Para más información sobre los grupos de disponibilidad básica, vea [Grupos de disponibilidad básica](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).    

##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> Escalabilidad y rendimiento de RDBMS  
  
|Característica|Enterprise|Estándar|Web|Express|  
|-------------|----------------|--------------|---------|------------------------| 
|Almacén de columnas <sup>1</sup>|Sí|Sí|Sí|Sí|  
|Archivos binarios de objetos de gran tamaño en índices de almacén de columnas en clúster|Sí|Sí|Sí|Sí|  
|Recompilación de índices de almacén de columnas no en clúster en línea|Sí|No|No|No|
|OLTP en memoria <sup>1</sup>|Sí|Sí|Sí|Sí|
|Memoria principal persistente|Sí|Sí|Sí|Sí|
|Particiones de tabla e índice|Sí|Sí|Sí|Sí|  
|Compresión de datos|Sí|Sí|Sí|Sí|
|regulador de recursos|Sí|No|No|No|  
|Paralelismo de tabla con particiones|Sí|No|No|No|
|Asignación de memoria de página grande habilitada para NUMA y matriz de búferes|Sí|No|No|No|
|Regulación de recursos de E/S|Sí|No|No|No|  
|Perdurabilidad diferida|Sí|Sí|Sí|Sí|
|Ajuste automático|Sí|No|No|No|
|Combinaciones adaptables de modo de proceso por lotes|Sí|No|No|No|
|Comentarios de concesión de memoria de modo de proceso por lotes|Sí|No|No|No|
|Ejecución intercalada de funciones con valores de tabla de múltiples instrucciones|Sí|Sí|Sí|Sí|
|Mejoras de inserción masiva|Sí|Sí|Sí|Sí|


<sup>1</sup> El tamaño de los datos de OLTP en memoria y la memoria caché del segmento del almacén de columnas se limitan a la cantidad de memoria especificada por edición en la sección Límites de escala. Los grados de paralelismo máximos están limitados. El grado paralelismo (DOP) de la compilación de un índice se limita a 2 DOP para Standard Edition y 1 DOP para las ediciones Web y Express. Se refiere a los índices de almacén de columnas que se crean en tablas basadas en disco y en tablas optimizadas para memoria.

##  <a name="rdbms-security"></a><a name="RDBMSS"></a> Seguridad de RDBMS  
  
|Característica|Enterprise|Estándar|Web|Express|
|-------------|----------------|--------------|---------|------------------------------------| 
|Seguridad de nivel de fila|Sí|Sí|Sí|Sí|  
|Always Encrypted|Sí|Sí|Sí|Sí| 
|Enmascaramiento de datos dinámicos|Sí|Sí|Sí|Sí|   
|Auditoría básica|Sí|Sí|Sí|Sí| 
|Auditoría específica|Sí|Sí|Sí|Sí| 
|Cifrado de base de datos transparente|Sí|Sí|No|No|   
|Roles definidos por el usuario|Sí|Sí|Sí|Sí| 
|Bases de datos independientes|Sí|Sí|Sí|Sí| 
|Cifrado para copias de seguridad|Sí|Sí|No|No|  

##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> Capacidad de administración de RDBMS  
  
|Característica|Enterprise|Estándar|Web|Express|   
|-------------|----------------|--------------|---------|------------------------|  
|Conexión de administración dedicada|Sí|Sí|Sí|Sí, con marca de seguimiento|   
|Compatibilidad con PowerShell scripting|Sí|Sí|Sí|Sí| 
|Compatibilidad con las operaciones de componentes de aplicación de capa de datos: extracción, implementación, actualización, eliminación|Sí|Sí|Sí|Sí| 
|Automatización de directivas (comprobar en la programación y cambio)|Sí|Sí|Sí|No|  
|Recopilador de datos de rendimiento|Sí|Sí|Sí|No|
|Informes de rendimiento estándar|Sí|Sí|Sí|No|
|Guías del plan y congelación del plan para las guías del plan|Sí|Sí|Sí|No| 
|Consulta directa de vistas indexadas (mediante la sugerencia NOEXPAND)|Sí|Sí|Sí|Sí| 
|Mantenimiento automático de vistas indexadas|Sí|Sí|Sí|No|
|Vistas distribuidas con particiones|Sí|No|No|No| 
|Operaciones indizadas en paralelo|Sí|No|No|No|  
|Uso automático de vistas indexadas por el optimizador de consultas|Sí|No|No|No| 
|Comprobación de coherencia en paralelo|Sí|No|No|No| 
|Punto de control de la utilidad de SQL Server|Sí|No|No|No|    

##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Característica|Enterprise|Estándar|Web|Express 
|-------------|----------------|--------------|---------|------------------------|  
|JSON|Sí|Sí|Sí|Sí|   
|Almacén de consultas|Sí|Sí|Sí|Sí|   
|Temporal|Sí|Sí|Sí|Sí|   
|Compatibilidad con XML nativo|Sí|Sí|Sí|Sí| 
|Índices XML|Sí|Sí|Sí|Sí| 
|Funciones MERGE y UPSERT|Sí|Sí|Sí|Sí|   
|Tipos de datos de fecha y hora|Sí|Sí|Sí|Sí|  
|Compatibilidad para internacionalización|Sí|Sí|Sí|Sí| 
|Búsqueda de texto completo y semántica|Sí|Sí|Sí|Sí|
|Especificación de idioma en la consulta|Sí|Sí|Sí|Sí|
|Service Broker (mensajería)|Sí|Sí|No (solo cliente)|No (solo cliente)|
|Transact-SQL, extremos|Sí|Sí|Sí|No|
|Grafo|Sí|Sí|Sí|Sí|  


<sup>1</sup> El escalado horizontal con varios nodos de cálculo exige un nodo principal.

## <a name="integration-services"></a><a name="IS"></a> Integration Services

Para obtener información sobre las características de Integration Services (SSIS) compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vea [Características de Integration Services compatibles con las ediciones de SQL Server](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Servicios espaciales y de ubicación  
  
|Nombre de la característica|Enterprise|Estándar|Web|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Índices espaciales|Sí|Sí|Sí|Sí|   
|Tipos de datos planares y geodésicos|Sí|Sí|Sí|Sí| 
|Bibliotecas espaciales avanzadas|Sí|Sí|Sí|Sí|   
|Importación y exportación de formatos de datos espaciales estándar del sector|Sí|Sí|Sí|Sí|   

## <a name="unsupported-features--services"></a><a name="Unsupported"></a> Características y servicios no admitidos

Las siguientes características y servicios no están disponibles para SQL Server 2019 en Linux. La compatibilidad de estas características se incrementará con el tiempo.

| Área | Característica o servicio no admitido |
|-----|-----|
| **Motor de base de datos** | Replicación de mezcla |
| &nbsp; | Stretch DB |
| &nbsp; | Consulta distribuida con conexiones de terceros |
| &nbsp; | Servidores vinculados a orígenes de datos distintos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  |
| &nbsp; | Procedimientos extendidos almacenados del sistema (XP_CMDSHELL, etc.) |
| &nbsp; | Filetable, FILESTREAM |
| &nbsp; | Ensamblados de CLR con el conjunto de permisos EXTERNAL_ACCESS o UNSAFE |
| &nbsp; | Buffer Pool Extension |
| &nbsp; | Copia de seguridad en URL: blob en páginas <sup>2</sup> |
| **Agente SQL Server** |  Subsistemas: CmdExec, PowerShell, Agente de lectura de cola, SSIS, SSAS, SSRS |
| &nbsp; | Alertas |
| &nbsp; | Copia de seguridad administrada |
| **Alta disponibilidad** | Creación de reflejo de la base de datos  |
| **Seguridad** | Administración extensible de claves |
| &nbsp; | Autenticación de AD para servidores vinculados | 
| &nbsp; | Autenticación de AD para grupos de disponibilidad | 
| **Servicios** | SQL Server Browser |
| &nbsp; | SQL Server R Services<sup>1</sup> |
| &nbsp; | StreamInsight |
| &nbsp; | Analysis Services |
| &nbsp; | Reporting Services |
| &nbsp; | Data Quality Services |
| &nbsp; | Master Data Services |

<sup>1</sup> SQL Server R es compatible con SQL Server, pero SQL Server R Services como paquete independiente no lo es.

<sup>2</sup> La copia de seguridad en URL es compatible con los blobs en bloques, mediante la [firma de acceso compartido](../relational-databases/backup-restore/sql-server-backup-to-url.md#SAS).

## <a name="next-steps"></a>Pasos siguientes
 [Características compatibles con las ediciones de SQL Server 2017 en Linux](sql-server-linux-editions-and-components-2017.md)  
 [Ediciones y características admitidas de SQL Server 2019 en Windows](../sql-server/editions-and-components-of-sql-server-version-15.md)  
 [Ediciones y características admitidas de SQL Server 2017 en Windows](../sql-server/editions-and-components-of-sql-server-2017.md)  
 [Ediciones y características admitidas de SQL Server 2016 en Windows](../sql-server/editions-and-components-of-sql-server-2016.md)  
 [Instalación de SQL Server](../database-engine/install-windows/install-sql-server.md)  
 [Especificaciones de producto para SQL Server](../sql-server/index.yml)
