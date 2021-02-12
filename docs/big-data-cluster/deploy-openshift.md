---
title: Implementación en OpenShift
titleSuffix: SQL Server Big Data Cluster
description: Obtenga información sobre cómo actualizar Clústeres de macrodatos de SQL Server en OpenShift.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 5bcff7203d3bf9e224d6557c4c2af6a47fb0af12
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047285"
---
# <a name="deploy-big-data-clusters-2019-on-openshift-on-premises-and-azure-red-hat-openshift"></a>Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en el entorno local de OpenShift y Red Hat OpenShift en Azure

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se explica cómo implementar un Clúster de macrodatos (BDC) de SQL Server en entornos de OpenShift, en el entorno local o en Red Hat OpenShift en Azure (ARO).

> [!TIP]
> Si busca una forma rápida de arrancar un entorno de ejemplo mediante ARO y luego tener el BDC implementado en esta plataforma, puede usar el script de Python disponible [aquí](quickstart-big-data-cluster-deploy-aro.md).


SQL Server 2019 CU5 presenta compatibilidad con los Clústeres de macrodatos de SQL Server en OpenShift. Puede implementar clústeres de macrodatos en OpenShift en el entorno local o en Red Hat OpenShift en Azure (ARO). La implementación requiere que la versión del clúster de OpenShift sea como mínimo la 4.3. Aunque el flujo de trabajo de implementación es similar a la implementación en otras plataformas basadas en Kubernetes ([kubeadm](deploy-with-kubeadm.md) y [AKS](deploy-on-aks.md)), existen algunas diferencias. La diferencia que hay está principalmente relacionada con la ejecución de aplicaciones como un usuario no raíz y el contexto de seguridad usado para el espacio de nombres en el que se implementa el BDC.

Para implementar el clúster de OpenShift en el entorno local, vea la [Documentación de Red Hat OpenShift](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-installation-and-upgrade). Para las implementaciones de ARO, vea [Red Hat OpenShift en Azure](/azure/openshift/intro-openshift).

En este artículo se describen los pasos de implementación específicos de la plataforma OpenShift, se indican las opciones que tiene para acceder al entorno de destino y el espacio de nombres que se usa para implementar el clúster de macrodatos.

## <a name="pre-requisites"></a>Requisitos previos

> [!IMPORTANT]
> Los requisitos previos que se indican a continuación los debe realizar un administrador de clústeres de OpenShift (rol de clúster administrador de clústeres) que tenga permisos suficientes para crear estos objetos de nivel de clúster. Para obtener más información sobre los roles de clúster en OpenShift, vea [Usar RBAC para definir y aplicar permisos](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html).

