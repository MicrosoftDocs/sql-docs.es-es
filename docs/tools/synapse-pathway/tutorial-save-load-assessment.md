---
title: Guardado y carga de valoraciones con la versión preliminar de Azure Synapse Pathway
description: Guarde y cargue valoraciones del almacenamiento de datos con la versión preliminar de Azure Synapse Pathway.
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: fbaa518e2f2a5485f85fc7812291beb88896ac6e
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186264"
---
# <a name="save-and-load-assessments-with-azure-synapse-pathway-preview"></a>Guardado y carga de valoraciones con la versión preliminar de Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Las siguientes instrucciones detalladas indican cómo guardar y cargar una valoración del almacenamiento de datos desde un archivo mediante Azure Synapse Pathway.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Guardar una valoración en un archivo
> * Cargar la valoración a partir de un archivo

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, asegúrese de que ha instalado [Azure Synapse Pathway](synapse-pathway-download.md). Consulte [Información general de Azure Synapse Pathway](azure-synapse-pathway-overview.md) para obtener más información sobre la herramienta.

## <a name="saving-an-assessment-to-a-file"></a>Guardado de una valoración en un archivo

1. Una vez que haya ejecutado la traducción, debería ver el informe con el resumen de la traducción de código. ![Información general del informe de valoración de Azure Synapse Pathway.](./media/tutorial-save-load-assessment/report-overview.png)
1. Seleccione el botón **Guardar valoración**, especifique el nombre del archivo y seleccione **Guardar**.
![Valoración de Azure Synapse Pathway](./media/tutorial-save-load-assessment/save-assessment.png)

1. Se crea un archivo .asmprj en el destino especificado.

## <a name="loading-an-assessment-from-a-file"></a>Carga de una valoración a partir de un archivo

1. Para abrir la misma valoración, seleccione **Cargar valoración** e indique el nombre del archivo .asmprj. ![Exploración de Azure Synapse Pathway en ubicación de la base de datos.](./media/tutorial-save-load-assessment/browse-location.png)

1. Las carpetas de origen, entrada y salida se rellenarán en función de la valoración seleccionada.
![Configuración de valoración de Azure Synapse Pathway que muestra el tipo de traducción, el directorio de entrada y el directorio de salida.](./media/tutorial-save-load-assessment/load-assessment.png)
1. Seleccione **Traducir** para volver a ejecutar la traducción de código.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Generación de informes](report-generation.md)
