---
title: SqlPackage en canalizaciones de desarrollo
description: Aprenda a usar SqlPackage.exe para solucionar los problemas de las canalizaciones de desarrollo de la base de datos mediante la comprobación del número de la compilación instalada.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 5bc3b1591dd05722f1b0d89a0113df4723f26d67
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061000"
---
# <a name="sqlpackage-in-development-pipelines"></a>SqlPackage en canalizaciones de desarrollo

**SqlPackage.exe** es una utilidad de línea de comandos que automatiza varias tareas de desarrollo de la base de datos y que se puede incorporar en canalizaciones de CI/CD.

## <a name="managed-virtual-environments"></a>Entornos virtuales administrados

Los entornos virtuales que se usan para los ejecutores hospedados en Acciones de GitHub y las imágenes virtuales de VM de Azure Pipelines se administran en el repositorio [virtual-environments](https://github.com/actions/virtual-environments) de GitHub.  SqlPackage está incluido en el entorno `windows-latest`. Las actualizaciones de las imágenes se producen unas semanas después del lanzamiento de cada versión de SqlPackage.

## <a name="checking-the-sqlpackage-version"></a>Comprobación de la versión de SqlPackage

Durante la solución de problemas, es importante saber cuál es la versión de SqlPackage que está en uso.  Para capturar esta información, puede agregar un paso a la canalización para ejecutar SqlPackage con el parámetro `/version`.  A continuación, presentamos ejemplos basados en los entornos administrados de Microsoft y GitHub. Los entornos autohospedados pueden tener rutas de instalación distintas para el directorio de trabajo.

### <a name="azure-pipelines"></a>Azure Pipelines

Al usar la palabra clave [script](/azure/devops/pipelines/yaml-schema#script) en una instancia de Azure Pipelines, se puede agregar un paso a la instancia de Azure Pipelines para generar el número de versión de SqlPackage.

```yaml
- script: sqlpackage.exe /version
  workingDirectory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  displayName: 'get sqlpackage version'
```

### <a name="github-actions"></a>Acciones de GitHub

Al usar la palabra clave [run](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions) en un flujo de trabajo de Acciones de GitHub, se puede agregar un paso a la instancia de Acciones de GitHub para generar el número de versión de SqlPackage.

```yaml
- name: get sqlpackage version
  working-directory: C:\Program Files\Microsoft SQL Server\150\DAC\bin\
  run: ./sqlpackage.exe /version
```

:::image type="content" source="media/sqlpackage-pipelines-github-action.png" alt-text="Salida de la instancia de Acciones de GitHub que muestra el número de compilación 15.0.4897.1":::

## <a name="next-steps"></a>Pasos siguientes

- Mas información sobre [sqlpackage](sqlpackage.md)