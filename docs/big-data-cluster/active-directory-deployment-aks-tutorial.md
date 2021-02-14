---
title: 'Implementación en Active Directory en Azure Kubernetes Service (AKS): tutorial'
titleSuffix: SQL Server Big Data Cluster
description: Obtenga información sobre cómo implementar clústeres de macrodatos de SQL Server en modo de AD en Azure Kubernetes Service (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 760fc7070f0673f61a28cd126be0c6542ac4ee93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039867"
---
# <a name="tutorial-deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>Tutorial: Implementación de clústeres de macrodatos de SQL Server en modo de AD en Azure Kubernetes Service (AKS)

En este artículo se explica cómo implementar un clúster de macrodatos (BDC) de SQL Server en el modo de autenticación de Active Directory con una arquitectura de referencia. La arquitectura de referencia amplía la instancia local de Active Directory Domain Services (AD DS) a Azure. Puede implementarla en el [Centro de arquitectura de Azure](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain) con [bloques de creación de Azure](https://github.com/mspnp/template-building-blocks/wiki/Install-Azure-Building-Blocks).

## <a name="prerequisites"></a>Requisitos previos

Antes de implementar un clúster de macrodatos de SQL Server, debe realizar lo siguiente:

* Acceder a una máquina virtual de Azure para la administración. Esta máquina virtual requiere acceso a la red virtual (VNet) de Azure donde se implementará el BDC. Debe residir en la misma red virtual o en una [red virtual emparejada](/azure/virtual-network/virtual-network-manage-peering).
* [Instalar las herramientas de macrodatos](deploy-big-data-tools.md) en la máquina virtual de administración.
* Prepararse para implementar el clúster en el [modo de autenticación de Active Directory](active-directory-prerequisites.md) en el controlador de AD local.

## <a name="create-aks-subnet"></a>Creación de una subred de AKS

1. Establecimiento de variables de entorno

   ```console
   export REGION_NAME=< your Azure Region >
   export RESOURCE_GROUP=<your resource group >
   export SUBNET_NAME=aks-subnet
   export VNet_NAME= adds-vnet
   export AKS_NAME= <your aks cluster name>
   ```

1. Creación de una subred de AKS

   ```console
   SUBNET_ID=$(az network vnet subnet show \
    --resource-group $RESOURCE_GROUP \
    --vnet-name $VNet_NAME \
    --name $SUBNET_NAME \
    --query id -o tsv)
   ```

La siguiente captura de pantalla muestra cómo se planean las subredes que se encuentran en la red virtual de la arquitectura.

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="Clúster de AKS con AD y clúster de macrodatos de SQL Server":::

## <a name="create-an-aks-private-cluster"></a>Creación de un clúster privado de AKS

Puede usar el siguiente comando para implementar un clúster privado de AKS. Si no necesita un clúster privado, quite el parámetro `--enable-private-cluster` del comando. Para obtener información sobre otros requisitos, vea [Tutorial: Implementación de un clúster de Azure Kubernetes Service (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster).

```azurecli
az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $AKS_NAME \
    --load-balancer-sku standard \
    --enable-private-cluster \
    --network-plugin azure \
    --vnet-subnet-id $SUBNET_ID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.3.0.10 \
    --service-cidr 10.3.0.0/24 \
    --node-vm-size Standard_D13_v2 \
    --node-count 2 \
    --generate-ssh-keys
```

Después de implementar un clúster de AKS, [conéctese al clúster de AKS](/azure/aks/tutorial-kubernetes-deploy-cluster#connect-to-cluster-using-kubectl).

## <a name="verify-reverse-dns-entry-for-domain-controller"></a>Comprobación de la entrada de DNS inverso para el controlador de dominio

Antes de iniciar la implementación de BDC en el modo de AD en el clúster de AKS, compruebe que el propio controlador de dominio tiene **un registro A** y **un registro PTR** (entrada de DNS inverso) registrados en el servidor DNS.

Para comprobar esta configuración, ejecute el comando `nslookup` o ejecute el [script de PowerShell](troubleshoot-ad-reverse-lookup-zone.md) para confirmar si tiene configurada una entrada de DNS inverso (registro PTR).

## <a name="create-bdc-deployment-profile"></a>Creación de un perfil de implementación del clúster de macrodatos

El siguiente comando crea un perfil de implementación:

```console
azdata bdc config init --source kubeadm-prod  --target bdc-ad-aks
```

Los siguientes comandos se usan para establecer los parámetros de un perfil de implementación.

### <a name="controljson"></a>control.json

```console
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.data.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.storage.logs.className=default"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[0].dnsName=controller.contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.spec.endpoints[1].dnsName=proxys.contoso.com"

# security settings 
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.ouDistinguishedName=OU\=bdc\,DC\=contoso\,DC\=com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.dnsIpAddresses=[\"192.168.0.4\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainControllerFullyQualifiedDns=[\"ad1.contoso.com\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.domainDnsName=contoso.com"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterAdmins=[\"bdcadminsgroup\"]"
azdata bdc config replace -p bdc-ad-aks/control.json -j "$.security.activeDirectory.clusterUsers=[\"bdcusersgroup\"]"
```

### <a name="bdcjson"></a>bdc.json

```console
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].dnsName=master.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].dnsName=mastersec.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.master.spec.endpoints[1].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].dnsName=gateway.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.gateway.spec.endpoints[0].serviceType=NodePort"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].dnsName=approxy.contoso.com"
azdata bdc config replace -p bdc-ad-aks/bdc.json -j "$.spec.resources.appproxy.spec.endpoints[0].serviceType=NodePort"
```

## <a name="initiate-deployment"></a>Inicio de la implementación

El comando siguiente inicia una implementación de BDC:

```console
azdata bdc create --config-profile bdc-ad-aks --accept-eula yes
```

## <a name="next-steps"></a>Pasos siguientes

[Conexión a un clúster de macrodatos de SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)

[Tutorial: Carga de datos de ejemplo en un clúster de macrodatos de SQL Server](tutorial-load-sample-data.md).