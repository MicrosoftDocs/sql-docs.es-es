---
title: Reconciliación de DNS de Active Directory y Kubernetes en implementaciones de clústeres de macrodatos
description: Configuración de reconciliación DNS para el clúster de macrodatos de SQL Server en el modo de Active Directory
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a20afc81637eba30427b7b1062738b2d81ae67a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047885"
---
# <a name="active-directory-and-kubernetes-dns-reconciliation-in-big-data-clusters-deployments"></a>Reconciliación de DNS de Active Directory y Kubernetes en implementaciones de clústeres de macrodatos

En este artículo se describen algunos de los desafíos y las soluciones para admitir la integración de Active Directory al implementar clústeres de macrodatos (BDC).

## <a name="overview"></a>Información general

Cuando no se implementa el clúster de macrodatos con la integración de Active Directory, se confía en el servicio [CoreDNS de Kubernetes](https://kubernetes.io/docs/tasks/administer-cluster/coredns/) para las resoluciones DNS internas. Kubernetes usa un dominio interno como `<namespace>.svc.cluster.local`. Crea registros A (búsqueda directa) y PTR (búsqueda inversa) en el servidor DNS con nombres en este dominio.

Pero, cuando se habilita el modo de Active Directory, un nuevo dominio entra en escena con su propio conjunto de servidores DNS. Durante la resolución de nombres interna, esto puede dar lugar a confusión sobre qué conjunto de servidores DNS se va a usar para las búsquedas directas e inversas.

## <a name="challenges"></a>Desafíos

* Cuando se implementan pods de Kubernetes nuevos, deben agregarse las entradas de DNS en ambos conjuntos de servidores DNS. Kubernetes se encarga de la grabación de las entradas en su CoreDNS, pero el flujo de trabajo de implementación del clúster de macrodatos es el responsable de agregar las entradas necesarias en los servidores DNS del controlador de dominio de Active Directory. Del mismo modo, cuando se elimina un clúster de macrodatos, el flujo de trabajo debe asegurarse de que estas entradas se quitan.
* Los servidores DNS de Active Directory son externos al clúster de Kubernetes. Pero el clúster de macrodatos tiene su propio espacio de direcciones IP dentro de Kubernetes y no puede crear registros para este espacio IP en un servidor DNS ubicado externamente, ya que este espacio IP no es visible fuera de los límites del clúster.
* Cuando se producen eventos de conmutación por error en el clúster de Kubernetes, también se deben actualizar los registros de los servidores DNS de AD.
* Además de los nombres del pod, los nombres del servicio de Kubernetes también deben ser direccionables a través de las búsquedas de nombres de dominio de AD. Esto crea un desafío adicional en el DNS de Active Directory porque un nombre de servicio puede estar asignado a varias direcciones IP del pod.
* La propagación de actualizaciones de registros y los retrasos de replicación en los servidores DNS de Active Directory de la organización pueden ser significativos e ir más allá del control de los flujos de trabajo de administración del clúster de macrodatos. Esto puede afectar a la funcionalidad del clúster de macrodatos inmediatamente después de la implementación y la conmutación por error. Por el contrario, CoreDNS de Kubernetes es más rápido y eficaz debido a su localidad.

## <a name="solution"></a>Solución

Para solucionar los problemas mencionados anteriormente, la solución implementada en el clúster de macrodatos implica un nuevo servicio CoreDNS interno que se administra dentro del espacio de nombres del clúster de macrodatos. Este es el único servicio DNS al que accederán los pods del espacio de nombres del clúster de macrodatos para las resoluciones de nombres. El nuevo servicio CoreDNS oculta la complejidad de los dominios múltiples.

Por ejemplo, en el diagrama siguiente, los pods usan el servidor de CoreDNS del clúster de macrodatos para la resolución de nombres. Los pods no interactúan directamente con el servidor de CoreDNS de Kubernetes o el servidor DNS de AD. 

:::image type="content" source="media/active-directory-dns-reconciliation/bdc-ad-dns-reconciliation.png" alt-text="Los pods se conectan al servidor de CoreDNS en su propio espacio de nombres":::

Estos son algunos de los detalles de implementación que aclaran cómo funciona este diseño en el clúster de macrodatos:

### <a name="centralized-management-of-multiple-domains"></a>Administración centralizada de varios dominios

La complejidad de lo que sucede en las búsquedas de nombres permanece oculta detrás del servicio DNS interno de manera centralizada. Esto evita que la carga de la administración de varios dominios recaiga en pods individuales y simplifica el diseño.

### <a name="no-records-for-internal-pods-in-external-dns-servers"></a>Sin registros para pods internos en servidores DNS externos

Como resultado de este principio de diseño, el clúster de macrodatos no tendrá que crear y administrar registros A y PTR para pods en el espacio de IP de Kubernetes en servidores DNS externos.

### <a name="no-duplication-of-records"></a>Sin duplicación de registros

Registros DNS internos en varios lugares. El único almacenamiento de estos registros es CoreDNS de Kubernetes. El CoreDNS interno de BDC realizará una reescritura computacional y un reenvío de las consultas de DNS a CoreDNS de Kubernetes.

### <a name="computational-rewriting"></a>Reescritura computacional

Dado que el clúster de macrodatos no almacena ningún registro, no es responsable de traducir las consultas de búsqueda directa entrantes con nombres que tienen el dominio de AD a nombres con el dominio de Kubernetes y reenviar esta consulta a CoreDNS de Kubernetes.
Por ejemplo, una consulta entrante para `compute-0-0.contoso.local` se volvería a escribir como `compute-0-0.compute-0-svc.contoso.svc.cluster.local` y esta solicitud se reenviaría a CoreDNS de Kubernetes.
En el caso de las búsquedas inversas, la solicitud se reenvía con direcciones IP internas cuando se envían a CoreDNS de Kubernetes, y la respuesta se vuelve a escribir desde allí al nombre de dominio de AD antes de responder al cliente.

### <a name="simplicity-in-pod-configurations"></a>Simplicidad en las configuraciones del pod

Dado que solo se hace referencia al CoreDNS del clúster de macrodatos interno en /etc/resolv.conf de todos los pods del clúster de macrodatos, se simplifica la vista de red de los pods. En su lugar, la complejidad se ocultará en el CoreDNS interno.

### <a name="static-and-reliable-ip-address-for-dns-service"></a>Dirección IP estática y confiable para el servicio DNS

El servicio CoreDNS que implementa el clúster de macrodatos tendrá una dirección IP interna estática registrada a la que se puede acceder desde todos los pods. Esto garantizará que no será necesario actualizar los valores de /etc/resolv.conf.

### <a name="service-load-balance-management-is-retained-by-kubernetes"></a>Kubernetes retiene la administración del equilibrio de carga del servicio

Cuando se producen búsquedas para servicios en lugar de pods individuales, seguirán yendo a CoreDNS de Kubernetes, por lo que el clúster de macrodatos no es el responsable de implementar el equilibrio de carga especialmente para el dominio de AD.

Por ejemplo, si se trata de una solicitud de búsqueda directa para `compute-0-svc.contoso.local`, se volverá a escribir como ` compute-0-svc.contoso.svc.cluster`.local. Esta solicitud se reenviará a CoreDNS de Kubernetes y el equilibrio de carga se producirá allí. Una respuesta será una dirección IP para una de las distintas instancias del grupo de proceso (réplicas de pod).

### <a name="scalability"></a>Escalabilidad

Dado que el clúster de macrodatos no almacena ningún registro, el CoreDNS del clúster de macrodatos interno se puede escalar sin la retención de estado y la replicación de registros en varias réplicas. Si los registros DNS se van a almacenar en el clúster de macrodatos, la replicación del estado en todos los pods también tendrá que encargarse de él.

### <a name="externally-visible-service-entries-stay-in-ad-dns"></a>Las entradas de servicio visibles externamente permanecen en el DNS de AD

En el caso de los puntos de conexión de servicio que deben ser accesibles para los clientes fuera del clúster de Kubernetes, se crearán entradas DNS en el servidor DNS de AD a medida que se implementa el clúster de macrodatos. El usuario escribirá los nombres DNS desde los que se va a registrar a través de los perfiles de configuración de implementación.

### <a name="self-deprovisioning"></a>Desaprovisionamiento automático

Una vez eliminado el clúster de macrodatos, no hay ningún trabajo dinámico adicional para eliminar las entradas de DNS cuando se está desaprovisionando el clúster. Las únicas entradas del DNS de Active Directory remoto que se deben limpiar son las de los servicios externos y son estáticas en número. Las entradas de DNS internas se quitarán automáticamente con el clúster.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación del clúster de macrodatos de SQL Server en el modo de Active Directory](active-directory-deploy.md)
- [Administración del acceso al clúster de macrodatos en el modo de Active Directory](active-directory-objects.md)
- [Implementación de varios clústeres de macrodatos de SQL Server en el mismo dominio de Active Directory](active-directory-deployment-background.md)
