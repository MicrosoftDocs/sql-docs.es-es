---
title: Preguntas más frecuentes sobre la versión preliminar de Azure Synapse Pathway
description: Preguntas más frecuentes sobre Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: tools-other
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: 8352fb6a70c54ede61d544a147f970237404c9f5
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186344"
---
# <a name="azure-synapse-pathway-preview-faq"></a>Preguntas más frecuentes sobre la versión preliminar de Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

En esta guía encontrará las preguntas más frecuentes sobre la versión preliminar de Azure Synapse Pathway.

## <a name="general"></a>General

### <a name="q-what-is-azure-synapse-pathway"></a>Q. ¿Qué es Azure Synapse Pathway?

A. Azure Synapse Pathway es una herramienta de traducción de código que le ayuda a realizar la actualización a una plataforma moderna de almacenamiento de datos. Para ello, emplea la automatización de la traducción de código del almacenamiento de datos existente, de modo que sea compatible con T-SQL basado en Azure Synapse SQL.

### <a name="q-how-can-i-download-azure-synapse-pathway"></a>Q. ¿Cómo se puede descargar Azure Synapse Pathway?

A. Synapse Pathway está disponible para su descarga en el [Centro de descarga de Microsoft](https://aka.ms/synapse-pathway-download).

### <a name="q-how-much-does-azure-synapse-pathway-cost"></a>Q. ¿Cuánto cuesta Azure Synapse Pathway?

A. No hay ningún costo asociado a la descarga y la ejecución de las traducciones de código mediante Synapse Pathway.

### <a name="q-what-sourcetarget-pairs-does-azure-synapse-pathway-currently-support"></a>Q. ¿Qué pares de origen-destino admite actualmente Azure Synapse Pathway?

A. En esta versión preliminar de Synapse Pathway, se incluyen como orígenes los almacenamientos de datos siguientes:
- Microsoft SQL Server
- Snowflake
- Netezza

### <a name="q-what-is-included-as-part-of-the-code-conversion"></a>Q. ¿Qué se incluye como parte de la conversión de código?

A. Synapse Pathway admite la traducción de código de tablas, esquemas, vistas y procedimientos almacenados.

### <a name="q-can-it-also-scan-my-environment-and-provide-an-assessment-report-of-all-the-objects-that-need-to-be-convertedtranslated"></a>Q. ¿Puede examinar también mi entorno y proporcionar un informe de valoración de todos los objetos que se deben convertir o traducir?

A. En esta versión preliminar de Synapse Pathway, tendrá que proporcionar el vínculo a los scripts DDL o DML que deben traducirse. Synapse Pathway no examinará el entorno actual para identificar qué objetos hay que traducir.

### <a name="q-what-information-does-azure-synapse-pathway-capture-about-my-current-data-warehouse-instance"></a>Q. ¿Qué información captura Azure Synapse Pathway sobre mi instancia actual de almacenamiento de datos?

A. Dado que puede ejecutar Synapse Pathway en su entorno local, no se captura ningún dato, excepto su nombre y organización. Esta información es necesaria cuando se descarga el archivo MSI en el Centro de descarga de Microsoft.

### <a name="q-where-can-i-raise-issues-encountered-in-azure-synapse-pathway"></a>Q. ¿Dónde puedo notificar los problemas que surgen en relación con Azure Synapse Pathway?

A. Azure Synapse Pathway se encuentra actualmente en **versión preliminar**.   Puede recibir soporte técnico para Synapse Pathway a través del canal de soporte técnico de Microsoft. Para notificar una incidencia, diríjase a Azure Portal o a los portales estándar (generalmente de soporte técnico local).


> [!NOTE] 
> Al igual que en el caso de cualquier otro servicio de Azure, todos los servicios en versión preliminar pueden recibir soporte técnico, pero sin ningún contrato de nivel de servicio.

<!-- ### Troubleshooting and optimization

#### Q. Why do I see slow performance while running the code conversion?

#### Q. Translation of errors or unexpected results? -->

## <a name="next-steps"></a>Pasos siguientes

[Descarga de Azure Synapse Pathway](synapse-pathway-download.md)