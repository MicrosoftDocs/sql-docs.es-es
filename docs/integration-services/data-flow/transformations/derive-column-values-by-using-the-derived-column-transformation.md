---
description: Derivación de valores de columna mediante la transformación Columna derivada
title: Derivación de valores de columna mediante la transformación Columna derivada | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- columns [Integration Services]
- derived columns
- columns [Integration Services], values
- Derived Column transformation
ms.assetid: 28b07746-fc6f-42b2-b741-9de6fac3f29c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 06ecff11e6fc32ad59e6457ec2404292a9c22955
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353727"
---
# <a name="derive-column-values-with-the-derived-column-transformation"></a>Derivación de valores de columna mediante la transformación Columna derivada

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Para agregar y configurar una transformación Columna derivada, el paquete ya debe incluir por lo menos una tarea Flujo de datos y un origen.  
  
 La transformación Columna derivada usa expresiones para actualizar los valores de las columnas existentes o agregar valores a las nuevas. Si se elige agregar valores a columnas nuevas, el cuadro de diálogo **Editor de transformación Columna derivada** evalúa la expresión y define los metadatos de las columnas en función del resultado. Por ejemplo, si una expresión concatena dos columnas (cada una con el tipo de datos DT_WSTR y una longitud de 50) con un espacio entre los dos valores de las columnas, la nueva columna tiene el tipo de datos DT_WSTR y una longitud de 101. El tipo de datos de las columnas nuevas se puede actualizar. El único requisito es que el tipo de datos sea compatible con los datos insertados. Por ejemplo, el cuadro de diálogo **Editor de transformación Columna derivada** genera un error de validación si se asigna un valor de datos a una columna con tipo de datos entero. En función del tipo de datos seleccionado, se puede especificar la longitud, precisión, escala y página de códigos de la columna.  
  
### <a name="to-derive-column-values"></a>Para derivar valores de columna  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  Haga clic en la pestaña **Flujo de datos** y, a continuación, desde el **cuadro de herramientas**, arrastre la transformación Columna derivada a la superficie de diseño.  
  
4.  Conecte la transformación Columna derivada al flujo de datos arrastrando el conector desde el origen o la transformación anterior a la transformación Columna derivada.  
  
5.  Haga doble clic en la transformación Columna derivada.  
  
6.  En el cuadro de diálogo **Editor de transformación Columna derivada** , genere las expresiones que se usan como condiciones arrastrando variables, columnas, funciones y operadores a la columna **Expresión** en la cuadrícula. Como alternativa, puede escribir la expresión en la columna **Expresión** .  
  
    > [!NOTE]  
    >  Si la expresión no es válida, el texto de la expresión se resalta y la información sobre herramientas de la columna describe los errores.  
  
7.  En la lista **Columna derivada**, seleccione **\<add as new column>** para escribir el resultado de la evaluación de la expresión en una columna nueva, o seleccione una columna existente para actualizarla con el resultado de la evaluación.  
  
     Si se decide usar una columna nueva, el cuadro de diálogo **Editor de transformación Columna derivada** evalúa la expresión y asigna un tipo de datos a la columna, en función del tipo de datos, la longitud, precisión, escala y página de códigos.  
  
8.  Al utilizar una nueva columna, seleccione un tipo de datos en la lista **Tipo de datos** . Según el tipo de datos seleccionado, actualice opcionalmente los valores en las columnas **Longitud**, **Precisión**, **Escala** y **Página de códigos** . Los metadatos de las columnas existentes no pueden cambiarse.  
  
9. Opcionalmente, modifique los valores de la columna **Nombre de columna derivada** .  
  
10. Para configurar la salida de error, haga clic en **Configurar la salida de errores**. Para más información, consulte [Debugging Data Flow](../../../integration-services/troubleshooting/debugging-data-flow.md).  
  
11. Haga clic en **OK**.  
  
12. Para guardar el paquete actualizado, haga clic en **Guardar los elementos seleccionados**, en el menú **Archivo**.  
  
## <a name="see-also"></a>Consulte también  
 [Derived Column Transformation](../../../integration-services/data-flow/transformations/derived-column-transformation.md)   
 [Tipos de datos de Integration Services](../../../integration-services/data-flow/integration-services-data-types.md)   
 [Transformaciones de Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Rutas de Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [Tarea Flujo de datos](../../../integration-services/control-flow/data-flow-task.md)   
 [Expresiones de Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md)  
  
  
