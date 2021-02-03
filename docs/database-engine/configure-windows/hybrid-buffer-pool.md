---
title: Grupo de búferes híbrido | Microsoft Docs
description: Vea cómo el grupo de búferes híbrido permite el acceso a los dispositivos de memoria persistentes a través del bus de memoria. Active y desactive esta característica de SQL Server 2019 y vea procedimientos recomendados.
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
ms.assetid: ''
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: ccf508d0415bc6baabc9c054a95ec26cf7989880
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236523"
---
# <a name="hybrid-buffer-pool"></a>Grupo de búferes híbrido
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

El grupo de búferes híbridos permite que los objetos de grupo de búfer hagan referencia a las páginas de datos de los archivos de base de datos que residen en dispositivos de memoria persistente (PMEM), en lugar de a copias de las páginas de datos almacenadas en caché en DRAM volátil. Esta característica se introdujo en [!INCLUDE[sqlv15](../../includes/sssql19-md.md)].

![Grupo de búferes híbrido](./media/hybrid-buffer-pool.png)

Los dispositivos de memoria persistente (PMEM) son direccionables por bytes y si se usa un sistema de archivos con reconocimiento de memoria persistente (como XFS, EXT4 o NTFS) de acceso directo (DAX), se puede tener acceso a los archivos del sistema de archivos mediante las API habituales del sistema de archivos del sistema operativo. Como alternativa, puede realizar lo que se conoce como operaciones de carga y almacenamiento en las asignaciones de memoria de los archivos en el dispositivo. Esto permite que las aplicaciones compatibles con PMEM, como SQL Server, tengan acceso a los archivos del dispositivo sin atravesar la pila de almacenamiento tradicional.

El grupo de búferes híbridos usa esta capacidad de desempeño de operaciones de carga y almacenamiento en archivos de asignación de memoria para aprovechar el dispositivo PMEM como memoria caché para el grupo de búferes, así como para almacenar archivos de base de datos. Esto crea la situación única en la que una lectura lógica y una lectura física son esencialmente la misma operación. Los dispositivos de memoria persistentes son accesibles a través del bus de memoria, al igual que la DRAM volátil normal.

Solo se almacenan en caché las páginas de datos sin errores en el dispositivo para el grupo de búferes híbridos. Cuando una página se marca como con errores, se copia en el grupo de búferes DRAM antes de escribirse definitivamente en el dispositivo PMEM y se vuelve a marcar como sin errores. Esto ocurrirá durante las operaciones de punto de comprobación normales de una manera similar a la que se realiza en un dispositivo de bloque estándar.

La característica de grupo de búferes híbrido está disponible para Windows y Linux. El dispositivo PMEM debe formatearse con un sistema de archivos que admita DAX (DirectAccess). Los sistemas de archivos XFS, EXT4 y NTFS admiten DAX. SQL Server detectará automáticamente si los archivos de datos residen en un dispositivo PMEM con el formato apropiado y realizará una asignación de memoria al espacio de los archivos de base de datos al inicio, cuando se adjunta, restaura o crea una base de datos nueva.

Para más información, consulte:

* [Descripción e implementación de memoria persistente (Windows)](/windows-server/storage/storage-spaces/deploy-pmem/)
* [Configuración de la memoria persistente (PMEM) para SQL Server en Linux](../../linux/sql-server-linux-configure-pmem.md)


## <a name="enable-hybrid-buffer-pool"></a>Habilitar el grupo de búferes híbrido

[!INCLUDE[sqlv15](../../includes/sssql19-md.md)] presenta el lenguaje dinámico de datos (DDL) para controlar el grupo de búferes híbrido.

En el ejemplo siguiente se habilita el grupo de búferes híbrido para una instancia de SQL Server:

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
```

De forma predeterminada, el grupo de búferes híbrido está deshabilitado en el ámbito de la instancia. Cabe decir que, para que el cambio de configuración surta efecto, debe reiniciar la instancia de SQL Server. Se requiere un reinicio para facilitar la asignación de suficientes páginas hash que den cabida a la capacidad de PMEM total en el servidor.

En el ejemplo siguiente se habilita el grupo de búferes híbrido para una base de datos específica.

```sql
ALTER DATABASE <databaseName> SET MEMORY_OPTIMIZED = ON;
```

De forma predeterminada, el grupo de búferes híbrido está habilitado en el ámbito de la base de datos.

## <a name="disable-hybrid-buffer-pool"></a>Deshabilitar el grupo de búferes híbrido

El siguiente ejemplo deshabilita el grupo de búferes híbrido en el nivel de la instancia:

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = OFF;
```

De forma predeterminada, el grupo de búferes híbrido está deshabilitado en el ámbito del nivel. Para que se produzca este cambio, la instancia debe reiniciarse. Esto garantiza que se asignen suficientes páginas hash para el grupo de búferes, ya que ahora es necesario tener en cuenta la capacidad de PMEM en el servidor.

En el ejemplo siguiente se deshabilita el grupo de búferes híbrido para una base de datos específica.

```sql
ALTER DATABASE <databaseName> SET MEMORY_OPTIMIZED = OFF;
```

De forma predeterminada, el grupo de búferes híbrido está habilitado en el ámbito de la base de datos.

## <a name="view-hybrid-buffer-pool-configuration"></a>Ver la configuración del grupo de búferes híbrido

En el ejemplo siguiente se devuelve el estado de configuración del grupo de búferes híbridos actual de la instancia.

```sql
SELECT * FROM
sys.server_memory_optimized_hybrid_buffer_pool_configuration;
```

En el ejemplo siguiente se enumeran las bases de datos y la configuración del nivel de base de datos para el grupo de búferes híbrido (`is_memory_optimized_enabled`).

```sql
SELECT name, is_memory_optimized_enabled FROM sys.databases;
```

## <a name="best-practices-for-hybrid-buffer-pool"></a>Procedimientos recomendados para el grupo de búferes híbrido

 - Al dar formato al dispositivo PMEM en Windows, use el tamaño de unidad de asignación más grande disponible para NTFS (2 MB en Windows Server 2019) y asegúrese de que el dispositivo se ha formateado para DAX (DirectAccess).

 - Use [Páginas bloqueadas en la memoria](./enable-the-lock-pages-in-memory-option-windows.md) en Windows.

 - Los tamaños de archivo deben ser múltiplo de 2 MB (el módulo de 2 MB debe ser igual a cero).

 - Si la configuración del ámbito del servidor para el grupo de búferes híbridos está deshabilitada, ninguna base de datos de usuario utilizará esta característica.

 - Si está habilitada la configuración de ámbito de servidor para el grupo de búferes híbridos, puede usar la configuración de ámbito de base de datos para deshabilitar la característica para las bases de datos de usuario individuales.
