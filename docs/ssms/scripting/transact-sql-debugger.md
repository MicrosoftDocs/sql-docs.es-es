---
title: Depurador de Transact-SQL
description: Obtenga información sobre cómo ejecutar paso a paso el código y ver el estado de ejecución mediante el depurador de Transact-SQL, y cómo realizar otras tareas de depuración.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, introduction
ms.assetid: 6e914699-0d85-46c2-aa2d-3e339ac2c4ce
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/22/2020
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: 573091d7d142af73fda87ed802186b66e1988f07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351602"
---
# <a name="transact-sql-debugger"></a>Depurador de Transact-SQL

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] ayuda a buscar errores en el código de [!INCLUDE[tsql](../../includes/tsql-md.md)] mediante la investigación del comportamiento en tiempo de ejecución del código. Después de establecer la ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en el modo de depuración, puede detener la ejecución de líneas específicas de código e inspeccionar la información y los datos usados por las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] o devueltos por ellas.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="stepping-through-transact-sql-code"></a>Avance paso a paso por el código Transact-SQL

El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] proporciona las siguientes opciones que puede usar para navegar por el código [!INCLUDE[tsql](../../includes/tsql-md.md)] cuando la ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] esté en el modo de depuración:

- Establecer puntos de interrupción en instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] individuales.

    Un punto de interrupción especifica un punto en el que desea que la ejecución se pause para poder examinar los datos. Cuando inicie el depurador, se detiene en la primera línea de código de la ventana del Editor de consultas. Para ejecutar el primer punto de interrupción que haya establecido, puede usar la característica **Continuar** . Asimismo puede usar **esta característica** para ejecutar el siguiente punto de interrupción desde cualquier ubicación en la que esté detenida la ventana. Puede modificar los puntos de interrupción para especificar acciones como las condiciones en las que el punto de interrupción debe pausar la ejecución, la información que se va a imprimir en la ventana de **resultados** y para cambiar la ubicación del punto de interrupción.  

- Ir a la siguiente instrucción.  

    Esta opción permite navegar por un conjunto de instrucciones de una en una, así como observar su comportamiento a medida que avanza.  

- Ir a una llamada o recorrerla paso a paso en un procedimiento almacenado o función.  

    Si está seguro de que no hay errores en un procedimiento almacenado, puede omitirlo. El procedimiento se ejecuta completo y los resultados se devuelven en el código.  

    Si desea depurar una procedimiento almacenado o una función, puede depurar paso a paso por instrucciones el módulo. [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] abre una nueva ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] que se rellena con el código fuente del módulo, pone la ventana en el modo de depuración y, a continuación, detiene la ejecución en la primera instrucción del módulo. Después puede navegar por el código del módulo, por ejemplo, estableciendo puntos de interrupción o recorriendo el código.  

Para obtener más información sobre cómo el depurador le permite navegar por el código, vea [Avanzar paso a paso por el código Transact-SQL](./step-through-transact-sql-code.md).  

## <a name="viewing-debugger-information"></a>Visualización de la información del depurador

Siempre que el depurador detenga la ejecución de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] específica, puede usar las siguientes ventanas del depurador para ver el estado de la ejecución actual:  

- **Variables locales** e **Inspección.** Estas ventanas muestran las expresiones de [!INCLUDE[tsql](../../includes/tsql-md.md)] asignadas actualmente. Las expresiones son cláusulas de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se evalúan en una sola expresión escalar. El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] admite la visualización de expresiones que hagan referencia a las variables, parámetros o funciones integradas de [!INCLUDE[tsql](../../includes/tsql-md.md)] que tengan nombres que empiecen por @@. Estas ventanas también muestran los valores de los datos asignados actualmente a las expresiones.  

- **Inspección rápida.** Esta ventana muestra el valor de una expresión de [!INCLUDE[tsql](../../includes/tsql-md.md)] y permite guardarla en una ventana **Inspección** .  

- **Puntos de interrupción.** Esta ventana muestra los puntos de interrupción establecidos actualmente y permite administrarlos.  

- **Pila de llamadas.** Esta ventana muestra la ubicación de ejecución actual. Además, proporciona información sobre el paso de la ejecución desde la ventana del Editor de consultas por las funciones, procedimientos administrados o desencadenadores hasta llegar a la ubicación de ejecución actual.  

- **Resultados.** Esta ventana muestra diferentes mensajes y datos del programa, como mensajes del sistema enviados desde el depurador.  

- **Resultados** y **Mensajes.** Estas pestañas de la ventana del Editor de consultas muestran el resultado de las instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] ejecutadas previamente.  

## <a name="transact-sql-debugger-tasks"></a>Tareas del depurador de Transact-SQL  

|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Describe cómo configurar el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] para realizar la depuración remota.|[Configurar reglas de firewall antes de ejecutar al depurador de TSQL](./configure-firewall-rules-before-running-the-tsql-debugger.md)|  
|Describe cómo iniciar, detener y controlar el funcionamiento del depurador.|[Ejecutar el depurador de Transact-SQL](./run-the-transact-sql-debugger.md)|  
|Describe cómo utilizar el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] para recorrer el código.|[Avanzar paso a paso por el código Transact-SQL](./step-through-transact-sql-code.md)|  
|Describe cómo utilizar el depurador para ver los datos de [!INCLUDE[tsql](../../includes/tsql-md.md)] , como los parámetros y las variables, así como la información del sistema.|[Ver información del depurador de Transact-SQL](./transact-sql-debugger-information.md)|  

## <a name="see-also"></a>Consulte también

[Editores de consultas y texto &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)