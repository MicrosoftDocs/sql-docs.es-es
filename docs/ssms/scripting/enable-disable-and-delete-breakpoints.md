---
title: Habilitar, deshabilitar y eliminar puntos de interrupción
description: Aprenda a usar la ventana Puntos de interrupción para ver, eliminar, deshabilitar y habilitar puntos de interrupción.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 357b5874-273f-43a9-8e30-83872bdea5dc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fec0b9a118326b0afade7d7bab0a26777d8e523
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354638"
---
# <a name="enable-disable-and-delete-breakpoints"></a>Habilitar, deshabilitar y eliminar puntos de interrupción

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Para ver y administrar todos los puntos de interrupción abiertos, puede usar la ventana **Puntos de interrupción** . Use la ventana para ver la información de punto de interrupción y para realizar acciones tales como eliminar, deshabilitar o habilitar puntos de interrupción.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
## <a name="the-breakpoints-window"></a>Ventana Puntos de interrupción  
 La ventana **Puntos de interrupción** muestra información como la línea de código en la que está ubicado el punto de interrupción. Además en **Puntos de interrupción** , puede eliminar, deshabilitar y habilitar los puntos de interrupción. Para obtener más información sobre la ventana **Puntos de interrupción** , vea [Breakpoints Window](./transact-sql-debugger-breakpoints-window.md)  
  
 Al deshabilitar un punto de interrupción, se evita que detenga la ejecución, pero deja la definición en su lugar en caso de que desee habilitar el punto de interrupción posteriormente. Al eliminar un punto de interrupción se quita permanentemente. Debe alternar un nuevo punto de interrupción para pausar la ejecución de la instrucción.  
  
## <a name="to-open-the-breakpoints-window"></a>Para abrir la ventana Puntos de interrupción  
 **To open the Breakpoints window**  
  
 Puede abrir la ventana **Puntos de interrupción** de una de las siguientes formas:  
  
-   En el menú **Depurar** , haga clic en **Ventanas** y, a continuación, haga clic en **Puntos de interrupción**.  
  
-   En la barra de herramientas **Depurar** , haga clic en el botón **Puntos de interrupción** .  
  
-   Presione CTRL+ALT+B.  
  
## <a name="to-disable-a-single-breakpoint"></a>Para deshabilitar un punto de interrupción  
 **To disable a single breakpoint**  
  
 Puede deshabilitar un punto de interrupción de una de las siguientes maneras:  
  
-   En la ventana del Editor de consultas, haga clic con el botón derecho en el punto de interrupción y, luego, haga clic en **Deshabilitar punto de interrupción**.  
  
-   En la ventana Puntos de interrupción, desactive la casilla situada a la izquierda del punto de interrupción.  
  
## <a name="to-disable-all-breakpoints"></a>Para deshabilitar todos los puntos de interrupción  
 **To disable all breakpoints**  
  
 Puede deshabilitar todos los puntos de interrupción de una de las siguientes maneras:  
  
-   En el menú **Depurar** , haga clic en **Deshabilitar todos los puntos de interrupción**.  
  
-   En la barra de herramientas de la ventana **Puntos de interrupción** , haga clic en el botón **Deshabilitar todos los puntos de interrupción** .  
  
## <a name="to-enable-a-single-breakpoint"></a>Para habilitar un punto de interrupción  
 **To enable a single breakpoint**  
  
 Puede habilitar un punto de interrupción de una de las siguientes maneras:  
  
-   En la ventana del Editor de consultas, haga clic con el botón derecho en el punto de interrupción y, luego, haga clic en **Habilitar punto de interrupción**.  
  
-   En la ventana Puntos de interrupción, active la casilla situada a la izquierda del punto de interrupción.  
  
## <a name="to-enable-all-breakpoints"></a>Para habilitar todos los puntos de interrupción  
 **To enable all breakpoints**  
  
 Puede habilitar todos los puntos de interrupción de una de las siguientes maneras:  
  
-   En el menú **Depurar** , haga clic en **Habilitar todos los puntos de interrupción**.  
  
-   En la barra de herramientas de la ventana **Puntos de interrupción** , haga clic en el botón **Habilitar todos los puntos de interrupción** .  
  
## <a name="to-delete-a-single-breakpoint"></a>Para eliminar un punto de interrupción  
 **To delete a single breakpoint**  
  
 Puede eliminar un punto de interrupción de una de las siguientes maneras:  
  
-   En la ventana del Editor de consultas, haga clic con el botón derecho en el punto de interrupción y, luego, haga clic en **Eliminar punto de interrupción**.  
  
-   En la ventana Puntos de interrupción, haga clic con el botón derecho en el punto de interrupción y, luego, haga clic en el comando **Eliminar** del menú contextual.  
  
-   En la ventana Puntos de interrupción, seleccione el punto de interrupción y, a continuación, presione SUPR.  
  
## <a name="to-delete-all-breakpoints"></a>Para eliminar todos los puntos de interrupción  
 **To delete all breakpoints**  
  
 Puede eliminar todos los puntos de interrupción de una de las siguientes maneras:  
  
-   En el menú **Depurar**, haga clic en **Eliminar todos los puntos de interrupción**.  
  
-   En la barra de herramientas de la ventana **Puntos de interrupción** , haga clic en el botón **Eliminar todos los puntos de interrupción** .  
  
## <a name="see-also"></a>Consulte también  
 [Alternar un punto de interrupción](./toggle-a-breakpoint.md)  
  
