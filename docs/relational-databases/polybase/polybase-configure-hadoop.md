---
title: 'Acceso a datos externos: Hadoop: PolyBase'
description: En el artículo se usa PolyBase en una instancia de SQL Server con Hardoop. PolyBase es adecuado para consultas ad hoc de tablas externas y de importación y exportación de datos.
ms.date: 12/13/2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>= sql-server-2016'
ms.custom: seo-dt-2019
ms.openlocfilehash: 1124d05dc57697d9e27757dad2b4fee842a22a5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351803"
---
# <a name="configure-polybase-to-access-external-data-in-hadoop"></a>Configurar PolyBase para obtener acceso a datos externos en Hadoop

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

En el artículo se explica cómo usar PolyBase en una instancia de SQL Server para consultar datos externos en Hadoop.

## <a name="prerequisites"></a>Prerrequisitos

- Si no ha instalado PolyBase, consulte [Instalación de PolyBase](polybase-installation.md). En el artículo de instalación se explican los requisitos previos.

<!--SQL Server 2019-->
::: moniker range=">= sql-server-ver15 "

- A partir de SQL Server 2019, también debe [habilitar la característica PolyBase](polybase-installation.md#enable).

::: moniker-end

- PolyBase es compatible con dos proveedores de Hadoop: Hortonworks Data Platform (HDP) y Cloudera Distributed Hadoop (CDH). Hadoop sigue el patrón “Principal.Secundaria.Versión” para sus revisiones nuevas y se admiten todas las versiones dentro de una revisión principal y secundaria compatible. Se admiten los siguientes proveedores de Hadoop:

  - Hortonworks HDP 1.3, 2.1-2.6, 3.0 en Linux
  - Hortonworks HDP 1.3, 2.1-2.3 en Window Server
  - Cloudera CDH 4.3, 5.1 - 5.5, 5.9 - 5.13 en Linux

> [!NOTE]
> PolyBase admite las zonas de cifrado de Hadoop a partir de SQL Server 2016 SP1 CU7 y SQL Server 2017 CU3. Si se usan [grupos de escalabilidad horizontal de PolyBase](polybase-scale-out-groups.md), todos los nodos de proceso deben estar también en una compilación que incluya compatibilidad con las zonas de cifrado de Hadoop.

### <a name="configure-hadoop-connectivity"></a>Configurar la conectividad de Hadoop

En primer lugar, configure SQL Server PolyBase para usar el proveedor específico de Hadoop.

1. Ejecute [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) con la “conectividad de hadoop” y defina un valor adecuado para el proveedor. Para hallar el valor, consulte [Configuración de la conectividad de PolyBase](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md). De forma predeterminada, la conectividad de Hadoop se establece en 7.

   ```sql  
   -- Values map to various external data sources.  
   -- Example: value 7 stands for Hortonworks HDP 2.1 to 2.6 on Linux,
   -- 2.1 to 2.3 on Windows Server, and Azure blob storage  
   sp_configure @configname = 'hadoop connectivity', @configvalue = 7;
   GO

   RECONFIGURE
   GO
   ```  

2. Debe reiniciar SQL Server con **services.msc**. Al reiniciar SQL Server, se reinician estos servicios:  

   - Servicio de movimiento de datos de SQL Server PolyBase  
   - Motor de SQL Server PolyBase  
  
   ![detener e iniciar servicios de PolyBase en services.msc](../../relational-databases/polybase/media/polybase-stop-start.png "detener e iniciar servicios de PolyBase en services.msc")  
  
## <a name="enable-pushdown-computation"></a><a id="pushdown"></a> Habilitar el cálculo de la aplicación  

Para mejorar el rendimiento de las consultas, habilite el cálculo de la aplicación para el clúster de Hadoop:  
  
1. Busque el archivo **yarn-site.xml** en la ruta de acceso de instalación de SQL Server. Normalmente, la ruta de acceso es:  

   ```xml  
   C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf\  
   ```  

1. En el equipo de Hadoop, busque el archivo análogo en el directorio de configuración de Hadoop. En el archivo, busque y copie el valor de la clave de configuración yarn.application.classpath.  
  
1. En el equipo de SQL Server, en el **archivo yarn.site.xml,** busque la propiedad **yarn.application.classpath**. Pegue el valor de la máquina de Hadoop en el elemento de valor.  
  
1. Para todas las versiones 5.X de CDH, se deberán agregar los parámetros de configuración mapreduce.application.classpath al final del archivo yarn.site.xml o en el archivo mapred-site.xml. HortonWorks incluye estas configuraciones dentro de las configuraciones yarn.application.classpath. Consulte [PolyBase configuration](../../relational-databases/polybase/polybase-configuration.md) (Configuración de PolyBase) para obtener ejemplos.

## <a name="configure-an-external-table"></a>Configurar una tabla externa

Para consultar los datos en el origen de datos de Hadoop, debe definir una tabla externa para usar en las consultas de Transact-SQL. Los pasos siguientes describen cómo configurar la tabla externa.

1. Cree una clave maestra en la base de datos, si aún no hay ninguna. Esto es necesario para cifrar el secreto de credencial.

     ```sql
      CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';  
     ```
    ## <a name="arguments"></a>Argumentos
    PASSWORD ='password'

    Es la contraseña usada para cifrar la clave maestra de la base de datos. password debe cumplir los requisitos de la directiva de contraseñas de Windows del equipo que hospeda la instancia de SQL Server.
1. Cree una credencial de ámbito de base de datos para los clústeres de Hadoop protegidos mediante Kerberos.

   ```sql
   -- IDENTITY: the Kerberos user name.  
   -- SECRET: the Kerberos password  
   CREATE DATABASE SCOPED CREDENTIAL HadoopUser1
   WITH IDENTITY = '<hadoop_user_name>', Secret = '<hadoop_password>';  
   ```

2. Cree un origen de datos externo con [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

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

3. Cree un formato de archivo externo con [CREATE EXTERNAL FILE FORMAT](../../t-sql/statements/create-external-file-format-transact-sql.md).

   ```sql
   -- FORMAT TYPE: Type of format in Hadoop (DELIMITEDTEXT,  RCFILE, ORC, PARQUET).
   CREATE EXTERNAL FILE FORMAT TextFileFormat WITH (  
         FORMAT_TYPE = DELIMITEDTEXT,
         FORMAT_OPTIONS (FIELD_TERMINATOR ='|',
               USE_TYPE_DEFAULT = TRUE))
   ```

4. Cree una tabla externa que señale a los datos almacenados en Hadoop con [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md). En este ejemplo, los datos externos contienen datos de sensor de vehículo.

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

5. Cree estadísticas en una tabla externa.

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

La siguiente consulta ad hoc combina datos relacionales con datos de Hadoop. Selecciona a los clientes que conducen a más de 35 mph, combinando los datos estructurados del cliente almacenados en SQL Server con datos de sensor de vehículo almacenados en Hadoop.  

```sql  
SELECT DISTINCT Insured_Customers.FirstName,Insured_Customers.LastName,
       Insured_Customers. YearlyIncome, CarSensor_Data.Speed  
FROM Insured_Customers, CarSensor_Data  
WHERE Insured_Customers.CustomerKey = CarSensor_Data.CustomerKey and CarSensor_Data.Speed > 35
ORDER BY CarSensor_Data.Speed DESC  
OPTION (FORCE EXTERNALPUSHDOWN);   -- or OPTION (DISABLE EXTERNALPUSHDOWN)  
```  

### <a name="importing-data"></a>Importar datos  

La consulta siguiente importa datos externos en SQL Server. En este ejemplo se importan los datos relativos a los conductores más rápidos en SQL Server para hacer un análisis más profundo. Para mejorar el rendimiento, aprovecha la tecnología Columnstore.  

```sql
SELECT DISTINCT
      Insured_Customers.FirstName, Insured_Customers.LastName,   
      Insured_Customers.YearlyIncome, Insured_Customers.MaritalStatus  
INTO Fast_Customers from Insured_Customers INNER JOIN   
(  
      SELECT * FROM CarSensor_Data where Speed > 35   
) AS SensorD  
ON Insured_Customers.CustomerKey = SensorD.CustomerKey  
ORDER BY YearlyIncome  
  
CREATE CLUSTERED COLUMNSTORE INDEX CCI_FastCustomers ON Fast_Customers;  
```  

### <a name="exporting-data"></a>Exportación de datos  

La consulta siguiente exporta datos de SQL Server a Hadoop. Para ello, primero debe habilitar la exportación de PolyBase. Posteriormente, cree una tabla externa para el destino antes de realizar la exportación de datos.

```sql
-- Enable INSERT into external table  
sp_configure 'allow polybase export', 1;  
reconfigure  
  
-- Create an external table.
CREATE EXTERNAL TABLE [dbo].[FastCustomers2009] (  
      [FirstName] char(25) NOT NULL,
      [LastName] char(25) NOT NULL,
      [YearlyIncome] float NULL,
      [MaritalStatus] char(1) NOT NULL  
)  
WITH (  
      LOCATION='/old_data/2009/customerdata',  
      DATA_SOURCE = HadoopHDP2,  
      FILE_FORMAT = TextFileFormat,  
      REJECT_TYPE = VALUE,  
      REJECT_VALUE = 0  
);  

-- Export data: Move old data to Hadoop while keeping it query-able via an external table.  
INSERT INTO dbo.FastCustomer2009  
SELECT T.* FROM Insured_Customers T1 JOIN CarSensor_Data T2  
ON (T1.CustomerKey = T2.CustomerKey)  
WHERE T2.YearMeasured = 2009 and T2.Speed > 40;  
```  

## <a name="view-polybase-objects-in-ssms"></a>Ver objetos PolyBase en SSMS  

En SSMS, las tablas externas se muestran en una carpeta independiente llamada " **Tablas externas**". Los orígenes de datos y los formatos de archivo externos se encuentran en subcarpetas de **Recursos externos**.  
  
![Objetos PolyBase en SSMS](media/polybase-management.png)  

## <a name="next-steps"></a>Pasos siguientes

Descubra más formas de usar y supervisar PolyBase en los siguientes artículos:

[Grupos de escalabilidad horizontal de PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md).  
[Solución de problemas de PolyBase](polybase-troubleshooting.md).  
