---
title: Grupos de escalado horizontal de PolyBase | Microsoft Docs
description: Use la característica Grupo de PolyBase para crear un clúster de instancias de SQL Server. Esto mejora el rendimiento de las consultas de grandes conjuntos de datos de orígenes externos.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
f1_keywords:
- sql13.swb.polybasescaleoutcluster.page.f1
helpviewer_keywords:
- PolyBase
- PolyBase, scale-out groups
- scale-out PolyBase
ms.assetid: c7810135-4d63-4161-93ab-0e75e9d10ab5
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: c7c2594dfd40ff952c28cf203e05f37c789b3d06
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076666"
---
# <a name="polybase-scale-out-groups"></a>Grupos de escalado horizontal de PolyBase

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Una instancia de SQL Server independiente con PolyBase puede convertirse en un cuello de botella de rendimiento cuando se trabaja con grandes conjuntos de datos en Hadoop o Azure Blog Storage. La característica de grupos de PolyBase permite crear un clúster de instancias de SQL Server para procesar grandes conjuntos de datos a partir de orígenes de datos externos, como Hadoop o Almacenamiento de blobs de Azure, en un modo de escalado horizontal para mejorar el rendimiento de las consultas. Ahora puede escalar la capacidad de proceso de SQL Server para satisfacer las demandas de rendimiento de la carga de trabajo. Los grupos de escalado horizontal de PolyBase, un grupo de instancias de SQL Server, permiten procesar grandes conjuntos de datos externos en una arquitectura de procesamiento en paralelo. El rendimiento de las consultas y la carga de datos puede aumentar linealmente a medida que agrega más instancias de SQL Server al grupo. 
  
Vea [Introducción a PolyBase](./polybase-guide.md) y [Guía de PolyBase](../../relational-databases/polybase/polybase-guide.md).
  
![Diagrama en el que se muestran los grupos de escalado horizontal de PolyBase.](../../relational-databases/polybase/media/polybase-scale-out-groups.png "Grupos de escalado horizontal de PolyBase")  
  
## <a name="head-node"></a>Nodo principal  

El nodo principal contiene la instancia de SQL Server a la que se envían las consultas PolyBase. Cada grupo de PolyBase solo puede tener un nodo principal. Un nodo principal es un grupo lógico formado por el motor de base de datos de SQL Server, el motor de PolyBase y el servicio de movimiento de datos de PolyBase de la instancia de SQL Server. Con SQL Server 2017 y SQL Server 2016, el nodo principal debe ser una edición Enterprise. A partir de SQL Server 2019, el nodo principal de PolyBase puede ser una edición Enterprise o Standard.
  
## <a name="compute-node"></a>Nodo de ejecución

Un nodo de ejecución contiene la instancia de SQL Server que ayuda a escalar horizontalmente el procesamiento de las consultas de los datos externos. Un nodo de ejecución es un grupo lógico de SQL Server y el servicio de movimiento de datos de PolyBase de la instancia de SQL Server. Un grupo de PolyBase puede tener varios nodos de ejecución. El nodo principal y los nodos de ejecución deben ejecutar la misma versión de SQL Server. La versión inicial de SQL Server 2016 permitía que los nodos de proceso fueran una edición Enterprise o Standard. A partir de SQL Server 2016 SP1, todas las ediciones de SQL Server pueden ser un nodo de proceso.

## <a name="scale-out-reads"></a>Lecturas de escalabilidad horizontal

Al consultar instancias externas de SQL Server, Oracle o Teradata, las tablas con particiones se beneficiarán de las lecturas de escalabilidad horizontal. Cada nodo de un grupo de escalado horizontal de PolyBase puede poner en marcha hasta 8 lectores con el fin de leer los datos externos. Y cada lector se asigna a una partición para leer en la tabla externa. 

Por ejemplo, supongamos que tiene una tabla externa de SQL Server con 12 particiones mensuales y un grupo de escalado horizontal de PolyBase de 3 nodos. Cada nodo usará 4 lectores de PolyBase para procesar cada una de las 12 particiones. Este ejemplo se ilustra en la siguiente imagen. 

> [!NOTE]
>  Y es diferente de las lecturas de escalabilidad horizontal a través de Hadoop. 

![Lecturas de escalabilidad horizontal de PolyBase](../../relational-databases/polybase/media/polybase-scale-out-groups2.png "Grupos de escalado horizontal de PolyBase")
  
## <a name="distributed-query-processing"></a>Procesamiento de consultas distribuidas  

Las consultas de PolyBase se envían a SQL Server en el nodo principal. La parte de la consulta que hace referencia a las tablas externas se entregan al motor de PolyBase.
  
El motor de PolyBase es el componente principal que está detrás de las consultas de PolyBase. Analiza la consulta en los datos externos, genera el plan de consulta y distribuye el trabajo al servicio de movimiento de datos de los nodos de ejecución para ejecutarlos. Tras finalizar el trabajo, recibe los resultados de los nodos de ejecución y los envía a SQL Server para procesarlos y devolverlos al cliente.
  
El servicio de movimiento de datos de PolyBase recibe instrucciones del motor de PolyBase y transfiere los datos entre HDFS y SQL Server, y entre las instancias de SQL Server de los nodos principal y de ejecución.
  
## <a name="next-steps"></a>Pasos siguientes

Para configurar un grupo de escalado horizontal de PolyBase, consulte la guía siguiente:

[Improve PolyBase scale-out groups on Windows](configure-scale-out-groups-windows.md) (mejora de los grupos de escalado horizontal de PolyBase en Windows)

## <a name="see-also"></a>Consulte también

 [sys-dm-exec-compute-nodes](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md)   
 [sys-dm-exec-compute-node-status](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-status-transact-sql.md)   
 [sys.dm_exec_compute_node_errors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-node-errors-transact-sql.md)
