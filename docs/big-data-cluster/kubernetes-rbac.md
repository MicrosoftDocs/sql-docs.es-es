---
title: RBAC de Kubernetes
titleSuffix: SQL Server big data clusters
description: En este artículo se describe cómo los Clústeres de macrodatos de SQL Server usan RBAC con Kubernetes.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: f8759714fe2846a86f53a4974960b511f4b4c3dd
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343893"
---
# <a name="kubernetes-rbac-model--impact-on-users-and-service-accounts-managing-bdc"></a>Modelo e impacto de RBAC de Kubernetes en los usuarios y las cuentas de servicio que administran BDC

En este artículo se describen los requisitos de permisos para los usuarios que administran clústeres de macrodatos y la semántica de la cuenta de servicio predeterminada y el acceso Kubernetes desde dentro del clúster de macrodatos.

> [!NOTE]
> Para obtener recursos adicionales sobre el modelo RBAC de Kubernetes, vea [Uso de la autorización de RBAC: Kubernetes](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) y [Uso de RBAC para definir y aplicar permisos: OpenShift](https://docs.openshift.com/container-platform/4.4/authentication/using-rbac.html).

## <a name="role-required-for-deployment"></a>Rol necesario para la implementación

El BDC usa cuentas de servicio (como `sa-mssql-controller` o `master`) para orquestar el aprovisionamiento de los pods, servicios, alta disponibilidad y supervisión del clúster, entre otras cosas. La implementación del BDC (por ejemplo, `azdata bdc create`), [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] hace lo siguiente al iniciarse:

1. Comprueba si existe el espacio de nombres proporcionado.
2. Si no existe, se crea uno y se aplica la etiqueta `MSSQL_CLUSTER`.
3. Crea la cuenta de servicio de `sa-mssql-controller`.
4. Crea un rol de `<namespaced>-admin` con permisos completos en el espacio de nombres o en el proyecto, pero no en los permisos de nivel de clúster.
5. Crea una asignación de roles de esa cuenta de servicio para el rol.

Una vez completados estos pasos, se aprovisionan los pods de plano de control y la cuenta de servicio implementa el resto del clúster de macrodatos.  

Por consiguiente, el usuario de implementación debe tener permisos para lo siguiente:

- Enumerar los espacios de nombres en el clúster (1).
- Aplicar una revisión al espacio de nombres nuevo o existente con la etiqueta (2).
- Crear la cuenta de servicio de `sa-mssql-controller`, el rol de `<namespaced>-admin` y el enlace de rol (3-5).

El rol de `admin` predeterminado no tiene estos permisos, por lo que el usuario que implementa el clúster de macrodatos debe tener al menos permisos de administrador de nivel de espacio de nombres.

## <a name="cluster-role-required-for-pods-and-nodes-metrics-collection"></a>Rol de clúster necesario para la recopilación de métricas de pods y nodos

A partir de SQL Server 2019 CU5, Telegraf requiere una cuenta de servicio con permisos de rol en todo el clúster para recopilar las métricas de pods y nodos. Durante la implementación (o la actualización de las implementaciones existentes), se intenta crear la cuenta de servicio y el rol de clúster necesarios, pero si el usuario que implementa el clúster o realiza la actualización no tiene permisos suficientes, la implementación o la actualización continuará con una advertencia y se realizará correctamente. En este caso, no se recopilarán las métricas de pods y nodos. El usuario que implementa el clúster debe pedirle a un administrador que cree el rol y la cuenta de servicio (antes o después de la implementación o actualización). Una vez creados, BDC los usa. 

Estos son los pasos para mostrar cómo crear los artefactos necesarios:

1. Cree un archivo *metrics-role.yaml* con el siguiente contenido. Asegúrese de reemplazar los marcadores de posición *<clusterName>* por el nombre del clúster de macrodatos.

   ```yaml
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRole
   metadata:
     name: <clusterName>:cr-mssql-metricsdc-reader
   rules:
   - apiGroups:
     - '*'
     resources:
     - pods
     - nodes/stats
     - nodes/proxy
     verbs:
     - get
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: <clusterName>:crb-mssql-metricsdc-reader
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: <clusterName>:cr-mssql-metricsdc-reader
   subjects:
   - kind: ServiceAccount
     name: sa-mssql-metricsdc-reader
     namespace: <clusterName>
   ```

2. Cree el rol y el enlace de rol de clúster:

   ```bash
   kubectl create -f metrics-role.yaml
   ```

La cuenta de servicio, el rol de clúster y el enlace de rol de clúster se pueden crear antes o después de implementar el BDC. Kubernetes actualiza automáticamente el permiso para la cuenta de servicio de Telegraf. Si se crean como una implementación de pod, verá un retraso de unos minutos en el pod y se recopilarán las métricas de nodos.

> [!NOTE]
> SQL Server 2019 CU5 presenta dos modificadores de características para controlar la recopilación de métricas de pods y nodos. De forma predeterminada, estos parámetros se establecen en true en todos los destinos de entorno, excepto OpenShift, donde está invalidado el valor predeterminado. 

Se puede personalizar esta configuración en la sección de seguridad del archivo de configuración de implementación de `control.json`:

```json
  "security": {
    …
    "allowNodeMetricsCollection": false,
    "allowPodMetricsCollection": false
  }
```

Si esta configuración se establece en `false`, el flujo de trabajo de implementación del BDC no intentará crear la cuenta de servicio, el rol de clúster y el enlace para Telegraf.

## <a name="default-service-account-usage-from-within-a-bdc-pod"></a>Uso de la cuenta de servicio predeterminada desde un pod de BDC

Para usar un modelo de seguridad más estricto, SQL Server 2019 CU5 deshabilitó el montaje de las credenciales predeterminadas de la cuenta de servicio Kubernetes predeterminada dentro de los pods de BDC. Esto se aplica a las implementaciones nuevas y actualizadas en CU5 o versiones posteriores.
El token de credenciales dentro de los pods se puede usar para acceder al servidor de API de Kubernetes y el nivel de permisos depende de la configuración de la directiva de autorización de Kubernetes. Si tiene casos de uso específicos que requieren revertir al comportamiento de CU5 anterior, en CU6 se está introduciendo un nuevo modificador de características para que pueda activar el montaje automático solo en el momento de la implementación. Puede hacerlo mediante el archivo de implementación de configuración control.json y estableciendo *automountServiceAccountToken* en *true*. Ejecute este comando para actualizar esta configuración en el archivo de configuración personalizado *control.json* mediante [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]: 

``` bash
azdata bdc config replace -c custom-bdc/control.json -j "$.security.automountServiceAccountToken=true"
```
