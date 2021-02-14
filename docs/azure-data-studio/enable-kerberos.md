---
title: Conexión a una instancia de SQL Server mediante autenticación de Windows (Kerberos)
description: Obtenga información sobre cómo conectar Azure Data Studio a la instancia de SQL Server mediante la autenticación integrada de Microsoft Kerberos.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: how-to
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: 2a4e92577f9938b764a3754e19c27a8e7f22658b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048625"
---
# <a name="connect-azure-data-studio-to-sql-server-using-windows-authentication---kerberos"></a>Conexión de Azure Data Studio a SQL Server mediante autenticación de Windows: Kerberos

Azure Data Studio admite la conexión a SQL Server mediante Kerberos.

Para usar autenticación integrada (autenticación de Windows) en MacOS o Linux, debe configurar un *vale Kerberos* que vincule el usuario actual a una cuenta de dominio de Windows.

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesitará lo siguiente:

- Acceso a una máquina Windows unida a un dominio para consultar el controlador de dominio de Kerberos.
- SQL Server debe configurarse para permitir la autenticación Kerberos. En el caso del controlador de cliente que se ejecuta en Unix, la autenticación integrada solo se admite con Kerberos. Para más información, vea [Uso de la autenticación integrada de Kerberos para conectar con SQL Server](../connect/jdbc/using-kerberos-integrated-authentication-to-connect-to-sql-server.md). Debe haber nombres de entidad de seguridad de servicio registrados para cada instancia de SQL Server a la que se esté intentando conectar. Para más información, consulte [Registro de un nombre de entidad de servicio](/previous-versions/sql/sql-server-2008-r2/ms191153(v=sql.105)#SPN%20Formats).


## <a name="check-if-sql-server-has-a-kerberos-setup"></a>Comprobación de si SQL Server tiene configuración de Kerberos

Inicie sesión en la máquina host de SQL Server. En el símbolo del sistema de Windows, use `setspn -L %COMPUTERNAME%` para enumerar todos los nombres de entidad de seguridad de servicio (SPN) del host. Debería ver las entradas que comienzan por MSSQLSvc/HostName.Domain.com, lo que significa que SQL Server ha registrado un SPN y está listo para aceptar la autenticación Kerberos.

Si no tiene acceso al host de la instancia de SQL Server, desde cualquier otro sistema operativo Windows unido a la misma instancia de Active Directory, puede usar el comando `setspn -L <SQLSERVER_NETBIOS>`, donde *<SQLSERVER_NETBIOS>* es el nombre de equipo del host de la instancia de SQL Server.


## <a name="get-the-kerberos-key-distribution-center"></a>Obtener el Centro de distribución de claves de Kerberos

Busque el valor de configuración del Centro de distribución de claves (KDC) de Kerberos. Ejecute el siguiente comando en un equipo Windows que esté unido al dominio de Active Directory.

Inicie `cmd.exe` y ejecute `nltest`.

```
nltest /dsgetdc:DOMAIN.COMPANY.COM (where "DOMAIN.COMPANY.COM" maps to your domain's name)

Sample Output
DC: \\dc-33.domain.company.com
Address: \\2111:4444:2111:33:1111:ecff:ffff:3333
...
The command completed successfully
```
Copie el nombre del controlador de dominio que es el valor de configuración del KDC requerido. En este caso, es dc-33.domain.company.com.

## <a name="join-your-os-to-the-active-directory-domain-controller"></a>Unión del sistema operativo al controlador de dominio de Active Directory

### <a name="ubuntu"></a>Ubuntu
```bash
sudo apt-get install realmd krb5-user software-properties-common python-software-properties packagekit
```

Edite el archivo `/etc/network/interfaces` para que la dirección IP del controlador de dominio de Active Directory aparezca como dns-nameserver. Por ejemplo:

```/etc/network/interfaces
<...>
# The primary network interface
auth eth0
iface eth0 inet dhcp
dns-nameservers **<AD domain controller IP address>**
dns-search **<AD domain name>**
```

> [!NOTE]
> La interfaz de red (eth0) puede diferir en los distintos equipos. Para averiguar cuál es la que está usando, ejecute ifconfig y copie la interfaz que tiene una dirección IP y bytes transmitidos y recibidos.

Después de editar este archivo, reinicie el servicio de red:

```bash
sudo ifdown eth0 && sudo ifup eth0
```

Ahora compruebe que el archivo `/etc/resolv.conf` contiene una línea similar a la siguiente:

```Code
nameserver **<AD domain controller IP address>**
```

```bash
sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
<...>
* Success
```
   
### <a name="redhat-enterprise-linux"></a>RedHat Enterprise Linux
```bash
sudo yum install realmd krb5-workstation
```

Edite el archivo `/etc/sysconfig/network-scripts/ifcfg-eth0` (u otro archivo de configuración de interfaz que corresponda) para que la dirección IP del controlador de dominio de Active Directory aparezca como un servidor DNS:

```/etc/sysconfig/network-scripts/ifcfg-eth0
<...>
PEERDNS=no
DNS1=**<AD domain controller IP address>**
```

Después de editar este archivo, reinicie el servicio de red:

```bash
sudo systemctl restart network
```

Ahora compruebe que el archivo `/etc/resolv.conf` contiene una línea similar a la siguiente:  

```Code
nameserver **<AD domain controller IP address>**
```

```bash
sudo realm join contoso.com -U 'user@CONTOSO.COM' -v
<...>
* Success
   
```

### <a name="macos"></a>macOS

Una el equipo macOS al controlador de dominio de Active Directory mediante estos pasos.

## <a name="configure-kdc-in-krb5conf"></a>Configuración de KDC en krb5.conf

Edite el archivo `/etc/krb5.conf` en el editor que prefiera. Configure las claves siguientes:

```bash
sudo vi /etc/krb5.conf

[libdefaults]
  default_realm = DOMAIN.COMPANY.COM
 
[realms]
DOMAIN.COMPANY.COM = {
   kdc = dc-33.domain.company.com
}
```

Luego, guarde el archivo krb5.conf y salga.

> [!NOTE]
> El dominio debe estar en MAYÚSCULAS.


## <a name="test-the-ticket-granting-ticket-retrieval"></a>Prueba de la recuperación de un vale de concesión de vales

Obtenga un vale de concesión de vales (TGT) del KDC.

```bash
kinit username@DOMAIN.COMPANY.COM
```

Vea los vales disponibles mediante klist. Si el comando kinit se ha ejecutado correctamente, debería ver un vale.

```bash
klist

krbtgt/DOMAIN.COMPANY.COM@ DOMAIN.COMPANY.COM.
```

## <a name="connect-by-using-azure-data-studio"></a>Conexión mediante Azure Data Studio

1. Cree un nuevo perfil de conexión.

1. Seleccione **Autenticación de Windows** como tipo de autenticación.

1. Complete el perfil de conexión y seleccione **Conectar**.

Después de conectarse correctamente, el servidor aparece en la barra lateral **SERVIDORES**.
