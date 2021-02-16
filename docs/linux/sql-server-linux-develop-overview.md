---
title: Desarrollo de aplicaciones para SQL Server en Linux
description: Puede crear aplicaciones que se conecten a SQL Server en Linux desde diversos lenguajes de programación y marcos de trabajo web conocidos.
author: VanMSFT
ms.author: vanto
ms.date: 11/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 758cb738-b018-465b-9ab0-59a24b892e66
ms.openlocfilehash: 01655b948afa721365b7ea7adebd1e22f8f6f851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273086"
---
# <a name="how-to-get-started-developing-applications-for-sql-server-on-linux"></a>Procedimiento para empezar a desarrollar aplicaciones para SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Puede crear aplicaciones que se conecten a SQL Server en Linux y lo usen desde diversos lenguajes de programación, como C#, Java, Node.js, PHP, Python, Ruby y C++. También puede usar marcos web populares y marcos de asignación relacional de objetos (ORM).

> [!VIDEO https://channel9.msdn.com/events/Connect/2017/T153/player]

> [!TIP]
> Estas mismas opciones de desarrollo también le permiten tener SQL Server como destino en otras plataformas. Las aplicaciones pueden tener SQL Server como destino en ejecución de forma local o en la nube, en Linux, Windows o Docker en macOS. O bien puede tener Azure SQL Database y Azure Synapse Analytics como destino.

## <a name="try-the-tutorials"></a>Prueba de los tutoriales

La mejor manera de empezar y compilar aplicaciones con SQL Server es probarlo usted mismo.

- Vaya a [Tutoriales de introducción](https://aka.ms/sqldev).
- Seleccione su lenguaje y plataforma de desarrollo.
- Pruebe los ejemplos de código.

> [!TIP]
> Si quiere desarrollar para SQL Server en Docker, eche un vistazo a los tutoriales de **macOS**.

## <a name="create-new-applications"></a>Crear nuevas aplicaciones

Si va a crear una nueva aplicación, eche un vistazo a la lista de [bibliotecas de conectividad](sql-server-linux-develop-connectivity-libraries.md) para ver un resumen de los conectores y los marcos populares que están disponibles para diversos lenguajes de programación.

## <a name="use-existing-applications"></a>Uso de aplicaciones existentes

Si tiene una aplicación de base de datos existente, puede cambiar simplemente su cadena de conexión para tener SQL Server en Linux como destino. Asegúrese de leer sobre los [problemas conocidos](sql-server-linux-release-notes.md) de SQL Server en Linux.

## <a name="use-existing-sql-tools-on-windows-with-sql-server-on-linux"></a>Uso de las herramientas de SQL existentes en Windows con SQL Server en Linux

Las herramientas que se ejecutan actualmente en Windows, como SSMS, SSDT y PowerShell, también funcionan con SQL Server en Linux. Aunque no se ejecutan de forma nativa en Linux, puede administrar instancias remotas de SQL Server en Linux. 

Vea los siguientes temas para obtener más información:

- [SQL Server Management Studio (SSMS)](sql-server-linux-manage-ssms.md)
- [SQL Server Data Tools (SSDT)](sql-server-linux-develop-use-ssdt.md)
- [SQL PowerShell](sql-server-linux-manage-powershell.md)

> [!Note]
> Asegúrese de que está usando las versiones más recientes de estas herramientas para tener la mejor experiencia.

## <a name="use-new-sql-tools-for-linux"></a>Uso de las nuevas herramientas de SQL para Linux

Puede usar la nueva [extensión mssql](https://aka.ms/mssql-marketplace) para [Visual Studio Code](https://code.visualstudio.com) en Linux, macOS y Windows. Para consultar un tutorial detallado, vea el siguiente tutorial:

- [Usar Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)

También puede usar las nuevas herramientas de línea de comandos que son nativas para Linux. Entre estas herramientas se incluyen las siguientes:

- [sqlcmd](../tools/sqlcmd-utility.md)
- [bcp](sql-server-linux-migrate-bcp.md)
- [mssql-conf](sql-server-linux-configure-mssql-conf.md)

## <a name="next-steps"></a>Pasos siguientes

Para empezar, instale SQL Server en Linux mediante uno de los siguientes inicios rápidos:

- [Instalación en Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Instalación en SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Instalación en Ubuntu](quickstart-install-connect-ubuntu.md)
- [Ejecución en Docker](quickstart-install-connect-ubuntu.md)