---
title: Ediciones y características admitidas
titleSuffix: SQL Server 2016
description: En este artículo se describen las características compatibles con las distintas ediciones de SQL Server 2016 que admiten diferentes requisitos de rendimiento, entorno de ejecución y precio.
ms.custom:
- seo-lt-2019
- sqlfreshmay19
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- Enterprise Edition [SQL Server]
- Developer Edition [SQL Server]
- 32-bit vs. 64-bit editions [SQL Server]
- default components
- Workgroup Edition [SQL Server]
- Internet servers [SQL Server]
- installing SQL Server, components
- Setup [SQL Server], components
- SQL Server, editions
- SQL Server, components
- client/server applications [SQL Server]
- editions [SQL Server]
- versions [SQL Server]
- Setup [SQL Server], editions
- SQL Server Installation Wizard
- components [SQL Server]
- Standard Edition [SQL Server]
- 64-bit edition [SQL Server]
- IIS [SQL Server]
- installing SQL Server, editions
- editions [SQL Server], about edition options
- Setup [SQL Server]
- CPSA
ms.assetid: e5186f02-dd91-47d0-8fa4-de3f41c76903
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 49d32254b0faaf9dea2ae4c31f7e3548179e75a7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236611"
---
# <a name="editions-and-supported-features-of-sql-server-2016"></a>Ediciones y características admitidas de SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  

En este tema se proporcionan detalles de las características compatibles con las ediciones de SQL Server.

Para información sobre otras versiones, consulte:

* [SQL Server 2019](editions-and-components-of-sql-server-version-15.md)  
* [SQL Server 2017](editions-and-components-of-sql-server-2017.md)  
* [SQL Server 2014](/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014?view=sql-server-2014&preserve-view=true).

Los requisitos de instalación varían según las necesidades de las aplicaciones. Las distintas ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] han sido diseñadas para satisfacer los requisitos de rendimiento, tiempo de ejecución y precio propios de cada organización y cada persona. Los componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que instale también dependen de sus necesidades concretas. Las secciones siguientes le servirán de ayuda para elegir la mejor opción entre las ediciones y los componentes disponibles en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

La edición de evaluación de SQL Server está disponible durante un período de prueba de 180 días.  
  
