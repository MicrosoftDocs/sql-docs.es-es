---
title: 'Acceso a datos externos: Hadoop: PolyBase'
description: Explica cómo configurar polybase en el almacenamiento de datos paralelos para conectarse a Hadoop externo.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019, seo-lt-2019
ms.openlocfilehash: 18bec3d694985e15b1ae2d813fddf2f378c9d78a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049111"
---
# <a name="configure-polybase-in-parallel-data-warehouse-to-access-external-data-in-hadoop"></a>Configuración de polybase en el almacenamiento de datos paralelos para acceder a datos externos en Hadoop

En el artículo se explica cómo usar polybase en un dispositivo APS para consultar datos externos en Hadoop.

## <a name="prerequisites"></a>Prerrequisitos

PolyBase es compatible con dos proveedores de Hadoop: Hortonworks Data Platform (HDP) y Cloudera Distributed Hadoop (CDH). Hadoop sigue el patrón “Principal.Secundaria.Versión” para sus revisiones nuevas y se admiten todas las versiones dentro de una revisión principal y secundaria compatible. Se admiten los siguientes proveedores de Hadoop:
 - Hortonworks HDP 1.3 en Linux y Windows Server  
 - Hortonworks HDP 2,1-2,6 en Linux
 - Hortonworks HDP 3,0-3,1 en Linux
 - Hortonworks HDP 2.1 - 2.3 en Windows Server  
 - Cloudera CDH 4.3 en Linux  
 - Cloudera CDH 5,1-5,5, 5,9-5,13, 5,15 & 5,16 en Linux

### <a name="configure-hadoop-connectivity"></a>Configurar la conectividad de Hadoop

En primer lugar, configure APS para usar su proveedor de Hadoop específico.

1. Ejecute [sp_configure](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) con la “conectividad de hadoop” y defina un valor adecuado para el proveedor. Para hallar el valor, consulte [Configuración de la conectividad de PolyBase](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). 

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 and 3.0 - 3.1 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Reinicie la región APS mediante la página estado del servicio en el [Configuration Manager del dispositivo](launch-the-configuration-manager.md).
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a> Habilitar el cálculo de aplicación  

Para mejorar el rendimiento de las consultas, habilite el cálculo de la aplicación para el clúster de Hadoop:  
  
1. Abra una conexión de escritorio remoto al nodo de control de PDW.

2. Busque el archivo **yarn-site.xml** en el nodo de control. Normalmente, la ruta de acceso es:  

   ```xml  
   C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf\  
   ```  

3. En el equipo de Hadoop, busque el archivo análogo en el directorio de configuración de Hadoop. En el archivo, busque y copie el valor de la clave de configuración yarn.application.classpath.  
  
4. En el nodo de control, en el **archivo deyarn.site.xml,** busque la propiedad **hilados. Application. classpath** . Pegue el valor de la máquina de Hadoop en el elemento de valor.  
  
5. Para todas las versiones 5.X de CDH, deberá agregar los parámetros de configuración mapreduce.application.classpath al final del archivo yarn.site.xml o en el archivo mapred-site.xml. HortonWorks incluye estas configuraciones dentro de las configuraciones yarn.application.classpath. Consulte [PolyBase configuration](../relational-databases/polybase/polybase-configuration.md) (Configuración de PolyBase) para obtener ejemplos.

## <a name="example-xml-files-for-cdh-5x-cluster-default-values"></a>Archivos XML de ejemplo para los valores predeterminados de clúster de CDH 5. X

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

Si decide dividir los dos valores de configuración en el mapred-site.xml y en el yarn-site.xml, los archivos serían los siguientes:

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

Tenga en cuenta que hemos agregado la propiedad mapreduce.application.classpath. En CDH 5. x, encontrará los valores de configuración en la misma Convención de nomenclatura en Ambari.

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

