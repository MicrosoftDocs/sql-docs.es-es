---
title: Generación de informes para la versión preliminar de Azure Synapse Pathway
description: Azure Synapse Pathway proporciona informes completos sobre los scripts traducidos.
author: anshul82-ms
ms.author: anrampal
ms.topic: tutorial
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 69f7e4578585d1fde0040df4671c411f01537afd
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873188"
---
# <a name="report-generation-for-azure-synapse-pathway-preview"></a>Generación de informes para la versión preliminar de Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure Synapse Pathway proporciona un informe completo sobre el número de scripts que se han traducido correctamente. En el informe también se muestra el número de errores y advertencias en instrucciones que no se han traducido.

## <a name="generate-report"></a>Generación de un informe

Cuando seleccione **Traducir**, aparecerá un informe como el siguiente:

![Informe de Azure Synapse Pathway](./media/report-generaration/report-overview.png)

## <a name="report-summary"></a>Resumen del informe

En Conversion Success (Éxito de conversión) se muestra el porcentaje de scripts que se han traducido correctamente.

![Azure Synapse Pathway](./media/report-generaration/conversion-success.png)

En la sección Statement Distribution (Distribución de instrucciones) se detalla el número de instrucciones DDL y DML que se han traducido, incluido el número de esquemas, tablas y bases de datos que se han creado.

![Informe 1 de Azure Synapse](./media/report-generaration/statement-distribution.png)

El ahorro en la migración se calcula en función del número y la complejidad (simple, alta o media) de los objetos traducidos. Synapse Pathway calcula que se dedican 30 minutos de esfuerzo manual por cada traducción de objeto.

![Informe 2 de Azure Synapse](./media/report-generaration/migration-savings.png)

## <a name="next-steps"></a>Pasos siguientes

[Descarga de Azure Synapse Pathway](synapse-pathway-download.md)