Para leer las notas de la versión más reciente e información sobre las novedades, vea lo siguiente:
- [Notas de la versión de SQL Server 2017](../sql-server/sql-server-2017-release-notes.md)
- [Notas de la versión de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md)
- [Novedades de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [Novedades de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)

### <a name="try-sql-server"></a>Pruebe SQL Server.    
    
> [![Descargar desde el Centro de evaluación](/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) **[Descargar SQL Server 2016 desde el Centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)**    
> 
> ![Máquina virtual de Azure pequeña](/analysis-services/analysis-services/media/azure-virtual-machine-small.png) **[Poner en marcha una máquina virtual con SQL Server 2016 ya instalado](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)**   
  
## <a name="ssnoversion-editions"></a>Ediciones de[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]  
 En la siguiente tabla se describen las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. 
  
|Edición de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Definición|  
|---------------------------------------|----------------|  
|Enterprise|La mejor oferta, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition ofrece funciones de centro de datos de tecnología avanzada completas con un rendimiento ultrarrápido, virtualización ilimitada e inteligencia empresarial integral, lo que habilita los mayores niveles de servicio para las cargas de trabajo de gran importancia y el acceso del usuario final a información sobre los datos.|  
|Estándar|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard Edition proporciona administración básica de datos y base de datos de inteligencia empresarial para que los departamentos y pequeñas organizaciones ejecuten sus aplicaciones y admite las herramientas de desarrollo comunes, tanto locales como en la nube, que habilitan la administración eficaz de bases de datos con recursos de TI mínimos.|  
|Web|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition es una opción de bajo costo total de propiedad que permite a los proveedores de servicios de hosting web y VAP web proporcionar funcionalidades de escalabilidad, rentabilidad y facilidad de administración para propiedades web de pequeña a gran escala.|  
|Desarrollador|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer Edition permite a los desarrolladores compilar cualquier tipo de aplicación en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Incluye toda la funcionalidad de la edición Enterprise, pero tiene licencias para usarse como sistema de prueba y desarrollo, no como un servidor de producción. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer es una opción ideal para los usuarios que compilan [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] y prueban aplicaciones.|  
|Ediciones Express|Express Edition es una base de datos gratuita para principiantes y es ideal para aprender a compilar pequeñas aplicaciones de servidor y de escritorio orientadas a datos. Es la mejor opción para los fabricantes de software independientes, los desarrolladores y los aficionados que compilan aplicaciones cliente. Si necesita características de base de datos más avanzadas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express se puede actualizar sin problemas a otras versiones superiores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB es una versión ligera de Express que tiene todas sus características de programación, pero se ejecuta en modo usuario y tiene una instalación rápida sin configuración y una lista reducida de requisitos previos.|  
  
## <a name="using-ssnoversion-with-an-internet-server"></a>Usar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con un servidor de Internet  
 En un servidor de Internet, como el servidor en el que se ejecuta Internet Information Services (IIS), se instalan normalmente las herramientas de cliente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Las herramientas de cliente incluyen los componentes de conectividad del cliente utilizados por una aplicación que se conecta a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!NOTE]
> Aunque puede instalar una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un equipo en el que se ejecute IIS, solos se suele hacer con sitios web pequeños que tienen un único equipo servidor. La mayoría de los sitios web tienen los sistemas IIS de capa intermedia en un servidor o un clúster de servidores, y las bases de datos en un servidor o federación de servidores independientes.  
  
## <a name="using-ssnoversion-with-clientserver-applications"></a>Usar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con aplicaciones cliente/servidor  
 Puede instalar solo los componentes de cliente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un equipo en el que se ejecuten aplicaciones cliente/servidor conectadas directamente a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Una instalación de componentes de cliente también es una buena opción si administra una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un servidor de bases de datos, o si tiene pensado desarrollar aplicaciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 La opción de herramientas cliente instala las características siguientes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] : componentes de compatibilidad con versiones anteriores, herramientas de datos de [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], componentes de conectividad, herramientas de administración, kit de desarrollo de software y componentes de los Libros en pantalla de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Para obtener más información, consulte [Instalar SQL Server](../database-engine/install-windows/install-sql-server.md).  
  
## <a name="deciding-among-ssnoversion-components"></a>Decidir entre componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]  
 Use la página Selección de características del Asistente para la instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para seleccionar los componentes que va a incluir en la instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. De forma predeterminada, ninguna de las características del árbol están seleccionadas.  
  
 Use la información de las tablas siguientes para determinar el conjunto de características que mejor satisface sus necesidades.  
  
|Componentes de servidor|Descripción|  
|-----------------------|-----------------|  
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] incluye el [!INCLUDE[ssDE](../includes/ssde-md.md)], el servicio principal para almacenar, procesar y proteger datos; también incluye replicación, búsqueda de texto completo, herramientas para administrar datos XML y relacionales, integración de análisis en base de datos e integración de PolyBase para el acceso a Hadoop y a otros orígenes de datos heterogéneos, además del servidor de [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS).|  
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] incluye las herramientas para crear y administrar aplicaciones de procesamiento analítico en línea (OLAP) y de minería de datos.|  
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] incluye componentes de servidor y de cliente para crear, administrar e implementar informes tabulares, matriciales, gráficos y de forma libre. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] es también una plataforma extensible que se puede usar para desarrollar aplicaciones de informes.|  
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] es un conjunto de herramientas gráficas y objetos programables para mover, copiar y transformar datos. También incluye el componente de [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) para [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|  
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) es la solución de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para la administración de datos maestros. MDS se pueden configurar para administrar cualquier dominio (productos, clientes, cuentas) e incluye jerarquías, seguridad específica, transacciones, creación de versiones de datos y reglas de negocios, así como un [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] que se puede usar para administrar datos.|  
|[!INCLUDE[rsql_productname](../includes/rsql-productname-md.md)]|[!INCLUDE[rsql_productname](../includes/rsql-productname-md.md)] admite soluciones de R escalables y distribuidas en varias plataformas, así como el uso de varios orígenes de datos empresariales, como Linux, Hadoop y Teradata.|  
  
