---
title: Avanzar paso a paso por el código Transact-SQL
description: Obtenga información sobre cómo usar el depurador de Transact-SQL para controlar qué instrucciones de Transact-SQL se ejecutan en una ventana del Editor de consultas del Motor de base de datos.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, debugging code
- Transact-SQL debugger, step over
- Transact-SQL debugger, step out
- Transact-SQL debugger, step into
ms.assetid: e09079b8-c4c9-42b4-821b-4ce81a98a086
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7a48a07ddec4cb6c1f0670a5e6b137690b6f2185
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340685"
---
# <a name="step-through-transact-sql-code"></a>Avanzar paso a paso por el código Transact-SQL

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] permite controlar las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se ejecutan en una ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Puede detener el depurador en instrucciones individuales y, a continuación, ver el estado de los elementos de código en ese punto.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="breakpoints"></a>Puntos de interrupción

Un punto de interrupción indica al depurador que detenga la ejecución en una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] específica. Para más información sobre los puntos de interrupción, vea [Transact-SQL Breakpoints](./transact-sql-breakpoints.md) (Puntos de interrupción de Transact-SQL).  
  
## <a name="controlling-statement-execution"></a>Controlar la ejecución de instrucciones

En el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] , puede especificar las siguientes opciones para su ejecución desde la instrucción actual del código [!INCLUDE[tsql](../../includes/tsql-md.md)] :

- Ejecutar un proceso hasta el siguiente punto de interrupción.

- Ir a la siguiente instrucción.  

    Si la siguiente instrucción invoca un procedimiento almacenado, función o desencadenador de [!INCLUDE[tsql](../../includes/tsql-md.md)] , el depurador muestra una nueva ventana del Editor de consultas que contiene el código del módulo. La ventana está en el modo de depuración y la ejecución se detiene en la primera instrucción del módulo. Después puede desplazarse por el código del módulo, por ejemplo, estableciendo puntos de interrupción o recorriendo el código.

- Paso a paso por la siguiente instrucción.

    Se ejecuta la siguiente instrucción. Sin embargo, si la instrucción invoca un procedimiento almacenado, una función o un desencadenador, el código del módulo se ejecuta hasta que termine y los resultados se devuelven al código de llamada. Si está seguro de que no hay errores en un procedimiento almacenado, puede omitirlo. La ejecución se detiene en la instrucción que sigue a la llamada al procedimiento almacenado, a la función o al desencadenador.

- Salir de un procedimiento almacenado, función o desencadenador.  

    La ejecución se detiene en la instrucción que sigue a la llamada al procedimiento almacenado, a la función o al desencadenador.  

- Ejecutar el proceso desde la ubicación actual hasta la ubicación actual del puntero e ignorar todos los puntos de interrupción.  

 La tabla siguiente muestra las distintas formas en las que puede controlar la ejecución de las instrucciones del depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
|Acción|Realizar acción:|  
|------------|---------------------|  
|Ejecutar todas las instrucciones desde la instrucción actual hasta el siguiente punto de interrupción|Haga clic en **Continuar** en el menú **Depurar** .<br /><br /> Haga clic en el botón **Continuar** en la barra de herramientas **Depurar** .|  
|Ir a la siguiente instrucción o módulo|Haga clic en **Paso a paso por instrucciones** en el menú **Depurar** .<br /><br /> Haga clic en el botón **Paso a paso por instrucciones** en la barra de herramientas **Depurar** .<br /><br /> Presione F11.|  
|Paso a paso por la siguiente instrucción o módulo|Haga clic en **Paso a paso por procedimientos** en el menú **Depurar** .<br /><br /> Haga clic en el botón **Paso a paso por procedimientos** en la barra de herramientas **Depurar** .<br /><br /> Presione F10.|  
|Salir de un módulo|Haga clic en **Paso a paso para salir** en el menú **Depurar** .<br /><br /> Haga clic en el botón **Paso a paso para salir** en la barra de herramientas **Depuración** .<br /><br /> Presione MAYÚS+F11.|  
|Ejecutar un proceso hasta la ubicación del cursor actual|Haga clic con el botón derecho en la ventana del Editor de consultas y, después, haga clic en **Ejecutar hasta el cursor**.<br /><br /> Presione CTRL+F10.|  
  
## <a name="see-also"></a>Consulte también

- [Ver información del depurador de Transact-SQL](./transact-sql-debugger-information.md)