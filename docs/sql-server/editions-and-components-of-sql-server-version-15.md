---
description: Ediciones y características admitidas de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]
title: Ediciones y características admitidas de SQL Server 2019 | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
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
ms.assetid: ''
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 98b630f4471d35259db609a5bf1740ff96b8d604
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336178"
---
# <a name="editions-and-supported-features-of-sssql19-md"></a>Ediciones y características admitidas de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx](../includes/applies-to-version/sqlserver.md)]

En este tema se proporcionan detalles de las características compatibles con las diversas ediciones de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)].

Para obtener información sobre versiones anteriores, consulte:

* [SQL Server 2017](editions-and-components-of-sql-server-2017.md)
* [SQL Server 2016](editions-and-components-of-sql-server-2016.md)

Los requisitos de instalación varían según las necesidades de las aplicaciones. Las distintas ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] han sido diseñadas para satisfacer los requisitos de rendimiento, tiempo de ejecución y precio propios de cada organización y cada persona. Los componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] que instale también dependen de sus necesidades concretas. Las secciones siguientes le servirán de ayuda para elegir la mejor opción entre las ediciones y los componentes disponibles en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

La edición de evaluación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] está disponible durante un período de prueba de 180 días.

Para leer las notas de la versión más reciente e información sobre las novedades, vea lo siguiente:

* [Notas de la versión de [!INCLUDE[sssql19-md](../includes/sssql19-md.md)]](../sql-server/sql-server-version-15-release-notes.md)
* [Novedades de la versión [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]2019](../sql-server/what-s-new-in-sql-server-ver15.md)

