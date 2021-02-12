---
title: Restricción del tráfico de salida de clústeres de macrodatos en el clúster privado de Azure Kubernetes Service (AKS)
titleSuffix: SQL Server Big Data Clusters
description: Obtenga información sobre cómo restringir el tráfico de salida de clústeres de macrodatos en el clúster privado de Azure Kubernetes Service (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 589031e40ba0f1e53afd5b4082d132358e9cda32
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046025"
---
# <a name="restrict-egress-traffic-of-big-data-clusters-bdc-clusters-in-azure-kubernetes-service-aks-private-cluster"></a>Restricción del tráfico de salida de clústeres de macrodatos en el clúster privado de Azure Kubernetes Service (AKS)

AKS aprovisiona un equilibrador de carga de SKU estándar para que se configure u se use para la salida de manera predeterminada. Sin embargo, es posible que la configuración predeterminada no cumpla los requisitos de todos los escenarios si no se permiten direcciones IP públicas o se requieren saltos adicionales para la salida. Defina una tabla de rutas definida por el usuario si el clúster no permite direcciones IP públicas y se encuentra detrás de una aplicación virtual de red (NVA).

De forma predeterminada, los clústeres de AKS tienen acceso a Internet de salida sin restricciones para los fines de administración y funcionamiento. Los nodos de trabajo de un clúster de AKS deben tener acceso a determinados puertos y nombres de dominio completo (FQDN); por ejemplo:

* Actualizaciones de seguridad del SO del nodo de trabajo, el clúster debe extraer imágenes de contenedor del sistema base desde Microsoft Container Registry (MCR)
* Los nodos de trabajo de AKS habilitados para GPU necesitan acceder a algunos puntos de conexión desde NVIDIA para instalar el controlador.
* En el escenario en el que los clientes usan el trabajo de AKS junto con los servicios de Azure, como Azure Policy para el cumplimiento de nivel empresarial, la acción de Azure Monitor (con información de contenedor) 
* Espacio de desarrollo habilitado y más escenarios de naturaleza similar

> [!NOTE]
> Actualmente, cuando se implementa un clúster de macrodatos en el clúster privado de Azure Kubernetes Service (AKS), no hay dependencias de entrada en el escenario, aparte de lo que se menciona en este artículo; se pueden encontrar todas las dependencias de salida en [Control del tráfico de salida de los nodos de clúster en Azure Kubernetes Service (AKS)](/azure/aks/limit-egress-traffic).

En este artículo se explica cómo implementar un clúster de macrodatos en un clúster privado de AKS con redes avanzadas y UDR (ruta definida por el usuario), así como su mayor integración con el entorno de red de nivel empresarial.

## <a name="use-azure-firewall-to-restrict-egress-traffic"></a>Uso de Azure Firewall para restringir el tráfico de salida

Azure Firewall proporciona una etiqueta FQDN de Azure Kubernetes Service (`(AzureKubernetesService)`) para simplificar esta configuración.

Para obtener información completa sobre esta etiqueta, consulte [Restricción del tráfico de salida mediante Azure Firewall](/azure/aks/limit-egress-traffic#restrict-egress-traffic-using-azure-firewall).

En la imagen siguiente se muestra cómo se restringe el tráfico en un clúster privado de AKS. 

:::image type="content" source="media/private-cluster-restrict-egress-traffic/aks-azure-firewall-egress.png" alt-text="Tráfico de salida del firewall del clúster privado de AKS":::

Estas son las etapas para configurar una arquitectura básica con Azure Firewall:

1. Creación del grupo de recursos y la red virtual.
2. Creación y configuración de Azure Firewall.
3. Creación de una tabla de rutas definida por el usuario.
4. Configurar reglas de firewall
5. Creación de una entidad de servicio.
6. Creación de un clúster privado de AKS.
7. Creación de un perfil de implementación del clúster de macrodatos.
8. Implementación del clúster de macrodatos.

En los pasos siguientes se proporcionan detalles.

## <a name="create-the-resource-group-and-vnet"></a>Creación del grupo de recursos y la red virtual.

1. Defina un conjunto de variables de entorno para crear recursos.

   ```console
   export REGION_NAME=<region>
   export RESOURCE_GROUP=private-bdc-aksudr-rg
   export SUBNET_NAME=aks-subnet
   export VNET_NAME=bdc-vnet
   export AKS_NAME=bdcaksprivatecluster
   ```

1. Creación del grupo de recursos

   ```azurecli
   az group create -n $RESOURCE_GROUP -l $REGION_NAME
   ```

1. Cree la red virtual.

   ```azurecli
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
   ```

## <a name="create-and-set-up-azure-firewall"></a>Cree y configure Azure Firewall.

1. Defina un conjunto de variables de entorno para crear recursos.

   ```console
   export FWNAME=bdcaksazfw
   export FWPUBIP=$FWNAME-ip
   export FWIPCONFIG_NAME=$FWNAME-config

   az extension add --name azure-firewall
   ```

1. Cree una subred dedicada para el firewall.

   > [!NOTE]
   > No se puede cambiar el nombre del firewall después de la creación.

   ```azurecli
   az network vnet subnet create \
     --resource-group $RESOURCE_GROUP \
     --vnet-name $VNET_NAME \
     --name AzureFirewallSubnet \
     --address-prefix 10.3.0.0/24

    az network firewall create -g $RESOURCE_GROUP -n $FWNAME -l $REGION_NAME --enable-dns-proxy true

    az network public-ip create -g $RESOURCE_GROUP -n $FWPUBIP -l $REGION_NAME --sku "Standard"

    az network firewall ip-config create -g $RESOURCE_GROUP -f $FWNAME -n $FWIPCONFIG_NAME --public-ip-address $FWPUBIP --vnet-name $VNET_NAME
   ```

De manera predeterminada, Azure enruta automáticamente el tráfico entre redes locales, las redes virtuales y las subredes de Azure. 

## <a name="create-user-defined-route-table"></a>Creación de una tabla de rutas definida por el usuario.

Cree una tabla de rutas definida por el usuario (UDR) con un salto a Azure Firewall.

```azurecli

export SUBID= <your Azure subscription ID>
export FWROUTE_TABLE_NAME=bdcaks-rt
export FWROUTE_NAME=bdcaksroute
export FWROUTE_NAME_INTERNET=bdcaksrouteinet

export FWPUBLIC_IP=$(az network public-ip show -g $RESOURCE_GROUP -n $FWPUBIP --query "ipAddress" -o tsv)
export FWPRIVATE_IP=$(az network firewall show -g $RESOURCE_GROUP -n $FWNAME --query "ipConfigurations[0].privateIpAddress" -o tsv)

# Create UDR and add a route for Azure Firewall

az network route-table create -g $RESOURCE_GROUP --name $FWROUTE_TABLE_NAME

az network route-table route create -g $RESOURCE_GROUP --name $FWROUTE_NAME --route-table-name $FWROUTE_TABLE_NAME --address-prefix 0.0.0.0/0 --next-hop-type VirtualAppliance --next-hop-ip-address $FWPRIVATE_IP --subscription $SUBID

az network route-table route create -g $RESOURCE_GROUP --name $FWROUTE_NAME_INTERNET --route-table-name $FWROUTE_TABLE_NAME --address-prefix $FWPUBLIC_IP/32 --next-hop-type Internet
```

## <a name="set-up-firewall-rules"></a>Configurar reglas de firewall 

```azurecli
# Add FW Network Rules

az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'apiudp' --protocols 'UDP' --source-addresses '*' --destination-addresses "AzureCloud.$REGION_NAME" --destination-ports 1194 --action allow --priority 100
az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'apitcp' --protocols 'TCP' --source-addresses '*' --destination-addresses "AzureCloud.$REGION_NAME" --destination-ports 9000
az network firewall network-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwnr' -n 'time' --protocols 'UDP' --source-addresses '*' --destination-fqdns 'ntp.ubuntu.com' --destination-ports 123

# Add FW Application Rules

az network firewall application-rule create -g $RESOURCE_GROUP -f $FWNAME --collection-name 'aksfwar' -n 'fqdn' --source-addresses '*' --protocols 'http=80' 'https=443' --fqdn-tags "AzureKubernetesService" --action allow --priority 100
```

Puede asociar la tabla de rutas definida por el usuario (UDR) a un clúster de AKS en el que se implementó el clúster de macrodatos previamente con el siguiente comando:

```azurecli
az network vnet subnet update -g $RESOURCE_GROUP --vnet-name $VNET_NAME --name $SUBNET_NAME --route-table $FWROUTE_TABLE_NAME
```

## <a name="create--configure-service-principal-sp"></a>Creación y configuración de la entidad de servicio

En este paso, debe crear la entidad de servicio y asignar el permiso a la red virtual.

Observe el ejemplo siguiente: 

```azurecli
# Create SP and Assign Permission to Virtual Network

az ad sp create-for-rbac -n "bdcaks-sp" --skip-assignment

APPID=<your service principal ID >
PASSWORD=< your service principal password >
VNETID=$(az network vnet show -g $RESOURCE_GROUP --name $VNET_NAME --query id -o tsv)

# Assign SP Permission to VNET

az role assignment create --assignee $APPID --scope $VNETID --role "Network Contributor"


RTID=$(az network route-table show -g $RESOURCE_GROUP -n $FWROUTE_TABLE_NAME --query id -o tsv)
az role assignment create --assignee $APPID --scope $RTID --role "Network Contributor"
```

## <a name="create-aks-cluster"></a>Creación de un clúster de AKS

A continuación, cree el clúster de AKS con `userDefinedRouting` como tipo de salida.

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --location $REGION_NAME \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --outbound-type userDefinedRouting \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --service-principal $APPID \
    --client-secret $PASSWORD \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

## <a name="build-big-data-cluster-bdc-deployment-profile"></a>Compilación del perfil de implementación del clúster de macrodatos

Puede crear clústeres de macrodatos con perfil personalizado: 

```console
azdata bdc config init --source aks-dev-test --target private-bdc-aks --force
```

## <a name="generate-and-config-bdc-custom-deployment-profile"></a>Generación y configuración de un perfil de implementación personalizado de clúster de macrodatos: 
```console
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.docker.imageTag=2019-CU6-ubuntu-16.04"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.storage.logs.className=default"

azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"

azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -c private-bdc-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="deploy-bdc-in-aks-private-cluster"></a>Implementación del clúster de macrodatos en el clúster privado de AKS

```console
export AZDATA_USERNAME=<your bdcadmin username>
export AZDATA_PASSWORD=< your bdcadmin password>

azdata bdc create --config-profile private-bdc-aks --accept-eula yes
```

## <a name="use-third-party-firewall-instead-of-azure-firewall"></a>Uso de firewall de terceros en lugar de Azure Firewall

Use un firewall de terceros para restringir el tráfico de salida cuando se implemente un clúster de macrodatos con un clúster privado de AKS. Por ejemplo, consulte [los firewalls de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?search=firewall&page=1). Los firewalls de terceros se pueden usar en soluciones de implementación privadas con configuraciones más compatibles. El firewall debe proporcionar las siguientes reglas de red:

* Todas las [reglas de red de salida y FQDN necesarios para clústeres de AKS](/azure/aks/limit-egress-traffic#required-outbound-network-rules-and-fqdns-for-aks-clusters) y todos los puntos de conexión HTTP/HTTPS comodín y las dependencias que pueden variar con el clúster de AKS en función de una serie de calificadores y sus requisitos reales.
* Las reglas de aplicación, FQDN o reglas de red obligatorias globales de Azure mencionadas [aquí](/azure/aks/limit-egress-traffic#azure-global-required-network-rules).
* Las reglas opcionales recomendadas de FQDN o aplicación para clústeres de AKS mencionadas [aquí](/azure/aks/limit-egress-traffic#optional-recommended-fqdn--application-rules-for-aks-clusters). 

Compruebe cómo [administrar el clúster de macrodatos en el clúster privado de AKS](private-manage.md) y, a continuación, el paso siguiente es [conectarse al clúster de macrodatos](connect-to-big-data-cluster.md).

Vea scripts de automatización para este escenario en el [repositorio de ejemplos de SQL Server en GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/private-aks).

## <a name="next-steps"></a>Pasos siguientes

[Administración de clústeres de macrodatos en el clúster privado de AKS](private-manage.md)

[Conexión a un clúster de macrodatos de SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)
