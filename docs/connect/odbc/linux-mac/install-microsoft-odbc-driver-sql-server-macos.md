---
title: Instalación de Microsoft ODBC Driver for SQL Server (macOS)
description: Aprenda a instalar el controlador Microsoft ODBC Driver for SQL Server en clientes de macOS para habilitar la conectividad de base de datos.
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2f07e613a6bd5ce8416c4b2c015749a1e631ea3f
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525149"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-macos"></a>Instalación de Microsoft ODBC Driver for SQL Server (macOS)

En este artículo se explica cómo instalar Microsoft ODBC Driver for SQL Server en macOS. También se incluyen instrucciones para las herramientas de línea de comandos opcionales para SQL Server (`bcp` y `sqlcmd`), y los encabezados de desarrollo de unixODBC.

En este artículo se proporcionan comandos para instalar el controlador ODBC desde el shell de Bash. Si quiere descargar directamente los paquetes, vea [Descarga del controlador ODBC para SQL Server](../download-odbc-driver-for-sql-server.md).

> [!Note]
> Microsoft ODBC Driver for SQL Server en macOS solo se admite en la arquitectura x64. El chip M1 de Apple no se admite.

## <a name="microsoft-odbc-17"></a>Microsoft ODBC 17

Para instalar Microsoft ODBC Driver 17 for SQL Server en macOS, ejecute los comandos siguientes:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=Y brew install msodbcsql17 mssql-tools
```

> [!IMPORTANT]
> Si ha instalado el paquete `msodbcsql` v17 que estaba disponible brevemente, debe quitarlo antes de instalar el paquete `msodbcsql17`. Esto evitará conflictos. El paquete `msodbcsql17` se puede instalar en paralelo con el paquete `msodbcsql` v13.

## <a name="previous-versions"></a>Versiones anteriores

En las secciones siguientes se proporcionan instrucciones para instalar versiones anteriores de Microsoft ODBC Driver en macOS.

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

Use los comandos siguientes para instalar Microsoft ODBC Driver 13.1 for SQL Server en OS X 10.11 (El Capitan) y macOS 10.12 (Sierra):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install msodbcsql@13.1.9.2 mssql-tools@14.0.6.0
```

## <a name="driver-files"></a>Archivos de controlador

El controlador ODBC en macOS consta de los componentes siguientes:

|Componente|Descripción|  
|---------------|-----------------|  
|libmsodbcsql.17.dylib o libmsodbcsql.13.dylib|El archivo de biblioteca dinámica (`dylib`) que contiene toda la funcionalidad del controlador. Este archivo se instala en `/usr/local/lib/`.|  
|`msodbcsqlr17.rll` o `msodbcsqlr13.rll`|El archivo de recursos asociado de la biblioteca de controladores. Este archivo se instala en `[driver .dylib directory]../share/msodbcsql17/resources/en_US/` para el controlador 17 y en `[driver .dylib directory]../share/msodbcsql/resources/en_US/` para el controlador 13. | 
|msodbcsql.h|El archivo de encabezado que contiene todas las definiciones nuevas necesarias para utilizar el controlador.<br /><br /> **Nota:**  No se puede hacer referencia a msodbcsql.h y odbcss.h en el mismo programa.<br /><br /> msodbcsql.h se instala en `/usr/local/include/msodbcsql17/` para el controlador 17 y en `/usr/local/include/msodbcsql/` para el controlador 13. |
|LICENSE.txt|El archivo de texto que contiene los términos del Contrato de licencia del usuario final. Este archivo se coloca en `/usr/local/share/doc/msodbcsql17/` para el controlador 17 y en `/usr/local/share/doc/msodbcsql/` para el controlador 13. |
|RELEASE_NOTES|El archivo de texto que contiene las notas de la versión. Este archivo se coloca en `/usr/local/share/doc/msodbcsql17/` para el controlador 17 y en `/usr/local/share/doc/msodbcsql/` para el controlador 13. |

## <a name="resource-file-loading"></a>Carga del archivo de recursos

El controlador debe cargar el archivo de recursos para poder funcionar. Este archivo se denomina `msodbcsqlr17.rll` o `msodbcsqlr13.rll` según la versión del controlador. La ubicación del archivo `.rll` es relativa a la ubicación del propio controlador (`so` o `dylib`), tal como se indica en la tabla anterior. A partir de la versión 17.1, el controlador también intentará cargar el `.rll` desde el directorio predeterminado si se produce un error en la carga de la ruta de acceso relativa. La ruta de acceso del archivo de recursos predeterminada en macOS es `/usr/local/share/msodbcsql17/resources/en_US/`

## <a name="troubleshooting"></a>Solución de problemas

Algunos usuarios experimentan un problema al intentar conectarse después de instalar el controlador ODBC y reciben un error como: `"[01000] [unixODBC][Driver Manager]Can't open lib 'ODBC Driver 17 for SQL Server' : file not found (0) (SQLDriverConnect)"`. Puede deberse a que unixODBC no está configurado correctamente para detectar los controladores registrados. En estos casos, la creación de un par de vínculos simbólicos puede resolver el problema.

```bash
sudo ln -s /usr/local/etc/odbcinst.ini /etc/odbcinst.ini
sudo ln -s /usr/local/etc/odbc.ini /etc/odbc.ini
```

En otros casos, si no puede establecer una conexión con SQL Server con el controlador ODBC, vea el artículo sobre problemas conocidos en [Solución de problemas de conexión](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Pasos siguientes

Después de instalar el controlador, puede probar la [aplicación de ejemplo ODBC de C++](../../odbc/cpp-code-example-app-connect-access-sql-db.md). Para obtener más información sobre el desarrollo de aplicaciones ODBC, vea [Desarrollo de aplicaciones](../../../odbc/reference/develop-app/developing-applications.md).

Para obtener más información, vea las [notas de la versión](release-notes-odbc-sql-server-linux-mac.md) y los [requisitos del sistema](system-requirements.md) del controlador ODBC.
