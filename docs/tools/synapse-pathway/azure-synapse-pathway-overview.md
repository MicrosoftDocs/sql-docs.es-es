---
title: Información general de la versión preliminar de Azure Synapse Pathway
description: Azure Synapse Pathway es una herramienta para migrar un almacenamiento de datos a Azure Synapse Analytics.
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: Azure Synapse Pathway
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: d7289d2bfe099dad7bbc91ccd5060797f7aad997
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873217"
---
# <a name="azure-synapse-pathway-preview"></a>Versión preliminar de Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Cuando los clientes deciden modernizar sus sistemas de almacenamiento de datos, uno de los principales obstáculos a los que se enfrentan es la traducción del código SQL. El código existente está escrito y optimizado para su sistema actual, pero debe optimizarse para el nuevo al que van a realizar la migración.

Las organizaciones de todo el mundo aspiran a modernizar su plataforma de análisis para disfrutar de las ventajas de la innovación y del costo total de propiedad (TCO). El problema es que los clientes han invertido miles de horas de trabajo y millones de dólares, y han escrito decenas de miles de líneas de código para su almacenamiento de datos.
 
Para traducir este código SQL crítico, los clientes tienen que reescribir manualmente el código SQL existente o invertir un gran porcentaje del presupuesto para que una empresa externa lo reescriba o lo convierta.

> [!IMPORTANT]
> Azure Synapse Pathway se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
 
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

**Azure Synapse Pathway** le ayuda a realizar la actualización a una plataforma moderna de almacenamiento de datos mediante la automatización de la traducción de código del almacenamiento de datos existente. Es una herramienta gratuita, intuitiva y fácil de usar que automatiza la traducción de código, lo que permite una migración más rápida a Azure Synapse Analytics.

 ![Información general sobre Azure Synapse Pathway](./media/pathway-overview/synapse-pathway-overview.png) 

Synapse Pathway traduce instrucciones del lenguaje de definición de datos (DDL) y del lenguaje de manipulación de datos (DML) a un lenguaje compatible con T-SQL que, a su vez, es compatible con Azure Synapse SQL.

## <a name="behind-the-scenes"></a>Entre bambalinas

Para obtener más información sobre Azure Synapse Pathway, consulte [Versión preliminar de Azure Synapse Pathway, entre bambalinas](synapse-pathway-behind-the-scenes.md), donde podrá conocer con detalle cómo funciona.

## <a name="get-azure-synapse-pathway"></a>Obtención de Azure Synapse Pathway

Si quiere instalar Synapse Pathway, en [Descarga de Azure Synapse Pathway](synapse-pathway-download.md) encontrará los requisitos previos y el vínculo para descargar la versión más reciente.

## <a name="supported-sources"></a>Orígenes compatibles

Azure Synapse Pathway admite la conversión de código de bases de datos, esquemas y tablas para los orígenes siguientes:
- **IBM Netezza** 
- **Microsoft SQL Server**
- **Snowflake**

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Revise la página de [Preguntas más frecuentes](pathway-faq.md) para obtener más información sobre Azure Synapse Pathway.

## <a name="next-steps"></a>Pasos siguientes

- [Ejecución de la primera traducción con Azure Synapse Pathway](synapse-pathway-assessment.md)
- Blog de anuncios: [Presentación de Azure Synapse Pathway: optimización de la migración del almacenamiento de datos (Microsoft Tech Community)](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/announcing-azure-synapse-pathway-turbocharge-your-data-warehouse/ba-p/2176630)


