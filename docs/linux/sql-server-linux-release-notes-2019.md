---
title: Notas de la versión de SQL Server 2019 en Linux
description: Este artículo contiene las notas de la versión y las características admitidas de SQL Server 2019 ejecutándose en Linux. Se incluyen las notas de la versión más reciente y de varias versiones anteriores.
author: VanMSFT
ms.author: vanto
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: c81ca4cb8d531626c7f5ae51f8b4e264d907d2ce
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464714"
---
# <a name="release-notes-for-sql-server-2019-on-linux"></a>Notas de la versión de SQL Server 2019 en Linux

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx-linuxonly.md)]

Las siguientes notas de la versión se aplican a la versión de SQL Server 2019 que se ejecuta en Linux. Este artículo se divide en secciones para cada versión. Cada versión tiene un vínculo a un artículo de asistencia en el que se describen los cambios de la CU, así como vínculos a las descargas de los paquetes de Linux.

> [!TIP]
> Para obtener información sobre las nuevas características de Linux en SQL Server 2019, consulte [Novedades de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true#sql-server-on-linux).

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

## <a name="tools"></a>Herramientas

La mayoría de las herramientas cliente existentes que tienen SQL Server como destino pueden dirigirse sin problemas a SQL Server en ejecución en Linux. Algunas herramientas pueden tener un requisito de versión específica para funcionar correctamente con Linux. Para obtener una lista completa de las herramientas de SQL Server, vea [Herramientas y utilidades de SQL para SQL Server](../tools/overview-sql-tools.md).

## <a name="release-history"></a>Historial de versiones

En la tabla siguiente se muestra el historial de las versiones de SQL Server 2019.

| Release                   | Versión       | Fecha de la versión |
|---------------------------|---------------|--------------|
| [CU9](#cu9)               | 15.0.4102.2   | 10/02/2021   |
| [CU8-GDR](#cu8-GDR)               | 15.0.4083.2  | 2021-01-12   |
| [CU8](#cu8)               | 15.0.4073.23  | 07/10/2020   |
| [CU7 (quitado)](https://support.microsoft.com/help/4570012)     | 15.0.4063.15  | 02-09-2020   |
| [CU6](#cu6)               | 15.0.4053.23  | 04-08-2020   |
| [CU5](#cu5)               | 15.0.4043.16  | 22-06-2020   |
| [CU4](#cu4)               | 15.0.4033.1   | 31-03-2020   |
| [CU3](#cu3)               | 15.0.4023.6   | 12-03-2020   |
| [CU2](#cu2)               | 15.0.4013.40  | 13-02-2020   |
| [CU1](#cu1)               | 15.0.4003.23  | 07-01-2020   |
| [GA](#ga)                 | 15.0.2000.5   | 2019-11-04   |
| [Versión candidata para lanzamiento](#rc)  | 15.0.1900.25  | 2019-08-21   |

## <a name="how-to-install-updates"></a><a id="cuinstall"></a> Procedimiento para instalar las actualizaciones

Si ha configurado el repositorio de actualizaciones acumulativas (mssql-server-2019), obtendrá la actualización acumulativa más reciente de paquetes de SQL Server al realizar nuevas instalaciones. Si necesita imágenes de contenedor de Docker, consulte las imágenes oficiales de [Microsoft SQL Server en Linux para el motor de Docker](https://hub.docker.com/r/microsoft/mssql-server/). Para obtener más información sobre la configuración del repositorio, vea [Configuración de repositorios para SQL Server en Linux](sql-server-linux-change-repo.md).

Si va a actualizar paquetes de SQL Server existentes, ejecute el comando de actualización adecuado para cada paquete a fin de obtener la CU más reciente. Para obtener instrucciones de actualización específicas para cada paquete, vea las siguientes guías de instalación:

- [Instalar el paquete de SQL Server](sql-server-linux-setup.md#upgrade)
- [Instalar la búsqueda de texto completo SQL Server en Linux](sql-server-linux-setup-full-text-search.md)
- [Instalar SQL Server Integration Services](sql-server-linux-setup-ssis.md)
- [Instalar la compatibilidad con R y Python de la versión de SQL Server 2019 Machine Learning Services en Linux](sql-server-linux-setup-machine-learning.md)
- [Instalación del paquete de PolyBase](../relational-databases/polybase/polybase-linux-setup.md)
- [Habilitar el Agente SQL Server](sql-server-linux-setup-sql-agent.md)

## <a name="cu9-february-2021"></a><a id="cu9"></a> CU 9 (febrero de 2021)

Esta es la versión de la actualización acumulativa 9 (CU 9) de SQL Server 2019 (15.x). La versión del Motor de base de datos de SQL Server es la 15.0.4102.2. Para obtener información sobre las correcciones y mejoras, vea <https://support.microsoft.com/help/5000642>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> **Ubuntu 18.04** ya se admite en SQL Server 2019 a partir de la CU3. Los vínculos de instalación de paquetes sin conexión para Ubuntu señalan a los paquetes de Ubuntu 18.04. Si busca paquetes de Ubuntu 16.04, consulte la ruta de descarga <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4102.2-4 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4102.2-4.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4102.2-4.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4102.2-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4102.2-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4102.2-4.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4102.2-4.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4102.2-4 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4102.2-4.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4102.2-4.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4102.2-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4102.2-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4102.2-4.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4102.2-4.x86_64.rpm)|
| Paquete de Debian para Ubuntu 18.04 | 15.0.4102.2-4 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4102.2-4_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4102.2-4_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4102.2-4_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4102.2-4_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4102.2-4_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4102.2-4_amd64.deb)|

## <a name="cu8-gdr-january-2021"></a><a id="cu8-GDR"></a> CU8-GDR (enero de 2021)

Esta es la versión de la actualización acumulativa 8-GDR (CU8-GDR) de SQL Server 2019 (15.x). La versión del Motor de base de datos de SQL Server es la 15.0.4083.2. Para obtener información sobre las correcciones y mejoras, vea <https://support.microsoft.com/help/4577194>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> **Ubuntu 18.04** ya se admite en SQL Server 2019 a partir de la CU3. Los vínculos de instalación de paquetes sin conexión para Ubuntu señalan a los paquetes de Ubuntu 18.04. Si busca paquetes de Ubuntu 16.04, consulte la ruta de descarga <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4083.2-15 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4083.2-15.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4083.2-15.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4083.2-15.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4083.2-15.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4083.2-15.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4083.2-15.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4083.2-15 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4083.2-15.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4083.2-15.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4083.2-15.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4083.2-15.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4083.2-15.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4083.2-15.x86_64.rpm)|
| Paquete de Debian para Ubuntu 18.04 | 15.0.4083.2-15 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4083.2-15_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4083.2-15_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4083.2-15_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4083.2-15_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4083.2-15_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4083.2-15_amd64.deb)|

## <a name="cu8-september-2020"></a><a id="cu8"></a> CU8 (septiembre de 2020)

Esta es la versión de la actualización acumulativa 8 (CU8) de SQL Server 2019 (15.x). La versión del Motor de base de datos de SQL Server es la 15.0.4073.23. Para obtener información sobre las correcciones y mejoras, vea <https://support.microsoft.com/help/4577194>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> **Ubuntu 18.04** ya se admite en SQL Server 2019 a partir de la CU3. Los vínculos de instalación de paquetes sin conexión para Ubuntu señalan a los paquetes de Ubuntu 18.04. Si busca paquetes de Ubuntu 16.04, consulte la ruta de descarga <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4073.23-4 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4073.23-4 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4073.23-4.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4073.23-4.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4073.23-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4073.23-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4073.23-4.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4073.23-4.x86_64.rpm)|
| Paquete de Debian para Ubuntu 18.04 | 15.0.4073.23-4 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4073.23-4_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4073.23-4_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4073.23-4_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4073.23-4_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4073.23-4_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4073.23-4_amd64.deb)|

## <a name="cu6-july-2020"></a><a id="cu6"></a> CU6 (julio de 2020)

Esta es la versión de la actualización acumulativa 6 (CU6) de SQL Server 2019 (15.x). La versión del Motor de base de datos de SQL Server es 15.0.4053.23. Para obtener información sobre las correcciones y mejoras, vea <https://support.microsoft.com/help/4563110>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> **Ubuntu 18.04** ya se admite en SQL Server 2019 a partir de la CU3. Los vínculos de instalación de paquetes sin conexión para Ubuntu señalan a los paquetes de Ubuntu 18.04. Si busca paquetes de Ubuntu 16.04, consulte la ruta de descarga <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4053.23-2 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4053.23-2 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4053.23-2.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4053.23-2.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4053.23-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4053.23-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4053.23-2.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4053.23-2.x86_64.rpm)|
| Paquete de Debian para Ubuntu 18.04 | 15.0.4053.23-2 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4053.23-2_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4053.23-2_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4053.23-2_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4053.23-2_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4053.23-2_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4053.23-2_amd64.deb)|

## <a name="cu5-june-2020"></a><a id="cu5"></a> CU5 (junio de 2020)

Esta es la versión de la actualización acumulativa 5 (CU5) de SQL Server 2019 (15.x). La versión del Motor de base de datos de SQL Server es 15.0.4043.16. Para obtener información sobre las correcciones y mejoras, vea <https://support.microsoft.com/help/4552255>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> **Ubuntu 18.04** ya se admite en SQL Server 2019 a partir de la CU3. Los vínculos de instalación de paquetes sin conexión para Ubuntu señalan a los paquetes de Ubuntu 18.04. Si busca paquetes de Ubuntu 16.04, consulte la ruta de descarga <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4043.16-4 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4043.16-4 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4043.16-4.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4043.16-4.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4043.16-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4043.16-4.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4043.16-4.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4043.16-4.x86_64.rpm)|
| Paquete de Debian para Ubuntu 18.04 | 15.0.4043.16-4 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4043.16-4_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4043.16-4_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4043.16-4_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4043.16-4_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4043.16-4_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4043.16-4_amd64.deb)|

## <a name="cu4-april-2020"></a><a id="cu4"></a> CU4 (abril de 2020)

Esta es la versión de la actualización acumulativa 4 (CU4) de SQL Server 2019 (15.x). La versión del Motor de base de datos de SQL Server es la 15.0.4033.1. Para obtener información sobre las correcciones y mejoras, vea <https://support.microsoft.com/help/4548597>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> **Ubuntu 18.04** ya se admite en SQL Server 2019 a partir de la CU3. Los vínculos de instalación de paquetes sin conexión para Ubuntu señalan a los paquetes de Ubuntu 18.04. Si busca paquetes de Ubuntu 16.04, consulte la ruta de descarga <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4033.1-2 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4033.1-2 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4033.1-2.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4033.1-2.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4033.1-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4033.1-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4033.1-2.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4033.1-2.x86_64.rpm)|
| Paquete de Debian para Ubuntu 18.04 | 15.0.4033.1-2 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4033.1-2_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4033.1-2_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4033.1-2_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4033.1-2_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4033.1-2_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4033.1-2_amd64.deb)|

## <a name="cu3-march-2020"></a><a id="cu3"></a> CU3 (marzo de 2020)

Esta es la versión de la actualización acumulativa 3 (CU3) de SQL Server 2019 (15.x). La versión de Motor de base de datos de SQL Server de esta versión es la 15.0.4023.6. Para obtener información sobre las correcciones y mejoras, vea <https://support.microsoft.com/help/4538853>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.
>
> **Ubuntu 18.04** ya se admite en SQL Server 2019 a partir de la CU3. Los vínculos de instalación de paquetes sin conexión para Ubuntu señalan a los paquetes de Ubuntu 18.04. Si busca paquetes de Ubuntu 16.04, consulte la ruta de descarga <https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/>

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4023.6-2 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4023.6-2 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4023.6-2.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4023.6-2.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4023.6-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4023.6-2.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4023.6-2.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4023.6-2.x86_64.rpm)|
| Paquete de Debian para Ubuntu 18.04 | 15.0.4023.6-2 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4023.6-2_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4023.6-2_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4023.6-2_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4023.6-2_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4023.6-2_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4023.6-2_amd64.deb)|

## <a name="cu2-february-2020"></a><a id="cu2"></a> CU2 (febrero de 2020)

Esta es la versión de la actualización acumulativa 2 (CU2) de SQL Server 2019 (15.x). La versión de Motor de base de datos de SQL Server de esta versión es la 15.0.4013.40. Para obtener información sobre las correcciones y mejoras, vea <https://support.microsoft.com/help/4536075>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4013.40-8 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4013.40-8 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4013.40-8.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4013.40-8.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4013.40-8.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4013.40-8.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4013.40-8.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4013.40-8.x86_64.rpm)|
| Paquete de Debian para Ubuntu 16.04 | 15.0.4013.40-8 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4013.40-8_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4013.40-8_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4013.40-8_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4013.40-8_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4013.40-8_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4013.40-8_amd64.deb)|

## <a name="cu1-january-2020"></a><a id="cu1"></a> CU1 (enero de 2020)

Esta es la versión de la actualización acumulativa 1 (CU1) de SQL Server 2019 (15.X). La versión de Motor de base de datos de SQL Server de esta versión es la 15.0.4003.23. Para obtener información sobre las correcciones y mejoras de esta versión, vea <https://support.microsoft.com/en-us/help/4527376>.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

> [!NOTE]
> A partir de CU1, los vínculos de instalación de paquetes sin conexión para Red Hat señalan a los paquetes RHEL 8. Si busca paquetes RHEL 7, consulte la ruta de acceso de descarga <https://packages.microsoft.com/rhel/7/mssql-server-2019/>.

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.4003.23-3 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/8/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.4003.23-3 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.4003.23-3.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.4003.23-3.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.4003.23-3.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.4003.23-3.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.4003.23-3.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.4003.23-3.x86_64.rpm)|
| Paquete de Debian para Ubuntu 16.04 | 15.0.4003.23-3 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.4003.23-3_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.4003.23-3_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.4003.23-3_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.4003.23-3_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.4003.23-3_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.4003.23-3_amd64.deb)|

## <a name="ga-november-2019"></a><a id="ga"></a> Disponibilidad general (noviembre de 2019)

Esta es la versión de disponibilidad general de SQL Server 2019 (15.x). La versión de Motor de base de datos de SQL Server de esta versión es la 15.0.2000.5.

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.2000.5-5 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/7/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.2000.5-5 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-15.0.2000.5-5.x86_64.rpm)</br>[Paquete RPM de alta disponibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-ha-15.0.2000.5-5.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-fts-15.0.2000.5-5.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-15.0.2000.5-5.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-extensibility-java-15.0.2000.5-5.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-2019/mssql-server-polybase-15.0.2000.5-5.x86_64.rpm)|
| Paquete de Debian para Ubuntu 16.04 | 15.0.2000.5-5 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server/mssql-server_15.0.2000.5-5_amd64.deb)</br>[Paquete de Debian de alta disponibilidad](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-ha/mssql-server-ha_15.0.2000.5-5_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.2000.5-5_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.2000.5-5_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.2000.5-5_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-2019/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.2000.5-5_amd64.deb)|

## <a name="release-candidate-august-2019"></a><a id="rc"></a> Versión candidata para lanzamiento (agosto de 2019)

En las secciones siguientes se proporcionan las ubicaciones de los paquetes y problemas conocidos de la versión candidata para lanzamiento. Para obtener más información sobre las nuevas características de Linux en SQL Server 2019, consulte [Novedades de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="package-details"></a>Detalles del paquete

En el caso de las instalaciones de paquetes manuales o sin conexión, puede descargar los paquetes RPM y Debian con la información de la tabla siguiente:

| Paquete | Versión del paquete | Descargas |
|-----|-----|-----|
| Paquete RPM de Red Hat | 15.0.1900.25-1 | [Paquete RPM del motor](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/rhel/7/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Paquete RPM de SLES | 15.0.1900.25-1 | [Paquete RPM del motor mssql-server](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-15.0.1900.25-1.x86_64.rpm)</br>[Paquete RPM de búsqueda de texto completo](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-fts-15.0.1900.25-1.x86_64.rpm)</br>[Paquete de RPM de extensibilidad](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-15.0.1900.25-1.x86_64.rpm)</br>[Paquete de RPM de extensibilidad de Java](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-extensibility-java-15.0.1900.25-1.x86_64.rpm)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/sles/12/mssql-server-preview/mssql-server-polybase-15.0.1900.25-1.x86_64.rpm)|
| Paquete de Debian para Ubuntu 16.04 | 15.0.1900.25-1 | [Paquete de Debian del motor](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server/mssql-server_15.0.1900.25-1_amd64.deb)</br>[Paquete de Debian de la búsqueda de texto completo](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-fts/mssql-server-fts_15.0.1900.25-1_amd64.deb)</br>[Paquete de Debian de extensibilidad](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility/mssql-server-extensibility_15.0.1900.25-1_amd64.deb)</br>[Paquete de Debian de extensibilidad de Java](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-extensibility-java/mssql-server-extensibility-java_15.0.1900.25-1_amd64.deb)</br>[Paquete de RPM de PolyBase](https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview/pool/main/m/mssql-server-polybase/mssql-server-polybase_15.0.1900.25-1_amd64.deb)|

## <a name="known-issues"></a>Problemas conocidos

En las secciones siguientes, se describen problemas conocidos con la versión de disponibilidad general de SQL Server 2019 (15.x) en Linux.

### <a name="general"></a>General

- La longitud del nombre de host donde se instale [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ha de tener 15 caracteres o menos. 

    - **Solución:** cambie el nombre en /etc/hostname para que tenga 15 caracteres de longitud o menos.

- Al retrasar de forma manual la hora del sistema, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] dejará de actualizar la hora interna del sistema en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

    - **Solución:** Reinicie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

- Solo se admiten las instalaciones de instancia única.

    - **Solución:** si tiene más de una instancia en un host específico, puede usar máquinas virtuales o contenedores de Docker. 

- [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Configuration Manager no puede conectarse a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Linux.

- El idioma predeterminado del inicio de sesión de **sa** es el inglés.

    - **Solución:** Cambie el idioma del inicio de sesión de **sa** con la instrucción **ALTER LOGIN**.

- El proveedor OLEDB registra la advertencia `Failed to verify the Authenticode signature of 'C:\binn\msoledbsql.dll'. Signature verification of SQL Server DLLs will be skipped. Genuine copies of SQL Server are signed. Failure to verify the Authenticode signature might indicate that this is not an authentic release of SQL Server. Install a genuine copy of SQL Server or contact customer support.`.

   - **Solución:** No se requiere ninguna acción. El proveedor OLEDB se firma con SHA256. El motor de base de datos de SQL Server no valida correctamente el archivo .dll firmado.

### <a name="databases"></a>Bases de datos

- La base de datos maestra no se puede mover con la utilidad mssql-conf. Otras bases de datos del sistema se pueden mover con la utilidad mssql-conf.

- Al restaurar una base de datos cuya copia de seguridad se ha realizado en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Windows, tendrá que usar la cláusula **WITH MOVE** en la instrucción Transact-SQL.

- Algunos algoritmos (conjuntos de cifrado) para TLS (Seguridad de la capa de transporte) no funcionan correctamente con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Linux. Esto causa errores de conexión al intentar conectarse a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], así como problemas al establecer conexiones entre réplicas en grupos de alta disponibilidad.

   - **Solución:** modifique el script de configuración **mssql.conf** para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Linux para deshabilitar los conjuntos de cifrado problemáticos; para hacerlo, siga este procedimiento:

      1. Agregue el texto siguiente a /var/opt/mssql/mssql.conf.

          ```
          [network]
          tlsciphers= AES256-GCM-SHA384:AES128-GCM-SHA256:AES256-SHA256:AES128-SHA256:AES256-SHA:AES128-SHA:!ECDHE-RSA-AES128-GCM-SHA256:!ECDHE-RSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES256-GCM-SHA384:!ECDHE-ECDSA-AES128-GCM-SHA256:!ECDHE-ECDSA-AES256-SHA384:!ECDHE-ECDSA-AES128-SHA256:!ECDHE-ECDSA-AES256-SHA:!ECDHE-ECDSA-AES128-SHA:!ECDHE-RSA-AES256-SHA384:!ECDHE-RSA-AES128-SHA256:!ECDHE-RSA-AES256-SHA:!ECDHE-RSA-AES128-SHA:!DHE-RSA-AES256-GCM-SHA384:!DHE-RSA-AES128-GCM-SHA256:!DHE-RSA-AES256-SHA:!DHE-RSA-AES128-SHA:!DHE-DSS-AES256-SHA256:!DHE-DSS-AES128-SHA256:!DHE-DSS-AES256-SHA:!DHE-DSS-AES128-SHA:!DHE-DSS-DES-CBC3-SHA:!NULL-SHA256:!NULL-SHA
          ```

         > [!NOTE]
         > En el código anterior, `!` niega la expresión. Esto indica a OpenSSL que no use el siguiente conjunto de cifrado.  

      1. Reinicie [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] con el comando siguiente.

          ```bash
          sudo systemctl restart mssql-server
          ```

- Las bases de datos de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] en Windows que usan OLTP en memoria no se pueden restaurar en SQL Server 2019 (15.x) en Linux. Para restaurar una base de datos de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] que use OLTP en memoria, primero actualice las bases de datos a [!INCLUDE[sssql15-md](../includes/sssql16-md.md)], SQL Server 2017 o SQL Server 2019 en Windows antes de moverlas a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Linux mediante las funciones de copia de seguridad y restauración, o asociar/desasociar.

- El permiso de usuario **ADMINISTER BULK OPERATIONS** no se admite en Linux en este momento.

### <a name="networking"></a>Redes

Las características relacionadas con conexiones TCP salientes del proceso sqlservr, como servidores vinculados o grupos de disponibilidad, puede que no funcionen si se cumplen las condiciones siguientes:

1. El servidor de destino se especifica como un nombre de host, y no como una dirección IP.

1. La instancia de origen tiene deshabilitado IPv6 en el kernel. Para verificar si el sistema tiene IPv6 habilitado en el kernel, todas las pruebas siguientes necesitan completarse sin errores:

   - `cat /proc/cmdline` imprimirá la línea de comandos de arranque del kernel actual. El resultado no tiene que contener `ipv6.disable=1`.
   - El directorio /proc/sys/net/ipv6/ tiene que existir.
   - Un programa de C que realice una llamada a `socket(AF_INET6, SOCK_STREAM, IPPROTO_IP)` tiene que ejecutarse sin errores (la llamada del sistema tiene que devolver “fd != -1” y no producir errores con EAFNOSUPPORT).

El error exacto depende de la característica. En el caso de servidores vinculados, esto se manifiesta como un error de tiempo de espera de inicio de sesión. Para los grupos de disponibilidad, el DDL `ALTER AVAILABILITY GROUP JOIN` en el servidor secundario producirá un error de tiempo de espera de configuración de descarga después de 5 minutos.

Para solucionar este problema, siga uno de estos procedimientos:

1. Use direcciones IP en lugar de nombres de host para especificar el destino de la conexión TCP.

1. Habilite IPv6 en el kernel; para hacerlo, quite `ipv6.disable=1` de la línea de comandos de arranque. Los pasos exactos dependen de la distribución de Linux y el cargador de arranque que use (por ejemplo, grub). Si no quiere deshabilitar IPv6, puede deshabilitarlo si establece `net.ipv6.conf.all.disable_ipv6 = 1` en la configuración de `sysctl` (por ejemplo, `/etc/sysctl.conf`). Esto impedirá que el adaptador de red del sistema obtenga una dirección IPv6, pero permitirá que funcionen las características de sqlservr.

#### <a name="network-file-system-nfs"></a>Network File System (NFS)
Si usa recursos compartidos remotos de **NFS (Network File System)** en producción, tenga en cuenta los siguientes requisitos de compatibilidad:

- Use la versión de NFS **4.2 o posteriores**. Las versiones anteriores de NFS no admiten las características necesarias (como `fallocate` y la creación de archivos dispersos) que son comunes en los sistemas de archivos modernos.
- Busque solo los directorios **/var/opt/mssql** en el montaje NFS. Otros archivos, como los archivos binarios del sistema de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], no se admiten.
- Asegúrese de que los clientes de NFS usen la opción `nolock` al montar el recurso compartido remoto.

