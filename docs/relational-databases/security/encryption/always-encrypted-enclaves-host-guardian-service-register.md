---
title: Registro del equipo con el servicio de protección de host
description: Registre el equipo de SQL Server con el servicio de protección de host para Always Encrypted con enclaves seguros.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 64971ecd3ed3a821ca823ad3cc5d0d05d08669f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031447"
---
# <a name="register-computer-with-host-guardian-service"></a>Registro del equipo con el servicio de protección de host

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

En este artículo se describe cómo registrar equipos de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] para atestar con el servicio de protección de host (HGS).

> [!NOTE]
> Para el proceso de registro de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con HGS se necesita un esfuerzo conjunto del administrador de HGS y el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Vea [Roles y responsabilidades al configurar la atestación con HGS](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

Antes de empezar, asegúrese de que ha implementado al menos un equipo de HGS y ha configurado el servicio de atestación de HGS.
Para obtener más información, consulte [Implementación del servicio de protección de host para [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md).

## <a name="step-1-install-the-attestation-client-components"></a>Paso 1: Instalación de los componentes de cliente de atestación

> [!NOTE]
> Este paso lo debe realizar el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Para permitir que un cliente de SQL compruebe que está conectando con un equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] de confianza, el equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] debe realizar correctamente la atestación del servicio de protección de host.
El proceso de atestación se administra mediante un componente de Windows opcional denominado cliente del HGS.
En los pasos siguientes verá cómo instalar este componente y comenzar a realizar la atestación.

1. Asegúrese de que el equipo [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] cumple los [requisitos previos descritos en el documento de planificación de HGS](./always-encrypted-enclaves-host-guardian-service-plan.md#prerequisites).

2. Ejecute el siguiente comando en una consola de PowerShell con privilegios elevados para instalar la característica de compatibilidad de Hyper-V de protección de host, que contiene los componentes de cliente de HGS y de atestación.

    ```powershell
    Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
    ```

3. Reinicie el equipo para completar la instalación.

## <a name="step-2-verify-virtualization-based-security-is-running"></a>Paso 2: Verificación del funcionamiento de la seguridad basada en virtualización

> [!NOTE]
> Este paso lo debe realizar el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Al instalar la característica de compatibilidad de Hyper-V de protección de host, se configura y habilita automáticamente la seguridad basada en virtualización (VBS).
Los enclaves para Always Encrypted de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] están protegidos y se ejecutan dentro del entorno de VBS.
Puede ser que VBS no se inicie si el equipo no tiene un dispositivo IOMMU instalado y habilitado.
Para comprobar si se está ejecutando VBS, abra la herramienta de información del sistema mediante la ejecución de `msinfo32.exe` y busque los elementos `Virtualization-based security` hacia la parte inferior del Resumen del sistema.

![Captura de pantalla de Información del sistema en la que se muestra el estado y la configuración de la seguridad basada en la virtualización](./media/always-encrypted-enclaves/msinfo32-vbs-status.png)

El primer elemento que se va a comprobar es `Virtualization-based security`, que puede tener los tres valores siguientes:

- `Running` significa que la VBS está bien configurada y se ha podido iniciar correctamente. Si el equipo muestra este estado, puede ir directamente al paso 3.
- `Enabled but not running` significa que la VBS está configurada para ejecutarse, pero el hardware no tiene los requisitos de seguridad mínimos para ejecutar VBS. Es posible que tenga que cambiar la configuración del hardware en la BIOS o UEFI para habilitar características opcionales del procesador como IOMMU o, si el hardware realmente no admite las características necesarias, puede que necesite reducir los requisitos de seguridad de la VBS. Para obtener más información, siga leyendo esta sección.
- `Not enabled` significa que la VBS no está configurada para ejecutarse. La característica de compatibilidad de Hyper-V de protección de host habilita automáticamente la VBS. Por lo tanto, si ve este estado, es recomendable que repita el paso 1.

Si la VBS no se está ejecutando en el equipo, compruebe las propiedades de `Virtualization-based security`. Compare los valores del elemento `Required Security Properties` con los valores del elemento `Available Security Properties`.
Las propiedades necesarias deben ser iguales a las propiedades de seguridad disponibles, o un subconjunto de ellas, para que se ejecute la VBS.

