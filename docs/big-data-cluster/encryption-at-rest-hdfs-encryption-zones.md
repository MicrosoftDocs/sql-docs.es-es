---
title: Guía de uso de las zonas de cifrado de HDFS de Clústeres de macrodatos de SQL Server
titleSuffix: SQL Server big data clusters
description: En este artículo se muestra cómo usar la característica Zonas de cifrado de HDFS de SQL Server del BDC.
author: DaniBunny
ms.author: dacoelho
ms.reviewer: mihaelab
ms.date: 10/19/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 4c56d065de396a282c97c0723f118e5911617544
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489299"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>Guía de uso de las zonas de cifrado de HDFS de Clústeres de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En esta guía se muestra cómo usar las funcionalidades de cifrado en reposo de Clústeres de macrodatos de SQL Server para cifrar carpetas de HDFS mediante las zonas de cifrado. También se tratan las tareas de administración de claves de HDFS.

Tenga en cuenta que ya hay una zona de cifrado predeterminada montada en __```/securelake```__ que está lista para su uso. Se creó con una clave de 256 bits generada por el sistema denominada __securelakekey__. Esta clave se puede usar para crear zonas de cifrado adicionales.

## <a name="prerequisites"></a><a id="prereqs"></a> Requisitos previos

- [Clústeres de macrodatos de SQL Server CU8 (y versiones posteriores)](release-notes-big-data-cluster.md) con integración de [Active Directory](active-directory-prerequisites.md).
- Usuario con privilegios de administración.
- [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] configurada y conectada en el clúster en el modo de AD.

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>Creación de una zona de cifrado con la clave administrada por el sistema proporcionada

1. Creación de una carpeta de HDFS

   ```console
   azdata bdc hdfs mkdir -p /user/zone/folder
   ```

1. Emita el comando de creación de zonas de cifrado para cifrar la carpeta mediante la clave __securelakekey__.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/zone/folder --keyname securelakekey
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>Creación de una zona de cifrado y una clave

1. Use el patrón siguiente para crear una clave de 256 bits.

   ```console
   azdata bdc hdfs key create -n mydatalakekey
   ```

1. Cree y cifre una ruta de acceso de HDFS nueva con la clave del usuario.

   ```console
   azdata bdc hdfs encryption-zone create --path /user/mydatalake --keyname mydatalakekey
   ```

## <a name="hdfs-key-rotation-and-encryption-zone-re-encryption"></a>Recifrado de la zona de cifrado y rotación de claves de HDFS

1. Esto crea una nueva versión de la clave __securelakekey__ con nuevo material de clave.

   ```console
   azdata hdfs bdc key roll -n securelakekey
   ```

1. Vuelva a cifrar la zona de cifrado asociada con la clave anterior.

   ```console
   azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
   ```

## <a name="hdfs-key-and-encryption-zone-monitoring"></a>Supervisión de la clave y la zona de cifrado de HDFS

1. Supervise el estado de un nuevo cifrado de la zona de cifrado. 

   ```console
   azdata bdc hdfs encryption-zone status
   ```

1. Obtenga la información de cifrado de un archivo en una zona de cifrado.

   ```console
   azdata bdc hdfs encryption-zone get-file-encryption-info --path /securelake/data.csv
   ```

1. Enumere todas las zonas de cifrado.

   ```console
   azdata bdc hdfs encryption-zone list
   ```

1. Enumere todas las claves disponibles para HDFS.

   ```console
   azdata bdc hdfs key list
   ```

1. Cree una clave personalizada para el cifrado de HDFS. Los tamaños posibles son 128, 192 y 256. El valor predeterminado es de 256.

   ```console
   azdata hdfs key create --name key1 --size 256
   ```

## <a name="next-steps"></a>Pasos siguientes

Use azdata con Clústeres de macrodatos, vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).

Use azdata con [servicios de datos habilitados para Azure Arc](/azure/azure-arc/data/).