**Pruebe [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [Descargar[!INCLUDE[sssql19-md](../includes/sssql19-md.md)] del Centro de evaluación](https://www.microsoft.com//evalcenter/evaluate-sql-server)**

## <a name="ssnoversion-editions"></a>Ediciones de[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)]

En la siguiente tabla se describen las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

|Edición de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Definición|
|---------------------------------------|----------------|
|Enterprise|La oferta prémium, [!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Enterprise Edition, proporciona funciones de centro de datos de tecnología avanzada completas con un rendimiento ultrarrápido, virtualización ilimitada<sup>1</sup> e inteligencia empresarial integral, lo que habilita los mayores niveles de servicio para las cargas de trabajo de gran importancia y el acceso del usuario final a información sobre los datos.|
|Estándar|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Standard Edition proporciona administración básica de datos y base de datos de inteligencia empresarial para que los departamentos y pequeñas organizaciones ejecuten sus aplicaciones y admite las herramientas de desarrollo comunes, tanto locales como en la nube, que habilitan la administración eficaz de bases de datos con recursos de TI mínimos.|
|Web|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Web Edition es una opción de bajo costo total de propiedad que permite a los proveedores de servicios de hosting web y VAP web proporcionar funcionalidades de escalabilidad, rentabilidad y facilidad de administración para propiedades web de pequeña a gran escala.|
|Desarrollador|[!INCLUDE[ssNoVersion](../includes/ssNoVersion-md.md)] Developer Edition permite a los desarrolladores compilar cualquier tipo de aplicación en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Incluye toda la funcionalidad de la edición Enterprise, pero tiene licencias para usarse como sistema de prueba y desarrollo, no como un servidor de producción. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Developer es una opción ideal para las personas que compilan y prueban aplicaciones.|
|Ediciones Express|Express Edition es una base de datos gratuita para principiantes y es ideal para aprender a compilar pequeñas aplicaciones de servidor y de escritorio orientadas a datos. Es la mejor opción para los fabricantes de software independientes, los desarrolladores y los aficionados que compilan aplicaciones cliente. Si necesita características de base de datos más avanzadas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express se puede actualizar sin problemas a otras versiones superiores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express LocalDB es una versión ligera de Express que tiene todas sus características de capacidad de programación, se ejecuta en modo usuario y presenta una instalación rápida y sin configuración, y una lista reducida de requisitos previos.|

<sup>1</sup> La virtualización ilimitada está disponible en Enterprise Edition para los clientes con [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default). Las implementaciones deben cumplir con la guía de licencias. Para más información consulte la página sobre precios y licencias.

## <a name="using-ssnoversion-with-clientserver-applications"></a>Uso de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con aplicaciones cliente/servidor

Puede instalar solo los componentes de cliente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un equipo en el que se ejecuten aplicaciones cliente/servidor conectadas directamente a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Una instalación de componentes de cliente también es una buena opción si administra una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un servidor de bases de datos, o si tiene pensado desarrollar aplicaciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

La opción de herramientas cliente instala las características siguientes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] : componentes de compatibilidad con versiones anteriores, herramientas de datos de [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)], componentes de conectividad, herramientas de administración, kit de desarrollo de software y componentes de los Libros en pantalla de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Para obtener más información, consulte [Instalar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/install-sql-server.md).

### <a name="running-with-iis"></a>Ejecución con IIS

En un servidor de Internet, como el servidor en el que se ejecuta Internet Information Services (IIS), se instalan normalmente las herramientas de cliente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Las herramientas de cliente incluyen los componentes de conectividad del cliente utilizados por una aplicación que se conecta a una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

>[!NOTE]
>Aunque puede instalar una instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un equipo en el que se ejecute IIS, esto suele hacerse únicamente para sitios web pequeños que tienen un único equipo servidor. La mayoría de los sitios web tienen los sistemas IIS de capa intermedia en un servidor o un clúster de servidores, y las bases de datos en un servidor o federación de servidores independientes.

## <a name="deciding-among-ssnoversion-components"></a>Decisión entre componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Use la página Selección de características del Asistente para la instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para seleccionar los componentes que va a incluir en la instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. De forma predeterminada, ninguna de las características del árbol están seleccionadas.

Use la información de las tablas siguientes para determinar el conjunto de características que mejor satisface sus necesidades.

|Componentes de servidor|Descripción|
|-----------------------|-----------------|
|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|[!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] incluye [!INCLUDE[ssDE](../includes/ssde-md.md)], el servicio principal para almacenar, procesar y proteger datos; también incluye replicación, búsqueda de texto completo, herramientas para administrar datos XML y relacionales, integración de análisis en base de datos e integración de PolyBase para el acceso a Hadoop y a otros orígenes de datos heterogéneos, y Machine Learning Services para ejecutar scripts de Python y R con datos relacionales.|
|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] incluye las herramientas para crear y administrar aplicaciones de procesamiento analítico en línea (OLAP) y de minería de datos.|
|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] incluye componentes de servidor y de cliente para crear, administrar e implementar informes tabulares, matriciales, gráficos y de forma libre. [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] es también una plataforma extensible que se puede usar para desarrollar aplicaciones de informes.|
|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] es un conjunto de herramientas gráficas y objetos programables para mover, copiar y transformar datos. También incluye el componente de [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) para [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].|
|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]|[!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] (MDS) es la solución de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para la administración de datos maestros. MDS se pueden configurar para administrar cualquier dominio (productos, clientes, cuentas) e incluye jerarquías, seguridad específica, transacciones, creación de versiones de datos y reglas de negocios, así como un [!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] que se puede usar para administrar datos.|
|Machine Learning Services (en base de datos)|Machine Learning Services (en base de datos) admite soluciones de aprendizaje automático distribuidas y escalables con orígenes de datos empresariales. En [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016, se admitía el lenguaje R. [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] admite R y Python.|
|Machine Learning Server (independiente)|Machine Learning Server (independiente) admite la implementación de soluciones de aprendizaje automático distribuidas y escalables en varias plataformas y con varios orígenes de datos empresariales, incluidos Linux y Hadoop. En [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016, se admitía el lenguaje R. [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] admite R y Python.|

|Herramientas de administración|Descripción|
|----------------------|-----------------|
|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]|[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) es un entorno integrado para obtener acceso a los componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y configurarlos, administrarlos y desarrollarlos. SSMS permite a los desarrolladores de software y a los administradores de diversos grados de conocimientos utilizar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La edición más reciente de SSMS actualiza SMO, que incluye la [API de SQL Assessment](../tools/sql-assessment-api/sql-assessment-api-overview.md).<br /><br/> Descarga e instalación <br />[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] desde [Descargar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Management Studio](https://msdn.microsoft.com/library/mt238290.aspx)|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Administrador de configuración|El Administrador de configuración de[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] proporciona administración de configuración básica para los servicios, protocolos de servidor, protocolos de cliente y alias de cliente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .|
|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]|[!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] proporciona una interfaz gráfica de usuario para supervisar una instancia del [!INCLUDE[ssDE](../includes/ssde-md.md)] o de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)].|
|Asistente para la optimización de[!INCLUDE[ssDE](../includes/ssde-md.md)]|El Asistente para la optimización de[!INCLUDE[ssDE](../includes/ssde-md.md)] crea conjuntos óptimos de índices, vistas indizadas y particiones.|
|Cliente de calidad de datos|Proporciona una interfaz gráfica de usuario muy simple y intuitiva para conectarse al servidor de DQS y realizar operaciones de limpieza de datos. También permite supervisar de forma centralizada diversas actividades realizadas durante la operación de limpieza de datos.|
|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]|[!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] proporciona un IDE para compilar soluciones para los componentes de Business Intelligence: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]y [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].<br /><br /> (Anteriormente denominado Business Intelligence Development Studio).<br /><br /> [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)] SSDT también incluye "Proyectos de base de datos", que proporcionan un entorno integrado para que los desarrolladores de software de bases de datos realicen todo el trabajo de diseño de base de datos para cualquier plataforma de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (tanto interna como externa) en Visual Studio. Los desarrolladores de software de base de datos pueden usar el Explorador de servidores mejorado en Visual Studio para crear o modificar objetos de base de datos y datos fácilmente, o bien para ejecutar consultas.|
|Componentes de conectividad|Instala componentes para la comunicación entre clientes y servidores, y bibliotecas de red para DB-Library, ODBC y OLE DB.|

