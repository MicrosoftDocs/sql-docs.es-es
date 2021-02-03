---
title: Planificación de la atestación del Servicio de protección de host
description: Planee la atestación del Servicio de protección de host para Always Encrypted con enclaves seguros de SQL Server.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d156b2c02d67c947041de268962579d6417202e2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235753"
---
# <a name="plan-for-host-guardian-service-attestation"></a>Planificación de la atestación del Servicio de protección de host

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Cuando use [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md), asegúrese de que la aplicación cliente se comunica con un enclave de confianza dentro del proceso de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. En el caso de un enclave de seguridad basado en virtualización (VBS), este requisito incluye comprobar que el código incluido en el enclave es válido y que el equipo en el que se hospeda [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] es de confianza. La atestación remota logra este objetivo mediante la introducción de un tercero que puede validar la identidad (y, opcionalmente, la configuración) del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Antes de que [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] pueda usar un enclave para ejecutar una consulta, debe proporcionar información al servicio de atestación sobre su entorno operativo para obtener un certificado de mantenimiento. Este certificado de mantenimiento se envía al cliente, que puede comprobar de forma independiente su autenticidad con el servicio de atestación. Una vez que el cliente confía en el certificado de mantenimiento, sabe que está hablando con un enclave de VBS de confianza y emitirá la consulta que usará dicho enclave.

El rol de Servicio de protección de host (HGS) en Windows Server 2019 proporciona capacidades de atestación remota para Always Encrypted con enclaves de VBS.
Este artículo es una guía sobre las decisiones y los requisitos previos a la implementación a fin de usar Always Encrypted con los enclaves de VBS y la atestación de HGS.

## <a name="architecture-overview"></a>Información general sobre la arquitectura

El Servicio de protección de host (HGS) es un servicio web en clúster que se ejecuta en Windows Server 2019.
En una implementación típica, habrá de uno a tres servidores HGS, al menos un equipo que ejecute [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y un equipo que ejecute una aplicación cliente o herramientas, como SQL Server Management Studio.
Puesto que el HGS es responsable de determinar qué equipos que ejecutan [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] son de confianza, requiere un aislamiento físico y lógico de la instancia de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] que está protegiendo.
Si los mismos administradores tienen acceso al HGS y a un equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], podrían configurar el servicio de atestación para permitir que un equipo malintencionado ejecute [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], lo que les permitiría poner en peligro el enclave de VBS.

### <a name="hgs-domain"></a>Dominio de HGS

La configuración de HGS creará automáticamente un nuevo dominio de Active Directory para los servidores HGS, los recursos de clúster de conmutación por error y las cuentas de administrador.

No es necesario que el equipo que ejecuta [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] esté en un dominio, pero si lo está, debe ser un dominio diferente del que usa el servidor HGS.

### <a name="high-availability"></a>Alta disponibilidad

La característica de HGS instala y configura automáticamente un clúster de conmutación por error.
En un entorno de producción, se recomienda usar tres servidores HGS para lograr alta disponibilidad. Consulte la [documentación del clúster de conmutación por error](/windows-server/failover-clustering/manage-cluster-quorum) para obtener información sobre cómo se determina el cuórum de clúster y sobre las configuraciones alternativas, como los clústeres de dos nodos con un testigo externo.

No es necesario el almacenamiento compartido entre los nodos de HGS. Una copia de la base de datos de atestación se almacena en cada servidor HGS y se replica automáticamente a través de la red mediante el servicio de clúster.

### <a name="network-connectivity"></a>Conectividad de red

Tanto el cliente SQL como [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] deben ser capaces de comunicarse con HGS a través de HTTP.
Configure HGS con un certificado TLS para cifrar todas las comunicaciones entre el cliente SQL y HGS, así como entre SQL Server y HGS.
Esta configuración ayuda a protegerse de ataques de tipo "Man in the middle" y garantiza que se comunica con el servidor HGS correcto.

