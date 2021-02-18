---
title: Instalación de la documentación de SQL Server para la vista sin conexión
description: Obtenga información sobre cómo instalar la documentación sin conexión de SQL Server 2019, 2017, 2016, 2014 y 2012. Utilice SQL Server Management Studio (SSMS) para ver el contenido sin conexión.
ms.prod: sql
ms.technology: install
ms.topic: conceptual
ms.assetid: 51f8a08c-51d0-41d8-8bc5-1cb4d42622fb
author: markingmyname
ms.author: maghan
ms.date: 08/12/2020
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: ab5ee55bf91e1a9287f22584da83ca7112dadece
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336772"
---
# <a name="install-sql-server-documentation-to-view-offline-in-ssms"></a>Instalación de la documentación de SQL Server para la vista sin conexión en SSMS

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

En este artículo se describe cómo descargar y ver el contenido de SQL Server sin conexión en [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md). El contenido sin conexión le permite acceder a la documentación sin conexión a Internet (aunque se necesita una conexión a Internet para descargarla).

La documentación sin conexión está disponible para las versiones de SQL Server 2012 y posteriores. Aunque puede ver el contenido de las [versiones anteriores en línea](/previous-versions/sql/), una opción sin conexión proporciona una manera cómoda de acceder al contenido anterior.

