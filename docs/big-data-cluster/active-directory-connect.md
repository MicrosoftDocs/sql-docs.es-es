---
title: Conexión en el modo de Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Obtenga información sobre cómo conectar los clústeres de macrodatos de SQL Server en un dominio de Active Directory.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 809a91c59973bee1602f335ed37fbef5931bbfeb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039915"
---
# <a name="connect-big-data-clusters-2019-active-directory-mode"></a>Conexión de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]: Modo de Active Directory

En este artículo se describe cómo conectarse a SQL Server puntos de conexión de clúster de macrodatos implementados en modo de Active Directory. En las tareas de este artículo, es necesario tener un clúster de macrodatos de SQL Server implementado en el modo de Active Directory. Si no tiene un clúster, vea [Implementación [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en modo de Active Directory](active-directory-deploy.md).

## <a name="overview"></a>Información general

Inicie sesión en la instancia maestra de SQL Server con la autenticación de AD.

Para comprobar las conexiones de AD a la instancia de SQL Server, conéctese a la instancia maestra de SQL con `sqlcmd`. Los inicios de sesión para los grupos proporcionados se crean automáticamente tras la implementación (`clusterUsers` y `clusterAdmins`).

Si usa Linux, ejecute `kinit` primero como usuario de AD y, luego, `sqlcmd`. Si usa Windows, solo tiene que iniciar la sesión del usuario que prefiera desde una **máquina de cliente unida a un dominio**.

## <a name="connect-to-master-instance-from-linuxmac"></a>Conexión a una instancia maestra desde Linux/Mac

```bash
kinit <username>@<domain name>
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="connect-to-master-instance-from-windows"></a>Conexión a una instancia maestra desde Windows

```cmd
sqlcmd -S <DNS name for master instance>,31433 -E
```

## <a name="log-in-to-sql-server-master-instance-using-azure-data-studio-or-ssms"></a>Inicio de sesión en la instancia maestra de SQL Server con Azure Data Studio o SSMS

Desde un cliente unido a un dominio, se puede abrir SSMS o Azure Data Studio y conectarse a la instancia maestra. Esta es la misma experiencia que la conexión a cualquier instancia de SQL Server mediante la autenticación de AD.

Desde SSMS:

![Cuadro de diálogo Conectar a SQL Server en SSMS](./media/deploy-active-directory/image23.png)

Desde Azure Data Studio:

![Cuadro de diálogo Conectarse a SQL Server en Azure Data Studio](./media/deploy-active-directory/image24.png)}

## <a name="log-in-to-controller-with-ad-authentication"></a>Inicio de sesión en el controlador con la autenticación de AD

### <a name="connect-to-controller-with-ad-authentication-from-linuxmac"></a>Conexión al controlador con la autenticación de AD desde Linux/Mac

Hay dos opciones para conectarse al punto de conexión del controlador mediante [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] y la autenticación de AD. Puede usar el parámetro *--endpoint/-e*:

```bash
kinit <username>@<domain name>
azdata login -e https://<controller DNS name>:30080 --auth ad
```

También puede conectarse mediante el parámetro *--namespace/-n*, que es el nombre del clúster de macrodatos:

```bash
kinit <username>@<domain name>
azdata login -n <clusterName> --auth ad
```

### <a name="connect-to-controller-with-ad-authentication-from-windows"></a>Conexión al controlador con la autenticación de AD desde Windows

```cmd
azdata login -e https://<controller DNS name>:30080 --auth ad
```

## <a name="use-ad-authentication-to-knox-gateway-webhdfs"></a>Uso de la autenticación de AD en la puerta de enlace Knox (webHDFS)

También se pueden emitir comandos HDFS mediante el uso de curl a través del punto de conexión de la puerta de enlace Knox. Esto requiere la autenticación de AD en Knox. El siguiente comando curl emite una llamada REST de webHDFS a través de la puerta de enlace Knox para crear un directorio denominado `products`

```bash
curl -k -v --negotiate -u : https://<Gateway DNS name>:30443/gateway/default/webhdfs/v1/products?op=MKDIRS -X PUT
```

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas de integración de Active Directory de un Clúster de macrodatos de SQL Server](troubleshoot-active-directory.md)

[Concepto: Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en modo de Active Directory](active-directory-deployment-background.md)