|Documentación|Descripción|
|-------------------|-----------------|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Libros en pantalla|Documentación principal para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|

**Ediciones Developer y Evaluation** Para obtener las características compatibles con las ediciones Developer y Evaluation, vea las características enumeradas para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition en las tablas siguientes.

La edición Developer sigue siendo compatible con solo un cliente de [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Distributed Replay](../tools/distributed-replay/sql-server-distributed-replay.md).

## <a name="scale-limits"></a><a name="Cross-BoxScaleLimits"></a> Límites de escala

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|-------|--------:|------:|---:|-------------------------------:|-----:|
|Capacidad máxima de cálculo que usa una sola instancia: [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]<sup>1</sup>|Sistema operativo máximo|Limitada a menos de 4 sockets o 24 núcleos|Limitada a menos de 4 sockets o 16 núcleos|Limitada a menos de 1 socket o 4 núcleos|Limitada a menos de 1 socket o 4 núcleos|
|Capacidad máxima de cálculo que usa una sola instancia: [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] o [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Sistema operativo máximo|Limitada a menos de 4 sockets o 24 núcleos|Limitada a menos de 4 sockets o 16 núcleos|Limitada a menos de 1 socket o 4 núcleos|Limitada a menos de 1 socket o 4 núcleos|
|Memoria máxima para el grupo de búferes por cada instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Sistema operativo máximo|128 <nobr/>GB|64 <nobr/>GB|1410 <nobr/>MB|1410<nobr/> MB|
|Cantidad máxima de memoria para la caché de segmento del almacén de columnas por cada instancia de [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria ilimitada| 32 <nobr/>GB| 16 <nobr/>GB| 352 <nobr/>MB| 352 <nobr/>MB|
|Tamaño máximo de datos optimizados para memoria por base de datos en [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)]|Memoria ilimitada| 32 <nobr/>GB| 16 <nobr/>GB| 352 <nobr/>MB| 352 <nobr/>MB|
|Memoria máxima usada por instancia de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]|Sistema operativo máximo|16 <nobr/>GB<sup>2</sup><br /><br /> 64 <nobr/>GB<sup>3</sup>|No aplicable|N/D|No aplicable|
|Memoria máxima usada por instancia de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]|Sistema operativo máximo|64 <nobr/>GB|64 <nobr/>GB|4 <nobr/>GB|No aplicable|
|Tamaño máximo de la base de datos relacional|524 <nobr/> PB|524 <nobr/> PB|524 <nobr/> PB|10 <nobr/>GB|10 <nobr/>GB|

