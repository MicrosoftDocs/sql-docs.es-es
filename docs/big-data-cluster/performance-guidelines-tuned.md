---
title: Procedimientos recomendados de rendimiento para clústeres de macrodatos de SQL Server
description: En este artículo se ofrecen instrucciones y procedimientos recomendados para ejecutar clústeres de macrodatos de SQL Server en Kubernetes.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7b430e09a06523bc3ee369434911738f83ab5ee4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044207"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-big-data-clusters"></a>Procedimientos recomendados de rendimiento e instrucciones de configuración para clústeres de macrodatos de SQL Server

[!INCLUDE [sqlserver2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se ofrecen procedimientos recomendados y recomendaciones para maximizar el rendimiento de las aplicaciones dirigidas a los servicios que se ejecutan con un clúster de macrodatos.

Las siguientes directrices se centran en las recomendaciones para configurar el sistema operativo Linux que hospeda los nodos de trabajo de Kubernetes en los que se implementará el clúster de macrodatos. Como procedimiento recomendado, configure el perfil de optimización antes de implementar el clúster de macrodatos. La configuración incluida en el perfil de optimización propuesto se validó durante el caso práctico realizado por Microsoft e Intel. Los resultados del estudio se publican para su descarga en estas [notas del producto](https://aka.ms/sql-bdc-spark-perf/).

> [!TIP]
> Para conocer las configuraciones de optimización específicas de SQL Server en Linux, consulte [Procedimientos recomendados e instrucciones de configuración de SQL Server en Linux](../linux/sql-server-linux-performance-best-practices.md). Además, se siguen aplicando otros procedimientos recomendados como el diseño de índices para las bases de datos de SQL Server.

## <a name="proposed-linux-settings-using-a-tuned-mssql-bdc-profile"></a>Configuración propuesta de Linux mediante un perfil `mssql-bdc` optimizado

Cree un perfil de configuración **tuned.conf** con el contenido siguiente.

```bash
[main]
summary=Optimize for Microsoft SQL Server Big Data Clusters TPC-DS performance
include=throughput-performance
 
[sysctl]
#network tunings
net.ipv4.conf.default.rp_filter=1
net.ipv4.tcp_timestamps=0
net.ipv4.tcp_sack = 1
net.core.netdev_max_backlog = 25000
net.core.rmem_max = 2147483647
net.core.wmem_max = 2147483647
net.core.rmem_default = 33554431
net.core.wmem_default = 33554432
net.core.optmem_max = 33554432
net.ipv4.tcp_rmem =8192 33554432 2147483647
net.ipv4.tcp_wmem =8192 33554432 2147483647
net.ipv4.tcp_low_latency=1
net.ipv4.tcp_adv_win_scale=1
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv4.conf.all.arp_filter=1
net.ipv4.tcp_retries2=5
net.ipv6.conf.lo.disable_ipv6 = 1
net.core.somaxconn = 65535
 
#memory cache settings
vm.swappiness=1
vm.overcommit_memory=0
vm.dirty_background_ratio=1
 
#kernel NUMA
kernel.numa_balancing=0

#filesystem
fs.aio-max-nr=1048576
 
[vm]
#should be revisited for SQL large pages use in master/data/compute pods
transparent_hugepages=never
```

## <a name="install-tuned-utility-on-all-the-kubernetes-worker-nodes"></a>Instale la utilidad **Tuned** en todos los nodos de trabajo de Kubernetes

Para instalar **Tuned**, ejecute:

```bash
apt-get -y install tuned
```

## <a name="apply-tuning-settings-to-all-kubernetes-worker-nodes"></a>Aplicación de la configuración de optimización a todos los nodos de trabajo de Kubernetes

En cada uno de los nodos de trabajo de destino, copie el archivo **tuned.conf** creado anteriormente:

```bash
cd /usr/lib/tuned
scp -r <sourcePath> ./mssql-bdc
```

Para habilitar este perfil optimizado **mssql-bdc**, guarde estas definiciones en un archivo **tuned.conf** en una carpeta `/usr/lib/tuned/mssql-bdc` en todos los nodos de trabajo de Kubernetes y habilite el perfil mediante:

```bash
chmod +x /usr/lib/tuned/mssql-bdc/tuned.conf
tuned-adm profile mssql-bdc
```

Compruebe que se ha habilitado con este comando:

```bash
tuned-adm active
```

or

```bash
tuned-adm list
```

Si se cambia el perfil dinámicamente, para que los nuevos cambios surtan efecto, debe reiniciar **Tuned** en todos los nodos de trabajo afectados:

```bash
systemctl restart tuned
```
 
Los registros para el servicio de **Tuned** se pueden encontrar en */var/log/tuned/tuned.log*.

Opcionalmente, puede configurar el perfil de optimización en un nodo del clúster de Kubernetes y usar el siguiente script para copiarlo y configurarlo en el resto de los nodos.

```bash
#!/bin/bash -e
# This script takes a list of servers (IPs like `cat ~administrator/workerhosts)) as input
# and update these servers with the local version of mssql-bdc tuned.conf.
 
is_root() {
    local is_root_set=0
    if [ "$EUID" -ne 0 ]; then
        echo "Please run as root"
    else
        is_root_set=1
    fi
    return "${is_root_set}"
}
 
# function main
if is_root -eq 0; then
    exit 0
fi
 
while [ $# -gt 0 ]
do
    # sometimes, people add non-breaking space characters to their *host* files.
    WORKER_IP=$(echo "$1" | sed -e 's/\xC2\xA0//g')
    echo -e "updating mssql-bdc tuned on \e[42m${WORKER_IP}\e[0m"
    # the following commands assume tuned was not set up and active...
    # TODO: add the tuned install and status checks
    ssh "${WORKER_IP}" mkdir -p /usr/lib/tuned/mssql-bdc
    scp ~administrator/tuned.conf "${WORKER_IP}":/usr/lib/tuned/mssql-bdc/tuned.conf
    ssh "${WORKER_IP}" tuned-adm profile mssql-bdc
    ssh "${WORKER_IP}" systemctl restart tuned
    ssh "${WORKER_IP}" tuned-adm active
    shift
done

```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más recursos, incluidas las arquitecturas de referencia de los clústeres de macrodatos de SQL Server, consulte:

* [Caso práctico: Cargas de trabajo de SQL que se ejecutan en Apache Spark en el clúster de macrodatos de MS SQL Server 2019](https://aka.ms/sql-bdc-spark-perf/)

* [Arquitectura de referencia de HPE para ofrecer una visión general de todos los datos con clústeres de macrodatos de Microsoft SQL Server 2019](https://h20195.www2.hpe.com/V2/GetDocument.aspx?docname=a50001963enw)

* [PowerStore de Dell EMC: Clústeres de macrodatos de Microsoft SQL Server 2019](https://www.dellemc.com/resources/en-us/asset/white-papers/products/storage/h18231-dell-emc-powerstore-sql-server-big-data-clusters.pdf)

* [Clústeres de macrodatos de Microsoft SQL Server 2019: Una solución de macrodatos con la infraestructura de Dell EMC](https://infohub.delltechnologies.com/t/microsoft-sql-server-2019-big-data-clusters-a-big-data-solution-using-dell-emc-infrastructure/)

* [Clústeres de macrodatos de Microsoft SQL Server 2019 en la arquitectura de referencia de Cisco UCS](https://www.cisco.com/c/en/us/solutions/collateral/data-center-virtualization/unified-computing/sql-server-on-big-data-cluster-on-ucs.html)