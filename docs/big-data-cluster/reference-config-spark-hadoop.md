---
title: Propiedades de configuración de Apache Hadoop y Apache Spark
titleSuffix: SQL Server big data clusters
description: Artículo de referencia de las propiedades de configuración de Apache Spark y Apache Hadoop (HDFS).
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 08/04/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c6a73d8cf4a110990260db4917d565c33bd59766
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514892"
---
# <a name="apache-spark--apache-hadoop-hdfs-configuration-properties"></a>Propiedades de configuración de Apache Spark y Apache Hadoop (HDFS)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

La plataforma Clústeres de macrodatos admite la configuración en tiempo de implementación y posterior a la implementación de componentes de Hadoop y Apache Spark en el ámbito del servicio y el ámbito del recurso. Los Clústeres de macrodatos usan los mismos valores predeterminados de configuración que el proyecto de código abierto respectivo para la mayoría de las configuraciones. A continuación, se muestran los valores que sí se cambian, junto con una descripción y el valor predeterminado. Aparte del recurso de puerta de enlace, no hay ninguna diferencia entre los valores que se pueden configurar en el ámbito del servicio y el ámbito del recurso.

Puede encontrar todas las configuraciones posibles y los valores predeterminados de cada una en el sitio de documentación de Apache asociado:
- Apache Spark: https://spark.apache.org/docs/latest/configuration.html
- Apache Hadoop:
  - HDFS HDFS-Site: https://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml
  - HDFS Core-Site: https://hadoop.apache.org/docs/r2.8.0/hadoop-project-dist/hadoop-common/core-default.xml  
  - Yarn: https://hadoop.apache.org/docs/r3.1.1/hadoop-yarn/hadoop-yarn-site/ResourceModel.html
- Hive: https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties#ConfigurationProperties-MetaStore
- Livy: https://github.com/cloudera/livy/blob/master/conf/livy.conf.template
- Puerta de enlace de Apache Knox: https://knox.apache.org/books/knox-0-14-0/user-guide.html#Gateway+Details

A continuación, también se enumeran las opciones de configuración que no se admiten.

> [!NOTE]
> Para incluir Spark en el bloque de almacenamiento, establezca el valor booleano `includeSpark` del archivo de configuración `bdc.json` en `spec.resources.storage-0.spec.settings.spark`. Consulte [Configuración de Apache Spark y Apache Hadoop en clústeres de macrodatos](configure-spark-hdfs.md).


##  <a name="big-data-clusters-specific-default-spark-settings"></a>Configuración predeterminada de Spark específica para Clústeres de macrodatos
La configuración de Spark que se muestra a continuación son los valores predeterminados específicos de BDC, pero que el usuario puede configurar. No se incluyen las configuraciones administradas por el sistema.