<sup>1</sup> La licencia basada en Enterprise Edition con licencia de servidor y acceso de cliente (CAL) (no disponible para nuevos contratos) está limitada a un máximo de 20 núcleos por instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. No hay ningún límite en el modelo de licencias de servidor basado en núcleos. Para obtener más información, vea [Límites de la capacidad de cálculo de cada edición de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../sql-server/compute-capacity-limits-by-edition-of-sql-server.md).

<sup>2</sup> Tabular

<sup>3</sup> MOLAP

## <a name="rdbms-high-availability"></a><a name="RDBMSHA"></a> Alta disponibilidad de RDBMS

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Rápida|
|-------|:--------:|:------:|:-:|--------------------------------:|:-----:|
|Compatibilidad con Server Core<sup>1</sup>|Sí|Sí|Sí|Sí|Sí|
|Trasvase de registros|Sí|Sí|Sí|No|No|
|Creación de reflejo de la base de datos|Sí|Sí<sup>2</sup>|Sí<sup>3</sup>|Sí<sup>3</sup>|Sí<sup>3</sup>|
|Compresión de copia de seguridad|Sí|Sí|No|No|No|
|Instantáneas de base de datos|Sí|Sí|Sí|Sí|Sí|
|Instancias de clúster de conmutación por error de Always On<sup>4</sup>|Sí|Sí|No|No|No|
|Grupos de disponibilidad AlwaysOn<sup>5</sup>|Sí|No|No|No|No|
|Grupos de disponibilidad básica<sup>6</sup>|No|Sí|No|No|No|
|Reenrutamiento de la conexión de lectura y escritura automática |Sí|No|No|No|No|
|Restauración de archivos y páginas en línea|Sí|No|No|No|No|
|Creación y recompilación del índice en línea|Sí|No|No|No|No|
|Recompilaciones de índices en línea reanudables|Sí|No|No|No|No|
|Cambio de esquema en línea|Sí|No|No|No|No|
|Recuperación rápida|Sí|No|No|No|No|
|Recuperación acelerada de bases de datos.|Sí|Sí|Sí|No|No|
|Copias de seguridad reflejadas|Sí|No|No|No|No|
|Agregar memoria y CPU sin interrupción|Sí|No|No|No|No|
|Asistente para la recuperación de base de datos|Sí|Sí|Sí|Sí|Sí|
|Copia de seguridad cifrada|Sí|Sí|No|No|No|
|Copia de seguridad híbrida en Windows Azure (copia de seguridad en dirección URL)|Sí|Sí|Sí|No|No|
|Grupo de disponibilidad sin clúster <sup>5, 6</sup>|Sí|Sí|No|No|No|
|Servidores de conmutación por error para recuperación ante desastres<sup>7</sup>|Sí|Sí|No|No|No|
|Servidores de conmutación por error para alta disponibilidad<sup>7</sup>|Sí|Sí|No|No|No|
|Servidores de conmutación por error para la recuperación ante desastres en Azure<sup>7</sup>|Sí|Sí|No|No|No|

<sup>1</sup> Para más información sobre cómo instalar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Server Core, vea [Instalar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Server Core](../database-engine/install-windows/install-sql-server-on-server-core.md).

<sup>2</sup> Solo seguridad completa

<sup>3</sup> Solo testigo

<sup>4</sup> En Enterprise Edition, el número de nodos es el máximo del sistema operativo. En Standard Edition hay compatibilidad con dos nodos.

<sup>5</sup> En Enterprise Edition, proporciona compatibilidad con hasta 8 réplicas secundarias, incluidas 5 réplicas secundarias sincrónicas.

<sup>6</sup> Standard Edition admite grupos de disponibilidad básica. Un grupo de disponibilidad básica admite dos réplicas, con una base de datos. Para más información sobre los grupos de disponibilidad básica, vea [Grupos de disponibilidad básica](../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).

<sup>7</sup> Se requiere [Software assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default).