### <a name="localization"></a>Localización

- Si usa una configuración regional distinta del inglés (en_us) durante la configuración, tendrá que usar la codificación UTF-8 en el terminal o en la sesión de Bash. Si usa la codificación ASCII, puede que se muestre un error parecido al siguiente:

   ```
   UnicodeEncodeError: 'ascii' codec can't encode character u'\xf1' in position 8: ordinal not in range(128)
   ```

   Si no puede usar la codificación UTF-8, ejecute el programa de instalación con la variable de entorno MSSQL_LCID para especificar la opción de idioma.

   ```bash
   sudo MSSQL_LCID=<LcidValue> /opt/mssql/bin/mssql-conf setup
   ```

- Al ejecutar el programa de instalación mssql-conf y realizar una instalación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en un idioma distinto del inglés, se muestran caracteres extendidos incorrectos después del texto localizado, “Configurando SQL Server…”. O bien, para instalaciones basadas en idiomas no latinos, puede que la frase no se muestre en absoluto. La frase que falta tendría que mostrar la siguiente cadena localizada: “El PID de licencias de proceso correctamente. La edición nueva es la [edición \<Name\>]". La cadena se muestra solo con fines informativos, y en la próxima actualización acumulativa de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se solucionará este problema para todos los idiomas. Esto no afecta a la instalación correcta de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de ningún modo. 