|Herramientas de administración|Descripción|  
|----------------------|-----------------|  
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] es un entorno integrado para obtener acceso a los componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]y configurarlos, administrarlos y desarrollarlos. [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] permite a los desarrolladores de software y a los administradores de diversos grados de conocimientos utilizar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].<br /><br /> Descarga e instalación <br />                [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] desde  [Descargar SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Administrador de configuración|El Administrador de configuración de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] proporciona administración de configuración básica para los servicios, protocolos de servidor, protocolos de cliente y alias de cliente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .|  
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] proporciona una interfaz gráfica de usuario para supervisar una instancia del [!INCLUDE[ssDE](../includes/ssde-md.md)] o de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|  
|Asistente para la optimización de[!INCLUDE[ssDE](../includes/ssde-md.md)]|El Asistente para la optimización de[!INCLUDE[ssDE](../includes/ssde-md.md)] crea conjuntos óptimos de índices, vistas indizadas y particiones.|  
|Cliente de calidad de datos|Proporciona una interfaz gráfica de usuario muy simple y intuitiva para conectarse al servidor de DQS y realizar operaciones de limpieza de datos. También permite supervisar de forma centralizada diversas actividades realizadas durante la operación de limpieza de datos.|  
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] proporciona un IDE para compilar soluciones para los componentes de Business Intelligence: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]y [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (Anteriormente denominado Business Intelligence Development Studio).<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] SSDT también incluye "Proyectos de base de datos", que proporcionan un entorno integrado para que los desarrolladores de software de bases de datos realicen todo el trabajo de diseño de base de datos para cualquier plataforma de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (tanto interna como externa) en Visual Studio. Los desarrolladores de software de base de datos pueden usar el Explorador de servidores mejorado en Visual Studio para crear o modificar objetos de base de datos y datos fácilmente, o bien para ejecutar consultas.|  
|Componentes de conectividad|Instala componentes para la comunicación entre clientes y servidores, y bibliotecas de red para DB-Library, ODBC y OLE DB.|  
  
|Documentación|Descripción|  
|-------------------|-----------------|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Libros en pantalla|Documentación principal para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].| 

**Ediciones Developer y Evaluation**  
Para obtener las características compatibles con las ediciones Developer y Evaluation, vea las características enumeradas para SQL Server Enterprise Edition en las tablas siguientes.


La edición Developer sigue siendo compatible con solo un cliente de [SQL Server Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md). 
  
