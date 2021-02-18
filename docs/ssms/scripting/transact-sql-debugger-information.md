---
title: Ver información del depurador de Transact-SQL
description: Obtenga información sobre cómo ver la salida del depurador de Transact-SQL, que incluye información como pilas de llamadas, subprocesos, puntos de interrupción, código, variables y comandos.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, Locals Window
- Transact-SQL debugger, Watch Window
- Transact-SQL debugger, Threads Window
- Transact-SQL debugger, Call Stack Window
- Transact-SQL debugger, QuickWatch
- Transact-SQL debugger, viewing information
ms.assetid: b99819cc-f388-41a1-b304-36e78ce24147
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 412ae1788542d4fa00df6a17df1b6450f7605851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341006"
---
# <a name="transact-sql-debugger---information"></a>Transact-SQL Debugger - Information (Depurador de Transact-SQL: Información)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Siempre que el depurador detenga la ejecución de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] específica, puede usar las distintas ventanas del depurador para ver el estado de la ejecución actual. 

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="debugger-windows"></a>Ventanas del depurador  

En el modo de depurador, éste abre dos ventanas en la parte inferior de la ventana principal de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] . El depurador muestra toda su información en estas dos ventanas. Cada una de ellas tiene pestañas que puede seleccionar para controlar el conjunto de información que se muestra en la ventana. La ventana izquierda del depurador contiene las pestañas **Variables locales**, **Inspección1**, **Inspección2**, **Inspección3** e **Inspección4** . La ventana derecha del depurador contiene las pestañas **Pila de llamadas**, **Subprocesos**, **Puntos de interrupción**, **Ventana de comandos** y **Salida** .  
  
> [!NOTE]  
>  Las descripciones anteriores se aplican a las ubicaciones predeterminadas de las ventanas del depurador. Puede arrastrar una pestaña para moverla de una ventana a otra. O bien, puede desacoplar una pestaña para crear una nueva ventana que puede colocar donde prefiera.  
  
 De forma predeterminada, no todas estas pestañas o ventanas están activas. Puede abrir una ventana concreta mediante cualquiera de los procedimientos siguientes:  
  
-   En el menú **Depurar** , haga clic en **Ventanas** y, a continuación, seleccione la ventana que desee.  
  
-   En la barra de herramientas **Depuración** , haga clic en **Puntos de interrupción** y, a continuación, seleccione la ventana que desee.  
  
## <a name="transact-sql-expressions"></a>Expresiones de Transact-SQL  
 Las expresiones son cláusulas de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se evalúan en un único valor escalar, como las variables o los parámetros. La ventana izquierda del depurador puede mostrar los valores de datos asignados actualmente a expresiones en un máximo de cinco pestañas o ventanas: **Variables locales, Inspección1**, **Inspección2**, **Inspección3** e **Inspección4**.  
  
 La ventana **Variables locales** muestra información sobre las variables locales del ámbito actual del depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] . El conjunto de expresiones que se muestran en **esta ventana** cambia a medida que el depurador se ejecuta en las distintas partes del código.  
  
 Las expresiones de **QuickWatch** y las cuatro ventanas de **Inspección** no se limitan únicamente a enumerar identificador de una variable. Puede especificar una expresión de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se evalúa como un valor único, por ejemplo,  agregar un número a una variable, o una instrucción SELECT que se evalúa como un valor único. Algunos ejemplos son:  
  
-   El nombre de una variable, como @IntegerCounter.  
  
-   Una operación aritmética en una variable, como @IntegerCounter + 1.  
  
-   Una operación de cadena con dos variables de carácter, como @FirstName + @LastName.  
  
