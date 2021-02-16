---
title: Configuración de memoria persistente (PMEM)
description: Obtenga información sobre cómo configurar la memoria persistente (PMEM) para SQL Server en Linux y cómo crear espacios de nombres para dispositivos PMEM.
ms.custom: seo-lt-2019
author: briancarrig
ms.author: brcarrig
ms.date: 10/31/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15  || >= sql-server-ver15'
ms.openlocfilehash: c11807faa2180d3f86036392d42c7f6ee6b9f473
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348205"
---
# <a name="configure-persistent-memory-pmem-for-sql-server-on-linux"></a>Configuración de la memoria persistente (PMEM) para SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Este artículo describe cómo configurar la memoria persistente (PMEM) para [!INCLUDE[sqlv15](../includes/sssql19-md.md)] en Linux.

## <a name="overview"></a>Información general

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] tiene una serie de características en memoria que usan la memoria persistente. En este documento se describen los pasos necesarios para configurar la memoria persistente para SQL Server en Linux.

> [!NOTE]
> El término _habilitación_ se incluyó para transmitir el concepto de trabajar con un sistema de archivos con reconocimiento de memoria persistente. Se facilita acceso directo al sistema de archivos desde aplicaciones de espacio de usuario mediante la asignación de memoria (`mmap()`). Cuando se crea una asignación de memoria para un archivo, la aplicación puede emitir instrucciones de carga o almacenamiento que omiten por completo la pila de E/S. Esto se considera un método de acceso a archivos "habilitado" desde la perspectiva de la aplicación de extensión de host (que es el código de caja negra que permite a SQLPAL interactuar con el sistema operativo Windows o Linux).

## <a name="create-namespaces-for-pmem-devices"></a>Creación de espacios de nombres para dispositivos PMEM

### <a name="configure-the-devices"></a>Configuración de los dispositivos

En Linux, use la utilidad `ndctl`.

- Instale `ndctl` para configurar el dispositivo PMEM. Puede encontrarlo [aquí](https://docs.pmem.io/getting-started-guide/installing-ndctl).
- Use `ndctl` para crear un espacio de nombres. Los espacios de nombres se intercalan en NVDIMMs de PMEM y pueden proporcionar diferentes tipos de acceso de espacio de usuario a las regiones de memoria del dispositivo. `fsdax` es el modo predeterminado y el modo deseado para SQL Server.

```bash 
ndctl create-namespace -f -e namespace0.0 --mode=fsdax* --map=dev
```

Tenga en cuenta que se ha elegido el modo `fsdax` y se está usando la memoria del sistema para almacenar los metadatos por página. Recomendamos para ello usar `--map=dev`. Esto almacena directamente los metadatos en el espacio de nombres. El almacenamiento de metadatos en memoria mediante `--map=mem` se considera experimental en este momento.

Use `ndctl` para comprobar el espacio de nombres. 
  
La salida de ejemplo es la siguiente:

```bash
# ndctl list -N
{
  "dev":"namespace0.0",
  "mode":"fsdax",
  "map":"dev",
  "size":4294967296,
  "sector_size":512,
  "blockdev":"pmem0",
  "numa_node":0
}
```

### <a name="create-and-mount-pmem-device"></a>Creación y montaje del dispositivo PMEM

Por ejemplo, con XFS

```bash
mkfs.xfs -f /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
xfs_io -c "extsize 2m" /mnt/dax
```

Por ejemplo, con EXT4

```bash
mkfs.ext4 -b 4096 -E stride=512 -F /dev/pmem0
mount -o dax,noatime /dev/pmem0 /mnt/dax
```

## <a name="technical-considerations"></a>Consideraciones técnicas

- Asignación de bloquees de 2 MB para XFS/EXT4, como se ha descrito anteriormente
- La alineación incorrecta entre la asignación de bloques y `mmap` da como resultado un retorno silencioso a 4 KB
- Los tamaños de archivo deben ser un múltiplo de 2 MB (módulo de 2 MB)
- No deshabilite páginas de gran tamaño transparentes (THP) (habilitadas de forma predeterminada en la mayoría de distribuciones)

Una vez que el dispositivo se haya configurado con `ndctl`, creado y montado, puede colocar archivos de base de datos en él o crear una base de datos.

Dado que los dispositivos PMEM son aptos para O_DIRECT (E/S directa), considere la posibilidad de habilitar la marca de seguimiento 3979 para deshabilitar el mecanismo de vaciado forzado. Para obtener más información, vea [Soporte técnico de FUA](https://support.microsoft.com/help/4131496/enable-forced-flush-mechanism-in-sql-server-2017-on-linux). Los elementos internos de acceso a la unidad forzada se describen aquí [componentes internos de FUA](/archive/blogs/bobsql/sql-server-on-linux-forced-unit-access-fua-internals).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre SQL Server en Linux, consulte [SQL Server en Linux](sql-server-linux-overview.md).
Para conocer los procedimientos de rendimiento recomendados para SQL Server en Linux, vea [Procedimientos de rendimiento recomendados](sql-server-linux-performance-best-practices.md).