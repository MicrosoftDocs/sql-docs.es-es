---
title: ¿Qué es la instancia maestra?
titleSuffix: SQL Server big data clusters
description: En este artículo, se describe la instancia maestra de SQL Server en un clúster de macrodatos de SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bd97d5b0da5d88974dcd56aa0f32cbc3243532f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052149"
---
# <a name="what-is-the-master-instance-in-a-sql-server-big-data-cluster"></a>¿Cuál es la instancia maestra en un clúster de macrodatos de SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo, se describe el rol de la *instancia maestra de SQL Server* en un clúster de macrodatos de SQL Server. La instancia maestra es una instancia de SQL Server que se ejecuta en un clúster de macrodatos de SQL Server para administrar la conectividad, las consultas de escalabilidad horizontal, los metadatos y las bases de datos de usuario, así como los servicios de aprendizaje automático.

La instancia maestra de SQL Server proporciona las siguientes funciones:

## <a name="connectivity"></a>Conectividad

La instancia maestra de SQL Server proporciona un punto de conexión TDS accesible externamente para el clúster. Puede conectar aplicaciones o herramientas de SQL Server (como Azure Data Studio o SQL Server Management Studio) a este punto de conexión, igual que conectaría cualquier otra instancia de SQL Server.

## <a name="scale-out-query-management"></a>Administración de consultas de escalado horizontal

La instancia maestra de SQL Server contiene el motor de consultas de escalado horizontal que se usa para distribuir consultas en distintas instancias de SQL Server en nodos del [grupo de proceso](concept-compute-pool.md). El motor de consultas de escalado horizontal también proporciona acceso mediante Transact-SQL a todas las tablas de Hive en el clúster, sin necesidad de usar una configuración adicional.

## <a name="metadata-and-user-databases"></a>Metadatos y bases de datos de usuario

Además de las bases de datos del sistema de SQL Server estándar, la instancia maestra de SQL también contiene lo siguiente:

- Base de datos de metadatos que contiene metadatos de tablas HDFS.
- Mapa de particiones del plano de datos.
- Detalles de las tablas externas que proporcionan acceso al plano de datos del clúster.
- Orígenes de datos externos de PolyBase y tablas externas definidas en bases de datos de usuario.

También puede agregar sus propias bases de datos de usuario a la instancia maestra de SQL Server.

## <a name="machine-learning-services"></a>Machine Learning Services

Machine Learning Services de SQL Server es la característica complementaria para el motor de la base de datos que se usa para ejecutar código de Java, R y Python en SQL Server. Esta característica se basa en el marco de extensibilidad de SQL Server, que aísla los procesos externos de los procesos del motor principal, pero que se integra por completo con los datos relacionales como procedimientos almacenados, como un script de T-SQL que contiene instrucciones de R o Python, o bien como código de Java, R o Python que contiene T-SQL.

Como parte del clúster de macrodatos de SQL Server, la característica Machine Learning Services estará disponible de forma predeterminada en la instancia maestra de SQL Server. Esto quiere decir que, cuando se habilite la ejecución del script externo en la instancia maestra de SQL Server, se podrán ejecutar scripts de Java, R y Python mediante sp_execute_external_script.

### <a name="advantages-of-machine-learning-services-in-a-big-data-cluster"></a>Ventajas de Machine Learning Services en un clúster de macrodatos

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] permite encontrar fácilmente macrodatos para unirlos a los datos dimensionales que suelen almacenarse en la base de datos empresarial. Este valor de los macrodatos se incrementa en gran medida cuando no solo pertenece a una organización, sino que también se incluye en informes, paneles y aplicaciones. Al mismo tiempo, los científicos de datos pueden seguir usando las herramientas del ecosistema de Spark/HDFS y acceder fácilmente y en tiempo real a los datos en la instancia maestra de SQL Server y en orígenes de datos externos accesibles _mediante_ la instancia maestra de SQL Server.

Con [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], puede hacer más cosas con sus lagos de datos empresariales. Los desarrolladores y analistas de SQL Server pueden:

* Crear aplicaciones que usen datos de lagos de datos empresariales.
* Razonar sobre todos los datos con consultas Transact-SQL.
* Usar el ecosistema existente de herramientas de aplicaciones de SQL Server para acceder a datos empresariales y analizarlos.
* Reducir la necesidad del movimiento de datos mediante la virtualización de datos y data marts.
* Seguir usando Spark para escenarios de macrodatos.
* Crea aplicaciones empresariales inteligentes que usen Spark o SQL Server para entrenar modelos con lagos de datos.
* Hacer operativos modelos en bases de datos de producción para obtener el mejor rendimiento.
* Transmitir por streaming datos directamente en data marts empresariales para obtener análisis en tiempo real.
* Explorar datos visualmente mediante análisis interactivos y herramientas de BI.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea los recursos siguientes:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Taller: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