Los servidores HGS requieren conectividad entre cada nodo del clúster para asegurarse de que la base de datos del servicio de atestación permanezca sincronizada. Es un procedimiento recomendado del clúster de conmutación por error para conectar los nodos de HGS en una red para la comunicación del clúster y usar una red independiente para que otros clientes se comuniquen con HGS.

### <a name="attestation-modes"></a>Modos de atestación

Cuando un equipo que ejecuta [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] intenta realizar una atestación con HGS, primero preguntará a HGS cómo debe realizar la atestación.
HGS admite dos modos de atestación para su uso con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]:

| Modo de atestación | Explicación |
| ---------------- | ------- |
| TPM | La atestación del Módulo de plataforma segura (TPM) proporciona la mayor garantía sobre la identidad y la integridad del equipo que realiza la atestación con HGS. Requiere que los equipos que ejecutan [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] tengan instalada la versión 2.0 de TPM. Cada chip TPM contiene una identidad única e inmutable (clave de aprobación) que se puede usar para identificar un equipo determinado. Los TPM también miden el proceso de arranque del equipo y almacenan los valores hash de las medidas que afectan a la seguridad en los registros de configuración de la plataforma (PCR) que se pueden leer, pero que el sistema operativo no puede modificar. Estas medidas se utilizan durante la atestación para proporcionar una prueba criptográfica de que un equipo está en la configuración de seguridad que dice estar. |
| Clave de host | La atestación de clave de host es una forma más sencilla de atestación que solo comprueba la identidad de un equipo mediante un par de claves asimétricas. La clave privada se almacena en el equipo que ejecuta [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y la clave pública se proporciona a HGS. No se mide la configuración de seguridad del equipo y no es necesario un chip TPM 2.0 en el equipo que ejecuta [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Es importante proteger la clave privada instalada en el equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], ya que cualquier persona que obtenga esta clave puede suplantar a un equipo legítimo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y al enclave de VBS que se ejecuta dentro de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. |

En general, se proponen las recomendaciones siguientes:

- Para los **servidores de producción físicos**, se recomienda usar la atestación de TPM debido a las garantías adicionales que proporciona.
- Para los **servidores de producción virtuales**, se recomienda la atestación de clave de host porque la mayoría de las máquinas virtuales no tienen TPM virtuales ni arranque seguro. Si va a usar una máquina virtual con seguridad mejorada, como un [máquina virtual blindada local](/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node), puede usar el modo TPM. En todas las implementaciones virtualizadas, el proceso de atestación solo analiza el entorno de la máquina virtual, no la plataforma de virtualización subyacente.
- Para **escenarios de desarrollo y pruebas**, se recomienda la atestación de clave de host porque es más fácil de configurar.

### <a name="trust-model"></a>Modelo de confianza

En el modelo de confianza del enclave de VBS, las consultas y los datos cifrados se evalúan en un enclave basado en software para protegerlo del sistema operativo del host.
El acceso a este enclave está protegido por el hipervisor de la misma manera que dos máquinas virtuales que se ejecutan en el mismo equipo no pueden tener acceso a la memoria de los demás.
Para que un cliente tenga la confianza de que se está comunicando con una instancia legítima de VBS, debe usar la atestación basada en TPM que establece una raíz de confianza del hardware en el equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Las medidas de TPM que se capturan durante el proceso de arranque incluyen la clave de identidad única de la instancia de VBS, lo que garantiza que el certificado de mantenimiento solo sea válido en ese equipo exacto.
Además, cuando un TPM está disponible en un equipo que ejecuta VBS, el TPM protege la parte privada de la clave de identidad de VBS, lo que impide que alguien intente suplantar a esa instancia de VBS.

El arranque seguro es obligatorio con la atestación de TPM para garantizar que UEFI ha cargado un cargador de arranque legítimo firmado por Microsoft y que ningún rootkit haya interceptado el proceso de arranque del hipervisor.
Además, se requiere un dispositivo IOMMU de forma predeterminada para asegurarse de que los dispositivos de hardware con acceso directo a memoria no puedan inspeccionar o modificar la memoria del enclave.

Todas estas protecciones suponen que el equipo que ejecuta [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] es una máquina física.
Si virtualiza el equipo que ejecuta [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], ya no podrá garantizar que la memoria de la máquina virtual sea segura para la inspección por parte del hipervisor o el administrador del hipervisor. Por ejemplo, un administrador de hipervisor podría volcar la memoria de la máquina virtual y obtener acceso a la versión de texto no cifrado de la consulta y a los datos del enclave.
Del mismo modo, aunque la máquina virtual tenga un TPM virtual, solo puede medir el estado y la integridad del sistema operativo de la máquina virtual y el entorno de arranque.
No puede medir el estado del hipervisor que controla la máquina virtual.

Sin embargo, incluso cuando [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] está virtualizado, el enclave todavía está protegido contra los ataques que se originan en el sistema operativo de la máquina virtual.
Si confía en el hipervisor o en el proveedor de la nube, y se preocupa principalmente por los ataques del administrador de la base de datos y del administrador del sistema operativo en los datos confidenciales, un [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] virtualizado puede satisfacer sus necesidades.

Del mismo modo, la atestación de clave de host sigue siendo valiosa en situaciones en las que un módulo de TPM 2.0 no está instalado en el equipo que ejecuta [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] o en escenarios de desarrollo y pruebas en los que la seguridad no es primordial.
Todavía puede usar muchas de las características de seguridad mencionadas anteriormente, incluido el arranque seguro y un módulo de TPM 1.2, para proteger mejor VBS y el sistema operativo en su conjunto.
Pero, dado que no hay forma de que HGS compruebe que el equipo tiene esta configuración habilitada con la atestación de clave de host, el cliente no está seguro de que el host realmente use todas las protecciones disponibles.

## <a name="prerequisites"></a>Prerrequisitos

### <a name="hgs-server-prerequisites"></a>Requisitos previos del servidor HGS

Los equipos que ejecutan el rol del Servicio de protección de host deben cumplir los siguientes requisitos:

| Componente | Requisito |
| --------- | ----------- |
| Sistema operativo | Windows Server 2019, edición Standard o Datacenter |
| CPU | 2 núcleos (mín), 4 núcleos (recomendado) |
| RAM | 8 GB (mín) |
| NIC | Dos NIC con direcciones IP estáticas recomendadas (una para el tráfico de clúster, una para el servicio de HGS) |

HGS es un rol enlazado a la CPU debido al número de acciones que requieren cifrado y descifrado.
El uso de procesadores modernos con capacidades de aceleración criptográfica mejorará el rendimiento de HGS.
Los requisitos de almacenamiento para los datos de atestación son mínimos, en el intervalo de 10 KB a 1 MB por equipo único que realiza la atestación.

No una los equipos HGS a un dominio antes de empezar.

### <a name="ssnoversion-md-computer-prerequisites"></a>Requisitos previos de los equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

Los equipos que ejecutan [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] deben cumplir los [requisitos para instalar SQL Server](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) y los [requisitos de hardware de Hyper-V](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements#hardware-requirements).

Estos requisitos incluyen:

- [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] o posterior
- Windows 10 Enterprise, versión 1809 o posteriores; Windows Server 2019 Datacenter Edition. Otras ediciones de Windows 10 y Windows Server no admiten la atestación con HGS.
- Compatibilidad de CPU con tecnologías de virtualización:
  - Intel VT-x con tablas de página extendida.
  - AMD-V con indexación de virtualización rápida.
  - Si va a ejecutar [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] en una máquina virtual, el hipervisor y la CPU física deben ofrecer funciones de virtualización anidadas. Consulte la sección del [modelo de confianza](#trust-model) para obtener información sobre las garantías al ejecutar enclaves de VBS en una máquina virtual.
    - En Hyper-V 2016 o versiones posteriores, [habilite las extensiones de virtualización anidada en el procesador de máquina virtual](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#configure-nested-virtualization).
    - En Azure, seleccione un tamaño de máquina virtual que admita la virtualización anidada. Todas las máquinas virtuales de la serie v3 admiten la virtualización anidada, por ejemplo, Dv3 y Ev3. Consulte [Creación de una máquina virtual de Azure compatible con el anidamiento](/azure/virtual-machines/windows/nested-virtualization#create-a-nesting-capable-azure-vm).
    - En VMware vSphere 6.7 o posterior, habilite la compatibilidad con la seguridad basada en virtualización en la máquina virtual, como se describe en la [documentación de VMware](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-C2E78F3E-9DE2-44DB-9B0A-11440800AADD.html).
    - Es posible que otros hipervisores y nubes públicas admitan funciones de virtualización anidada que también permitan Always Encrypted con enclaves de VBS. Consulte la documentación de la solución de virtualización para obtener instrucciones relativas a la compatibilidad y configuración.
- Si tiene previsto usar la atestación de TPM, necesitará un chip TPM 2.0 rev 1.16 listo para su uso en el servidor. En este momento, la atestación de HGS no funciona con los chips TPM 2.0 rev 1.38. Además, el TPM debe tener un certificado de clave de aprobación válido.

## <a name="roles-and-responsibilities-when-configuring-attestation-with-hgs"></a>Roles y responsabilidades al configurar la atestación con HGS

La configuración de la atestación con HGS implica la configuración de componentes de tipos diferentes: HGS, equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], instancias de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y aplicaciones que desencadenan la atestación de enclave. Los usuarios realizan la configuración de los componentes de cada tipo y asumen uno de los roles siguientes:

- Administrador de HGS: implementa HGS, registra equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con HGS y comparte la dirección URL de atestación de HGS con los administradores de equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y administradores de aplicaciones cliente.
- Administrador del equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]: instala los componentes cliente de atestación, habilita VBS en los equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], proporciona al administrador de HGS la información necesaria para registrar los equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] con HGS, configura la dirección URL de atestación en los equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y comprueba que los equipos con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] pueden realizar correctamente la atestación con HGS.
- DBA: configura enclaves seguros en instancias de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
- Administrador de aplicaciones: configura la aplicación con la dirección URL de atestación obtenida del administrador de HGS.

