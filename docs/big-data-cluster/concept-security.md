---
title: Conceptos de seguridad
titleSuffix: SQL Server big data clusters
description: En este artículo se describen los conceptos relativos a la seguridad de los clústeres de macrodatos de SQL Server. Este contenido incluye la descripción de los puntos de conexión del clúster y la autenticación del clúster.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e4fbb1222168200d2107198091db7109ef6247ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039425"
---
# <a name="security-concepts-for-big-data-clusters-2019"></a>Conceptos de seguridad para [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se tratan los conceptos clave relacionados con la seguridad en los clústeres de macrodatos.

[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] proporciona una autorización y una autenticación coherentes. Un clúster de macrodatos se puede integrar con Active Directory (AD) a través de una implementación totalmente automatizada que configura la integración de Active Directory en un dominio existente. Una vez que un clúster de macrodatos se configura con la integración de AD, se pueden aprovechar las identidades y los grupos de usuarios existentes para el acceso unificado en todos los puntos de conexión. Además, una vez creadas las tablas externas en SQL Server, se puede controlar el acceso a los orígenes de datos mediante la concesión de acceso a tablas externas a los usuarios y grupos de AD, centralizando así las directivas de acceso a datos en una sola ubicación.

En este vídeo de 14 minutos obtendrá información general sobre la seguridad del clúster de macrodatos:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Big-Data-Cluster-Security/player?WT.mc_id=dataexposed-c9-niner]


## <a name="authentication"></a>Authentication

Los puntos de conexión de los clústeres externos admiten la autenticación de AD. Use su identidad de AD para autenticarse en el clúster de macrodatos.

### <a name="cluster-endpoints"></a>Puntos de conexión de clúster

Un clúster de macrodatos consta de cinco puntos de entrada.

* Instancia maestra: punto de conexión TDS para acceder a la Instancia maestra de SQL Server en el clúster mediante las herramientas de las bases de datos y aplicaciones como SSMS o Azure Data Studio. Al usar los comandos de HDFS o SQL Server de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], la herramienta se conecta al resto de puntos de conexión, en función de la operación.

* Puerta de enlace para acceder a archivos HDFS, Spark (Knox): punto de conexión HTTPS para acceder a servicios como webHDFS y Spark.

* Punto de conexión del Servicio de administración de clústeres (controlador): servicio de administración de clústeres de macrodatos que expone las API REST para administrar el clúster. La herramienta azdata requiere conectarse a este punto de conexión.

* Proxy de administración: para acceder al panel Búsqueda de registros y al panel Métricas.

* Proxy de aplicación: punto de conexión para administrar las aplicaciones implementadas en el clúster de macrodatos.

![Puntos de conexión de clúster](media/concept-security/cluster_endpoints.png)

Actualmente, no hay ninguna opción para abrir puertos adicionales para acceder al clúster desde el exterior.

## <a name="authorization"></a>Authorization

En todo el clúster, al emitir consultas de Spark y SQL Server, la seguridad integrada entre los distintos componentes permite pasar la identidad del usuario original a HDFS. Tal como se ha mencionado anteriormente, los distintos puntos de conexión externos del clúster admiten la autenticación de AD.

Hay dos niveles de comprobaciones de la autorización en el clúster para administrar el acceso a los datos. La autorización en el contexto de los macrodatos se realiza en SQL Server mediante los permisos convencionales de SQL Server en objetos, que asocian las identidades de los usuarios a permisos específicos. En HDFS, se efectúa con listas de control (ACL).

Un clúster de macrodatos seguro implica una compatibilidad consistente y coherente con los escenarios de autenticación y autorización, tanto en SQL Server como en HDFS/Spark. La autenticación es el proceso de comprobar la identidad de un usuario o servicio y asegurarse de que es quien dice ser. La autorización se refiere a la concesión o denegación de acceso a recursos específicos en función de la identidad del usuario que lo solicita. Este paso se realiza después de que un usuario se identifica a través de la autenticación.

