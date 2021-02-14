---
title: mssql-cli
description: mssql-cli es una herramienta de consulta de línea de comandos interactiva para SQL Server que se ejecuta en Windows, macOS o Linux.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein, maghan
ms.custom: tools|mssql-cli
ms.date: 02/22/2018
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 1514c776944b66757a23baf06bb5169f66171f2d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033525"
---
# <a name="mssql-cli-command-line-query-tool-for-sql-server-preview"></a>Herramienta de consulta de la línea de comandos para SQL Server (versión preliminar)

[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

mssql-cli es una herramienta de línea de comandos interactiva para consultar SQL Server y se ejecuta en Windows, macOS o Linux.

## <a name="install-mssql-cli"></a>Instalación de mssql-cli

Para obtener instrucciones de instalación detalladas, vea la [Guía de instalación](https://github.com/dbcli/mssql-cli/tree/master/doc/installation). A continuación se resumen los escenarios de instalación más comunes.

### <a name="windows-and-macos-installation"></a>Instalación de Windows y macOS

En Windows y macOS, mssql-cli se instala mediante `pip`:

```$ pip install mssql-cli```

Para obtener instrucciones de instalación más detalladas, vea la [Guía de instalación](https://github.com/dbcli/mssql-cli/tree/master/doc/installation).

### <a name="linux-installation"></a>Instalación de Linux

Después de registrar el repositorio de Microsoft, mssql-cli se puede instalar y actualizar a través de administradores de paquetes en varias distribuciones de Linux.

El ejemplo siguiente se aplica a Ubuntu 18.04 (Bionic); puede encontrar más información y ejemplos para otras distribuciones en la [Guía de instalación](https://github.com/dbcli/mssql-cli/tree/master/doc/installation).

```
# Import the public repository GPG keys
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Register the Microsoft Ubuntu repository
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod

# Update the list of products
sudo apt-get update

# Install mssql-cli
sudo apt-get install mssql-cli

# Install missing dependencies
sudo apt-get install -f
```

## <a name="mssql-cli-documentation"></a>Documentación de mssql-cli

La documentación de mssql-cli se encuentra en el [repositorio mssql-cli de GitHub](https://github.com/dbcli/mssql-cli).

- [Página principal/archivo Léame](https://github.com/dbcli/mssql-cli)
- [Guía de instalación](https://github.com/dbcli/mssql-cli/tree/master/doc/installation)
- [Guía de uso](https://github.com/dbcli/mssql-cli/blob/master/doc/usage_guide.md)

La documentación adicional se encuentra en la [carpeta de documentos](https://github.com/dbcli/mssql-cli/tree/master/doc).
