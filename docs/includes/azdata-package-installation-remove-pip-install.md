---
author: MikeRayMSFT
ms.prod: sql
ms.topic: include
ms.date: 01/07/2020
ms.author: mikeray
ms.openlocfilehash: 0c15fb58bb076724402ab72f81e58ce46c8b7b1e
ms.sourcegitcommit: ae474d21db4f724523e419622ce79f611e956a22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92257428"
---
### <a name="pythonpip-installation"></a>Instalación de Python/Pip

Puede instalar [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] en Linux con yum, apt o zypper, o en MacOS con los administradores de paquetes de instalación de Homebrew. Antes de que estos administradores de paquetes estuvieran disponibles, la instalación requería Python y pip.

>[!IMPORTANT]
>Antes de continuar, debe quitar cualquier instalación de `azdata` que se haya instalado en el sistema de Python global. Los nuevos instaladores o paquetes nativos agregan `azdata` a la ruta de acceso y es imposible saber qué es lo que ocurre primero.
Si tiene un `azdata` existente instalado en Python del sistema global, quítelo antes de continuar.

Para ver la instalación actual, ejecute el comando siguiente:

```bash
$ pip list --format columns
```

Si `azdata` se instala mediante pip, devuelve el paquete y la versión. Por ejemplo:

```
 Package             Version
------------------- ----------
azdata-cli          15.0.X
azdata-cli-app      15.0.X
azdata-cli-cluster  15.0.X
azdata-cli-core     15.0.X
azdata-cli-hdfs     15.0.X
azdata-cli-notebook 15.0.X
azdata-cli-profile  15.0.X
azdata-cli-spark    15.0.X
azdata-cli-sql      15.0.X
```

En el ejemplo siguiente se quita una instalación de pip de `azdata`.

```bash
$ pip freeze | grep azdata-* | xargs pip uninstall -y
```

Después de comprobar que quitó toda instalación de `azdata` que se instaló con PIP, continúe con la instalación.