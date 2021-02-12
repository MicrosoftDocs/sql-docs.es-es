---
title: Implementación de un clúster de macrodatos en el clúster privado de Azure Kubernetes Service (AKS)
titleSuffix: SQL Server Big Data Cluster
description: Aprenda a implementar clústeres de macrodatos de SQL Server con el clúster privado de Azure Kubernetes Service (AKS) con redes avanzadas (CNI).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d9197e936f3df8f0a98db672f69ac05fce07ebd8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043885"
---
# <a name="deploy-bdc-in-azure-kubernetes-service-aks-private-cluster"></a>Implementación de un clúster de macrodatos en el clúster privado de Azure Kubernetes Service (AKS)

En este artículo se explica cómo implementar clústeres de macrodatos de SQL Server en el clúster privado de Azure Kubernetes Service (AKS). Esta configuración admite el uso restringido de direcciones IP públicas en el entorno de redes de empresa.

Una implementación privada proporciona las siguientes ventajas:

* Uso restringido de direcciones IP públicas.
* El tráfico de red entre los servidores de aplicaciones y los grupos de nodos de clúster se mantiene solo en la red privada.
* Configuración personalizada para los tráficos de salida obligatorios para ajustarse a requisitos específicos

En este artículo se muestra cómo usar un clúster privado de AKS para restringir el uso de la dirección IP pública cuando están aplicadas las cadenas de seguridad respectivas.

## <a name="deploy-private-bdc-cluster-with-aks-private-cluster"></a>Implementación de un clúster de macrodatos privado con un clúster privado de AKS

Para empezar, cree un [clúster privado de AKS](/azure/aks/private-clusters) para asegurarse de que el tráfico de red entre el servidor de API y los grupos de nodos permanece solo en la red privada. El plano de control o el servidor de API tiene direcciones IP internas en un clúster privado de AKS.

En esta sección se muestra cómo implementar un clúster de macrodatos en un clúster privado de Azure Kubernetes Service (AKS) con redes avanzadas (CNI).

## <a name="create-a-private-aks-cluster-with-advanced-networking"></a>Creación de un clúster de AKS privado con redes avanzadas

```bash

export REGION_NAME=<your Azure region >
export RESOURCE_GROUP=< your resource group name >
export SUBNET_NAME=aks-subnet
export VNET_NAME=bdc-vnet
export AKS_NAME=< your aks private cluster name >
 
az group create -n $RESOURCE_GROUP -l $REGION_NAME
 
az network vnet create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $VNET_NAME \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name $SUBNET_NAME \
    --subnet-prefix 10.1.0.0/16
 

SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
 
echo $SUBNET_ID
## will be displayed something similar as the following: 
/subscriptions/xxxx-xxxx-xxx-xxxx-xxxxxxxx/resourceGroups/your-bdc-rg/providers/Microsoft.Network/virtualNetworks/your-aks-vnet/subnets/your-aks-subnet
```

### <a name="create-aks-private-cluster-with-advanced-networking-cni"></a>Creación de un clúster privado de AKS con redes avanzadas (CNI)

Para avanzar al paso siguiente, debe aprovisionar un clúster de AKS con Standard Load Balancer con la característica de clúster privado habilitada. El comando tendrá el siguiente aspecto: 

```bash
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

Una vez implementado correctamente, puede ir al grupo de recursos `<MC_yourakscluster>` y encontrará que `kube-apiserver` es un punto de conexión privado. Por ejemplo, consulte la sección siguiente.

## <a name="connect-to-an-aks-cluster"></a>Conexión a un clúster de ACS

```azurecli
az aks get-credentials -n $AKS_NAME -g $RESOURCE_GROUP
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>Compilación del perfil de implementación del clúster de macrodatos

Después de conectarse a un clúster de AKS, puede empezar a implementar el clúster de macrodatos, y puede preparar la variable de entorno e iniciar una implementación: 

```azurecli
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

Generación y configuración de un perfil de implementación personalizado de clúster de macrodatos:

```azurecli
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-private-sql-server-big-data-cluster-with-ha"></a>Implementación de un clúster de macrodatos de SQL Server privado con alta disponibilidad

En caso de que esté [implementando un clúster de macrodatos de SQL Server (SQL-BDC) con alta disponibilidad](deployment-high-availability.md), estará usando el perfil de implementación `aks-dev-test-ha`. Una vez completada correctamente la implementación, puede usar el mismo comando `kubectl get svc` y verá que se crea un servicio `master-secondary-svc` adicional. No es necesario configurar `ServiceType` como `NodePort`. Otros pasos serán similares a lo que se mencionó en la sección anterior.

En el siguiente ejemplo se establece `ServiceType` como `NodePort`:

```azurecli
azdata bdc config replace -c private-bdc-aks /bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>Implementación del clúster de macrodatos en el clúster privado de AKS

```azurecli
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="check-deployment-status"></a>Comprobar el estado de implementación

La implementación tardará unos minutos; puede usar el siguiente comando para comprobar el estado de la implementación: 

```console
kubectl get pods -n mssql-cluster -w
```

## <a name="check-the-service-status"></a>Comprobación del estado del servicio

Utilice el comando siguiente para comprobar los servicios. Compruebe que todos sean correctos sin direcciones IP externas:

```console
kubectl get services -n mssql-cluster
```

Vea cómo [administrar el clúster de macrodatos en el clúster privado de AKS](private-manage.md) y, a continuación, el paso siguiente es [conectarse al clúster de macrodatos](connect-to-big-data-cluster.md).

Vea scripts de automatización para este escenario en el [repositorio de ejemplos de SQL Server en GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks).

## <a name="next-steps"></a>Pasos siguientes

[Administración de un clúster privado](private-manage.md)

[Restricción del tráfico de salida del clúster de macrodatos privado](private-restrict-egress-traffic.md)

[Conexión a un clúster de macrodatos de SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)