## <a name="example-xml-files-for-hdp-3x-cluster-default-values"></a>Archivos XML de ejemplo para los valores predeterminados del clúster HDP 3. X

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
     <value>$HADOOP_CONF_DIR,/usr/hdp/3.1.0.0-78/hadoop/*,/usr/hdp/3.1.0.0-78/hadoop/lib/*,/usr/hdp/current/hadoop-hdfs-client/*,/usr/hdp/current/hadoop-hdfs-client/lib/*,/usr/hdp/current/hadoop-yarn-client/*,/usr/hdp/current/hadoop-yarn-client/lib/*,/usr/hdp/3.1.0.0-78/hadoop-mapreduce/*,/usr/hdp/3.1.0.0-78/hadoop-yarn/*,/usr/hdp/3.1.0.0-78/hadoop-yarn/lib/*,/usr/hdp/3.1.0.0-78/hadoop-mapreduce/lib/*,/usr/hdp/share/hadoop/common/*,/usr/hdp/share/hadoop/common/lib/*,/usr/hdp/share/hadoop/tools/lib/*</value>
  </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
  <property>
     <name>yarn.resourcemanager.principal</name>
     <value></value>
  </property>
-->
</configuration>
```

## <a name="configure-an-external-table"></a>Configurar una tabla externa

Para consultar los datos en el origen de datos de Hadoop, debe definir una tabla externa para usar en las consultas de Transact-SQL. Los pasos siguientes describen cómo configurar la tabla externa.

1. Cree una clave maestra en la base de datos. Es necesario cifrar el secreto de la credencial.

   ```sql
   CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'S0me!nfo';  
   ```

2. Cree una credencial de ámbito de base de datos para los clústeres de Hadoop protegidos mediante Kerberos.

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

3. Cree un origen de datos externo con [CREATE EXTERNAL DATA SOURCE](../t-sql/statements/create-external-data-source-transact-sql.md).

   ```sql
   -- LOCATION (Required) : Hadoop Name Node IP address and port.  
   -- RESOURCE MANAGER LOCATION (Optional): Hadoop Resource Manager location to enable pushdown computation.  
   -- CREDENTIAL (Optional):  the database scoped credential, created above.  
   CREATE EXTERNAL DATA SOURCE MyHadoopCluster WITH (  
         TYPE = HADOOP,
         LOCATION ='hdfs://10.xxx.xx.xxx:xxxx',
         RESOURCE_MANAGER_LOCATION = '10.xxx.xx.xxx:xxxx',
         CREDENTIAL = HadoopUser1
   );  
   ```

4. Cree un formato de archivo externo con [CREATE EXTERNAL FILE FORMAT](../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE)  
   ```

5. Cree una tabla externa que señale a los datos almacenados en Hadoop con [CREATE EXTERNAL TABLE](../t-sql/statements/create-external-table-transact-sql.md). En este ejemplo, los datos externos contienen datos de sensor de vehículo.

   ```sql
   -- LOCATION: path to file or directory that contains the data (relative to HDFS root).  
   CREATE EXTERNAL TABLE [dbo].[CarSensor_Data] (  
         [SensorKey] int NOT NULL,
         [CustomerKey] int NOT NULL,
         [GeographyKey] int NULL,
         [Speed] float NOT NULL,
         [YearMeasured] int NOT NULL  
   )  
   WITH (LOCATION='/Demo/',
         DATA_SOURCE = MyHadoopCluster,  
         FILE_FORMAT = TextFileFormat  
   );  
   ```

6. Cree estadísticas en una tabla externa.

   ```sql
   CREATE STATISTICS StatsForSensors on CarSensor_Data(CustomerKey, Speed)  
   ```

## <a name="polybase-queries"></a>Consultas de PolyBase

PolyBase es adecuado para realizar tres funciones:  
  
- Consultas ad hoc en tablas externas.  
- Importar datos.  
- Exportar datos.  

Las siguientes consultas proporcionan un ejemplo con datos de sensor de vehículo ficticios.

### <a name="ad-hoc-queries"></a>Consultas ad hoc  

La siguiente consulta ad hoc combina datos relacionales con datos de Hadoop. Selecciona los clientes que tienen más de 35 mph y combinan datos de clientes estructurados almacenados en APS con datos de sensor de automóviles almacenados en Hadoop.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>Importar datos  

La siguiente consulta importa datos externos en APS. En este ejemplo se importan los datos de los controladores rápidos en APS para realizar análisis más profundos. Para mejorar el rendimiento, aprovecha la tecnología de almacén de columnas en APS.  

```sql
CREATE TABLE Fast_Customers
WITH
(CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH (CustomerKey))
AS
SELECT DISTINCT
      Insured_Customers.CustomerKey, Insured_Customers.FirstName, Insured_Customers.LastName,   
      Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
from Insured_Customers INNER JOIN   
(  
      SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
```  

### <a name="exporting-data"></a>Exportación de datos  

La siguiente consulta exporta datos de APS a Hadoop. Se puede usar para archivar datos relacionales en Hadoop y, al mismo tiempo, poder consultarlos.

```sql
-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] 
WITH (  
      LOCATION='/archive/customer/2009',  
      DATA_SOURCE = HadoopHDP2,  
      FILE_FORMAT = TextFileFormat
)  
AS
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

## <a name="view-polybase-objects-in-ssdt"></a>Ver objetos de polybase en SSDT  

En SQL Server Data Tools, las tablas externas se muestran en una carpeta independiente **tablas externas**. Los orígenes de datos y los formatos de archivo externos se encuentran en subcarpetas de **Recursos externos**.  
  
![Objetos polybase en SSDT](media/polybase/external-tables-datasource.png)  

## <a name="next-steps"></a>Pasos siguientes

Para la configuración de seguridad de Hadoop, consulte Configuración de la [seguridad de Hadoop](polybase-configure-hadoop-security.md).<br>
Para más información sobre PolyBase, consulte [¿Qué es PolyBase?](../relational-databases/polybase/polybase-guide.md) 
 
