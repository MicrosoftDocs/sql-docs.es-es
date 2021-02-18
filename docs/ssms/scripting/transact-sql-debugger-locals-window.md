---
title: Ventana de locales
description: Aprenda a usar la ventana Variables locales del depurador de Transact-SQL para mostrar y modificar expresiones en el marco de pila de llamadas actual.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Locals Window [Transact-SQL]
ms.assetid: 59bea640-7823-4b4d-832c-f384d83cca2f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dd6fc308bf65904d943b298546f2006b9f259abe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351614"
---
# <a name="transact-sql-debugger---locals-window"></a>Depurador de Transact-SQL: ventana Variables locales

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

La ventana **Variables locales** muestra información sobre las expresiones locales en el ámbito actual del depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] . El ámbito está establecido en el marco de la pila de llamadas actual que está seleccionado en la ventana **Pila de llamadas** . Debe estar en modo de depuración para mostrar las expresiones locales.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Lista de tareas

**Para tener acceso a la ventana Variables locales**
  
-   En el menú **Depurar** , haga clic en **Ventanas** y, a continuación, haga clic en **Variables locales**.  
  
 **Para cambiar el valor de una expresión**  
  
-   Haga clic con el botón derecho en la expresión y, después, seleccione **Editar valor**.  
  
## <a name="columns"></a>Columnas  
 **Nombre**  
 Es el nombre de la expresión local. El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] muestra las variables, los parámetros y las funciones de sistema cuyos nombres comienzan por @@.  
  
 **Valor**  
 Muestra el valor que está asignado actualmente a la expresión local. Esta columna está en blanco cuando no se ha asignado ningún valor a la expresión.  
  
 Si la longitud de una expresión es mayor que el ancho de la columna **Valor** , una información sobre herramientas muestra el valor completo al mover el puntero sobre la celda **Valor** para esa expresión.  
  
 Un icono de lupa en una celda **Valor** indica que el visualizador del depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] está disponible. En la lista, puede especificar **Visualizador de texto**, **Visualizador XML** o **Visualizador HTML**. Para iniciar un visualizador del depurador, haga clic en el icono de lupa. El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] abre un cuadro de diálogo que muestra los datos en un formato adecuado al tipo de datos.  
  
 **Tipo**  
 Muestra el tipo de datos de la expresión.  
  
## <a name="see-also"></a>Consulte también  
 [Depurador de Transact-SQL](./transact-sql-debugger.md)   
 [Ver información del depurador de Transact-SQL](./transact-sql-debugger-information.md)   
 [Ventana de inspección](./transact-sql-debugger-watch-window.md)   
 [Ventana de pila de llamadas](./transact-sql-debugger-call-stack-window.md)   
 [Cuadro de diálogo Inspección rápida](./transact-sql-debugger-quickwatch-dialog-box.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)