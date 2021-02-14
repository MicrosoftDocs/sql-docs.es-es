---
title: Guía para la implementación
titleSuffix: SQL Server Big Data Clusters
description: Obtenga información sobre cómo implementar clústeres de macrodatos de SQL Server en Kubernetes.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d97c5e9b8c508387298cf00a8fbbe96ded1a2eea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047165"
---
# <a name="how-to-deploy-big-data-clusters-2019-on-kubernetes"></a>Procedimientos para implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Un clúster de macrodatos de SQL Server se implementa como contenedores de Docker en un clúster de Kubernetes. Esta es una introducción a los pasos de instalación y configuración:

- Configure un clúster de Kubernetes en una sola máquina virtual, un clúster de máquinas virtuales, en Azure Kubernetes Service (AKS), Red Hat OpenShift o Red Hat OpenShift en Azure (ARO).
- Instale la herramienta de configuración de clúster [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] en el equipo cliente.
- Implemente un clúster de macrodatos de SQL Server en un clúster de Kubernetes.

## <a name="supported-platforms"></a>Plataformas compatibles

Vea [Plataformas admitidas](release-notes-big-data-cluster.md#supported-platforms) para obtener una lista completa de las distintas plataformas validadas por Kubernetes para implementar clústeres de macrodatos de SQL Server.

### <a name="sql-server-editions"></a>Ediciones de SQL Server

|Edición|Notas|
|---------|---------|
|Enterprise<br/>Estándar<br/>Desarrollador| La edición del clúster de macrodatos la determina la edición de la instancia maestra de SQL Server. En el momento de la implementación, se implementa de forma predeterminada la edición Developer. Puede cambiar la edición después de la implementación. Vea [Configuración de la instancia maestra de SQL Server](./configure-sql-server-master-instance.md). |

## <a name="kubernetes"></a><a id="prereqs"></a> Kubernetes

### <a name="kubernetes-cluster-setup"></a><a id="kubernetes"></a> Configuración del clúster de Kubernetes

Si ya tiene un clúster de Kubernetes que cumple los requisitos previos anteriores, puede ir directamente al [paso de implementación](#deploy). En esta sección se da por supuesto que tiene un conocimiento básico de los conceptos de Kubernetes.  Para obtener información detallada sobre Kubernetes, consulte la [documentación de Kubernetes](https://kubernetes.io/docs/home).

Puede optar por implementar Kubernetes de estas maneras:

| Implementar Kubernetes en: | Descripción | Vínculo |
|---|---|---|
| **Azure Kubernetes Services (AKS)** | Un servicio de contenedor de Kubernetes administrado en Azure. | [Instrucciones](deploy-on-aks.md) |
| **Una o varias máquinas (`kubeadm`)** | Un clúster de Kubernetes implementado en máquinas físicas o virtuales mediante `kubeadm` | [Instrucciones](deploy-with-kubeadm.md) |
|**Red Hat OpenShift en Azure** | Una oferta administrada de OpenShift que se ejecuta en Azure. | [Instrucciones](deploy-openshift.md)|
|**Red Hat OpenShift**|Una plataforma de aplicaciones de Kubernetes empresarial de nube híbrida.| [Instrucciones](deploy-openshift.md)|

> [!TIP]
> También puede crear un script de la implementación de AKS y un clúster de macrodatos en un único paso. Para obtener más información, vea cómo realizar este procedimiento en un [script de Python](quickstart-big-data-cluster-deploy.md) o en un [cuaderno](notebooks-deploy.md) de Azure Data Studio.

### <a name="verify-kubernetes-configuration"></a>Comprobación de la configuración de Kubernetes

Ejecute el comando `kubectl` para ver la configuración del clúster. Asegúrese de que kubectl apunta al contexto de clúster correcto.

```bash
kubectl config view
```

> [!Important] 
> Si va a implementar en un clúster de Kuberntes de varios nodos que ha arrancado mediante `kubeadm`, antes de iniciar la implementación del clúster de macrodatos, asegúrese de que los relojes estén sincronizados en todos los nodos de Kubernetes a los que se destina la implementación. El clúster de macrodatos tiene propiedades de estado integradas para varios servicios que son sensibles al tiempo y los sesgos de reloj pueden dar lugar a un estado incorrecto.

Después de haber configurado el clúster de Kubernetes, puede continuar con la implementación de un nuevo clúster de macrodatos de SQL Server. Si va a actualizar desde una versión anterior, vea [Procedimientos para actualizar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md).

## <a name="ensure-you-have-storage-configured"></a>Cómo asegurarse de que ha configurado el almacenamiento

La mayoría de las implementaciones de clúster de macrodatos deben tener almacenamiento persistente. En este momento, debe asegurarse de tener un plan sobre cómo va a proporcionar almacenamiento persistente en el clúster de Kubernetes antes de implementar el BDC.

Si implementa en AKS, no es necesario realizar ninguna configuración de almacenamiento. AKS proporciona clases de almacenamiento integradas con aprovisionamiento dinámico. Puede personalizar la clase de almacenamiento (`default` o `managed-premium`) en el archivo de configuración de implementación. Los perfiles integrados usan una clase de almacenamiento `default`. Si va a realizar la implementación en un clúster de Kubernetes que se ha implementado mediante `kubeadm`, deberá asegurarse de tener suficiente espacio de almacenamiento para un clúster de la escala deseada disponible y configurado para su uso. Si quiere personalizar la forma en que se usa el almacenamiento, debe hacerlo antes de continuar. Vea [Persistencia de los datos con un clúster de macrodatos de SQL Server en Kubernetes](concept-data-persistence.md).

## <a name="install-sql-server-2019-big-data-tools"></a>Instalación de las herramientas de macrodatos de SQL Server 2019

Antes de implementar un clúster de macrodatos de SQL Server 2019, [instale primero las herramientas de macrodatos](deploy-big-data-tools.md):

- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]
- `kubectl`
- Azure Data Studio
- [Extensión de virtualización de datos](../azure-data-studio/extensions/data-virtualization-extension.md) para Azure Data Studio


## <a name="deployment-overview"></a><a id="deploy"></a> Información general sobre la implementación

La mayoría de la configuración del clúster de macrodatos se define en un archivo de configuración de implementación JSON. Puede usar un perfil de implementación predeterminado para clústeres de AKS o Kubernetes creados con `kubeadm`, o bien puede personalizar un archivo de configuración de implementación propio para usarlo durante la instalación. Por motivos de seguridad, la configuración de autenticación se pasa mediante variables de entorno.

En las secciones siguientes se proporcionan más detalles sobre cómo configurar las implementaciones del clúster de macrodatos, así como ejemplos de personalizaciones comunes. Además, puede editar en todo momento el archivo de configuración de implementación personalizado con un editor (por ejemplo, VS Code).

## <a name="default-configurations"></a><a id="configfile"></a> Configuraciones predeterminadas

Las opciones de implementación del clúster de macrodatos se definen en archivos de configuración JSON. Puede iniciar la personalización de la implementación del clúster desde los perfiles de implementación integrados que están disponibles en [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. 

> [!NOTE]
> Las imágenes de contenedor necesarias para la implementación de clústeres de macrodatos se hospedan en el Registro de contenedor de Microsoft (`mcr.microsoft.com`) en el repositorio `mssql/bdc`. De forma predeterminada, esta configuración ya está incluida en el archivo de configuración `control.json` en cada uno de los perfiles de implementación que se incluyen con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Además, la etiqueta de imagen de contenedor de cada versión también se rellena previamente en el mismo archivo de configuración. Si necesita extraer las imágenes de contenedor en un registro de contenedor privado propio y modificar la configuración del repositorio o el registro de contenedores, siga las instrucciones del artículo [Instalación sin conexión](deploy-offline.md).

Ejecute este comando para encontrar las plantillas disponibles:

```
azdata bdc config list -o table 
```

Las plantillas siguientes están disponibles desde SQL Server 2019 CU5: 

| Perfil de implementación | Entorno de Kubernetes |
|---|---|
| `aks-dev-test` | Implementación de un clúster de macrodatos de SQL Server en Azure Kubernetes Service (AKS)|
| `aks-dev-test-ha` | Implemente un clúster de macrodatos de SQL Server en Azure Kubernetes Service (AKS). Los servicios críticos como la instancia maestra de SQL Server y el nodo de nombre de HDFS se configuran para lograr alta disponibilidad.|
| `aro-dev-test`|Implemente un clúster de macrodatos de SQL Server en Red Hat OpenShift en Azure para desarrollo y pruebas. <br/><br/>Introducido en SQL Server 2019 CU5.|
| `aro-dev-test-ha`|Implemente un clúster de macrodatos de SQL Server con alta disponibilidad en un clúster de Red Hat OpenShift para desarrollo y pruebas. <br/><br/>Introducido en SQL Server 2019 CU5.|
| `kubeadm-dev-test` | Implemente un clúster de macrodatos de SQL Server en un clúster de Kubernetes creado con kubeadm mediante una o varias máquinas virtuales o físicas.|
| `kubeadm-prod`| Implemente un clúster de macrodatos de SQL Server en un clúster de Kubernetes creado con kubeadm mediante una o varias máquinas virtuales o físicas. Use esta plantilla para permitir que los servicios de clúster de macrodatos se integren con Active Directory. Los servicios críticos como la instancia maestra de SQL Server y el nodo de nombre de HDFS se implementan en una configuración de alta disponibilidad.  |
| `openshift-dev-test`|Implemente un clúster de macrodatos de SQL Server en un clúster de Red Hat OpenShift para desarrollo y pruebas. <br/><br/>Introducido en SQL Server 2019 CU5.|
| `openshift-prod`|Implemente un clúster de macrodatos de SQL Server con alta disponibilidad en un clúster de Red Hat OpenShift. <br/><br/>Introducido en SQL Server 2019 CU5.|

Puede implementar un clúster de macrodatos mediante la ejecución de `azdata bdc create`. Esto le pedirá que elija una de las configuraciones predeterminadas y después le guiará por la implementación.

La primera vez que ejecute [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], debe incluir `--accept-eula=yes` para aceptar el contrato de licencia para el usuario final (CLUF).

```bash
azdata bdc create --accept-eula=yes
```

En este escenario, se le pide la configuración que no forme parte de la configuración predeterminada, como las contraseñas. 

> [!IMPORTANT]
> El nombre predeterminado del clúster de macrodatos es `mssql-cluster`. Es importante saberlo para ejecutar cualquiera de los comandos de `kubectl` que especifican el espacio de nombres de Kubernetes con el parámetro `-n`.

## <a name="custom-configurations"></a><a id="customconfig"></a> Configuraciones personalizadas

También es posible personalizar la implementación para acomodar las cargas de trabajo que planea ejecutar. No puede cambiar la escala (el número de réplicas) ni la configuración de almacenamiento para los servicios de clúster de macrodatos después de las implementaciones, por lo que debe planear cuidadosamente la configuración de implementación para evitar problemas de capacidad. Para personalizar la implementación, siga estos pasos:

1. Comience con uno de los perfiles de implementación estándar que coincidan con su entorno de Kubernetes. Puede usar el comando `azdata bdc config list` para enumerarlos:

   ```bash
   azdata bdc config list
   ```

1. Para personalizar la implementación, cree una copia del perfil de implementación con el comando `azdata bdc config init`. Por ejemplo, el comando siguiente crea una copia de los archivos de configuración de implementación de `aks-dev-test` en un directorio de destino denominado `custom`:

   ```bash
   azdata bdc config init --source aks-dev-test --target custom
   ```

   >[!TIP]
   >`--target` especifica un directorio que contiene los archivos de configuración, `bdc.json` y `control.json`, en función del parámetro `--source`.

1. Para personalizar la configuración del perfil de configuración de implementación, puede editar el archivo de configuración de implementación en una herramienta adecuada para editar archivos JSON, como VS Code. Para la automatización con scripts, también puede editar el perfil de implementación personalizado con el comando `azdata bdc config`. Por ejemplo, el comando siguiente modifica un perfil de implementación personalizado para cambiar el nombre del clúster implementado del valor predeterminado (`mssql-cluster`) a `test-cluster`:  

   ```bash
   azdata bdc config replace --config-file custom/bdc.json --json-values "metadata.name=test-cluster"
   ```

   > [!TIP]
   > También puede pasar el nombre del clúster en el momento de la implementación con el parámetro *--name* del comando *azdata create bdc*. Los parámetros del comando tienen prioridad sobre los valores de los archivos de configuración.
   >
   > Una herramienta útil para buscar rutas de acceso JSON es [JSONPath Online Evaluator](https://jsonpath.com/).
   >
   Además de pasar pares clave-valor, también puede proporcionar valores JSON insertados o pasar archivos de revisión JSON. Para obtener más información, vea [Configuración de opciones de implementación para clústeres de macrodatos](deployment-custom-configuration.md).

1. Pase el archivo de configuración personalizado a `azdata bdc create`. Tenga en cuenta que debe establecer las [variables de entorno](#env) necesarias; de lo contrario, el terminal le solicitará los valores:

   ```bash
   azdata bdc create --config-profile custom --accept-eula yes
   ```

> Para obtener más información sobre la estructura de un archivo de configuración de implementación, vea la [Referencia del archivo de configuración de implementación](reference-deployment-config.md). Para consultar más ejemplos de configuración, vea [Configuración de opciones de implementación para clústeres de macrodatos](deployment-custom-configuration.md).

## <a name="environment-variables"></a><a id="env"></a> Variables de entorno

Las siguientes variables de entorno se usan para la configuración de seguridad que no se almacena en un archivo de configuración de implementación. Tenga en cuenta que la configuración de Docker, excepto las credenciales, se puede establecer en el archivo de configuración.

| Variable de entorno | Requisito |Descripción |
|---|---|---|
| `AZDATA_USERNAME` | Obligatorio |El nombre de usuario para el administrador de clústeres de macrodatos de SQL Server. En la instancia maestra de SQL Server se crea un inicio de sesión de sysadmin con el mismo nombre. Como procedimiento de seguridad recomendado, la cuenta `sa` está deshabilitada. <br/><br/>[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]|
| `AZDATA_PASSWORD` | Obligatorio |La contraseña para la cuenta de usuario que se ha creado antes. En los clústeres implementados antes de SQL Server 2019 CU5, se usa la misma contraseña para el usuario `root`, a fin de proteger la puerta de enlace Knox y HDFS. |
| `ACCEPT_EULA`| Obligatorio para el primer uso de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]| Establecido en "Sí". Cuando se establece como una variable de entorno, aplica el CLUF a SQL Server y [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Si no se establece como variable de entorno, puede incluir `--accept-eula=yes` en el primer uso del comando [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].|
| `DOCKER_USERNAME` | Opcional | Nombre de usuario para acceder a las imágenes de contenedor en caso de que se almacenen en un repositorio privado. Consulte el tema [Implementaciones sin conexión](deploy-offline.md) para obtener más información sobre cómo usar un repositorio privado de Docker para la implementación del clúster de macrodatos.|
| `DOCKER_PASSWORD` | Opcional |Contraseña para acceder al repositorio privado anterior. |

Estas variables de entorno se deben establecer antes de llamar a `azdata bdc create`. Si no se establece ninguna variable, se le pedirá que lo haga.

En el ejemplo siguiente se muestra cómo establecer las variables de entorno para Linux (Bash) y Windows (PowerShell):

```bash
export AZDATA_USERNAME=admin
export AZDATA_PASSWORD=<password>
export ACCEPT_EULA=yes
```

```PowerShell
SET AZDATA_USERNAME=admin
SET AZDATA_PASSWORD=<password>
```

> [!NOTE]
> En los clústeres implementados antes de SQL Server 2019 CU5, debe utilizar el usuario `root` para la puerta de enlace Knox con la contraseña anterior. `root` es el único usuario que se admite en esta autenticación básica (nombre de usuario y contraseña).
> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]
> Para conectarse a SQL Server con autenticación básica, utilice los mismos valores que las [variables de entorno](#env) AZDATA_USERNAME y AZDATA_PASSWORD. 

Después de establecer las variables de entorno, debe ejecutar `azdata bdc create` para desencadenar la implementación. En este ejemplo se usa el perfil de configuración de clúster creado anteriormente:

```bash
azdata bdc create --config-profile custom --accept-eula yes
```

Tenga en cuenta las directrices siguientes:

- Asegúrese de incluir la contraseña entre comillas dobles si contiene algún carácter especial. Puede establecer `AZDATA_PASSWORD` en el valor que quiera, pero asegúrese de que la contraseña sea suficientemente compleja y no use los caracteres `!`, `&` ni `'`. Tenga en cuenta que los delimitadores de comillas dobles solo funcionan en los comandos de Bash.
- El inicio de sesión de `AZDATA_USERNAME` es un administrador del sistema en la instancia maestra de SQL Server que se crea durante la configuración. Después de crear el contenedor de SQL Server, la variable de entorno `AZDATA_PASSWORD` especificada se reconoce mediante la ejecución de `echo $AZDATA_PASSWORD` en el contenedor. Por motivos de seguridad, cambie la contraseña como procedimiento recomendado.

## <a name="unattended-install"></a><a id="unattended"></a> Instalación desatendida

En una implementación desatendida, debe establecer todas las variables de entorno necesarias, usar un archivo de configuración y llamar al comando `azdata bdc create` con el parámetro `--accept-eula yes`. En los ejemplos de la sección anterior se muestra la sintaxis de una instalación desatendida.

## <a name="monitor-the-deployment"></a><a id="monitor"></a> Supervisión de la implementación

Durante el arranque del clúster, la ventana de comandos del cliente devuelve el estado de la implementación. Durante el proceso de implementación, debería ver una serie de mensajes en los que está esperando el pod del controlador:

```output
Waiting for cluster controller to start.
```

Después de 15 a 30 minutos, se le notificará que el pod del controlador se está ejecutando:

```output
Cluster controller endpoint is available at 11.111.111.11:30080.
Cluster control plane is ready.
```

> [!IMPORTANT]
> La implementación completa puede tardar mucho tiempo debido al tiempo necesario para descargar las imágenes de contenedor de los componentes del clúster de macrodatos. Pero no debería tardar muchas horas. Si tiene problemas con la implementación, vea [Supervisión y solución de problemas de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

Cuando finalice la implementación, la salida le notificará que se ha realizado correctamente:

```output
Cluster deployed successfully.
```

> [!TIP]
> El nombre predeterminado del clúster de macrodatos implementado es `mssql-cluster` a menos que una configuración personalizada lo haya modificado.

## <a name="retrieve-endpoints"></a><a id="endpoints"></a> Recuperación de puntos de conexión

Una vez que el script de implementación se haya completado de forma correcta, puede seguir los pasos siguientes para obtener las direcciones de los puntos de conexión externos del clúster de macrodatos.

1. Después de la implementación, busque la dirección IP del punto de conexión del controlador desde la salida estándar de la implementación o mediante el examen de la salida de EXTERNAL-IP del comando `kubectl` siguiente:

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > Si no ha cambiado el nombre predeterminado durante la implementación, use `-n mssql-cluster` en el comando anterior. `mssql-cluster` es el nombre predeterminado del clúster de macrodatos.

1. Inicie sesión en el clúster de macrodatos con [azdata login](../azdata/reference/reference-azdata.md). Establezca el parámetro `--endpoint` en la dirección IP externa del punto de conexión del controlador.

   ```bash
   azdata login --endpoint https://<ip-address-of-controller-svc-external>:30080 --username <user-name>
   ```

   Especifique el nombre de usuario y la contraseña que ha configurado para el administrador del clúster de macrodatos (AZDATA_USERNAME y AZDATA_PASSWORD) durante la implementación.

   > [!TIP]
   > Si es el administrador del clúster de Kubernetes y tiene acceso al archivo de configuración del clúster (el archivo kube config), puede configurar el contexto actual para que apunte al clúster de Kubernetes de destino. En este caso, puede iniciar sesión con `azdata login -n <namespaceName>`, donde `namespace` es el nombre del clúster de macrodatos. Si no se especifican en el comando de inicio de sesión, se le solicitarán las credenciales.
   
1. Ejecute [azdata bdc endpoint list](../azdata/reference/reference-azdata-bdc-endpoint.md) para obtener una lista con una descripción de cada punto de conexión y los valores correspondientes de dirección IP y puerto. 

   ```bash
   azdata bdc endpoint list -o table
   ```

   En la lista siguiente, se muestra un resultado de ejemplo de este comando:

   ```output
   Description                                             Endpoint                                                   Ip              Name               Port    Protocol
   ------------------------------------------------------  ---------------------------------------------------------  --------------  -----------------  ------  ----------
   Gateway to access HDFS files, Spark                     https://11.111.111.111:30443                               11.111.111.111  gateway            30443   https
   Spark Jobs Management and Monitoring Dashboard          https://11.111.111.111:30443/gateway/default/sparkhistory  11.111.111.111  spark-history      30443   https
   Spark Diagnostics and Monitoring Dashboard              https://11.111.111.111:30443/gateway/default/yarn          11.111.111.111  yarn-ui            30443   https
   Application Proxy                                       https://11.111.111.111:30778                               11.111.111.111  app-proxy          30778   https
   Management Proxy                                        https://11.111.111.111:30777                               11.111.111.111  mgmtproxy          30777   https
   Log Search Dashboard                                    https://11.111.111.111:30777/kibana                        11.111.111.111  logsui             30777   https
   Metrics Dashboard                                       https://11.111.111.111:30777/grafana                       11.111.111.111  metricsui          30777   https
   Cluster Management Service                              https://11.111.111.111:30080                               11.111.111.111  controller         30080   https
   SQL Server Master Instance Front-End                    11.111.111.111,31433                                       11.111.111.111  sql-server-master  31433   tcp
   HDFS File System Proxy                                  https://11.111.111.111:30443/gateway/default/webhdfs/v1    11.111.111.111  webhdfs            30443   https
   Proxy for running Spark statements, jobs, applications  https://11.111.111.111:30443/gateway/default/livy/v1       11.111.111.111  livy               30443   https
   ```

También puede obtener todos los puntos de conexión de servicio implementados para el clúster si ejecuta el comando `kubectl` siguiente:

```bash
kubectl get svc -n <your-big-data-cluster-name>
```

## <a name="verify-the-cluster-status"></a><a id="status"></a> Comprobación del estado del clúster

Después de la implementación, puede comprobar el estado del clúster con el comando [azdata bdc status show](../azdata/reference/reference-azdata-bdc-status.md).

```bash
azdata bdc status show
```

> [!TIP]
> Para ejecutar los comandos de estado, primero debe iniciar sesión con el comando `azdata login`, que se ha mostrado en la sección sobre puntos de conexión anterior.

Este es un resultado de ejemplo de este comando:

```output
Bdc: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Services: ready                                                                                                                                                                                                     Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Servicename    State    Healthstatus    Details

 sql            ready    healthy         -
 hdfs           ready    healthy         -
 spark          ready    healthy         -
 control        ready    healthy         -
 gateway        ready    healthy         -
 app            ready    healthy         -


 Sql Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Hdfs Services: ready                                                                                                                                                                                                Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 nmnode-0        ready    healthy         StatefulSet nmnode-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
 sparkhead       ready    healthy         StatefulSet sparkhead is healthy


 Spark Services: ready                                                                                                                                                                                               Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 sparkhead       ready    healthy         StatefulSet sparkhead is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Control Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy


 Gateway Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 gateway         ready    healthy         StatefulSet gateway is healthy


 App Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 appproxy        ready    healthy         ReplicaSet appproxy is healthy
```

También puede obtener un estado más detallado con los comandos siguientes:

- [azdata bdc control status show](../azdata/reference/reference-azdata-bdc-control-status.md) devuelve el estado de mantenimiento de todos los componentes asociados al servicio de administración de control
```
azdata bdc control status show
```
Salida del ejemplo:
```output
Control: ready                                                                                                                                                                                                      Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy
```

- `azdata bdc sql status show` devuelve el estado de mantenimiento de todos los recursos que tienen un servicio de SQL Server
```
azdata bdc sql status show
```
Salida del ejemplo:
```output
Sql: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
```

> [!IMPORTANT]
> Cuando se usa el parámetro `--all`, la salida de estos comandos contiene direcciones URL a los paneles de Kibana y Grafana para un análisis más detallado.

Además de usar [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], también puede utilizar Azure Data Studio para buscar información de los puntos de conexión y el estado. Para obtener más información sobre cómo ver el estado del clúster con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] y Azure Data Studio, vea [Procedimientos para ver el estado de un clúster de macrodatos](view-cluster-status.md).

## <a name="connect-to-the-cluster"></a><a id="connect"></a> Conexión al clúster

Para obtener más información sobre cómo conectarse al clúster de macrodatos, consulte [Conexión a un clúster de macrodatos de SQL Server con Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la implementación del clúster de macrodatos, consulte los siguientes recursos:

- [Configuración de opciones de implementación para clústeres de macrodatos](deployment-custom-configuration.md)
- [Realización de una implementación sin conexión de un clúster de macrodatos de SQL Server](deploy-offline.md)
- [Taller: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)