#### <a name="full-text-search"></a>Búsqueda de texto completo

- No todos los filtros están disponibles en esta versión, incluidos los filtros para documentos de Office. Para obtener una lista de los filtros admitidos, vea [Instalar la búsqueda de texto completo de SQL Server en Linux](sql-server-linux-setup-full-text-search.md#filters).

### <a name="sql-server-integration-services-ssis"></a><a id="ssis"></a> SQL Server Integration Services (SSIS)

- El paquete **mssql-server-is** no se admite en SUSE en esta versión. Se admite actualmente en Ubuntu y en Red Hat Enterprise Linux (RHEL).

- Con [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] en Linux CTP 2.1 Refresh y posteriores, los paquetes de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pueden usar conexiones ODBC en Linux. Esta función se ha probado con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y los controladores ODBC de MySQL, pero también se espera que funcione con cualquier controlador ODBC de Unicode que respete la especificación de ODBC. En tiempo de diseño, puede proporcionar un DSN o una cadena de conexión para conectarse a los datos ODBC; también puede usar la autenticación de Windows. Para obtener más información, vea [la entrada de blog que anuncia la compatibilidad con ODBC en Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

- Las características siguientes no se admiten en esta versión al ejecutar paquetes de SSIS en Linux:
  - [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] Base de datos del catálogo
  - Ejecución de paquetes programada por el Agente SQL
  - Autenticación de Windows
  - Componentes de terceros
  - Captura de datos modificados (CDC)
  - [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] Escalar horizontalmente
  - Azure Feature Pack para SSIS
  - Compatibilidad con Hadoop y HDFS
  - Microsoft Connector for SAP BW

Para obtener una lista de los componentes de SSIS integrados que no se admiten actualmente, o que se admiten con determinadas limitaciones, vea [Limitaciones y problemas conocidos de SSIS en Linux](sql-server-linux-ssis-known-issues.md#components).

Para obtener más información sobre SSIS en Linux, vea los artículos siguientes:
-   [Entrada de blog donde se anuncia la compatibilidad de SSIS para Linux](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/).
-   [Instalar SQL Server Integration Services (SSIS) en Linux](sql-server-linux-setup-ssis.md)
-   [Extracción, transformación y carga de datos en Linux con SSIS](sql-server-linux-migrate-ssis.md)

### <a name="sql-server-management-studio-ssms"></a><a id="ssms"></a> SQL Server Management Studio (SSMS)

Las siguientes limitaciones solo se aplican a [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] en Windows conectado a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en Linux.

- No se admiten los planes de mantenimiento.

- El almacén de administración de datos (MDW) y el recopilador de datos no se admiten en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. 

- Los componentes de la interfaz de usuario de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] que tienen opciones de autenticación de Windows o de registro de eventos de Windows no funcionan con Linux. Puede usar estas características con otras opciones, como inicios de sesión de SQL. 

- El número de archivos de registro que se conservarán no se puede modificar.

## <a name="next-steps"></a>Pasos siguientes

Para empezar, vea las siguientes guías de inicio rápido:

- [Instalación en Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Instalación en SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Instalación en Ubuntu](quickstart-install-connect-ubuntu.md)
- [Ejecución en Docker](quickstart-install-connect-ubuntu.md)
- [Aprovisionar una máquina virtual de SQL en Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)
- [Ejecución y conexión: nube](quickstart-install-connect-clouds.md)

Para obtener respuesta a las preguntas más frecuentes, vea [Preguntas más frecuentes sobre SQL Server en Linux](sql-server-linux-faq.yml).