1. Asegúrese de que el valor `pidsLimit` de OpenShift se actualiza para adaptarse a las cargas de trabajo de SQL Server. El valor predeterminado de OpenShift es demasiado bajo para cargas de trabajo similares a las de producción. Comience con al menos `4096`, aunque el valor óptimo depende del parámetro `max worker threads` de SQL Server y del número de procesadores de CPU en el nodo de host de OpenShift. 
    - A fin de averiguar cómo actualizar `pidsLimit` para el clúster de OpenShift, use [estas instrucciones]( https://github.com/openshift/machine-config-operator/blob/master/docs/ContainerRuntimeConfigDesign.md). Tenga en cuenta que las versiones de OpenShift anteriores a la `4.3.5` tuvieron un defecto que hacía que el valor actualizado no surtiera efecto. Asegúrese de actualizar OpenShift a la versión más reciente. 
    - Para ayudarle a calcular el valor óptimo en función de su entorno y de las cargas de trabajo de SQL Server previstas, puede usar la estimación y los ejemplos siguientes:

    |Número de procesadores|Máximo de subprocesos de trabajo predeterminados|Trabajos predeterminados por procesador|Valor mínimo de pidsLimit|
    |--------------------|--------------------------|-----------------------------|-----------------------|
    |          64        |           512            |             16              | 512 + (64 *16) = 1536 |
    |         128        |           512            |             32              | 512 + (128*32) = 4608 |

    > [!NOTE]
    > Otros procesos (por ejemplo, copias de seguridad, CLR, Fulltext y SQLAgent) también agregan cierta sobrecarga, por lo que debe agregar un búfer al valor estimado.

1. Descargue la restricción de contexto de seguridad personalizada (SCC) [`bdc-scc.yaml`](#bdc-sccyaml-file):

    ```console
    curl https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml -o bdc-scc.yaml
    ```

1. Aplique dicha restricción al clúster.

    ```console
    oc apply -f bdc-scc.yaml
    ```

    > [!NOTE]
    > La SCC personalizada para el BDC se basa en la SCC `nonroot` integrada en OpenShift, con permisos adicionales. Para obtener más información sobre las restricciones de contexto de seguridad en OpenShift, vea [Administrar restricciones de contexto de seguridad](https://docs.openshift.com/container-platform/4.3/authentication/managing-security-context-constraints.html). Para obtener información detallada sobre los permisos adicionales necesarios para los clústeres de macrodatos en la SCC `nonroot`, descargue las notas del producto [aquí](https://aka.ms/sql-bdc-openshift-security).

3. Creación de un espacio de nombres o proyecto:

   ```console
   oc new-project <namespaceName>
   ```

4. Asigne la SCC personalizada a las cuentas de servicio para los usuarios dentro del espacio de nombres en el que se implementa el BDC:

   ```console
   oc adm policy add-scc-to-group bdc-scc system:serviceaccounts:<namespaceName>
   ```

5. Asigne el permiso adecuado al usuario que implementa el BDC. Realice una de las acciones siguientes. 

   - Si el usuario que implementa el BDC tiene el rol de administrador de clústeres, continúe en [implementación del clúster de macrodatos](#deploy-big-data-cluster).

   - Si el usuario que implementa el BDC es un administrador de espacio de nombres, asigne el rol local de administrador de clústeres de usuario para el espacio de nombres creado. Esta es la opción preferida para que el usuario que implementa y administra el clúster de macrodatos tenga permisos de administrador de nivel de espacio de nombres.

   ```console
   oc adm policy add-role-to-user cluster-admin <deployingUser> -n <namespaceName>
   ```

   El usuario que implementa el clúster de macrodatos debe iniciar sesión en la consola de OpenShift:

   ```console
   oc login -u <deployingUser> -p <password>
   ```

## <a name="deploy-big-data-cluster"></a>Implementación de un clúster de macrodatos

1. Instalación de la versión más reciente de [azdata](../azdata/install/deploy-install-azdata.md).

1. Clone uno de los archivos de configuración integrados para OpenShift, en función del entorno de destino (OpenShift local o ARO) y del escenario de implementación. Vea más abajo la sección *Configuración específica de OpenShift en los archivos de configuración de implementación* para observar la configuración específica de OpenShift en los archivos de configuración integrados. Para obtener más detalles sobre los archivos de configuración disponibles, vea [guía de implementación](deployment-guidance.md).

   Enumere todos los archivos de configuración integrados disponibles.

   ```console
   azdata bdc config list
   ```

   Para clonar uno de los archivos de configuración integrados, ejecute el siguiente comando (opcionalmente, puede reemplazar el perfil según su plataforma o escenario de destino):

   ```console
   azdata bdc config init --source openshift-dev-test --target custom-openshift
   ```

   En el caso de una implementación de ARO, comience con uno de los perfiles `aro-`, que incluye los valores predeterminados para `serviceType` y `storageClass` adecuados para este entorno. Por ejemplo:

   ```console
   azdata bdc config init --source aro-dev-test --target custom-openshift
   ```

1. Personalice los archivos de configuración control.json y bdc.json. Estos son algunos recursos adicionales que le guiarán a través de las personalizaciones admitidas para varios casos de uso:

   - [Storage](concept-data-persistence.md)
   - [Configuración relacionada con AD](active-directory-deploy.md)
   - [Otras personalizaciones](deployment-custom-configuration.md)

   > [!NOTE]
   > No se admite la integración con Azure Active Directory para los BDC; por lo tanto, no se puede usar este método de autenticación cuando se implementa en ARO.

1. Establezca [variables de entorno](deployment-guidance.md#env).

1. Implemente un clúster de macrodatos.

   ```console
   azdata bdc create --config custom-openshift --accept-eula yes
   ```

1. Una vez realizada la implementación correctamente, puede iniciar sesión y enumerar los puntos de conexión del clúster externo:

   ```console
      azdata login -n mssql-cluster
      azdata bdc endpoint list
   ```

## <a name="openshift-specific-settings-in-the-deployment-configuration-files"></a>Configuración específica de OpenShift en los archivos de configuración de implementación

SQL Server 2019 CU5 presenta dos modificadores de características para controlar la recopilación de métricas de pods y nodos. Estos parámetros se establecen en `false` de forma predeterminada en los perfiles integrados para OpenShift, ya que los contenedores de supervisión requieren [contexto de seguridad con privilegios](https://www.openshift.com/blog/managing-sccs-in-openshift), lo que relajará algunas de las restricciones de seguridad para el espacio de nombres en el que se implementa el BDC.

```json
    "security": {
      "allowNodeMetricsCollection": false,
      "allowPodMetricsCollection": false
}
```

El nombre de la clase de almacenamiento predeterminada en ARO es Managed-Premium (en lugar de AKS, donde se llama a la clase de almacenamiento predeterminada se llama predeterminada). Lo encontraría en el elemento `control.json` correspondiente a `aro-dev-test` y `aro-dev-test-ha`:

```json
    },
    "storage": {
      "data": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "managed-premium",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
      }
```

## <a name="bdc-sccyaml-file"></a>Archivo `bdc-scc.yaml`

El archivo de la SCC para esta implementación es:

:::code language="yaml" source="../../sql-server-samples/samples/features/sql-big-data-cluster/deployment/openshift/bdc-scc.yaml":::

## <a name="next-steps"></a>Pasos siguientes

[Tutorial: Carga de datos de ejemplo en un clúster de macrodatos de SQL Server](tutorial-load-sample-data.md).
