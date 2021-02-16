---
title: Configuración y seguridad de PolyBase para Hadoop | Microsoft Docs
description: Use esta configuración para la conectividad de PolyBase con Hadoop, incluidos Hadoop.RPC.Protection, archivos XML de ejemplo para un clúster CDH 5.X y configuración de Kerberos.
ms.date: 04/23/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 455c1de1f11b58d4d12cf982d035846b73e60a43
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351855"
---
# <a name="polybase-configuration-and-security-for-hadoop"></a>Configuración y seguridad de PolyBase para Hadoop

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

En este artículo se proporciona una referencia para distintos valores de configuración que afectan a la conectividad de PolyBase con Hadoop. Para ver un tutorial sobre cómo usar PolyBase con Hadoop, eche un vistazo a [Configure PolyBase to access external data in Hadoop](polybase-configure-hadoop.md) (Configuración de PolyBase para acceder a datos externos en Hadoop).

## <a name="hadooprpcprotection-setting"></a><a id="rpcprotection"></a> Configuración de Hadoop.RPC.Protection

Una forma habitual de proteger la comunicación en un clúster de Hadoop consiste en cambiar la configuración de hadoop.rpc.protection a "Privacy" (Privacidad) o "Integrity" (Integridad). De forma predeterminada, PolyBase da por hecho que la configuración está establecida en "Autenticar". Para reemplazar esta configuración predeterminada, agregue la siguiente propiedad al archivo core-site.xml. Si cambia esta configuración, permitirá que se efectúe una transferencia de datos segura entre los nodos de Hadoop y que se establezca una conexión TLS con SQL Server.

```xml
<!-- RPC Encryption information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG -->
   <property>
     <name>hadoop.rpc.protection</name>
     <value></value>
   </property> 
```

Para utilizar "Privacidad" o "Integridad" para hadoop.rpc.protection, SQL Server debe ser al menos SQL Server 2016 SP1 CU7, SQL Server 2016 SP2 o SQL Server 2017 CU3.

## <a name="example-xml-files-for-cdh-5x-cluster"></a>Archivos XML de ejemplo para el clúster CDH 5.X

Yarn-site.xml con la configuración yarn.application.classpath y mapreduce.application.classpath.

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/,$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

Si decide dividir las dos opciones de configuración en mapred-site.xml y yarn-site.xml, los archivos serían los siguientes:

**yarn-site.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
   <property>
      <name>yarn.resourcemanager.connect.max-wait.ms</name>
      <value>40000</value>
   </property>
   <property>
      <name>yarn.resourcemanager.connect.retry-interval.ms</name>
      <value>30000</value>
   </property>
<!-- Applications' Configuration-->
   <property>
     <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
      <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
      <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
      <name>yarn.application.classpath</name>
      <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/*</value>
   </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
      <name>yarn.resourcemanager.principal</name>
      <value></value>
   </property>
-->
</configuration>
```

**mapred-site.xml**

Tenga en cuenta que hemos agregado la propiedad mapreduce.application.classpath. En CDH 5.x, encontrará los valores de configuración en la misma convención de nomenclatura de Ambari.

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration xmlns:xi="http://www.w3.org/2001/XInclude">
   <property>
     <name>mapred.min.split.size</name>
       <value>1073741824</value>
   </property>
   <property>
     <name>mapreduce.app-submission.cross-platform</name>
     <value>true</value>
   </property>
<property>
     <name>mapreduce.application.classpath</name>
     <value>$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH</value>
   </property>


<!--kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
   <property>
     <name>mapreduce.jobhistory.principal</name>
     <value></value>
   </property>
   <property>
     <name>mapreduce.jobhistory.address</name>
     <value></value>
   </property>
-->
</configuration>
```

## <a name="kerberos-configuration"></a>Configuración de Kerberos  

Tenga en cuenta que cuando PolyBase se autentica en un clúster protegido de Kerberos, espera que la configuración de hadoop.rpc.protection esté establecida en "Autenticar" de forma predeterminada. Esta acción dejará la comunicación de datos entre los nodos Hadoop sin cifrar. Para usar la configuración de "Privacidad" o "Integridad" para hadoop.rpc.protection, actualice el archivo core-site.xml en el servidor de PolyBase. Para más información, vea la sección anterior titulada [Conectarse al clúster de Hadoop con la opción de configuración Hadoop.RPC.Protection](#rpcprotection).

Para conectarse a un clúster de Hadoop protegido con Kerberos mediante MIT KDC:

1. Busque el directorio de configuración de Hadoop en la ruta de acceso de instalación de SQL Server. Normalmente, la ruta de acceso es:  

   ```  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf  
   ```  

2. Busque el valor de configuración del lado de Hadoop de las claves de configuración que se muestran en la tabla. (En el equipo de Hadoop, busque los archivos en el directorio de configuración de Hadoop).  
   
3. Copie los valores de configuración en la propiedad Value en los archivos correspondientes en el equipo de SQL Server.  
   
   |**#**|**Archivo de configuración**|**Clave de configuración**|**Acción**|  
   |------------|----------------|---------------------|----------|   
   |1|core-site.xml|polybase.kerberos.kdchost|Especifique el nombre de host KDC. Por ejemplo: kerberos.su-dominio.com.|  
   |2|core-site.xml|polybase.kerberos.realm|Especifique el dominio Kerberos. Por ejemplo: SU-DOMINIO.COM <br><br>**Nota de la configuración**: El nombre del dominio kerberos debe estar en mayúsculas.|  
   |3|core-site.xml|hadoop.security.authentication|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: KERBEROS<br></br>**Nota de seguridad:** KERBEROS debe estar en mayúsculas. Si aparece en minúsculas, puede que no esté activado.|   
   |4|hdfs-site.xml|dfs.namenode.kerberos.principal|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: hdfs/_HOST@YOUR-REALM.COM|  
   |5|mapred-site.xml|mapreduce.jobhistory.principal|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: mapred/_HOST@YOUR-REALM.COM|  
   |6|mapred-site.xml|mapreduce.jobhistory.address|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: 10.193.26.174:10020|  
   |7|yarn-site.xml yarn.|yarn.resourcemanager.principal|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: yarn/_HOST@YOUR-REALM.COM|  

4. Cree un objeto de credencial con ámbito de base de datos para especificar la información de autenticación para cada usuario de Hadoop. Vea [PolyBase T-SQL objects (Objetos T-SQL de PolyBase)](../../relational-databases/polybase/polybase-t-sql-objects.md).  

## <a name="next-steps"></a>Pasos siguientes  

Para más información, consulte los siguientes artículos.

[Configure PolyBase to access external data in Hadoop](polybase-configure-hadoop.md) (Configuración de PolyBase para acceder a datos externos en Hadoop)
[Introducción a PolyBase](../../relational-databases/polybase/polybase-guide.md)
