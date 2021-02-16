---
title: Solución de problemas de conectividad de Kerberos con PolyBase | Microsoft Docs
description: Para solucionar problemas de autenticación de PolyBase con un clúster de Hadoop protegido con Kerberos, puede usar el diagnóstico interactivo integrado en PolyBase.
author: alazad-msft
ms.author: alazad
ms.reviewer: mikeray
ms.technology: polybase
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/02/2019
ms.prod: sql
ms.prod_service: polybase, sql-data-warehouse, pdw
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 0734f0a8163486ab46ef8143966a24adee35194d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079426"
---
# <a name="troubleshoot-polybase-kerberos-connectivity"></a>Solución de problemas de conectividad de Kerberos con PolyBase

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Puede usar el diagnóstico interactivo integrado en PolyBase para ayudar a solucionar problemas de autenticación cuando se usa PolyBase en un clúster de Hadoop protegido con Kerberos. 

Este artículo sirve como guía para describir el proceso de depuración de ese tipo de problemas mediante este diagnóstico integrado.

> [!TIP]
> En lugar de seguir los pasos descritos en esta guía, puede ejecutar [HDFS Kerberos Tester](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/hdfs-kerberos-tester) para solucionar problemas de las conexiones Kerberos de HDFS para PolyBase, cuando experimente un error de Kerberos de HDFS al crear una tabla externa en un clúster de HDFS protegido con Kerberos.
> Esta herramienta le ayudará a evitar incidencias que no son de SQL Server, para ayudarle a concentrarse en la resolución de problemas de instalación de HDFS de Kerberos, en especial la identificación de incidencias de errores de configuración de nombre de usuario y contraseña, y de instalación de clúster de Kerberos.      
> Esta herramienta es completamente independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Está disponible como Jupyter Notebook y requiere Azure Data Studio.

## <a name="prerequisites"></a>Requisitos previos

1. [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] RTM CU6 / [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU3 / [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] o superior con PolyBase instalado
1. Un clúster de Hadoop (Cloudera o Hortonworks) protegido con Kerberos (Active Directory o MIT)

## <a name="introduction"></a>Introducción
Ayuda a comprender el protocolo Kerberos por encima. Aquí participan tres actores:

1. Cliente Kerberos (SQL Server)
1. Recursos protegidos (HDFS, MR2, YARN, historial de trabajos, etc.)
1. Centro de distribución de claves (que en Active Directory se denomina controlador de dominio)

Cada uno de los recursos protegidos de Hadoop se registra en el **Centro de distribución de claves (KDC)** con un **Nombre de entidad de seguridad de servicio (SPN)** único cuando Kerberos se configura en el clúster de Hadoop. El objetivo es que el cliente obtenga un vale de usuario temporal, llamado **Vale de concesión de vales (TGT)** , para solicitar otro vale temporal, llamado **Vale de servicio (ST)** , desde el Centro de distribución de claves contra el nombre de entidad de seguridad de servicio que desea acceder.  

En PolyBase, cuando se solicita autenticación con respecto a cualquier recurso protegido con Kerberos, se produce el siguiente protocolo de enlace de cuatro recorridos de ida y vuelta:

1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se conecta con el Centro de distribución de claves y obtiene un TGT para el usuario. El TGT se cifra con la clave privada del Centro de distribución de claves.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] llama al recurso protegido de Hadoop, HDFS, y determina para qué nombre de entidad de seguridad de servicio necesita un ST.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelve al Centro de distribución de claves, devuelve el TGT y solicita un ST para acceder a ese recurso protegido concreto. El ST se cifra con la clave privada del servicio protegido.
1. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desvía el ST a Hadoop y se autentica para que se cree una sesión sobre ese servicio.

![Polybase SQL Server](./media/polybase-sqlserver.png)

Los problemas con la autenticación pertenecen a uno o más de los cuatro pasos mencionados. Para facilitar una depuración más rápida, PolyBase introdujo una herramienta de diagnóstico integrada para permitir la identificación del punto de error.

## <a name="troubleshooting"></a>Solución de problemas

PolyBase tiene los siguientes archivos XML de configuración que contienen las propiedades del clúster de Hadoop:

- core-site.xml
- hdfs-site.xml
- hive-site.xml
- jaas.conf
- mapred-site.xml
- yarn-site.xml

Estos archivos se encuentran en:

