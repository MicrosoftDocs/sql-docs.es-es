---
title: Ejecutar el depurador de Transact-SQL
description: Obtenga información sobre cómo personalizar el depurador de Transact-SQL y cómo usarlo para depurar el código de Transact-SQL. Puede ejecutar el depurador en una instancia del Motor de base de datos que se encuentra en otro equipo.
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, sysadmin requirement
- Transact-SQL debugger, supported versions
- Query Editor [Database Engine], right-click menu
- debugging [SQL Server], T-SQL debugger
- Transact-SQL debugger, Query Editor shortcut menu
- Transact-SQL debugger, stopping
- Transact-SQL debugger, Debug menu
- debugging [SQL Server]
- Transact-SQL debugger, Debug toolbar
- Transact-SQL debugger, keyboard shortcuts
- Transact-SQL debugger, starting
ms.assetid: 386f6d09-dbec-4dc7-9e8a-cd9a4a50168c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cb075a71ec6a4004de4691a53b8b487eadbd65c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354536"
---
# <a name="run-the-transact-sql-debugger"></a>Ejecutar el depurador de Transact-SQL

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Puede iniciar el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] después de abrir una ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] . A continuación, puede ejecutar el código de [!INCLUDE[tsql](../../includes/tsql-md.md)] en modo de depuración hasta que se detenga el depurador. Puede establecer las opciones para personalizar cómo se ejecuta el depurador.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="starting-and-stopping-the-debugger"></a>Iniciar y detener el depurador

Los requisitos para iniciar el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] son los siguientes:

- Si el Editor de consultas del [!INCLUDE[ssDE](../../includes/ssde-md.md)] está conectado a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] en otro equipo, debe haber configurado el depurador para la depuración remota. Para obtener más información, vea [Configurar reglas de firewall antes de ejecutar al depurador de TSQL](./configure-firewall-rules-before-running-the-tsql-debugger.md).
  
- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] se debe ejecutar con una cuenta de Windows que sea miembro del rol fijo de servidor sysadmin.

- La ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] se debe conectar mediante el uso de un inicio de sesión de autenticación de Windows o de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que sea miembro del rol fijo de servidor sysadmin.
  
- La ventana del Editor de consultas de [!INCLUDE[ssDE](../../includes/ssde-md.md)] debe estar conectada a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] desde [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2 (SP2) o posterior. No puede ejecutar el depurador cuando la ventana del Editor de consultas esté conectada a una instancia cuyo modo sea de usuario único.  
  
 Recomendamos que el código [!INCLUDE[tsql](../../includes/tsql-md.md)] se depure en un servidor de prueba, no en un servidor de producción, por las siguientes razones:
  
- La depuración es una operación con muchos privilegios. Por consiguiente, solo los miembros del rol fijo de servidor sysadmin pueden depurar en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
- Las sesiones de depuración se suelen ejecutar durante períodos de tiempo prolongados mientras se investiga el funcionamiento de varias instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] . Los bloqueos, como los bloqueos de actualización, que adquiere la sesión se pueden mantener durante períodos prolongados, hasta que finalice la sesión o se confirme o revierta la transacción.  
  
 Al iniciarse el depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] , la ventana del Editor de consultas se pone en el modo de depuración. Cuando la ventana del Editor de consultas entra en el modo de depuración, el depurador se detiene en la primera línea de código. Luego, puede recorrer el código, detener la ejecución en instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] específicas y usar las ventanas del depurador para ver el estado de ejecución actual. Puede iniciar el depurador haciendo clic en el botón **Depurador** de la barra de herramientas **Consulta** o en la opción **Iniciar depuración** del menú **Depurar** .  
  
 La ventana del Editor de consultas permanece en estado de depuración hasta que finalice la última instrucción de dicha ventana o se detenga el modo de depuración. Puede detener el modo de depuración y la ejecución de la instrucción mediante uno de los siguientes métodos:  
  
- En el menú **Depurar** , haga clic en **Detener depuración**.  
  
- En la barra de herramientas **Depurar** , haga clic en el botón **Detener depuración** .  
  
- En el menú **Consulta** , haga clic en **Cancelar ejecución de la consulta**.  
  
- En la barra de herramientas **Consulta** , haga clic en el botón **Cancelar ejecución de la consulta** .  
  
 También puede detener el modo de depuración y dejar que finalice la ejecución del resto de las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] haciendo clic en la opción **Desasociar todo** del menú **Depurar** .  
  
## <a name="controlling-the-debugger"></a>Controlar el depurador

 Puede controlar el funcionamiento del depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] usando los siguiente comandos de menú, barras de herramientas y accesos directos:  
  
- El menú **Depurar** y la barra de herramientas **Depurar** . Tanto el menú **Depurar** como la barra de herramientas **Depurar** están inactivos hasta que el foco se coloque en una ventana abierta del Editor de consultas. Permanecen activas hasta que se cierre el proyecto actual.  
  
- Los métodos abreviados de teclado del depurador.  
  
- El menú contextual de Editor de consultas. El menú contextual se muestra cuando se hace clic con el botón secundario en una línea de una ventana del Editor de consultas. Cuando esta ventana esté en el modo de depuración, el menú contextual muestra los comandos del depurador que se aplican a la línea o cadena seleccionadas.  
  
- Los elementos de menú y los comandos contextuales de las ventanas que abre el depurador, como las ventanas **Inspección** o **Puntos de interrupción** .  
  
 La siguiente tabla muestra los comandos de menú, los botones de las barras de herramientas y los métodos abreviados de teclado del depurador.  
  
