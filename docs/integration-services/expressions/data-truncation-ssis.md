---
description: Truncamiento de datos (SSIS)
title: Truncamiento de datos (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- truncating data
- data truncation [Integration Services]
- expressions [Integration Services], data truncation
- truncate options [Integration Services]
ms.assetid: 02461e15-49ca-445b-abb3-5c369c283ec2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 12431bc9717b28552cc79d6f7d8e3026ed90c7cd
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123268"
---
# <a name="data-truncation-ssis"></a>Truncamiento de datos (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La conversión de valores de un tipo de datos a otro puede provocar que esos valores se trunquen.  
  
 Puede producirse un truncamiento en los siguientes casos:  
  
-   Al traducir datos de cadena de *DT_WSTR* a *DT_STR* con la misma longitud, si la cadena original contiene caracteres de doble byte.  
  
-   Al convertir un entero de *DT_I4* a *DT_I2* pueden perderse dígitos importantes.  
  
-   Al convertir un entero sin firmar en un entero firmado pueden perderse dígitos importantes.  
  
-   Al convertir un número real de *DT_R8* a *DT_R4* pueden perderse dígitos no importantes.  
  
-   Al convertir un entero de *DT_I4* a *DT_R4* pueden perderse dígitos no importantes.  
  
 El evaluador de expresiones identifica las conversiones explícitas que pueden causar el truncamiento y emite una advertencia al analizar la expresión. Por ejemplo, el evaluador de expresiones emite una advertencia si se convierte una cadena de 30 caracteres en una cadena de 20 caracteres.  
  
 Sin embargo, el truncamiento no se comprueba en tiempo de ejecución. En tiempo de ejecución, los datos se truncan sin advertencia. La mayoría de los adaptadores y las transformaciones de datos admiten salidas de error que pueden controlar la disposición de las filas de error.  
  
 Para más información sobre cómo controlar el truncamiento de datos, vea [Control de errores en los datos](../../integration-services/data-flow/error-handling-in-data.md).  
  
  
