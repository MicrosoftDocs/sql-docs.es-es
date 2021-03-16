---
title: Valoración de la versión preliminar de Azure Synapse Pathway
description: Realización de una traducción de código de almacenamiento de datos con Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 3e5e8536c135244288d022879764d021c53e67ca
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770517"
---
# <a name="tutorial-to-perform-your-first-code-translation-with-azure-synapse-pathway-preview"></a>Tutorial para realizar la primera traducción de código con la versión preliminar de Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

La versión preliminar de Azure Synapse Pathway incluye compatibilidad con la traducción de esquemas, tablas, vistas, funciones, etc., de **Netezza**, **Snowflake** y **Microsoft SQL Server** a código compatible con T-SQL que automatiza la migración a Azure Synapse Analytics.

Para obtener más información, consulte [Información general de la versión preliminar de Azure Synapse Pathway](azure-synapse-pathway-overview.md).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Ejecutar su primera traducción de scripts SQL del almacenamiento de datos existente a scripts de T-SQL para SQL de Azure Synapse 
> * Elegir uno de los orígenes disponibles
> * Ver los errores y las advertencias sobre los objetos que no se han traducido

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, asegúrese de que tiene instalado [Azure Synapse Pathway](synapse-pathway-download.md). Si necesita una introducción, consulte [Información general sobre Azure Synapse Pathway](azure-synapse-pathway-overview.md).

## <a name="run-the-translation"></a>Ejecución de la traducción

1. Inicie el MSI de Azure Synapse Pathway. 

1. Seleccione uno de los orígenes disponibles; los que se agregarán pronto aparecerán atenuados.
1. En la carpeta del directorio de entrada, seleccione Examinar y apunte la herramienta a la ubicación de la carpeta de los scripts **DDL** y **DML** que se deben traducir.

    > [!Note]
    > Solo se pueden proporcionar como origen de entrada archivos con la extensión .sql. Si el usuario proporciona scripts DDL o DML en un archivo .txt, la herramienta no realizará ninguna traducción.

1. Al traducir código de Netezza a Azure Synapse Analytics, elija IBM Netezza en la lista desplegable Tipo de traducción.
  ![Entrada de la valoración de Azure Synapse.](./media/synapse-pathway-assessment/assessment-input.png)

1. Para indicar el directorio de salida, seleccione Examinar para especificar la ubicación donde se generará la salida.
 ![Directorio de la salida de Azure Synapse.](./media/synapse-pathway-assessment/output-directory.png)

1. Seleccione **Traducir** para iniciar la traducción.

## <a name="view-results"></a>Visualización de los resultados

1. La duración de la valoración depende del número de bases de datos agregadas y del tamaño de esquema de cada base de datos. Los resultados se mostrarán para cada base de datos en cuanto estén disponibles.
 ![Informe de la valoración de Azure Synapse.](./media/synapse-pathway-assessment/assessment-report-rendering.png)

1. Cuando seleccione Ver resultados, se le llevará al directorio de salida especificado en el paso anterior y verá los archivos de script traducidos en función de la estructura de directorios de entrada.

1. Incluye la estructura del proyecto, que puede confirmarse fácilmente en el repositorio de GitHub.
  
1. En el mismo directorio de salida se cargará un archivo de resultados que contendrá una lista de errores y advertencias.

## <a name="run-the-translation-using-command-line"></a>Ejecución de la traducción mediante la línea de comandos
1. Durante la instalación, AspCmd.exe estará disponible en C:\Archivos de programa (x86)\Azure Synapse Pathway (versión preliminar).
1. Inicie el símbolo del sistema y vaya a la ubicación del archivo. 
1. Escriba aspcmd.exe --help para obtener una lista de comandos

  ![Comandos de ayuda de la línea de comandos de la valoración de Azure Synapse.](./media/synapse-pathway-assessment/command-line-help.png)


4. Puede empezar a ejecutar las traducciones mediante la línea de comandos.

 ![Valoración de Azure Synapse mediante la línea de comandos.](./media/synapse-pathway-assessment/command-line-assessment.png)

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre cómo guardar y cargar la valoración](tutorial-save-load-assessment.md)