##  <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Scale Limits  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|
|Capacidad máxima de cálculo que usa una sola instancia: [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Sistema operativo máximo|Limitada a menos de 4 sockets o 24 núcleos|Limitada a menos de 4 sockets o 16 núcleos|Limitada a menos de 1 socket o 4 núcleos|Limitada a menos de 1 socket o 4 núcleos| 
|Capacidad máxima de cálculo que usa una sola instancia: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] o [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Sistema operativo máximo|Limitada a menos de 4 sockets o 24 núcleos|Limitada a menos de 4 sockets o 16 núcleos|Limitada a menos de 1 socket o 4 núcleos|Limitada a menos de 1 socket o 4 núcleos|  
|Memoria máxima para el grupo de búferes por cada instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Sistema operativo máximo|128 GB|64 GB|1410 MB|1410 MB|
|Cantidad máxima de memoria para la caché de segmento del almacén de columnas por cada instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria ilimitada| 32 GB<sup>2</sup>| 16 GB<sup>2</sup>| 352 GB<sup>2</sup>| 352 GB<sup>2</sup>|  
|Tamaño máximo de datos optimizados para memoria por base de datos en [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria ilimitada| 32 GB<sup>2</sup>| 16 GB<sup>2</sup>| 352 GB<sup>2</sup>| 352 GB<sup>2</sup>|  
|Memoria máxima usada por instancia de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|Sistema operativo máximo|Tabular: 16 GB<br /><br /> MOLAP: 64 GB|No aplicable|N/D|No aplicable|  
|Memoria máxima usada por instancia de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Sistema operativo máximo|64 GB|64 GB|4 GB|No aplicable|
|Tamaño máximo de la base de datos relacional|524 PB|524 PB|524 PB|10 GB|10 GB|  
  
<sup>1</sup> La licencia basada en Enterprise Edition con licencia de servidor y acceso de cliente (CAL) (no disponible para nuevos contratos) está limitada a un máximo de 20 núcleos por instancia de SQL Server. No hay ningún límite en el modelo de licencias de servidor basado en núcleos. Para obtener más información, consulte [Compute Capacity Limits by Edition of SQL Server](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).  
  
<sup>2</sup> se aplica a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] SP1 como parte de la creación de un área expuesta de programación común (CPSA) entre ediciones. 

##  <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> RDBMS High Availability  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Compatibilidad con Server Core <sup>1</sup>|Sí|Sí|Sí|Sí|Sí|  
|Trasvase de registros|Sí|Sí|Sí|No|No|  
|Creación de reflejo de la base de datos|Sí|Sí<br /><br /> Solo seguridad completa|Solo testigo|Solo testigo|Solo testigo| 
|Compresión de copia de seguridad|Sí|Sí|No|No|No| 
|Instantáneas de base de datos|Sí|Sí <sup>3</sup>|Sí <sup>3</sup>|Sí <sup>3</sup>|Sí <sup>3</sup>|
|Instancias de clúster de conmutación por error de AlwaysOn|Sí<br /><br /> 16|Sí<br /><br /> Compatibilidad con 2 nodos|No|No|No|  
|Grupos de disponibilidad AlwaysOn|Sí<br /><br /> Hasta 8 réplicas secundarias, incluidas 2 réplicas secundarias sincrónicas|No|No|No|No|
|Grupos de disponibilidad básica <sup>2</sup>|No|Sí<br /><br /> Compatibilidad con 2 nodos|No|No|No|
|Restauración de archivos y páginas en línea|Sí|No|No|No|No|
|Índices en línea|Sí|No|No|No|No|
|Cambio de esquema en línea|Sí|No|No|No|No|
|Recuperación rápida|Sí|No|No|No|No|
|Copias de seguridad reflejadas|Sí|No|No|No|No|
|Agregar memoria y CPU sin interrupción|Sí|No|No|No|No|
|Asistente para la recuperación de base de datos|Sí|Sí|Sí|Sí|Sí|
|Copia de seguridad cifrada|Sí|Sí|No|No|No|
|Copia de seguridad híbrida en Azure (copia de seguridad en dirección URL)|Sí|Sí|No|No|No|  
  
 <sup>1</sup> Para obtener más información sobre cómo instalar SQL Server en Server Core, vea [Instalar SQL Server en Server Core](../database-engine/install-windows/install-sql-server-on-server-core.md). 

<sup>2</sup> Para más información sobre los grupos de disponibilidad básica, vea [Grupos de disponibilidad básica](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).  

<sup>3</sup> se aplica a [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)] 2016 SP1 como parte de la creación de un área expuesta de programación común (CPSA) entre ediciones. 
  