`\[System Drive\]:{install path}\{MSSQL##.INSTANCENAME}\MSSQL\Binn\PolyBase\Hadoop\conf`

Por ejemplo, el valor predeterminado para [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] es `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase\Hadoop\conf`.

Actualice **core-site.xml** y agregue las tres propiedades siguientes. Establezca los valores según el entorno:

```xml
<property>
    <name>polybase.kerberos.realm</name>
    <value>CONTOSO.COM</value>
</property>
<property>
    <name>polybase.kerberos.kdchost</name>
    <value>kerberos.contoso.com</value>
</property>
<property>
    <name>hadoop.security.authentication</name>
    <value>KERBEROS</value>
</property>
```
> [!NOTE]
> El valor de la propiedad `polybase.kerberos.realm` debe estar en mayúsculas.

Los otros XML también se deberán actualizar más adelante si se desean operaciones de inserción pero, con solo este archivo configurado, al menos se debería poder acceder al sistema de archivos HDFS.

La herramienta se ejecuta de forma independiente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], por lo que no es necesario que esté en ejecución ni reiniciarla si se realizan actualizaciones de los archivos XML de configuración. Para ejecutar la herramienta, ejecute los comandos siguientes en el host con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instalado:

```cmd
> cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\PolyBase  
> java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge {Name Node Address} {Name Node Port} {Service Principal} {Filepath containing Service Principal's Password} {Remote HDFS file path (optional)}
```

## <a name="arguments"></a>Argumentos

