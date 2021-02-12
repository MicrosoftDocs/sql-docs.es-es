---
title: Escenario común de trabajo con Clústeres de macrodatos (BDC) con cuadernos de Jupyter y Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Escenario común de trabajo con BDC con cuadernos de Jupyter y Azure Data Studio en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8cbf034873b690262ff6acb395ed8eca5d09ae79
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039515"
---
# <a name="common-notebooks-for-sql-server-big-data-clusters"></a>Cuadernos comunes para Clústeres de macrodatos de SQL Server

En este artículo se enumeran los cuadernos para Clústeres de macrodatos de SQL Server. Los notebooks ejecutables (. ipynb) están diseñados para SQL Server 2019 como ayuda para mostrar escenarios comunes sobre Clústeres de macrodatos.

Cada cuaderno está diseñado para comprobar sus propias dependencias. Una operación de tipo **Ejecutar todas las celdas** se realiza correctamente o genera una excepción con una *sugerencia* con hipervínculo a otro cuaderno para resolver la dependencia que falta. Siga el hipervínculo de la sugerencia al cuaderno siguiente, presione **Ejecutar todas las celdas** y, cuando el proceso finalice correctamente, vuelva al cuaderno original y presione **Ejecutar todas las celdas**.

Una vez que se instalan todas las dependencias y que la operación **Ejecutar todas las celdas** produce un error, cada cuaderno analizará los resultados y, siempre que sea posible, generará una sugerencia con hipervínculo a otro cuaderno para ayudarlo a resolver el problema.

## <a name="gathering-logs-from-big-data-cluster-bdc"></a>Recopilación de registros del Clúster de macrodatos (BDC)

Los cuadernos de esta sección se usan como requisitos previos para otros cuadernos, como el inicio y el cierre de sesión de un clúster.

|Nombre |Descripción |
|---|---|
|SOP005: inicio de sesión de az|Use la interfaz de la línea de comandos de az para iniciar sesión en Azure. |
|SOP006: cierre de sesión de az|Use la interfaz de la línea de comandos de az para cerrar la sesión de Azure.|
|SOP007: información de versión (azdata, BDC, Kubernetes)|Defina las funciones auxiliares que se usan en el cuaderno sobre la información del control de versiones.|
|SOP011: establecimiento del contexto de configuración de Kubernetes|Establezca la configuración de Kubernetes que se va a usar. |
|SOP028: inicio de sesión de azdata|Use la interfaz de la línea de comandos de azdata para iniciar sesión en un clúster de macrodatos. |
|SOP033: cierre de sesión de azdata|Use la interfaz de la línea de comandos de azdata para cerrar sesión en un clúster de macrodatos. |
|SOP034: Espera hasta que el BDC esté en buen estado|Se bloquea hasta que el Clúster de macrodatos esté en buen estado o el tiempo de espera especificado expire. El parámetro min_pod_count indica que la verificación de estado no se aprobará hasta que exista al menos esta cantidad de pods en el clúster. Si alguno de los pods existentes más allá de este límite no está en buen estado, el clúster no está en buen estado.|
|OPR001: Creación de App-Deploy|Use este cuaderno para crear una aplicación en el Clúster de macrodatos. |
|OPR002: Ejecución de App-Deploy|Use este cuaderno para ejecutar una aplicación en el Clúster de macrodatos. |
|OPR003: Creación de cronjob|Use este cuaderno para crear un cronjob en un Clúster de macrodatos. |
|OPR004: Suspensión de cronjob|Use este cuaderno para suspender un cronjob en el Clúster de macrodatos. |
|OPR005: Reanudación de cronjob|Use este cuaderno para reanudar un cronjob en el Clúster de macrodatos. |
|OPR006: Eliminación de cronjob|Use este cuaderno para eliminar un cronjob en un Clúster de macrodatos. |
|OPR007: Eliminación de App-Deploy|Use este cuaderno para eliminar una aplicación en el Clúster de macrodatos. |
|OPR100: Implementación y programación de cuadernos|Utilice este cuaderno para implementar una lista de cuadernos en un pod de App-Deploy, ejecutar App-Deploy según una programación con un CronJob de Kubernetes e instalar un panel de Grafana para ver los resultados.|
|OPR600: Supervisión de la infraestructura (Kubernetes)|Use este cuaderno para supervisar la infraestructura.|
|OPR700: Creación de un panel de Grafana para aplicaciones App-Deploy|Use este cuaderno para crear un panel en Grafana para supervisar los resultados de App-Deploy y generar alertas si se produce un error en una aplicación o en un valor controlado.|
|OPR900: Solución de problemas de ejecución de App-Deploy|Use este cuaderno para ejecutar el script de App-Deploy directamente en el contenedor (mediante kubectl exec). Esto puede proporcionar más información de depuración para la solución de problemas, especialmente en relación con problemas en la fase de inicio del script.|
|OPR901: Solución de problemas de cronjob|Use este cuaderno para ejecutar el script de cronjob directamente en el contenedor (mediante kubectl exec). Esto puede proporcionar más información de depuración para solucionar problemas.|


