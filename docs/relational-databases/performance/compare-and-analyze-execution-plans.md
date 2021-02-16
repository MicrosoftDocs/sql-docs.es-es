---
title: Comparación y análisis de los planes de ejecución | Microsoft Docs
description: Obtenga más información sobre cómo comparar y analizar planes de ejecución mediante el uso de SQL Server Management Studio. Los planes de ejecución muestran métodos de recuperación de datos del optimizador de consultas.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Showplan results
- execution plans [SQL Server]
- queries [SQL Server], tuning
- execution plans [SQL Server], how-to topics
- SQL Server Management Studio [SQL Server], execution plans
- tuning queries [SQL Server]
ms.assetid: bcd6f094-c613-4835-ae19-4caaadb4bb17
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 8b0eed72c447f2f5414d7a87a8b8a9ae08f0b415
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344720"
---
# <a name="compare-and-analyze-execution-plans"></a>Comparación y análisis de los planes de ejecución
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
Esta sección explica cómo comparar y analizar los planes de ejecución mediante Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Esta característica está disponible a partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v17.4.  
  
Los planes de ejecución muestran de forma gráfica los métodos de recuperación de datos que usa el optimizador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los planes de ejecución representan el costo de ejecución de las instrucciones y consultas específicas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando iconos en lugar de la representación tabular que crean las instrucciones [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md) o [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md). Este enfoque gráfico resulta muy útil para comprender las características de rendimiento de una consulta. 

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] incluye la funcionalidad que permite a los usuarios comparar dos planes de ejecución, por ejemplo entre planes que parecen buenos y otros que parecen malos en la misma consulta y realizar análisis de causa raíz. También incluye la funcionalidad para realizar análisis de plan de consulta únicos, lo que permite obtener información sobre los escenarios que pueden afectar al rendimiento de una consulta mediante análisis de su plan de ejecución.

Para obtener más información sobre los planes de ejecución de consultas, vea [Mostrar el plan de ejecución estimado](../../relational-databases/performance/display-the-estimated-execution-plan.md), [Mostrar un plan de ejecución real](../../relational-databases/performance/display-an-actual-execution-plan.md) y la [Guía de arquitectura de procesamiento de consultas](../../relational-databases/query-processing-architecture-guide.md).
  
## <a name="in-this-section"></a>En esta sección  
[Comparación de los planes de ejecución](../../relational-databases/performance/display-the-estimated-execution-plan.md)     
[Análisis de un plan de ejecución real](../../relational-databases/performance/display-an-actual-execution-plan.md)      
  
