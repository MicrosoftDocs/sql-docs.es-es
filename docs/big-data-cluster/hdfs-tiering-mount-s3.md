---
title: Montaje de S3 para los niveles de HDFS
titleSuffix: SQL Server big data clusters
description: En este artículo se explica cómo configurar los niveles de HDFS para montar un sistema de archivos S3 externo en HDFS en un clúster de macrodatos de SQL Server 2019.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6fce2d87f618fec23f204d14cd813beea4ac74bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046515"
---
# <a name="how-to-mount-s3-for-hdfs-tiering-in-a-big-data-cluster"></a>Cómo montar S3 para los niveles de HDFS en un clúster de macrodatos

En las secciones siguientes se proporciona un ejemplo de cómo configurar los niveles de HDFS con un origen de datos de almacenamiento S3.

## <a name="prerequisites"></a>Prerrequisitos

- [Clúster de macrodatos implementado](deployment-guidance.md)
- [Herramientas de macrodatos](deploy-big-data-tools.md)
  - **azdata**
  - **kubectl**
- Crear y cargar datos en un cubo S3 
  - Cargue los archivos CSV o Parquet en el cubo S3. Estos son los datos de HDFS externos que se montarán en HDFS en el clúster de macrodatos.

## <a name="access-keys"></a>Claves de acceso

### <a name="set-environment-variable-for-access-key-credentials"></a>Establecimiento de la variable de entorno para las credenciales de clave de acceso

Abra un símbolo del sistema en un equipo cliente que pueda acceder al clúster de macrodatos. Establezca una variable de entorno con el siguiente formato. Tenga en cuenta que las credenciales deben estar en una lista separada por comas. El comando "set" se usa en Windows. Si usa Linux, use "Export" en su lugar.

   ```text
    set MOUNT_CREDENTIALS=fs.s3a.access.key=<Access Key ID of the key>,
    fs.s3a.secret.key=<Secret Access Key of the key>
   ```

   > [!TIP]
   > Para obtener más información sobre cómo crear teclas de acceso S3, consulte [Claves de acceso S3](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys).

## <a name="mount-the-remote-hdfs-storage"></a><a id="mount"></a> Montaje del almacenamiento de HDFS remoto

Ahora que ha preparado un archivo de credenciales con claves de acceso, puede iniciar el montaje. En los pasos siguientes se monta el almacenamiento de HDFS remoto en S3 en el almacenamiento de HDFS local del clúster de macrodatos.

1. Use **kubectl** para buscar la dirección IP del servicio **controller-svc-external** de punto de conexión en el clúster de macrodatos. Busque el valor de **External-IP**.

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

1. Inicie sesión con **azdata** mediante la dirección IP externa del punto de conexión del controlador con el nombre de usuario y la contraseña del clúster:

   ```bash
   azdata login -e https://<IP-of-controller-svc-external>:30080/
   ```
   
1. Establezca la variable de entorno MOUNT_CREDENTIALS siguiendo las instrucciones anteriores.

1. Monte el almacenamiento de HDFS remoto en Azure con el comando **azdata bdc hdfs mount create**. Reemplace los valores de marcador de posición antes de ejecutar el siguiente comando:

   ```bash
   azdata bdc hdfs mount create --remote-uri s3a://<S3 bucket name> --mount-path /mounts/<mount-name>
   ```

   > [!NOTE]
   > El comando mount create es asincrónico. En este momento, no hay ningún mensaje que indique si el montaje se ha realizado correctamente. Consulte la sección sobre el [estado](#status) para comprobar el estado de los montajes.

Si se ha montado correctamente, debería poder consultar los datos de HDFS y ejecutar trabajos de Spark en ellos. Aparecerán en el HDFS del clúster de macrodatos en la ubicación que especifique `--mount-path`.

## <a name="get-the-status-of-mounts"></a><a id="status"></a> Obtención del estado de los montajes

Para mostrar el estado de todos los montajes en el clúster de macrodatos, use el siguiente comando:

```bash
azdata bdc hdfs mount status
```

Para mostrar el estado de un montaje en una ruta de acceso específica de HDFS, use el siguiente comando:

```bash
azdata bdc hdfs mount status --mount-path <mount-path-in-hdfs>
```

## <a name="refresh-a-mount"></a>Actualización de un montaje

En el siguiente ejemplo se actualiza el montaje.

```bash
azdata bdc hdfs mount refresh --mount-path <mount-path-in-hdfs>
```

## <a name="delete-the-mount"></a><a id="delete"></a> Eliminación del montaje

Para eliminar el montaje, use el comando **azdata bdc hdfs mount delete** y especifique la ruta de acceso de montaje en HDFS:

```bash
azdata bdc hdfs mount delete --mount-path <mount-path-in-hdfs>
```

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].
