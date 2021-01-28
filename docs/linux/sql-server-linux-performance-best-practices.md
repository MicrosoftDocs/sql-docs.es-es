---
title: Procedimientos recomendados de rendimiento para SQL Server en Linux
description: En este artículo se ofrecen instrucciones y procedimientos recomendados de rendimiento para ejecutar SQL Server en Linux.
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 01/19/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 9a73013e7d49523f8aba418a2961336998190fc5
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689122"
---
# <a name="performance-best-practices-and-configuration-guidelines-for-sql-server-on-linux"></a>Procedimientos recomendados de rendimiento e instrucciones de configuración para SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se ofrecen procedimientos recomendados y recomendaciones para maximizar el rendimiento de las aplicaciones de base de datos que se conectan a SQL Server en Linux. Estas recomendaciones son específicas para la ejecución en la plataforma Linux. Se siguen aplicando todas las recomendaciones normales de SQL Server, como el diseño de índices.

Las instrucciones siguientes contienen recomendaciones para configurar SQL Server y el sistema operativo (SO) Linux.

## <a name="linux-os-configuration"></a>Configuración del sistema operativo Linux

Considere la posibilidad de usar las siguientes opciones de configuración del sistema operativo Linux para obtener el mejor rendimiento para una instalación de SQL Server.

### <a name="storage-configuration-recommendation"></a>Recomendación de configuración de almacenamiento

#### <a name="use-storage-subsystem-with-appropriate-iops-throughput-and-redundancy"></a>Uso del subsistema de almacenamiento con los valores adecuados de IOPS, rendimiento y redundancia

El subsistema de almacenamiento que hospeda los datos, los registros de transacciones y otros archivos asociados (por ejemplo, los archivos de punto de comprobación para OLTP en memoria) debe ser capaz de administrar las cargas de trabajo de tipo medio y máximo correctamente. Normalmente, en entornos locales, el proveedor de almacenamiento admite la configuración de RAID de hardware adecuada con la división en varios discos para garantizar los valores adecuados de IOPS, rendimiento y redundancia. Pero esto puede diferir en otros proveedores de almacenamiento y en otras ofertas de almacenamiento con distintas arquitecturas.

En el caso de SQL Server en Linux implementado en Azure Virtual Machines, considere la posibilidad de usar RAID de software para asegurarse de que se consiguen los requisitos de rendimiento e IOPS adecuados. Consulte el artículo siguiente al configurar SQL Server en máquinas virtuales de Azure para obtener consideraciones de almacenamiento similares: [Configuración del almacenamiento para VM de SQL Server](/azure/azure-sql/virtual-machines/windows/storage-configuration)

El siguiente es un ejemplo de cómo crear RAID de software en Linux en Azure Virtual Machines. A continuación se proporciona un ejemplo, pero debe usar el número adecuado de discos de datos para los valores necesarios de rendimiento e IOPS para los volúmenes en función de los requisitos de datos, registro de transacciones y E/S de tempdb. En este ejemplo, se han conectado 8 discos de datos a la máquina virtual de Azure: 4 para hospedar archivos de datos, 2 para registros de transacciones y 2 para la carga de trabajo de tempdb.

```bash
# To locate the devices (for example /dev/sdc) for RAID creation, use the lsblk command
# For Data volume, using 4 devices, in RAID 5 configuration with 8KB stripes
mdadm --create --verbose /dev/md0 --level=raid5 --chunk=8K --raid-devices=4 /dev/sdc /dev/sdd /dev/sde /dev/sdf

# For Log volume, using 2 devices in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md1 --level=raid10 --chunk=64K --raid-devices=2 /dev/sdg /dev/sdh

# For tempdb volume, using 2 devices in RAID 0 configuration with 64KB stripes
mdadm --create --verbose /dev/md2 --level=raid0 --chunk=64K --raid-devices=2 /dev/sdi /dev/sdj
```

#### <a name="disk-partitioning-and-configuration-recommendations"></a>Recomendaciones de configuración y partición de discos