En el contexto de atestación de enclaves de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], las propiedades de seguridad tienen la importancia siguiente:

- Siempre se requiere `Base virtualization support`, ya que representa las características de hardware mínimas necesarias para ejecutar un hipervisor.
- Se recomienda `Secure Boot`, aunque no es necesario para Always Encrypted de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. El arranque seguro protege contra rootkits, ya que requiere que se ejecute un cargador de arranque firmado por Microsoft inmediatamente después de que se complete la inicialización de UEFI. Si usa la atestación de módulo de plataforma segura (TPM), se medirá y aplicará la habilitación del arranque seguro independientemente de si la VBS está configurada para requerir un arranque seguro.
- Se recomienda `DMA Protection`, aunque no es necesario para Always Encrypted de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. La protección de DMA usa IOMMU para proteger la memoria de la VBS y del enclave ante ataques de acceso directo a la memoria. En un entorno de producción, siempre debe usar equipos con protección de DMA. En un entorno de desarrollo y pruebas, se admite eliminar el requisito de protección de DMA. Si la instancia de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] está virtualizada, lo más probable es que no tenga disponible protección de DMA y tendrá que eliminar el requisito de ejecución de la VBS. Revise el [modelo de confianza](./always-encrypted-enclaves-host-guardian-service-plan.md#trust-model) para obtener información sobre las garantías de seguridad reducidas al ejecutarse en una máquina virtual.

Antes de reducir las características de seguridad necesarias de VBS, consulte al proveedor de servicios en la nube o OEM si existe una manera de habilitar los requisitos de plataforma que faltan en UEFI o BIOS (por ejemplo, para habilitar el arranque seguro, Intel VT-d o AMD IOV).

Para cambiar las características de seguridad de la plataforma necesarias para la VBS, ejecute el siguiente comando en una consola de PowerShell con privilegios elevados:

```powershell
# Value 0 = No security features required (use this for Azure VMs)
# Value 1 = Only Secure Boot is required
# Value 2 = Only DMA protection is required (default configuration)
# Value 3 = Both Secure Boot and DMA protection are required
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
```

Después de cambiar el registro, reinicie el equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y compruebe si VBS se está ejecutando de nuevo.

Si el equipo está administrado por su empresa, la directiva de grupo o el administrador de puntos de conexión de Microsoft puede invalidar cualquier cambio que se realice en estas claves del registro después de reiniciar.
Contacte con el departamento de soporte técnico de TI para ver si implementan directivas que administran su configuración de VBS.

## <a name="step-3-configure-the-attestation-url"></a>Paso 3: Configurar la URL de atestación

> [!NOTE]
> Este paso lo debe realizar el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

A continuación, configurará el equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con la dirección URL del servicio de atestación de HGS, que ha obtenido del administrador de HGS.

En una consola de PowerShell con privilegios elevados, actualice y ejecute los siguientes comandos para configurar la URL de atestación.

- Reemplace `hgs.bastion.local` por el nombre del clúster HGS.
- Puede ejecutar `Get-HgsServer` en cualquier equipo HGS para obtener el nombre del clúster.
- La URL de atestación siempre debe terminar con `/Attestation`.
- SQL Server no se beneficia de las características de protección de claves de HGS. Por lo tanto, indique cualquier URL ficticia, como `http://localhost` hasta `-KeyProtectionServerUrl`.

```powershell
Set-HgsClientConfiguration -AttestationServerUrl "https://hgs.bastion.local/Attestation" -KeyProtectionServerUrl "http://localhost"
```

A menos que haya registrado este equipo con HGS antes, el comando informa de un error en la atestación. Este resultado es normal.

El campo `AttestationMode` de la salida del cmdlet indica qué modo de atestación usa HGS.

Vaya al [paso 4A](#step-4a-register-a-computer-in-tpm-mode) para registrar el equipo en modo TPM o al [paso 4B](#step-4b-register-a-computer-in-host-key-mode) para registrar el equipo en modo de clave de host.

## <a name="step-4a-register-a-computer-in-tpm-mode"></a>Paso 4A: Registro de un equipo en modo TPM

> [!NOTE]
> Este paso lo realizan de forma conjunta el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y el administrador de HGS. Vea las notas siguientes para obtener más información.

### <a name="prepare"></a>Preparación

> [!NOTE]
> Esta acción la debe realizar el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

En este paso, recopilará información sobre el estado de TPM del equipo y lo registrará con HGS.

Si el servicio de atestación de HGS está configurado para usar el modo de clave de host, vaya al [paso 4B](#step-4b-register-a-computer-in-host-key-mode).

Antes de empezar a recopilar las medidas de TPM, asegúrese de que está trabajando en una configuración correcta conocida del equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
El equipo debe tener instalado todo el hardware necesario y aplicar las últimas actualizaciones de firmware y software.
HGS mide los equipos comparándolos con esta base de referencia al realizar la atestación, por lo que es importante tener el estado más seguro e intencionado posible al recopilar las medidas de TPM.

Hay tres archivos de datos recopilados para la atestación de TPM, algunos de los cuales se pueden reutilizar si tiene equipos configurados de forma idéntica.

| Artefacto de atestación | ¿Qué mide? | Unicidad |
| -------------------- | ---------------- | ---------- |
| Identificador de plataforma  | La clave de aprobación pública en el TPM del equipo y el certificado de clave de aprobación del fabricante del TPM. | 1 para cada equipo |
| Base de referencia de TPM | Los registros de control de plataforma (PCR) del TPM que miden el firmware y la configuración del sistema operativo cargados durante el proceso de arranque. Algunos ejemplos son el estado de arranque seguro y si los volcados de memoria están cifrados. | Una base de referencia por configuración única de equipo (si el hardware y el software son idénticos, pueden usar la misma base de referencia) |
| Directiva de integridad de código | La directiva de [Windows Defender Application Control](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) en la que confía para proteger los equipos | Uno por directiva de integración continua única implementada en los equipos. |

Puede configurar más de un artefacto de atestación en HGS para prestar asistencia a una flota mixta de hardware y software.
HGS solo requiere que la atestación de un equipo coincida con una directiva de cada categoría de directivas.
Por ejemplo, si tiene tres bases de referencia de TPM registradas en HGS, las medidas del equipo pueden coincidir con cualquiera de esas bases de referencia para cumplir el requisito de la directiva.

### <a name="configure-a-code-integrity-policy"></a>Configuración de una directiva de integridad de código

> [!NOTE]
> Los pasos siguientes los debe realizar el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

HGS requiere que todos los equipos que se comparan en el modo TPM tengan aplicada una directiva de control de aplicaciones de Windows Defender (WDAC).
Las directivas de integridad de código de WDAC restringen el software que se puede ejecutar en un equipo comprobando cada proceso que intenta ejecutar código en una lista de editores de confianza y hashes de archivo.
En el caso de uso de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], los enclaves están protegidos por la seguridad basada en la virtualización y no se pueden modificar desde el sistema operativo del host, por lo que la rigurosidad de la directiva de WDAC no afecta a la seguridad de las consultas cifradas.
Por tanto, se recomienda implementar una directiva de modo de auditoría en los equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] para cumplir el requisito de atestación sin imponer restricciones adicionales en el sistema.

Si ya usa una directiva de integridad de código de WDAC personalizada en los equipos para proteger la configuración del sistema operativo, puede ir a la sección [Recopilación de información sobre la atestación de TPM](#collect-tpm-attestation-information).

1. Windows Server 2019, Windows 10 versión 1809 y los sistemas operativos posteriores disponen de directivas de ejemplo predefinidas. La directiva `AllowAll` permite la ejecución de cualquier software en el equipo sin restricciones. Para usar la directiva, conviértala a un formato binario que el sistema operativo y HGS entiendan. En una consola de PowerShell, ejecute los comandos siguientes para compilar la directiva `AllowAll`:

    ```powershell
    # We are changing the policy to disable enforcement and user mode code protection before compiling
    $temppolicy = "$HOME\Desktop\allowall_edited.xml"
    Copy-Item -Path "$env:SystemRoot\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml" -Destination $temppolicy
    Set-RuleOption -FilePath $temppolicy -Option 0 -Delete
    Set-RuleOption -FilePath $temppolicy -Option 3

    ConvertFrom-CIPolicy -XmlFilePath $temppolicy -BinaryFilePath "$HOME\Desktop\allowall_cipolicy.bin"
    ```

2. Siga las instrucciones que encontrará en la [guía de implementación del Control de aplicaciones de Microsoft Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide) para implementar el archivo `allowall_cipolicy.bin` en los equipos de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con la [directiva de grupo](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy). En el caso de los equipos del grupo de trabajo, siga el mismo proceso con el editor de la directiva de grupo local (`gpedit.msc`).

3. Ejecute `gpupdate /force` en los equipos de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] para configurar la nueva directiva de integridad de código y después reinicie los equipos para aplicar la directiva.

### <a name="collect-tpm-attestation-information"></a>Recopilación de información sobre la atestación de TPM

> [!NOTE]
> Los pasos siguientes los debe realizar el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Repita los pasos siguientes para cada equipo [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] en el que se vaya a realizar la atestación con HGS:

1. Con el equipo en buen estado, ejecute los siguientes comandos en PowerShell para recopilar la información de atestación de TPM:

    ```powershell
    # Collects the TPM EKpub and EKcert
    $name = $env:computername
    $path = "$HOME\Desktop"
    (Get-PlatformIdentifier -Name $name).Save("$path\$name-EK.xml")

    # Collects the TPM baseline (current PCR values)
    Get-HgsAttestationBaselinePolicy -Path "$path\$name.tcglog" -SkipValidation

    # Collects the applied CI policy, if one exists
    Copy-Item -Path "$env:SystemRoot\System32\CodeIntegrity\SIPolicy.p7b" -Destination "$path\$name-CIpolicy.bin"
    ```

2. Comparta los tres archivos de atestación con el administrador de HGS. 

### <a name="register-the-sql-server-computer-with-hgs"></a>Registro del equipo SQL Server con HGS

> [!NOTE]
> Los pasos siguientes los debe realizar el administrador de HGS.

Repita los pasos siguientes para cada equipo [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] en el que se vaya a realizar la atestación con HGS:

1. Copie en el servidor HGS los archivos de atestación que ha obtenido del administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. 
2. En el servidor HGS, ejecute los siguientes comandos en una consola de PowerShell con privilegios elevados para registrar el equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]:

    ```powershell
    # TIP: REMEMBER TO CHANGE THE FILENAMES
    # Registers the unique TPM with HGS (required for every computer)
    Add-HgsAttestationTpmHost -Path "C:\temp\SQL01-EK.xml"

    # Registers the TPM baseline (required ONCE for each unique hardware and software configuration)
    Add-HgsAttestationTpmPolicy -Name "MyHWSoftwareConfig" -Path "C:\temp\SQL01.tcglog"

    # Registers the CI policy (required ONCE for each unique CI policy)
    Add-HgsAttestationCiPolicy -Name "AllowAll" -Path "C:\temp\SQL01-CIpolicy.bin"
    ```

    > [!TIP]
    > Si se produce un error al intentar registrar el identificador de TPM único, asegúrese de que [ha importado los certificados intermedios y raíz de TPM](./always-encrypted-enclaves-host-guardian-service-deploy.md#switch-to-tpm-attestation) en el equipo HGS que está usando.

Además del identificador de plataforma, la base de referencia de TPM y la directiva de integridad de código, hay directivas integradas configuradas y aplicadas por HGS que es posible que deba cambiar.
Estas directivas integradas se miden a partir de la base de referencia de TPM que se recopila del servidor y representan una serie de opciones de configuración de seguridad que deben habilitarse para proteger el equipo.
Si tiene equipos que no dispongan de la directiva IOMMU para protegerse frente a ataques de DMA (por ejemplo, una máquina virtual), deshabilite dicha directiva.

Para deshabilitar el requisito de IOMMU, ejecute el siguiente comando en el servidor HGS:

```powershell
Disable-HgsAttestationPolicy Hgs_IommuEnabled
```

> [!NOTE]
> Si deshabilita la directiva IOMMU, las directivas IOMMU no serán necesarias para ningún equipo que realice la atestación con HGS.
> La directiva IOMMU no se puede deshabilitar para un solo equipo.

Puede revisar la lista de hosts y directivas de TPM registrados con los siguientes comandos de PowerShell:

```powershell
Get-HgsAttestationTpmHost
Get-HgsAttestationTpmPolicy
```

## <a name="step-4b-register-a-computer-in-host-key-mode"></a>Paso 4B: Registro de un equipo en modo de clave de host

> [!NOTE]
> Este paso lo realizan de forma conjunta el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y el administrador de HGS. Vea las notas siguientes para obtener más información.

En este paso, seguirá el proceso de generación de una clave única para el host y de su registro con HGS.
Si el servicio de atestación de HGS está configurado para usar el modo TPM, siga las instrucciones del [paso 4A](#step-4a-register-a-computer-in-tpm-mode).

### <a name="generate-a-key-for-a-ssnoversion-md-computer"></a>Generación de una clave para un equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

> [!NOTE]
> Esta parte la debe realizar de forma conjunta el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

La atestación de clave de host funciona mediante la generación de un par de claves asimétricas en el equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y el suministro de HGS con la mitad pública de esa clave.

Repita los pasos siguientes para cada equipo [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] en el que se vaya a realizar la atestación con HGS:

1. Para generar el par de claves, ejecute el comando siguiente en una consola de PowerShell con privilegios elevados:

    ```powershell
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

    Si ya ha creado una clave de host y quiere generar un nuevo par de claves, use los comandos siguientes:

    ```powershell
    Remove-HgsClientHostKey
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

2. Comparta el archivo de certificado con el administrador de HGS.

### <a name="register-the-sql-server-computer-with-hgs"></a>Registro del equipo SQL Server con HGS

> [!NOTE]
> Los pasos siguientes los debe realizar el administrador de HGS.

Repita los pasos siguientes para cada equipo [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] en el que se vaya a realizar la atestación con HGS:

1. Copie en un servidor HGS el archivo de certificado que ha obtenido del administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
2. Ejecute el comando siguiente en una consola de PowerShell con privilegios elevados para registrar el equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]:

    ```powershell
    Add-HgsAttestationHostKey -Name "YourComputerName" -Path "C:\temp\yourcomputername.cer"
   ```

## <a name="step-5-confirm-the-host-can-attest-successfully"></a>Paso 5: Confirmación de la posibilidad del host de realizar la atestación correctamente

> [!NOTE]
> Este paso lo debe realizar el administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Una vez que haya registrado el equipo de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con HGS ([paso 4A](#step-4a-register-a-computer-in-tpm-mode) para el modo TPM, [paso 4B](#step-4b-register-a-computer-in-host-key-mode) para el modo de clave de host), debe confirmar que es capaz de realizar la atestación correctamente.

Puede comprobar la configuración del cliente de atestación de HGS y realizar un intento de atestación en cualquier momento con [Get-HgsClientConfiguration](/powershell/module/hgsclient/get-hgsclientconfiguration?view=win10-ps&preserve-view=true).

La salida del comando será similar a esta:

```
PS C:\> Get-HgsClientConfiguration


IsHostGuarded                  : True
Mode                           : HostGuardianService
KeyProtectionServerUrl         : http://localhost
AttestationServerUrl           : http://hgs.bastion.local/Attestation
AttestationOperationMode       : HostKey
AttestationStatus              : Passed
AttestationSubstatus           : NoInformation
FallbackKeyProtectionServerUrl :
FallbackAttestationServerUrl   :
IsFallbackInUse                : False
```

Los dos campos más importantes de la salida son `AttestationStatus`, que indica si el equipo ha pasado la atestación, y `AttestationSubStatus`, que explica en qué directivas se produjo el error del equipo en caso de error en la atestación del equipo.

A continuación se explican los valores más comunes que pueden aparecer en `AttestationStatus`:

| `AttestationStatus` | Explicación |
| ----------------- | ----------- |
| Expirada | El host ha pasado la atestación, pero el certificado de mantenimiento que se emitió ha expirado. Asegúrese de que la hora del host y del HGS están sincronizadas. |
| `InsecureHostConfiguration` | El equipo no cumplía una o más directivas de atestación configuradas en el servidor HGS. Para obtener más información, vea `AttestationSubStatus`. |
| NoConfigurado | El equipo no está configurado con una dirección URL de atestación. [Configurar la URL de atestación](#step-3-configure-the-attestation-url) |
| Superado | El equipo ha superado la atestación y se confía en él para ejecutar los enclaves de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. |
| `TransientError` | No se ha podido realizar el intento de atestación debido a un error temporal. Este error suele significar que se ha producido un problema al contactar con HGS a través de la red. Compruebe la conexión de red y asegúrese de que el equipo pueda resolver el nombre del servicio HGS y redirigirlo. |
| `TpmError` | El dispositivo TPM del equipo ha detectado un error durante el intento de atestación. Consulte los registros de TPM para obtener más información. La eliminación del TPM puede resolver el problema, pero antes de hacerlo debe asegurarse de suspender BitLocker y otros servicios que dependan del TPM. |
| `UnauthorizedHost` | HGS no conoce la clave del host. Siga las instrucciones del [paso 4B](#step-4b-register-a-computer-in-host-key-mode) para registrar el equipo con HGS. |

Cuando en `AttestationStatus` se muestra `InsecureHostConfiguration`, el campo `AttestationSubStatus` se rellenará con uno o más nombres de directivas con errores.
En la tabla siguiente se explican los valores más comunes y cómo corregirlos.

| AttestationSubStatus | Qué significa y qué hay que hacer |
| -------------------- | ---------------------------- |
| CodeIntegrityPolicy | La directiva de integridad de código del equipo no está registrada en el HGS *o* en estos momentos el equipo no está utilizando una directiva de integridad de código. Encontrará una guía en [Configuración de una directiva de integridad de código](#configure-a-code-integrity-policy). |
| DumpsEnabled | El equipo está configurado para permitir volcados de memoria, pero la directiva Hgs_DumpsEnabled no permite los volcados. Para continuar, deshabilite los volcados en este equipo o deshabilite la directiva Hgs_DumpsEnabled. |
| FullBoot | El equipo se ha reanudado de un estado de suspensión o hibernación, lo que provoca cambios en las mediciones del TPM. Reinicie el equipo para generar medidas de TPM limpias. |
| HibernationEnabled | El equipo está configurado para permitir la hibernación con archivos de hibernación sin cifrar. Para resolver este problema, deshabilite la hibernación en el equipo. |
| HypervisorEnforcedCodeIntegrityPolicy | El equipo no está configurado para usar una directiva de integridad de código. Vaya a Directiva de grupo o Directiva de grupo local > Configuración del equipo > Plantillas administrativas > Sistema > Device Guard > active Seguridad basada en la virtualización > Protección basada en la virtualización de la integridad del código. Este elemento de directiva debe estar "Habilitado sin el bloqueo UEFI". |
| Iommu | Este equipo no tiene ningún dispositivo IOMMU habilitado. Si es un equipo físico, habilite IOMMU en el menú de configuración de UEFI. Si se trata de una máquina virtual y no tiene IOMMU disponible, ejecute `Disable-HgsAttestationPolicy Hgs_IommuEnabled` en el servidor de HGS. |
| SecureBoot | El arranque seguro no está habilitado en este equipo. Habilite el arranque seguro en el menú de configuración de UEFI para resolver este error. |
| VirtualSecureMode | La seguridad basada en la virtualización no se está ejecutando en este equipo. Siga las instrucciones que encontrará en el [Paso 2: Verificación de la ejecución de VBS en el equipo](#step-2-verify-virtualization-based-security-is-running). |

## <a name="next-steps"></a>Pasos siguientes

- [Configuración del enclave seguro en SQL Server](always-encrypted-enclaves-configure-enclave-type.md)