| Nombre de la opción de configuración                                                                                 | Descripción                                                                                                                                                           | Tipo   | Valor predeterminado                                                                                                                              |
| ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| capacity-scheduler.yarn.scheduler.capacity.maximum-applications                      | Número máximo de aplicaciones en el sistema que pueden estar activas de manera simultánea, tanto en ejecución como pendientes.                                                               | int    | 10000                                                                                                                                      |
| capacity-scheduler.yarn.scheduler.capacity.resource-calculator                       | Implementación de ResourceCalculator que se va a usar para comparar los recursos del programador.                                                                               | string | org.apache.hadoop.yarn.util.resource.DominantResourceCalculator                                                                            |
| capacity-scheduler.yarn.scheduler.capacity.root.queues                               | Programador de capacidad con cola predefinida denominada raíz.                                                                                                              | string | default                                                                                                                                    |
| capacity-scheduler.yarn.scheduler.capacity.root.default.capacity                     | Capacidad de cola en porcentaje (%) como capacidad mínima de cola de recursos absoluta para la cola raíz.                                                                          | int    | 100                                                                                                                                        |
| spark-defaults-conf.spark.driver.cores                                               | Número de núcleos que se van a usar para el proceso de controlador, solo en modo de clúster.                                                                                                  | int    | 1                                                                                                                                          |
| spark-defaults-conf.spark.driver.memoryOverhead                                      | Cantidad de memoria fuera del montón que se va a asignar por controlador en modo de clúster.                                                                                             | int    | 384                                                                                                                                        |
| spark-defaults-conf.spark.executor.instances                                         | Número de ejecutores para la asignación estática.                                                                                                                        | int    | 1                                                                                                                                          |
| spark-defaults-conf.spark.executor.cores                                             | Número de núcleos que se usarán para cada ejecutor.                                                                                                                          | int    | 1                                                                                                                                          |
| spark-defaults-conf.spark.driver.memory                                              | Cantidad de memoria que se usará para el proceso del controlador.                                                                                                                       | string | 1g                                                                                                                                         |
| spark-defaults-conf.spark.executor.memory                                            | Cantidad de memoria que se usará por proceso de ejecutor.                                                                                                                         | string | 1g                                                                                                                                         |
| spark-defaults-conf.spark.executor.memoryOverhead                                    | Cantidad de memoria fuera del montón que se va a asignar por ejecutor.                                                                                                           | int    | 384                                                                                                                                        |
| yarn-site.yarn.nodemanager.resource.memory-mb                                        | Cantidad de memoria física, en MB, que se puede asignar para los contenedores.                                                                                               | int    | 8192                                                                                                                                       |
| yarn-site.yarn.scheduler.maximum-allocation-mb                                       | Asignación máxima para cada solicitud de contenedor en el administrador de recursos.                                                                                           | int    | 8192                                                                                                                                       |
| yarn-site.yarn.nodemanager.resource.cpu-vcores                                       | Número de núcleos de CPU que se pueden asignar para los contenedores.                                                                                                             | int    | 32                                                                                                                                         |
| yarn-site.yarn.scheduler.maximum-allocation-vcores                                   | Asignación máxima para cada solicitud de contenedor en el administrador de recursos, en términos de núcleos de CPU virtual.                                                            | int    | 8                                                                                                                                          |
| yarn-site.yarn.nodemanager.linux-container-executor.secure-mode.pool-user-count      | Número de usuarios de grupos para el ejecutor de contenedores de Linux en modo seguro.                                                                                             | int    | 6                                                                                                                                          |
| yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent                        | Porcentaje máximo de recursos del clúster que se pueden usar para ejecutar patrones de aplicación.                                                                              | FLOAT  | 0,1                                                                                                                                        |
| yarn-site.yarn.nodemanager.container-executor.class                                  | Ejecutores de contenedores para sistemas operativos específicos.                                                                                                               | string | org.apache.hadoop.yarn.server.nodemanager.LinuxContainerExecutor                                                                           |
| capacity-scheduler.yarn.scheduler.capacity.root.default.user-limit-factor            | Múltiplo de la capacidad de la cola que se puede configurar para permitir que un usuario único adquiera más recursos.                                                          | int    | 1                                                                                                                                          |
| capacity-scheduler.yarn.scheduler.capacity.root.default.maximum-capacity             | Máximo de capacidad de cola en porcentaje (%) como valor float O como capacidad máxima de la cola de recursos absoluta. Al establecer este valor en-1, la capacidad máxima se establece en 100 %.           | int    | 100                                                                                                                                        |
| capacity-scheduler.yarn.scheduler.capacity.root.default.state                        | Estado de la cola. Puede ser Running (En ejecución) o Stopped (Detenido).                                                                                                                      | string | RUNNING                                                                                                                                    |
| capacity-scheduler.yarn.scheduler.capacity.root.default.maximum-application-lifetime | Duración máxima de una aplicación que se envía a una cola en segundos. Cualquier valor menor o igual que cero se considerará deshabilitado.                     | int    | -1                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.root.default.default-application-lifetime | Duración predeterminada de una aplicación que se envía a una cola en segundos. Cualquier valor menor o igual que cero se considerará deshabilitado.                     | int    | -1                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.node-locality-delay                       | Número de oportunidades de programación perdidas después de las cuales CapacityScheduler intenta programar contenedores locales de bastidor.                                               | int    | 40                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.rack-locality-additional-delay            | Número de oportunidades de programación perdidas adicionales a las de retraso de localidad de nodo después de las cuales CapacityScheduler intenta programar contenedores fuera del conmutador. | int    | -1                                                                                                                                         |
| hadoop-env.HADOOP_HEAPSIZE_MAX                                                       | Tamaño máximo predeterminado del montón de todos los procesos JVM de Hadoop.                                                                                                                 | int    | 2048                                                                                                                                       |
| yarn-env.YARN_RESOURCEMANAGER_HEAPSIZE                                               | Tamaño del montón del administrador de recursos de YARN.                                                                                                                                     | int    | 2048                                                                                                                                       |
| yarn-env.YARN_NODEMANAGER_HEAPSIZE                                                   | Tamaño del montón del administrador de nodos de YARN.                                                                                                                                         | int    | 2048                                                                                                                                       |
| mapred-env.HADOOP_JOB_HISTORYSERVER_HEAPSIZE                                         | Tamaño del montón de servidor del historial de trabajos de Hadoop.                                                                                                                                 | int    | 2048                                                                                                                                       |
| hive-env.HADOOP_HEAPSIZE                                                             | Tamaño del montón de Hadoop para Hive.                                                                                                                                          | int    | 2048                                                                                                                                       |
| livy-conf.livy.server.session.timeout-check                                          | Comprobación del tiempo de espera de sesión del servidor Livy.                                                                                                                                | bool   | true                                                                                                                                       |
| livy-conf.livy.server.session.timeout-check.skip-busy                                | Omisión de ocupado para Comprobación del tiempo de espera de sesión del servidor Livy.                                                                                                                  | bool   | true                                                                                                                                       |
| livy-conf.livy.server.session.timeout                                                | Tiempo de espera de sesión del servidor Livy en (ms/s/m | min/h/d/a).                                                                                                              | string | 2h                                                                                                                                         |
| livy-conf.livy.server.yarn.poll-interval                                             | Intervalo de sondeo para YARN en el servidor Livy en (ms/s/m | min/h/d/a).                                                                                                     | string | 500ms                                                                                                                                      |
| livy-conf.livy.rsc.jars                                                              | Archivos JAR RSC de Livy.                                                                                                                                                        | string | local:/opt/livy/rsc-jars/livy-api.jar,local:/opt/livy/rsc-jars/livy-rsc.jar,local:/opt/livy/rsc-jars/netty-all.jar                         |
| livy-conf.livy.repl.jars                                                             | Archivos JAR REPL de Livy.                                                                                                                                                       | string | local:/opt/livy/repl_2.11-jars/livy-core.jar,local:/opt/livy/repl_2.11-jars/livy-repl.jar,local:/opt/livy/repl_2.11-jars/commons-codec.jar |
| livy-conf.livy.rsc.sparkr.package                                                    | Paquete SparkR RSC de Livy.                                                                                                                                              | string | hdfs:///system/livy/sparkr.zip                                                                                                             |
| livy-env.LIVY_SERVER_JAVA_OPTS                                                       | Livy Server Java Options.                                                                                                                                             | string | -Xmx2g                                                                                                                                     |
| spark-defaults-conf.spark.r.backendConnectionTimeout                                 | Tiempo de espera de conexión establecido por el proceso de R en su conexión con RBackend en segundos.                                                                                         | int    | 86400                                                                                                                                      |
| spark-defaults-conf.spark.pyspark.python                                             | Opción de Python para Spark.                                                                                                                                              | string | /opt/bin/python3                                                                                                                           |
| spark-defaults-conf.spark.yarn.jars                                                  | Archivos JAR de YARN.                                                                                                                                                            | string | local:/opt/spark/jars/*                                                                                                                    |
| spark-history-server-conf.spark.history.fs.cleaner.maxAge                            | Antigüedad máxima de los archivos del historial de trabajos antes de que los elimine el limpiador del historial del sistema de archivos en (ms/s/m | min/h/d/a).                                                   | string | 7d                                                                                                                                         |
| spark-history-server-conf.spark.history.fs.cleaner.interval                          | Intervalo del limpiador del historial de Spark en (ms/s/m | min/h/d/a).                                                                                                        | string | 12h                                                                                                                                        |
| hadoop-env.HADOOP_CLASSPATH                                                          | Establece la variable Classpath adicional de Hadoop.                                                                                                                                 | string |                                                                                                                                            |
| spark-env.SPARK_DAEMON_MEMORY                                                        | Memoria de demonio de Spark.                                                                                                                                                  | string | 2g                                                                                                                                         |
| yarn-site.yarn.log-aggregation.retain-seconds                                        | Cuando la agregación de registros está habilitada, esta propiedad determina la cantidad de segundos que se deben conservar los registros.                                                                       | int    | 604800                                                                                                                                     |
| yarn-site.yarn.nodemanager.log-aggregation.compression-type                          | Tipo de compresión para la agregación de registros del administrador de nodos de Yarn.                                                                                                            | string | gz                                                                                                                                         |
| yarn-site.yarn.nodemanager.log-aggregation.roll-monitoring-interval-seconds          | Intervalo en segundos para la supervisión de la renovación de la agregación de registros del administrador de nodos.                                                                                                  | int    | 3600                                                                                                                                       |
| yarn-site.yarn.scheduler.minimum-allocation-mb                                       | Asignación mínima para cada solicitud de contenedor en el administrador de recursos, en MB.                                                                                   | int    | 512                                                                                                                                        |
| yarn-site.yarn.scheduler.minimum-allocation-vcores                                   | Asignación mínima para cada solicitud de contenedor en el administrador de recursos, en términos de núcleos de CPU virtual.                                                             | int    | 1                                                                                                                                          |
| yarn-site.yarn.nm.liveness-monitor.expiry-interval-ms                                | Indica cuánto se debe esperar hasta que un administrador de nodos se considere inactivo.                                                                                                             | int    | 180000                                                                                                                                     |
| yarn-site.yarn.resourcemanager.zk-timeout-ms                                         | Tiempo de espera de la sesión de "ZooKeeper" en milisegundos.                                                                                                                          | int    | 40000                                                                                                                                      |
| capacity-scheduler.yarn.scheduler.capacity.root.default.acl_application_max_priority | ACL de quién puede enviar aplicaciones con una prioridad configurada. Por ejemplo, [user={name} group={name} max_priority={priority} default_priority={priority}].             | string | *                                                                                                                                          |
| includeSpark                                                                         | Valor booleano para configurar si los trabajos de Spark pueden o no ejecutarse en el bloque de almacenamiento.                                                                                           | bool   | true                                                                                                                                       |
| enableSparkOnK8s                                                                     | Valor booleano para configurar si habilitar o no Spark en K8s, lo que agrega contenedores para K8s en el nodo principal de Spark.                                                               | bool   | false                                                                                                                                      |
| sparkVersion                                                                         | Versión de Spark.                                                                                                                                                  | string | 2.4                                                                                                                                        |
| spark-env.PYSPARK_ARCHIVES_PATH                                                      | Ruta de acceso a los archivos JAR de PySpark que se usan en trabajos de Spark.                                                                                                                      | string | local:/opt/spark/python/lib/pyspark.zip,local:/opt/spark/python/lib/py4j-0.10.7-src.zip                                                    |

En las secciones siguientes se enumeran las configuraciones no admitidas.

##  <a name="big-data-clusters-specific-default-hdfs-settings"></a>Configuración predeterminada de HDFS específica para Clústeres de macrodatos
La configuración de HDFS que se muestra a continuación son los valores predeterminados específicos de BDC, pero que el usuario puede configurar. No se incluyen las configuraciones administradas por el sistema.

| Nombre de la opción de configuración                                                                       | Descripción                                                                                         | Tipo   | Valor predeterminado                                                                    |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------ | -------------------------------------------------------------------------------------- |
| hdfs-site.dfs.replication                                                  | Replicación de bloque predeterminada.                                                                          | int    | 2                                                                                      |
| hdfs-site.dfs.namenode.provided.enabled                                    | Permite que el nodo de nombres controle los almacenamientos proporcionados.                                                   | bool   | true                                                                                   |
| hdfs-site.dfs.datanode.provided.enabled                                    | Permite que el nodo de datos controle los almacenamientos proporcionados.                                                   | bool   | true                                                                                   |
| hdfs-site.dfs.datanode.provided.volume.lazy.load                           | Habilita la carga diferida en el nodo de datos para los almacenamientos proporcionados.                                                 | bool   | true                                                                                   |
| hdfs-site.dfs.provided.aliasmap.inmemory.enabled                           | Habilita la asignación de alias en memoria para los almacenamientos proporcionados.                                                     | bool   | true                                                                                   |
| hdfs-site.dfs.provided.aliasmap.class                                      | La clase que se usa para especificar el formato de entrada de los bloques en los almacenamientos proporcionados.              | string | org.apache.hadoop.hdfs.server.common.blockaliasmap.impl.InMemoryLevelDBAliasMapClient  |
| hdfs-site.dfs.namenode.provided.aliasmap.class                             | La clase que se usa para especificar el formato de entrada de los bloques en los almacenamientos proporcionados para el nodo de nombres. | string | org.apache.hadoop.hdfs.server.common.blockaliasmap.impl.NamenodeInMemoryAliasMapClient |
| hdfs-site.dfs.provided.aliasmap.load.retries                               | Cantidad de reintentos en el nodo de datos para cargar la asignación de alias proporcionada.                                    | int    | 0                                                                                      |
| hdfs-site.dfs.provided.aliasmap.inmemory.batch-size                        | Tamaño del lote al recorrer en iteración la base de datos de copia de seguridad de la asignación de alias.                               | int    | 500                                                                                    |
| hdfs-site.dfs.datanode.provided.volume.readthrough                         | Habilita la lectura para los almacenamientos proporcionados en el nodo de datos.                                               | bool   | true                                                                                   |
| hdfs-site.dfs.provided.cache.capacity.mount                                | Habilita el montaje de capacidad de la caché para los almacenamientos proporcionados.                                                  | bool   | true                                                                                   |
| hdfs-site.dfs.provided.overreplication.factor                              | Factor de sobrerreplicación para los almacenamientos proporcionados.                                                       | FLOAT  | 1                                                                                      |
| hdfs-site.dfs.provided.cache.capacity.fraction                             | Fracción de la capacidad de la caché para el almacenamiento proporcionado.                                                       | FLOAT  | 0,01                                                                                   |
| hdfs-site.dfs.ls.limit                                                     | Limita el número de archivos que imprime ls.                                                            | int    | 500                                                                                    |
| hdfs-env.HDFS_NAMENODE_OPTS                                                | Opciones de Namenode de HDFS.                                                                              | string | -Dhadoop.security.logger=INFO,RFAS -Xmx2g                                              |
| hdfs-env.HDFS_DATANODE_OPTS                                                | Opciones de nodo de datos de HDFS.                                                                              | string | -Dhadoop.security.logger=ERROR,RFAS -Xmx2g                                             |
| hdfs-env.HDFS_ZKFC_OPTS                                                    | Opciones de ZKFC de HDFS.                                                                                  | string | -Xmx1g                                                                                 |
| hdfs-env.HDFS_JOURNALNODE_OPTS                                             | Opciones de JournalNode de HDFS.                                                                           | string | -Xmx2g                                                                                 |
| hdfs-env.HDFS_AUDIT_LOGGER                                                 | Opciones del registrador de auditoría de HDFS.                                                                          | string | INFO,RFAAUDIT                                                                          |
| core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels | Niveles de jerarquía para el grupo de búsqueda LDAP de Hadoop del sitio principal.                                            | int    | 10                                                                                     |
| core-site.fs.permissions.umask-mode                                        | Modo UMask de permisos.                                                                              | string | 077                                                                                    |
| core-site.hadoop.security.kms.client.failover.max.retries                  | Número máximo de reintentos para la conmutación por error del cliente.                                                                    | int    | 20                                                                                     || zoo-cfg.tickTime                                       | Hora de tic para la configuración de "ZooKeeper".                         | int    | 2000                |
| zoo-cfg.initLimit                                      | Hora de inicialización para la configuración de "ZooKeeper".                         | int    | 10                  |
| zoo-cfg.syncLimit                                      | Hora de sincronización para la configuración de "ZooKeeper".                         | int    | 5                   |
| zoo-cfg.maxClientCnxns                                 | Número máximo de conexiones de cliente para la configuración de "ZooKeeper".            | int    | 60                  |
| zoo-cfg.minSessionTimeout                              | Tiempo de espera mínimo de sesión para la configuración de "ZooKeeper".           | int    | 4000                |
| zoo-cfg.maxSessionTimeout                              | Tiempo de espera máximo de sesión para la configuración de "ZooKeeper".           | int    | 40000               |
| zoo-cfg.autopurge.snapRetainCount                      | Número de instantáneas retenidas para la configuración de purga automática de "ZooKeeper".       | int    | 3                   |
| zoo-cfg.autopurge.purgeInterval                        | Intervalo de purga para la configuración de purga automática de "ZooKeeper".          | int    | 0                   |
| zookeeper-java-env.JVMFLAGS                            | Marcas de JVM para el entorno de Java en "ZooKeeper".            | string | -Xmx1G -Xms1G       |
| zookeeper-log4j-properties.zookeeper.console.threshold | Umbral de la consola log4j en "ZooKeeper".               | string | INFO                |
| zoo-cfg.zookeeper.request.timeout                      | Controla el tiempo de espera de solicitud de "ZooKeeper" en milisegundos. | int    | 40000               |
| kms-site.hadoop.security.kms.encrypted.key.cache.size | Tamaño de la caché para la clave cifrada en Hadoop KMS. | int  | 500 |
    

##  <a name="big-data-clusters-specific-default-gateway-settings"></a>Configuración predeterminada de puerta de enlace específica para Clústeres de macrodatos
La configuración de puerta de enlace que se muestra a continuación son los valores predeterminados específicos de BDC, pero que el usuario puede configurar. No se incluyen las configuraciones administradas por el sistema. La configuración de puerta de enlace solo se puede configurar en el ámbito del **recurso**.

| Nombre de la opción de configuración                                          | Descripción                                            | Tipo   | Valor predeterminado |
| --------------------------------------------- | ------------------------------------------------------ | ------ | ------------------- |
| gateway-site.gateway.httpclient.socketTimeout | Tiempo de espera de socket para el cliente HTTP en la puerta de enlace en (ms/s/m). | string | 90s                 |
| gateway-site.sun.security.krb5.debug          | Depuración de seguridad de Kerberos.                           | bool   | true                |
| knox-env.KNOX_GATEWAY_MEM_OPTS                | Opciones de memoria de puerta de enlace de Knox.                           | string | -Xmx2g              |

## <a name="unsupported-spark-configurations"></a>Configuraciones de Spark no admitidas

Las configuraciones siguientes de `spark` no se admiten y no se pueden cambiar en el contexto del Clúster de macrodatos.

| Category  | Subcategoría               | Archivo                       | Configuraciones no admitidas                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|           | yarn-site                  | yarn-site.xml              | yarn.log-aggregation-enable                                             |
|           |                            |                            | yarn.log.server.url                                                     |
|           |                            |                            | yarn.nodemanager.pmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.vmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.aux-services                                           |
|           |                            |                            | yarn.resourcemanager.address                                            |
|           |                            |                            | yarn.nodemanager.address                                                |
|           |                            |                            | yarn.client.failover-no-ha-proxy-provider                               |
|           |                            |                            | yarn.client.failover-proxy-provider                                     |
|           |                            |                            | yarn.http.policy                                                        |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.use-pool-user     |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.pool-user-prefix  |
|           |                            |                            | yarn.nodemanager.linux-container-executor.nonsecure-mode.local-user     |
|           |                            |                            | yarn.acl.enable                                                         |
|           |                            |                            | yarn.admin.acl                                                          |
|           |                            |                            | yarn.resourcemanager.hostname                                           |
|           |                            |                            | yarn.resourcemanager.principal                                          |
|           |                            |                            | yarn.resourcemanager.keytab                                             |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-keytab-file                          |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-principal                            |
|           |                            |                            | yarn.nodemanager.principal                                              |
|           |                            |                            | yarn.nodemanager.keytab                                                 |
|           |                            |                            | yarn.nodemanager.webapp.spnego-keytab-file                              |
|           |                            |                            | yarn.nodemanager.webapp.spnego-principal                                |
|           |                            |                            | yarn.resourcemanager.ha.enabled                                         |
|           |                            |                            | yarn.resourcemanager.cluster-id                                         |
|           |                            |                            | yarn.resourcemanager.zk-address                                         |
|           |                            |                            | yarn.resourcemanager.ha.rm-ids                                          |
|           |                            |                            | yarn.resourcemanager.hostname.*                                         |
|           | capacity-scheduler         | capacity-scheduler.xml     | yarn.scheduler.capacity.root.acl_submit_applications                    |
|           |                            |                            | yarn.scheduler.capacity.root.acl_administer_queue                       |
|           |                            |                            | yarn.scheduler.capacity.root.default.acl_application_max_priority       |
|           | yarn-env                   | yarn-env.sh                |                                                                         |
|           | spark-defaults-conf        | spark-defaults.conf        | spark.yarn.archive                                                      |
|           |                            |                            | spark.yarn.historyServer.address                                        |
|           |                            |                            | spark.eventLog.enabled                                                  |
|           |                            |                            | spark.eventLog.dir                                                      |
|           |                            |                            | spark.sql.warehouse.dir                                                 |
|           |                            |                            | spark.sql.hive.metastore.version                                        |
|           |                            |                            | spark.sql.hive.metastore.jars                                           |
|           |                            |                            | spark.extraListeners                                                    |
|           |                            |                            | spark.metrics.conf                                                      |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword  
|           |                            |                            | spark.ui.enabled                                                        |
|           | spark-env                  | spark-env.sh               | SPARK_NO_DAEMONIZE                                                      |
|           |                            |                            | SPARK_DIST_CLASSPATH                                                    |
|           | spark-history-server-conf  | spark-history-server.conf  | spark.history.fs.logDirectory                                           |
|           |                            |                            | spark.ui.proxyBase                                                      |
|           |                            |                            | spark.history.fs.cleaner.enabled                                        |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword                                              |
|           |                            |                            | spark.history.kerberos.enabled                                          |
|           |                            |                            | spark.history.kerberos.principal                                        |
|           |                            |                            | spark.history.kerberos.keytab                                           |
|           |                            |                            | spark.ui.filters                                                        |
|           |                            |                            | spark.acls.enable                                                       |
|           |                            |                            | spark.history.ui.acls.enable                                            |
|           |                            |                            | spark.history.ui.admin.acls                                             |
|           |                            |                            | spark.history.ui.admin.acls.groups                                      |
|           | livy-conf                  | livy.conf                  | livy.keystore                                                           |
|           |                            |                            | livy.keystore.password                                                  |
|           |                            |                            | livy.spark.master                                                       |
|           |                            |                            | livy.spark.deploy-mode                                                  |
|           |                            |                            | livy.rsc.jars                                                           |
|           |                            |                            | livy.repl.jars                                                          |
|           |                            |                            | livy.rsc.pyspark.archives                                               |
|           |                            |                            | livy.rsc.sparkr.package                                                 |
|           |                            |                            | livy.repl.enable-hive-context                                           |
|           |                            |                            | livy.superusers                                                         |
|           |                            |                            | livy.server.auth.type                                                   |
|           |                            |                            | livy.server.launch.kerberos.keytab                                      |
|           |                            |                            | livy.server.launch.kerberos.principal                                   |
|           |                            |                            | livy.server.auth.kerberos.principal                                     |
|           |                            |                            | livy.server.auth.kerberos.keytab                                        |
|           |                            |                            | livy.impersonation.enabled                                              |
|           |                            |                            | livy.server.access-control.enabled                                      |
|           |                            |                            | livy.server.access-control.*                                            |
|           | livy-env                   | livy-env.sh                |                                                                         |
|           | hive-site                  | hive-site.xml              | javax.jdo.option.ConnectionURL                                          |
|           |                            |                            | javax.jdo.option.ConnectionDriverName                                   |
|           |                            |                            | javax.jdo.option.ConnectionUserName                                     |
|           |                            |                            | javax.jdo.option.ConnectionPassword                                     |
|           |                            |                            | hive.metastore.uris                                                     |
|           |                            |                            | hive.metastore.pre.event.listeners                                      |
|           |                            |                            | hive.security.authorization.enabled                                     |
|           |                            |                            | hive.security.metastore.authenticator.manager                           |
|           |                            |                            | hive.security.metastore.authorization.manager                           |
|           |                            |                            | hive.metastore.use.SSL                                                  |
|           |                            |                            | hive.metastore.keystore.path                                            |
|           |                            |                            | hive.metastore.keystore.password                                        |
|           |                            |                            | hive.metastore.truststore.path                                          |
|           |                            |                            | hive.metastore.truststore.password                                      |
|           |                            |                            | hive.metastore.kerberos.keytab.file                                     |
|           |                            |                            | hive.metastore.kerberos.principal                                       |
|           |                            |                            | hive.metastore.sasl.enabled                                             |
|           |                            |                            | hive.metastore.execute.setugi                                           |
|           |                            |                            | hive.cluster.delegation.token.store.class                               |
|           | hive-env                   | hive-env.sh                

## <a name="unsupported-hdfs-configurations"></a>Configuraciones de HDFS no admitidas

Las configuraciones siguientes de `hdfs` no se admiten y no se pueden cambiar en el contexto del Clúster de macrodatos.

| Category  | Subcategoría               | Archivo                       | Configuraciones no admitidas                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | core-site                   | core-site.xml                 | fs.defaultFS                                          |
|          |                             |                               | ha.zookeeper.quorum                                   |
|          |                             |                               | hadoop.tmp.dir                                        |
|          |                             |                               | hadoop.rpc.protection                                 |
|          |                             |                               | hadoop.security.auth_to_local                         |
|          |                             |                               | hadoop.security.authentication                        |
|          |                             |                               | hadoop.security.authorization                         |
|          |                             |                               | hadoop.http.authentication.simple.anonymous.allowed   |
|          |                             |                               | hadoop.http.authentication.type                       |
|          |                             |                               | hadoop.http.authentication.kerberos.principal         |
|          |                             |                               | hadoop.http.authentication.kerberos.keytab            |
|          |                             |                               | hadoop.http.filter.initializers                       |
|          |                             |                               | hadoop.security.group.mapping.*                       |                               |
|          |                             |                               | hadoop.security.key.provider.path                     |                               |
|          | mapred-env                  | mapred-env.sh                 |                                                       |
|          | hdfs-site                   | hdfs-site.xml                 | dfs.namenode.name.dir                                 |
|          |                             |                               | dfs.datanode.data.dir                                 |
|          |                             |                               | dfs.namenode.acls.enabled                             |
|          |                             |                               | dfs.namenode.datanode.registration.ip-hostname-check  |
|          |                             |                               | dfs.client.retry.policy.enabled                       |
|          |                             |                               | dfs.permissions.enabled                               |
|          |                             |                               | dfs.nameservices                                      |
|          |                             |                               | dfs.ha.namenodes.nmnode-0                             |
|          |                             |                               | dfs.namenode.rpc-address.nmnode-0.*                   |
|          |                             |                               | dfs.namenode.shared.edits.dir                         |
|          |                             |                               | dfs.ha.automatic-failover.enabled                     |
|          |                             |                               | dfs.ha.fencing.methods                                |
|          |                             |                               | dfs.journalnode.edits.dir                             |
|          |                             |                               | dfs.client.failover.proxy.provider.nmnode-0           |
|          |                             |                               | dfs.namenode.http-address                             |
|          |                             |                               | dfs.namenode.httpS-address                            |
|          |                             |                               | dfs.http.policy                                       |
|          |                             |                               | dfs.encrypt.data.transfer                             |
|          |                             |                               | dfs.block.access.token.enable                         |
|          |                             |                               | dfs.data.transfer.protection                          |
|          |                             |                               | dfs.encrypt.data.transfer.cipher.suites               |
|          |                             |                               | dfs.https.port                                        |
|          |                             |                               | dfs.namenode.keytab.file                              |
|          |                             |                               | dfs.namenode.kerberos.principal                       |
|          |                             |                               | dfs.namenode.kerberos.internal.spnego.principal       |
|          |                             |                               | dfs.datanode.data.dir.perm                            |
|          |                             |                               | dfs.datanode.address                                  |
|          |                             |                               | dfs.datanode.http.address                             |
|          |                             |                               | dfs.datanode.ipc.address                              |
|          |                             |                               | dfs.datanode.https.address                            |
|          |                             |                               | dfs.datanode.keytab.file                              |
|          |                             |                               | dfs.datanode.kerberos.principal                       |
|          |                             |                               | dfs.journalnode.keytab.file                           |
|          |                             |                               | dfs.journalnode.kerberos.principal                    |
|          |                             |                               | dfs.journalnode.kerberos.internal.spnego.principal    |
|          |                             |                               | dfs.web.authentication.kerberos.keytab                |
|          |                             |                               | dfs.web.authentication.kerberos.principal             |
|          |                             |                               | dfs.webhdfs.enabled                                   |
|          |                             |                               | dfs.permissions.superusergroup                        |
|          | hdfs-env                    | hdfs-env.sh                   | HADOOP_HEAPSIZE_MAX                                   |
|          | zoo-cfg                     | zoo.cfg                       | secureClientPort                                      |
|          |                             |                               | clientPort                                            |
|          |                             |                               | dataDir                                               |
|          |                             |                               | dataLogDir                                            |
|          |                             |                               | 4lw.commands.whitelist                                |
|          | zookeeper-java-env          | java.env                      | ZK_LOG_DIR                                            |
|          |                             |                               | SERVER_JVMFLAGS                                       |
|          | zookeeper-log4j-properties  | log4j.properties (zookeeper)  | log4j.rootLogger                                      |
|          |                             |                               | log4j.appender.CONSOLE.*                              |

> [!NOTE]
> Este artículo incluye el término *whitelist* (lista de aprobados), un término que Microsoft considera inadecuado en este contexto. El término aparece en este artículo porque actualmente aparece en el software. Cuando se quite el término del software, se quitará también del artículo.

## <a name="unsupported-gateway-configurations"></a>Configuraciones de `gateway` no admitidas

Las configuraciones siguientes de `gateway` no se admiten y no se pueden cambiar en el contexto del Clúster de macrodatos.

| Category  | Subcategoría               | Archivo                       | Configuraciones no admitidas                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | gateway-site                | gateway-site.xml              | gateway.port                                          |
|          |                             |                               | gateway.path                                          |
|          |                             |                               | gateway.gateway.conf.dir                              |
|          |                             |                               | gateway.hadoop.kerberos.secured                       |
|          |                             |                               | java.security.krb5.conf                               |
|          |                             |                               | java.security.auth.login.config                       |
|          |                             |                               | gateway.websocket.feature.enabled                     |
|          |                             |                               | gateway.scope.cookies.feature.enabled                 |
|          |                             |                               | ssl.exclude.protocols                                 |
|          |                             |                               | ssl.include.ciphers                                   |

## <a name="next-steps"></a>Pasos siguientes

[Configuración de Clústeres de macrodatos de SQL Server](configure-bdc-overview.md)