Para SQL Server, se recomienda usar configuraciones RAID. La unidad de franja del sistema de archivos implementada (sunit) y el ancho de franja deben coincidir con la geometría de RAID. Este es un ejemplo basado en sistema de archivos XFS para un volumen de registro. 

```bash
# Creating a log volume, using 6 devices, in RAID 10 configuration with 64KB stripes
mdadm --create --verbose /dev/md3 --level=raid10 --chunk=64K --raid-devices=6 /dev/sda /dev/sdb /dev/sdc /dev/sdd /dev/sde /dev/sdf

mkfs.xfs /dev/sda1 -f -L log 
meta-data=/dev/sda1              isize=512    agcount=32, agsize=18287648 blks 
         =                       sectsz=4096  attr=2, projid32bit=1 
         =                       crc=1        finobt=1, sparse=1, rmapbt=0 
         =                       reflink=1 
data     =                       bsize=4096   blocks=585204384, imaxpct=5 
         =                       sunit=16     swidth=48 blks 
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1 
log      =internal log           bsize=4096   blocks=285744, version=2 
         =                       sectsz=4096  sunit=1 blks, lazy-count=1 
realtime =none                   extsz=4096   blocks=0, rtextents=0 
```

La matriz de registro es una unidad RAID-10 de 6 unidades con una franja de 64 k. Como puede ver:
   1. "sunit=16 blks", 16*4096 blk size= 64k coincide con el tamaño de franja. 
   2. "swidth = 48 blks", swidth/sunit = 3 es el número de unidades de datos de la matriz, excepto las unidades de paridad. 

#### <a name="file-system-configuration-recommendation"></a>Recomendación de configuración del sistema de archivos

SQL Server es compatible con los sistemas de archivos EXT4 y XFS para hospedar la base de datos, los registros de transacciones y archivos adicionales, como los de punto de comprobación para OLTP en memoria en SQL Server. Microsoft recomienda usar el sistema de archivos XFS para hospedar los archivos de datos y de registro de transacciones de SQL Server.

```bash
# Formatting the volume with XFS filesystem
mkfs.xfs /dev/md0 -f -L datavolume
mkfs.xfs /dev/md1 -f -L logvolume
mkfs.xfs /dev/md2 -f -L tempdb
```

> [!NOTE]
> Es posible configurar el sistema de archivos XFS para que no distinga mayúsculas de minúsculas al crear y dar formato al volumen XFS. No es la configuración que se use habitualmente en el ecosistema de Linux, pero se puede utilizar por motivos de compatibilidad.
>
> Ejemplo: mkfs.xfs /dev/md0 -f -n version=ci -L datavolume
>
> En el ejemplo, se usan los parámetros `-n version=ci` para configurar el sistema de archivos XFS para que no distinga mayúsculas de minúsculas.

##### <a name="persistent-memory-filesystem-recommendation"></a>Recomendación del sistema de archivos de memoria persistente

