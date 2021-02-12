---
title: Implementación de un clúster kubeadm de un solo nodo
titleSuffix: SQL Server Big Data Clusters
description: Use un script de implementación de Bash para implementar un clúster de macrodatos de SQL Server 2019 en un clúster de kubeadm de un solo nodo.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 63e1e3776c62033280287e55c288791901a2d15f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046941"
---
# <a name="deploy-with-a-bash-script-to-a-single-node-kubeadm-cluster"></a>Implementación con un script de bash en un clúster de kubeadm de un solo nodo

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este tutorial, usará un script de implementación de bash de ejemplo para implementar un clúster de Kubernetes de un solo nodo con kubeadm y un clúster de macrodatos SQL Server en él.

## <a name="prerequisites"></a>Prerrequisitos

- Una máquina física o virtual de **servidor** de Vanilla Ubuntu 18.04 o 16.04. El script, que se ejecuta desde la máquina virtual, configura todas las dependencias.

  > [!NOTE]
  > Todavía no se admite el uso de máquinas virtuales Linux de Azure.

- La máquina virtual debe tener al menos 8 CPU, 64 GB de RAM y 100 GB de espacio en disco. Después de extraer todas las imágenes de Docker del clúster de macrodatos, le quedarán 50 GB para los datos y los registros de todos los componentes.

- Actualice los paquetes existentes con los siguientes comandos para asegurarse de que la imagen del sistema operativo está actualizada.

   ``` bash
   sudo apt update && sudo apt upgrade -y
   sudo systemctl reboot
   ```

## <a name="recommended-virtual-machine-settings"></a>Configuración recomendada de máquinas virtuales

1. Use la configuración de memoria estática para la máquina virtual. Por ejemplo, en las instalaciones de Hyper-V no se debe usar la asignación de memoria dinámica, sino que se deben asignar los 64 GB recomendados o más.

1. Use la funcionalidad de punto de control o de instantánea del hipervisor para poder revertir la máquina virtual a un estado limpio.


## <a name="instructions-to-deploy-sql-server-big-data-cluster"></a>Instrucciones para implementar el clúster de macrodatos de SQL Server

1. Descargue el script en la máquina virtual que planea usar para la implementación.

   ```bash
   curl --output setup-bdc.sh https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/setup-bdc.sh
   ```

2. Ejecute el script con el siguiente comando.

   ```bash
   chmod +x setup-bdc.sh
   ```

3. Ejecute el script (asegúrese de que lo está ejecutando con *sudo*).

   ```bash
   sudo ./setup-bdc.sh
   ```

   Cuando se le solicite, proporcione la contraseña que se va a usar para los siguientes puntos de conexión externos: controlador, SQL Server maestro y puerta de enlace. La contraseña debe ser suficientemente compleja en función de las reglas existentes para las contraseñas de SQL Server. De forma predeterminada, el nombre de usuario del controlador es *admin*.

4. Configure un alias para la herramienta **azdata**.

   ```bash
   source ~/.bashrc
   ```

5. Actualice la configuración del alias para azdata.

   ```bash
   azdata --version
   ```

## <a name="cleanup"></a>Limpieza

El script [cleanup-bdc.sh](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu-single-node-vm/cleanup-bdc.sh) se proporciona para facilitarle el restablecimiento del entorno si es necesario. Sin embargo, se recomienda usar una máquina virtual con fines de prueba y usar la funcionalidad de instantánea en el hipervisor para revertir la máquina virtual a un estado limpio.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a usar los clústeres de macrodatos, consulte [Tutorial: Carga de datos de ejemplo en un clúster de macrodatos de SQL Server](tutorial-load-sample-data.md).
