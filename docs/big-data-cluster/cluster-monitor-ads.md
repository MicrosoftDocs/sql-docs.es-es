---
title: Supervisión de clústeres con Azure Data Studio
titleSuffix: SQL Server Big Data Clusters
description: Supervisión de clústeres con Azure Data Studio en una instancia de Clústeres de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d03dc9c26f33fd512e035ab3cc9e167e1d9ef7cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047745"
---
# <a name="monitor-cluster-status-with-azure-data-studio"></a>Supervisión del estado de un clúster con Azure Data Studio

En este artículo se explica cómo ver el estado de un clúster de macrodatos mediante Azure Data Studio.

## <a name="use-azure-data-studio"></a><a id="datastudio"></a> Usar Azure Data Studio

Después de descargar la versión más reciente de la **compilación para los participantes del programa Insider** de [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md), podrá ver los puntos de conexión del servicio y el estado de un clúster de macrodatos con el panel del clúster de macrodatos de SQL Server. Algunas de las siguientes características solo estarán disponibles por primera vez en la compilación de los participantes del programa Insider de Azure Data Studio.

1. Primero, cree una conexión al clúster de macrodatos en Azure Data Studio. Para obtener más información, vea [Conectarse a un clúster de macrodatos de SQL Server con Azure Data Studio](connect-to-big-data-cluster.md).

1. Haga clic con el botón derecho en el punto de conexión del clúster de macrodatos y, después, seleccione **Administrar**.

   ![botón derecho y administrar](media/view-cluster-status/right-click-manage.png)

1. Seleccione la pestaña **Clúster de macrodatos de SQL Server** para acceder al panel del clúster de macrodatos.

   ![Panel del clúster de macrodatos](media/view-cluster-status/bdc-dashboard.png)

### <a name="service-endpoints"></a>Puntos de conexión del servicio

Es importante que se pueda acceder fácilmente a distintos servicios de un clúster de macrodatos. En el panel del clúster de macrodatos, se proporciona una tabla de puntos de conexión del servicio que le permite ver y copiar los puntos de conexión del servicio.

![puntos de conexión del servicio](media/view-cluster-status/service-endpoints.png)

En estos servicios, se muestran los puntos de conexión que se pueden copiar y pegar si necesita usar el punto de conexión para conectarse a esos servicios. Por ejemplo, puede hacer clic en el icono de copiar a la derecha del punto de conexión y, después, pegarlo en una ventana de texto en la que se solicite este punto de conexión. El punto de conexión del servicio de administración de clústeres es necesario para ejecutar el [cuaderno de estado del clúster](#notebook).

### <a name="dashboards"></a>Paneles

En la tabla de puntos de conexión del servicio, también se muestran varios paneles de supervisión:

- Métricas (Grafana)
- Registros (Kibana)
- Supervisión de trabajos de Spark
- Administración de recursos de Spark

Puede hacer clic directamente de estos vínculos. Se le pedirá que se autentique al acceder a estos paneles. En el caso de los paneles de métricas y registros, indique las credenciales de administrador del controlador que estableció en el momento de la implementación mediante las variables de entorno **AZDATA_USERNAME** y **AZDATA_PASSWORD**. Los paneles de Spark usarán credenciales de puerta de enlace (Knox), ya sea la identidad de AD en un clúster integrado con AD o **AZDATA_USERNAME** y **AZDATA_PASSWORD**, si se usa la autenticación básica en el clúster.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

### <a name="cluster-status-notebook"></a><a id="notebook"></a> Cuaderno de estado del clúster

1. También puede ver el estado del clúster de macrodatos si inicia el cuaderno de estado del clúster. Para iniciar el cuaderno, haga clic en la tarea **Estado del clúster**.

    ![iniciar](media/view-cluster-status/cluster-status-launch.png)

2. Antes de empezar, necesitará los elementos siguientes:

    - Nombre del clúster de macrodatos
    - Nombre de usuario del controlador
    - Contraseña del controlador
    - Puntos de conexión del controlador

    El nombre del clúster de macrodatos predeterminado es **mssql-cluster**, excepto si lo personaliza durante la implementación. Encontrará el punto de conexión del controlador del panel del clúster de macrodatos en la tabla de puntos de conexión del servicio. El punto de conexión se muestra como **Servicio de administración de clústeres**. Si no conoce las credenciales, solicítelas al administrador que implementó el clúster.

3. En la barra de herramientas superior, haga clic en **Ejecutar celdas**.

4. Verá una solicitud de credenciales. Presione ENTRAR después de escribir cada credencial para el nombre del clúster de macrodatos, el nombre de usuario del controlador y la contraseña del controlador.

    > [!Note]
    > Si no tiene un archivo de configuración preparado con los macrodatos, se le pedirá que especifique el punto de conexión del controlador. Escríbalo o péguelo y, después, presione ENTRAR para continuar.

5. Si la conexión se establece correctamente, en el resto del cuaderno se mostrará el resultado de cada componente del clúster de macrodatos. Para volver a ejecutar una determinada celda de código, mantenga el puntero sobre la celda de código y haga clic en el icono de **Ejecutar**.


## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].