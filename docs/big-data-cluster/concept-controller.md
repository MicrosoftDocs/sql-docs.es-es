---
title: ¿Qué es el controlador?
titleSuffix: SQL Server big data clusters
description: En este artículo se describe el controlador de un clúster de macrodatos de SQL Server 2019.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9e2a7413578a8625231ffa0dad1750e98445f339
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039475"
---
# <a name="what-is-the-controller-on-a-sql-server-big-data-cluster"></a>¿Qué es el controlador de un clúster de macrodatos de SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

El controlador hospeda la lógica básica para implementar y administrar un clúster de macrodatos de SQL Server. Se encarga de todas las interacciones con Kubernetes, las instancias de SQL Server que forman parte del clúster y otros componentes como HDFS y Spark.

El servicio del controlador proporciona la siguiente funcionalidad básica:

- Administración del ciclo de vida del clúster: arranque y eliminación del clúster, configuraciones de actualización.
- Administración de las instancias maestras de SQL Server.
- Administración de procesos, datos y bloques de almacenamiento.
- Exposición de las herramientas de supervisión para observar el estado del clúster.
- Exposición de las herramientas de solución de problemas para detectar y reparar problemas inesperados.
- Administración de la seguridad del clúster:
  - Garantía de que los puntos de conexión son seguros.
  - Administración de los usuarios y los roles.
  - Configuración de las credenciales para la comunicación dentro del clúster.

## <a name="deploying-the-controller-service"></a>Implementación del controlador del servicio

El controlador se implementa y se hospeda en el mismo espacio de nombres de Kubernetes en el que el cliente quiere crear un clúster de macrodatos. Un administrador de Kubernetes instala este servicio durante el arranque del clúster mediante la utilidad de línea de comandos **azdata**. Para obtener más información, vea [Introducción a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deploy-get-started.md).

El flujo de trabajo de creación diseñará sobre Kubernetes un clúster de macrodatos de SQL Server totalmente funcional que incluye todos los componentes que se describen en el artículo de [introducción](big-data-cluster-overview.md). El flujo de trabajo de arranque crea primero el servicio del controlador y, una vez implementado, el servicio del controlador coordinará la instalación y la configuración del resto de los servicios que forman parte de los grupos maestro, de proceso, de datos y de almacenamiento.

## <a name="managing-the-cluster-through-the-controller-service"></a>Administración del clúster mediante el servicio del controlador

Puede administrar el clúster mediante el servicio del controlador con los comandos de **azdata**. Si implementa objetos de Kubernetes adicionales como pods en el mismo espacio de nombres, el servicio del controlador no los administra ni los supervisa. También puede usar los comandos de **kubectl** para administrar el clúster en el nivel de Kubernetes. Para obtener más información, vea [Supervisión y solución de problemas de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

El controlador y los objetos de Kubernetes (conjuntos con estado, pods, secretos, etc.) creados para un clúster de macrodatos residen en un espacio de nombres de Kubernetes dedicado. El administrador de clústeres de Kubernetes concederá permiso al servicio del controlador para administrar todos los recursos de ese espacio de nombres.  La directiva de RBAC de este escenario se configura automáticamente como parte de la implementación inicial del clúster mediante **azdata**.

### <a name="azdata"></a>azdata

**azdata** es una utilidad de línea de comandos escrita en Python que permite a los administradores de clústeres arrancar y administrar clústeres de macrodatos mediante las API REST que expone el servicio del controlador.

## <a name="controller-service-security"></a>Seguridad del servicio del controlador

Toda la comunicación con el servicio del controlador se realiza mediante una API REST a través de HTTPS. Se generará automáticamente un certificado autofirmado en el momento del arranque. 

La autenticación en el punto de conexión de servicio del controlador se realiza mediante una identidad de Active Directory o basada en un nombre de usuario y una contraseña. Estas credenciales se aprovisionan en el momento de arranque del clúster mediante la entrada de las variables de entorno `AZDATA_USERNAME` y `AZDATA_PASSWORD`.

> [!NOTE]
> Debe proporcionar una contraseña que cumpla los [requisitos de complejidad de las contraseñas de SQL Server](../relational-databases/security/password-policy.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea los recursos siguientes:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Taller: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/microsoft/sqlworkshops-bdc)