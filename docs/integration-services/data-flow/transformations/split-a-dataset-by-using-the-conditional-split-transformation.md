---
description: Dividir un conjunto de datos usando la transformación División condicional
title: División de un conjunto de datos con la transformación División condicional | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Conditional Split transformation
- splitting dataset
- datasets [Integration Services], splitting
ms.assetid: 23b3e84f-9296-4dc9-81c0-c7f06ae3f1ff
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2deebc6e6c730be90264bf7f0f3586f56b3d1546
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350991"
---
# <a name="split-a-dataset-by-using-the-conditional-split-transformation"></a>Dividir un conjunto de datos usando la transformación División condicional

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Para agregar y configurar una transformación División condicional, el paquete ya debe incluir por lo menos una tarea Flujo de datos y un origen.  
  
### <a name="to-conditionally-split-a-dataset"></a>Para realizar la división condicional de un conjunto de datos  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  Haga clic en la pestaña **Flujo de datos** y, desde el **cuadro de herramientas**, arrastre la transformación División condicional a la superficie de diseño.  
  
4.  Conecte la transformación División condicional al flujo de datos arrastrando el conector desde el origen de datos o la transformación anterior a la transformación División condicional.  
  
5.  Haga doble clic en la transformación División condicional.  
  
6.  En el **Editor de transformación División condicional**, genere las expresiones que se usan como condiciones arrastrando variables, columnas, funciones y operadores a la columna **Condición** de la cuadrícula. O bien, puede escribir la expresión en la columna **Condición** .  
  
    > [!NOTE]  
    >  Se puede usar una variable o columna en varias expresiones.  
  
    > [!NOTE]  
    >  Si la expresión no es válida, el texto de la expresión se resalta y la información sobre herramientas de la columna describe los errores.  
  
7.  Opcionalmente, modifique los valores en la columna **Nombre de salida** . Los nombres predeterminados son Caso 1, Caso 2, etc.  
  
8.  Para modificar la secuencia en la que se evalúan las condiciones, haga clic en la flecha arriba o flecha abajo.  
  
    > [!NOTE]  
    >  Coloque las condiciones que es más probable encontrar en la parte superior de la lista.  
  
9. Opcionalmente, modifique el nombre de la salida predeterminada para filas de datos que no coinciden con ninguna condición.  
  
10. Para configurar una salida de error, haga clic en **Configurar la salida de errores**. Para más información, consulte [Debugging Data Flow](../../../integration-services/troubleshooting/debugging-data-flow.md).  
  
11. Haga clic en **OK**.  
  
12. Para guardar el paquete actualizado, haga clic en **Guardar los elementos seleccionados**, en el menú **Archivo**.  
  
## <a name="see-also"></a>Consulte también  
 [Conditional Split Transformation](../../../integration-services/data-flow/transformations/conditional-split-transformation.md)   
 [Transformaciones de Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Rutas de Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [Tipos de datos de Integration Services](../../../integration-services/data-flow/integration-services-data-types.md)   
 [Tarea Flujo de datos](../../../integration-services/control-flow/data-flow-task.md)   
 [Expresiones de Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md)  
  
  