| Argumento | Descripción|
| --- | --- |
| *Dirección del nodo de nombre* | La dirección IP o el nombre de dominio completo del nodo de nombre. Hace referencia al argumento "LOCATION" de CREATE EXTERNAL DATA SOURCE T-SQL. Nota: En la versión SQL Server 2019 de la herramienta es necesario que *hdfs:\/\/* preceda a la dirección IP o el FQDN.|
| *Puerto del nodo de nombre* | El puerto del nodo de nombre. Hace referencia al argumento "LOCATION" de CREATE EXTERNAL DATA SOURCE T-SQL. Por ejemplo, 8020. |
| *Entidad de seguridad de servicio* | La entidad de servicio de administración del Centro de distribución de claves. Coincide con el argumento "IDENTITY" de `CREATE DATABASE SCOPED CREDENTIAL` T-SQL.|
| *Contraseña del servicio* | En lugar de escribir la contraseña en la consola, almacénela en un archivo y pase aquí la ruta de acceso al archivo. El contenido del archivo debe coincidir con el que use como argumento "SECRET" en `CREATE DATABASE SCOPED CREDENTIAL` T-SQL. |
| *Ruta de acceso al archivo HDFS remoto (opcional)* | La ruta de acceso de un archivo existente al cual acceder. Si no se especifica, se usará la raíz "/". |

## <a name="example"></a>Ejemplo

```cmd
java -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
```

El resultado es detallado para una mejor depuración, pero hay solo cuatro puntos de comprobación que observar, independientemente de si usa MIT o AD. Corresponden a los cuatro pasos descritos anteriormente. 

Los extractos siguientes provienen de un Centro de distribución de claves de MIT. Puede hacer referencia a salidas de ejemplo completas de MIT y AD en las referencias que aparecen al final de este artículo.

## <a name="checkpoint-1"></a>Punto de comprobación 1
Debe haber un volcado hexadecimal de un vale con `Server Principal = krbtgt/MYREALM.COM@MYREALM.COM`. Indica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ha autenticado correctamente en el Centro de distribución de claves y ha recibido un TGT. Si no es así, el problema se encuentra estrictamente entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el Centro de distribución de claves, no en Hadoop.

PolyBase **no** admite relaciones de confianza entre AD y MIT y se debe configurar respecto del mismo Centro de distribución de claves que se configuró en el clúster de Hadoop. En dichos entornos, funcionará la creación manual de una cuenta de servicio en ese Centro de distribución de claves y su uso para realizar la autenticación.

```cmd
|>>> KrbAsReq creating message 
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=143 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=143 
 >>> KrbKdcReq send: #bytes read=646 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbAsRep cons in KrbAsReq.getReply myuser 
 [2017-04-25 21:34:33,548] INFO 687[main] - com.microsoft.polybase.client.KerberosSecureLogin.secureLogin(KerberosSecureLogin.java:97) - Subject: 
 Principal: admin_user@CONTOSO.COM 
 Private Credential: Ticket (hex) = 
 0000: 61 82 01 48 30 82 01 44 A0 03 02 01 05 A1 0E 1B a..H0..D........ 
 0010: 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 21 30 .CONTOSO.COM.!0 
 0020: 1F A0 03 02 01 02 A1 18 30 16 1B 06 6B 72 62 74 ........0...krbt 
 0030: 67 74 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D gt..CONTOSO.COM 
 0040: A3 82 01 08 30 82 01 04 A0 03 02 01 10 A1 03 02 ....0........... 
 *[...Condensed...]* 
 0140: 67 6D F6 41 6C EB E0 C3 3A B2 BD B1 gm.Al...:... 
 Client Principal = admin_user@CONTOSO.COM 
 Server Principal = krbtgt/CONTOSO.COM@CONTOSO.COM 
 *[...Condensed...]* 
 [2017-04-25 21:34:34,500] INFO 1639[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1579) - Successfully authenticated against KDC server. 
```

## <a name="checkpoint-2"></a>Punto de comprobación 2
PolyBase intentará acceder a HDFS, pero no podrá hacerlo porque la solicitud no contenía el vale de servicio necesario.

```cmd
 [2017-04-25 21:34:34,501] INFO 1640[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1584) - Attempting to access external filesystem at URI: hdfs://10.193.27.232:8020 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Entered Krb5Context.initSecContext with state=STATE_NEW 
 Found ticket for admin_user@CONTOSO.COM to go to krbtgt/CONTOSO.COM@CONTOSO.COM expiring on Wed Apr 26 21:34:33 UTC 2017 
 Service ticket not found in the subject 
```

## <a name="checkpoint-3"></a>Punto de comprobación 3
Un segundo volcado hexadecimal indica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha usado correctamente el TGT y ha adquirido el vale de servicio aplicable para el SPN del nodo de nombre del Centro de distribución de claves.

```cmd
 >>> KrbKdcReq send: kdc=kerberos.contoso.com UDP:88, timeout=30000, number of retries =3, #bytes=664 
 >>> KDCCommunication: kdc=kerberos.contoso.com UDP:88, timeout=30000,Attempt =1, #bytes=664 
 >>> KrbKdcReq send: #bytes read=669 
 >>> KdcAccessibility: remove kerberos.contoso.com 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 >>> KrbApReq: APOptions are 00100000 00000000 00000000 00000000 
 >>> EType: sun.security.krb5.internal.crypto.Des3CbcHmacSha1KdEType 
 Krb5Context setting mySeqNumber to: 1033039363 
 Created InitSecContextToken: 
 0000: 01 00 6E 82 02 4B 30 82 02 47 A0 03 02 01 05 A1 ..n..K0..G...... 
 0010: 03 02 01 0E A2 07 03 05 00 20 00 00 00 A3 82 01 ......... ...... 
 0020: 63 61 82 01 5F 30 82 01 5B A0 03 02 01 05 A1 0E ca.._0..[....... 
 0030: 1B 0C 41 50 53 48 44 50 4D 53 2E 43 4F 4D A2 26 ..CONTOSO.COM.& 
 0040: 30 24 A0 03 02 01 00 A1 1D 30 1B 1B 02 6E 6E 1B 0$.......0...nn. 
 0050: 15 73 68 61 73 74 61 2D 68 64 70 32 35 2D 30 30 .hadoop-hdp25-00 
 0060: 2E 6C 6F 63 61 6C A3 82 01 1A 30 82 01 16 A0 03 .local....0..... 
 0070: 02 01 10 A1 03 02 01 01 A2 82 01 08 04 82 01 04 ................ 
 *[...Condensed...]* 
 0240: 03 E3 68 72 C4 D2 8D C2 8A 63 52 1F AE 26 B6 88 ..hr.....cR..&.. 
 0250: C4 . 
```

## <a name="checkpoint-4"></a>Punto de comprobación 4
Por último, las propiedades de la ruta de acceso de destino se deberían imprimir junto con un mensaje de confirmación. Las propiedades del archivo confirman que Hadoop ha autenticado [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el ST y que se ha concedido acceso al recurso protegido a una sesión.

Si llega a este punto, se confirma que: (i) los tres actores se pueden comunicar correctamente, (ii) los archivos core-site.xml y jaas.conf son correctos y (iii) el Centro de distribución de claves reconoció sus credenciales.

```cmd
 [2017-04-25 21:34:35,096] INFO 2235[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1586) - File properties for "/": FileStatus{path=hdfs://10.193.27.232:8020/; isDirectory=true; modification_time=1492028259862; access_time=0; owner=hdfs; group=hdfs; permission=rwxr-xr-x; isSymlink=false} 
 [2017-04-25 21:34:35,098] INFO 2237[main] - com.microsoft.polybase.client.HdfsBridge.main(HdfsBridge.java:1587) - Successfully accessed the external file system. 
```

## <a name="common-errors"></a>Errores comunes
Si se ejecutó la herramienta y *no* se imprimieron las propiedades de archivo de la ruta de acceso de destino (punto de comprobación 4), debería haber una excepción a medio camino. Revísela y considere el contexto en que se produjo dentro del flujo de cuatro pasos. Considere los siguientes problemas comunes que pueden haberse producido, en orden:

| Excepción y mensajes | Causa | 
| --- | --- |
| org.apache.hadoop.security.AccessControlException<br>La autenticación SIMPLE no está habilitada. Disponible:[TOKEN, KERBEROS] | El archivo core-site.xml no tiene la propiedad hadoop.security.authentication establecida en "KERBEROS".|
|javax.security.auth.login.LoginException<br>No se encuentra el cliente en la base de datos de Kerberos (6) - CLIENT_NOT_FOUND |    La entidad de servicio de administración que se suministró no existe en el dominio kerberos especificado en core-site.xml.|
| javax.security.auth.login.LoginException<br> Suma de comprobación errónea |La entidad de servicio de administración existe, pero la contraseña es incorrecta. |
| Nombre de configuración nativa: C:\Windows\krb5.ini<br>Cargado desde la configuración nativa | Este mensaje indica que el módulo krb5LoginModule de Java ha detectado configuraciones de cliente personalizadas en el equipo. Compruebe la configuración de cliente personalizada porque puede estar causando problemas. |
| javax.security.auth.login.LoginException<br>java.lang.IllegalArgumentException<br>Nombre de entidad de seguridad no válido admin_user@CONTOSO.COM: org.apache.hadoop.security.authentication.util.KerberosName$NoMatchingRule: Ninguna regla se aplica a admin_user@CONTOSO.COM. | Agregue la propiedad “hadoop.security.auth_to_local” a core-site.xml con las reglas correspondientes por clúster de Hadoop. |
| java.net.ConnectException<br>Intento de acceder al sistema de archivos externo en el URI: hdfs://10.193.27.230:8020<br>Error de llamada desde IAAS16981207/10.107.0.245 a 10.193.27.230:8020 en la excepción de conexión | La autenticación en el Centro de distribución de claves se realizó correctamente, pero no se pudo tener acceso al nodo de nombre de Hadoop. Compruebe el puerto y la dirección IP del nodo de nombre. Compruebe que el firewall esté deshabilitado en Hadoop. |
| java.io.FileNotFoundException<br>El archivo no existe: /test/data.csv |    La autenticación se realizó correctamente, pero la ubicación especificada no existe. Compruebe la ruta de acceso o pruebe primero con la raíz "/". |

## <a name="debugging-tips"></a>Sugerencias de depuración

### <a name="mit-kdc"></a>KDC de MIT  
Puede ver todos los nombres de entidad de seguridad de servicio registrados con el KDC, incluidos los administradores, si ejecuta **kadmin.local** > (inicio de sesión de administrador) > **listprincs** en el host de KDC o en cualquier cliente de KDC configurado. Si Kerberos está bien configurado en el clúster de Hadoop, debería haber un SPN para cada uno de los servicios disponibles en el clúster (por ejemplo: `nn`, `dn`, `rm`, `yarn`, `spnego`, etc.) De manera predeterminada, puede ver los archivos keytab correspondientes (sustitutos de la contraseña) en **/etc/security/keytabs**. Se cifran con la clave privada del Centro de distribución de claves.  