En entornos de producción (donde se controlan datos confidenciales reales), es importante que la organización se adhiera a la separación de roles al configurar la atestación, donde cada rol distinto es asumido por usuarios diferentes. En concreto, si el objetivo de implementar Always Encrypted en la organización es reducir el área expuesta a ataques asegurándose de que los administradores de los equipo con [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y los DBA no puedan acceder a los datos confidenciales, los administradores de [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] y los DBA no deben controlar los servidores HGS.

## <a name="devtest-environment-considerations"></a>Consideraciones sobre el entorno de desarrollo y pruebas

Si usa Always Encrypted con enclaves de VBS en un entorno de desarrollo o de prueba y no requiere alta disponibilidad ni protección segura del equipo que ejecuta [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], puede realizar una o todas las concesiones siguientes para una implementación simplificada:

- Implemente solo un nodo de HGS. Aunque HGS instala un clúster de conmutación por error, no es necesario agregar nodos adicionales si la alta disponibilidad no es un problema.
- Use el modo de clave de host en lugar del modo TPM para obtener una experiencia de instalación más sencilla.
- Virtualice HGS y/o [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] para guardar recursos físicos.
- Ejecute SSMS u otras herramientas para configurar Always Encrypted con enclaves seguros en el mismo equipo que [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Esto conserva las claves maestras de columna en el mismo equipo que [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], por lo que no se debe hacer en un entorno de producción.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación del Servicio de protección de host para [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md)