##  <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> RDBMS Scalability and Performance  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Almacén de columnas <sup>1</sup>|Sí|Sí <sup>2</sup>|Sí <sup>2</sup>|Sí<sup>2</sup>|Sí<sup>2</sup>|  
|OLTP en memoria <sup>1</sup>|Sí|Sí <sup>2</sup>|Sí <sup>2</sup>|Sí <sup>2</sup>, <sup>3</sup>|Sí <sup>2</sup>|
|Stretch Database|Sí|Sí|Sí|Sí|Sí|
|Memoria principal persistente|Sí|Sí|Sí|Sí|Sí|
|Compatibilidad con varias instancias|50|50|50|50|50|
|Particiones de tabla e índice|Sí|Sí <sup>2</sup>|Sí <sup>2</sup>|Sí <sup>2</sup>|Sí <sup>2</sup>|  
|Compresión de datos|Sí|Sí <sup>2</sup>|Sí <sup>2</sup>|Sí <sup>2</sup>|Sí <sup>2</sup>|
|regulador de recursos|Sí|No|No|No|No|  
|Paralelismo de tabla con particiones|Sí|No|No|No|No|
|Varios contenedores de secuencias de archivo|Sí|Sí <sup>2</sup>|Sí <sup>2</sup>|Sí <sup>2</sup>|Sí <sup>2</sup>|
|Asignación de memoria de página grande habilitada para NUMA y matriz de búferes|Sí|No <sup>4</sup>|No|No|No|
|Buffer Pool Extension|Sí|Sí|No|No|No|
|Regulación de recursos de E/S|Sí|No|No|No|No|
|Lectura anticipada|Sí|No|No|No|No|
|Recorrido avanzado|Sí|No|No|No|No|
|Perdurabilidad diferida|Sí|Sí|Sí|Sí|Sí|

<sup>1</sup> El tamaño de los datos de OLTP en memoria y la memoria caché del segmento del almacén de columnas se limitan a la cantidad de memoria especificada por edición en la sección Límites de escala. Los grados de paralelismo máximos están limitados. Los grados de paralelismo (DOP) para la compilación de un índice se limitan a 2 DOP para Standard Edition y 1 DOP para las ediciones Web y Express. Se refiere a los índices de almacén de columnas que se crean en tablas basadas en disco y en tablas optimizadas para memoria.

<sup>2</sup> Se aplica a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] SP1 como parte de la creación de un área expuesta de programación común (CPSA) entre ediciones. Aplicación de agregados, Aplicación del predicado de la cadena y Optimizaciones de SIMD son mejoras de escalabilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition. Para más detalles, consulte [Novedades de los índices de almacén de columnas](../relational-databases/indexes/columnstore-indexes-what-s-new.md). 

<sup>3</sup> Esta característica no está incluida en la opción de instalación LocalDB.

<sup>4</sup> La licencia basada en Standard Edition y SQL Server con licencia de acceso de cliente (CAL) puede restringir el número de procesadores que puede usar SQL Server Standard, pero SQL Server Standard es compatible con NUMA. 
##  <a name="rdbms-security"></a><a name="RDBMSS"></a> RDBMS Security  
  
|Característica|Enterprise|Estándar|Web|Express|Express con Advanced Services|  
|-------------|----------------|--------------|---------|-------------|------------------------------------| 
|Seguridad de nivel de fila|Sí|Sí|Sí <sup>1</sup>|Sí <sup>1</sup>|Sí <sup>1</sup>|  
|Always Encrypted|Sí|Sí <sup>1</sup>|Sí <sup>1</sup>|Sí <sup>1</sup>|Sí <sup>1</sup>| 
|Enmascaramiento de datos dinámicos|Sí|Sí|Sí <sup>1</sup>|Sí <sup>1</sup>|Sí <sup>1</sup>|   
|Auditoría de servidor|Sí|Sí|Sí|Sí|Sí| 
|Auditoría de base de datos|Sí|Sí <sup>1</sup>|Sí <sup>1</sup>|Sí <sup>1</sup>|Sí <sup>1</sup>| 
|Cifrado de base de datos transparente|Sí|No|No|No|No|   
|Administración extensible de claves|Sí|No|No|No|No| 
|Roles definidos por el usuario|Sí|Sí|Sí|Sí|Sí| 
|Bases de datos independientes|Sí|Sí|Sí|Sí|Sí| 
|Cifrado para copias de seguridad|Sí|Sí|No|No|No|  

