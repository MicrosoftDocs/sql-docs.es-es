---
title: Novedades de SQL Server 2019 en Linux
description: En este artículo se destacan las novedades de SQL Server 2019 en Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/12/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: d6978bfa68d3a9b922e46454481618e448e91b96
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346204"
---
# <a name="whats-new-for-sql-server-2019-on-linux"></a>Novedades de SQL Server 2019 en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se describen las características y los servicios principales disponibles para SQL Server 2019 en ejecución en Linux. Para descargar los paquetes y consultar los problemas conocidos, vea [Notas de la versión](sql-server-linux-release-notes-2019.md).

## <a name="ubuntu-1804-supported"></a>Compatibilidad con Ubuntu 18.04

A partir de SQL Server 2019 CU3,ahora se admite Ubuntu 18.04. Consulte nuestro Inicio rápido sobre [instalación de SQL Server y creación de una base de datos en Ubuntu](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="rhel-8-supported"></a>Compatibilidad con RHEL 8

A partir de SQL Server 2019 CU1,ahora se admite RHEL 8. Consulte nuestro Inicio rápido sobre [instalación de SQL Server y creación de una base de datos en Red Hat](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="updates"></a>Actualizaciones

Las actualizaciones se han realizado en SQL Server 2019 en Linux:

| Nueva característica o actualización | Detalles |
|:-----|:-----|
|Compatibilidad con la replicación |[Replicación de SQL Server en Linux](sql-server-linux-replication.md)
|Compatibilidad con el Coordinador de transacciones distribuidas de Microsoft (MSDTC) |[Cómo configurar el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Linux](sql-server-linux-configure-msdtc.md) |
|Compatibilidad de OpenLDAP con proveedores terceros de AD |[Tutorial: Usar la autenticación de Active Directory con SQL Server en Linux](sql-server-linux-active-directory-authentication.md) |
|Machine Learning en Linux |[Instalar Machine Learning en Linux](sql-server-linux-setup-machine-learning.md) |
|Mejoras de `tempdb` | De forma predeterminada, una nueva instalación de SQL Server en Linux crea varios archivos de datos `tempdb` en función del número de núcleos lógicos (con un máximo de 8 archivos de datos). Esto no es aplicable a actualizaciones de versión principal o secundaria en contexto. Cada archivo `tempdb` es de 8 MB con un crecimiento automático de 64 MB. Este comportamiento es similar a la instalación de SQL Server predeterminada en Windows. |
| PolyBase en Linux | [Instale PolyBase](../relational-databases/polybase/polybase-linux-setup.md) en Linux para conectores que no son Hadoop.<br/><br/>[Asignación del tipo de PolyBase](../relational-databases/polybase/polybase-type-mapping.md). |
| Compatibilidad con Captura de datos modificados (CDC) | Captura de datos modificados (CDC) ahora es compatible con Linux para SQL Server 2019. |
| Registro de contenedor de Microsoft | Ahora [Registro de contenedor de Microsoft](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/) reemplaza a Docker Hub para nuevas imágenes de contenedor de Microsoft oficiales, incluido [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]. |
| Contenedores no raíz | [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] presenta la capacidad de crear contenedores más seguros mediante el inicio del proceso de [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] como un usuario no raíz de forma predeterminada. Vea [Compilación y ejecución de contenedores de SQL Server como un usuario no raíz](./sql-server-linux-docker-container-security.md#buildnonrootcontainer) para más información. |

## <a name="next-steps"></a>Pasos siguientes

Para instalar SQL Server en Linux, use uno de los siguientes tutoriales:

- [Instalación en Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true)
- [Instalación en SUSE Linux Enterprise Server](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true)
- [Instalación en Ubuntu](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true)
- [Ejecución en Docker](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true)
- [Aprovisionar una máquina virtual de SQL en Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

Para obtener respuesta a las preguntas más frecuentes, vea [Preguntas más frecuentes sobre SQL Server en Linux](sql-server-linux-faq.md). Para ver otras mejoras presentadas en SQL Server 2019, vea [Novedades de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]