## <a name="rdbms-scalability-and-performance"></a><a name="RDBMSSP"></a> Escalabilidad y rendimiento de RDBMS

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|------:|:--------:|:------:|:-:|:-------------------------------:|:------:|
|Almacén de columnas<sup>1</sup> <sup>2</sup>|Sí|Sí|Sí|Sí|Sí|
|Archivos binarios de objetos de gran tamaño en índices de almacén de columnas en clúster|Sí|Sí|Sí|Sí|Sí|
|Recompilación de índices de almacén de columnas no en clúster en línea|Sí|No|No|No|No|
|Base de datos en memoria: OLTP en memoria<sup>1</sup>|Sí|Sí|Sí|Sí<sup>3</sup>|Sí|
|Base de datos en memoria: grupo de búferes híbrido|Sí|Sí|No|No|No|
|Base de datos en memoria: metadatos tempdb optimizados para memoria|Sí|No|No|No|No|
|Base de datos en memoria: compatibilidad con memoria persistente|Sí|Sí|Sí|Sí|Sí|
|Stretch Database|Sí|Sí|Sí|Sí|Sí|
|Compatibilidad con varias instancias|50|50|50|50|50|
|Particiones de tabla e índice|Sí|Sí|Sí|Sí|Sí|
|Compresión de datos|Sí|Sí|Sí|Sí|Sí|
|Regulador de recursos|Sí|No|No|No|No|
|Paralelismo de tabla con particiones|Sí|No|No|No|No|
|Varios contenedores de secuencias de archivo|Sí|Sí|Sí|Sí|Sí|
|Asignación de memoria de página grande habilitada para NUMA y matriz de búferes|Sí|No|No|No|No|
|Extensión del grupo de búferes|Sí|Sí|No|No|No|
|Regulación de recursos de E/S|Sí|No|No|No|No|
|Lectura anticipada|Sí|No|No|No|No|
|Examen avanzado|Sí|No|No|No|No|
|Durabilidad diferida|Sí|Sí|Sí|Sí|Sí|
|Base de datos inteligente: ajuste automático|Sí|No|No|No|No|
|Base de datos inteligente: modo por lotes para el almacén de filas <sup>1</sup>|Sí|No|No|No|No|
|Base de datos inteligente: comentarios de concesión de memoria de modo de fila|Sí|No|No|No|No|
|Base de datos inteligente: recuento aproximado distinto|Sí|Sí|Sí|Sí|Sí|
|Base de datos inteligente: compilación diferida de variables de tabla|Sí|Sí|Sí|Sí|Sí|
|Base de datos inteligente: inserción de UDF escalar|Sí|Sí|Sí|Sí|Sí|
|Combinaciones adaptables de modo de proceso por lotes|Sí|No|No|No|No|
|Comentarios de concesión de memoria de modo de proceso por lotes|Sí|No|No|No|No|
|Ejecución intercalada de funciones con valores de tabla de múltiples instrucciones|Sí|Sí|Sí|Sí|Sí|
|Mejoras de inserción masiva|Sí|Sí|Sí|Sí|Sí|

