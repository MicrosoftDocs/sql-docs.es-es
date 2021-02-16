---
title: Introducción a la migración a Azure de SQL Server Integration Services | Microsoft Docs
description: En este artículo se describen el proceso y las herramientas para migrar SQL Server Integration Services a Azure.
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f443bc4209365253cf6e4e4005d19afe9ee11268
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061470"
---
# <a name="migrate-on-premises-ssis-workloads-to-ssis-in-adf"></a>Migración de cargas de trabajo de SSIS locales a SSIS en ADF

En este artículo se enumeran los procesos y las herramientas que pueden ayudarle a migrar las cargas de trabajo de SQL Server Integration Services (SSIS) a SSIS en ADF.

En [Información general sobre la migración](/azure/data-factory/scenario-ssis-migration-overview) se describe el proceso general de migración de las cargas de trabajo de ETL de SSIS local a SSIS en ADF.

El proceso de migración consta de dos fases: [Evaluación](/azure/data-factory/scenario-ssis-migration-overview#assessment) y [Migración](/azure/data-factory/scenario-ssis-migration-overview#migration).

## <a name="assessment"></a>Evaluación

Data Migration Assistant (DMA) es una herramienta que se puede descargar de forma gratuita para este fin que se puede instalar y ejecutar localmente. Se puede crear un proyecto de evaluación de DMA de tipo Integration Services para evaluar paquetes de SSIS en lotes e identificar los problemas de compatibilidad.

Obtenga [Data Migration Assistant](../../dma/dma-overview.md) y [realice una evaluación de paquetes](../../dma/dma-assess-ssis.md).

## <a name="migration"></a>Migración

En función de los tipos de almacenamiento de los paquetes de SSIS de origen y el destino de la migración de las cargas de trabajo de base de datos, los pasos para migrar los paquetes de SSIS y los trabajos del Agente SQL Server que realizan la programación de las ejecuciones de paquetes de SSIS pueden variar. Para más información, consulte [esta página](/azure/data-factory/scenario-ssis-migration-overview#migration).

## <a name="next-steps"></a>Pasos siguientes

- [Migración de paquetes de SSIS a Azure SQL Managed Instance](/azure/dms/how-to-migrate-ssis-packages-managed-instance).
- [Migración de trabajos de SSIS a Azure Data Factory (ADF) con SQL Server Management Studio (SSMS)](/azure/data-factory/how-to-migrate-ssis-job-ssms).