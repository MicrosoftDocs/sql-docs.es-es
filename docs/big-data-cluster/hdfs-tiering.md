---
title: Configuración de la organización en niveles de HDFS
titleSuffix: SQL Server big data clusters
description: En este artículo se explica cómo configurar la organización en niveles de HDFS para montar un sistema de archivos de Azure Data Lake Storage externo en HDFS en un clúster de macrodatos de SQL Server 2019.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3c95bb10dbd063c56029a01b5a828f80eb5ba4d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046715"
---
# <a name="configure-hdfs-tiering-on-big-data-clusters-2019"></a>Configuración de la organización en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

La organización en niveles de HDFS ofrece la posibilidad de montar un sistema de archivos externo compatible con HDFS en HDFS. En este artículo se explica cómo configurar la organización en niveles de HDFS en clústeres de macrodatos de SQL Server. En este momento, se admite la conexión con Azure Data Lake Storage Gen2 y Amazon S3. 

## <a name="hdfs-tiering-overview"></a>Introducción a la organización en niveles de HDFS

Con la organización en niveles, las aplicaciones pueden acceder sin problemas a los datos de diversos almacenes externos como si los datos residieran en el HDFS local. El montaje es una operación de metadatos, donde los metadatos que describen el espacio de nombres en el sistema de archivos externo se copian en el HDFS local. Estos metadatos incluyen información sobre los directorios y archivos externos con sus permisos y listas de control de acceso (ACL). Los datos correspondientes solo se copian a petición, cuando se accede a los propios datos a través de, por ejemplo, una consulta. Ahora se puede acceder a los datos externos del sistema de archivos desde el clúster de macrodatos de SQL Server. Puede ejecutar trabajos de Spark y consultas SQL en estos datos de la misma manera que los ejecutaría en los datos locales almacenados en HDFS en el clúster.

En este vídeo de 7 minutos se proporciona información general sobre la organización en niveles de HDFS:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Unify-your-data-lakes-with-HDFS-tiering/player?WT.mc_id=dataexposed-c9-niner]


### <a name="caching"></a>Almacenamiento en memoria caché
En la actualidad, de forma predeterminada, el 1 % del almacenamiento total de HDFS se reservará para el almacenamiento en caché de los datos montados. El almacenamiento en caché es una configuración global entre los montajes.

> [!NOTE]
> La organización en niveles de HDFS es una característica desarrollada por Microsoft y se ha publicado una versión anterior como parte de la distribución de Apache Hadoop 3.1. Para más información, consulte [https://issues.apache.org/jira/browse/HDFS-9806](https://issues.apache.org/jira/browse/HDFS-9806).

En las secciones siguientes se ofrece un ejemplo de cómo configurar la organización en niveles de HDFS con un origen de datos de Azure Data Lake Storage Gen2.

## <a name="refresh"></a>Actualizar

La organización en niveles de HDFS admite la actualización. Actualice un montaje existente para la instantánea más reciente de los datos remotos.

## <a name="prerequisites"></a>Prerrequisitos

- [Clúster de macrodatos implementado](deployment-guidance.md)
- [Herramientas de macrodatos](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**

## <a name="mounting-instructions"></a>Instrucciones de montaje

Se admite la conexión con Azure Data Lake Storage Gen2 y Amazon S3. Puede encontrar instrucciones sobre cómo montar en estos tipos de almacenamiento en los siguientes artículos:

- [Cómo montar ADLS Gen2 en niveles de HDFS en un clúster de macrodatos](hdfs-tiering-mount-adlsgen2.md)
- [Cómo montar S3 en niveles de HDFS en un clúster de macrodatos](hdfs-tiering-mount-s3.md)

## <a name="known-issues-and-limitations"></a><a id="issues"></a> Limitaciones y problemas conocidos

En la lista siguiente se indican los problemas conocidos y las limitaciones actuales al usar la organización en niveles de HDFS en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]:

- Si el montaje se queda en un estado `CREATING` durante mucho tiempo, lo más probable es que se haya producido un error. En esta situación, cancele el comando y, si es necesario, elimine el montaje. Compruebe que los parámetros y las credenciales sean correctos antes de volver a intentarlo.

- No se pueden crear montajes en directorios existentes.

- No se pueden crear montajes en montajes existentes.

- Si alguno de los antecesores del punto de montaje no existe, se creará con los permisos predeterminados para r-xr-xr-x (555).

- La creación de montajes puede tardar algún tiempo según el número y el tamaño de los archivos que se montan. Durante este proceso, los archivos del montaje no son visibles para los usuarios. Mientras se crea el montaje, todos los archivos se agregarán a una ruta de acceso temporal, cuyo valor predeterminado es `/_temporary/_mounts/<mount-location>`.

- El comando de creación de montaje es asincrónico. Después de ejecutar el comando, se puede comprobar el estado de montaje para conocerlo.

- Al crear el montaje, el argumento que se usa para **--mount-path** es esencialmente un identificador único del montaje. La misma cadena (incluida la "/" al final si está presente) debe usarse en los siguientes comandos.

- Los montajes son de solo lectura. No se pueden crear directorios ni archivos en un montaje.

- No se recomienda el montaje de directorios y archivos que puedan cambiar. Una vez creado el montaje, los cambios o las actualizaciones de la ubicación remota no se reflejarán en el montaje en HDFS. Si se producen cambios en la ubicación remota, puede optar por eliminar y volver a crear el montaje para que refleje el estado actualizado.

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].
