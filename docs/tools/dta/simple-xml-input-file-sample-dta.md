---
title: Ejemplo de archivo de entrada XML simple (DTA)
description: En este artículo se incluye un ejemplo de archivo de entrada de XML que se utiliza para optimizar las cargas de trabajo que se van a usar con el Asistente para la optimización de motor de base de datos.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- sample applications [DTA]
ms.assetid: 5b00e4eb-1742-43ec-98d8-d84216b6b840
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 1b475ce68b38283903ecd7295a89b18fce6b9636
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354410"
---
# <a name="simple-xml-input-file-sample-dta"></a>Ejemplo de archivo de entrada XML simple (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Copie y pegue este ejemplo de archivo de entrada XML simple que se utiliza para optimizar las cargas de trabajo en un editor XML o editor de texto. A continuación, sustituya los valores especificados en los elementos **Server**, **Database**, **Schema**, **Table**, **Workload** y **TuningOptions** por los de la sesión de optimización concreta. Para obtener más información sobre los atributos y elementos secundarios que se pueden usar con estos elementos, vea [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md). En el siguiente ejemplo, se utiliza únicamente un subconjunto de opciones de atributos y elementos secundarios disponibles.  
  
## <a name="code"></a>Código  
 [!code-xml[InputFileSamples#SimpleXMLInputFile](../../tools/dta/codesnippet/xml/simple-xml-input-file-sa_1.xml)]  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar y utilizar el Asistente para la optimización de motor de base de datos](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
