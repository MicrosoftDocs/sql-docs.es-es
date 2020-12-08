---
title: Guardar eventos Showplan XML por separado
description: Obtenga información sobre cómo usar SQL Server Profiler para guardar en archivos independientes los eventos Showplan XML capturados mediante seguimiento. Abra los archivos en SQL Server Management Studio.
titleSuffix: SQL Server Profiler
ms.custom: seo-dt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Showplan XML events
- saving Showplan XML events
- events [SQL Server], Showplan XML
ms.assetid: 33320a7a-36e8-401c-876d-5b82c49abd85
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34c79b9f3dad13a8927296d424d827eb693ad1dd
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505029"
---
# <a name="save-showplan-xml-events-separately-sql-server-profiler"></a>Guardar eventos Showplan XML por separado (SQL Server Profiler)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo guardar eventos **Showplan XML** capturados en seguimientos en archivos .SQLPlan distintos mediante el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. Puede abrir los archivos de evento **Showplan XML** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para poder ver el plan de ejecución gráfico de cada evento.  
  
## <a name="save-showplan-xml-events-separately"></a>Guardar eventos Showplan XML por separado  
  
1. En el menú **Archivo**, seleccione **Nuevo seguimiento** y, a continuación, conéctese a una instancia de SQL Server.  
  
     Aparecerá el cuadro de diálogo **Propiedades de seguimiento** .  
  
    > [!NOTE]  
    >  Si selecciona **Iniciar el seguimiento inmediatamente tras realizar la conexión**, el cuadro de diálogo **Propiedades de seguimiento** no aparecerá y, en su lugar, se iniciará el seguimiento. Para desactivar esta configuración, en el menú **Herramientas**, seleccione **Opciones** y desmarque la casilla **Iniciar el seguimiento inmediatamente tras realizar la conexión**.  
  
2. En el cuadro de diálogo **Propiedades de seguimiento** , en el cuadro **Nombre de seguimiento** , escriba un nombre para el seguimiento.  
  
3. En la lista **Usar la plantilla**, seleccione una plantilla de seguimiento en la que se basará el seguimiento. Si no quiere usar una plantilla, seleccione **En blanco**.  
  
4. Realice una de las siguientes acciones:  
  
    -   Active la casilla **Guardar en archivo** para capturar el seguimiento en un archivo. Especifique un valor en **Establecer el tamaño máximo de archivo (MB)** . 
    
        Opcionalmente, active las casillas **Habilitar sustitución incremental de archivos** y **El servidor procesa los datos de seguimiento** .  
  
    -   Active la casilla **Guardar en tabla** para capturar el seguimiento en una tabla de base de datos. 
    
        Si quiere, seleccione **Establecer número máximo de filas** y especifique un valor.  
  
5. Opcionalmente, active la casilla **Habilitar hora de detención de seguimiento** para especificar una fecha y hora de detención. 
  
6. Seleccione la pestaña **Selección de eventos**.  
  
7. En la columna de datos **Eventos**, expanda la categoría de evento **Rendimiento** y, luego, active la casilla **Showplan XML**. Si la categoría de eventos **Performance** no está disponible, seleccione **Mostrar todos los eventos** para mostrarla.  
  
     La pestaña **Configuración de extracción de eventos** se agrega al cuadro de diálogo **Propiedades de seguimiento**.  
  
8. En la pestaña **Configuración de extracción de eventos**, seleccione **Guardar eventos de plan de presentación XML por separado**.  
  
9. En el cuadro de diálogo **Guardar como** , especifique el nombre de archivo para guardar los eventos **Showplan XML** .  
  
10. Seleccione **Todos los lotes de plan de presentación en un solo archivo** para guardar todos los eventos **Showplan XML** en un solo archivo XML. O bien seleccione **Cada lote de plan de presentación en un archivo independiente** para crear un nuevo archivo XML para cada evento **Showplan XML**.  
  
11. Para ver el archivo del evento **Showplan XML** en SQL Server Management Studio, en el menú **Archivo**, seleccione **Abrir** y, a continuación, **Archivo**. Navegue al directorio donde ha guardado el archivo o archivos de evento **Showplan XML** para seleccionar uno y abrirlo. Los archivos de evento **Showplan XML** tienen la extensión de archivo .SQLPlan.  

## <a name="see-also"></a>Consulte también  
 [Analyze queries with Showplan results in SQL Server Profiler](../../tools/sql-server-profiler/analyze-queries-with-showplan-results-in-sql-server-profiler.md) (Analizar consultas con resultados de plan de presentación en SQL Server Profiler)  
  
  
