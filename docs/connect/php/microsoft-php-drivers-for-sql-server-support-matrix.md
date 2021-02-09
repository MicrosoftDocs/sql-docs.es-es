---
title: Matriz de compatibilidad de controladores de Microsoft para PHP
description: Esta página contiene la matriz de compatibilidad y la directiva de ciclo de vida de soporte técnico de los controladores de Microsoft para PHP para SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca05a57c88f93a890cf546b3b1c60382f672d3be
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076437"
---
# <a name="microsoft-php-drivers-for-sql-server-support-matrix"></a>Matriz de compatibilidad de los controladores de Microsoft para PHP para SQL Server

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Esta página contiene la matriz de compatibilidad y la directiva de ciclo de vida de soporte técnico de los controladores de Microsoft para PHP para SQL Server.

## <a name="microsoft-php-drivers-support-lifecycle-matrix-and-policy"></a>Directiva y matriz de ciclo de vida de soporte técnico de los controladores de Microsoft para PHP

La directiva de ciclo de vida de soporte de Microsoft (MSL) proporciona información transparente y predecible sobre el ciclo de vida de soporte de productos de Microsoft. Las versiones 3.x, 4.x y 5.x de los controladores PHP incluyen cinco años de soporte técnico estándar del controlador a partir de la fecha de publicación. El soporte estándar se define en el [sitio web del ciclo de vida de soporte técnico de Microsoft](https://support.microsoft.com/lifecycle).

Las opciones de soporte técnico extendido y personalizado no están disponibles para controladores de Microsoft para PHP.

Se admiten los siguientes controladores de Microsoft para PHP hasta la fecha indicada de finalización del soporte técnico.

|Nombre del controlador|Versión del paquete de controladores|Finalización del soporte estándar|
|-|:-:|-|
|Controladores PHP de Microsoft 5.9 para SQL Server|5.9|29 de enero de 2026|
|Controladores PHP de Microsoft 5.8 para SQL Server|5.8|31 de enero de 2025|
|Controladores PHP de Microsoft 5.6 para SQL Server|5.6|21 de febrero de 2024|
|Controladores PHP de Microsoft 5.3 para SQL Server|5.3|20 de julio de 2023|
|Controladores PHP de Microsoft 5.2 para SQL Server|5.2|9 de febrero de 2023|
|Controladores PHP de Microsoft 4.3 para SQL Server|4.3|6 de julio de 2022|
|Controladores PHP de Microsoft 4.0 para SQL Server|4.0|11 de julio de 2021|
| &nbsp; | &nbsp; | &nbsp; |

Ya no se admiten los siguientes controladores de Microsoft para PHP.

|Nombre del controlador|Versión del paquete de controladores|Finalización del soporte estándar|
|-|:-:|-|
|Controladores PHP de Microsoft 3.2 para SQL Server|3.2|9 de marzo de 2020|
|Controladores PHP de Microsoft 3.1 para SQL Server|3.1|12 de diciembre de 2019|
|Controladores PHP de Microsoft 3.0 para SQL Server|3.0|6 de marzo de 2017|
|Controladores PHP de Microsoft 2.0 para SQL Server|2.0|10 de agosto de 2015|
|Controladores PHP de Microsoft 1.0 para SQL Server|1.0|28 de abril de 2014|
| &nbsp; | &nbsp; | &nbsp; |

## <a name="sql-server-version-certified-compatibility"></a>Compatibilidad certificada con la versión de SQL Server
 En la matriz siguiente se enumeran las versiones de base de datos probadas y certificadas como compatibles con la versión de controlador correspondiente. Nos esforzamos por mantener la compatibilidad con versiones anteriores del controlador, pero solo se prueba y certifica el último controlador compatible con las nuevas versiones de SQL Server, ya que es SQL Server lo que se publica.

|Versión del controlador&nbsp;&#8594;<br />&#8595; Versión de la base de datos|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Azure SQL Database        |Sí|Sí|Sí|Sí|Sí|Sí|   |   |
|Instancia administrada de Azure SQL|Sí|Sí|Sí|Sí|Sí|Sí|   |   |
|Azure Synapse Analytics   |Sí|Sí|Sí|Sí|Sí|Sí|   |   |
|SQL Server 2019           |Sí|Sí|   |   |   |   |   |   |
|SQL Server 2017           |Sí|Sí|Sí|Sí|Sí|Sí|   |   |
|SQL Server 2016           |Sí|Sí|Sí|Sí|Sí|Sí|Sí|   |
|SQL Server 2014           |Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|SQL Server 2012           |Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|SQL Server 2008 R2        |   |   |Sí|Sí|Sí|Sí|Sí|Sí|
|SQL Server 2008           |   |   |   |   |   |   |Sí|Sí|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

Para información sobre el uso de PHP con Azure SQL Database, consulte [Conexión a Microsoft Azure SQL Database](connecting-to-microsoft-azure-sql-database.md).

## <a name="php-version-support"></a>Compatibilidad de versiones de PHP

Se admiten las versiones siguientes de PHP con la versión indicada de los controladores de Microsoft para PHP:

|Versión del controlador&nbsp;&#8594;<br />&#8595; versión de PHP|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|:---:|---|---|---|---|---|---|---|---|
|8.0|8.0.0 y versiones posteriores |                |                |                |                |       |        |        |
|7.4|7.4.0+ |7.4.0+          |                |                |                |       |        |        |
|7.3|7.3.0+ |7.3.0+          |7.3.0+          |                |                |       |        |        |
|7.2|       |7.2+<sup>1</sup>|7.2+<sup>1</sup>|7.2+<sup>1</sup>|7.2+<sup>1</sup>|       |        |        |
|7.1|       |                |7.1.0+          |7.1.0+          |7.1.0+          |7.1.0+ |        |        |
|7.0|       |                |                |7.0.0+          |7.0.0+          |7.0.0+ |7.0.0+  |        |
|5.6|       |                |                |                |                |       |        |5.6.4+  |
|5.5|       |                |                |                |                |       |        |5.5.16+ |
|5.4|       |                |                |                |                |       |        |5.4.32  |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> Las versiones 7.2.1 y posteriores se admiten en Windows, mientras que la versión 7.2.0 y las versiones posteriores se admiten en Linux y macOS.

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

Las versiones siguientes del sistema operativo Windows son compatibles con la versión indicada de los controladores de Microsoft para PHP:

|Versión del controlador&nbsp;&#8594;<br />&#8595; sistema operativo|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Windows Server 2019                 |Sí|Sí|Sí|   |   |   |   |   |
|Windows Server 2016                 |Sí|Sí|Sí|Sí|Sí|Sí|   |   |
|Windows Server 2012 R2              |Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Windows Server 2012                 |Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Windows Server 2008 R2 SP1          |   |   |   |   |   |   |Sí|Sí|
|Windows Server 2008 R2              |   |   |   |   |   |   |   |   |
|Windows Server 2008 SP2             |   |   |   |   |   |   |Sí|Sí|
|Windows 10                          |Sí|Sí|Sí|Sí|Sí|Sí|Sí|   |
|Windows 8.1                         |Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|
|Windows 8                           |   |   |   |   |   |Sí|Sí|Sí|
|Windows 7 SP1                       |   |   |   |   |   |   |Sí|Sí|
|Windows Vista SP2                   |   |   |   |   |   |   |Sí|Sí|
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

Las versiones siguientes de los sistemas operativos Linux y macOS (solo de 64 bits) son compatibles con la versión indicada de los controladores de Microsoft para PHP:

|Versión del controlador&nbsp;&#8594;<br />&#8595; sistema operativo|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Ubuntu 20.10 (64 bits)               |Sí|   |   |   |   |   |   |   |
|Ubuntu 20.04 (64 bits)               |Sí|Sí|   |   |   |   |   |   |
|Ubuntu 19.10 (64 bits)               |   |Sí|   |   |   |   |   |   |
|Ubuntu 18.10 (64 bits)               |   |   |Sí|   |   |   |   |   |
|Ubuntu 18.04 (64 bits)               |Sí|Sí|Sí|Sí|   |   |   |   |
|Ubuntu 17.10 (64 bits)               |   |   |   |Sí|Sí|   |   |   |
|Ubuntu 16.04 (64 bits)               |Sí|Sí|Sí|Sí|Sí|Sí|Sí|   |
|Ubuntu 15.10 (64 bits)               |   |   |   |   |   |Sí|   |   |
|Ubuntu 15.04 (64 bits)               |   |   |   |   |   |   |Sí|   |
|Debian 10 (64 bits)                  |Sí|Sí|   |   |   |   |   |   |
|Debian 9 (64 bits)                   |Sí|Sí|Sí|Sí|Sí|   |   |   |
|Debian 8 (64 bits)                   |   |Sí|Sí|Sí|Sí|Sí|   |   |
|Red Hat Enterprise Linux 8 (64 bits) |Sí|Sí|   |   |   |   |   |   |
|Red Hat Enterprise Linux 7 (64 bits) |Sí|Sí|Sí|Sí|Sí|Sí|Sí|   |
|Suse Enterprise Linux 15 (64 bits)   |Sí|Sí|Sí|   |   |   |   |   |
|Suse Enterprise Linux 12 (64 bits)   |Sí|Sí|Sí|Sí|Sí|   |   |   |
|Alpine Linux 3.12 (64 bits)          |Sí|   |   |   |   |   |   |   |
|Alpine Linux 3.11 (64 bits)          |Sí|Sí<sup>1</sup>|   |   |   |   |   |   |
|macOS Big Sur (64 bits)              |Sí|   |   |   |   |   |   |   |
|macOS Catalina (64 bits)             |Sí|Sí|   |   |   |   |   |   |
|macOS Mojave (64 bits)               |Sí|Sí|Sí|   |   |   |   |   |
|macOS High Sierra (64 bits)          |   |Sí|Sí|Sí|   |   |   |   |
|macOS Sierra (64 bits)               |   |   |Sí|Sí|Sí|Sí|   |   |
|macOS El Capitan (64 bits)           |   |   |   |Sí|Sí|Sí|   |   |
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |

<sup>1</sup> La compatibilidad con Alpine Linux es experimental para la versión 5.8.0. La versión 5.8.1 presenta soporte técnico de producción.

## <a name="see-also"></a>Consulte también

[Notas de la versión](release-notes-php-sql-driver.md)

[Recursos de soporte técnico](support-resources-for-the-php-sql-driver.md)

[Requisitos del sistema](system-requirements-for-the-php-sql-driver.md)
