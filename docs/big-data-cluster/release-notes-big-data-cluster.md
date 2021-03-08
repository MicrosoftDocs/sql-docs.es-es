---
title: Notas de la versión para clústeres de macrodatos de SQL Server
titleSuffix: SQL Server big data clusters
description: En este artículo se describen las actualizaciones más recientes y los problemas conocidos de los clústeres de macrodatos de SQL Server.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c4cb10ac3ba1e0fd8b437e7f0509dc16cc72d854
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837072"
---
# <a name="sql-server-2019-big-data-clusters-release-notes"></a>Notas de la versión para los Clústeres de macrodatos de SQL Server 2019

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Las notas de la versión siguientes se aplican a [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Este artículo se divide en secciones para cada versión. Cada versión tiene un vínculo a un artículo de asistencia en el que se describen los cambios de la CU, así como vínculos a las descargas de los paquetes de Linux. En el artículo también se enumeran los [problemas conocidos](#known-issues) para las versiones más recientes de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] (BDC).

## <a name="supported-platforms"></a>Plataformas compatibles

En esta sección se explican las plataformas compatibles con BDC.

### <a name="kubernetes-platforms"></a>Plataformas Kubernetes

|Plataforma|Versiones compatibles|
|---------|---------|
|Kubernetes (ascendente) estándar|Implemente BDC en el entorno local con un clúster de la versión 1.13 de Kubernetes como mínimo. Vea [Versión de Kubernetes y directiva de soporte de asimetría de versiones](https://kubernetes.io/docs/setup/release/version-skew-policy/).|
|Red Hat OpenShift|Implemente BDC en el entorno local con un clúster de la versión 4.3 de OpenShift como mínimo. Vea [Directiva de ciclo de vida de la plataforma de contenedores Red Hat OpenShift](https://access.redhat.com/support/policy/updates/openshift).<br><br> Compatibilidad introducida en SQL Server 2019 CU5.|
|Azure Kubernetes Service (AKS)|Implemente BDC en un clúster de la versión 1.13 de AKS como mínimo.<br/>Vea [Versiones de Kubernetes compatibles en Azure Kubernetes Service (AKS)](/azure/aks/supported-kubernetes-versions) para obtener la directiva de compatibilidad de versiones.|
|Red Hat OpenShift en Azure (ARO)|Implemente BDC en un clúster de la versión 4.3 de ARO como mínimo. Vea [Red Hat OpenShift en Azure](/azure/openshift/). <br><br> Compatibilidad introducida en SQL Server 2019 CU5.|

### <a name="host-os-for-kubernetes"></a>SO del host para Kubernetes

|Plataforma|Sistema operativo del host|Versiones compatibles|
|---------|---------|---------|
|Kubernetes|Ubuntu|16.04|
|Kubernetes|Red Hat Enterprise Linux|7.3, 7.4, 7.5, 7.6|
|OpenShift|Red Hat Enterprise Linux / CoreOS |Vea [Notas de la versión de OpenShift](https://docs.openshift.com/container-platform/4.3/release_notes/ocp-4-3-release-notes.html#ocp-4-3-about-this-release)|

### <a name="sql-server-editions"></a>Ediciones de SQL Server

|Edición|Notas|
|---------|---------|
|Enterprise<br/>Estándar<br/>Desarrollador| La edición del clúster de macrodatos la determina la edición de la instancia maestra de SQL Server. En el momento de la implementación, se implementa de forma predeterminada la edición Developer. Puede cambiar la edición después de la implementación. Vea [Configuración de la instancia maestra de SQL Server](./configure-sql-server-master-instance.md). |

## <a name="tools"></a>Herramientas

|Plataforma|Versiones compatibles|
|---------|---------|
|[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]|Como procedimiento recomendado, use la versión más reciente disponible. A partir de la versión SQL Server 2019 CU5, [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] tiene una versión semántica independiente del servidor. <br/><br/>Ejecute `azdata –-version` para validar la versión.<br/><br/>Vea [Historial de versiones](#release-history) para obtener la versión más reciente.|
|Azure Data Studio|Obtenga la versión más reciente de [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).|

Para obtener una lista completa, vea [¿Qué herramientas son necesarias?](deploy-big-data-tools.md#which-tools-are-required)

## <a name="release-history"></a>Historial de versiones

En la tabla siguiente, se muestra la lista del historial de versiones de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

| Versión <sup>1</sup> | Versión de BDC | [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] versión <sup>2</sup> | Fecha de la versión |
|--|--|--|--|
| [CU9](#cu9) |  15.0.4102.2 | 20.3.0    | 11-02-2021 |
| [CU8-GDR](#cu8-gdr) | 15.0.4083.2  | 20.2.6    | 2021-01-12 |
| [CU8](#cu8)     | 15.0.4073.23 | 20.2.2    | 19-10-2020 |
| [CU6](#cu6)     | 15.0.4053.23 | 20.0.1    | 04-08-2020 |
| [CU5](#cu5)     | 15.0.4043.16 | 20.0.0    | 22-06-2020 |
| [CU4](#cu4)     | 15.0.4033.1  | 15.0.4033 | 31-03-2020 |
| [CU3](#cu3)     | 15.0.4023.6  | 15.0.4023 | 12-03-2020 |
| [CU2](#cu2)     | 15.0.4013.40 | 15.0.4013 | 13-02-2020 |
| [CU1](#cu1)     | 15.0.4003.23 | 15.0.4003 | 07-01-2020 |
| [GDR1](#rtm)    | 15.0.2070.34 | 15.0.2070 | 2019-11-04 |

<sup>1</sup> CU7 no está disponible para el BDC.

La versión <sup>2</sup> de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] refleja la versión de la herramienta en el momento de la versión de CU. La versión de `azdata` también puede ser independientemente de la versión del servidor, por lo que podría obtener versiones más recientes al instalar los paquetes más recientes. Las versiones más recientes son compatibles con las versiones anteriores de CU.

## <a name="how-to-install-updates"></a>Instalación de las actualizaciones

Para instalar las actualizaciones, consulte [Cómo actualizar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md).

## <a name="cu9-february-2021"></a><a id="cu9"></a> CU 9 (febrero de 2021)

Versión de actualización acumulativa 9 (CU9) para SQL Server 2019.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4102.2|[2019-CU9-ubuntu-16.04]|

SQL Server 2019 CU9 para Clústeres de macrodatos de SQL Server incluye funcionalidades importantes:

- Compatibilidad para configurar BDC después de la implementación y proporcionar una mayor visibilidad de la configuración del sistema.

   Los clústeres que usan `mssql-conf` para las configuraciones de la instancia maestra de SQL Server requieren pasos adicionales después de la actualización a CU9. Siga las instrucciones que encontrará [aquí](bdc-upgrade-configuration.md).

- Experiencia de [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] mejorada para el cifrado en reposo.
- Capacidad para [instalar dinámicamente paquetes de Python y Spark](spark-install-packages.md) mediante entornos virtuales.
- Versiones de software actualizadas para la mayoría de nuestros componentes de OSS (Grafana, Kibana, FluentBit, etc.) para garantizar que las imágenes de BDC están actualizadas con las últimas mejoras y correcciones. Consulte el artículo [Referencia de software de código abierto](reference-open-source-software.md).
- Otras mejoras y correcciones de errores.

## <a name="cu8-gdrjanuary-2021"></a><a id="cu8-gdr"></a> CU8-GDR (enero de 2021)

Versión de la actualización acumulativa 8 GDR (CU8-GDR) para SQL Server 2019.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4083.2 |[2019-CU8-GDR2-ubuntu-16.04]|

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (septiembre de 2020)

Versión de actualización acumulativa 8 (CU8) para SQL Server 2019.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4073.23 |[2019-CU8-ubuntu-16.04]

Esta versión incluye varias correcciones y un par de mejoras.

### <a name="added-capabilities"></a>Funcionalidades agregadas

- [Cifrado en reposo de Clústeres de macrodatos de SQL Server](encryption-at-rest-concepts-and-configuration.md) mediante el uso de certificados y claves administrados por el sistema.
   > [!CAUTION]
   > Esta es la versión inicial del cifrado en reposo del BDC de SQL Server. Consulte los artículos siguientes: 
   > - [Conceptos de seguridad para [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](concept-security.md)
   > - [Conceptos y guía de configuración del cifrado en reposo](encryption-at-rest-concepts-and-configuration.md)
- Compatibilidad del [usuario de proxy de Oracle](tutorial-query-oracle.md) con el escenario de virtualización de datos.

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (julio de 2020)

Versión de actualización acumulativa 6 (CU6) para SQL Server 2019.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4053.23 |[2019-CU6-ubuntu-16.04]

Esta versión incluye mejoras y correcciones menores. En los siguientes artículos se incluye información relacionada con estas actualizaciones:

- [Administración del acceso al clúster de macrodatos en el modo de Active Directory](manage-user-access.md)
- [Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en el modo de Active Directory](active-directory-deploy.md)
- [Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en AKS en el modo de Active Directory](active-directory-deployment-aks.md)
- [Implementación de clústeres de macrodatos con el clúster privado de Azure Kubernetes Service (AKS)](private-deploy.md)
- [Restricción del tráfico de salida de clústeres de macrodatos en el clúster privado de Azure Kubernetes Service (AKS)](private-restrict-egress-traffic.md)
- [Implementación de clústeres de macrodatos de SQL Server con alta disponibilidad](deployment-high-availability.md)
- [Configuración de un clúster de macrodatos de SQL Server](./configure-bdc-overview.md)
- [Configuración de Apache Spark y Apache Hadoop en clústeres de macrodatos](configure-spark-hdfs.md)
- [Propiedades de configuración de la instancia maestra de SQL Server](reference-config-master-instance.md)
- [Propiedades de configuración de Apache Spark y Apache Hadoop (HDFS)](reference-config-spark-hadoop.md)
- [Modelo e impacto de RBAC de Kubernetes en los usuarios y las cuentas de servicio que administran BDC](kubernetes-rbac.md)

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (junio de 2020)

Versión de actualización acumulativa 5 (CU5) para SQL Server 2019.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4043.16 |[2019-CU5-ubuntu-16.04]

### <a name="added-capabilities"></a>Funcionalidades agregadas

- Compatibilidad con la implementación de clústeres de macrodatos en Red Hat OpenShift. La compatibilidad incluye la plataforma de contenedores OpenShift implementada en la versión local 4.3 y superiores, y Red Hat OpenShift en Azure. Vea [Implementación de clústeres de macrodatos de SQL Server en OpenShift](deploy-openshift.md).
- Se ha actualizado el modelo de seguridad de implementación de BDC, de modo que los contenedores con privilegios implementados como parte de BDC ya no son *necesarios*. Además de que ya no necesiten privilegios, los contenedores se ejecutan como un usuario que no es de raíz de forma predeterminada para todas las implementaciones nuevas mediante SQL Server 2019 CU5. 
- Se ha agregado compatibilidad para implementar varios clústeres de macrodatos en un dominio de Active Directory.
- La CLI de Azure [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] tiene su propia versión semántica, independiente del servidor. Se quitan todas las dependencias entre el cliente y la versión del servidor de azdata. Se recomienda usar la versión más reciente del cliente y del servidor para asegurarse de que se beneficia de las últimas mejoras y correcciones.
- Se han introducido dos nuevos procedimientos almacenados, sp_data_source_objects y sp_data_source_table_columns, para admitir la introspección de determinados orígenes de datos externos. Los clientes pueden usarlos directamente a través de T-SQL para la detección de esquemas y ver qué tablas están disponibles para su virtualización. Estos cambios se aprovechan en el Asistente para tablas externas de la [extensión Virtualización de datos](../azure-data-studio/extensions/data-virtualization-extension.md) para Azure Data Studio, lo que permite crear tablas externas a partir de SQL Server, Oracle, MongoDB y Teradata.
- Se ha agregado compatibilidad para conservar las personalizaciones realizadas en Grafana. Antes de CU5, los clientes observaban que las modificaciones en las configuraciones de Grafana se perdían al reiniciar el pod `metricsui` (en el que se hospeda el panel de Grafana). Este problema se ha corregido y ahora se conservan todas las configuraciones. 
- Se ha corregido un problema de seguridad relacionado con la API que se usa para recopilar métricas de pods y nodos mediante Telegraf (que se hospeda en los pods `metricsdc`). Como resultado de este cambio, ahora en Telegraf es necesario que una cuenta de servicio, el rol de clúster y los enlaces de clúster tengan los permisos necesarios para recopilar las métricas de pods y nodos. Vea [Rol de clúster necesario para la recopilación de métricas de pods y nodos](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection) para obtener más información.
- Se han agregado dos modificadores de características para controlar la recopilación de métricas de pods y nodos. En caso de que use otras soluciones para la supervisión de la infraestructura de Kubernetes, puede desactivar la recopilación de métricas integradas para pods y nodos de host si establece *allowNodeMetricsCollection* y *allowPodMetricsCollection* en false en el archivo de configuración de implementación control.json. Para los entornos de OpenShift, estos valores se establecen en false de forma predeterminada en los perfiles de implementación integrados, ya que la recopilación de métricas de pods y nodos requiere funciones con privilegios.

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (abril de 2020)

Versión de actualización acumulativa 4 (CU4) para SQL Server 2019. La versión del Motor de base de datos de SQL Server es la 15.0.4033.1.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4033.1 |[2019-CU4-ubuntu-16.04]

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (marzo de 2020)

Versión de actualización acumulativa 3 (CU3) para SQL Server 2019. La versión de Motor de base de datos de SQL Server de esta versión es la 15.0.4023.6.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4023.6 |[2019-CU3-ubuntu-16.04]

### <a name="resolved-issues"></a>Problemas resueltos

En SQL Server 2019 CU3 se resuelven los problemas siguientes de las versiones anteriores.

- [Implementación con repositorio privado](#deployment-with-private-repository)
- [Puede generarse un error de actualización debido a un tiempo de espera](#upgrade-may-fail-due-to-timeout)

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU2 (febrero de 2020)

Versión de actualización acumulativa 2 (CU2) para SQL Server 2019. La versión de Motor de base de datos de SQL Server de esta versión es la 15.0.4013.40.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4013.40 |[2019-CU2-ubuntu-16.04]

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 (enero de 2020)

Versión de actualización acumulativa 1 (CU1) para SQL Server 2019. La versión de Motor de base de datos de SQL Server de esta versión es la 15.0.4003.23.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.4003.23|[2019-CU1-ubuntu-16.04]

## <a name="gdr1-november-2019"></a><a id="rtm"></a> GDR1 (noviembre de 2019)

Versión de distribución general 1 (GDR1) de SQL Server 2019, presenta la disponibilidad general para [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-nover.md)]. La versión de Motor de base de datos de SQL Server de esta versión es la 15.0.2070.34.

|Versión del paquete | Etiqueta de imagen |
|-----|-----|
|15.0.2070.34|[2019-GDR1-ubuntu-16.04]

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="known-issues"></a>Problemas conocidos

### <a name="partial-loss-of-logs-collected-in-elasticsearch-upon-rollback"></a>Pérdida parcial de registros recopilados en ElasticSearch tras la reversión

- **Versiones afectadas**: clústeres existentes cuando una actualización a CU9 con error provoca una reversión, o bien el usuario emite una degradación a una versión anterior.

- **Problema e impacto en el cliente**: la versión de software que se usa para la búsqueda elástica se ha actualizado con CU9, y la nueva versión no es compatible con versiones anteriores con los metadatos y el formato de los registros anteriores. Si el componente ElasticSearch se actualiza correctamente, pero se desencadena una reversión posterior, los registros recopilados entre la actualización de ElasticSearch y la reversión se perderán de forma permanente. Si emite un cambio a una versión anterior de BDC (lo que no se recomienda), se perderán los registros almacenados en Elasticsearch. Tenga en cuenta que, si el usuario vuelve a realizar la actualización a CU9, los datos se restaurarán.

- **Solución alternativa**: si es necesario, puede solucionar el problema mediante registros recopilados con el comando `azdata bdc debug copy-logs`.

### <a name="missing-pods-and-container-metrics"></a>Pods y métricas de contenedor no disponibles

- **Versiones afectadas**: clústeres nuevos y existentes tras la actualización a CU9

- **Problema e impacto en el cliente**: Como resultado de la actualización de la versión de Telegraf que se usa para los componentes de supervisión de BDC en CU9, al actualizar el clúster a la versión CU9, observará que no se recopilan los pods ni las métricas de contenedor. Esto se debe a que se necesita un recurso adicional en la definición del rol de clúster que se usa para Telegraf como resultado de la actualización de software. Si el usuario que implementa el clúster o realiza la actualización no tiene permisos suficientes, la implementación o la actualización continúa con una advertencia y se realiza correctamente, pero no se recopilarán las métricas de nodos ni los pods.

- **Solución alternativa**: puede pedirle a un administrador que cree o actualice el rol, y la cuenta de servicio correspondiente (antes o después de la implementación o actualización), para que se usen en BDC. En [este artículo](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection) se describe cómo crear los artefactos necesarios.

### <a name="issuing-azdata-bdc-copy-logs-does-not-result-in-logs-being-copied"></a>Registros no copiados tras la emisión de `azdata bdc copy-logs`

- **Versiones afectadas**: [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] versión *20.0.0*

- **Problema e impacto en el cliente**: la implementación del comando *copy-logs* asume que la versión 1.15 o posterior de la herramienta cliente `kubectl` está instalada en el equipo cliente desde el que se emite el comando. Si se usa la versión 1.14 de `kubectl`, el comando *azdata bdc debug copy-logs* se completará sin errores, pero no se copiarán los registros. Cuando se ejecuta con la marca *--debug*, puede ver este error en la salida: *el origen "." no es válido*.

- **Solución alternativa**: instale la versión 1.15 o posterior de la herramienta `kubectl` en el mismo equipo cliente y vuelva a emitir el comando `azdata bdc copy-logs`. Vea [estas](deploy-big-data-tools.md) instrucciones sobre cómo instalar `kubectl`.

### <a name="msdtc-capabilities-can-not-be-enabled-for-sql-server-master-instance-running-within-bdc"></a>Imposibilidad de habilitar las funcionalidades de MSDTC para la instancia maestra de SQL Server que se ejecuta en BDC

- **Versiones afectadas**: todas las configuraciones de implementación de clúster de macrodatos, independientemente de la versión.

- **Problema e impacto en el cliente**: con SQL Server implementado en BDC como instancia maestra de SQL Server, no se puede habilitar la característica MSDTC. No hay ninguna solución alternativa para este problema.

### <a name="ha-sql-server-database-encryption-key-encryptor-rotation"></a>Rotación del tipo de sistema de cifrado de claves de las claves de cifrado de bases de datos SQL Server de alta disponibilidad

- **Versiones afectadas**: todas las versiones hasta CU8. Resuelto para CU9.

- **Problema e impacto en el cliente**: con SQL Server implementado con alta disponibilidad, se produce un error en la rotación de certificados para la base de datos cifrada. Cuando se ejecute el comando siguiente en el grupo maestro, aparecerá un mensaje de error:
    ```
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER
    CERTIFICATE <NewCertificateName>
    ```
    No hay ningún impacto, se produce un error en el comando y el cifrado de la base de datos de destino se conserva con el certificado anterior.

### <a name="enabling-hdfs-encryption-zones-support-on-cu8"></a>Habilitación de la compatibilidad con las zonas de cifrado de HDFS en CU8

- **Versiones afectadas**: este escenario se muestra al realizar la actualización específicamente a la versión CU8 desde CU6 o una versión anterior. Esto no sucederá en las nuevas implementaciones de CU8 y versiones posteriores o al hacerlo directamente a CU9.

- **Problema e impacto en el cliente**: la compatibilidad con las zonas de cifrado de HDFS no está habilitada de forma predeterminada en este escenario y debe configurarse mediante los pasos proporcionados en la [guía de configuración](encryption-at-rest-concepts-and-configuration.md).

### <a name="empty-livy-jobs-before-you-apply-cumulative-updates"></a>Trabajos de Livy vacíos antes de aplicar las actualizaciones acumulativas

- **Versiones afectadas**: todas las versiones hasta CU6. Resuelto para CU8.

- **Problema e impacto en el cliente**: Durante una actualización, `sparkhead` devuelve un error 404.

- **Solución alternativa**: antes de actualizar BDC, asegúrese de que no haya trabajos por lotes o sesiones de Livy activos. Siga las instrucciones de [Actualización desde una versión compatible](deployment-upgrade.md#upgrade-from-supported-release) para evitar esto. 

   Si Livy devuelve un error 404 durante el proceso de actualización, reinicie el servidor Livy en ambos nodos `sparkhead`. Por ejemplo:

   ```console
   kubectl -n <clustername> exec -it sparkhead-0/sparkhead-1 -c hadoop-livy-sparkhistory -- exec supervisorctl restart livy
   ```

### <a name="big-data-cluster-generated-service-accounts-passwords-expiration"></a>Expiración de contraseñas de cuentas de servicio generadas por el clúster de macrodatos

- **Versiones afectadas**: todas las implementaciones de clústeres de macrodatos con la integración de Active Directory, independientemente de la versión.

- **Problema e impacto en el cliente**: durante la implementación del clúster de macrodatos, el flujo de trabajo genera un conjunto de [cuentas de servicio](active-directory-objects.md). En función de la directiva de expiración de contraseñas establecida en el controlador de dominio, las contraseñas de estas cuentas pueden expirar (el tiempo predeterminado es 42 días). En este momento, no hay ningún mecanismo para rotar las credenciales para todas las cuentas del clúster de macrodatos, por lo que el clúster dejará de funcionar cuando se cumpla el período de expiración.

- **Solución alternativa**: actualice la directiva de expiración de las cuentas de servicio del clúster de macrodatos a "La contraseña nunca expira" en el controlador de dominio. Para obtener una lista completa de estas cuentas, vea [Objetos de Active Directory generados automáticamente](active-directory-objects.md). Esta acción se puede realizar antes o después de la fecha de expiración. En el último caso, Active Directory reactivará las contraseñas expiradas.

### <a name="credentials-for-accessing-services-through-gateway-endpoint"></a>Credenciales para acceder a los servicios a través del punto de conexión de puerta de enlace

- **Versiones afectadas**: clústeres nuevos implementados a partir de CU5.

- **Problema e impacto en el cliente**: para los nuevos clústeres de macrodatos implementados con SQL Server 2019 CU5, el nombre de usuario de puerta de enlace no es **root** (raíz). Si la aplicación que se usa para conectarse al punto de conexión de puerta de enlace usa las credenciales incorrectas, verá un error de autenticación. Este cambio es el resultado de la ejecución de aplicaciones en el clúster de macrodatos como un usuario distinto de root (un nuevo comportamiento predeterminado a partir de la versión SQL Server 2019 CU5; cuando se implementa un nuevo clúster de macrodatos con CU5, el nombre de usuario del punto de conexión de puerta de enlace se basa en el valor que se pasa a través de la variable de entorno **AZDATA_USERNAME**). Es el mismo nombre de usuario que se usa para los puntos de conexión del controlador y SQL Server. Esto solo afecta a las implementaciones nuevas; en los clústeres de macrodatos existentes implementados con cualquiera de las versiones anteriores se seguirá usando **root**. No se produce ningún impacto en las credenciales cuando el clúster se implementa para usar la autenticación de Active Directory. 

- **Solución alternativa**: Azure Data Studio controlará de forma transparente el cambio de credenciales para la conexión realizada a la puerta de enlace a fin de habilitar la experiencia de exploración de HDFS en el Explorador de objetos. Debe instalar la [versión más reciente Azure Data Studio](../azure-data-studio/download-azure-data-studio.md) que incluya los cambios necesarios que solucionan este caso de uso.
Para otros escenarios en los que debe proporcionar credenciales para acceder al servicio a través de la puerta de enlace (por ejemplo, el inicio de sesión con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], el acceso a los paneles web de Spark), tendrá que asegurarse de que se usen las credenciales correctas. Si el destino es un clúster existente implementado antes de CU5, seguirá usando el nombre de usuario **root** para conectarse a la puerta de enlace, incluso después de actualizar el clúster a CU5. Si implementa un nuevo clúster mediante la compilación CU5, inicie sesión con el nombre de usuario correspondiente a la variable de entorno **AZDATA_USERNAME**.

### <a name="pods-and-nodes-metrics-not-being-collected"></a>Métricas de pods y nodos no recopiladas

- **Versiones afectadas**: clústeres nuevos y existentes en los que se usan imágenes de CU5

- **Problema e impacto en el cliente**: como resultado de una corrección de seguridad relacionada con la API que `telegraf` usaba para recopilar métricas de pods y nodo host, es posible que los clientes vean que no se recopilan métricas. Esto es posible en implementaciones nuevas y existentes de BDC (después de la actualización a CU5). Como resultado de la corrección, ahora en Telegraf se necesita una cuenta de servicio con permisos de rol en la totalidad del clúster. La implementación intenta crear la cuenta de servicio y el rol de clúster necesarios, pero si el usuario que implementa el clúster o realiza la actualización no tiene permisos suficientes, la implementación o la actualización continúa con una advertencia y se realiza correctamente, pero no se recopilarán las métricas de nodos y pods.

- **Solución alternativa**: puede pedirle a un administrador que cree el rol y la cuenta de servicio (antes o después de la implementación o actualización), para que se usen en BDC. En [este artículo](kubernetes-rbac.md#cluster-role-required-for-pods-and-nodes-metrics-collection) se describe cómo crear los artefactos necesarios.

### <a name="azdata-bdc-copy-logs-command-failure"></a>Error del comando `azdata bdc copy-logs`

- **Versiones afectadas**: [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] versión *20.0.0*

- **Problema e impacto en el cliente**: la implementación del comando *copy-logs* asume que la herramienta cliente `kubectl` está instalada en el equipo cliente desde el que se emite el comando. Si emite el comando en un clúster de BDC instalado en OpenShift, desde un cliente donde solo está instalada la herramienta `oc`, obtendrá un error: *Se produjo un error al recopilar los registros: [WinError 2] El sistema no encuentra el archivo especificado.* .

- **Solución alternativa**: instale la herramienta `kubectl` en el mismo equipo cliente y vuelva a emitir el comando `azdata bdc copy-logs`. Vea [estas](deploy-big-data-tools.md) instrucciones sobre cómo instalar `kubectl`.

### <a name="deployment-with-private-repository"></a>Implementación con repositorio privado

- **Versiones afectadas**: GDR1, CU1, CU2. Resuelto para CU3.

- **Problema e impacto en el cliente**: La actualización del repositorio privado tiene requisitos específicos

- **Solución alternativa**: si usa un repositorio privado para extraer previamente las imágenes para implementar o actualizar BDC, asegúrese de que las imágenes de compilación actuales, así como las imágenes de compilación de destino se encuentran en el repositorio privado. Esto permite una reversión correcta, si es necesario. Además, si cambió las credenciales del repositorio privado desde la implementación original, actualice el secreto correspondiente en Kubernetes antes de la actualización. [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] no admite la actualización de las credenciales a través de las variables de entorno `AZDATA_PASSWORD` y `AZDATA_USERNAME`. Actualice el secreto mediante [`kubectl edit secrets`](https://kubernetes.io/docs/concepts/configuration/secret/#editing-a-secret). 

No se admite la actualización mediante distintos repositorios privados para compilaciones actuales y de destino.

### <a name="upgrade-may-fail-due-to-timeout"></a>Puede generarse un error de actualización debido a un tiempo de espera

- **Versiones afectadas**: GDR1, CU1, CU2. Resuelto para CU3.

- **Problema e impacto en el cliente**: Puede generarse un error de actualización debido a un tiempo de espera.

   En el código siguiente se muestra el aspecto del error:

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

   Es más probable que este error se produzca al actualizar BDC en Azure Kubernetes Service (AKS).

- **Solución alternativa**: aumente el tiempo de espera de la actualización. 

   Para aumentar los tiempos de espera de una actualización, edite la asignación de configuración de la actualización. Para editar la asignación de configuración de la actualización:

   1. Ejecute el siguiente comando:

      ```bash
      kubectl edit configmap controller-upgrade-configmap
      ```

   2. Edite estos campos:

       **`controllerUpgradeTimeoutInMinutes`** designa el número de minutos que se esperará a que finalice la actualización del controlador o de la base de datos del controlador. El valor predeterminado es 5. Actualice al menos a 20.

       **`totalUpgradeTimeoutInMinutes`** : designa la cantidad de tiempo para que el controlador y la base de datos del controlador terminen de actualizarse (actualización `controller` + `controllerdb`). El valor predeterminado es 10. Actualice al menos a 40.

       **`componentUpgradeTimeoutInMinutes`** : designa la cantidad de tiempo en que se debe completar cada fase posterior de la actualización. El valor predeterminado es 30. Actualice a 45.

   3. Guarde y salga.

   El script de Python siguiente es otra manera de establecer el tiempo de espera:

   ```python
   from kubernetes import client, config
   import json

   def set_upgrade_timeouts(namespace, controller_timeout=20, controller_total_timeout=40, component_timeout=45):
       """ Set the timeouts for upgrades

       The timeout settings are as follows

       controllerUpgradeTimeoutInMinutes: sets the max amount of time for the controller
           or controllerdb to finish upgrading

       totalUpgradeTimeoutInMinutes: sets the max amount of time to wait for both the
           controller and controllerdb to complete their upgrade

       componentUpgradeTimeoutInMinutes: sets the max amount of time allowed for
           subsequent phases of the upgrade to complete
       """
       config.load_kube_config()

       upgrade_config_map = client.CoreV1Api().read_namespaced_config_map("controller-upgrade-configmap", namespace)

       upgrade_config = json.loads(upgrade_config_map.data["controller-upgrade"])

       upgrade_config["controllerUpgradeTimeoutInMinutes"] = controller_timeout

       upgrade_config["totalUpgradeTimeoutInMinutes"] = controller_total_timeout

       upgrade_config["componentUpgradeTimeoutInMinutes"] = component_timeout

       upgrade_config_map.data["controller-upgrade"] = json.dumps(upgrade_config)

       client.CoreV1Api().patch_namespaced_config_map("controller-upgrade-configmap", namespace, upgrade_config_map)
   ```

### <a name="livy-job-submission-from-azure-data-studio-ads-or-curl-fail-with-500-error"></a>Error 500 al enviar el trabajo de Livy desde Azure Data Studio (ADS) o curl

- **Problema e impacto en el cliente**: en una configuración de alta disponibilidad, los recursos compartidos de Spark `sparkhead` se configuran con varias réplicas. En este caso, es posible que experimente errores con el envío del trabajo de Livy desde Azure Data Studio (ADS) o `curl`. Para comprobarlo, la ejecución de `curl` en cualquier pod de `sparkhead` da como resultado una conexión rechazada. Por ejemplo, `curl https://sparkhead-0:8998/` o `curl https://sparkhead-1:8998` devuelve el error 500.

   Esto sucede en los escenarios siguientes:

   - Los pods de Zookeeper o los procesos de cada instancia de Zookeeper se reinician varias veces.
   - Cuando la conectividad de red no es confiable entre el pod de `sparkhead` y los pods de Zookeeper.

- **Solución alternativa**: reinicie los dos servidores de Livy.

   ```bash
   kubectl -n <clustername> exec sparkhead-0 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

   ```bash
   kubectl -n <clustername> exec sparkhead-1 -c hadoop-livy-sparkhistory supervisorctl restart livy
   ```

### <a name="create-memory-optimized-table-when-master-instance-in-an-availability-group"></a>Creación de una tabla optimizada para memoria cuando la instancia maestra está en un grupo de disponibilidad

- **Problema e impacto en el cliente**: no se puede usar el punto de conexión principal expuesto para conectarse a las bases de datos del grupo de disponibilidad (agente de escucha) para crear tablas optimizadas para memoria.

- **Solución alternativa**: para crear tablas optimizadas para memoria cuando la instancia maestra de SQL Server es una configuración de grupo de disponibilidad, [conéctese a la instancia de SQL Server](deployment-high-availability.md#instance-connect), exponga un punto de conexión, conéctese a la base de datos SQL Server y cree las tablas optimizadas para memoria en la sesión creada con la nueva conexión.

### <a name="insert-to-external-tables-active-directory-authentication-mode"></a>Inserción en el modo de autenticación de Active Directory en tablas externas

- **Problema e impacto en el cliente**: cuando la instancia maestra de SQL Server está en el modo de autenticación Active Directory, una consulta que selecciona solo en tablas externas, donde al menos una está en un grupo de almacenamiento, e inserta en otra tabla externa, devuelve lo siguiente:

   ```
   Msg 7320, Level 16, State 102, Line 1
   Cannot execute the query "Remote Query" against OLE DB provider "SQLNCLI11" for linked server "SQLNCLI11". Only domain logins can be used to query Kerberized storage pool.
   ```

- **Solución alternativa**: modifique la consulta de una de las maneras siguientes. Puede unir la tabla de bloque de almacenamiento a una tabla local, o bien insertar primero en la tabla local y después leer desde la tabla local para insertar en el grupo de datos.

### <a name="transparent-data-encryption-capabilities-can-not-be-used-with-databases-that-are-part-of-the-availability-group-in-the-sql-server-master-instance"></a>Las funcionalidades de Cifrado de datos transparente no se pueden usar con las bases de datos que forman parte del grupo de disponibilidad en la instancia maestra de SQL Server.

- **Problema e impacto en el cliente**: en una configuración de alta disponibilidad, las bases de datos que tienen habilitado el cifrado no se pueden usar después de una conmutación por error, ya que la clave maestra usada para el cifrado es distinta en cada réplica. 

- **Solución alternativa**: No hay ninguna solución alternativa para este problema. Se recomienda no habilitar el cifrado en esta configuración hasta que se realice una corrección.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md)?