---
title: Recursos de administración de Clústeres de macrodatos (BDC)
titleSuffix: SQL Server
description: En este artículo, se explica cómo ver el estado de un clúster de macrodatos mediante Azure Data Studio, cuadernos y comandos de la CLI de datos de Azure (azdata).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 10/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 194f74fa175ad3f1c08ceab542b6db7812987157
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045686"
---
# <a name="administration-resources-for-big-data-clusters-bdc"></a>Recursos de administración de Clústeres de macrodatos (BDC) 

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describe cómo administrar un Clúster de macrodatos de SQL Server desde afuera hacia adentro.

## <a name="know-your-architecture"></a>Conozca su arquitectura

A partir de SQL Server 2019 (15.x), los Clústeres de macrodatos de SQL Server permiten implementar clústeres escalables de contenedores de SQL Server, Spark y HDFS que se ejecutan en Kubernetes. En los siguientes artículos se ofrece información general sobre el clúster de macrodatos:
- [¿Qué son los Clústeres de macrodatos de SQL Server?](big-data-cluster-overview.md)

Los Clústeres de macrodatos de SQL Server proporcionan una autorización y una autenticación coherentes. En los siguientes artículos se ofrece información general sobre la seguridad del clúster de macrodatos:
- [Conceptos de seguridad para los Clústeres de macrodatos de SQL Server](concept-security.md)

## <a name="manage-and-operate-with-tools"></a>Administración y trabajo con herramientas

En los artículos siguientes se describe cómo administrar y trabajar con el Clúster de macrodatos de las siguientes maneras: 

- [Conexión a un clúster de macrodatos de SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)
- [Administración de clústeres de macrodatos en el panel del controlador de SQL Server](manage-with-controller-dashboard.md)
- [Administración de Clústeres de macrodatos de SQL Server con cuadernos de Azure Data Studio](notebooks-manage-bdc.md)
- [Administración de Clústeres de macrodatos (BDC) con cuadernos](cluster-manage-notebooks.md)
- [Ejecución de un cuaderno de ejemplo con Spark](notebooks-tutorial-spark.md)

## <a name="monitor-with-tools"></a>Supervisión con herramientas

En los artículos siguientes se describe cómo administrar el Clúster de macrodatos de las siguientes maneras: 

- [Supervisión de un clúster de BDC con Azure Data Studio](cluster-monitor-ads.md)
- [Supervisión de un clúster de BDC con la utilidad Azdata](cluster-monitor-cmdlet.md)
- [Supervisión de un clúster de BDC con el panel de Grafana](cluster-monitor-grafana.md)
- [Supervisión de un clúster de BDC con cuadernos de Juypter y Azure Data Studio](cluster-monitor-notebooks.md)

## <a name="monitor-and-inspect-logs-with-notebooks"></a>Supervisión e inspección de registros con cuadernos

En los artículos siguientes se enumeran muchos de los cuadernos de Jupyter que están disponibles en Azure Data Studio.

- [Supervisión de clúster con cuadernos](cluster-monitor-notebooks.md)
- [Recopilación y análisis de registros en el clúster con cuadernos](cluster-logging-notebooks.md)

## <a name="big-data-clusters-troubleshooting-resources"></a>Recursos para la solución de problemas de Clústeres de macrodatos

En los artículos siguientes se describe cómo solucionar problemas del Clúster de macrodatos:

- [Solución de problemas del clúster de BDC con la utilidad kubectl](cluster-troubleshooting-commands.md) 
- [Solución de problemas de la libreta de Pyspark](troubleshoot-pyspark-notebook.md)
- [Solución de problemas del clúster de BDC con cuadernos de Juypter y Azure Data Studio (ADS)](cluster-troubleshooter-notebooks.md)
- [Restauración de permisos de HDFS](troubleshoot-hdfs-restore-admin.md)

En los artículos siguientes se describe cómo solucionar problemas del Clúster de macrodatos implementado en modo de Active Directory:
- [Solución de problemas del clúster de BDC en modo de Active Directory](troubleshoot-active-directory.md) 
- [Solución de problemas de inicio de sesión en modo AD](troubleshoot-ad-login-failed-untrusted-domain.md)
- [Solución de problemas de implementación de modo AD de BDC detenida](troubleshoot-ad-reverse-lookup-zone.md)

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md) para obtener más información sobre los clústeres de macrodatos.