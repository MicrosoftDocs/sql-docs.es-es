---
title: Administración de clústeres de macrodatos (BDC) en el clúster privado de Azure Kubernetes Service (AKS)
titleSuffix: SQL Server Big Data Cluster
description: Obtenga información sobre cómo administrar clústeres de macrodatos de SQL Server en el clúster privado de Azure Kubernetes Service (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 768818a9e034961aa0460515e3db84ce3a4a8092
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043875"
---
# <a name="manage-big-data-cluster-in-aks-private-cluster"></a>Administración de clústeres de macrodatos en el clúster privado de AKS

En este artículo se explica cómo administrar un clúster privado de Azure Kubernetes Service (AKS) con clústeres de macrodatos (BDC) de SQL Server implementados en Azure.

Tal como se describe en [Creación de un clúster privado](/azure/aks/private-clusters/), el punto de conexión de servidor de API del clúster privado de AKS no tiene ninguna dirección IP pública. Para administrar el servidor de API, use una máquina virtual que tenga acceso a la red virtual de Azure (VNet) del clúster de AKS.

## <a name="azure-vm---same-vnet"></a>Máquina virtual de Azure: la misma red virtual

El método más sencillo es implementar una máquina virtual de Azure en la misma red virtual que el clúster de AKS.

1. Implemente una máquina virtual de Azure en la misma red virtual con el clúster de AKS. Esto se denomina en ocasiones *jumpbox*.
1. Conéctese a esa máquina virtual e [Instalación de las herramientas de macrodatos de SQL Server 2019](deployment-guidance.md#install-sql-server-2019-big-data-tools).

Por motivos de seguridad, puede usar las características de AKS para que los intervalos IP autorizados de servidor de API limiten el acceso al servidor de API (en el plano de control de AKS). El acceso limitado permite direcciones IP específicas como una máquina virtual de jumpbox o de administración, o bien un intervalo de direcciones IP para un grupo de desarrolladores y la dirección IP de front-end pública del firewall.

## <a name="other-options"></a>Otras opciones

Entre las alternativas al uso de un jumpbox se incluyen las siguientes:

* Usar una máquina virtual de una red diferente y configurar el [emparejamiento de red virtual](/azure/virtual-network/virtual-network-peering-overview) con la red virtual.

* Conexiones [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) o [VPN Gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways).

   En [Opciones para conectarse al clúster privado](/azure/aks/private-clusters#options-for-connecting-to-the-private-cluster) se explican cada uno de estos métodos mencionados con anterioridad.

* Si el servicio se ejecuta detrás de una instancia de [Azure Standard Load Balancer](/azure/aks/load-balancer-standard), se puede habilitar para [Azure Private Link](/azure/private-link/private-link-service-overview#limitations). Con Azure Private Link, puede habilitar el acceso privado desde otras redes virtuales de Azure.

* En un escenario híbrido, también puede configurar las conexiones [Azure ExpressRoute](/azure/expressroute/expressroute-introduction) o [VPN Gateway](/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="next-steps"></a>Pasos siguientes

[Conexión a un clúster de macrodatos de SQL Server con Azure Data Studio](connect-to-big-data-cluster.md)