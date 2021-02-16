---
title: Implementación de un contenedor de SQL Server con Azure Kubernetes Service (AKS)
description: En este tutorial se explica cómo implementar una solución de alta disponibilidad de SQL Server con Kubernetes en Azure Kubernetes Service.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/01/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 53d63320bc72b0c5f0e1891b67fd41b81d46458e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337294"
---
# <a name="deploy-a-sql-server-container-in-kubernetes-with-azure-kubernetes-services-aks"></a>Implementación de un contenedor de SQL Server en Kubernetes con Azure Kubernetes Service (AKS)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Aprenda a configurar una instancia de SQL Server en Kubernetes en Azure Kubernetes Service (AKS), con almacenamiento persistente para la alta disponibilidad. La solución proporciona resistencia. Si se produce un error en la instancia de SQL Server, Kubernetes vuelve a crearla automáticamente en un nuevo pod. Kubernetes también proporciona resistencia frente a un error de nodo.

En este tutorial se explica cómo configurar una instancia de SQL Server de alta disponibilidad en un contenedor en AKS.

> [!div class="checklist"]
> * Creación de una contraseña de administrador del sistema
> * Creación de almacenamiento
> * Creación de la implementación
> * Conexión con SQL Server Management Studio (SSMS)
> * Comprobación de errores y recuperación

## <a name="ha-solution-on-kubernetes-running-in-azure-kubernetes-service"></a>Solución de alta disponibilidad en Kubernetes en ejecución en Azure Kubernetes Service

