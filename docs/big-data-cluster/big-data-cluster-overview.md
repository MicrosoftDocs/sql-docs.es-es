---
title: ¿Qué son los clústeres de macrodatos?
titleSuffix: SQL Server Big Data Clusters
description: Obtenga información sobre los clústeres de macrodatos de SQL Server que se ejecutan en Kubernetes y proporcionan opciones de escalabilidad horizontal para datos relacionales y HDFS.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 01/07/2020
ms.topic: overview
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c1109745a2fda000732c0645094dd5a157089717
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047928"
---
# <a name="what-are-big-data-clusters-2019"></a>¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

A partir de [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)], los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ahora permiten implementar clústeres escalables de contenedores de SQL Server, Spark y HDFS que se ejecutan en Kubernetes. Estos componentes se ejecutan en paralelo con objeto de que se puedan leer, escribir y procesar macrodatos de Transact-SQL o Spark, lo que permite combinar y analizar fácilmente los datos relacionales de alto valor con grandes volúmenes de datos.

Use [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] para:

- [Implementación de clústeres escalables](./deploy-get-started.md) de contenedores de SQL Server, Spark y HDFS que se ejecutan en Kubernetes. 
- Leer, escribir y procesar macrodatos desde Transact-SQL o Spark.
- Combinar y analizar de forma sencilla datos relacionales de alto valor con macrodatos de gran volumen.
- Consultar orígenes de datos externos.
- Almacenar macrodatos en HDFS administrados mediante SQL Server.
- Consultar datos de varios orígenes de datos externos a través del clúster.
- Usar los datos para tareas de inteligencia artificial, aprendizaje automático y otras tareas de análisis.
- [Implementar y ejecutar aplicaciones](./concept-application-deployment.md) en [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)].
- Virtualización de datos con [PolyBase](../relational-databases/polybase/polybase-guide.md). Consulte datos de orígenes de datos externos de SQL Server, Oracle, Teradata, MongoDB y ODBC con tablas externas.
- Proporcione alta disponibilidad para la instancia maestra de SQL Server y todas las bases de datos mediante la tecnología de grupos de disponibilidad AlwaysOn.

Para obtener más información sobre las nuevas características y los problemas conocidos de la versión más reciente, consulte las [notas de la versión](release-notes-big-data-cluster.md).

## <a name="scenarios"></a>Escenarios

Los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ofrecen flexibilidad a la hora de interactuar con los macrodatos. Puede consultar orígenes de datos externos, almacenar macrodatos en HDFS administrados por SQL Server o consultar datos de varios orígenes de datos externos a través del clúster. Luego puede usar los datos en tareas de inteligencia artificial, aprendizaje automático y otras tareas de análisis. En las secciones siguientes se proporciona más información sobre estos escenarios.

### <a name="data-virtualization"></a>Virtualización de datos

Al aprovechar [PolyBase](../relational-databases/polybase/polybase-guide.md), las instancias de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] pueden consultar orígenes de datos externos sin necesidad de mover o copiar los datos. [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] introduce nuevos conectores para orígenes de datos.

![Virtualización de datos](media/big-data-cluster-overview/data-virtualization.png)

### <a name="data-lake"></a>Data Lake

Un clúster de macrodatos de SQL Server incluye un *bloque de almacenamiento* de HDFS escalable. Se puede usar para almacenar macrodatos, que pueden ingerirse de varios orígenes externos. Una vez que los macrodatos se almacenan en HDFS en el clúster de macrodatos, se puede analizar y consultar los datos y combinarlos con los datos relacionales.

![Data Lake](media/big-data-cluster-overview/data-lake.png)

### <a name="scale-out-data-mart"></a>Data mart de escalado horizontal

Los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] ofrecen almacenamiento y procesos de escalabilidad horizontal para mejorar el rendimiento del análisis de los datos. Se pueden ingerir y distribuir datos procedentes de diversos orígenes en varios nodos de *grupo de datos* como caché para su análisis posterior.

![DataMart](media/big-data-cluster-overview/data-mart.png)

### <a name="integrated-ai-and-machine-learning"></a>IA y Machine Learning integradas

Los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] permiten realizar tareas de aprendizaje automático y de inteligencia artificial en los datos almacenados en bloques de almacenamiento de HDFS y en los grupos de datos. Puede usar Spark y las herramientas de inteligencia artificial integradas en SQL Server, con R, Python, Scala o Java.