-   Una instrucción SELECT que devuelve un valor único, como SELECT CharCol FROM MyTable WHERE PrimaryKey = 1.  
  
 Puede usar la ventana **Inspección rápida** para ver el valor de una expresión de [!INCLUDE[tsql](../../includes/tsql-md.md)] y, a continuación, guardarla en una ventana **Inspección** . Para seleccionar una expresión en **Inspección rápida**, seleccione o escriba el nombre de la expresión en el cuadro **Expresión** .  
  
 Las cuatro ventanas **Inspección** muestran información sobre las variables y las expresiones que haya seleccionado. El conjunto de expresiones que se muestran en **estas ventanas** no cambian hasta que agregue o elimine expresiones desde la lista.  
  
 Para agregar una expresión a una ventana **Inspección** , puede seleccionar la opción **Agregar inspección** del cuadro de diálogo **Inspección rápida** o escribir el nombre de la expresión en la columna **Nombre** de una fila vacía en una ventana **Inspección** .  
  
 Puede establecer los valores de datos de las variables en las ventanas **Variables locales**, **Inspección** o **Inspección rápida** ; para ello, haga clic con el botón derecho en la fila y, después, seleccione **Editar valor**. La columna **Valor** de la ventana **Variables locales** , la ventana **Inspección** y el cuadro de diálogo **Inspección rápida** admiten visualizadores de texto, de datos XML y HTML. Los visualizadores se representan mediante una sugerencia de datos de lupa en el extremo derecho de la ventana **Valores** . Puede usar los visualizadores para ver texto, valores de datos XML o HTML en presentaciones que hagan coincidir los tipos de valores, por ejemplo, ver archivos XML en la ventana de un explorador.  
  
 En modo de depuración, al mover el puntero sobre un identificador, aparece la ventana emergente **Quick Info** con el nombre de la expresión y su valor actual. Para obtener más información, vea [Información rápida &#40;IntelliSense&#41;](./quick-info-intellisense.md).  
  
## <a name="breakpoints"></a>Puntos de interrupción  
 Puede utilizar la ventana **Puntos de interrupción** para ver y administrar los puntos de interrupción establecidos actualmente. Para obtener más información, vea [Avanzar paso a paso por el código Transact-SQL](./step-through-transact-sql-code.md).  
  
## <a name="call-stacks"></a>Pilas de llamadas  
 La ventana **Pila de llamadas** muestra la ubicación de ejecución actual, así como información sobre el paso de la ejecución desde la ventana del editor original a través de cualquier módulo de [!INCLUDE[tsql](../../includes/tsql-md.md)] (funciones, procedimientos almacenados o desencadenadores) hasta llegar a la ubicación de ejecución actual. Todas las filas de la ventana **Pila de llamadas** se denominan marco de pila y representan cualquiera de los siguientes elementos:  
  
-   Ubicación de ejecución actual.  
  
-   Llamada de un módulo a otro.  
  
-   Llamada de una ventana del editor a un módulo de [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 El orden de la pila es el inverso al orden en el que se llamó a los módulos. La ubicación de ejecución actual está en la parte superior de la pila y la llamada original en la inferior. Una flecha amarilla en el margen izquierda del marco de pila identifica el marco en el que el depurador detuvo la ejecución.  
  
 La columna **Nombre** registra la siguiente información:  
  
-   El módulo de origen que contiene la línea de código que invocó el siguiente nivel.  
  
-   La línea de código que llamó al siguiente módulo de la pila.  
  
-   Si la llamada se dirigió a un procedimiento almacenado o a una función que tomó parámetros, también se muestran los nombres, tipos de datos y valores de todos los parámetros.  
  
 Las expresiones de las ventanas **Variables locales**, **Inspección** e **Inspección rápida** se evalúan para el marco de pila actual. De forma predeterminada, este marco es el marco superior de la pila, en el que el depurador detuvo la ejecución. Cuando especifique otro marco de pila como marco actual, las expresiones de las ventanas **Variables locales**, **Inspección** e **Inspección rápida** se vuelven a evaluar para el nuevo marco de pila. Puede cambiar el marco de pila actual haciendo doble clic en un marco o haciendo clic en un marco y seleccionando **Cambiar a marco**. En ese punto, las expresiones de las ventanas **Variables locales**, **Inspección** e **Inspección rápida** se vuelven a evaluar para el nuevo marco. Cuando el marco de pila actual no sea el marco superior de la pila, una flecha verde situada en el margen izquierdo del marco de pila identifica el marco de pila actual.  
  
 Al hacer clic con el botón derecho en un marco de pila y seleccionar **Ir a código fuente**, se muestra el código del marco en una ventana del Editor de consultas. Sin embargo, ese marco no es el marco actual y no se modifica el contenido de las ventanas **Variables locales**, **Inspección** e **Inspección rápida** .  
  
## <a name="system-information-and-transact-sql-results"></a>Información del sistema y resultados de Transact-SQL  
 El depurador muestra su estado y los mensajes de eventos en la ventana **Resultados** . En ella se incluye información como el momento en que el depurador se adjunta a otros procesos o cuándo finalizan los subprocesos del depurador.  
  
 En el modo de depuración, las pestañas **Resultados** y **Mensajes** continúan estando activas en el Editor de consultas. La pestaña **Resultados** continúa mostrando los conjuntos de resultados de las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se ejecutan durante una sesión de depuración. La pestaña **Mensajes** continúa mostrando los mensajes del sistema, como *xx* filas afectadas y el resultado de las instrucciones PRINT y RAISERROR.  
  
## <a name="see-also"></a>Consulte también  
 [Ventana de locales](./transact-sql-debugger-locals-window.md)   
 [Ventana de inspección](./transact-sql-debugger-watch-window.md)   
 [Cuadro de diálogo Inspección rápida](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [Ventana de puntos de interrupción](./transact-sql-debugger-breakpoints-window.md)   
 [Ventana de pila de llamadas](./transact-sql-debugger-call-stack-window.md)   
 [Ventana de subprocesos](./transact-sql-debugger-threads-window.md)   
 [Resultados (ventana)](./transact-sql-debugger-output-window.md)   
 [Depurador de Transact-SQL](./transact-sql-debugger.md)  
  
