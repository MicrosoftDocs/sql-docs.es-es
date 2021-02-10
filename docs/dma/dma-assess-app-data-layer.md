---
title: Evalúe el nivel de acceso a datos de una aplicación con Data Migration Assistant
description: Aprenda a usar Data Migration Assistant para evaluar el nivel de acceso a datos de una aplicación.
ms.date: 01/23/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: f56976c9140cb4ac18c048d7d57d6949fc4ae74d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065164"
---
# <a name="assess-an-apps-data-access-layer-with-data-migration-assistant"></a>Evalúe el nivel de acceso a datos de una aplicación con Data Migration Assistant

Normalmente, las aplicaciones se conectan y conservan los datos en una base de datos. La capa de acceso a datos de la aplicación proporciona un acceso simplificado a estos datos. Data Migration Assistant (DMA) ha habilitado a los usuarios para evaluar sus bases de datos y objetos relacionados. La versión más reciente de DMA (v 5.0) incorpora compatibilidad para analizar la conectividad de la base de datos y las consultas SQL incrustadas en el código de la aplicación.

Considere el siguiente segmento de código de C#:

![Ejemplo de segmento de código C#](../dma/media/dma-assess-app-data-layer/dma-sample-c-sharp-code-segment.png)

En este caso, puede ver que la aplicación está usando una consulta SQL para obtener el nombre de un empleado.

![Detalle de segmento de código C# de ejemplo](../dma/media/dma-assess-app-data-layer/dma-sample-c-sharp-code-detail.png)

Como propietario de la aplicación, es necesario poder identificar las distintas bases de datos a las que se puede conectar la aplicación y las consultas incrustadas en el nivel de acceso a datos de la aplicación. Además, es necesario identificar los cambios necesarios para modernizar la aplicación a los servicios de datos de Azure.

## <a name="assess-an-app-with-data-access-migration-toolkit"></a>Evaluación de una aplicación con el kit de herramientas de migración de acceso a datos

Para habilitar esta evaluación, recientemente incorporamos el kit de herramientas de migración de acceso a datos (DAMT), una extensión de Visual Studio Code. La versión más reciente de esta extensión (v 0,2) agrega compatibilidad con las aplicaciones .net y el dialecto T-SQL.

1. Descargue e instale VS Code desde [aquí](https://code.visualstudio.com/download).
2. Habilite la [extensión de Data Access Migration Toolkit](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit) desde el Marketplace de extensiones.

   ![Página de extensión de Data Access Migration Toolkit](../dma/media/dma-assess-app-data-layer/dma-damt-extension-page.png)

3. Abra el proyecto de aplicación en Visual Studio Code.

   ![Proyecto de aplicación en Visual Studio Code](../dma/media/dma-assess-app-data-layer/dma-app-project-in-vscode.png)

4. Inicie la consola de extensión (Ctrl-Shft-P) y, a continuación, ejecute el comando **Data Access: Analyze del área de trabajo** .

   ![Consola de extensión en Visual Studio Code](../dma/media/dma-assess-app-data-layer/dma-vscode-extension-console.png)

5. Seleccione el dialecto SQL Server.

   ![Selección de dialecto SQL Server](../dma/media/dma-assess-app-data-layer/dma-sql-server-dialect.png)

   Al final del análisis, el comando genera un informe de comandos y consultas de conectividad de SQL.

   ![Informe de acceso a datos](../dma/media/dma-assess-app-data-layer/dma-data-access-report.png)

6. Revise el informe de los componentes de conectividad de datos y las consultas SQL incrustadas en el código de la aplicación, que aparecen resaltadas.

   ![Consultas SQL en el código de aplicación](../dma/media/dma-assess-app-data-layer/dma-sql-queries-in-app-code.png)

   Estas consultas se pueden analizar a través de DMA para los problemas de compatibilidad y de paridad de características basados en la plataforma SQL de destino.

7. Para evaluar la capa de datos de la aplicación, exporte el informe como archivo JSON.

   ![exportación del archivo. JSON](../dma/media/dma-assess-app-data-layer/dma-json-file-export.png)

   En este caso, el archivo generado es:

   ![Ver el contenido del archivo JSON](../dma/media/dma-assess-app-data-layer/dma-json-file-contents.png)

   Data Migration Assistant permite evaluar las consultas identificadas en la aplicación en el contexto de modernización de la base de datos a la plataforma de datos de Azure.

8. Inicie Data Migration Assistant y, a continuación, cree un proyecto de evaluación.

   ![Data Migration Assistant nuevo proyecto de evaluación](../dma/media/dma-assess-app-data-layer/dma-new-assessment-project.png)

9. Seleccione la instancia de SQL Server de origen.

   ![Data Migration Assistant seleccionar origen de SQL Server](../dma/media/dma-assess-app-data-layer/dma-select-sql-source.png)

10. Seleccione la base de datos a la que se conecta la aplicación.

    ![Seleccionar base de datos de aplicación de migración de acceso a datos](../dma/media/dma-assess-app-data-layer/dma-select-app-database.png)

    Para facilitar la evaluación del acceso a los datos, DMA presenta la posibilidad de incluir archivos JSON con consultas de la aplicación. A continuación, incluiremos el archivo JSON creado anteriormente con las consultas de la aplicación.

11. Seleccione la base de datos y vaya al archivo JSON exportado desde el kit de herramientas de migración de acceso a datos para incluir las consultas de la aplicación para la evaluación.

    ![Data Migration Assistant abrir el archivo JSON DMAT](../dma/media/dma-assess-app-data-layer/dma-open-damt-json-file.png)

12. Inicie la evaluación.

    ![Data Migration Assistant iniciar la evaluación](../dma/media/dma-assess-app-data-layer/dma-start-assessment.png)

13. Revise el informe de evaluación. El informe generado incluirá los problemas de compatibilidad o de paridad de características detectados en las consultas de aplicación, tal como se muestra a continuación.

    ![Informe de evaluación de Data Migration Assistant](../dma/media/dma-assess-app-data-layer/dma-assessment-report.png)

Ahora, además de tener en cuenta la perspectiva de la migración de la base de datos, los usuarios también tienen una vista desde la perspectiva de la aplicación.

## <a name="see-also"></a>Consulte también

* [Información general de Data Migration Assistant](../dma/dma-overview.md)
* [Data Migration Assistant: opciones de configuración](../dma/dma-configurationsettings.md)
* [Data Migration Assistant: procedimientos recomendados](../dma/dma-bestpractices.md)
* [Kit de herramientas de migración de acceso a datos](https://marketplace.visualstudio.com/items?itemName=ms-databasemigration.data-access-migration-toolkit)