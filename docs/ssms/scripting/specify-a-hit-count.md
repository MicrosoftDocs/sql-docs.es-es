---
title: Especificar un número de llamadas
description: Obtenga información sobre cómo establecer un recuento de visitas para un punto de interrupción, de modo que el depurador no se interrumpa en ese punto de interrupción hasta que se alcance el recuento de visitas.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.breakpt.hitcount
helpviewer_keywords:
- Transact-SQL debugger, breakpoint hit count
ms.assetid: 24836939-94ed-4e57-aa85-5d6938d859e4
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7856ce9244f1a8767a4ce46b889fd39ca6a720f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353917"
---
# <a name="specify-a-hit-count"></a>Especificar un número de llamadas

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Un número de llamadas al punto de interrupción es un contador que el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] incrementa cada vez que se alcanza el punto de interrupción. Si se alcanza el número de llamadas especificado y se satisface la condición de punto de interrupción especificada, el depurador realiza la acción definida para el punto de interrupción.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="hit-count-considerations"></a>Consideraciones sobre el número de llamadas

 De forma predeterminada, la ejecución se interrumpe cada vez se ejecuta un punto de interrupción. Puede elegir entre las siguientes opciones:  
  
-   Interrumpir siempre ( valor predeterminado).  
  
-   Interrumpir cuando el número de llamadas alcanza un valor especificado.  
  
-   Interrumpir cuando el número de llamadas alcanza un múltiplo de un valor especificado.  
  
-   Interrumpir cuando el número de llamadas es mayor o igual que un valor especificado.  
  
 Los números de llamadas al punto de interrupción se incrementan dentro del ámbito de una sesión de depuración. Todos los números de llamadas se establecen en cero al comienzo de cada sesión de depuración.  
  
 Si desea realizar un seguimiento del número de veces que se ejecuta un punto de interrupción sin disponer de la ejecución de detención de punto de interrupción, especifique un número de llamadas con un valor muy alto para que nunca se produzca la detención de punto de interrupción.  
  
 La acción predeterminada para un punto de interrupción es que se detenga la ejecución cuando se hayan satisfecho el número de llamadas y la condición de punto de interrupción. Para obtener información sobre cómo especificar otras acciones, vea [Especificar una acción del punto de interrupción](./specify-a-breakpoint-action.md).  
  
#### <a name="to-specify-a-hit-count"></a>Para especificar un número de llamadas  
  
1.  En la ventana del editor, haga clic con el botón derecho en el glifo de punto de interrupción y, luego, haga clic en **Número de llamadas** del menú contextual.  
  
     O bien  
  
     En la ventana **Puntos de interrupción** , haga clic con el botón derecho en el glifo de punto de interrupción y, luego, haga clic en **Número de llamadas** del menú contextual.  
  
2.  En el cuadro de diálogo **Número de llamadas al punto de interrupción** , seleccione el comportamiento que desea en el cuadro **Cuando el punto de interrupción se visita** .  
  
     Si elige un valor distinto de **Interrumpir siempre**, aparecerá un cuadro de texto a la derecha de la lista. Escriba un número entero en el cuadro de texto para especificar el número de llamadas que desea.  
  
3.  Haga clic en **Aceptar** para implementar los cambios o en **Cancelar** para salir sin aplicar los cambios.  
  
#### <a name="to-view-or-reset-the-current-hit-count"></a>Para ver o restablecer el número de llamadas actual  
  
1.  En la ventana del editor, haga clic con el botón derecho en el glifo de punto de interrupción y, luego, haga clic en **Número de llamadas** del menú contextual.  
  
     O bien  
  
     En la ventana **Puntos de interrupción** , haga clic con el botón derecho en el glifo de punto de interrupción y, luego, haga clic en **Número de llamadas** del menú contextual.  
  
2.  En el cuadro de diálogo **Número de llamadas al punto de interrupción** , se muestra el **Número actual** justo encima del botón **Restablecer** .  
  
3.  Haga clic en **Restablecer** si desea establecer en cero el número de llamadas actual.  
  
4.  Haga clic en **Aceptar** o en **Cancelar** para salir del cuadro de diálogo.  
  
## <a name="see-also"></a>Consulte también  
 [Especificar una condición de punto de interrupción](./specify-a-breakpoint-condition.md)  
  
