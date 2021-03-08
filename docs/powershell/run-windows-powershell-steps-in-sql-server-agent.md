---
title: Ejecutar los pasos Windows PowerShell del Agente SQL Server
description: Obtenga información sobre cómo ejecutar los pasos de Windows PowerShell en un trabajo de Agente SQL Server.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.openlocfilehash: 8029d18dee3dd49342c3c029fc78992900394ed4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839413"
---
# <a name="run-windows-powershell-steps-in-sql-server-agent"></a>Ejecutar los pasos Windows PowerShell del Agente SQL Server

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

Use el Agente SQL Server para ejecutar scripts de SQL Server PowerShell en horas programadas.

[!INCLUDE[sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

[!INCLUDE[sql-server-powershell-no-sqlps](../includes/sql-server-powershell-no-sqlps.md)]

## <a name="to-run-powershell-from-sql-server-agent"></a>Para ejecutar PowerShell desde el Agente SQL Server

Hay varios tipos de pasos de trabajo del Agente SQL Server. Cada tipo se asocia a un subsistema que implementa un entorno concreto, como un agente de replicación o un entorno del símbolo del sistema. Puede codificar scripts de Windows PowerShell y, a continuación, utilizar el Agente SQL Server para incluir los scripts en trabajos que se ejecuten en los momentos programados o como respuesta a eventos de SQL Server. Los scripts de Windows PowerShell pueden ejecutarse mediante un paso de trabajo del símbolo del sistema o un paso de trabajo de PowerShell.

- Use un paso de trabajo de PowerShell para que el subsistema del Agente SQL Server ejecute la utilidad **sqlps**, que inicia PowerShell e importa el módulo **sqlps**. Si está ejecutando SQL Server 2019 o una versión posterior, se recomienda usar el módulo **[SqlServer](sql-server-powershell.md#sql-server-agent)** en el paso de trabajo del Agente SQL.

- Use un paso de trabajo de símbolo del sistema para ejecutar PowerShell.exe y especificar un script que importa el módulo **sqlps** .

### <a name="caution-about-memory-consumption"></a><a name="LimitationsRestrictions"></a> Precaución sobre el consumo de memoria

Cada paso de trabajo del Agente SQL Server que ejecuta PowerShell con el módulo **sqlps** inicia un proceso que consume aproximadamente **20 MB** de memoria. Si ejecuta muchos pasos de trabajo de Windows PowerShell simultáneos, el rendimiento se puede ver afectado adversamente.

## <a name="create-a-powershell-job-step"></a><a name="PShellJob"></a> Crear un paso de trabajo de PowerShell

### <a name="to-create-a-powershell-job-step"></a>Para crear un paso de trabajo de PowerShell

1. Expanda el **Agente SQL Server**, cree un trabajo o haga clic con el botón derecho en uno existente y, después, seleccione **Propiedades**. Para obtener más información acerca de cómo crear un trabajo, vea [Crear trabajos](../ssms/agent/create-jobs.md).

2. En el cuadro de diálogo **Propiedades del trabajo**, seleccione la página **Pasos** y, a continuación, **Nuevo**.

3. En el cuadro de diálogo **Nuevo paso de trabajo** , escriba un nombre para el paso de trabajo en **Nombre del paso**.

4. En la lista **Tipo**, seleccione **PowerShell**.

5. En la lista **Ejecutar como** , seleccione la cuenta de proxy con las credenciales que utilizará el trabajo.

6. En el cuadro **Comando** , especifique la sintaxis del script de PowerShell que se ejecutará para el paso de trabajo. También puede seleccionar **Abrir** y elegir un archivo que contenga la sintaxis del script.

7. Seleccione la página **Avanzadas** para establecer las siguientes opciones de paso de trabajo: acción que se llevará a cabo si el paso de trabajo progresa o no, número de veces que el Agente SQL Server debe intentar ejecutar el paso de trabajo y frecuencia de los intentos.

## <a name="create-a-command-prompt-job-step"></a><a name="CmdExecJob"></a> Crear un paso de trabajo del símbolo del sistema

### <a name="to-create-a-cmdexec-job-step"></a>Para crear un paso de trabajo de CmdExec

1. Expanda el **Agente SQL Server**, cree un trabajo o haga clic con el botón derecho en uno existente y, después, seleccione **Propiedades**. Para obtener más información acerca de cómo crear un trabajo, vea [Crear trabajos](../ssms/agent/create-jobs.md).

2. En el cuadro de diálogo **Propiedades del trabajo**, seleccione la página **Pasos** y, a continuación, **Nuevo**.

3. En el cuadro de diálogo **Nuevo paso de trabajo** , escriba un nombre para el paso de trabajo en **Nombre del paso**.

4. En la lista **Tipo** , elija **Sistema operativo (CmdExec)** .

5. En la lista **Ejecutar como** , seleccione la cuenta e proxy con las credenciales que utilizará el trabajo. De forma predeterminada, los pasos de trabajo de CmdExec se ejecutan en el contexto de la cuenta de servicio de Agente SQL Server.

6. En el cuadro **Procesar código de salida de un comando correcto** , escriba un valor de 0 a 999999.

7. En el cuadro **Comando** , escriba powershell.exe con parámetros que especifiquen el script de PowerShell que se va a ejecutar.

8. Seleccione la página **Avanzadas** para configurar las opciones del paso de trabajo como, por ejemplo: la acción que se realizará si el paso de trabajo es correcto o si es erróneo, el número de veces que Agente SQL Server intentará ejecutar el paso de trabajo y el archivo en el que Agente SQL Server puede escribir la salida del paso de trabajo. Solo los miembros del rol fijo de servidor **sysadmin** pueden escribir la salida de paso de trabajo en un archivo del sistema operativo.

## <a name="see-also"></a>Consulte también

- [SQL Server PowerShell](sql-server-powershell.md)
- [Agente SQL Server con PowerShell](sql-server-powershell.md#sql-server-agent)