La autorización en el contexto de macrodatos se realiza mediante listas de control de acceso (ACL), que asocian las identidades de usuario a permisos específicos. HDFS admite la autorización limitando el acceso a las API de servicio, los archivos HDFS y la ejecución de trabajos.

## <a name="encryption-in-flight-and-other-security-mechanisms"></a>Cifrado en movimiento y otros mecanismos de seguridad

El cifrado de la comunicación entre los clientes y los puntos de conexión externos, así como entre los componentes dentro del clúster, se protege con TLS/SSL, mediante certificados.

Todas las comunicaciones de SQL Server a SQL Server, como la de la instancia maestra SQL con un grupo de datos, se protegen mediante inicios de sesión SQL.

> [!IMPORTANT]
>  Los clústeres de macrodatos usan `etcd` para almacenar las credenciales. Como procedimiento recomendado, debe asegurarse de que el clúster de Kubernetes está configurado para usar el cifrado de `etcd` en reposo. De manera predeterminada, los secretos almacenados en `etcd` no están cifrados. En la documentación de Kubernetes se proporcionan detalles sobre esta tarea administrativa: https://kubernetes.io/docs/tasks/administer-cluster/kms-provider/ y https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/.

## <a name="data-encryption-at-rest"></a>Cifrado de datos en reposo

La funcionalidad de cifrado en reposo de Clústeres de macrodatos de SQL Server admite el escenario principal de cifrado de nivel de aplicación para los componentes de HDFS y SQL Server. Consulte el artículo [Conceptos y guía de configuración del cifrado en reposo](encryption-at-rest-concepts-and-configuration.md) para ver una guía de uso completa de las características.

> [!IMPORTANT]
> Se recomienda el cifrado de volumen para todas las implementaciones de Clústeres de macrodatos de SQL Server. También se deben cifrar los volúmenes de almacenamiento proporcionados por el cliente en los clústeres de Kubernetes, como un enfoque integral del cifrado de datos en reposo. La funcionalidad de cifrado en reposo de Clústeres de macrodatos de SQL Server es un nivel de seguridad adicional que proporciona el cifrado de nivel de aplicación de los archivos de registro y datos de SQL Server y la compatibilidad con las zonas de cifrado de HDFS.


## <a name="basic-administrator-login"></a>Inicio de sesión básico de administrador

Puede elegir implementar el clúster en el modo de AD, o bien usar solo el inicio de sesión básico de administrador. El uso exclusivo del inicio de sesión básico de administrador no representa un modo de seguridad admitido en el entorno de producción, y su finalidad es principalmente la evaluación del producto.

Incluso aunque elija el modo de Active Directory, se crearán inicios de sesión básicos para el administrador de clústeres. Esta característica proporciona acceso alternativo, en caso de que la conectividad de AD esté desactivada.

Tras la implementación, a este inicio de sesión básico se le concederán permisos de administrador en el clúster. Esto significa que el usuario de inicio de sesión será administrador del sistema en la Instancia maestra de SQL Server y administrador en el controlador de clústeres.
Los componentes de Hadoop no admiten la autenticación de modo mixto, lo que significa que no se puede usar un inicio de sesión básico de administrador para autenticarse en la puerta de enlace (Knox).

Las credenciales de inicio de sesión que debe definir durante la implementación incluyen lo siguiente:

Nombre de usuario del administrador del clúster:

 + `AZDATA_USERNAME=<username>`

Contraseña del administrador del clúster:  
 + `AZDATA_PASSWORD=<password>`

> [!NOTE]
> Tenga en cuenta que, en el modo que no es de AD, es necesario usar el nombre de usuario en combinación con la contraseña anterior a fin de autenticarse en la puerta de enlace (Knox) para el acceso a HDFS/Spark. Antes de SQL Server 2019 CU5, el nombre de usuario era `root`.
> 
> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

## <a name="next-steps"></a>Pasos siguientes

[¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)

[Taller: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)

[RBAC de Kubernetes](kubernetes-rbac.md)