<sup>1</sup> se aplica a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] SP1 como parte de la creación de un área expuesta de programación común (CPSA) entre ediciones.      

##  <a name="replication"></a><a name="Replication"></a> Replicación  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Suscriptores heterogéneos|Sí|Sí|No|No|No|  
|Replicación de mezcla|Sí|Sí|Sí (solo suscriptor)|Sí (solo suscriptor)|Sí (solo suscriptor)|   
|Publicación de Oracle|Sí|No|No|No|No| 
|Replicación transaccional punto a punto|Sí|No|No|No|No|   
|Replicación de instantáneas|Sí|Sí|Sí (solo suscriptor)|Sí (solo suscriptor)|Sí (solo suscriptor)|   
|Seguimiento de cambios de SQL Server|Sí|Sí|Sí|Sí|Sí| 
|Replicación transaccional|Sí|Sí|Sí (solo suscriptor)|Sí (solo suscriptor)|Sí (solo suscriptor)|   
|Replicación transaccional a Azure|Sí|Sí|No|No|No|   
|Suscripción actualizable a la replicación transaccional|Sí|Sí|No|No|No|  
  
##  <a name="management-tools"></a><a name="SSMS"></a> Management Tools  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Objetos de administración de SQL (SMO)|Sí|Sí|Sí|Sí|Sí|  
|Administrador de configuración de SQL|Sí|Sí|Sí|Sí|Sí|   
|SQL CMD (herramienta del símbolo del sistema)|Sí|Sí|Sí|Sí|Sí|      
|Distributed Replay: herramienta de administración|Sí|Sí|Sí|Sí|No|  
|Distribute Replay: cliente|Sí|Sí|Sí|No|No|  
|Distributed Replay: controlador|Sí (hasta 16 clientes)|Sí (1 cliente)|Sí (1 cliente)|No|No|   
|SQL Profiler|Sí|Sí|No <sup>1</sup>|No <sup>1</sup>|No <sup>1</sup>|  
|Agente SQL Server|Sí|Sí|Sí|No|No| 
|Paquete de administración de Microsoft System Center Operations Manager|Sí|Sí|Sí|No|No|  
|Asistente para la optimización de motor de base de datos (DTA)|Sí|Sí <sup>2</sup>|Sí <sup>2</sup>|No|No|      
  
 <sup>1</sup> Se pueden generar perfiles de SQL Server Web, SQL Server Express, SQL Server Express con herramientas y SQL Server Express con Advanced Services mediante las ediciones SQL Server Standard y SQL Server Enterprise.  
  
 <sup>2</sup> La optimización se habilita solo en las características de la edición Standard  
  
##  <a name="rdbms-manageability"></a><a name="RDBMSM"></a> RDBMS Manageability  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Instancias de usuario|No|No|No|Sí|Sí| 
|LocalDB|No|No|No|Sí|No| 
|Conexión de administración dedicada|Sí|Sí|Sí|Sí, con marca de seguimiento|Sí, con marca de seguimiento|   
|Compatibilidad con PowerShell scripting|Sí|Sí|Sí|Sí|Sí| 
|Compatibilidad con SysPrep <sup>1</sup>|Sí|Sí|Sí|Sí|Sí| 
|Compatibilidad con las operaciones de componentes de aplicación de capa de datos: extracción, implementación, actualización, eliminación|Sí|Sí|Sí|Sí|Sí| 
|Automatización de directivas (comprobar en la programación y cambio)|Sí|Sí|Sí|No|No|   
|Recopilador de datos de rendimiento|Sí|Sí|Sí|No|No| 
|Capacidad de inscribirse como instancia administrada en una administración de varias instancias|Sí|Sí|Sí|No|No|   
|Informes de rendimiento estándar|Sí|Sí|Sí|No|No| 
|Guías del plan y congelación del plan para las guías del plan|Sí|Sí|Sí|No|No|   
|Consulta directa de vistas indexadas (mediante la sugerencia NOEXPAND)|Sí|Sí|Sí|Sí|Sí| 
|Mantenimiento automático de vistas indexadas|Sí|Sí|Sí|No|No| 
|Vistas distribuidas con particiones|Sí|No|No|No|No| 
|Operaciones indizadas en paralelo|Sí|No|No|No|No|  
|Uso automático de vistas indexadas por el optimizador de consultas|Sí|No|No|No|No| 
|Comprobación de coherencia en paralelo|Sí|No|No|No|No| 
|Punto de control de la utilidad de SQL Server|Sí|No|No|No|No|    
|Extensión del grupo de búferes|Sí|Sí|No|No|No| 
  
 <sup>1</sup> Para más información, vea [Consideraciones acerca de la instalación de SQL Server con SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).  
 
