---
title: Descarga e instalación de Azure Data Studio
description: Descargue e instale Azure Data Studio para Windows, macOS o Linux. En este artículo se proporcionan fechas de lanzamiento, números de versión, requisitos del sistema y vínculos de descarga.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 1/22/2021
ms.openlocfilehash: 1832c528f302d45a4f098a6e8fe2aa6300ca10ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054440"
---
# <a name="download-and-install-azure-data-studio"></a>Descarga e instalación de Azure Data Studio

Azure Data Studio es una herramienta de base de datos multiplataforma para profesionales de datos que usan plataformas de datos locales y en la nube en Windows, macOS y Linux.

Azure Data Studio ofrece una experiencia de editor moderna con IntelliSense, fragmentos de código, integración del control de código fuente y un terminal integrado. Se ha diseñado para usuarios de plataformas de datos, con gráficos integrados de conjuntos de resultados de consultas y paneles personalizables. Para obtener más información sobre Azure Data Studio, visite [Qué es Azure Data Studio](what-is-azure-data-studio.md).

## <a name="download-the-latest-release"></a>Descargar la versión más reciente

| Plataforma | Descargar | Fecha de la versión | Versión |
|----------|----------|--------------|---------|
| Windows | [Instalador de usuario (recomendado)](https://go.microsoft.com/fwlink/?linkid=2150927)<br>[Instalador del sistema](https://go.microsoft.com/fwlink/?linkid=2150928)<br>[.zip](https://go.microsoft.com/fwlink/?linkid=2151312) | 22 de enero de 2021 | 1.25.2 |
| macOS | [.zip](https://go.microsoft.com/fwlink/?linkid=2151311) | 22 de enero de 2021 | 1.25.2 |
| Linux | [.deb](https://go.microsoft.com/fwlink/?linkid=2151506)<br>[.rpm](https://go.microsoft.com/fwlink/?linkid=2151407)<br>[.tar.gz](https://go.microsoft.com/fwlink/?linkid=2151508) | 22 de enero de 2021 | 1.25.2 |

**Para obtener información sobre la última versión, consulta las [notas de la versión](./release-notes-azure-data-studio.md).**

## <a name="get-azure-data-studio-for-windows"></a>Descargar Azure Data Studio para Windows

[!INCLUDE [ssms-ads-install](../includes/ssms-azure-data-studio-install.md)]

En esta versión de Azure Data Studio, se incluye una experiencia de Windows Installer estándar y un archivo .zip.

Le recomendamos que use el *instalador de usuario*, ya que este no requiere privilegios de administrador, lo que simplifica tanto el proceso de instalación como las actualizaciones. El instalador de usuario no requiere privilegios de administrador porque la ubicación se encuentra en la carpeta AppData local del usuario (LOCALAPPDATA). Además, el instalador de usuario también proporciona una experiencia de actualización en segundo plano más fluida. Para obtener más información, vea [Configuración del usuario para Windows](https://code.visualstudio.com/updates/v1_26#_user-setup-for-windows).

**Instalador de usuario** (recomendado)

1. Descargue y ejecute el instalador de *usuario* de [Azure Data Studio para Windows](https://go.microsoft.com/fwlink/?linkid=2150927).
2. Inicie la aplicación Azure Data Studio.

**Instalador del sistema**

1. Descargue y ejecute el instalador de *sistema* de [Azure Data Studio para Windows](https://go.microsoft.com/fwlink/?linkid=2150928).
2. Inicie la aplicación Azure Data Studio.

**archivo zip**

1. Descargue [el archivo .zip de Azure Data Studio para Windows](https://go.microsoft.com/fwlink/?linkid=2151312).
2. Busque el archivo descargado y extraiga su contenido.
3. Ejecute `\azuredatastudio-windows\azuredatastudio.exe`:

## <a name="get-azure-data-studio-for-macos"></a>Descargar Azure Data Studio para macOS

1. Descargue [Azure Data Studio para macOS](https://go.microsoft.com/fwlink/?linkid=2151311).
2. Para extraer el contenido del archivo zip, haga doble clic en este.
3. Para que Azure Data Studio esté disponible en *Launchpad*, arrastre *Azure Data Studio.app* hasta la carpeta *Aplicaciones*.

## <a name="get-azure-data-studio-for-linux"></a>Descargar Azure Data Studio para Linux

1. Descargue Azure Data Studio para Linux mediante uno de los instaladores, o bien descargue el archivo tar.gz:
    - [.deb](https://go.microsoft.com/fwlink/?linkid=2151506)
    - [.rpm](https://go.microsoft.com/fwlink/?linkid=2151407)
    - [.tar.gz](https://go.microsoft.com/fwlink/?linkid=2151508)
1. Para extraer el archivo e iniciar Azure Data Studio, abra una ventana de terminal y escriba los comandos siguientes:

   **Instalación de Debian:**

   ```bash
   cd ~
   sudo dpkg -i ./Downloads/azuredatastudio-linux-<version string>.deb

   azuredatastudio
   ```

   **Instalación de RPM:**

   ```bash
   cd ~
   yum install ./Downloads/azuredatastudio-linux-<version string>.rpm

   azuredatastudio
   ```

   **Instalación de tar.gz:**

   ```bash
   cd ~
   cp ~/Downloads/azuredatastudio-linux-<version string>.tar.gz ~ 
   tar -xvf ~/azuredatastudio-linux-<version string>.tar.gz 
   echo 'export PATH="$PATH:~/azuredatastudio-linux-x64"' >> ~/.bashrc
   source ~/.bashrc
   azuredatastudio
   ```

   > [!NOTE]
   > En Debian, Red Hat y Ubuntu, puede haber dependencias que faltan. Use los siguientes comandos para instalar estas dependencias según la versión de Linux:

   **Debian:**

   ```bash
   sudo apt-get install libunwind8
   ```

   **Redhat:**

   ```bash
   yum install libXScrnSaver
   ```

   **Ubuntu:**

   ```bash
   sudo apt-get install libxss1

   sudo apt-get install libgconf-2-4

   sudo apt-get install libunwind8
   ```

## <a name="download-insiders-build-of-azure-data-studio"></a>Descargar la compilación para los participantes del programa Insider de Azure Data Studio

En general, los usuarios tienen que descargar la versión estable de Azure Data Studio anterior. Pero, si quiere probar las características de la versión beta y enviarnos sus comentarios, puede descargar la [compilación para participantes del programa Insider de Azure Data Studio](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-main).

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

Azure Data Studio se ejecuta en Windows, macOS y Linux, y es compatible con las plataformas siguientes:

### <a name="windows"></a>Windows

- Windows 10 (64 bits)
- Windows 8.1 (64 bits)
- Windows 8 (64 bits)
- Windows 7 (SP1)
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2 (64 bits)
- Windows Server 2012 (64 bits)
- Windows Server 2008 R2 (64 bits)

### <a name="macos"></a>macOS

- macOS 10.15 Catalina
- macOS 10.14 Mojave
- macOS 10.13 High Sierra
- macOS 10.12 Sierra
- macOS 11.1 Big Sur

### <a name="linux"></a>Linux

- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.3
- SUSE Linux Enterprise Server v12 SP2
- Ubuntu 16.04

## <a name="recommended-system-requirements"></a>Requisitos del sistema recomendados

| Recomendados/mínimos | Núcleos de CPU | Memoria/RAM |
|---------------------|-----------|------------|
|     Recomendado     |     4     |   8 GB     |
|     Mínima         |     2     |   4 GB     |

## <a name="check-for-updates"></a>Buscar actualizaciones

Para buscar las actualizaciones más recientes, haga clic en el icono de engranaje de la parte inferior izquierda de la ventana y seleccione **Buscar actualizaciones**.

Las actualizaciones en entornos sin conexión se pueden aplicar mediante la [instalación de la versión más reciente](#download-and-install-azure-data-studio) directamente sobre una versión instalada previamente. No es necesario desinstalar las versiones anteriores de Azure Data Studio. En el caso de que haya una aplicación instalada, el instalador la actualizará.

## <a name="supported-sql-offerings"></a>Ofertas de SQL admitidas

- Esta versión de Azure Data Studio funciona con todas las [versiones admitidas de [!INCLUDE [sssql14-md](../includes/sssql14-md.md)] - [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](https://support.microsoft.com/lifecycle?C2=1044) y, además, proporciona compatibilidad para trabajar con las características de la nube más recientes en Azure SQL Database y Azure Synapse Analytics. Azure Data Studio también proporciona compatibilidad de vista previa con instancias administradas de Azure SQL.

## <a name="move-user-settings"></a>Migrar la configuración del usuario

Si actualiza SQL Operations Studio a Azure Data Studio y quiere mantener la configuración, los métodos abreviados de teclado o los fragmentos de código, siga los pasos que hay a continuación.

*Si ya tiene Azure Data Studio o nunca ha instalado ni personalizado SQL Operations Studio, puede omitir esta sección.*

1. Para abrir la configuración, haga clic en el icono de engranaje de la parte inferior izquierda y seleccione **Configuración**.

   ![Edición de la configuración en Azure Data Studio](./media/download/open-settings.png)

2. Haga clic con el botón derecho en la pestaña **Configuración de usuario** de la parte superior y seleccione **Mostrar en el Explorador**.

   ![Inicie el explorador para ir al sistema de archivos local.](./media/download/reveal-in-explorer.png)

3. Copie todos los archivos de esta carpeta y guárdelos en una ubicación a la que pueda acceder fácilmente desde la unidad local (por ejemplo, la carpeta Documentos).

   ![Uso y copia de los archivos en su ubicación](./media/download/copy-settings.png)

4. En la nueva versión de Azure Data Studio, siga los pasos 1-2 y, después, para el paso 3, pegue el contenido que haya guardado en la carpeta. También puede copiar de forma manual la configuración, los enlaces de teclado o los fragmentos de código en las ubicaciones correspondientes.

5. Si reemplaza una instalación existente, elimine el directorio de instalación anterior antes de la instalación para evitar errores relacionados con su cuenta de Azure en el explorador de recursos.

## <a name="unattended-install-for-windows"></a>Instalación desatendida para Windows

También puede instalar Azure Data Studio mediante un script del símbolo del sistema.

Si quiere instalar Azure Data Studio en segundo plano sin solicitudes de la interfaz gráfica de usuario y está en la plataforma Windows, siga los pasos que se indican a continuación.

1. Inicie el símbolo del sistema con privilegios elevados.

2. En el símbolo del sistema, escriba el comando siguiente.

    ```console
    <path where the azuredatastudio-windows-user-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    ```

    Ejemplo:

    ```console
    %systemdrive%\azuredatastudio-windows-user-setup-1.24.0.exe /VERYSILENT /MERGETASKS=!runcode
    ```

    > [!Note]
    > El ejemplo también funciona con el archivo instalador del sistema.
    > 
    > ```console
    > <path where the azuredatastudio-windows-setup-x.xx.x.exe file is located> /VERYSILENT /MERGETASKS=!runcode>
    > ```

    También puede pasar */SILENT* en lugar de */VERYSILENT* para ver la interfaz de usuario del programa de instalación.

3. Si todo va bien, podrá ver Azure Data Studio instalado.

## <a name="uninstall-azure-data-studio"></a>Desinstalar Azure Data Studio

Si ha instalado Azure Data Studio con Windows Installer, use el mismo método que para desinstalar cualquier aplicación de Windows.

Si ha instalado Azure Data Studio con un archivo .zip u otro archivo, elimine los archivos.

## <a name="next-steps"></a>Pasos siguientes

Para empezar, vea una de las siguientes guías de inicio rápido:

- [Qué es Azure Data Studio](what-is-azure-data-studio.md)
- [Notas de la versión de Azure Data Studio](release-notes-azure-data-studio.md)
- [Conectar y consultar SQL Server](quickstart-sql-server.md)
- [Conectar y consultar Azure SQL Database](quickstart-sql-database.md)
- [Conectar y hacer consultas en Azure Synapse Analytics](quickstart-sql-dw.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

[Declaración de privacidad de Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839) y [recopilación de datos de uso](usage-data-collection.md).
