---
title: Especificar una acción del punto de interrupción
description: 'Obtenga información sobre cómo especificar una acción de detección: una tarea personalizada para el depurador de Transact-SQL que se realizará cuando se alcance un punto de interrupción y se cumplan otras condiciones concretas.'
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint action
- Transact-SQL debugger, breakpoint when hit action
ms.assetid: f97f0097-6f51-40c1-b2e0-294a93ce1e1b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 42902b55150c4efe5d9c81eb5698b3cff59b7aab
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344147"
---
# <a name="specify-a-breakpoint-action"></a>Especificar una acción del punto de interrupción

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Las acciones **Cuando se llama** de punto de interrupción especifican una tarea personalizada que el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] realiza en un punto de interrupción. Si se alcanza el número de llamadas especificado y se satisface la condición de punto de interrupción especificada, el depurador realiza la acción definida para el punto de interrupción.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
##  <a name="action-considerations"></a><a name="BKMK_ActionConsiderations"></a> Consideraciones sobre las acciones

La acción predeterminada para un punto de interrupción es que se detenga la ejecución cuando se hayan satisfecho el número de llamadas y la condición de punto de interrupción. El uso principal de una acción **Cuando se llama** en el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] consiste, sin embargo, en imprimir información en la **ventana de salida** del depurador, donde se especificará un mensaje de impresión.  
  
Los mensajes de impresión se especifican en la opción **Imprimir un mensaje** como una cadena de texto que incluye expresiones con información de la instancia de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se está depurando. Estas expresiones pueden ser:  
  
-   Una expresión [!INCLUDE[tsql](../../includes/tsql-md.md)] escrita entre llaves ({}). Las expresiones pueden incluir variables, parámetros y funciones integradas de [!INCLUDE[tsql](../../includes/tsql-md.md)] . Algunos ejemplos son {@MyVariable}, {@NameParameter}, {@@SPID} o {SERVERPROPERTY('ProcessID')}.  
  
-   Una de las siguientes palabras clave:  
  
    1.  $ADDRESS devuelve el nombre del procedimiento almacenado o la función definida por el usuario donde se ha establecido el punto de interrupción. Si el punto de interrupción está establecido en la ventana del editor, $ADDRESS devuelve el nombre del archivo de script que se está editando. $ADDRESS y $FUNCTION devuelven la misma información en el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
    2.  $CALLER devuelve el nombre de la unidad de código de [!INCLUDE[tsql](../../includes/tsql-md.md)] encargada de llamar a un procedimiento almacenado o una función. Si el punto de interrupción se encuentra en la ventana del editor, $CALLER devuelve \<No caller available>. Si el punto de interrupción es un procedimiento almacenado o una función definida por el usuario que se ha llamado desde el código de la ventana del editor, $CALLER devuelve el nombre del archivo que se está editando. Si el punto de interrupción es un procedimiento almacenado o una función definida por el usuario que se ha llamado desde otro procedimiento almacenado o función, $CALLER devuelve el nombre del procedimiento o la función de llamada.  
  
    3.  $CALLSTACK devuelve la pila de llamadas de las funciones de la cadena que llamaron al procedimiento almacenado o la función definida por el usuario actual. Si el punto de interrupción está establecido en la ventana del editor, $CALLSTACK devuelve en nombre del archivo de script que se está editando.  
  
    4.  $FUNCTION devuelve el nombre del procedimiento almacenado o la función definida por el usuario donde se ha establecido el punto de interrupción. Si el punto de interrupción está establecido en la ventana del editor, $FUNCTION devuelve en nombre del archivo de script que se está editando.  
  
    5.  $PID y $PNAME devuelven el identificador y el nombre del proceso del sistema operativo que está ejecutando la instancia del motor de base de datos en la que se está ejecutando [!INCLUDE[tsql](../../includes/tsql-md.md)] . $PID devuelve el mismo identificador que SERVERPROPERTY('ProcessID'), salvo porque $PID es un valor hexadecimal mientras que SERVERPROPERTY('ProcessID') es un valor decimal.  
  
    6.  $TID y $TNAME devuelven el identificador y el nombre del subproceso del sistema operativo en el que se está ejecutando el lote [!INCLUDE[tsql](../../includes/tsql-md.md)] . El subproceso está asociado al proceso que ejecuta la instancia del motor de base de datos. $TID devuelve el mismo valor que SELECT kpid FROM sys.sysprocesses WHERE spid = @@SPID, salvo que $TID es un valor hexadecimal y kpid es un valor decimal.  
  
-   También puede usar el carácter de barra diagonal inversa (\\) como carácter de escape para permitir el uso de llaves y barras diagonales inversas en el mensaje: \\{, \\} y \\\\.  
  
#### <a name="to-specify-a-when-hit-action"></a>Para especificar una acción Cuando se llama  
  
1.  En la ventana del editor, haga clic con el botón derecho en el glifo de punto de interrupción y, en el menú contextual, haga clic en **Cuando se llama** .  
  
     O bien  
  
     En la ventana **Puntos de interrupción** , haga clic con el botón derecho en el glifo de punto de interrupción y, en el menú contextual, haga clic en **Cuando se llama** .  
  
2.  En el cuadro de diálogo **Cuando el punto de interrupción se visita** , seleccione el comportamiento que desee:  
  
    1.  Seleccione **Imprimir un mensaje** para imprimir un mensaje en la ventana de salida del depurador cuando se alcance el punto de interrupción.  
  
    2.  La opción **Ejecutar una macro** no está disponible en el depurador [!INCLUDE[tsql](../../includes/tsql-md.md)] y aparece deshabilitada.  
  
    3.  Seleccione **Continuar ejecución** si no desea que el punto de interrupción detenga la ejecución. Esta opción solo está activa si se ha seleccionado la opción **Imprimir un mensaje** .  
  
3.  Haga clic en **Aceptar** para implementar los cambios o en **Cancelar** para salir sin aplicar los cambios.  
  
## <a name="see-also"></a>Consulte también  
 [Especificar una condición de punto de interrupción](./specify-a-breakpoint-condition.md)   
 [Especificar un número de llamadas](./specify-a-hit-count.md)