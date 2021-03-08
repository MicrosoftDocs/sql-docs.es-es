---
title: Implementación en Active Directory en Azure Kubernetes Service (AKS)
titleSuffix: SQL Server Big Data Cluster
description: En este artículo se explican conceptos y otra información de planeamiento relativos a la implementación de clústeres de macrodatos de SQL Server en el modo de AD en Azure Kubernetes Service (AKS).
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 11/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a0407589eefab4d5e997d582fee900a20dfcb0a3
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837016"
---
# <a name="deploy-sql-server-big-data-clusters-in-ad-mode-on-azure-kubernetes-services-aks"></a>Implementación de clústeres de macrodatos de SQL Server en modo de AD en Azure Kubernetes Service (AKS)

Los clústeres de macrodatos de SQL Server admiten el [modo de implementación de Active Directory (AD)](./active-directory-prerequisites.md) para la **administración de identidades y acceso (IAM)** . La IAM para **Azure Kubernetes Service (AKS)** ha sido todo un desafío porque los protocolos estándar del sector, como OAuth 2.0 y OpenID Connect, ampliamente compatibles con la plataforma de identidades de Microsoft, no son compatibles con SQL Server.  

En este artículo se explica cómo implementar clústeres de macrodatos (BDC) en el modo de AD mientras se realiza la implementación en [Azure Kubernetes Service (AKS)](/azure/aks/intro-kubernetes). 

## <a name="architecture-topologies"></a>Topologías de arquitectura

**Active Directory Domain Services (AD DS)** se ejecuta en una máquina virtual de Azure [de la misma manera](/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm) en la que se ejecuta en muchas instancias locales.  Después de promocionar los nuevos controladores de dominio en Azure, configure los servidores DNS principal y secundario para la red virtual; de esta forma, los servidores DNS locales se degradarán a terciarios o valores inferiores. La autenticación de AD permite que los clientes unidos a un dominio en [Linux puedan autenticarse en SQL Server](../linux/sql-server-linux-active-directory-auth-overview.md) mediante sus credenciales de dominio y el protocolo Kerberos.

Hay varias maneras de implementar un BDC en modo de AD en AKS.  En este artículo se presentan dos métodos, que son más fáciles de implementar e integrar con las arquitecturas empresariales existentes:

* **Ampliación del dominio local de Active Directory a Azure.** Este método [permite que el entorno de Active Directory](/azure/architecture/reference-architectures/identity/adds-extend-domain) proporcione servicios de autenticación distribuidos mediante Active Directory Domain Services (AD DS) en Azure. Replicará su instancia local de Active Directory Domain Services (AD DS) para reducir la latencia causada por el envío de solicitudes de autenticación de la nube a dicha instancia local de AD DS. Un caso de uso típico de esta solución es cuando la aplicación se hospeda parcialmente de forma local y en Azure, y es necesario enviar y recibir las solicitudes de autenticación.

   Vea cómo implementar esta solución paso a paso [en esta arquitectura de referencia](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-extend-domain).