Para la configuración del sistema de archivos en dispositivos de memoria persistente, la asignación de bloques para el sistema de archivos subyacente debe ser de 2 MB. Para obtener más información sobre este tema, revise el artículo [Consideraciones técnicas](sql-server-linux-configure-pmem.md#technical-considerations).

#### <a name="disable-last-accessed-datetime-on-file-systems-for-sql-server-data-and-log-files"></a>Deshabilitación de la última fecha y hora de acceso en los sistemas de archivos para los archivos de datos y de registro de SQL Server

Para asegurarse de que la unidad adjuntada al sistema se vuelve a montar de forma automática después de un reinicio, se debe agregar al archivo `/etc/fstab`. También se recomienda usar el UUID (identificador único global) en `/etc/fstab` para hacer referencia a la unidad, en lugar de solo el nombre del dispositivo (por ejemplo, `/dev/sdc1`).

Se recomienda usar el atributo **noatime** con cualquier sistema de archivos que se utilice para almacenar archivos de datos y de registro de SQL Server. Consulte la documentación de Linux para obtener información sobre cómo establecer este atributo. A continuación se muestra un ejemplo de cómo habilitar la opción **noatime** para un volumen montado en la máquina virtual de Azure.

La entrada del punto de montaje en **_/etc/fstab_* _

```bash
UUID="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" /data1 xfs,rw,attr2,noatime 0 0
```

En el ejemplo anterior, UUID representa el dispositivo que puede encontrar mediante el comando _*_blkid_*_.

#### <a name="sql-server-and-forced-unit-access-fua-io-subsystem-capability"></a>Funcionalidad del subsistema de E/S de SQL Server y acceso de unidad forzada (FUA)

Existen ciertas versiones de distribuciones de Linux admitidas que proporcionan compatibilidad con la funcionalidad de subsistema de E/S de FUA para proporcionar durabilidad de datos. SQL Server usa la funcionalidad FUA para proporcionar E/S de gran eficacia y confiabilidad para la carga de trabajo de SQL Server. Para obtener información adicional sobre la compatibilidad de FUA con la distribución de Linux y su impacto en SQL Server, lea el siguiente blog: [SQL Server en Linux: Funcionamiento interno del acceso de unidad forzada (FUA)](https://bobsql.com/sql-server-on-linux-forced-unit-access-fua-internals/)

SUSE Linux Enterprise Server 12 SP5 y Red Hat Enterprise Linux 8.0 y versiones posteriores admiten la funcionalidad FUA en el subsistema de E/S. Si utiliza SQL Server 2017 CU6 y versiones posteriores, o bien SQL Server 2019, debe usar la siguiente configuración para una implementación de E/S eficaz y de alto rendimiento con FUA mediante SQL Server.

Use la configuración recomendada que se muestra a continuación si se cumplen las condiciones siguientes.

- Uso de SQL Server 2017 CU6 o posterior, o bien SQL Server 2019
- Uso de una distribución y una versión de Linux que admiten la funcionalidad de FUA (Red Hat Enterprise Linux 8.0 o posterior, o bien SUSE Linux Enterprise Server 12 SP5)
- En el subsistema de almacenamiento o hardware que admite y está configurado para la funcionalidad de FUA

Configuración recomendada:

1. Habilite la marca de seguimiento 3979 como parámetro de inicio.
2. Use _ *mssql-conf** para configurar `control.writethrough = 1` y `control.alternatewritethrough = 0`.

Para casi todas las demás configuraciones que no cumplen las condiciones anteriores, la configuración recomendada es la siguiente:

1. Habilite la marca de seguimiento 3982 como parámetro de inicio (que es el valor predeterminado para SQL Server en el ecosistema de Linux) y asegúrese de que la marca de seguimiento 3979 no está habilitada como parámetro de inicio.
2. Use **mssql-conf** para configurar `control.writethrough = 1` y `control.alternatewritethrough = 1`.

### <a name="kernel-and-cpu-settings-for-high-performance"></a>Configuración del kernel y la CPU para alto rendimiento

En la sección siguiente se describe la configuración recomendada del sistema operativo Linux relacionada con el alto rendimiento de una instalación de SQL Server. Vea la documentación del sistema operativo Linux para obtener información sobre el proceso de configuración de estas opciones. El uso que se describe de [**_Optimizado_* _](https://tuned-project.org) ayuda a establecer muchas configuraciones de CPU y kernel que se describen a continuación.

#### <a name="using-__tuned__-to-configure-kernel-settings"></a>Uso de _*_Optimizado_*_ para configurar las opciones del kernel

Para los usuarios de Red Hat Enterprise Linux (RHEL), el perfil de rendimiento [Optimizado](https://tuned-project.org) configura de forma automática algunas opciones del kernel y la CPU (excepto para C-States). A partir de RHEL 8.0, se ha desarrollado un perfil _*_Optimizado_*_ denominado _ *mssql** conjuntamente con Red Hat, que ofrece optimizaciones relacionadas con el rendimiento de Linux más precisas para cargas de trabajo de SQL Server. Este perfil incluye el perfil de rendimiento de RHEL y a continuación se presentan sus definiciones para que las revise con otras distribuciones de Linux y versiones de RHEL sin este perfil.

Para SUSE Linux Enterprise Server 12 SP5, Ubuntu 18.04 y Red Hat Enterprise Linux 7.x, el paquete **_Optimizado_ *_ se puede instalar manualmente. Se puede usar para crear y configurar el perfil _* mssql** como se describe a continuación.

##### <a name="proposed-linux-settings-using-a-tuned-mssql-profile"></a>Configuración propuesta de Linux mediante un perfil Optimizado mssql

```bash
#
# A Tuned configuration for SQL Server on Linux
#

[main]
summary=Optimize for Microsoft SQL Server
include=throughput-performance

[cpu]
force_latency=5

[sysctl]
vm.swappiness = 1
vm.dirty_background_ratio = 3
vm.dirty_ratio = 80
vm.dirty_expire_centisecs = 500
vm.dirty_writeback_centisecs = 100
vm.transparent_hugepages=always
# For multi-instance SQL deployments, use
# vm.transparent_hugepages=madvise
vm.max_map_count=1600000
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
kernel.numa_balancing=0
kernel.sched_latency_ns = 60000000
kernel.sched_migration_cost_ns = 500000
kernel.sched_min_granularity_ns = 15000000
kernel.sched_wakeup_granularity_ns = 2000000
```

Para habilitar este perfil Optimizado, guarde estas definiciones en un archivo **tuned.conf** en una carpeta `/usr/lib/tuned/mssql` y habilite el perfil mediante los comandos siguientes:

```bash
chmod +x /usr/lib/tuned/mssql/tuned.conf
tuned-adm profile mssql
```

Compruebe si está habilitado con el comando siguiente:

```bash
tuned-adm active
```

o

```bash
tuned-adm list
```

#### <a name="cpu-settings-recommendation"></a>Recomendación de configuración de CPU

En la tabla siguiente se proporcionan recomendaciones para la configuración de la CPU:

| Configuración | Value | Más información |
|---|---|---|
| Regulador de frecuencia de CPU | rendimiento | Consulte el comando **cpupower**. |
| ENERGY_PERF_BIAS | rendimiento | Consulte el comando **x86_energy_perf_policy**. |
| min_perf_pct | 100 | Consulte la documentación sobre Intel P-state. |
| C-States | Solo C1 | Consulte la documentación del sistema o de Linux para saber cómo asegurarse de que C-States solo se establece en C1 |

El uso de **_Optimizado_ *_ como se ha descrito antes configura automáticamente el regulador de frecuencia de la CPU, ENERGY_PERF_BIAS y los valores correctos de min_perf_pct debido al perfil de rendimiento que se usa como base para el perfil _* mssql**. El parámetro C-States se debe configurar manualmente según la documentación proporcionada por Linux o el distribuidor del sistema.

#### <a name="disk-settings-recommendations"></a>Recomendaciones de configuración de disco

En la tabla siguiente se proporcionan recomendaciones para la configuración del disco:

| Configuración | Value | Más información |
|---|---|---|
| Disco `readahead` | 4096 | Consulte el comando `blockdev`. |
| configuración de sysctl | kernel.sched_min_granularity_ns = 10000000<br/>kernel.sched_wakeup_granularity_ns = 15000000<br/>vm.dirty_ratio = 40<br/>vm.dirty_background_ratio = 10<br/>vm.swappiness = 1 | Consulte el comando **sysctl**. |

**Descripción:**

- **vm.swappiness**: este parámetro controla el peso relativo dado para intercambiar memoria de proceso en tiempo de ejecución en comparación con la caché del sistema de archivos. El valor predeterminado de este parámetro es 60, que indica el intercambio de páginas de memoria de proceso en tiempo de ejecución en comparación con la eliminación de páginas de caché del sistema de archivos con una proporción de 60:140. Al establecer el valor 1 se indica una preferencia segura por mantener la memoria de proceso en tiempo de ejecución en la memoria física a costa de la caché del sistema de archivos. Como SQL Server usa el grupo de búferes como una caché de páginas de datos y prefiere escribir en el hardware físico omitiendo la caché del sistema de archivos para una recuperación confiable, la configuración de intercambio intenso puede ser beneficiosa para una instancia dedicada de SQL Server y de alto rendimiento.
Puede encontrar información adicional en [Documentación de /proc/sys/vm/ - #swappiness](https://www.kernel.org/doc/html/latest/admin-guide/sysctl/vm.html#swappiness).

- **vm.dirty_\** _: los accesos de escritura de archivos de SQL Server no están almacenados en caché, lo que satisface sus requisitos de integridad de datos. Estos parámetros permiten un rendimiento de escritura asincrónico eficaz y reducen el impacto de la E/S de almacenamiento de las escrituras de almacenamiento en caché de Linux al permitir un almacenamiento en caché suficientemente grande mientras se limita el vaciado.

- _*kernel.sched_\**_: estos valores de parámetro representan la recomendación actual para ajustar el algoritmo de programación totalmente justa (CFS) en el kernel de Linux, para mejorar el rendimiento de las llamadas de E/S de red y de almacenamiento con respecto a las operaciones de adelantamiento y reanudación de los subprocesos.

El perfil _*_Optimizado_** **mssql*_ configura los valores _*vm.swappiness**, **vm.dirty_\* *_ y _* kernel.sched_\**_. La configuración del disco `readahead` mediante el comando `blockdev` se realiza por dispositivo y se debe realizar manualmente.

#### <a name="kernel-setting-auto-numa-balancing-for-multi-node-numa-systems"></a>Configuración del equilibrio automático de NUMA del kernel con sistemas NUMA de varios nodos

Si instala SQL Server en un sistema _ *NUMA** de varios nodos, la configuración de kernel **kernel.numa_balancing** siguiente está habilitada de forma predeterminada. Para permitir que SQL Server funcione con la máxima eficacia en un sistema **NUMA**, deshabilite el equilibrio automático de NUMA en un sistema NUMA de varios nodos:

```bash
sysctl -w kernel.numa_balancing=0
```

El perfil **_Optimizado_ *_ **mssql** configura la opción _* kernel.numa_balancing**.

#### <a name="kernel-settings-for-virtual-address-space"></a>Configuración del kernel para el espacio de direcciones virtuales

Es posible que la configuración predeterminada de **vm.max_map_count** (que es 65536) no sea lo suficientemente alta para una instalación SQL Server. Por este motivo, cambie el valor de **vm.max_map_count** a 262144 como mínimo para una implementación de SQL Server y consulte la sección [Configuración propuesta de Linux mediante un perfil Optimizado mssql](#proposed-linux-settings-using-a-tuned-mssql-profile) para realizar ajustes adicionales en estos parámetros de kernel. El valor máximo de vm.max_map_count es 2147483647.

```bash
sysctl -w vm.max_map_count=1600000
```

El perfil **_Optimizado_ *_ **mssql** configura la opción _* vm.max_map_count**.

#### <a name="leave-transparent-huge-pages-thp-enabled"></a>Habilitación de Transparent Huge Pages (THP)

La mayoría de las instalaciones de Linux deben tener esta opción activada de forma predeterminada. Se recomienda que, para obtener un rendimiento más coherente, deje habilitada esta opción de configuración. Pero, por ejemplo, si hay actividad de paginación de memoria alta en implementaciones de SQL Server con varias instancias, o la ejecución de SQL Server con otras aplicaciones que requieren mucha memoria del servidor, se recomienda probar el rendimiento de las aplicaciones después de ejecutar el comando siguiente:

```bash
echo madvise > /sys/kernel/mm/transparent_hugepage/enabled
```

O bien, modifique el perfil  **_Tuned_* _ **mssql** con la línea:

```bash
vm.transparent_hugepages=madvise
```

Y active el perfil _ *mssql** después de la modificación:

```bash
tuned-adm off
tuned-adm profile mssql
```

El perfil **_Optimizado_ *_ **mssql** configura la opción _* transparent_hugepage**.

#### <a name="network-setting-recommendations"></a>Recomendaciones de configuración de red

Al igual que existen recomendaciones de almacenamiento y de CPU, hay recomendaciones específicas de red que se enumeran a continuación como referencia. No todas las opciones de configuración que se mencionan a continuación están disponibles en otras NIC. Consulte a los proveedores de NIC si quiere obtener instrucciones para cada una de estas opciones. Pruebe y configure esto en entornos de desarrollo antes de aplicarlas en entornos de producción. Las opciones que se mencionan a continuación se explican con ejemplos y los comandos que se usan son específicos del tipo de NIC y del proveedor. 

1. Configuración del tamaño de búfer del puerto de red: en el ejemplo siguiente, la NIC se denomina "eth0", que es una NIC basada en Intel. En el caso de la NIC basada en Intel, el tamaño de búfer recomendado es de 4 KB (4096). Compruebe los valores máximos establecidos previamente y, después, configúrelo con los comandos de ejemplo que se muestran a continuación:

 ```bash
         #To check the pre-set maximums please run the command, example NIC name used here is:"eth0"
         ethtool -g eth0
         #command to set both the rx(recieve) and tx (transmit) buffer size to 4 KB. 
         ethtool -G eth0 rx 4096 tx 4096
         #command to check the value is properly configured is:
         ethtool -g eth0
  ```

2. Habilitación de marcos gigantes: antes de habilitar los marcos gigantes, compruebe que todos los conmutadores de red, los enrutadores y los demás elementos esenciales de la ruta del paquete de red entre los clientes y SQL Server los admiten. Solo entonces, la habilitación de los marcos gigantes puede mejorar el rendimiento. Después de habilitar los marcos gigantes, conéctese a SQL Server y cambie el tamaño de los paquetes de red a 8060 con `sp_configure` como se muestra a continuación:

```bash
         #command to set jumbo frame to 9014 for a Intel NIC named eth0 is
         ifconfig eth0 mtu 9014
         #verify the setting using the command:
         ip addr | grep 9014
```

```sql
         sp_configure 'network packet size' , '8060'
         go
         reconfigure with override
         go
```

3. De manera predeterminada, se recomienda establecer el puerto para la fusión de IRQ de RX/TX adaptable, lo que significa que la entrega de interrupción se ajustará para mejorar la latencia cuando la velocidad de los paquetes sea baja y mejorar el rendimiento cuando sea alta. Tenga en cuenta que es posible que esta configuración no esté disponible en todas la infraestructuras de red, por lo que debe revisar la infraestructura de red existente y confirmar que es compatible. En el ejemplo siguiente, la NIC se denomina "eth0", que es una NIC basada en Intel:

```bash
         #command to set the port for adaptive RX/TX IRQ coalescing
         echtool -C eth0 adaptive-rx on
         echtool -C eth0 adaptive-tx on
         #confirm the setting using the command:
         ethtool -c eth0
```

> [!NOTE]
> Para obtener un comportamiento predecible en entornos de alto rendimiento, como los de pruebas comparativas, deshabilite la fusión de IRQ de RX/TX adaptable y, después, establezca específicamente la fusión de interrupciones RX/TX. Vea los comandos de ejemplo para deshabilitar la fusión de IRQ de RX/TX y, después, establezca los valores específicamente:

```bash
         #commands to disable adaptive RX/TX IRQ coalescing
         echtool -C eth0 adaptive-rx off
         echtool -C eth0 adaptive-tx off
         #confirm the setting using the command:
         ethtool -c eth0
         #Let us set the rx-usecs parameter which specify how many microseconds after at least 1 packet is received before generating an interrupt, and the [irq] parameters are the corresponding delays in updating the #status when the interrupt is disabled. For Intel bases NICs below are good values to start with:
         ethtool -C eth0 rx-usecs 100 tx-frames-irq 512
         #confirm the setting using the command:
         ethtool -c eth0
```

4. También se recomienda habilitar la opción RSS (Escalabilidad de tráfico de entrada) de manera predeterminada, para combinar los lados rx y tx de las colas de RSS. Ha habido escenarios concretos en los que, al trabajar con Soporte técnico de Microsoft, también se ha mejorado el rendimiento al deshabilitar RSS. Pruebe esta configuración en entornos de prueba antes de aplicarla en entornos de producción. El comando de ejemplo que se muestra a continuación es para las NIC de Intel.

```bash
         #command to get pre-set maximums
         ethtool -l eth0 
         #note the pre-set "Combined" maximum value. let's consider for this example, it is 8.
         #command to combine the queues with the value reported in the pre-set "Combined" maximum value:
         ethtool -L eth0 combined 8
         #you can verify the setting using the command below
         ethtool -l eth0
```

5. Trabajar con la afinidad de IRQ de puertos de NIC. Para lograr el rendimiento esperado mediante la modificación de la afinidad de IRQ, tenga en cuenta algunos parámetros importantes como el control por parte de Linux de la topología del servidor, la pila de controladores de NIC, la configuración predeterminada y el valor irqbalance. Las optimizaciones de la configuración de las afinidades de IRQ de puertos de NIC se realizan con el conocimiento de la topología del servidor, deshabilitando el valor irqbalance y con la configuración específica del proveedor de la NIC. A continuación se muestra un ejemplo de infraestructura de red específica de Mellanox que facilita la explicación de la configuración. Tenga en cuenta que los comandos cambiarán en función del entorno. Póngase en contacto con el proveedor de la NIC para obtener instrucciones adicionales:

```bash
         #disable irqbalance or get a snapshot of the IRQ settings and force the daemon to exit
         systemctl disable irqbalance.service
         #or
         irqbalance --oneshot

         #download the Mellanox mlnx_tuning_scripts tarball, https://www.mellanox.com/sites/default/files/downloads/tools/mlnx_tuning_scripts.tar.gz and extract it
         tar -xvf mlnx_tuning_scripts.tar.gz
         # be sure, common_irq_affinity.sh is executable. if not, 
         # chmod +x common_irq_affinity.sh       

         #display IRQ affinity for Mellanox NIC port; e.g eth0
         ./show_irq_affinity.sh eth0

         #optimize for best throughput performance
         ./mlnx_tune -p HIGH_THROUGHPUT

         #set hardware affinity to the NUMA node hosting physically the NIC and its port
         ./set_irq_affinity_bynode.sh `\cat /sys/class/net/eth0/device/numa_node` eth0

         #verify IRQ affinity
         ./show_irq_affinity.sh eth0

         #add IRQ coalescing optimizations
         ethtool -C eth0 adaptive-rx off
         ethtool -C eth0 adaptive-tx off
         ethtool -C eth0  rx-usecs 750 tx-frames-irq 2048

         #verify the settings
         ethtool -c eth0
```

6. Una vez que se hayan realizado los cambios anteriores, compruebe la velocidad de la NIC para asegurarse de que coincida con la esperada mediante el comando siguiente:

```bash
         ethtool eth0 | grep -i Speed
```

#### <a name="additional-advanced-kernelos-configuration"></a>Configuración avanzada adicional del kernel o el sistema operativo

1. Para obtener el mejor rendimiento de E/S de almacenamiento, se recomienda el uso de la programación de varias colas de Linux para dispositivos de bloqueo. Esto permite que el rendimiento de la capa de bloqueo se escale correctamente con unidades de estado sólido (SSD) rápidas y sistemas de varios núcleos. Consulte la documentación si está habilitada de forma predeterminada en las distribuciones de Linux. En la mayoría de los demás casos, el arranque del kernel con **scsi_mod.use_blk_mq=y** lo habilita, a pesar de que en la documentación de la distribución de Linux en uso se pueden incluir instrucciones adicionales. Esto es coherente con el kernel de Linux ascendente.

1. Como la E/S de múltiples rutas se suele usar para implementaciones de SQL Server, el destino de múltiples rutas del asignador de dispositivos (DM) también se debe configurar para usar la infraestructura de `blk-mq` habilitando la opción de arranque del kernel **dm_mod.use_blk_mq=y**. El valor predeterminado es `n` (deshabilitado). Esta opción, cuando los dispositivos SCSI subyacentes usan `blk-mq`, reduce la sobrecarga de bloqueo en el nivel de DM. Consulte la documentación de la distribución de Linux en uso para obtener instrucciones adicionales sobre cómo configurarla.

#### <a name="configure-swapfile"></a>Configuración del archivo de intercambio

Asegúrese de que tiene un archivo de intercambio configurado correctamente para evitar problemas de memoria insuficiente. Consulte la documentación de Linux para obtener información sobre cómo crear y ajustar correctamente el tamaño de un archivo de intercambio.

#### <a name="virtual-machines-and-dynamic-memory"></a>Virtual Machines y Memoria dinámica

Si ejecuta SQL Server en Linux en una máquina virtual, asegúrese de seleccionar opciones para corregir la cantidad de memoria reservada para la máquina virtual. No use características como Memoria dinámica de Hyper-V.

## <a name="sql-server-configuration"></a>Configuración de SQL Server

Se recomienda realizar las siguientes tareas de configuración después de instalar SQL Server en Linux para lograr el mejor rendimiento de la aplicación.

### <a name="best-practices"></a>Procedimientos recomendados

- **Uso de PROCESS AFFINITY en el nodo o las CPU**

   Se recomienda usar `ALTER SERVER CONFIGURATION` para establecer `PROCESS AFFINITY` en todas las instancias de **NUMANODE** o las CPU que use para SQL Server (que suele ser para todos los NODE y las CPU) en un sistema operativo Linux. La afinidad del procesador contribuye a mantener un comportamiento eficaz de programación de Linux y SQL. El uso de la opción **NUMANODE** es el método más sencillo. Use **PROCESS AFFINITY** aunque solo tenga un único nodo NUMA en el equipo. Para obtener más información sobre cómo establecer **PROCESS AFFINITY**, vea el artículo [ALTER SERVER CONFIGURATION](../t-sql/statements/alter-server-configuration-transact-sql.md).

- **Configuración de varios archivos de datos de tempdb**

   Dado que una instalación de SQL Server en Linux no ofrece una opción para configurar varios archivos de tempdb, se recomienda que se plantee la posibilidad de crear varios archivos de datos de tempdb después de la instalación. Para obtener más información, consulte las instrucciones del artículo [Recomendaciones para reducir la contención de asignación en la base de datos tempdb de SQL Server](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d).

### <a name="advanced-configuration"></a>Configuración avanzada

Las siguientes recomendaciones son valores de configuración opcionales que puede ejecutar tras la instalación de SQL Server en Linux. Estas opciones se basan en los requisitos de la carga de trabajo y la configuración del sistema operativo Linux.

- **Establecimiento de un límite de memoria con mssql-conf**

   Para asegurarse de que hay suficiente memoria física disponible para el sistema operativo Linux, el proceso de SQL Server solo usa de forma predeterminada el 80 % de la RAM física. En algunos sistemas con gran cantidad de RAM física, el 20 % puede ser una cantidad considerable. Por ejemplo, en un sistema con 1 TB de RAM, el valor predeterminado dejaría alrededor de 200 GB de RAM sin usar. En esta situación, es posible que quiera configurar el límite de memoria en un valor más alto. Consulte la documentación sobre la herramienta **mssql-conf** y la opción [memory.memorylimitmb](sql-server-linux-configure-mssql-conf.md#memorylimit) que controla la memoria visible para SQL Server (en unidades de MB).

   Al cambiar esta opción, tenga cuidado de no establecer este valor demasiado alto. Si no deja memoria suficiente, podría experimentar problemas con el sistema operativo Linux y otras aplicaciones de Linux.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre características de SQL Server que mejoran el rendimiento, consulte [Introducción a las características de rendimiento](sql-server-linux-performance-get-started.md).

Para obtener más información sobre SQL Server en Linux, consulte [Introducción a SQL Server en Linux](sql-server-linux-overview.md).