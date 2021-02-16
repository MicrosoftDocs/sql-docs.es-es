---
title: Introducción a SQL Server en Linux
description: En este artículo se explica cómo se ejecuta SQL Server en Linux y se proporciona información para aprender más.
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9dcc6a90-0add-42c2-815b-862e4e2a21ac
ms.openlocfilehash: 4890633022a7888a21f14ae57c6f6db3934b7ce9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273056"
---
# <a name="sql-server-on-linux"></a>SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

::: moniker range="= sql-server-2017 || = sql-server-linux-2017"
A partir de SQL Server 2017, SQL Server se ejecuta en Linux. Es el mismo motor de base de datos de SQL Server, con muchas características y servicios similares, independientemente del sistema operativo.

> [!TIP]
> [SQL Server 2019](sql-server-linux-overview.md?view=sql-server-ver15&preserve-view=true) ya está disponible. Para obtener información sobre las novedades para Linux de la última versión, vea [Novedades de SQL Server 2019 para Linux](sql-server-linux-whats-new-2019.md?view=sql-server-ver15&preserve-view=true).
::: moniker-end

::: moniker range=">= sql-server-ver15 || >= sql-server-linux-ver15"
SQL Server 2019 se ejecuta en Linux. Es el mismo motor de base de datos de SQL Server, con muchas características y servicios similares, independientemente del sistema operativo. Para obtener más información sobre esta versión, vea [Novedades de SQL Server 2019 para Linux](sql-server-linux-whats-new-2019.md).
::: moniker-end

## <a name="install"></a>Instalar

Para empezar, instale SQL Server en Linux mediante uno de los siguientes inicios rápidos:

- [Instalación en Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Instalación en SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Instalación en Ubuntu](quickstart-install-connect-ubuntu.md)
- [Ejecución en Docker](quickstart-install-connect-docker.md)
- [Aprovisionar una máquina virtual de SQL en Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

> [!NOTE]
> El propio Docker se ejecuta en varias plataformas, lo que significa que puede ejecutar la imagen de Docker en Linux, Mac y Windows.

## <a name="connect"></a>Conectar

Después de la instalación, conéctese a la instancia de SQL Server en el equipo Linux. Puede conectarse de forma local o remota y con varias herramientas y controladores. Los inicios rápidos muestran cómo usar la herramienta de línea de comandos [sqlcmd](sql-server-linux-setup-tools.md). Las demás herramientas incluyen:

| Herramienta | Tutorial |
|-----|-----|
| Visual Studio Code (VS Code) | [Uso de VS Code con SQL Server en Linux](../tools/visual-studio-code/sql-server-develop-use-vscode.md) |
| SQL Server Management Studio (SSMS) | [Uso de SSMS en Windows para conectarse a SQL Server en Linux](sql-server-linux-manage-ssms.md) |
| SQL Server Data Tools (SSDT) | [Uso de SSDT con SQL Server en Linux](sql-server-linux-develop-use-ssdt.md) |

## <a name="explore"></a>Explorar

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

SQL Server 2017 tiene el mismo motor de base de datos subyacente en todas las plataformas compatibles, incluido Linux. Por lo tanto, muchas características y capacidades existentes funcionan de la misma manera en Linux. En esta parte de la documentación se exponen algunas de estas características desde la perspectiva de Linux. También se mencionan áreas que tienen requisitos únicos en Linux.

Si ya está familiarizado con SQL Server, revise las [Notas de la versión](sql-server-linux-release-notes.md) para obtener instrucciones generales y saber los problemas conocidos de esta versión. Eche un vistazo a [Novedades de SQL Server en Linux](sql-server-linux-whats-new.md) y a [Novedades de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"

[!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] tiene el mismo motor de base de datos subyacente en todas las plataformas compatibles, incluido Linux. Por lo tanto, muchas características y capacidades existentes funcionan de la misma manera en Linux. En esta parte de la documentación se exponen algunas de estas características desde la perspectiva de Linux. También se mencionan áreas que tienen requisitos únicos en Linux.

Si ya está familiarizado con SQL Server en Linux, revise las [Notas de la versión](sql-server-linux-release-notes-2019.md) para obtener instrucciones generales y saber los problemas conocidos de esta versión. Eche un vistazo a las [novedades de SQL Server 2019 en Linux](../sql-server/what-s-new-in-sql-server-ver15.md).

::: moniker-end


### <a name="all-versions-of-sql-server"></a>Todas las versiones de SQL Server

SQL Server 2017 y [!INCLUDE[SQL Server 2019](../includes/sssql19-md.md)] tienen el mismo motor de base de datos subyacente en todas las plataformas compatibles, incluido Linux. Por lo tanto, muchas características y capacidades existentes funcionan de la misma manera en Linux. En esta parte de la documentación se exponen algunas de estas características desde la perspectiva de Linux. También se mencionan áreas que tienen requisitos únicos en Linux.

Si ya está familiarizado con SQL Server en Linux, revise las notas de la versión:

- [Notas de la versión de SQL Server 2017](sql-server-linux-release-notes.md)
- [Notas de la versión de SQL Server 2019](sql-server-linux-release-notes-2019.md)

Eche un vistazo a las novedades:

- [Novedades de SQL Server 2017](sql-server-linux-whats-new.md)
- [Novedades de SQL Server 2019 en Linux](../sql-server/what-s-new-in-sql-server-ver15.md#sql-server-on-linux)

> [!TIP]
> Para obtener respuesta a las preguntas más frecuentes, vea [Preguntas más frecuentes sobre SQL Server en Linux](sql-server-linux-faq.md).

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