## <a name="analyze-logs-from-big-data-clusters-bdc"></a>Análisis de registros de Clústeres de macrodatos (BDC)

Conjunto de cuadernos de ejemplo en los que se muestran escenarios del clúster de macrodatos de SQL Server.

|Nombre |Descripción |
|---|---|
|SAM001a: Consulta de bloque de almacenamiento desde el grupo maestro de SQL Server (1 de 3): carga de datos de ejemplo|En este tutorial de tres partes, cargue datos en el bloque de almacenamiento (HDFS) usando azdata, conviértalo en Parquet (mediante Spark) y, en la tercera parte, consulte los datos usando el grupo maestro (SQL Server). |
|SAM001b: Consulta de bloque de almacenamiento desde el grupo maestro de SQL Server (2 de 3): conversión de datos en Parquet|En esta segunda parte de un tutorial de tres partes, use Spark para convertir un archivo. csv en un archivo parquet.|
|SAM001b: Consulta de bloque de almacenamiento desde el grupo maestro de SQL Server (3 de 3): consulta de HDFS desde SQL Server|En esta tercera parte del tutorial del bloque de almacenamiento, aprenderá a crear una tabla externa que apunte a los datos de HDFS en un clúster de macrodatos y a combinar estos datos con datos de gran valor en la instancia maestra.|
|SAM002: Bloque de almacenamiento (2 de 2) - consulta de HDFS|En esta segunda parte del tutorial del bloque de almacenamiento, aprenderá a crear una tabla externa que apunte a los datos de HDFS en un clúster de macrodatos y a combinar estos datos con datos de gran valor en la instancia maestra.|
|SAM003: ejemplo de grupo de datos|En este tutorial, obtendrá información sobre cómo crear un origen de grupo de datos y una tabla externa en el grupo de datos y, a continuación, insertar datos en tablas del grupo de datos y cargar datos de una tabla de grupos de datos a otra. Combine datos de la tabla de grupos de datos con otras tablas de grupos de datos, truncando además tablas y limpieza. |
|SAM004: virtualización de datos de MongoDB|Para consultar los datos de un origen de datos externos de MongoDB, debe crear tablas externas que hagan referencia a esos datos externos. En esta sección se proporciona código de ejemplo para crear estas tablas externas.|
|SAM005: virtualización de datos de Oracle|Para consultar los datos de un origen de datos externos de Oracle, debe crear tablas externas que hagan referencia a esos datos externos. En esta sección se proporciona código de ejemplo para crear estas tablas externas.|
|SAM006: virtualización de datos de SQL Server|Para consultar datos virtualmente de otro origen de datos de SQL Server, debe crear tablas externas que hagan referencia a esos datos externos. En esta sección se proporciona código de ejemplo para crear estas tablas externas.|
|SAM007: virtualización de datos de Teradata|Para consultar los datos de un origen de datos externos de Teradata, debe crear tablas externas que hagan referencia a esos datos externos. En esta sección se proporciona código de ejemplo para crear estas tablas externas.|
|SAM008: Spark con azdata|Comandos azdata y kubectl para trabajar con sesiones de Spark.|
|SAM009: HDFS con azdata|Comandos azdata y kubectl para trabajar con HDFS.|
|SAM010: aplicación con azdata|Comandos azdata y kubectl para trabajar con App-Deploy. |

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].