<sup>2</sup> se aplica a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] SP1 como parte de la creación de un área expuesta de programación común (CPSA) entre ediciones.      
  
##  <a name="development-tools"></a><a name="DevTools"></a> Development Tools  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express| 
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Integración de Microsoft Visual Studio|Sí|Sí|Sí|Sí|Sí| 
|IntelliSense (Transact-SQL y MDX)|Sí|Sí|Sí|Sí|Sí| 
|SQL Server Data Tools (SSDT)|Sí|Sí|Sí|Sí|No|    
|Herramientas de edición, depuración y diseño de MDX|Sí|Sí|No|No|No|   
  
##  <a name="programmability"></a><a name="Programmability"></a> Programmability  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express 
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Integración básica de R|Sí|Sí|Sí|Sí|No|   
|Integración avanzada de R|Sí|No|No|No|No| 
|R Server (Standalone)|Sí|No|No|No|No|   
|Nodo de ejecución de PolyBase|Sí|Sí <sup>1</sup>|Sí <sup>1</sup>, <sup>2</sup>|Sí <sup>1</sup>, <sup>2</sup>|Sí <sup>1</sup>, <sup>2</sup>| 
|Nodo principal de PolyBase|Sí|No|No|No|No| 
|JSON|Sí|Sí|Sí|Sí|Sí|   
|Almacén de consultas|Sí|Sí|Sí|Sí|Sí|   
|Temporal|Sí|Sí|Sí|Sí|Sí|   
|Integración de Common Language Runtime (CLR)|Sí|Sí|Sí|Sí|Sí|   
|Compatibilidad con XML nativo|Sí|Sí|Sí|Sí|Sí| 
|Índices XML|Sí|Sí|Sí|Sí|Sí| 
|Funciones MERGE y UPSERT|Sí|Sí|Sí|Sí|Sí|   
|compatibilidad con FILESTREAM|Sí|Sí|Sí|Sí|Sí| 
|FileTable|Sí|Sí|Sí|Sí|Sí| 
|Tipos de datos de fecha y hora|Sí|Sí|Sí|Sí|Sí|  
|Compatibilidad para internacionalización|Sí|Sí|Sí|Sí|Sí| 
|Búsqueda de texto completo y semántica|Sí|Sí|Sí|Sí|No| 
|Especificación de idioma en la consulta|Sí|Sí|Sí|Sí|No|   
|Service Broker (mensajería)|Sí|Sí|No (solo cliente)|No (solo cliente)|No (solo cliente)|   
|Transact-SQL, extremos|Sí|Sí|Sí|No|No| 

<sup>1</sup> El escalado horizontal con varios nodos de cálculo exige un nodo principal.

<sup>2</sup> se aplica a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] SP1 como parte de la creación de un área expuesta de programación común (CPSA) entre ediciones.     
  
## <a name="integration-services"></a><a name="IS"></a> Integration Services

Para obtener información sobre las características de Integration Services (SSIS) compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vea [Características de Integration Services compatibles con las ediciones de SQL Server](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

##  <a name="master-data-services"></a><a name="MDS"></a> Master Data Services  
 Para obtener información sobre las características de [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] y Data Quality Services compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Master Data Services y Data Quality Services compatibles con las ediciones de SQL Server](../master-data-services/master-data-services-and-data-quality-services-features-support.md). 

  
