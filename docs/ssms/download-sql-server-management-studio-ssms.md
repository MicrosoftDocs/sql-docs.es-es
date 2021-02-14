---
title: Descarga de SQL Server Management Studio (SSMS)
description: Descargue la última versión de SQL Server Management Studio (SSMS).
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
keywords:
- instalar ssms, descargar ssms, versiones de ssms más recientes
- SQL Server Management Studio
- ssms.exe
- sql man studio
- sql management studio
- instalación de sql management studio
- descargar sql management studio
- ms sql management studio
- instalar sql management studio
- ssms download
- sql server ssms
- ssms express
ms.assetid: adafeeef-4255-4924-8042-02f503d599ca
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 12/17/2020
ms.openlocfilehash: d88ccb88da94ba714904658fba034639c6ec2cd4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351693"
---
# <a name="download-sql-server-management-studio-ssms"></a>Descarga de SQL Server Management Studio (SSMS)

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

SQL Server Management Studio (SSMS) es un entorno integrado para administrar cualquier infraestructura de SQL, desde SQL Server a Azure SQL Database. SSMS proporciona herramientas para configurar, supervisar y administrar instancias de SQL Server y bases de datos. Use SSMS para implementar, supervisar y actualizar los componentes de nivel de datos que usan las aplicaciones, además de compilar consultas y scripts.

Use SSMS para consultar, diseñar y administrar bases de datos y almacenes de datos, estén donde estén, en el equipo local o en la nube.

## <a name="download-ssms"></a>Descargar SSMS

