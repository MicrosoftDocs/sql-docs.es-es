---
title: Instalación de un entorno de ejecución personalizado de R
description: Obtenga información sobre cómo instalar un entorno de ejecución personalizado de R para SQL Server mediante Extensiones de lenguaje. El entorno de ejecución personalizado de Python se puede usar para ejecutar scripts de aprendizaje automático.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 17e4a885281cf428e8a5051b4060199b2824bd90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072760"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Instalación de un entorno de ejecución personalizado de R para SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Obtenga información sobre cómo instalar un entorno de ejecución personalizado de R para ejecutar scripts de R externos con SQL Server en:

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SUSE Linux Enterprise Server (SLES)

El entorno de ejecución personalizado puede ejecutar scripts de aprendizaje automático y usa [Extensiones de lenguaje de SQL Server](../../language-extensions/language-extensions-overview.md).

Use la versión propia del entorno de ejecución de R con SQL Server, en lugar de la versión del entorno de ejecución predeterminada instalada con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

::: zone pivot="platform-windows"
[!INCLUDE [R custom runtime - Windows](includes/custom-runtime-r-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-r-linux-ubuntu.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - RHEL specific steps](includes/custom-runtime-r-linux-rhel.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - SLES specific steps](includes/custom-runtime-r-linux-sles.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

## <a name="enable-external-script"></a>Habilitación de scripts externos

Puede ejecutar un script externo de Python con el procedimiento almacenado [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Si quiere habilitar los scripts externos, use [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) para ejecutar la instrucción siguiente.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Comprobación de la instalación

Use el script de SQL siguiente para comprobar la instalación y la funcionalidad del entorno de ejecución personalizado de Python.

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="next-steps"></a>Pasos siguientes

+ [Instalación de un entorno de ejecución personalizado de Python para SQL Server](custom-runtime-python.md)
+ [Marco de extensibilidad en SQL Server](../concepts/extensibility-framework.md)
+ [Introducción a las extensiones de lenguaje](../../language-extensions/language-extensions-overview.md)