Kubernetes 1.6 y versiones posteriores admite [clases de almacenamiento](https://kubernetes.io/docs/concepts/storage/storage-classes/), [notificaciones de volumen persistente](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims) y el [tipo de volumen de disco de Azure](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). Puede crear y administrar las instancias de SQL Server de forma nativa en Kubernetes. En el ejemplo de este artículo se muestra cómo crear una [implementación](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) para lograr una configuración de alta disponibilidad similar a una instancia de clúster de conmutación por error de disco compartido. En esta configuración, Kubernetes desempeña el papel de orquestador de clústeres. Si se produce un error en una instancia de SQL Server de un contenedor, el orquestador arranca otra instancia del contenedor que se adjunta al mismo almacenamiento persistente.

![Contenedor de SQL Server en un clúster de Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

En el diagrama anterior, `mssql-server` es un contenedor en un [pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/). Kubernetes orquesta los recursos del clúster. Un [conjunto de réplicas](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) garantiza que el pod se recupere automáticamente tras un error de nodo. Las aplicaciones se conectan al servicio. En este caso, el servicio representa un equilibrador de carga que hospeda una dirección IP que permanece igual tras un error de `mssql-server`.

En el diagrama siguiente, se ha producido un error del contenedor `mssql-server`. Como orquestador, Kubernetes garantiza el recuento correcto de instancias correctas en el conjunto de réplicas e inicia un nuevo contenedor de acuerdo con la configuración. El orquestador inicia un nuevo pod en el mismo nodo y `mssql-server` se vuelve a conectar al mismo almacenamiento persistente. El servicio se conecta al nuevo `mssql-server` creado.

![Error de pod de SQL Server en el clúster de Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-pod-fail.png)

En el diagrama siguiente, se ha producido un error en el nodo que hospeda el contenedor `mssql-server`. El orquestador inicia el nuevo pod en otro nodo y `mssql-server` se vuelve a conectar al mismo almacenamiento persistente. El servicio se conecta al nuevo `mssql-server` creado.

![Recuperación del pod de SQL Server en un clúster de Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql-after-node-fail.png)

## <a name="prerequisites"></a>Prerrequisitos

* **Clúster de Kubernetes**
   - El tutorial requiere un clúster de Kubernetes. En los pasos se usa [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) para administrar el clúster. 

   - Consulte [Implementación de un clúster de Azure Kubernetes Service (AKS)](/azure/aks/tutorial-kubernetes-deploy-cluster) para crear un clúster de Kubernetes de un solo nodo en AKS con `kubectl` y conectarse a él. 

   >[!NOTE]
   >Para protegerse frente a errores de nodo, un clúster de Kubernetes requiere más de un nodo.

* **CLI de Azure 2.0.23**
   - Las instrucciones de este tutorial se han validado con la CLI de Azure 2.0.23.

## <a name="create-an-sa-password"></a>Creación de una contraseña de administrador del sistema

Cree una contraseña de administrador del sistema en el clúster de Kubernetes. Kubernetes puede administrar información de configuración confidencial (por ejemplo, las contraseñas) como [secretos](https://kubernetes.io/docs/concepts/configuration/secret/).

El siguiente comando crea una contraseña para la cuenta SA:

   ```console
   kubectl create secret generic mssql --from-literal=SA_PASSWORD="MyC0m9l&xP@ssw0rd"
   ```  

   Reemplace `MyC0m9l&xP@ssw0rd` por una contraseña compleja.

   Para crear un secreto en Kubernetes denominado `mssql` que contenga el valor `MyC0m9l&xP@ssw0rd` de `SA_PASSWORD`, ejecute el comando.


## <a name="create-storage"></a>Creación de almacenamiento

Configure un [volumen persistente](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) y una [notificación de volumen persistente](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volume-claim-protection) en el clúster de Kubernetes. Complete los pasos siguientes: 

1. Cree un manifiesto para definir la clase de almacenamiento y la notificación de volumen persistente.  El manifiesto especifica el aprovisionamiento de almacenamiento, los parámetros y la [directiva de recuperación](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#reclaiming). El clúster de Kubernetes usa este manifiesto para crear el almacenamiento persistente. 

   En el siguiente ejemplo de YAML se define una clase de almacenamiento y una notificación de volumen persistente. El almacenamiento de la clase de almacenamiento es `azure-disk` porque este clúster de Kubernetes está en Azure. El tipo de cuenta de almacenamiento es `Standard_LRS`. La notificación de volumen persistente se denomina `mssql-data`. Los metadatos de la notificación de volumen persistente incluyen una anotación que lo vuelve a conectar con la clase de almacenamiento. 

   ```yaml
   kind: StorageClass
   apiVersion: storage.k8s.io/v1
   metadata:
        name: azure-disk
   provisioner: kubernetes.io/azure-disk
   parameters:
     storageaccounttype: Standard_LRS
     kind: Managed
   ---
   kind: PersistentVolumeClaim
   apiVersion: v1
   metadata:
     name: mssql-data
     annotations:
       volume.beta.kubernetes.io/storage-class: azure-disk
   spec:
     accessModes:
     - ReadWriteOnce
     resources:
       requests:
         storage: 8Gi
   ```

   Guarde el archivo (por ejemplo, **pvc.yaml**).

1. Cree la notificación de volumen persistente en Kubernetes.

   ```console
   kubectl apply -f <Path to pvc.yaml file>
   ```

   `<Path to pvc.yaml file>` es la ubicación en la que ha guardado el archivo.

   El volumen persistente se crea automáticamente como una cuenta de Azure Storage y se enlaza a la notificación de volumen persistente. 

    ![Captura de pantalla del comando de la notificación de volumen persistente](media/tutorial-sql-server-containers-kubernetes/02_pvc_cmd.png)

1. Compruebe la notificación de volumen persistente.

   ```console
   kubectl describe pvc <PersistentVolumeClaim>
   ```

   `<PersistentVolumeClaim>` es el nombre de la notificación de volumen persistente.

   En el paso anterior, la notificación de volumen persistente se denomina `mssql-data`. Para ver los metadatos sobre la notificación de volumen persistente, ejecute el siguiente comando:

   ```console
   kubectl describe pvc mssql-data
   ```

   Los metadatos devueltos incluyen un valor denominado `Volume`. Este valor se asigna al nombre del blob.

   ![Captura de pantalla de los metadatos devueltos, incluido el volumen](media/tutorial-sql-server-containers-kubernetes/describe-volume.png)

   El valor del volumen coincide con parte del nombre del blob en la siguiente imagen de Azure Portal: 

   ![Captura de pantalla del nombre del blob en Azure Portal](media/tutorial-sql-server-containers-kubernetes/describe-volume-portal.png)

1. Compruebe el volumen persistente.

   ```console
   kubectl describe pv
   ```

   `kubectl` devuelve metadatos sobre el volumen persistente que se ha creado automáticamente y se ha enlazado a la notificación de volumen persistente. 

## <a name="create-the-deployment"></a>Creación de la implementación

En este ejemplo, el contenedor que hospeda la instancia de SQL Server se describe como un objeto de implementación de Kubernetes. La implementación crea un conjunto de réplicas. El conjunto de réplicas crea el pod. 

En este paso, cree un manifiesto para describir el contenedor basado en la imagen de Docker [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) de SQL Server. El manifiesto hace referencia a la notificación de volumen persistente `mssql-server` y al secreto `mssql` que ya ha aplicado al clúster de Kubernetes. El manifiesto también describe un [servicio](https://kubernetes.io/docs/concepts/services-networking/service/). Este servicio es un equilibrador de carga. El equilibrador de carga garantiza que la dirección IP se conserva después de que se recupere la instancia de SQL Server. 

1. Cree un manifiesto (un archivo YAML) para describir la implementación. En el ejemplo siguiente se describe una implementación, incluido un contenedor basado en la imagen de contenedor de SQL Server.

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: mssql-deployment
   spec:
     replicas: 1
     selector:
        matchLabels:
          app: mssql
     template:
       metadata:
         labels:
           app: mssql
       spec:
         terminationGracePeriodSeconds: 30
         hostname: mssqlinst
         securityContext:
           fsGroup: 10001
         containers:
         - name: mssql
           image: mcr.microsoft.com/mssql/server:2019-latest
           ports:
           - containerPort: 1433
           env:
           - name: MSSQL_PID
             value: "Developer"
           - name: ACCEPT_EULA
             value: "Y"
           - name: SA_PASSWORD
             valueFrom:
               secretKeyRef:
                 name: mssql
                 key: SA_PASSWORD 
           volumeMounts:
           - name: mssqldb
             mountPath: /var/opt/mssql
         volumes:
         - name: mssqldb
           persistentVolumeClaim:
             claimName: mssql-data
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: mssql-deployment
   spec:
     selector:
       app: mssql
     ports:
       - protocol: TCP
         port: 1433
         targetPort: 1433
     type: LoadBalancer
   ```

   Copie el código anterior en un nuevo archivo denominado `sqldeployment.yaml`. Actualice los siguientes valores: 

   * MSSQL_PID `value: "Developer"`: establece el contenedor para ejecutar la edición SQL Server Developer. La edición Developer no tiene licencia para los datos de producción. Si la implementación es para su uso en producción, establezca la edición adecuada (`Enterprise`, `Standard` o `Express`). 

      >[!NOTE]
      >Para obtener más información, vea [Cómo obtener una licencia de SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

   * `persistentVolumeClaim`: este valor requiere una entrada para `claimName:` que se asigne al nombre usado para la notificación de volumen persistente. En este tutorial se usa `mssql-data`. 

   * `name: SA_PASSWORD`: Configura la imagen de contenedor para establecer la contraseña de administrador del sistema, tal como se define en esta sección.

     ```yaml
     valueFrom:
       secretKeyRef:
         name: mssql
         key: SA_PASSWORD
     ```

    Cuando Kubernetes implementa el contenedor, hace referencia al secreto denominado `mssql` para obtener el valor de la contraseña.

   * `securityContext`: un objeto securityContext define la configuración de privilegios de control de acceso para un pod o contenedor, en este caso se especifica en el nivel de pod, de modo que todos los contenedores (en este caso, solo uno) se adhieren a ese contexto de seguridad. En el contexto de seguridad, se define fsGroup con el valor 10001 (que es el GID para el grupo mssql), lo que significa que todos los procesos del contenedor también forman parte del identificador de grupo complementario 10001(mssql). El propietario del volumen /var/opt/mssql y todos los archivos creados en ese volumen será el id. de grupo 10001(mssql group).

   >[!NOTE]
   >Al usar el tipo de servicio `LoadBalancer`, se puede acceder a la instancia de SQL Server de forma remota (mediante Internet) en el puerto 1433.

   Guarde el archivo (por ejemplo, **sqldeployment.yaml**).

1. Cree la implementación.

   ```console
   kubectl apply -f <Path to sqldeployment.yaml file>
   ```

   `<Path to sqldeployment.yaml file>` es la ubicación en la que ha guardado el archivo.

   ![Captura de pantalla del comando de implementación](media/tutorial-sql-server-containers-kubernetes/04_deploy_cmd.png)

   Se crean la implementación y el servicio. La instancia de SQL Server está en un contenedor, que está conectado al almacenamiento persistente.

   Para ver el estado del pod, escriba `kubectl get pod`.

   ![Captura de pantalla del comando get pod](media/tutorial-sql-server-containers-kubernetes/05_get_pod_cmd.png)

   En la imagen anterior, el pod tiene un estado de `Running`. Este estado indica que el contenedor está listo. Esto podría tardar varios minutos.

   >[!NOTE]
   >Una vez creada la implementación, pueden pasar unos minutos hasta que el pod se pueda ver. El retraso se debe a que el clúster extrae la imagen [mssql-server-linux](https://hub.docker.com/_/microsoft-mssql-server) del centro de Docker. Una vez que se extrae la imagen por primera vez, las implementaciones posteriores pueden ser más rápidas si la implementación se realiza en un nodo que ya tiene la imagen almacenada en la memoria caché. 

1. Compruebe que los servicios se está ejecutando. Ejecute el siguiente comando:

   ```console
   kubectl get services 
   ```

   Este comando devuelve los servicios que se están ejecutando, así como sus direcciones IP internas y externas. Anote la dirección IP externa del servicio `mssql-deployment`. Use esta dirección IP para conectarse a SQL Server. 

   ![Captura de pantalla del comando get service](media/tutorial-sql-server-containers-kubernetes/06_get_service_cmd.png)

   Para obtener más información sobre el estado de los objetos en el clúster de Kubernetes, ejecute:

   ```azurecli
   az aks browse --resource-group <MyResourceGroup> --name <MyKubernetesClustername>
   ```

1. También puede comprobar que el contenedor se ejecuta como no raíz mediante la ejecución del comando siguiente:

    ```console
    kubectl.exe exec <name of SQL POD> -it -- /bin/bash 
    ```

    Y después de ejecutar "whoami" debería ver el nombre de usuario como mssql, que es un usuario no raíz.

    ```console
    whoami
    ```

## <a name="connect-to-the-sql-server-instance"></a>Conexión con la instancia de SQL Server

Si ha configurado el contenedor como se ha descrito, puede conectarse con una aplicación desde fuera de la red virtual de Azure. Use la cuenta `sa` y la dirección IP externa del servicio. Use la contraseña que ha configurado como secreto de Kubernetes. 

Puede usar las siguientes aplicaciones para conectarse a la instancia de SQL Server. 

* [SSMS](./sql-server-linux-manage-ssms.md)

* [SSDT](./sql-server-linux-develop-use-ssdt.md)

* sqlcmd

   Para conectarse a `sqlcmd`, ejecute el siguiente comando:

   ```cmd
   sqlcmd -S <External IP Address> -U sa -P "MyC0m9l&xP@ssw0rd"
   ```

   Reemplace los siguientes valores:

  * `<External IP Address>` por la dirección IP del servicio `mssql-deployment`. 
  * `MyC0m9l&xP@ssw0rd` por la contraseña.

## <a name="verify-failure-and-recovery"></a>Comprobación de errores y recuperación

Para comprobar los errores y la recuperación, puede eliminar el pod. Siga estos pasos:

1. Enumere el pod que ejecuta SQL Server.

   ```console
   kubectl get pods
   ```

   Anote el nombre del pod que ejecuta SQL Server.

1. Elimine el pod.

   ```console
   kubectl delete pod mssql-deployment-0
   ```

   `mssql-deployment-0` es el valor devuelto del paso anterior para el nombre del pod. 

Kubernetes vuelve a crear automáticamente el pod para recuperar una instancia de SQL Server y se conecta al almacenamiento persistente. Use `kubectl get pods` para comprobar que se ha implementado un nuevo pod. Use `kubectl get services` para comprobar que la dirección IP del nuevo contenedor es la misma. 

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido a implementar contenedores de SQL Server en un clúster de Kubernetes para la alta disponibilidad. 

> [!div class="checklist"]
> * Creación de una contraseña de administrador del sistema
> * Creación de almacenamiento
> * Creación de la implementación
> * Conexión con SQL Server Management Studio (SSMS)
> * Comprobación de errores y recuperación

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
>[Introducción a Kubernetes](/azure/aks/intro-kubernetes)