:::image type="icon" source="media/download-icon.png" border="false"::: **[Descarga de SQL Server Management Studio (SSMS)](https://aka.ms/ssmsfullsetup)**

SSMS 18.8 es la versión de disponibilidad general (GA) más reciente de SSMS. Si tiene instalada una versión anterior, esta se actualizará a 18.8 con la nueva instalación.

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

- Número de versión: 18,8
- Número de compilación: 15.0.18369.0
- Fecha de lanzamiento: 17 de diciembre de 2020

Si tiene algún comentario o sugerencia, o quiere informar de alguna incidencia, la mejor manera de contactar con el equipo de SSMS es mediante los [comentarios de los usuarios de SQL Server](https://aka.ms/sqlfeedback).

La instalación de SSMS 18.x no actualiza ni reemplaza las versiones 17.x de SSMS ni las anteriores. SSMS 18.x se instala en paralelo con las versiones anteriores, de modo que ambas están disponibles para su uso. Sin embargo, si tiene instalada una *versión preliminar* de SSMS 18.x, debe desinstalarla antes de instalar SSMS 18.8. Para ver si tiene la versión preliminar, vaya a la ventana **Ayuda > Acerca de**.

Si un equipo contiene instalaciones en paralelo de SSMS, compruebe si inicia la versión correcta para sus necesidades específicas. La versión más reciente tiene la etiqueta **Microsoft SQL Server Management Studio 18**

> [!Note]
> Si accede a esta página desde una versión de idioma que no es el inglés y quiere ver el contenido más actualizado, visite la [versión en inglés de EE. UU. del sitio](). Puede descargar distintos idiomas en el sitio de la versión en inglés de EE. UU. si selecciona [idiomas disponibles](#available-languages).

## <a name="available-languages"></a>Idiomas disponibles

Esta versión de SSMS puede instalarse en los idiomas siguientes:

SQL Server Management Studio 18.8:  
[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2151644&clcid=0x40a)

> [!NOTE]
> El módulo SQL Server PowerShell se instala de forma independiente a través de la Galería de PowerShell. Para más información, vea [Descarga del módulo de PowerShell de SQL Server](../powershell/download-sql-server-ps-module.md).

## <a name="whats-new"></a>Novedades

Para obtener más detalles e información sobre las novedades de esta versión, vea [Notas de la versión de SSMS](release-notes-ssms.md).

Hay algunos [problemas conocidos](release-notes-ssms.md#known-issues-188) con esta versión.

## <a name="previous-versions"></a>Versiones anteriores

Este artículo es exclusivo para la versión más reciente de SSMS. Para descargar versiones anteriores de SSMS, visite [Versiones anteriores de SSMS](../ssms/release-notes-ssms.md#previous-ssms-releases).

[!INCLUDE[ssms-connect-azure-ad](../includes/ssms-connect-azure-ad.md)]

## <a name="unattended-install"></a>Instalación desatendida

También puede instalar SSMS mediante un script del símbolo del sistema.

Si quiere instalar SSMS en segundo plano sin ningún símbolo de GUI, siga los pasos siguientes.

1. Inicie el símbolo del sistema con privilegios elevados.

2. En el símbolo del sistema, escriba el comando siguiente.

    ```console
    start "" /w <path where SSMS-ENU.exe file is located> /Quiet SSMSInstallRoot=<path where you want to install SSMS>
    ```

    Ejemplo:

    ```console
    start "" /w %systemdrive%\SSMSfrom\SSMS-Setup-ENU.exe /Quiet SSMSInstallRoot=%systemdrive%\SSMSto
    ```

    También puede pasar */Passive* en lugar de */Quiet* para ver la interfaz de usuario del programa de instalación.

3. Si todo va bien, podrá ver SSMS instalado en %systemdrive%\SSMSto\Common7\IDE\Ssms.exe, según el ejemplo. Si ha habido algún problema, puede inspeccionar el código de error devuelto y echar un vistazo a %TEMP%\SSMSSetup para buscar el archivo de registro.

## <a name="installation-with-azure-data-studio"></a>Instalación con Azure Data Studio

- A partir de SSMS 18.7, SSMS instala de forma predeterminada una versión del sistema de Azure Data Studio.  Si ya hay una versión del sistema de Azure Data Studio igual o superior estable, o Insiders ya se encuentra en la estación de trabajo, al contrario de lo que sucede con la versión incluida de Azure Data Studio, se omite la instalación de Azure Data Studio por SSMS. La versión de Azure Data Studio se puede encontrar en las notas de la versión.
- El instalador del sistema de Azure Data Studio necesita los mismos derechos de seguridad que el instalador de SSMS.
- La instalación de Azure Data Studio se completa con las opciones de instalación predeterminadas de Azure Data Studio. Estas opciones son para crear una carpeta del menú Inicio y agregar Azure Data Studio a la ruta de acceso. No se crea un acceso directo de escritorio y Azure Data Studio no se registra como editor predeterminado de ningún tipo de archivo.
- La localización de Azure Data Studio se logra a través de las extensiones de paquete de idioma. Para localizar Azure Data Studio, descargue el paquete de idioma correspondiente del [marketplace de extensiones](../azure-data-studio/extensions/add-extensions.md).
- En este momento, la instalación de Azure Data Studio se puede omitir iniciando el instalador de SSMS con la marca de la línea de comandos `DoNotInstallAzureDataStudio=1`.

## <a name="uninstall"></a>Desinstalación

Hay componentes compartidos que permanecen instalados después de desinstalar SSMS.

Dichos componentes compartidos que permanecen instalados son los siguientes:

- Azure Data Studio
- Microsoft .NET Framework 4.7.2
- Controlador Microsoft OLE DB para SQL Server
- Microsoft ODBC Driver 17 for SQL Server
- Microsoft Visual C++ 2013 Redistributable (x86)
- Microsoft Visual C++ 2017 Redistributable (x86)
- Microsoft Visual C++ 2017 Redistributable (x64)
- Microsoft Visual Studio Tools for Applications 2017

Estos componentes no se desinstalan porque se pueden compartir con otros productos. Si los desinstala, puede correr el riesgo de deshabilitar otros productos.

## <a name="supported-sql-offerings"></a>Ofertas de SQL admitidas

- Esta versión de SSMS funciona con todas las [versiones compatibles de SQL Server 2008 - [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](https://support.microsoft.com/lifecycle?C2=1044) y proporciona el mayor nivel de compatibilidad para trabajar con las últimas características de nube en Azure SQL Database y en Azure Synapse Analytics.
- Además, SSMS 18.x puede instalarse en paralelo con SSMS 17.x, SSMS 16.x o SQL Server 2014 SSMS y versiones anteriores.
- SQL Server Integration Services (SSIS): SSMS versión 17.x o versiones posteriores no admite la conexión con el servicio heredado de SQL Server Integration Services. Para conectarse a una versión anterior del servicio heredado de Integration Services, use la versión de SSMS alineada con la versión de SQL Server. Por ejemplo, use SSMS 16.x para conectar con el servicio heredado de SQL Server 2016 Integration Services. SSMS 17.x y SSMS 16.x pueden instalarse en paralelo en el mismo equipo. Desde el lanzamiento de SQL Server 2012, la base de datos del catálogo de SSIS (SSISDB) es la manera recomendada de almacenar, administrar, ejecutar y supervisar los paquetes de Integration Services. Para más información, vea [Catálogo de SSIS](../integration-services/catalog/ssis-catalog.md).

## <a name="ssms-system-requirements"></a>Requisitos del sistema de SSMS

La versión actual de SSMS admite las siguientes plataformas de 64 bits cuando se usa con el Service Pack más reciente disponible:

Sistemas operativos admitidos:

- Windows 10 (64 bits), versión 1607 (10.0.14393) o posterior
- Windows 8.1 (64 bits)
- Windows Server 2019 (64 bits)
- Windows Server 2016 (64 bits)
- Windows Server 2012 R2 (64 bits)
- Windows Server 2012 (64 bits)
- Windows Server 2008 R2 (64 bits)

Hardware admitido:

- procesador x86 de 1,8 GHz o más rápido (Intel o AMD). Doble núcleo o superior recomendado.
- 2 GB de RAM; 4 GB de RAM recomendado (mínimo de 2,5 GB si se ejecuta en una máquina virtual)
- Espacio en disco duro: Mínimo de 2 GB de espacio disponible, con un máximo de 10 GB

> [!NOTE]
> SSMS solo está disponible como una aplicación de 32 bits para Windows. Si necesita una herramienta que se ejecute en sistemas operativos que no sean Windows, le recomendamos Azure Data Studio. Azure Data Studio es una herramienta multiplataforma que se ejecuta en macOS, Linux y Windows. Para más detalles, vea [What is Azure Data Studio?](../azure-data-studio/what-is-azure-data-studio.md) ¿Qué es Azure Data Studio?.

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Herramientas SQL](../tools/overview-sql-tools.md)
- [Documentación de SQL Server Management Studio](sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md)
- [Descargar las últimas herramientas de datos SQL Server](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Actualizaciones más recientes](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)
- [Guía de arquitectura de datos de Azure](/azure/architecture/data-guide/)
- [Blog de SQL Server](https://cloudblogs.microsoft.com/sqlserver/)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]