##  <a name="data-warehouse"></a><a name="DW"></a> Data Warehouse  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|   
|-------------|----------------|--------------|---------|------------------------------------|------------------------| 
|Crear cubos sin base de datos|Sí|Sí|No|No|No |   
|Generación automática del esquema de almacenamiento provisional y de almacenamiento de datos|Sí|Sí|No|No|No| 
|captura de datos modificados|Sí|Sí <sup>1</sup>|No|No|No| 
|Optimizaciones de consultas de combinación en estrella|Sí|No|No|No|No| 
|Configuración de solo lectura escalable de Analysis Services|Sí|No|No|No|No| 
|Procesamiento de consultas en paralelo en las tablas e índices con particiones|Sí|No|No|No|No|   
|Agregación global de lotes|Sí|No|No|No|No| 

<sup>1</sup> se aplica a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] SP1 como parte de la creación de un área expuesta de programación común (CPSA) entre ediciones.     

##  <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services  
  
Para obtener información sobre las características de Analysis Services compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Analysis Services compatibles con las ediciones de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016). 
  
##  <a name="bi-semantic-model-multi-dimensional"></a><a name="BIMD"></a> BI Semantic Model (Multi Dimensional)  
  
Para obtener información sobre las características de Analysis Services compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Analysis Services compatibles con las ediciones de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
   
##  <a name="bi-semantic-model-tabular"></a><a name="BIT"></a> BI Semantic Model (Tabular)  
  
Para obtener información sobre las características de Analysis Services compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Analysis Services compatibles con las ediciones de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="power-pivot-for-sharepoint"></a><a name="PPSP"></a> Power Pivot for SharePoint  
  
Para obtener información sobre las características de Power Pivot para SharePoint compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Analysis Services compatibles con las ediciones de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="data-mining"></a><a name="DM"></a> Data Mining  
  
Para obtener información sobre las características de minería de datos compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Analysis Services compatibles con las ediciones de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016).
  
##  <a name="reporting-services"></a><a name="SSRS"></a> Reporting Services  
  
Para obtener información sobre las características de Reporting Services compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Reporting Services compatibles con las ediciones de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

##  <a name="business-intelligence-clients"></a><a name="BIC"></a> Clientes de Business Intelligence  

Para obtener información sobre las características de Business Intelligence Client compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Analysis Services compatibles con las ediciones de SQL Server](/analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016) o [Características de Reporting Services compatibles con las ediciones de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).
  
##  <a name="spatial-and-location-services"></a><a name="SLS"></a> Spatial and Location Services  
  
|Nombre de la característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|  
|------------------|----------------|--------------|---------|------------------------------------|------------------------|
|Índices espaciales|Sí|Sí|Sí|Sí|Sí|   
|Tipos de datos planares y geodésicos|Sí|Sí|Sí|Sí|Sí| 
|Bibliotecas espaciales avanzadas|Sí|Sí|Sí|Sí|Sí|   
|Importación y exportación de formatos de datos espaciales estándar del sector|Sí|Sí|Sí|Sí|Sí|   
  
##  <a name="additional-database-services"></a><a name="ADS"></a> Additional Database Services  
  
|Nombre de la característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------| 
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Migration Assistant|Sí|Sí|Sí|Sí|Sí|   
|Correo electrónico de base de datos|Sí|Sí|Sí|No|No| 
  
##  <a name="other-components"></a><a name="Other"></a> Other Components  
  
|Nombre de la característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|   
|------------------|----------------|--------------|---------|------------------------------------|------------------------|  
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|No|No| 
|StreamInsight HA|StreamInsight Premium Edition|No|No|No|No|   
  
> [![Descargar SSMS](/analysis-services/analysis-services/media/download.png)](../ssms/download-sql-server-management-studio-ssms.md)Descargar la versión más reciente de **[SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)**      
  
## <a name="see-also"></a>Consulte también  
 [Instalar SQL Server](../database-engine/install-windows/install-sql-server.md)  
 
