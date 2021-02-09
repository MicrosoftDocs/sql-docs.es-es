---
title: Descarga e instalación de sqlpackage
description: Descarga e instalación de sqlpackage para Windows, macOS o Linux
ms.custom: tools|sos
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 10/02/2020
ms.openlocfilehash: 45aad0892dbc187e50368f7abcf6c0b02cbc7cca
ms.sourcegitcommit: e4b6357756a9c691b0441208a0058f7b8f3bea51
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99510296"
---
# <a name="download-and-install-sqlpackage"></a>Descarga e instalación de sqlpackage

sqlpackage se ejecuta en Windows, macOS y Linux.

Descargue e instale la versión más reciente de .NET Framework y las versiones preliminares de macOS y Linux:

|Plataforma|Descargar|Fecha de la versión|Versión|Build
|:---|:---|:---|:---|:---|
|[Windows](#get-sqlpackage-for-windows)|[Instalador MSI](https://go.microsoft.com/fwlink/?linkid=2143544)|18 de septiembre de 2020| 18.6 | 15.0.4897.1 |
|[macOS .NET Core](#get-sqlpackage-net-core-for-macos) |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2143659)|18 de septiembre de 2020| 18.6| 15.0.4897.1 |
|[Linux .NET Core](#get-sqlpackage-net-core-for-linux) |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2143497)|18 de septiembre de 2020| 18.6| 15.0.4897.1 |
|[Windows .NET Core](#get-sqlpackage-net-core-for-windows) |[archivo zip](https://go.microsoft.com/fwlink/?linkid=2143496)|18 de septiembre de 2020| 18.6| 15.0.4897.1 |

Para más información sobre la última versión, consulte las [notas de la versión](release-notes-sqlpackage.md). Para descargar idiomas adicionales, consulte la sección de [idiomas disponibles](#available-languages).


Hay disponible un vínculo permanente ([https://aka.ms/sqlpackage-linux](https://aka.ms/sqlpackage-linux)) que apunta a la versión actual de sqlpackage para [Linux .NET Core](#get-sqlpackage-net-core-for-linux), que se puede usar en la automatización de entornos con la utilidad sqlpackage más reciente.

## <a name="dacfx"></a>DacFx
SqlPackage es una interfaz de línea de comandos para el marco DacFx, que expone algunas de las API públicas de DacFx. DacServices ([Microsoft.SqlServer.Dac](/dotnet/api/microsoft.sqlserver.dac.dacservices)) es un mecanismo relacionado para integrar la implementación de bases de datos en la canalización de la aplicación.  La API DacServices está disponible en un paquete a través de NuGet, [Microsoft.SqlServer.DACFx](https://www.NuGet.org/packages/Microsoft.SqlServer.DACFx).  La versión actual de DacFx es 150.4897.1.

La instalación del paquete NuGet a través de la CLI de .NET se realiza con este comando:

```cmd
dotnet add package Microsoft.SqlServer.DACFx
```

>[!NOTE]
> Se han publicado paquetes NuGet adicionales con el nombre DacFx "Microsoft.SqlServer.DacFx.x64" y "Microsoft.SqlServer.DacFx.x86". La compatibilidad con ambas plataformas está incluida en el paquete "Microsoft.SqlServer.DACFx". Las nuevas referencias deben realizarse a este paquete, no a las variantes x64 o x86.

## <a name="get-sqlpackage-for-windows"></a>Obtención de sqlpackage para Windows

En esta versión de sqlpackage se incluye una experiencia de instalación estándar de Windows y un archivo .zip: 

1. Descargue y ejecute el [instalador de DacFramework.msi para Windows](https://go.microsoft.com/fwlink/?linkid=2143544).
2. Abra una nueva ventana del símbolo del sistema y ejecute sqlpackage.exe.
    - sqlpackage se instala en la carpeta ```C:\Program Files\Microsoft SQL Server\150\DAC\bin```.

## <a name="get-sqlpackage-net-core-for-windows"></a>Descarga de sqlpackage .NET Core para Windows

1. Descargue [sqlpackage para Windows](https://go.microsoft.com/fwlink/?linkid=2143496).
2. Para extraer el archivo, haga clic con el botón derecho en el archivo en el explorador de Windows, seleccione "Extraer todo..." y seleccione el directorio de destino.
3. Abra una nueva ventana de terminal y el CD en la ubicación donde se extrajo sqlpackage:

   ```cmd
   > sqlpackage
   ```

## <a name="get-sqlpackage-net-core-for-macos"></a>Descarga de sqlpackage .NET Core para macOS

1. Descargue [sqlpackage para macOS](https://go.microsoft.com/fwlink/?linkid=2143659).
2. Para extraer el archivo e iniciar sqlpackage, abra una nueva ventana de terminal y escriba los siguientes comandos:

   ```bash
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-osx-<version string>.zip -d ~/sqlpackage 
   $ echo 'export PATH="$PATH:~/sqlpackage"' >> ~/.bash_profile
   $ source ~/.bash_profile
   $ sqlpackage
   ```

   > [!NOTE]
   > La configuración de seguridad puede requerir una modificación para ejecutar sqlpackage en macOS. Use los comandos siguientes para interactuar con Gatekeeper desde la línea de comandos.

   **Antes de ejecutar sqlpackage:**
   ```bash
   $ sudo spctl --master-disable
   ```

   **Después de ejecutar sqlpackage:**
   ```bash
   $ sudo spctl --master-enable
   ```

## <a name="get-sqlpackage-net-core-for-linux"></a>Descarga de sqlpackage .NET Core para Linux

1. Descargue [sqlpackage para Linux](https://go.microsoft.com/fwlink/?linkid=2143497) mediante uno de los instaladores o el archivo tar.gz.
2. Para extraer el archivo e iniciar sqlpackage, abra una nueva ventana de terminal y escriba los siguientes comandos:

   ```bash
   $ cd ~
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-linux-<version string>.zip -d ~/sqlpackage 
   $ echo "export PATH=\"\$PATH:$HOME/sqlpackage\"" >> ~/.bashrc
   $ chmod a+x ~/sqlpackage/sqlpackage
   $ source ~/.bashrc
   $ sqlpackage
   ```

   > [!NOTE]
   > En Debian, Red Hat y Ubuntu, puede haber dependencias que faltan. Use los siguientes comandos para instalar estas dependencias según la versión de Linux:

   **Debian:**

   ```bash
   $ sudo apt-get install libunwind8
   ```

   **Redhat:**

   ```bash
   $ yum install libunwind
   $ yum install libicu
   ```

   **Ubuntu:**

   ```bash
   $ sudo apt-get install libunwind8

   # install the libicu library based on the Ubuntu version
   $ sudo apt-get install libicu52      # for 14.x
   $ sudo apt-get install libicu55      # for 16.x
   $ sudo apt-get install libicu57      # for 17.x
   $ sudo apt-get install libicu60      # for 18.x
   ```

## <a name="uninstall-sqlpackage"></a>Desinstalación de sqlpackage

Si ha instalado sqlpackage con el instalador de Windows, desinstale de la misma manera que quita cualquier aplicación de Windows.

Si ha instalado sqlpackage con un archivo .zip u otro archivo, elimine los archivos.

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

sqlpackage se ejecuta en Windows, macOS y Linux y se compila con .NET Core 3.1.  Los [requisitos del sistema operativo .NET Core 3.1](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md) se aplican a sqlpackage.

### <a name="windows-x64"></a>Windows (x64)

- Windows 10 (1607+)
- Windows 8.1
- Windows 7 SP1
- Windows Server Core
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019

### <a name="macos"></a>macOS

- macOS 10.15 "Catalina"
- macOS 10.14 "Mojave"
- macOS 10.13 "High Sierra"

### <a name="linux-x64"></a>Linux (x64)

- Red Hat Enterprise Linux 7+
- SUSE Linux Enterprise Server v12 SP2+
- Ubuntu 16.04+

## <a name="available-languages"></a>Idiomas disponibles

Esta versión de sqlpackage puede instalarse en los idiomas siguientes:

sqlpackage para Windows:  
[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2143544&clcid=0x40a)

sqlpackage .NET Core para Windows:  
[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2143496&clcid=0x40a)

sqlpackage .NET Core para macOS:  
[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2143659&clcid=0x40a)

sqlpackage .NET Core para Linux:  
[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2143497&clcid=0x40a)


## <a name="next-steps"></a>Pasos siguientes

- Mas información sobre [sqlpackage](sqlpackage.md)

[Declaración de privacidad de Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839)