|Comando del menú Depurar|Comando de método abreviado del editor|Botón de la barra de herramientas|Métodos abreviados de teclado|Acción|  
|------------------------|-----------------------------|--------------------|-----------------------|------------|  
|**Windows/Puntos de interrupción**|No disponible|**Puntos de interrupción**|CTRL+ALT+B|Mostrar la ventana **Puntos de interrupción** en la que puede ver y administrar los puntos de interrupción.|  
|**Windows/Inspección/Inspección1**|No disponible|**Puntos de interrupción/Inspección/Inspección1**|CTRL+ALT+W, 1|Mostrar la ventana **Inspección1** .|  
|**Windows/Inspección/Inspección2**|No disponible|**Puntos de interrupción/Inspección/Inspección2**|CTRL+ALT+W, 2|Mostrar la ventana **Inspección2** .|  
|**Windows/Inspección/Inspección3**|No disponible|**Puntos de interrupción/Inspección/Inspección3**|CTRL+ALT+W, 3|Mostrar la ventana **Inspección3** .|  
|**Windows/Inspección/Inspección4**|No disponible|**Puntos de interrupción/Inspección/Inspección4**|CTRL+ALT+W, 4|Mostrar la ventana **Inspección4** .|  
|**Windows/Variables locales**|No disponible|**Puntos de interrupción/Variables locales**|CTRL+ALT+V, L|Mostrar la ventana **Variables locales** .|  
|**Windows/Pila de llamadas**|No disponible|**Puntos de interrupción/Pila de llamadas**|CTRL+ALT+C|Mostrar la ventana **Pila de llamadas** .|  
|**Windows/Subprocesos**|No disponible|**Puntos de interrupción/Subprocesos**|CTRL+ALT+H|Mostrar la ventana **Subprocesos** .|  
|**Continuar**|No disponible|**Continuar**|ALT+F5|Ejecutar un proceso hasta el siguiente punto de interrupción. La opción **Continuar** no está activa hasta que coloque el foco en una ventana del Editor de consultas que esté en el modo de depuración.|  
|**Iniciar depuración**|No disponible|**Iniciar depuración**|ALT+F5|Poner una ventana del Editor de consultas en el modo de depuración y ejecutar hasta el primer punto de interrupción. Si el foco está en la ventana del Editor de consultas que está en el modo de depuración, la opción **Iniciar depuración** se reemplaza por **Continuar**.|  
|**Interrumpir todos**|No disponible|**Interrumpir todos**|CTRL+ALT+INTERR|El depurador de [!INCLUDE[tsql](../../includes/tsql-md.md)] no usa esta característica.|  
|**Detener la depuración**|No disponible|**Detener la depuración**|MAYÚS+F5|Sacar una ventana del Editor de consultas del modo de depuración y volver a ponerla en el modo normal.|  
|**Desasociar todo**|No disponible|No disponible|No disponible|Detener el modo de depuración, pero se ejecutan las instrucciones restantes en la ventana del Editor de consultas.|  
|**Depurar paso a paso por instrucciones**|No disponible|**Depurar paso a paso por instrucciones**|F11|Ejecutar la siguiente instrucción y, además, abrir una nueva ventana del Editor de consultas en el modo de depuración si la siguiente instrucción ejecuta un procedimiento almacenado, un desencadenador o una función.|  
|**Depurar paso a paso por procedimientos**|No disponible|**Depurar paso a paso por procedimientos**|F10|Igual que **Paso a paso por instrucciones**, salvo que no se depuran las funciones, los procedimientos almacenados ni los desencadenadores.|  
|**Depurar paso a paso para salir**|No disponible|**Depurar paso a paso para salir**|MAYÚS+F11|Ejecutar el resto del código de un desencadenador, función o procedimiento almacenado sin detenciones por puntos de interrupción. Se reanuda el modo de depuración normal cuando el control se devuelva al código que llamó al módulo.|  
|No disponible|**Ejecutar hasta el cursor**|No disponible|CTRL+F10|Ejecutar todo el código desde la última ubicación de detención hasta la actual sin detenerse en ningún punto de interrupción.|  
|**inspección rápida**|**inspección rápida**|No disponible|CTRL+ALT+Q|Mostrar la ventana **Inspección rápida** .|  
|**Alternar punto de interrupción**|**Punto de interrupción/Insertar punto de interrupción**|No disponible|F9|Colocar un punto de interrupción en la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] actual o seleccionada.|  
|No disponible|**Punto de interrupción/Eliminar punto de interrupción**|No disponible|No disponible|Eliminar el punto de interrupción de la línea seleccionada.|  
|No disponible|**Punto de interrupción/Deshabilitar punto de interrupción**|No disponible|No disponible|Deshabilitar el punto de interrupción en la línea seleccionada. El punto de interrupción permanece en la línea de código, pero no detendrá la ejecución hasta que se rehabilite.|  
|No disponible|**Punto de interrupción/Habilitar puntos de interrupción**|No disponible|No disponible|Habilitar el punto de interrupción en la línea seleccionada.|  
|**Eliminar todos los puntos de interrupción**|No disponible|No disponible|CTRL+MAYÚS+F9|Eliminar todos los puntos de interrupción.|  
|**Deshabilitar todos los puntos de interrupción**|No disponible|No disponible|No disponible|Deshabilitar todos los puntos de interrupción.|  
|No disponible|**Agregar inspección**|No disponible|No disponible|Agregar la expresión seleccionada a la ventana **Inspección** .|  
  
## <a name="see-also"></a>Consulte también

- [Depurador de Transact-SQL](./transact-sql-debugger.md)
- [Avanzar paso a paso por el código Transact-SQL](./step-through-transact-sql-code.md)
- [Ver información del depurador de Transact-SQL](./transact-sql-debugger-information.md)
- [Editor de consultas del motor de base de datos &#40;SQL Server Management Studio&#41;](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Estadísticas de consultas activas](../../relational-databases/performance/live-query-statistics.md)