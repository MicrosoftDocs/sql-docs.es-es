---
title: Supervisión de clústeres con cuadernos de Jupyter y Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Supervisión de clústeres con cuadernos de Jupyter y Azure Data Studio en una instancia de Clústeres de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bc370b42f247c4b8e57c5628feaae7385ba1502d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039378"
---
# <a name="monitoring-cluster-with-notebooks"></a>Supervisión de clústeres con cuadernos

Esta página es un índice de los cuadernos para Clústeres de macrodatos de SQL Server. Estos cuadernos ejecutables (.ipynb) están diseñados para SQL Server 2019 como ayuda para la supervisión de Clústeres de macrodatos.

Cada cuaderno está diseñado para comprobar sus propias dependencias. Una operación de tipo **Ejecutar todas las celdas** se realizará correctamente o generará una excepción con una sugerencia con hipervínculo a otro cuaderno para resolver la dependencia que falta. Siga el hipervínculo de la sugerencia al cuaderno siguiente, presione **Ejecutar todas las celdas** y, cuando el proceso finalice correctamente, vuelva al cuaderno original y presione **Ejecutar todas las celdas**.

Una vez que se instalan todas las dependencias y que la operación **Ejecutar todas las celdas** produce un error, cada cuaderno analizará los resultados y, siempre que sea posible, generará una sugerencia con hipervínculo a otro cuaderno para ayudarlo a resolver el problema.


## <a name="monitoring-kubernetes"></a>Supervisión de Kubernetes

Esta sección contiene un conjunto de cuadernos útiles para obtener información y el estado del clúster de macrodatos de SQL Server mediante la interfaz de la línea de comandos (CLI) `azdata`.

|Nombre |Descripción |
|---|---|---|---|
|TSG006: obtención del estado del pod del sistema|Vea el estado de todos los pods del sistema. |
|TSG007: obtención del estado del pod del BDC|Visualice el estado de los pods del clúster de macrodatos.|
|TSG008: obtención de información de la versión (Kubernetes)|Obtenga la información del clúster de Kubernetes.|
|TSG009: obtención de nodos (Kubernetes)|Obtenga los contextos de Kubernetes. |
|TSG010: obtención de los contextos de configuración|Uso de la consulta DMV para obtener más información sobre el error del procesador de consultas interno|
|TSG015: visualización de los servicios del BDC (Kubernetes)|Obtenga el estado de los servicios del clúster del BDC implementado en el clúster de Kubernetes. |
|TSG016: descripción de los pods del BDC|Obtenga el estado de los pods del BDC implementado en el clúster de Kubernetes. |
|TSG020: descripción de los nodos (Kubernetes)|Obtenga la información de nodo del clúster del BDC. Use la interfaz de la línea de comandos kubectl. |
|TSG021: obtención de información del clúster (Kubernetes)|Obtenga la información del clúster de Kubernetes. |
|TSG022: obtención de la dirección IP externa del host de kubeadm|Obtenga la dirección IP externa del host de kubeadm. |
|TSG023: obtención de todos los objetos del BDC (Kubernetes)|Obtenga un resumen de todos los recursos de Kubernetes para el espacio de nombres del sistema y del clúster de macrodatos. |
|TSG042: obtención del nombre del nodo y los montajes externos para PVC de datos y registros|Obtenga el nombre del nodo que hospeda el pod junto con los montajes externos de datos y registros. |
|TSG063: obtención de clases de almacenamiento (Kubernetes)|Use este cuaderno para obtener las clases de almacenamiento de Kubernetes. |
|TSG064: obtención de notificaciones de volúmenes persistentes del BDC|Vea las notificaciones de volúmenes persistentes (PVC) del clúster de macrodatos. |
|TSG065: obtención de los secretos del BDC (Kubernetes)|Vea los secretos del clúster de macrodatos. |
|TSG066: obtención de eventos del BDC (Kubernetes)|Vea los eventos del clúster de macrodatos.|
|TSG072: obtención de los volúmenes persistentes (Kubernetes)|Visualice el volumen persistente (PV) del clúster de Kubernetes. Los volúmenes persistentes son objetos que no son espacios de nombres. |
|TSG081: obtención de los espacios de nombres (Kubernetes)|Obtenga los espacios de nombres de Kubernetes. |
|TSG089: descripción de pods que no se ejecutan del BDC|Vea los pods del BDC que no se ejecutan para el clúster de Kubernetes. |
|TSG097: obtención de objetos StatefulSet del BDC (Kubernetes)|Vea los objetos StatefulSet del BDC para el clúster de Kubernetes. |
|TSG098: obtención de los objetos ReplicaSet del BDC (Kubernetes)|Vea los objetos ReplicaSet del BDC para el clúster de Kubernetes. |
|TSG099: obtención de objetos DaemonSet del BDC (Kubernetes)|Vea los objetos DaemonSet del BDC para el clúster de Kubernetes. |


## <a name="monitor-big-data-cluster-bdc"></a>Supervisión del clúster de macrodatos (BDC)

Esta sección contiene un conjunto de cuadernos útiles para obtener información y el estado del clúster de Kubernetes que hospeda un clúster de macrodatos (BDC) de SQL Server.

|Nombre |Descripción |
|---|---|---|---|
|TSG003: visualización de sesiones de Spark del BDC|Vea las sesiones de Spark del BDC. |
|TSG004: visualización de aplicaciones del BDC|Vea las aplicaciones que se ejecutan en el clúster del BDC.|
|TSG012: visualización del estado del BDC|Obtenga el estado actual de los distintos componentes del clúster del BDC.|
|TSG013: visualización de la lista de archivos en el bloque de almacenamiento (HDFS)|Obtenga la lista de archivos en el bloque de almacenamiento (HDFS). |
|TSG014: visualización de los puntos de conexión del BDC|Obtenga todos los puntos de conexión disponibles del clúster del BDC.|
|TSG017: visualización de la configuración del BDC|Obtenga la configuración del BDC. |
|TSG033: visualización del estado de SQL del BDC|Obtenga el estado de SQL Server del BDC implementado en el clúster de Kubernetes. |
|TSG049: visualización del estado del controlador del BDC|Obtenga el estado del controlador del BDC implementado en el clúster de Kubernetes. |
|TSG068: visualización del estado de HDFS del BDC|Obtenga el estado de HDFS del BDC implementado en el clúster de Kubernetes. |
|TSG069: visualización del estado de la puerta de enlace del clúster de macrodatos|Obtenga el estado de la puerta de enlace del BDC implementado en el clúster de Kubernetes. |
|TSG070: consulta del grupo maestro de SQL| Ejecute la consulta SQL en la instancia maestra. |

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].