* **Ampliación del bosque de recursos de Active Directory Domain Services (AD DS) en Azure.** En esta arquitectura, debe crear un dominio nuevo en Azure que sea de confianza para los bosques locales de AD. Esta arquitectura muestra una [confianza unidireccional entre el dominio de Azure y el bosque local](/azure/architecture/reference-architectures/identity/adds-forest).

   La confianza permite que los usuarios locales accedan a los recursos del dominio en Azure. Vea cómo implementar esta solución paso a paso [en esta arquitectura de referencia](https://github.com/mspnp/identity-reference-architectures/tree/master/adds-forest).

Las arquitecturas de referencia descritas anteriormente le permiten crear una zona de aterrizaje con todos los recursos que se van a implementar desde cero o cualquier solución adicional basada en la arquitectura existente. Además de esas arquitecturas de referencia, debe implementar BDC en un clúster de AKS en una subred independiente que permanezca en la red virtual de destino o en una emparejada.

La siguiente imagen representa una arquitectura típica:

:::image type="content" source="media/active-directory-deployment-aks/ad-in-aks-diagram.png" alt-text="Clúster de AKS con AD y clúster de macrodatos de SQL Server":::

## <a name="recommendations"></a>Recomendaciones

Las siguientes recomendaciones se aplican a la mayoría de las implementaciones de BDC en modo de AD en AKS. Las opciones disponibles se mencionarán en cada componente para proporcionar orientación y lograr una mejor integración con la arquitectura de nivel empresarial.

### <a name="networking-recommendations"></a>Recomendaciones de redes

Es posible usar algunos componentes clave para conectar su entorno local a Azure:

* **Azure VPN Gateway**: Una puerta de enlace de VPN es un tipo específico de puerta de enlace de red virtual que se usa para enviar tráfico cifrado entre una red virtual de Azure y una ubicación local por medio de la red pública de Internet. Usará la puerta de enlace de red virtual de Azure y la puerta de enlace de red virtual local. Consulte [¿Qué es VPN Gateway?](/azure/vpn-gateway/vpn-gateway-about-vpngateways) para obtener más información sobre cómo configurarlas.
* **Azure ExpressRoute**: Las conexiones de ExpressRoute no se realizan sobre una conexión pública a Internet, y ofrecen una mayor confiabilidad, seguridad y velocidad con una menor latencia que las conexiones a Internet típicas. La elección de la opción de conectividad afectará a la latencia, el rendimiento y el nivel de contrato de nivel de servicio de la solución, en función de las SKU. Para obtener información específica, vea [Acerca de las puertas de enlace de red virtual de ExpressRoute](/azure/expressroute/expressroute-about-virtual-network-gateways).

La mayoría de los clientes usan jumpbox o [Azure Bastion](/azure/bastion/bastion-overview) para acceder a otra infraestructura de Azure. **Azure Private Link** le permite acceder de forma segura a los servicios PaaS de Azure, incluido AKS de este escenario y otros servicios hospedados en Azure, por medio de un punto de conexión privado de la red virtual. El tráfico entre la red virtual y el servicio atraviesa la red troncal de Microsoft y elimina la exposición a la red pública de Internet. Puede crear su propio servicio Private Link en la red virtual y enviarlo de forma privada a los clientes.

### <a name="active-directory-and-azure-recommendation"></a>Recomendación de Active Directory y Azure

La instancia local de AD DS almacena información sobre las cuentas de usuario y permite que otros usuarios autorizados de la misma red accedan a dicha información mediante la autenticación de identidades asociadas a usuarios, equipos, aplicaciones u otros recursos incluidos en un límite de seguridad. En la mayoría de los escenarios híbridos, la autenticación de los usuarios se realiza a través de una conexión de VPN Gateway o ExpressRoute con el entorno de AD DS local.  

En el caso de una implementación de BDC en modo de AD, la solución para [integrar la instancia local de Active Directory con Azure](/azure/architecture/reference-architectures/identity/) debe cumplir los siguientes requisitos previos:

* Una [cuenta de AD tiene permiso específico](active-directory-prerequisites.md) para crear cuentas de usuario, de grupos y de equipos dentro de la unidad organizativa (UO) proporcionada en la instancia local de Active Directory.
* Un servidor DNS para [resolver el DNS interno](active-directory-dns-reconciliation.md). Debe contener tanto registros **A (búsqueda directa)** y **PTR (búsqueda inversa)** en el servidor DNS con nombres en este dominio. Especifique la configuración del DNS del dominio en el perfil de implementación de BDC.  

## <a name="next-steps"></a>Pasos siguientes

[Tutorial: Implementación de clústeres de macrodatos de SQL Server en modo de AD en Azure Kubernetes Service (AKS)](active-directory-deployment-aks-tutorial.md)