---
title: Novedades de SQL Server 2017 en Linux
description: En este artículo, obtendrá información sobre las características y los servicios principales disponibles para SQL Server 2017 en ejecución en Linux.
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: e4d772f7355982b1856ac467558242c561e6b391
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346192"
---
# <a name="whats-new-for-sql-server-2017-on-linux"></a>Novedades de SQL Server 2017 en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se describen las características y los servicios principales disponibles para SQL Server 2017 en ejecución en Linux.

> [!NOTE]
> Además de las funcionalidades de este artículo, se publican actualizaciones acumulativas a intervalos regulares. Estas actualizaciones acumulativas proporcionan muchas mejoras y correcciones. Para obtener más información sobre la versión de CU más reciente, vea [https://aka.ms/sql2017cu](https://aka.ms/sql2017cu). Para descargar los paquetes y consultar los problemas conocidos, vea [Notas de la versión](sql-server-linux-release-notes.md).

## <a name="ubuntu-1804-supported"></a>Compatibilidad con Ubuntu 18.04

A partir de SQL Server 2017 CU20, ahora se admite Ubuntu 18.04. Consulte nuestro Inicio rápido sobre [instalación de SQL Server y creación de una base de datos en Ubuntu](quickstart-install-connect-ubuntu.md).

## <a name="rhel-8-supported"></a>Compatibilidad con RHEL 8

A partir de SQL Server 2017 CU20, ahora se admite RHEL 8. Consulte nuestro Inicio rápido sobre [instalación de SQL Server y creación de una base de datos en Red Hat](quickstart-install-connect-red-hat.md).

## <a name="sql-server-database-engine"></a>Motor de base de datos de SQL Server

- Se han habilitado las funcionalidades principales del motor de base de datos de SQL Server.
- Compatibilidad con rutas de acceso nativas de Linux.
- Compatibilidad con IPv6.
- Compatibilidad con archivos de base de datos en NFS.
- Se ha habilitado el cifrado de [Seguridad de la capa de transporte](sql-server-linux-encrypted-connections.md) (TLS).
- Se ha habilitado la [autenticación de Azure Active Directory](sql-server-linux-active-directory-authentication.md).
- [Funcionalidad de grupos de disponibilidad](sql-server-linux-availability-group-overview.md) para la alta disponibilidad.
- Compatibilidad con la [búsqueda de texto completo](sql-server-linux-setup-full-text-search.md).

## <a name="sql-server-agent"></a>Agente SQL Server

- Se ha habilitado la compatibilidad del [Agente SQL Server](sql-server-linux-setup-sql-agent.md) con las siguientes tareas:
  - [Trabajos de Transact-SQL](sql-server-linux-run-sql-server-agent-job.md)
  - [Correo electrónico de base de datos](sql-server-linux-db-mail-sql-agent.md)
  - [Trasvase de registros](sql-server-linux-use-log-shipping.md)

## <a name="sql-server-integration-services-ssis"></a>SQL Server Integration Services (SSIS)

- Capacidad de ejecutar paquetes SSIS en Linux. Para obtener más información, vea [Configuración de SQL Server Integration Services en Linux con ssis-conf](sql-server-linux-configure-ssis.md).

## <a name="other-improvements"></a>Otras mejoras

- Herramienta de configuración de la línea de comandos, [mssql-conf](sql-server-linux-configure-mssql-conf.md).
- Compatibilidad de la instalación desatendida con las [variables de entorno](sql-server-linux-configure-environment-variables.md).
- [Extensión de mssql-server de Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md) multiplataforma.
- Generador de scripts multiplataforma, [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli/blob/dev/doc/usage_guide.md).
- Supervisión de la vista de administración dinámica (DMV) multiplataforma, [herramienta DBFS](https://github.com/Microsoft/dbfs).

## <a name="next-steps"></a>Pasos siguientes

Para instalar SQL Server en Linux, use uno de los siguientes tutoriales:

- [Instalación en Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Instalación en SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Instalación en Ubuntu](quickstart-install-connect-ubuntu.md)
- [Ejecución en Docker](quickstart-install-connect-docker.md)
- [Aprovisionar una máquina virtual de SQL en Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

Para obtener respuesta a las preguntas más frecuentes, vea [Preguntas más frecuentes sobre SQL Server en Linux](sql-server-linux-faq.md). Para ver otras mejoras presentadas en SQL Server 2017, consulte [Novedades de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
