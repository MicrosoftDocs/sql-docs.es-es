---
title: Implementación en el script de Python de Red Hat OpenShift en Azure
titleSuffix: SQL Server Big Data Clusters
description: Aprenda a usar un script de implementación para implementar clústeres de macrodatos de SQL Server en Red Hat OpenShift en Azure (ARO).
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d53950d277b245b405420af2c28fa7666ba653dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044138"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-red-hat-openshift-aro"></a>Use un script de Python para implementar un clúster de macrodatos de SQL Server en Red Hat OpenShift en Azure (ARO).

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este tutorial se usa un script de implementación de Python de ejemplo para implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] en [Red Hat OpenShift en Azure (ARO)](/azure/virtual-machines/linux/openshift-get-started). Esta opción de implementación se admite a partir de SQL Server 2019 CU5.

> [!TIP]
> ARO es solo una opción para hospedar Kubernetes para el clúster de macrodatos. Para obtener información sobre otras opciones de implementación y sobre cómo personalizarlas, vea [Cómo implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md).


> [!WARNING]
> Los volúmenes persistentes creados con la clase de almacenamiento integrada *managed-premium* tienen una directiva de reclamación de *Eliminar*. Por lo tanto, al eliminar el clúster de macrodatos de SQL Server, también se eliminarán las reclamaciones de volúmenes persistentes, así como los volúmenes persistentes. Debe crear clases de almacenamiento personalizadas mediante el aprovisionador azure-disk con una directiva de reclamación de tipo *Retener*, como se explica en los [conceptos de almacenamiento](/azure/aks/concepts-storage/#storage-classes). El script siguiente usa la clase de almacenamiento *managed-premium*. Consulte [Persistencia de los datos](concept-data-persistence.md) para más detalles.

La implementación del clúster de macrodatos predeterminada que se usa aquí consta de una instancia maestra de SQL, una instancia de grupo de proceso, dos instancias de grupo de datos y dos instancias de grupo de almacenamiento. Los datos se conservan con volúmenes persistentes de Kubernetes que usan las clases de almacenamiento predeterminadas de ARO. La configuración predeterminada que se usa en este tutorial es adecuada para entornos de desarrollo y pruebas.

## <a name="prerequisites"></a>Prerrequisitos

- Suscripción a Azure.
- [OC](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html)
- [Versión mínima de Python 3.0](https://www.python.org/downloads)
- [CLI de `az`](/cli/azure/install-azure-cli/)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)
- **Azure Data Studio**

## <a name="log-in-to-your-azure-account"></a>Iniciar sesión en su cuenta

El script usa la CLI de Azure para automatizar la creación de un clúster de ARO. Antes de ejecutar el script, debe iniciar sesión en su cuenta de Azure con la CLI de Azure al menos una vez. Ejecute el siguiente comando desde el símbolo del sistema.

```azurecli
az login
```

## <a name="instructions"></a>Instructions

1. Descargue el script de Python `deploy-sql-big-data-aro.py` y el archivo YAML `bdc-scc.yaml`.

   >Estos archivos se encuentran en este artículo en:
   - [`deploy-sql-big-data-aro.py`](#deploy-sql-big-data-aropy)
   - [`bdc-scc.yaml`](#bdc-sccyaml)

1. Ejecute el script mediante:

```console
python deploy-sql-big-data-aro.py
```

Cuando se le solicite, proporcione la entrada para el identificador de suscripción de Azure y el grupo de recursos de Azure en el que se van a crear los recursos. Opcionalmente, también puede proporcionar la entrada para otras configuraciones o usar los valores predeterminados proporcionados. Por ejemplo:

- `azure_region`
- `vm_size` para los nodos de trabajo de OpenShift. Para lograr una experiencia óptima mientras valida escenarios básicos, se recomienda al menos 8 vCPU y 64 GB de memoria en todos los nodos de trabajo del clúster. El script usa `Standard_D8s_v3` y 3 nodos de trabajo como valor predeterminado. Una configuración de tamaño predeterminado para los clústeres de macrodatos también usa aproximadamente 24 discos para las notificaciones de volúmenes persistentes en todos los componentes.
- configuración de red para la implementación de clústeres de OpenShift: consulte el [artículo sobre la implementación de ARO](\azure\openshift\tutorial-create-cluster) para obtener más detalles sobre cada parámetro.
- `cluster_name`: este valor se usa para el clúster de ARO y el clúster de macrodatos de SQL Server creados sobre ARO. Tenga en cuenta que el nombre del clúster de macrodatos de SQL va a ser un espacio de nombres de Kubernetes.
- `username `: es el nombre de usuario de las cuentas aprovisionadas durante la implementación para la cuenta de administrador del controlador, la cuenta de instancia maestra de SQL Server y la puerta de enlace. Tenga en cuenta que la cuenta de SQL Server de `sa` está deshabilitada automáticamente, como práctica recomendada.
- `password`: se usará el mismo valor para todas las cuentas.

El clúster de macrodatos de SQL Server ahora está implementado en ARO. Ahora puede usar Azure Data Studio para conectarse al clúster. Para obtener más información, vea [Conectarse a un clúster de macrodatos de SQL Server con Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="clean-up"></a>Limpieza

Si está probando [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Azure, debe eliminar el clúster de ARO cuando termine para evitar cargos inesperados. No quite el clúster si desea seguir utilizándolo.

> [!WARNING]
> En los pasos siguientes se anula el clúster de ARO, que también quita el clúster de macrodatos de SQL Server. Si tiene bases de datos o datos de HDFS que desea conservar, haga una copia de seguridad de los datos antes de eliminar el clúster.

Ejecute el siguiente comando de la CLI de Azure para quitar el clúster de macrodatos y el servicio ARO en Azure (reemplace `<resource group name>` por el **grupo de recursos de Azure** que ha especificado en el script de implementación):

```azurecli
az group delete -n <resource group name>
```

## `deploy-sql-big-data-aro.py` 

El script de esta sección implementa el clúster de macrodatos de SQL Server en Red Hat OpenShift en Azure. Copie el script en la estación de trabajo y guárdelo como `deploy-sql-big-data-aro.py` antes de comenzar la implementación.

```python
#
# Prerequisites: 
# 
# Azure CLI (https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), Azure Data CLI (`azdata`) (https://docs.microsoft.com/en-us/sql/big-data-cluster/deploy-install-azdata), oc CLI (https://www.openshift.com/blog/installing-oc-tools-windows)
#
# Run `az login` at least once BEFORE running this script
#

from subprocess import check_output, CalledProcessError, STDOUT, Popen, PIPE, getoutput
from time import sleep
import os
import getpass
import json

def executeCmd (cmd):
    if os.name=="nt":
        process = Popen(cmd.split(),stdin=PIPE, shell=True)
    else:
        process = Popen(cmd.split(),stdin=PIPE)
    stdout, stderr = process.communicate()
    if (stderr is not None):
        raise Exception(stderr)

#
# MUST INPUT THESE VALUES!!!!!
#
SUBSCRIPTION_ID = input("Provide your Azure subscription ID:").strip()
GROUP_NAME = input("Provide Azure resource group name to be created:").strip()
#
# This password will be use for Controller user, Gateway user and SQL Server Master SA accounts
AZDATA_USERNAME=input("Provide username to be used for Controller, SQL Server and Gateway endpoints - Press ENTER for using  `admin`:").strip() or "admin"
AZDATA_PASSWORD = getpass.getpass("Provide password to be used for Controller user, Gateway user and SQL Server Master accounts:")
#
# Optionally change these configuration settings
#
AZURE_REGION=input("Provide Azure region - Press ENTER for using `westus2`:").strip() or "westus2"
# MASTER_VM_SIZE=input("Provide VM size for master nodes for the ARO cluster - Press ENTER for using  `Standard_D2s_v3`:").strip() or "Standard_D2s_v3"
WORKER_VM_SIZE=input("Provide VM size for the worker nodes for the ARO cluster - Press ENTER for using  `Standard_D8s_v3`:").strip() or "Standard_D8s_v3"
OC_NODE_COUNT=input("Provide number of worker nodes for ARO cluster - Press ENTER for using  `3`:").strip() or "3"
VNET_NAME=input("Provide name of Virtual Network for ARO cluster - Press ENTER for using  `aro-vnet`:").strip() or "aro-vnet"
VNET_ADDRESS_SPACE=input("Provide Virtual Network Address Space for ARO cluster - Press ENTER for using  `10.0.0.0/16`:").strip() or "10.0.0.0/16"
MASTER_SUBNET_NAME=input("Provide Master Subnet Name for ARO cluster - Press ENTER for using  `master-subnet`:").strip() or "master-subnet"
MASTER_SUBNET_IP_RANGE=input("Provide address range of Master Subnet for ARO cluster - Press ENTER for using  `10.0.0.0/23`:").strip() or "10.0.0.0/23"
WORKER_SUBNET_NAME=input("Provide Worker Subnet Name for ARO cluster - Press ENTER for using  `worker-subnet`:").strip() or "worker-subnet"
WORKER_SUBNET_IP_RANGE=input("Provide address range of Worker Subnet for ARO cluster - Press ENTER for using  `10.0.2.0/23`:").strip() or "10.0.2.0/23"
#
# This is both Kubernetes cluster name and SQL Big Data cluster name
CLUSTER_NAME=input("Provide name of OpenShift cluster and SQL big data cluster - Press ENTER for using  `sqlbigdata`:").strip() or "sqlbigdata"
#
# Deploy the ARO cluster
#  
print ("Set azure context to subscription: "+SUBSCRIPTION_ID)
command = "az account set -s "+ SUBSCRIPTION_ID
executeCmd (command)
print ("Creating azure resource group: "+GROUP_NAME)
command="az group create --name "+GROUP_NAME+" --location "+AZURE_REGION
executeCmd (command)
command = "az network vnet create --resource-group "+GROUP_NAME+" --name "+VNET_NAME+" --address-prefixes "+VNET_ADDRESS_SPACE
print("Creating Virtual Network: "+VNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+MASTER_SUBNET_NAME+" --address-prefixes "+MASTER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Master Subnet: "+MASTER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+WORKER_SUBNET_NAME+" --address-prefixes "+WORKER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Worker Subnet: "+WORKER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet update --name "+MASTER_SUBNET_NAME+" --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --disable-private-link-service-network-policies true"
print("Updating Master Subnet by disabling Private Link Policies")
executeCmd(command)
command = "az aro create --resource-group "+GROUP_NAME+" --name "+CLUSTER_NAME+" --vnet "+VNET_NAME+" --master-subnet "+MASTER_SUBNET_NAME+" --worker-subnet "+WORKER_SUBNET_NAME+" --worker-count "+OC_NODE_COUNT+" --worker-vm-size "+WORKER_VM_SIZE +" --only-show-errors"
print("Creating OpenShift cluster: "+CLUSTER_NAME)
executeCmd (command)
#
# Login to oc console
#
command = "az aro list-credentials --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
OC_CLUSTER_USERNAME = str(output['kubeadminUsername'])
OC_CLUSTER_PASSWORD = str(output['kubeadminPassword'])
command = "az aro show --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
APISERVER = str(output['apiserverProfile']['url'])
command = "oc login "+ APISERVER+ " -u " + OC_CLUSTER_USERNAME + " -p "+ OC_CLUSTER_PASSWORD
executeCmd (command)
#
# Setup pre-requisites for deploying BDC on OpenShift
#
#
#Creating new project/namespace
command = "oc new-project "+ CLUSTER_NAME
executeCmd (command)
#
# create custom SCC for BDC
command = "oc apply -f bdc-scc.yaml"
executeCmd (command)
#
#Adding the custom scc to BDC namespace
command = "oc adm policy add-scc-to-group bdc-scc system:serviceaccounts:" + CLUSTER_NAME
executeCmd (command)
#
# Deploy big data cluster
#
print ('Set environment variables for credentials')
os.environ['AZDATA_PASSWORD'] = AZDATA_PASSWORD
os.environ['AZDATA_USERNAME'] = AZDATA_USERNAME
os.environ['ACCEPT_EULA']="Yes"
#
#Creating azdata configuration with aro-dev-test profile
command = "azdata bdc config init --source aro-dev-test --target custom --force"
executeCmd (command)
command="azdata bdc config replace -c custom/bdc.json -j ""metadata.name=" + CLUSTER_NAME + ""
executeCmd (command)
#
# Create BDC
command = "azdata bdc create --config-profile custom --accept-eula yes"
executeCmd(command)
#login into BDC cluster and list endpoints
command="azdata login -n " + CLUSTER_NAME
executeCmd (command)
print("")
print("SQL Server big data cluster endpoints: ")
command="azdata bdc endpoint list -o table"
executeCmd(command)
```

## `bdc-scc.yaml`

El siguiente manifiesto .yaml define restricciones de contexto de seguridad (SCC) personalizadas para la implementación del clúster de macrodatos. Cópielo en el mismo directorio que `deploy-sql-big-data-aro.py`.

```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities:
  - SETUID
  - SETGID
  - CHOWN
  - SYS_PTRACE
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: SQL Server BDC custom scc is based on 'nonroot' scc plus additional capabilities required by BDC.
  generation: 2
  name: bdc-scc
readOnlyRootFilesystem: false
requiredDropCapabilities:
  - KILL
  - MKNOD
runAsUser:
  type: MustRunAsNonRoot
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
volumes:
  - configMap
  - downwardAPI
  - emptyDir
  - persistentVolumeClaim
  - projected
  - secret
```

## <a name="next-steps"></a>Pasos siguientes

El script de implementación ha configurado Azure Kubernetes Service y también ha implementado un clúster de macrodatos de SQL Server 2019. También puede optar por personalizar las implementaciones futuras a través de instalaciones manuales. Para obtener más información sobre cómo implementar los clústeres de macrodatos y sobre cómo personalizar las implementaciones, vea [Cómo implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md).

Ahora que el clúster de macrodatos de SQL Server está implementado, puede cargar los datos de ejemplo y explorar los tutoriales:

> [!div class="nextstepaction"]
> [Tutorial: Cargar datos de ejemplo en un clúster de macrodatos de SQL Server 2019](tutorial-load-sample-data.md)