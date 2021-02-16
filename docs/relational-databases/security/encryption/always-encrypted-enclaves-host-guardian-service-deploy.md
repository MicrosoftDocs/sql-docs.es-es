---
title: Implementación del servicio guardián de host
description: Implemente el servicio de protección de host para Always Encrypted con enclaves seguros.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f6890f9c6a2870379fb2a54589b4657fa12467fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100015923"
---
# <a name="deploy-the-host-guardian-service-for-ssnoversion-md"></a>Implementación del servicio de protección de host para [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

En este artículo se describe cómo implementar el servicio de protección de host (HGS) como un servicio de atestación para [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Antes de empezar, lea el artículo [Planificación de la atestación del Servicio de protección de host](./always-encrypted-enclaves-host-guardian-service-plan.md) para obtener una lista completa de los requisitos previos e instrucciones de arquitectura.

> [!NOTE]
> El administrador de HGS es responsable de ejecutar todos los pasos que se describen en este artículo. Vea [Roles y responsabilidades al configurar la atestación con HGS](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

## <a name="step-1-set-up-the-first-hgs-computer"></a>Paso 1: Configuración del primer equipo HGS

El servicio de protección de host (HGS) se ejecuta como un servicio en clúster en uno o varios equipos.
En este paso, configurará un nuevo clúster de HGS en el primer equipo.
Si ya tiene un clúster HGS y le está agregando más equipos para obtener alta disponibilidad, vaya al [paso 2: Incorporación de más equipos HGS al clúster](#step-2-add-more-hgs-computers-to-the-cluster).

Antes de empezar, asegúrese de que el equipo que está usando ejecuta Windows Server 2019 Standard o Datacenter Edition, que tiene privilegios de administrador local y que el equipo no está vinculado a un dominio de Active Directory.

1. Inicie sesión en el primer equipo HGS como administrador local y abra una consola de Windows PowerShell con privilegios elevados. Ejecute el siguiente comando para instalar el rol del servicio de protección de host. El equipo se reiniciará automáticamente para aplicar los cambios.

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. Una vez reiniciado el equipo HGS, ejecute los siguientes comandos en una consola de Windows PowerShell con privilegios elevados para instalar el nuevo bosque de Active Directory:

    ```powershell
    # Select the name for your new Active Directory root domain.
    # Make sure the name does not conflict with, and is not subordinate to, any existing domains on your network.
    $HGSDomainName = 'bastion.local'

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

    El equipo HGS se reiniciará de nuevo para finalizar la configuración del bosque de Active Directory. La próxima vez que inicie sesión, la cuenta de administrador será una cuenta de administrador de dominio. Se recomienda revisar el documento [Operaciones de los Servicios de dominio de Active Directory](/windows-server/identity/ad-ds/manage/component-updates/ad-ds-operations) para obtener más información sobre cómo administrar y proteger el nuevo bosque.

3. Después, configurará el clúster HGS e instalará el servicio de atestación mediante la ejecución del siguiente comando en una consola de Windows PowerShell con privilegios elevados:

    ```powershell
    # Note: the name you provide here will be shared by all HGS nodes and used to point your SQL Server computers to the HGS cluster.
    # For example, if you provide "attsvc" here, a DNS record for "attsvc.yourdomain.com" will be created for every HGS computer.
    Initialize-HgsAttestation -HgsServiceName 'hgs'
    ```

## <a name="step-2-add-more-hgs-computers-to-the-cluster"></a>Paso 2: Incorporación de más equipos HGS al clúster

Una vez configurados el primer equipo HGS y el clúster, puede agregar más servidores HGS para obtener alta disponibilidad.
Si solo va a configurar un servidor HGS (en un entorno de desarrollo y pruebas, por ejemplo), puede ir directamente al paso 3.

Igual que con el primer equipo HGS, asegúrese de que el equipo que está incorporando al clúster ejecuta Windows Server 2019 Standard o Datacenter Edition, que tiene privilegios de administrador local y que el equipo no está vinculado a un dominio de Active Directory.

1. Inicie sesión en el equipo como administrador local y abra una consola de Windows PowerShell con privilegios elevados. Ejecute el siguiente comando para instalar el rol del servicio de protección de host. El equipo se reiniciará automáticamente para aplicar los cambios.

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. Compruebe la configuración del cliente DNS en el equipo para asegurarse de que puede resolver el dominio HGS. El siguiente comando debe devolver una dirección IP para el servidor de HGS. Si no puede resolver el dominio HGS, puede que necesite actualizar la información del servidor DNS en el adaptador de red para usar el servidor DNS de HGS para la resolución de nombres.

    ```powershell
    # Change 'bastion.local' to the domain name you specified in Step 1.2
    nslookup bastion.local

    # If it fails, use sconfig.exe, option 8, to set the first HGS computer as your preferred DNS server.
    ```

3. Una vez reiniciado el equipo, ejecute el siguiente comando en una consola de Windows PowerShell con privilegios elevados para incorporar el equipo al dominio de Active Directory creado por el primer servidor HGS. Necesitará credenciales de administrador del dominio para que el dominio de AD ejecute este comando. Cuando se complete el comando, el equipo se reiniciará y se convertirá en un controlador de dominio de Active Directory para el dominio HGS.

    ```powershell
    # Provide the fully qualified HGS domain name
    $HGSDomainName = 'bastion.local'

    # Provide a domain administrator's credential for the HGS domain
    $DomainAdminCred = Get-Credential

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $DomainAdminCred -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

4. Después de que el equipo se reinicie, inicie sesión con credenciales de administrador del dominio. Abra una consola de Windows PowerShell con privilegios elevados y ejecute los siguientes comandos para configurar el servicio de atestación. Dado que el servicio de atestación es compatible con clústeres, replicará su configuración a partir de otros miembros del clúster. Los cambios realizados en las directivas de atestación de cualquier nodo HGS se aplicarán a todos los demás nodos.

    ```powershell
    # Provide the IP address of an existing, initialized HGS server
    # If you are using separate networks for cluster and application traffic, choose an IP address on the cluster network.
    # You can find the IP address of your HGS server by signing in and running "ipconfig /all"
    Initialize-HgsAttestation -HgsServerIPAddress '172.16.10.20'
    ```

5. Repita el paso 2 para cada equipo que desee agregar al clúster HGS.

## <a name="step-3-configure-a-dns-forwarder-to-your-hgs-cluster"></a>Paso 3: Configuración de un reenviador DNS para el clúster HGS

HGS ejecuta su propio servidor DNS, que contiene los registros de nombre necesarios para resolver el servicio de atestación.
Los equipos de SQL Server no podrán resolver estos registros hasta que configure el servidor DNS de la red para reenviar las solicitudes a los servidores DNS de HGS.

El proceso de configuración de reenviadores DNS es específico del proveedor, por lo que se recomienda que se ponga en contacto con el administrador de la red para obtener las instrucciones pertinentes para la red en cuestión.

Si usa el rol de servidor DNS de Windows Server para la red corporativa, el administrador de DNS puede crear un reenviador condicional en el dominio HGS para que solo se reenvíen las solicitudes del dominio HGS.
Por ejemplo, si los servidores HGS usan el nombre de dominio "bastion.local" y tienen las direcciones IP 172.16.10.20, 172.16.10.21 y 172.16.10.22, puede ejecutar el siguiente comando en el servidor DNS corporativo para configurar un reenviador condicional:

```powershell
# Tip: make sure to provide every HGS server's IP address
# If you use separate NICs for cluster and application traffic, use the application traffic NIC IP addresses here
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers "172.16.10.20", "172.16.10.21", "172.16.10.22"
```

## <a name="step-4-configure-the-attestation-service"></a>Paso 4: Configuración del servicio de atestación

HGS admite dos modos de atestación: atestación de TPM para la comprobación criptográfica de la integridad e identidad de cada equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y atestación de clave de host para la comprobación simple de la identidad del equipo [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Si aún no ha seleccionado un modo de atestación, consulte la información sobre la atestación en la [guía de planificación](./always-encrypted-enclaves-host-guardian-service-plan.md#attestation-modes) para obtener más información sobre las garantías de seguridad y los casos de uso de cada modo.

En los pasos de esta sección se configuran las directivas de atestación básicas para un modo de atestación determinado.
En el paso 4, registrará la información específica del host.
Si necesita cambiar el modo de atestación en el futuro, repita los pasos 3 y 4 y seleccione el modo de atestación deseado.

### <a name="switch-to-tpm-attestation"></a>Cambio a atestación de TPM

Para configurar la atestación de TPM en HGS, necesitará un equipo con acceso a Internet y al menos un equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con un chip TPM 2.0 rev. 1.16.

Para configurar HGS para que use el modo TPM, abra una consola de PowerShell con privilegios elevados y ejecute el siguiente comando:

```powershell
Set-HgsServer -TrustTpm
```

Ahora todos los equipos HGS del clúster usarán el modo TPM cuando un equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] intente realizar una atestación.

Para poder registrar información de TPM de los equipos de SQL Server con HGS, debe instalar los certificados raíz de la clave de aprobación (EK) desde los proveedores de TPM.
Todos los TPM físicos vienen configurados de fábrica con una sola clave de aprobación acompañada por un certificado de clave de aprobación que identifica al fabricante.
Este certificado garantiza que el TPM es auténtico.
Al registrar un nuevo TPM con HGS, HGS comprueba el certificado de la clave de aprobación comparando la cadena de certificados con una lista de certificados raíz de confianza.

Microsoft publica una lista de certificados raíz de proveedores de TPM conocidos que se pueden importar en el almacén de certificados raíz del TPM de confianza para HGS.
Si los equipos de SQL Server están virtualizados, tendrá que contactar con su proveedor de servicios en la nube o con el proveedor de la plataforma de virtualización para obtener información sobre cómo obtener la cadena de certificados para la clave de aprobación del TPM virtual.

Para descargar el paquete de certificados raíz del TPM de confianza de Microsoft para TPM físicos, lleve a cabo los pasos siguientes:

1. En un equipo con acceso a Internet, descargue el paquete de certificados raíz del TPM más reciente: [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

2. Compruebe la firma del archivo .cab para asegurarse de que es auténtico.

    ```powershell
    # Note: replace the path below with the correct one to the file you downloaded in step 1
    Get-AuthenticodeSignature ".\TrustedTpm.cab"
    ```

    > [!WARNING]
    > No continúe si la firma no es válida y póngase en contacto con el soporte técnico de Microsoft para obtener ayuda.

3. Mueva el archivo .cab a un directorio nuevo.

    ```powershell
    mkdir .\TrustedTpmCertificates
    expand.exe -F:* ".\TrustedTpm.cab" ".\TrustedTpmCertificates"
    ```

4. En el nuevo directorio, verá los directorios de cada proveedor de TPM. Puede eliminar los directorios de los proveedores que no utilice.

5. Copie todo el directorio "TrustedTpmCertificates" en el servidor HGS.

6. Abra una consola de PowerShell con privilegios elevados en el servidor HGS y ejecute el siguiente comando para importar todos los certificados intermedios y raíz del TPM:

    ```powershell
    # Note: replace the path below with the correct location of the TrustedTpmCertificates folder on your HGS computer
    cd "C:\scratch\TrustedTpmCertificates"
    .\setup.cmd
    ```

7. Repita los pasos 5 y 6 para cada equipo HGS.

Si ha obtenido certificados de entidad de certificación intermedios y raíz del OEM, del proveedor de servicios en la nube o del proveedor de la plataforma de virtualización, puede importar directamente los certificados en el almacén de certificados de la máquina local correspondiente: `TrustedHgs_RootCA` o `TrustedHgs_IntermediateCA`. Por ejemplo, en PowerShell:

```powershell
# Imports MyCustomTpmVendor_Root.cer to the local machine's "TrustedHgs_RootCA" store
Import-Certificate -FilePath ".\MyCustomTpmVendor_Root.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedHgs_RootCA"
```

### <a name="switch-to-host-key-attestation"></a>Cambio a atestación de clave de host

Para usar la atestación de clave de host, ejecute el comando siguiente en un servidor HGS en una consola de PowerShell con privilegios elevados:

```powershell
Set-HgsServer -TrustHostKey
```

Ahora todos los equipos HGS del clúster usarán el modo de clave host cuando un equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] intente realizar una atestación.

## <a name="step-5-configure-the-hgs-https-binding"></a>Paso 5: Configuración del enlace HTTPS de HGS

En una instalación predeterminada, HGS solo expone un enlace HTTP (puerto 80).
Puede configurar un enlace HTTPS (puerto 443) para cifrar todas las comunicaciones entre equipos de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y HGS.
Se recomienda que todas las instancias de producción de HGS usen un enlace HTTPS.

1. Puede obtener un certificado TLS en su entidad de certificación, mediante el nombre del servicio HGS totalmente cualificado del paso 1.3 como nombre de sujeto. Si no conoce el nombre del servicio, puede encontrarlo ejecutando `Get-HgsServer` en cualquier equipo HGS. Puede agregar nombres DNS alternativos a la lista de nombres alternativos de sujeto si los equipos de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] usan otro nombre DNS para acceder al clúster de HGS (por ejemplo, si HGS está detrás de un equilibrador de carga de red con otra dirección).

2. En el equipo HGS, use [Set-HgsServer](/powershell/module/hgsserver/set-hgsserver) para habilitar el enlace HTTPS y especifique el certificado TLS obtenido en el paso anterior. Si el certificado ya está instalado en el equipo en el almacén de certificados local, use el siguiente comando para registrarlo con HGS:

    ```powershell
    # Note: you'll need to know the thumbprint for your certificate to configure HGS this way
    Set-HgsServer -Http -Https -HttpsCertificateThumbprint "54A043386555EB5118DB367CFE38776F82F4A181"
    ```

    Si ha exportado el certificado (con una clave privada) a un archivo PFX protegido por contraseña, puede registrarlo con HGS mediante la ejecución del siguiente comando:

    ```powershell
    $PFXPassword = Read-Host -AsSecureString -Prompt "PFX Password"
    Set-HgsServer -Http -Https -HttpsCertificatePath "C:\path\to\hgs_tls.pfx" -HttpsCertificatePassword $PFXPassword
    ```

3. Repita los pasos 1 y 2 para cada equipo HGS del clúster. Los certificados TLS no se replican automáticamente entre los nodos HGS. Además, cada equipo HGS puede tener su propio certificado TLS único, siempre que el sujeto coincida con el nombre del servicio HGS.

## <a name="step-6-determine-and-share-the-hgs-attestation-url"></a>Paso 6: Determinación y uso compartido de la dirección URL de atestación de HGS

Como administrador de HGS, debe compartir la dirección URL de atestación de HGS con los administradores de equipos con SQL Server y los administradores de aplicaciones de la organización. Los administradores de equipos con SQL Server necesitarán la dirección URL de atestación para comprobar que los equipos con SQL Server pueden realizar la atestación con HGS. Los administradores de aplicaciones necesitarán la dirección URL de atestación para configurar cómo se conectan sus aplicaciones a SQL Server.

Para determinar la dirección URL de atestación, ejecute el cmdlet siguiente.

```powershell
Get-HGSServer
```
La salida del comando será similar a esta:

```
Name                           Value                                                                         
----                           -----                                                                         
AttestationOperationMode       HostKey                                                                       
AttestationUrl                 {http://hgs.bastion.local/Attestation}                                        
KeyProtectionUrl               {}         
```

La dirección URL de atestación de HGS es el valor de la propiedad AttestationUrl.

## <a name="next-steps"></a>Pasos siguientes

- [Registro de equipos con HGS](./always-encrypted-enclaves-host-guardian-service-register.md)