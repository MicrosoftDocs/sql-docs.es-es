---
title: Configuración de la seguridad de Hadoop de polybase
description: Proporciona una referencia para las distintas opciones de configuración que afectan a la conectividad polybase de AP a Hadoop.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 3b99f9d274f7c2d30bf8fae326953e2f57e9810e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052736"
---
# <a name="configure-polybase-hadoop-security"></a>Configuración de la seguridad de Hadoop de polybase

En este artículo se proporciona una referencia para las distintas opciones de configuración que afectan a la conectividad polybase de AP a Hadoop. Para ver un tutorial sobre qué es polybase, consulte [¿Qué es](configure-polybase-connectivity-to-external-data.md)polybase?.

> [!NOTE]
> En APS, se necesitan cambios en los archivos XML en todos los nodos de proceso y el nodo de control.
> 
> Tenga especial cuidado al modificar archivos XML en APS. Cualquier etiqueta que falte o carácter no deseado puede invalidar el archivo XML impidiendo el usablilty de la característica.
> Los archivos de configuración de Hadoop se encuentran en la siguiente ruta de acceso:  
> ```  
> C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf 
> ``` 
> Cualquier cambio en los archivos XML requiere un reinicio del servicio para que sea efectivo.

## <a name="hadooprpcprotection-setting"></a><a id="rpcprotection"></a> Configuración de Hadoop.RPC.Protection

Una forma habitual de proteger la comunicación en un clúster de Hadoop consiste en cambiar la configuración de hadoop.rpc.protection a "Privacy" (Privacidad) o "Integrity" (Integridad). De forma predeterminada, PolyBase da por hecho que la configuración está establecida en "Autenticar". Para reemplazar esta configuración predeterminada, agregue la siguiente propiedad al archivo core-site.xml. Si cambia esta configuración, permitirá que se efectúe una transferencia de datos segura entre los nodos de Hadoop y que se establezca una conexión SSL con SQL Server.

```xml
<!-- RPC Encryption information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG -->
   <property>
     <name>hadoop.rpc.protection</name>
     <value></value>
   </property> 
```

## <a name="kerberos-configuration"></a><a id="kerberossettings"></a> Configuración de Kerberos  

Tenga en cuenta que cuando PolyBase se autentica en un clúster protegido de Kerberos, espera que la configuración de hadoop.rpc.protection esté establecida en "Autenticar" de forma predeterminada. Esta acción dejará la comunicación de datos entre los nodos Hadoop sin cifrar. Para usar la configuración de "Privacidad" o "Integridad" para hadoop.rpc.protection, actualice el archivo core-site.xml en el servidor de PolyBase. Para más información, vea la sección anterior titulada [Conectarse al clúster de Hadoop con la opción de configuración Hadoop.RPC.Protection](#rpcprotection).

Para conectarse a un clúster de Hadoop protegido con Kerberos mediante MIT KDC, se necesitan los siguientes cambios en todos los nodos de proceso de APS y el nodo de control:

1. Busque los directorios de configuración de Hadoop en la ruta de instalación de APS. Normalmente, la ruta de acceso es:  

   ```  
   C:\Program Files\Microsoft SQL Server Parallel Data Warehouse\100\Hadoop\conf  
   ```  

2. Busque el valor de configuración del lado de Hadoop de las claves de configuración que se muestran en la tabla. (En el equipo de Hadoop, busque los archivos en el directorio de configuración de Hadoop).  
   
3. Copie los valores de configuración en la propiedad Value en los archivos correspondientes en el equipo de SQL Server.  
   
   |**#**|**Archivo de configuración**|**Clave de configuración**|**Acción**|  
   |------------|----------------|---------------------|----------|   
   |1|core-site.xml|polybase.kerberos.kdchost|Especifique el nombre de host KDC. Por ejemplo: kerberos.su-dominio.com.|  
   |2|core-site.xml|polybase.kerberos.realm|Especifique el dominio Kerberos. Por ejemplo: SU-DOMINIO.COM|  
   |3|core-site.xml|hadoop.security.authentication|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: KERBEROS<br></br>**Nota de seguridad:** KERBEROS debe estar en mayúsculas. Si aparece en minúsculas, puede que no esté activado.|   
   |4|hdfs-site.xml|dfs.namenode.kerberos.principal|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: hdfs/_HOST@YOUR-REALM.COM|  
   |5|mapred-site.xml|mapreduce.jobhistory.principal|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: mapred/_HOST@YOUR-REALM.COM|  
   |6|mapred-site.xml|mapreduce.jobhistory.address|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: 10.193.26.174:10020|  
   |7|yarn-site.xml yarn.|yarn.resourcemanager.principal|Busque la configuración del lado de Hadoop y cópiela en el equipo de SQL Server. Por ejemplo: yarn/_HOST@YOUR-REALM.COM|  

**core-site.xml**
```xml
<property>
  <name>polybase.kerberos.realm</name>
  <value></value>
</property>
<property>
  <name>polybase.kerberos.kdchost</name>
  <value></value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```

**hdfs-site.xml**
```xml
<property>
  <name>dfs.namenode.kerberos.principal</name>
  <value></value> 
</property>
```

**mapred-site.xml**
```xml
<property>
  <name>mapreduce.jobhistory.principal</name>
  <value></value>
</property>
<property>
  <name>mapreduce.jobhistory.address</name>
  <value></value>
</property>
```

**yarn-site.xml**
```xml
<property>
  <name>yarn.resourcemanager.principal</name>
  <value></value>
</property>
```

4. Cree un objeto de credencial con ámbito de base de datos para especificar la información de autenticación para cada usuario de Hadoop. Vea [PolyBase T-SQL objects (Objetos T-SQL de PolyBase)](../relational-databases/polybase/polybase-t-sql-objects.md).

## <a name="hadoop-encryption-zone-setup"></a><a id="encryptionzone"></a> Configuración de zona de cifrado de Hadoop
Si usa la zona de cifrado de Hadoop, modifique core-site.xml y hdfs-site.xml de la siguiente manera. Proporcione la dirección IP en la que se ejecuta el servicio KMS con el número de puerto correspondiente. El puerto predeterminado para KMS en CDH es 16000.

**core-site.xml**
```xml
<property>
  <name>hadoop.security.key.provider.path</name>
  <value>kms://http@<ip address>:16000/kms</value> 
</property>
```

**hdfs-site.xml**
```xml
<property>
  <name>dfs.encryption.key.provider.uri</name>
  <value>kms://http@<ip address>:16000/kms</value>
</property>
<property>
  <name>hadoop.security.key.provider.path</name>
  <value>kms://http@<ip address>:16000/kms</value>
  </property>
```