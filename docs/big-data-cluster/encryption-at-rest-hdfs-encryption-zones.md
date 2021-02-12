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
ms.openlocfilehash: bfcc348595a645eca3b1150ae39e7c777f4e192a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046775"
---
# <a name="sql-server-big-data-clusters-hdfs-encryption-zones-usage-guide"></a>Guía de uso de las zonas de cifrado de HDFS de Clústeres de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En esta guía se muestra cómo usar las funcionalidades de cifrado en reposo de Clústeres de macrodatos de SQL Server para cifrar carpetas de HDFS mediante las zonas de cifrado.

Tenga en cuenta que ya hay una zona de cifrado predeterminada montada en __```/securelake```__ que está lista para usar. Se creó con una clave de 256 bits generada por el sistema denominada __securelakekey__. Esta clave se puede usar para crear zonas de cifrado adicionales.

## <a name="prerequisites"></a><a id="prereqs"></a> Requisitos previos

- [Clústeres de macrodatos de SQL Server CU8+](release-notes-big-data-cluster.md) con integración de Active Directory.
- Usuario con privilegios de administración.

## <a name="login-into-the-name-node"></a>Inicio de sesión en el nodo de nombre

Siga las [instrucciones de conexión de Active Directory](active-directory-connect.md) para iniciar sesión en el clúster. Inicie sesión en el NameNode (nmnode-0-0) para emitir comandos de zonas de cifrado y claves.

   ```console
   kubectl exec -it -c hadoop -n <cluster_namespace> nmnode-0-0 -- /bin/bash
   ```

## <a name="create-an-encryption-zone-using-the-provided-system-managed-key"></a>Creación de una zona de cifrado con la clave administrada por el sistema proporcionada

1. Creación de una carpeta de HDFS

   ```console
   hdfs dfs -mkdir -p /user/zone/folder
   ```

1. Emita el comando de creación de zonas de cifrado para cifrar la carpeta mediante la clave __securelakekey__.

   ```console
   hdfs crypto -createZone -keyName securelakekey -path /user/zone/folder
   ```

## <a name="create-a-custom-new-key-and-encryption-zone"></a>Creación de una zona de cifrado y una clave

1. Use el patrón siguiente para crear una clave de 256 bits.

   ```console
   kinit hdfs
   hadoop key create mydatalakekey -size 256
   ```

1. Cree y cifre una ruta de acceso de HDFS nueva con la clave del usuario.

   ```console
   hdfs dfs -mkdir -p /user/mydatalake
   hdfs crypto -createZone -keyName mydatalakekey -path /user/mydatalake
   ```
