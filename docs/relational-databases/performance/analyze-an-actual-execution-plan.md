---
title: Análisis de un plan de ejecución real | Microsoft Docs
description: Obtenga información sobre cómo analizar planes de ejecución gráfica reales, que contienen información del entorno de ejecución, mediante la característica Plan Analysis de SQL Server Management Studio.
ms.custom: ''
ms.date: 10/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- analyzing execution plans
- analyzing actual execution plans
- execution plans [SQL Server], analyzing
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 33ed1ed03b9e340e295c14481a6432fca0d3a385
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350062"
---
# <a name="analyze-an-actual-execution-plan"></a>Análisis de un plan de ejecución real

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describe cómo puede analizar planes de ejecución gráficos reales mediante la función Plan Analysis (Análisis de plan) de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Esta característica está disponible a partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v17.4. Por lo general, se recomienda que [instale la versión más reciente de SSMS](../../ssms/download-sql-server-management-studio-ssms.md).

> [!NOTE]
> Los planes de ejecución reales se generan tras ejecutar las consultas o los lotes del [!INCLUDE[tsql](../../includes/tsql-md.md)]. Por este motivo, un plan de ejecución real contiene información de tiempo de ejecución, como el número de filas real, las métricas de uso real de recursos o las advertencias en tiempo de ejecución (si las hubiera). Para obtener más información, vea [Mostrar un plan de ejecución real](../../relational-databases/performance/display-an-actual-execution-plan.md).
  
Para poder solucionar problemas de rendimiento de consulta, es necesario tener un buen conocimiento de los planes de ejecución y procesamiento de consultas, a fin de poder localizar y corregir las causas raíz.

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] incluye una funcionalidad que implementa cierto grado de automatización de la tarea de análisis de plan de ejecución real, especialmente para planes grandes y complejos. El objetivo es que sea más fácil encontrar los escenarios de [Cardinality Estimation](../../relational-databases/performance/cardinality-estimation-sql-server.md) (Estimación de cardinalidad) inexactos y obtener recomendaciones sobre las posibles mitigaciones disponibles.

> [!IMPORTANT]
> Asegúrese de que se realizan pruebas adecuadas de las mitigaciones propuestas antes de aplicarlas en entornos de producción.
  
## <a name="to-analyze-an-execution-plan-for-a-query"></a>Para analizar un plan de ejecución para una consulta  
  
1.  Abra un archivo de plan de ejecución de consulta guardado previamente (.sqlplan) desde el menú **Archivo** y haga clic en **Abrir archivo**, o arrastre un archivo de plan a la ventana [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. De forma alternativa, si acaba de ejecutar una consulta y elige mostrar su plan de ejecución, vaya a la pestaña **Plan de ejecución** en el panel de resultados. 

2.  Haga clic en un área en blanco del plan de ejecución y haga clic en **Analizar el plan de ejecución real**. 

    ![Analizar el plan de ejecución real haciendo clic con el botón derecho](../../relational-databases/performance/media/plananalysismenuoption.png "Analizar el plan de ejecución real haciendo clic con el botón derecho")   

3.  Se abre la ventana **Análisis del plan de presentación** en la parte inferior. La pestaña **Instrucción múltiple** es útil para analizar los planes con varias instrucciones, permitiendo que se analice la instrucción correcta.

4.  Seleccione la pestaña Escenarios para ver detalles sobre los problemas encontrados en el plan de ejecución real. Para cada operador enumerado en el panel izquierdo, el panel derecho muestra detalles sobre el escenario en el vínculo *Para más información sobre este escenario, haga clic aquí* y se muestran los motivos posibles para explicar ese escenario.

    ![Resultados del análisis del plan de ejecución](../../relational-databases/performance/media/plananalysis-scenarios.png "Resultados del análisis del plan de ejecución") 