- [SQL Server 2016 y versiones posteriores](#sql-server-2016-and-later-offline-content)
- [SQL Server 2014](#sql-server-2014-offline-content)
- [SQL Server 2012](#sql-server-2012-offline-content)

## <a name="sql-server-2016-and-later-offline-content"></a>Contenido sin conexión de SQL Server 2016 y versiones posteriores

Con los siguientes pasos se explica cómo cargar contenido sin conexión en SQL Server 2016 y versiones posteriores.

1. En SSMS, seleccione **Agregar y quitar contenido de la Ayuda** en el menú Ayuda.

   ![Incorporación y eliminación de contenido de ayuda](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   El Visor de Ayuda se abre en la pestaña Administrar contenido.

2. Para encontrar el contenido de Ayuda más reciente de SQL Server 2016 y versiones posteriores, en la pestaña **Administrar contenido**, elija **En línea** en el origen de la instalación y, después, escriba *sql server* en la barra de búsqueda.

   ![Búsqueda de libros de SQL Server](../sql-server/media/sql-server-offline-documentation/sql-online-search.png)

   > [!Note]
   > En Ruta de acceso del almacén local de la pestaña Administrar contenido, se muestra dónde se instala el contenido en el equipo local. Para cambiar la ubicación, seleccione **Mover**, especifique otra ruta de carpeta en el campo **A** y, después, seleccione **Aceptar**. Si se produce un error en la instalación de la Ayuda después de cambiar la ruta de acceso del almacén local, cierre y vuelva a abrir el Visor de Ayuda. Compruebe que la nueva ubicación aparece en la ruta de acceso del almacén local y vuelva a intentar realizar la instalación.

3. Para instalar el contenido de Ayuda más reciente de SQL Server 2016 y versiones posteriores, seleccione **Agregar** junto a cada paquete de contenido (libro) que quiera instalar y, después, seleccione **Actualizar** en la parte inferior derecha.

   ![Opciones Agregar y Actualizar para libros en línea de SQL Server](../sql-server/media/sql-server-offline-documentation/sql-add-update.png)

   > [!NOTE]
   > Si el Visor de ayuda se bloquea al agregar contenido, cambie la línea Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" en los archivos %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings o HlpViewer_VisualStudiox_en-US.settings a una fecha en el futuro. Para obtener más información sobre este problema, vea [Visual Studio Help Viewer freezes](/visualstudio/welcome-to-visual-studio)(El Visor de Ayuda de Visual Studio se bloquea).

4. Para comprobar si el contenido de SQL Server 2016 y versiones posteriores está cargado, busque *sql server 2016* en el panel de contenido de la izquierda.

   ![Libros de SQL Server 2016 actualizados automáticamente](../sql-server/media/sql-server-offline-documentation/sql-2016-content.png)

## <a name="sql-server-2014-offline-content"></a>Contenido sin conexión de SQL Server 2014

> [!IMPORTANT]
> El contenido de Transact-SQL de SQL 2014 solo está disponible sin conexión.

Con los siguientes pasos se explica cómo cargar contenido sin conexión en SQL Server 2014.

1. Descargue el contenido de la [Documentación del producto de Microsoft SQL Server 2014 para los entornos restringidos de firewall y proxy](https://www.microsoft.com/download/details.aspx?id=42557) del centro de descarga y guárdelo en una carpeta.

2. Descomprima el archivo para ver el archivo *.msha*.

   ![Archivo de instalación de la documentación de Ayuda de SQL Server 2014](../sql-server/media/sql-server-offline-documentation/sql-2014-help-content-setup-msha.png)

3. En SSMS, seleccione **Agregar y quitar contenido de la Ayuda** en el menú Ayuda.

   ![Agregar y quitar contenido en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   El Visor de Ayuda se abre en la pestaña Administrar contenido.

4. Para instalar el último contenido de Ayuda, seleccione **Disco** en Origen de la instalación y, después, haga clic en los puntos suspensivos (...).

   ![Origen del disco de Administrar contenido en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > En Ruta de acceso del almacén local de la pestaña Administrar contenido, se muestra dónde se encuentra el contenido en el equipo local. Para cambiar la ubicación, seleccione **Mover**, especifique otra ruta de carpeta en el campo **A** y, después, seleccione **Aceptar**.
   Si se produce un error en la instalación de la Ayuda después de cambiar la ruta de acceso del almacén local, cierre y vuelva a abrir el Visor de ayuda. Compruebe que la nueva ubicación aparece en la ruta de acceso del almacén local y vuelva a intentar realizar la instalación.

5. Encuentre la carpeta donde se descomprimió el contenido. Seleccione el archivo **HelpContentSetup.msha** en la carpeta y, después, seleccione **Abrir**.

   ![Abrir el archivo Setup.msha del contenido de Ayuda de SQL Server 2014](../sql-server/media/sql-server-offline-documentation/sql-2014-open-msha.png)

6. Escriba *sql server 2014* en la barra de búsqueda. Una vez que vea el contenido disponible de 2014, seleccione **Agregar** junto a cada paquete de contenido (libro) que desee instalar en el Visor de Ayuda y, a continuación, seleccione **Actualizar**.

   ![Búsqueda de libros de SQL Server 2014 en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/sql-2014-search.png)

   ![Libros de SQL Server 2014 sobre adición y actualización en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/sql-2014-add-update.png)

    > [!NOTE]
    > Si el Visor de ayuda se bloquea al agregar contenido, cambie la línea Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" en los archivos %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings o HlpViewer_VisualStudiox_en-US.settings a una fecha en el futuro. Para obtener más información sobre este problema, vea [Visual Studio Help Viewer freezes](/visualstudio/welcome-to-visual-studio)(El Visor de Ayuda de Visual Studio se bloquea).

7. Puede comprobar si el contenido de SQL Server 2014 está instalado; para ello, busque *sql server 2014* en el panel de contenido de la izquierda.

   ![Libros de SQL Server 2014 actualizados automáticamente](../sql-server/media/sql-server-offline-documentation/sql-2014-content.png)

## <a name="sql-server-2012-offline-content"></a>Contenido sin conexión de SQL Server 2012

Con los siguientes pasos se explica cómo cargar contenido sin conexión en SQL Server 2012.

1. Descargue el contenido de la [Documentación del producto de Microsoft SQL Server 2012 para los entornos restringidos de firewall y proxy](https://www.microsoft.com/download/details.aspx?id=35750) del centro de descarga y guárdelo en una carpeta.

2. Descomprima el archivo para ver el archivo *.msha*.

   ![Archivo de instalación del contenido de Ayuda de SQL Server 2012](../sql-server/media/sql-server-offline-documentation/sql-2012-help-content-setup-msha.png)

3. En SSMS, seleccione **Agregar y quitar contenido de la Ayuda** en el menú Ayuda.

   ![Agregar y quitar contenido en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/add-remove-content.png)

   El Visor de Ayuda se abre en la pestaña Administrar contenido.

4. Para instalar el último contenido de Ayuda, seleccione **Disco** en Origen de la instalación y, después, haga clic en los puntos suspensivos (...).

   ![Origen del disco de Administrar contenido en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/install-source-disk.png)

   > [!NOTE]
   > En Ruta de acceso del almacén local de la pestaña Administrar contenido, se muestra dónde se encuentra el contenido en el equipo local. Para cambiar la ubicación, seleccione **Mover**, especifique otra ruta de carpeta en el campo **A** y, después, seleccione **Aceptar**.
   Si se produce un error en la instalación de la Ayuda después de cambiar la ruta de acceso del almacén local, cierre y vuelva a abrir el Visor de ayuda. Compruebe que la nueva ubicación aparece en la ruta de acceso del almacén local y vuelva a intentar realizar la instalación.

5. Encuentre la carpeta donde se descomprimió el contenido. Seleccione el archivo **HelpContentSetup.msha** en la carpeta y, después, seleccione **Abrir**.

   ![Abrir el archivo Setup.msha del contenido de Ayuda de SQL Server 2012](../sql-server/media/sql-server-offline-documentation/sql-2012-open-msha.png)

6. Escriba *sql server 2012* en la barra de búsqueda. Una vez que vea el contenido disponible de 2012, seleccione **Agregar** junto a cada paquete de contenido (libro) que desee instalar en el Visor de Ayuda y, a continuación, seleccione **Actualizar**.

   ![Búsqueda de libros de SQL Server 2012 en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/sql-2012-search.png)

   ![Libros de SQL Server 2012 sobre adición y actualización en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/sql-2012-add-update.png)

    > [!NOTE]
    > Si el Visor de ayuda se bloquea al agregar contenido, cambie la línea Cache LastRefreshed="\<mm/dd/yyyy> 00:00:00" en los archivos %LOCALAPPDATA%\Microsoft\HelpViewer2.x\HlpViewer_SSMSx_en-US.settings o HlpViewer_VisualStudiox_en-US.settings a una fecha en el futuro. Para obtener más información sobre este problema, vea [Visual Studio Help Viewer freezes](/visualstudio/welcome-to-visual-studio)(El Visor de Ayuda de Visual Studio se bloquea).

7. Puede comprobar si el contenido de SQL Server 2012 está cargado; para ello, busque *sql server 2012* en el panel de contenido de la izquierda.

   ![Documentación de SQL Server 2012 actualizada automáticamente](../sql-server/media/sql-server-offline-documentation/sql-2012-content.png)

## <a name="view-offline-documentation"></a>Visualización de la documentación sin conexión

Puede ver el contenido de la Ayuda de SQL Server en el menú **AYUDA** de la última versión de [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).

### <a name="view-offline-help-content-in-ssms"></a>Visualización del contenido de la Ayuda sin conexión en SSMS

Para ver la Ayuda instalada en SSMS, seleccione **Iniciar en el Visor de Ayuda** en el menú Ayuda para iniciar el Visor de Ayuda.

   ![Iniciar en el Visor de Ayuda](../sql-server/media/sql-server-offline-documentation/helpviewer-view-offline.png)  

Este se abrirá en la pestaña Administrar contenido, con la tabla de contenido de la Ayuda instalada en el panel izquierdo. Seleccione los artículos de la tabla de contenido para que se muestren en el panel de la derecha.

> [!Important]
> Si el panel de contenido no está visible, seleccione Contenido en el margen izquierdo. Seleccione el icono de marcador para mantener abierto el panel de contenido.  

   ![Visor de Ayuda con contenido](../sql-server/media/sql-server-offline-documentation/view-offline-all.png)

## <a name="life-cycle-policy"></a>Directiva del ciclo de vida

Revise el ciclo de vida de los productos de Microsoft para obtener información sobre la compatibilidad de un producto, un servicio o una tecnología específicos:

- [Directiva del ciclo de vida de Microsoft](https://support.microsoft.com/lifecycle/selectindex)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el contenido archivado y el visor de la Ayuda, vea los siguientes vínculos.

- [Documentación en línea de SQL Server](../sql-server/index.yml?view=sql-server-2016&preserve-view=true)
- [Documentación en línea de SQL Server 2014](/previous-versions/sql/2014)
- [Versiones anteriores de la documentación en línea de SQL Server](previous-versions-sql-server.md)
- [Control del sistema de versiones de la documentación de SQL](../sql-server/versioning-system-monikers-ui-sql-server.md?view=sql-server-2016&preserve-view=true)