Considere también la posibilidad de usar [`kinit`](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) para comprobar las credenciales de administrador en el Centro de distribución de claves localmente. Un ejemplo de uso sería: `kinit identity@MYREALM.COM`. Un mensaje que aparece para solicitar contraseña indica que existe la identidad.  
De manera predeterminada, los registros de KDC están disponibles en **/var/log/krb5kdc.log**, lo que incluye todas las solicitudes de vales, incluida la dirección IP del cliente que hizo la solicitud. Debe haber dos solicitudes desde la dirección IP del equipo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se ha ejecutado la herramienta: en primer lugar, para el TGT del servidor de autenticación como **AS\_REQ**, seguido de **TGS\_REQ** para el vale de servicio desde el servidor de concesión de vales.

```bash
 [root@MY-KDC log]# tail -2 /var/log/krb5kdc.log 
 May 09 09:48:26 MY-KDC.local krb5kdc[2547](info): **AS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **krbtgt/CONTOSO.COM@CONTOSO.COM** 
 May 09 09:48:29 MY-KDC.local krb5kdc[2547](info): **TGS_REQ** (3 etypes {17 16 23}) 10.107.0.245: ISSUE: authtime 1494348506, etypes {rep=16 tkt=16 ses=16}, admin_user@CONTOSO.COM for **nn/hadoop-hdp25-00.local@CONTOSO.COM** 
```