![IA y ML](media/big-data-cluster-overview/ai-ml-spark.png)

### <a name="management-and-monitoring"></a>Administración y supervisión

Se ofrece administración y supervisión mediante una combinación de herramientas de línea de comandos, interfaces API, portales y vistas de administración dinámica.

Puede usar [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md) para realizar diversas tareas en el clúster de macrodatos:
- Fragmentos de código integrados para las tareas de administración comunes.
- Capacidad de examinar HDFS, cargar archivos, obtener una vista previa de los archivos y crear directorios.
- Capacidad para crear, abrir y ejecutar cuadernos compatibles con Jupyter.
- Asistente para la virtualización de datos para simplificar la creación de orígenes de datos externos (habilitado por la **Extensión de virtualización de datos**).

## <a name="architecture"></a><a id="architecture"></a> Arquitectura

Un clúster de macrodatos SQL Server es un clúster de contenedores de Linux organizados por [Kubernetes](https://kubernetes.io/docs/concepts/).

### <a name="kubernetes-concepts"></a>Conceptos de Kubernetes

Kubernetes es un orquestador de contenedores de código abierto, que puede escalar las implementaciones de contenedores según sea necesario. En la tabla siguiente se define alguna terminología importante de Kubernetes:

|Término|Descripción|
|:--|:--|
| **Clúster** | Un clúster de Kubernetes es un conjunto de máquinas, conocidas como nodos. Un nodo controla el clúster y se designa como nodo maestro; los nodos restantes son nodos de trabajo. El maestro de Kubernetes es responsable de distribuir el trabajo entre los nodos de trabajo y de supervisar el estado del clúster. |
| **Node** | Un nodo ejecuta aplicaciones en contenedores. Puede ser una máquina física o una máquina virtual. Un clúster de Kubernetes puede contener una combinación de nodos de máquina física y de máquina virtual. |
| **Pod** | Un pod es la unidad de implementación atómica de Kubernetes. Un pod es un grupo lógico de uno o más contenedores y recursos asociados necesarios para ejecutar una aplicación. Cada pod se ejecuta en un nodo; un nodo puede ejecutar uno o varios pods. El maestro de Kubernetes asigna pods automáticamente a los nodos del clúster. |
| &nbsp; ||

En los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], Kubernetes es responsable del estado de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]; Kubernetes compila y configura los nodos del clúster, asigna pods a los nodos y supervisa el estado del clúster.

### <a name="big-data-clusters-architecture"></a>Arquitectura de clústeres de macrodatos

En el diagrama siguiente se muestran los componentes de un clúster de macrodatos de SQL Server:

![Información general sobre la arquitectura](media/big-data-cluster-overview/architecture-diagram-overview.png)

### <a name="controller"></a><a id="controlplane"></a> Controlador

El controlador proporciona seguridad y administración para el clúster. Contiene el servicio de control, el almacén de configuración y otros servicios en el nivel de clúster, como Kibana, Grafana y búsqueda elástica.

### <a name="compute-pool"></a><a id="computeplane"></a> Grupo de proceso

El grupo de proceso proporciona recursos de cálculo al clúster. Contiene nodos que ejecutan SQL Server en pods de Linux. Los pods del grupo de proceso se dividen en *instancias de proceso de SQL* para tareas de procesamiento específicas. 

### <a name="data-pool"></a><a id="dataplane"></a> Grupo de datos

El grupo de datos se usa para el almacenamiento en caché y la persistencia de datos. El grupo de datos consta de uno o varios pods que ejecutan SQL Server en Linux. Se usa para ingerir datos de consultas SQL o trabajos de Spark. Los data marts del clúster de macrodatos de SQL Server se guardan en el grupo de datos. 

### <a name="storage-pool"></a>Bloque de almacenamiento

El grupo de almacenamiento consiste en módulos de almacenamiento que se componen de SQL Server en Linux, Spark y HDFS. Todos los nodos de almacenamiento de un clúster de macrodatos de SQL Server son miembros de un clúster de HDFS.

> [!TIP]
> Para obtener una visión detallada de la arquitectura y la instalación del clúster de macrodatos, consulte [Workshop: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/microsoft/sqlworkshops-bdc).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consulte [Introducción a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deploy-get-started.md).