---
title: Uso de NOSQLPS para el Agente SQL Server con PowerShell
description: Mensaje para explicar cómo usar el cmdlet de sqlserver de PowerShell en lugar del cmdlet sqlps con el Agente SQL Server
ms.topic: include
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier
ms.openlocfilehash: bf161bd583f3d48dc389cea6b69f55c32e2cce59
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839414"
---
A partir de SQL Server 2019, puede deshabilitar SQLPS. En la primera línea de un paso de trabajo del tipo PowerShell puede agregar `#NOSQLPS`, que impide que el Agente SQL cargue de forma automática el módulo SQLPS. Ahora, el trabajo del Agente SQL ejecutará la versión de PowerShell instalada en el equipo y, después, podrá usar cualquier otro módulo de PowerShell que quiera.

Para usar el [**módulo SqlServer**](https://www.powershellgallery.com/packages/Sqlserver/21.1.18235) en el paso de trabajo del Agente SQL, puede colocar este código en las dos primeras líneas del script.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```