---
title: Contenedores de Clústeres de macrodatos no raíz
titleSuffix: SQL Server big data clusters
description: En este artículo se describe cómo implementar contenedores no raíz en Clústeres de macrodatos de SQL Server
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 52a24c1dbe2a239450b440c07de1a7852f9c627a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046045"
---
# <a name="non-root-big-data-clusters-containers"></a>Contenedores de Clústeres de macrodatos no raíz

SQL Server 2019 CU5 incorpora compatibilidad para contenedores no raíz. La implementación de la plataforma es más segura garantizando que todas las aplicaciones de contenedor que se ejecutan en los BDC se inician como usuarios no raíz de forma predeterminada en todas las plataformas admitidas. Estas capacidades están disponibles para todas las implementaciones nuevas mediante la etiqueta de imagen correspondiente de SQL Server 2019 CU5. Este cambio no afectará a las implementaciones existentes del BDC anteriores a CU5 y las aplicaciones de estos clústeres seguirán ejecutándose como usuario raíz. 

## <a name="technical-background"></a>Información técnica previa

Revise [estas notas técnicas del producto](https://aka.ms/sql-bdc-openshift-security) que captura los detalles del diseño de seguridad para adaptar las implementaciones que usan usuarios no raíz, resaltando qué y por qué los Clústeres de macrodatos elevan los permisos temporalmente en ciertos casos. El contenido de las notas del producto se ha desarrollado en colaboración con expertos en seguridad de SQL Server y Red Hat, y se centra en los contextos de seguridad y las capacidades de OpenShift, pero el diseño y los conceptos de seguridad del BDC se aplican a todas las plataformas compatibles.

> [!NOTE]
> En el momento del lanzamiento de la versión CU5, el paso de instalación de las aplicaciones implementadas con las interfaces de [implementación de la aplicación](concept-application-deployment.md) se seguirá ejecutando como usuario *raíz*. Esto es necesario, ya que durante la configuración se instalan paquetes adicionales que usará la aplicación. Otro código de usuario implementado como parte de la aplicación se ejecutará como usuario con pocos privilegios. 

> [!NOTE]
> Se recomienda que el clúster se ejecute con la configuración no raíz predeterminada. En caso de que quiera revertir al comportamiento anterior a CU5 y que los contenedores del BDC se ejecuten como usuario `root`, puede usar el nuevo modificador de características `allowRunAsRoot` y desactivar el comportamiento predeterminado. Esto solo se puede establecer en el momento de la implementación. Para establecer esto, especifique el valor en la sección `security` del archivo de configuración de implementación de `control.json`:

```json
 "security": {
  …
    "allowRunAsRoot": true,
  …
}
```

> [!IMPORTANT]
> No se admite el cambio de esta configuración en entornos de OpenShift.

Como consecuencia de que los servicios en el BDC se ejecuten como usuarios no raíz, las credenciales usadas para conectarse a los servicios a través del punto de conexión de puerta de enlace no usan el nombre de usuario `root`. Si la aplicación que se usa para conectarse al punto de conexión de puerta de enlace usa las credenciales incorrectas, verá un error de autenticación. El nombre de usuario nuevo del punto de conexión de puerta de enlace se basa en el valor que se pasa a través de la variable de entorno `AZDATA_USERNAME`. Es el mismo nombre de usuario que se usa para los puntos de conexión del controlador y SQL Server. Esto solo afecta a las implementaciones nuevas; en los clústeres de macrodatos existentes implementados con cualquiera de las versiones anteriores se seguirá usando `root`. No se produce ningún impacto en las credenciales cuando el clúster se implementa para usar la autenticación de Active Directory. 

## <a name="use-the-latest-azure-data-studio"></a>Uso de la versión más reciente de Azure Data Studio

Azure Data Studio controla de forma transparente el cambio de credenciales para la conexión realizada a la puerta de enlace a fin de habilitar la experiencia de exploración de HDFS en el Explorador de objetos o enviar trabajos de Spark a través de cuadernos. Instale la [versión más reciente de la compilación Azure Data Studio Insiders](../azure-data-studio/download-azure-data-studio.md#download-insiders-build-of-azure-data-studio). Esta compilación incluye los cambios necesarios para este caso de uso.

Para otros escenarios en los que debe proporcionar credenciales para acceder al servicio a través de la puerta de enlace (por ejemplo, el inicio de sesión con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], el acceso a los paneles web de Spark), tendrá que asegurarse de que se usen las credenciales correctas. Si el destino es un clúster existente implementado antes de CU5, seguirá usando el nombre de usuario `root` para conectarse a la puerta de enlace, incluso después de actualizar el clúster a CU5. Si implementa un nuevo clúster mediante la compilación CU5, iniciará sesión con el nombre de usuario correspondiente a la variable de entorno `AZDATA_USERNAME`.

## <a name="configuration-file-switches"></a>Modificadores del archivo de configuración

A partir de CU5, se han agregado dos nuevos modificadores de características para controlar la recopilación de métricas de pods y nodos. En caso de que use otras soluciones para la supervisión de la infraestructura de Kubernetes, puede desactivar la recopilación de métricas integradas para pods y nodos de host si establece `allowNodeMetricsCollection` y `allowPodMetricsCollection`* en `false` en el archivo de configuración de implementación `control.json`. 

Por ejemplo 

```json
"security": {
  ...
  "allowPodMetricsCollection": true,
  "allowNodeMetricsCollection": true,
  ...
}
```

> [!NOTE]
> En el caso de los entornos OpenShift, esta configuración se establece en false de forma predeterminada en los perfiles de implementación integrados. La recopilación de las métricas de pods y nodos ha requerido capacidades con privilegios y el contexto de seguridad recomendado para OpenShift se basa en las restricciones *restringidas*.

Además de los contenedores sin privilegios, a partir de CU5, para todas las implementaciones nuevas del BDC, los contenedores se ejecutan como un usuario no raíz de forma predeterminada en todas las plataformas admitidas. Estas capacidades están disponibles para todas las implementaciones nuevas mediante la etiqueta de imagen correspondiente de SQL Server 2019 CU5. Esto no afectará a las implementaciones existentes del BDC anteriores a CU5 y las aplicaciones de estos clústeres seguirán ejecutándose como usuario raíz.

## <a name="next-steps"></a>Pasos siguientes
[Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md)

[Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en OpenShift](deploy-openshift.md)

[Notas del producto de seguridad](https://aka.ms/sql-bdc-openshift-security)
