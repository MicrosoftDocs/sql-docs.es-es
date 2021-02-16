---
description: Tecnologías y sistemas de base de datos en memoria
title: Tecnologías y características de sistemas de base de datos en memoria
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- in-memory systems
- in-memory technologies
- in-memory features
- database, in-memory database
- system, in-memory system
- features, in-memory features
- in-memory
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: briancarrig
ms.author: brcarrig
ms.openlocfilehash: 96f175f4eb6f913c7870f573e65201e1124dffad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351287"
---
# <a name="in-memory-database-systems-and-technologies"></a>Tecnologías y sistemas de base de datos en memoria

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Esta página está pensada para servir como página de referencia para las tecnologías y características en memoria de SQL Server. El concepto de un sistema de base de datos en memoria hace referencia a un sistema de base de datos diseñado para aprovechar las mayores capacidades de memoria disponibles en los sistemas de base de datos modernos. Una base de datos en memoria puede ser relacional o no relacional.

Por lo general, se presupone que las ventajas de rendimiento de un sistema de base de datos en memoria están principalmente preparadas para que acceda más rápido a los datos que residen en la memoria en lugar de a los datos que se encuentran incluso en los subsistemas de disco más rápidos disponibles (de forma significativa). Pero muchas cargas de trabajo de SQL Server pueden ajustar todo el espacio de trabajo en la memoria disponible. Muchos sistemas de bases de datos en memoria pueden conservar los datos en el disco y es posible que no siempre puedan ajustar todo el conjunto de datos en la memoria disponible.

Una memoria caché volátil rápida que se enfrenta a un medio considerablemente más lento pero duradero ha sido predominante para las cargas de trabajo de bases de datos relacionales. Requiere enfoques determinados para la administración de cargas de trabajo. Las oportunidades presentadas por velocidades de transferencia de memoria más rápidas, con mayor capacidad, o incluso memoria persistente, facilitan el desarrollo de nuevas tecnologías y características que pueden llevar nuevos enfoques a la administración de cargas de trabajo de bases de datos relacionales.

## <a name="hybrid-buffer-pool"></a>Grupo de búferes híbrido

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[El grupo de búferes híbridos](../database-engine/configure-windows/hybrid-buffer-pool.md) expande el grupo de búferes para los archivos de base de datos que residen en dispositivos de almacenamiento de memoria persistente direccionables en byte para plataformas Windows y Linux con [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

## <a name="memory-optimized-tempdb-metadata"></a>Metadatos `tempdb` optimizados para memoria

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduce una nueva característica, [metadatos tempdb optimizados para memoria](./databases/tempdb-database.md#memory-optimized-tempdb-metadata), que quita de forma eficaz algunos cuellos de botella de contención y desbloquea un nuevo nivel de escalabilidad para cargas de trabajo intensivas de tempdb.

## <a name="in-memory-oltp"></a>OLTP en memoria (optimización en memoria

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[OLTP en memoria](./in-memory-oltp/in-memory-oltp-in-memory-optimization.md) es una tecnología de base de datos disponible en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../includes/sssds-md.md)] para optimizar el rendimiento de escenarios de datos transitorios, carga de datos, ingesta de datos y procesamiento de transacciones.

## <a name="configuring-persistent-memory-support-for-linux"></a>Configuración de la compatibilidad con memoria persistente para Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] describe cómo configurar la memoria persistente (PMEM) con la [memoria persistente](../linux/sql-server-linux-configure-pmem.md) de la utilidad `ndctl`.

## <a name="persisted-log-buffer"></a>Búfer de registro persistente

El Service Pack 1 de [!INCLUDE[ssSQL16](../includes/sssql16-md.md)] presentó una optimización de rendimiento para cargas de trabajo intensivas de escritura enlazadas por esperas WRITELOG. La memoria persistente se utiliza para almacenar el búfer de registro. Este búfer, que es pequeño (20 MB por base de datos de usuario), debe vaciarse en el disco para que las transacciones escritas en el registro de transacciones se protejan. En el caso de cargas de trabajo OLTP intensivas de escritura, este mecanismo de vaciado puede convertirse en un cuello de botella. Con el búfer de registro en la memoria persistente, se reduce el número de operaciones necesarias para proteger el registro, lo que mejora los tiempos de transacción generales y aumenta el rendimiento de la carga de trabajo. Este proceso se presentó como [Cola de almacenamiento en caché del registro]( https://blogs.msdn.microsoft.com/bobsql/2016/11/08/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm/). Pero se ha producido un conflicto percibido con las [Copias de seguridad del registro de cola](./backup-restore/tail-log-backups-sql-server.md) y la comprensión tradicional de que el final del registro era la parte del registro de transacciones que se protegió, pero de la que aún no se ha realizado una copia de seguridad. Dado que el nombre de la característica oficial es Búfer de registro persistente, este es el nombre que se usa aquí.

Vea [Adición de búfer de registro persistente a una base de datos](./databases/add-persisted-log-buffer.md).
