---
description: Conversión del tipo de datos con la transformación Conversión de datos
title: Conversión del tipo de datos con la transformación Conversión de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- converting data types [Integration Services]
- Data Conversion transformation
- data types [Integration Services], converting
ms.assetid: 4aabbe4f-7666-4672-865a-9627bd25fbfd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9056c9d657d1d5f4c9b24004c0ce70a2d32ed6e2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350737"
---
# <a name="convert-data-type-with-data-conversion-transformation"></a>Conversión del tipo de datos con la transformación Conversión de datos

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Para agregar y configurar una transformación Conversión de datos, el paquete ya debe incluir por lo menos una tarea Flujo de datos y un origen.  
  
### <a name="to-convert-data-to-a-different-data-type"></a>Para convertir datos en un tipo de datos diferente  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  Haga clic en la pestaña **Flujo de datos** , y luego, desde el **cuadro de herramientas**, arrastre la transformación Conversión de datos a la superficie de diseño.  
  
4.  Conecte la transformación Conversión de datos al flujo de datos arrastrando el conector desde el origen o la transformación anterior a la transformación Conversión de datos.  
  
5.  Haga doble clic en la transformación Conversión de datos.  
  
6.  En el cuadro de diálogo **Editor de transformación Conversión de datos** , en la tabla **Columnas de entrada disponibles** , active la casilla que aparece junto a las columnas cuyo tipo de datos quiere convertir.  
  
    > [!NOTE]  
    >  Puede aplicar varias conversiones de datos a una columna de entrada.  
  
7.  Opcionalmente, modifique los valores predeterminados en la columna **Alias de salida** .  
  
8.  En la lista **Tipo de datos** , seleccione el nuevo tipo de datos para la columna. El tipo de datos predeterminado es el tipo de datos de la columna de entrada.  
  
9. Opcionalmente, según el tipo de datos seleccionado, actualice los valores en las columnas **Longitud**, **Precisión**, **Escala**, y **Página de códigos** .  
  
10. Para configurar la salida de error, haga clic en **Configurar la salida de errores**. Para más información, consulte [Debugging Data Flow](../../../integration-services/troubleshooting/debugging-data-flow.md).  
  
11. Haga clic en **OK**.  
  
12. Para guardar el paquete actualizado, haga clic en **Guardar los elementos seleccionados**, en el menú **Archivo**.  
  
## <a name="see-also"></a>Consulte también  
 [Data Conversion Transformation](../../../integration-services/data-flow/transformations/data-conversion-transformation.md)   
 [Transformaciones de Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)   
 [Rutas de Integration Services](../../../integration-services/data-flow/integration-services-paths.md)   
 [Tipos de datos de Integration Services](../../../integration-services/data-flow/integration-services-data-types.md)   
 [Tarea Flujo de datos](../../../integration-services/control-flow/data-flow-task.md)  
  
  
