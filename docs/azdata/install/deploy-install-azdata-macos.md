---
title: Instalación de la CLI de Azure Data (azdata) para macOS
titleSuffix: ''
description: Aprenda a instalar la herramienta de la CLI de Azure Data (azdata) en macOS.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3c172eea166f93d3e612145a2675e195c9dfdf76
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052766"
---
# <a name="install-azure-data-cli-azdata-on-macos"></a>Instalación de [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)] en macOS

Para la plataforma macOS, puede instalar `azdata-cli` mediante el administrador de paquetes Homebrew. El paquete de la CLI se ha probado en distintas versiones de macOS:

- 10.13 High Sierra
- 10.14 Mojave
- 10.15 Catalina

## <a name="install-with-homebrew"></a>Instalación con Homebrew

```bash
brew tap microsoft/azdata-cli-release
brew update
brew install azdata-cli
```

>[!IMPORTANT]
>La `azdata-cli` tiene una dependencia en los paquetes `python3`, `freetds`, `unixodbc`, `zeromq` de Homebrew y los instalará. Se garantiza que la `azdata-cli` será compatible con la versión más reciente de estas dependencias publicadas en Homebrew.

## <a name="verify-install"></a>Comprobación de la instalación

```bash
azdata
azdata --version
```

## <a name="update"></a>Actualizar

Debe actualizar la información del repositorio local y, a continuación, actualizar el paquete `azdata-cli`.

```bash
brew tap microsoft/azdata-cli-release
brew update
brew upgrade azdata-cli
```

## <a name="uninstall"></a>Desinstalación

Use Homebrew para desinstalar el paquete `azdata-cli`.

```bash
brew uninstall azdata-cli
```

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ver15.md)]?](../../big-data-cluster/big-data-cluster-overview.md) para obtener más información sobre los clústeres de macrodatos.

Use azdata con [Servicios de datos habilitados por Azure Arc](/azure/azure-arc/data/)
