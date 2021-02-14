---
title: Conexión a clústeres maestros y HDFS de macrodatos
description: Obtenga información sobre cómo conectarse a la instancia maestra de SQL Server y a la puerta de enlace HDFS/Spark para un clúster de macrodatos de SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b7f05551da025f513cd69ddf592002126c6f05c3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039072"
---
# <a name="connect-to-a-sql-server-big-data-cluster-with-azure-data-studio"></a>Conexión a un clúster de macrodatos de SQL Server con Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se explica cómo conectarse a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] desde Azure Data Studio.

## <a name="prerequisites"></a>Prerrequisitos

- Tener implementado un [clúster de macrodatos de SQL Server 2019](deployment-guidance.md).
- [Herramientas de macrodatos de SQL Server 2019](deploy-big-data-tools.md):
   - **Azure Data Studio**
   - **Extensión de SQL Server 2019**
   - **kubectl**
   - **azdata**

## <a name="connect-to-the-cluster"></a><a id="master"></a> Conexión al clúster

Para conectarse a un clúster de macrodatos con Azure Data Studio, cree una nueva conexión a la instancia maestra de SQL Server del clúster. A continuación, se indica cómo puede hacerlo.

1. Busque el punto de conexión de la instancia maestra de SQL Server:

   ```
   azdata bdc endpoint list -e sql-server-master
   ```

   > [!TIP]
   > Para obtener más información sobre cómo recuperar puntos de conexión, vea [Recuperación de puntos de conexión](deployment-guidance.md#endpoints).

1. En Azure Data Studio, presione **F1** > **Nueva conexión**.

1. En **Tipo de conexión**, seleccione **Microsoft SQL Server**.

1. Escriba el nombre del punto de conexión que ha encontrado para la instancia maestra de SQL Server en el cuadro de texto **Nombre del servidor** (por ejemplo: **\<IP_Address\>,31433**). 

1. Seleccione el tipo de autenticación. En una instancia maestra de SQL Server que se ejecute en un clúster de macrodatos solo se admiten **Autenticación de Windows** e **Inicio de sesión de SQL**. 

1. Si utiliza el inicio de sesión de SQL, escriba el inicio de sesión de SQL **nombre de usuario** y **contraseña**.

   > [!TIP]
   > De forma predeterminada, el nombre de usuario **SA** está deshabilitado durante la implementación del clúster de macrodatos. Durante la implementación se aprovisiona un nuevo usuario sysadmin con el nombre y la contraseña correspondientes a las variables de entorno **AZDATA_USERNAME** y **AZDATA_PASSWORD**, que se establecieron antes o durante la implementación.

1. Cambie el **Nombre de la base de datos** de destino por uno de sus bases de datos relacionales.

   ![Conexión a la instancia maestra](./media/connect-to-big-data-cluster/connect-to-cluster.png)

1. Presione **Conectar **. Debería aparecer el** Panel del servidor**.

Con la versión de febrero de 2019 de Azure Data Studio, la conexión a la instancia maestra de SQL Server también le permite interactuar con la puerta de enlace HDFS/Spark. Esto significa que no es necesario usar la conexión independiente para HDFS y Spark que se describe en la siguiente sección.

- El Explorador de objetos contiene ahora un nuevo nodo **Data Services** con compatibilidad con las tareas del clúster de macrodatos que se activan con el botón derecho, como la creación de nuevos cuadernos o el envío de trabajos de Spark. 
- El nodo de **Data Services** también contiene una carpeta **HDFS** para que pueda explorar el contenido de HDFS y realizar tareas comunes que impliquen a HDFS (por ejemplo, crear una tabla externa o abrir un cuaderno para analizar el contenido de HDFS).
- El **Panel del servidor** de la conexión también contiene pestañas para el **clúster de macrodatos de SQL Server** y **SQL Server 2019** cuando la extensión está instalada.

   ![Nodo Data Services de Azure Data Studio](./media/connect-to-big-data-cluster/connect-data-services-node.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)?