<sup>1</sup> El tamaño de los datos de OLTP en memoria y la memoria caché del segmento del almacén de columnas se limitan a la cantidad de memoria especificada por edición en la sección [Límites de escala](#Cross-BoxScaleLimits). El grado de paralelismo (DOP) de las operaciones del [modo de proceso por lotes](../relational-databases/query-processing-architecture-guide.md#batch-mode-execution) está limitado a dos para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard Edition y a uno para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web Edition y Express Edition. Se refiere a los índices de almacén de columnas que se crean en tablas basadas en disco y en tablas optimizadas para memoria.

<sup>2</sup> Aplicación de agregados, Aplicación del predicado de la cadena y Optimizaciones de SIMD son mejoras de escalabilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise Edition. Para más detalles, consulte [Novedades de los índices de almacén de columnas](../relational-databases/indexes/columnstore-indexes-what-s-new.md).

<sup>3</sup> Esta característica no está incluida en la opción de instalación LocalDB.

## <a name="rdbms-security"></a><a name="RDBMSS"></a> Seguridad de RDBMS

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-----:|:-----------------------:|:-----:|
|Seguridad de nivel de fila|Sí|Sí|Sí|Sí|Sí|
|Always Encrypted|Sí|Sí|Sí|Sí|Sí|
|Always Encrypted con enclaves seguros|Sí|Sí|Sí|Sí|Sí|
|Enmascaramiento de datos dinámicos|Sí|Sí|Sí|Sí|Sí|
|Auditoría de servidor|Sí|Sí|Sí|Sí|Sí|
|Auditoría de base de datos|Sí|Sí|Sí|Sí|Sí|
|Cifrado de base de datos transparente|Sí|Sí|Sí|No|No|
|Administración extensible de claves|Sí|Sí|No|No|No|
|Roles definidos por el usuario|Sí|Sí|Sí|Sí|Sí|
|Bases de datos independientes|Sí|Sí|Sí|Sí|Sí|
|Cifrado para copias de seguridad|Sí|Sí|No|No|No|
|Clasificación y auditoría de datos|Sí|Sí|Sí|Sí|Sí|

## <a name="replication"></a><a name="Replication"></a> Replicación

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|-------|:---------:|:-------:|:--:|:--------------------------------:|:------:|
|Suscriptores heterogéneos|Sí|Sí|No|No|No|
|Replicación de mezcla|Sí|Sí|Sí<sup>1</sup>|Sí<sup>1</sup>|Sí<sup>1</sup>|
|Publicación de Oracle|Sí|No|No|No|No|
|Replicación transaccional punto a punto|Sí|No|No|No|No|
|Replicación de instantáneas|Sí|Sí|Sí<sup>1</sup>|Sí<sup>1</sup>|Sí<sup>1</sup>|
|Seguimiento de los cambios de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Sí|Sí|Sí|Sí|Sí|
|Replicación transaccional|Sí|Sí|Sí<sup>1</sup>|Sí<sup>1</sup>|Sí<sup>1</sup>|
|Replicación transaccional a Azure|Sí|Sí|No|No|No|
|Suscripción actualizable a la replicación transaccional|Sí|Sí|No|No|No|

<sup>1</sup> Solo suscriptor

## <a name="management-tools"></a><a name="SSMS"></a> Herramientas de administración

|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|
|-------|:--------:|:------:|:-:|:----------------------------:|:-----:|
|Objetos de administración de SQL (SMO)|Sí|Sí|Sí|Sí|Sí|
|API de SQL Assessment|Sí|Sí|Sí|Sí|Sí|
|Evaluación de vulnerabilidad de SQL |Sí|Sí|Sí|Sí|Sí|
|Administrador de configuración de SQL|Sí|Sí|Sí|Sí|Sí|
|SQL CMD (herramienta del símbolo del sistema)|Sí|Sí|Sí|Sí|Sí|
|Distributed Replay: herramienta de administración|Sí|Sí|Sí|Sí|No|
|Distribute Replay: cliente|Sí|Sí|Sí|No|No|
|Distributed Replay: controlador|Sí<sup>1</sup>|Sí<sup>2</sup>|Sí<sup>2</sup>|No|No|
|SQL Profiler|Sí|Sí|No<sup>3</sup>|No<sup>3</sup>|No<sup>3</sup>|
|e[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Sí|Sí|Sí|No|No|
|Paquete de administración de Microsoft System Center Operations Manager|Sí|Sí|Sí|No|No|
|Asistente para la optimización de motor de base de datos (DTA)|Sí|Sí<sup>4</sup>|Sí<sup>4</sup>|No|No|

<sup>1</sup> Hasta 16 clientes

<sup>2</sup> 1 cliente

<sup>3</sup> Se pueden generar perfiles de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Web, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express con Tools y [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Express con Advanced Services mediante las ediciones [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Standard y [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Enterprise.

<sup>4</sup> La optimización se habilita solo en las características de la edición Standard

## <a name="rdbms-manageability"></a><a name="RDBMSM"></a> Capacidad de administración de RDBMS

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Instancias de usuario|No|No|No|Sí|Sí|
|LocalDB|No|No|No|Sí|No|
|Conexión de administración dedicada|Sí|Sí|Sí|Sí<sup>1</sup>|Sí<sup>1</sup>|
|Compatibilidad con SysPrep<sup>2</sup>|Sí|Sí|Sí|Sí|Sí|
|Compatibilidad con scripting de PowerShell<sup>3</sup>|Sí|Sí|Sí|Sí|Sí|
|Compatibilidad con las operaciones de componentes de aplicación de capa de datos: extracción, implementación, actualización, eliminación|Sí|Sí|Sí|Sí|Sí|
|Automatización de directivas (comprobar en la programación y cambio)|Sí|Sí|Sí|No|No|
|Recopilador de datos de rendimiento|Sí|Sí|Sí|No|No|
|Capacidad de inscribirse como instancia administrada en una administración de varias instancias|Sí|Sí|Sí|No|No|
|Informes de rendimiento estándar|Sí|Sí|Sí|No|No|
|Guías del plan y congelación del plan para las guías del plan|Sí|Sí|Sí|No|No|
|Consulta directa de vistas indexadas (mediante la sugerencia NOEXPAND)|Sí|Sí|Sí|Sí|Sí|
|Consulta directa de SQL Server Analysis Services|Sí|Sí|No|No|Sí|
|Mantenimiento automático de vistas indexadas|Sí|Sí|Sí|No|No|
|Vistas distribuidas con particiones|Sí|No|No|No|No|
|Operaciones indizadas en paralelo|Sí|No|No|No|No|
|Uso automático de vistas indexadas por el optimizador de consultas|Sí|No|No|No|No|
|Comprobación de coherencia en paralelo|Sí|No|No|No|No|
|Punto de control de la utilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Sí|No|No|No|No|
|Extensión del grupo de búferes|Sí|Sí|No|No|No|
|Instancia maestra para el clúster de macrodatos|Sí|Sí|No|No|No|
|Certificación de compatibilidad|Sí|Sí|Sí|Sí|Sí|

<sup>1</sup> Con marca de seguimiento

<sup>2</sup> Para más información, vea [Consideraciones acerca de la instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con SysPrep](../database-engine/install-windows/considerations-for-installing-sql-server-using-sysprep.md).

<sup>3</sup> En Linux, se admiten los scripts de PowerShell, desde equipos de Windows que se dirigen a los servidores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Linux.

## <a name="development-tools"></a><a name="DevTools"></a> Herramientas de desarrollo

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Integración de Microsoft Visual Studio|Sí|Sí|Sí|Sí|Sí|
|IntelliSense (Transact-SQL y MDX)|Sí|Sí|Sí|Sí|Sí|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools (SSDT)|Sí|Sí|Sí|Sí|No|
|Herramientas de edición, depuración y diseño de MDX|Sí|Sí|No|No|No|

## <a name="programmability"></a><a name="Programmability"></a> Programmability

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Rápida
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Integración básica de R<sup>1</sup>|Sí|Sí|Sí|Sí|No|
|Integración avanzada de R<sup>2</sup>|Sí|No|No|No|No|
|Integración básica de Python|Sí|Sí|Sí|Sí|No|
|Integración avanzada de Python|Sí|No|No|No|No|
|Machine Learning Server (independiente)|Sí|No|No|No|No|
|Nodo de ejecución de PolyBase|Sí|Sí<sup>3</sup>|Sí<sup>3</sup>|Sí<sup>3</sup>|Sí<sup>3</sup> |
|Nodo principal de PolyBase<sup>4</sup>|Sí|Sí|No|No|No|
|JSON|Sí|Sí|Sí|Sí|Sí|
|Almacén de consultas|Sí|Sí|Sí|Sí|Sí|
|Temporal|Sí|Sí|Sí|Sí|Sí|
|Integración de Common Language Runtime (CLR)|Sí|Sí|Sí|Sí|Sí|
|Integración de Java Language Runtime|Sí|Sí|Sí|Sí|Sí|
|Compatibilidad con XML nativo|Sí|Sí|Sí|Sí|Sí|
|Índices XML|Sí|Sí|Sí|Sí|Sí|
|Funciones MERGE y UPSERT|Sí|Sí|Sí|Sí|Sí|
|compatibilidad con FILESTREAM|Sí|Sí|Sí|Sí|Sí|
|FileTable|Sí|Sí|Sí|Sí|Sí|
|Tipos de datos de fecha y hora|Sí|Sí|Sí|Sí|Sí|
|Compatibilidad para internacionalización|Sí|Sí|Sí|Sí|Sí|
|Búsqueda de texto completo y semántica|Sí|Sí|Sí|Sí|No|
|Especificación de idioma en la consulta|Sí|Sí|Sí|Sí|No|
|Service Broker (mensajería)|Sí|Sí|No<sup>5</sup>|No<sup>5</sup>|No<sup>5</sup>|
|Transact-SQL, extremos|Sí|Sí|Sí|No|No|
|Grafo|Sí|Sí|Sí|Sí|Sí|
|Compatibilidad con UTF-8|Sí|Sí|Sí|Sí|Sí|

<sup>1</sup> La integración básica se limita a dos núcleos y conjuntos de datos en memoria.

<sup>2</sup> La integración avanzada puede usar todos los núcleos disponibles para el procesamiento paralelo de conjuntos de datos de cualquier tamaño sujeto a las limitaciones del hardware.

<sup>3</sup> El escalado horizontal con varios nodos de cálculo exige un nodo principal.

<sup> 4 </sup> Antes de SQL Server 2019, el nodo principal de PolyBase requiere Enterprise Edition.

<sup>5</sup> Solo cliente

## <a name="integration-services"></a><a name="IS"></a> Integration Services

Para obtener información sobre las [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]características de Integration Services (SSIS) compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], vea [Características de Integration Services compatibles con las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../integration-services/integration-services-features-supported-by-the-editions-of-sql-server.md).

## <a name="master-data-services"></a><a name="MDS"></a> Master Data Services

Para obtener información sobre las características de [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] y Data Quality Services compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Master Data Services y Data Quality Services compatibles con las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../master-data-services/master-data-services-and-data-quality-services-features-support.md).

## <a name="data-warehouse"></a><a name="DW"></a> Almacenamiento de datos

|Característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Rápida|
|-------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|Ensayo de generación automática y esquema de almacenamiento de datos|Sí|Sí|No|No|No|
|captura de datos modificados|Sí|Sí|No|No|No|
|Optimizaciones de consultas de combinación en estrella|Sí|No|No|No|No|
|Procesamiento de consultas en paralelo en las tablas e índices con particiones|Sí|No|No|No|No|
|Agregación global de lotes|Sí|No|No|No|No|

## <a name="analysis-services"></a><a name="SSAS"></a> Analysis Services

Para obtener información sobre las características de Analysis Services compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Analysis Services compatibles con las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="reporting-services"></a><a name="SSRS"></a> Reporting Services

Para obtener información sobre las características de Reporting Services compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Reporting Services compatibles con las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="business-intelligence-clients"></a><a name="BIC"></a> Clientes de inteligencia empresarial

Para obtener información sobre las características de Business Intelligence Client compatibles con las ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)], consulte [Características de Analysis Services compatibles con las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../analysis-services/analysis-services-features-supported-by-the-editions-of-sql-server-2016.md) o [Características de Reporting Services compatibles con las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md).

## <a name="spatial-and-location-services"></a><a name="SLS"></a> Servicios espaciales y de ubicación

|Nombre de la característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|-------------|:-------:|:------:|:-:|:-------------------------------:|:-----:|
|Índices espaciales|Sí|Sí|Sí|Sí|Sí|
|Tipos de datos planares y geodésicos|Sí|Sí|Sí|Sí|Sí|
|Bibliotecas espaciales avanzadas|Sí|Sí|Sí|Sí|Sí|
|Importación y exportación de formatos de datos espaciales estándar del sector|Sí|Sí|Sí|Sí|Sí|

## <a name="additional-database-services"></a><a name="ADS"></a> Servicios de bases de datos adicionales

|Nombre de la característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Migration Assistant|Sí|Sí|Sí|Sí|Sí|
|Correo electrónico de base de datos|Sí|Sí|Sí|No|No|

## <a name="other-components"></a><a name="Other"></a> Otros componentes

|Nombre de la característica|Enterprise|Estándar|Web|Express con<br/>Advanced Services|Express|
|------------|:--------:|:------:|:-:|:-------------------------------:|:-----:|
|StreamInsight|StreamInsight Premium Edition|StreamInsight Standard Edition|StreamInsight Standard Edition|No|No|
|StreamInsight HA|StreamInsight Premium Edition|No|No|No|No|

## <a name="next-steps"></a>Pasos siguientes

[Especificaciones de producto para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](https://msdn.microsoft.com/library/6445fd53-6844-4170-a86b-7fe76a9f64cb)

[Instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]](../database-engine/install-windows/installation-for-sql-server-2016.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
