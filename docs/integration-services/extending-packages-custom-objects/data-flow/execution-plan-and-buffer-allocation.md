---
description: Plan de ejecución y asignación de búfer
title: Plan de ejecución y asignación de búfer | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom data flow components [Integration Services], buffer allocations
- custom data flow components [Integration Services], execution plans
- buffer allocations [Integration Services]
- data flow components [Integration Services], buffer allocations
- data flow components [Integration Services], execution plans
- execution plans [Integration Services]
ms.assetid: 679d9ff0-641e-47c3-abb8-d1a7dcb279dd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b9827f4943e21b117353af4aaa4ea825cef19d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343694"
---
# <a name="execution-plan-and-buffer-allocation"></a>Plan de ejecución y asignación de búfer

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Antes de la ejecución, la tarea de flujo de datos examina sus componentes y genera un plan de ejecución para cada secuencia de componentes. En esta sección se proporcionan detalles sobre el plan de ejecución, cómo ver el plan y cómo se asignan búferes de entrada y salida en función del plan de ejecución.  
  
## <a name="understanding-the-execution-plan"></a>Descripción del plan de ejecución  
 Un plan de ejecución contiene subprocesos de origen y de trabajo; cada subproceso contiene listas de trabajo que especifican listas de trabajo de salida para los subprocesos de origen o listas de trabajo de entrada y salida para los subprocesos de trabajo. Los subprocesos de origen de un plan de ejecución representan los componentes de origen del flujo de datos y se identifican en el plan de ejecución mediante *SourceThreadn*, donde *n* es el número de base cero del subproceso de origen.  
  
 Cada subproceso de origen crea un búfer, establece un agente de escucha y llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> en el componente de origen. Aquí es donde se inicia la ejecución y se original los datos, a medida que el componente de origen comienza a agregar filas a los búferes de salida que le proporciona la tarea de flujo de datos. Una vez que los subprocesos de origen se están ejecutando, el equilibrio de trabajo se distribuye entre subprocesos de trabajo.  
  
 Un subproceso de trabajo puede contener listas de trabajo de entrada y salida, y se identifica en el plan de ejecución como *WorkThreadn*, donde *n* es el número de base cero del subproceso de trabajo. Estos subproceso contienen listas de trabajo de salida cuando el gráfico contiene un componente con salidas asincrónicas.  
  
 El plan de ejecución de ejemplo siguiente representa un flujo de datos que contiene un componente de origen conectado a una transformación con una salida asincrónica conectada a un componente de destino. En este ejemplo, WorkThread0 contiene una lista de trabajo de salida porque el componente de transformación tiene una salida asincrónica.  
  
```  
SourceThread0   
    Influences: 72 158   
    Output Work List   
        CreatePrimeBuffer of type 1 for output id 10   
        SetBufferListener: "WorkThread0" for input ID 73   
        CallPrimeOutput on component "OLE DB Source" (1)   
    End Output Work List   
    This thread drives 0 distributors   
End SourceThread0   
WorkThread0   
    Influences: 72 158   
    Input Work list, input ID 73   
        CallProcessInput on input ID 73 on component "Sort" (72) for view type 2   
    End Input Work list for input 73   
    Output Work List   
        CreatePrimeBuffer of type 3 for output id 74   
        SetBufferListener: "WorkThread1" for input ID 171with internal handoff   
        CallPrimeOutput on component "Sort" (72)   
    End Output Work List   
    This thread drives 0 distributors   
End WorkThread0   
WorkThread1   
    Influences: 158   
    Input Work list, input ID 171  
        CallProcessInput on input ID 171 on component "OLE DB Destination" (158) for view type 4  
    End Input Work list for input 171   
    Output Work List   
    End Output Work List   
    This thread drives 0 distributors   
End WorkThread1  
```  
  
> [!NOTE]  
>  El plan de ejecución se genera cada vez que se ejecuta un paquete y se puede capturar agregando un proveedor de registro al paquete, habilitando el registro y seleccionando el evento **PipelineExecutionPlan**.  
  
## <a name="understanding-buffer-allocation"></a>Descripción de la asignación de búferes  
 La tarea de flujo de datos, basándose en el plan de ejecución, crea búferes que contienen las columnas definidas en las salidas de los componentes de flujo de datos. El búfer se reutiliza a media que los datos fluyen a través de la secuencia de componentes, hasta que se encuentra un componente con salidas asincrónicas. Entonces, se crea un nuevo búfer que contiene las columnas de salida de la salida asincrónica y las columnas de salida de componentes de nivel inferior.  
  
 Durante la ejecución, los componentes tienen acceso al búfer en el subproceso de origen o trabajo actual. El búfer es un búfer de entrada, que proporciona el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>, o un búfer de salida, que proporciona el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. La propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.Mode%2A> de <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> también identifica cada búfer como búfer de entrada o salida.  
  
 Los componentes de transformación con salidas asincrónicas reciben el búfer de entrada existente del método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> y reciben el nuevo búfer de salida del método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. Un componente de transformación con salidas asincrónicas es el único tipo de componente de flujo de datos que recibe un búfer de entrada y un búfer de salida.  
  
 Dado que es probable que el búfer que se proporciona a un componente contenga más columnas de las que el componente incluye en sus colecciones de columnas de entrada o salida, los desarrolladores de componentes pueden llamar al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> para localizar una columna en el búfer mediante la especificación de su valor **LineageID**.  
  
