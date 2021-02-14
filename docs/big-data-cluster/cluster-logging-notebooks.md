---
title: Recopilación y análisis de registros con cuadernos de Jupyter y Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Registro de clústeres con cuadernos de Jupyter y Azure Data Studio en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8609fbe12e6218d328528dd5577bf1a40167162
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047785"
---
# <a name="gathering-and-analyzing-logs-in-the-cluster-with-notebooks"></a>Recopilación y análisis de registros en el clúster con cuadernos

Esta página es un índice de los cuadernos para Clústeres de macrodatos de SQL Server. Estos cuadernos ejecutables (.ipynb) están diseñados para SQL Server 2019 como ayuda para el registro de Clústeres de macrodatos.

Cada cuaderno está diseñado para comprobar sus propias dependencias. Una operación de tipo **Ejecutar todas las celdas** se realizará correctamente o generará una excepción con una sugerencia con hipervínculo a otro cuaderno para resolver la dependencia que falta. Siga el hipervínculo de la sugerencia al cuaderno siguiente, presione **Ejecutar todas las celdas** y, cuando el proceso finalice correctamente, vuelva al cuaderno original y presione **Ejecutar todas las celdas**.

Una vez que se instalan todas las dependencias y que la operación **Ejecutar todas las celdas** produce un error, cada cuaderno analizará los resultados y, siempre que sea posible, generará una sugerencia con hipervínculo a otro cuaderno para ayudarlo a resolver el problema.

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>Recopilación de registros del Clúster de macrodatos (BDC)

Esta sección contiene un conjunto de cuadernos útiles para obtener registros de un Clúster de macrodatos (BDC) de SQL Server.

| Nombre | Descripción |
|--|--|
| TSG001: ejecución de registros de copia de azdata | Use la interfaz de la línea de comandos de azdata para copiar datos en clústeres BDC. |
| TSG061: obtención del final de todos los registros de contenedor de los pods del espacio de nombres del BDC | Obtenga todos los registros de contenedor para pods del clúster de BDC en el espacio de nombres. |
| TSG062: obtención del final de todos los registros de contenedor anteriores de los pods del espacio de nombres del BDC | Obtenga todos los registros de contenedor anteriores para los pods del clúster de BDC en el espacio de nombres. |
| TSG083: ejecución del volcado de memoria de información del clúster de kubectl | Use la interfaz de la línea de comandos kubetl para volcar información relacionada con el clúster de BDC. |
| TSG084: Error interno del procesador de consultas | Use la consulta DMV para obtener más información sobre el error del procesador de consultas interno. |
| TSG091: obtención de los registros de la CLI de azdata | Obtenga los registros de azdata desde el equipo local. |



## <a name="analyse-logs-from-big-data-clusters-bdc"></a>Análisis de registros de Clústeres de macrodatos (BDC)

Conjunto de cuadernos para recopilar y analizar registros de un clúster de macrodatos de SQL Server.  El proceso de análisis sugerirá que se ejecuten cuadernos de seguimiento para detectar problemas conocidos en los registros.

|Nombre|Descripción |
|---|---|
|TSG030: archivos del registro de errores de SQL Server|Obtenga archivos de registro de errores de SQL Server, analice las entradas de registro y sugiera otras guías de resolución de problemas pertinentes. |
|TSG031: registros PolyBase de SQL Server|Obtenga registros de PolyBase de SQL Server, analice las entradas de registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG034: registros de Livy|Obtenga registros de Livy, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG035: registros del historial de Spark|Obtenga registros del historial de Spark, analice las entradas de registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG036: registros del controlador|Obtenga las últimas "n" horas de registros del controlador, analice las entradas de registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG046: registros de la puerta de enlace de Knox|Knox muestra un error 500 al cliente y quita los detalles (la pila) que apuntan a la causa del problema subyacente. Por lo tanto, use este cuaderno para obtener los registros de Knox del clúster. Obtenga registros de la puerta de enlace de Knox, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG073: registros InfluxDB|Obtenga registros de InfluxDB, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG076: Registros de búsqueda elástica|Obtenga registros de Búsqueda elástica, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG077: Registros de Kibana|Obtenga registros de Kibana, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG088: registros de nodo de datos de Hadoop|Obtenga registros de nodo de datos de Hadoop, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG090: registros de administrador de nodo de Yarn|Obtenga registros de administrador de nodos de Yarn, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG092: final del registro de Supervisord para todos los contenedores del BDC|Obtenga el final del registro de Supervisord para todos los contenedores en BDC, analice las entradas de registro y sugiera otras guías de resolución de problemas pertinentes.|
|TSG093: final del registro del agente para todos los contenedores del BDC|Obtenga registros del final del registro del agente para todos los contenedores de BDC, analice las entradas de registro y sugiera otras guías de resolución de problemas pertinentes.|
|TSG094: registros de Grafana|Obtenga registros de Grafana, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG095: registros de namenode de Hadoop|Obtenga registros de namenode de Hadoop, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|
|TSG096: registros de Zookeeper|Obtenga registros de Zookeeper, analice las entradas del registro y sugiera otras guías de solución de problemas pertinentes.|

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].