### <a name="active-directory"></a>Active Directory 
En Active Directory, puede ver los nombres de entidad de seguridad de servicio si va a Panel de control > Usuarios y equipos de Active Directory > *MyRealm* > *MyOrganizationalUnit*. Si Kerberos está bien configurado en el clúster de Hadoop, hay un SPN para cada uno de los servicios disponibles (por ejemplo: `nn`, `dn`, `rm`, `yarn`, `spnego`, etc.)

### <a name="general-debugging-tips"></a>Sugerencias generales de depuración
Resulta útil tener cierta experiencia en Java para revisar los registros y depurar los problemas de Kerberos, que son independientes de la característica PolyBase de SQL Server.

Si sigue teniendo problemas para acceder a Kerberos, siga los pasos que hay a continuación para depurar:

1. Asegúrese de que puede acceder a los datos de HDFS de Kerberos desde fuera de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Puede: 

    - Escribir su propio programa de Java, o
    - Usar la clase `HdfsBridge` desde la carpeta de instalación de PolyBase. Por ejemplo:

      ```java
      -classpath ".\Hadoop\conf;.\Hadoop\*;.\Hadoop\HDP2_2\*" com.microsoft.polybase.client.HdfsBridge 10.193.27.232 8020 admin_user C:\temp\kerberos_pass.txt
      ```

     En el ejemplo anterior, `admin_user` solo incluye el nombre de usuario, ningún elemento del dominio.

2. Si no puede acceder a datos de HDFS de Kerberos desde fuera de PolyBase:
    - Existen dos tipos de autenticación de Kerberos: Autenticación de Kerberos de Active Directory y autenticación de Kerberos de MIT.
    - Asegúrese de que el usuario existe en la cuenta de dominio y use la misma cuenta de usuario al intentar acceder a HDFS.

3. En el caso de Kerberos de Active Directory, asegúrese de que puede ver el vale en caché con el comando `klist` en Windows.
    - Inicie sesión en el equipo de PolyBase y ejecute `klist` y `klist tgt` en el símbolo del sistema para ver si el Centro de distribución de claves, el nombre de usuario y los tipos de cifrado son correctos.

4. Si el Centro de distribución de claves solo puede admitir AES256, asegúrese de que los [archivos de directiva de JCE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) estén instalados.

## <a name="see-also"></a>Consulte también
[Integrating PolyBase with Cloudera using Active Directory Authentication](/archive/blogs/microsoftrservertigerteam/integrating-polybase-with-cloudera-using-active-directory-authentication) (Integración de PolyBase con Cloudera mediante Autenticación de Active Directory)  
[Cloudera's Guide to setting up Kerberos for CDH](https://www.cloudera.com/documentation/enterprise/5-6-x/topics/cm_sg_principal_keytab.html) (Guía de Cloudera para configurar Kerberos para CDH)  
[Hortonworks' Guide to Setting up Kerberos for HDP](https://docs.hortonworks.com/HDPDocuments/Ambari-2.2.0.0/bk_Ambari_Security_Guide/content/ch_configuring_amb_hdp_for_kerberos.html) (Guía de Hortonworks para configurar Kerberos para HDP)  
[Solución de problemas de PolyBase](polybase-troubleshooting.md)