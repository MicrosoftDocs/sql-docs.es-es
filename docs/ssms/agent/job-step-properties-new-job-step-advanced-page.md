---
title: Propiedades de nuevo paso de trabajo (página Opciones avanzadas)
description: Propiedades de paso de trabajo - Nuevo paso de trabajo (página Opciones avanzadas)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.job.stepadvanced.f1
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2120c2f5d86c4d8ad4c549f95ba8090574506015
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978851"
---
# <a name="job-step-properties---new-job-step-advanced-page"></a>Propiedades de paso de trabajo - Nuevo paso de trabajo (página Opciones avanzadas)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Use esta página para ver y cambiar las propiedades de un paso de trabajo del Agente [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="options"></a>Opciones  
**Acción en caso de éxito**  
Establece la acción que debe realizar el Agente [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] si el paso de trabajo se realiza correctamente.  
  
**Número de reintentos**  
Establece el número de veces que el Agente [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] intenta volver a ejecutar un paso de trabajo con error.  
  
**Intervalo de reintento (minutos)**  
Establece el tiempo que espera el Agente [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] entre los reintentos.  
  
**Acción en caso de error**  
Establece la acción que debe realizar el Agente [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] si el paso de trabajo no se realiza correctamente.  
  
## <a name="options-for-transact-sql-job-steps"></a>Opciones de pasos de trabajo Transact-SQL  
**Archivo de salida**  
Establece el archivo que se utiliza para la salida desde el paso de trabajo. Esta opción solo está disponible para los miembros del rol fijo de servidor **sysadmin** .  
  
**...**  
Permite buscar el archivo que se utiliza para la salida desde el paso de trabajo.  
  
**Vista**  
En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], este botón está deshabilitado para ver los archivos de salida. En su lugar, use el Bloc de notas para ver los archivos de salida de paso de trabajo.  
  
**Anexar la salida al archivo existente**  
Anexa la salida al contenido existente del archivo. De lo contrario, el anterior contenido del archivo se sobrescribe cada vez que se ejecuta el paso de trabajo.  
  
**Registro en tabla**  
Registra la salida del paso de trabajo en la tabla **sysjobstepslogs** de la base de datos **msdb**.  
  
**Vista**  
Después de ejecutar el paso de trabajo al menos una vez, seleccione **Ver** para ver su salida en la tabla.  
  
**Anexar salida a la entrada existente de la tabla**  
Anexa la salida al contenido existente de la tabla. De lo contrario, el anterior contenido de la tabla se sobrescribe cada vez que se ejecuta el paso de trabajo.  
  
**Incluir salida de paso en historial**  
Seleccione esta opción para incluir la salida del paso de trabajo en el historial de trabajos.  
  
**Ejecutar como usuario**  
Si es miembro del rol fijo de servidor **sysadmin**, puede seleccionar otro inicio de sesión de SQL para ejecutar este paso de trabajo.  
  
## <a name="options-for-operating-system-cmdexec-job-steps"></a>Opciones de pasos de trabajo del sistema operativo (CmdExec)  
**Archivo de salida**  
Establece el archivo que se utiliza para la salida desde el paso de trabajo.  
  
**...**  
Permite buscar el archivo que se utiliza para la salida desde el paso de trabajo.  
  
**Vista**  
En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], este botón está deshabilitado para ver los archivos de salida. En su lugar, use el Bloc de notas para ver los archivos de salida de paso de trabajo.  
  
**Anexar la salida al archivo existente**  
Anexa la salida del paso de trabajo al contenido del archivo anterior cada vez que se ejecuta.  
  
**Registro en tabla**  
Registra la salida del paso de trabajo en la tabla **sysjobstepslogs** de la base de datos **msdb**.  
  
**Vista**  
Después de ejecutar el paso de trabajo al menos una vez, seleccione **Ver** para ver su salida en la tabla.  
  
**Anexar salida a la entrada existente de la tabla**  
Anexa la salida al contenido existente de la tabla. De lo contrario, el anterior contenido de la tabla se sobrescribe cada vez que se ejecuta el paso de trabajo.  
  
**Incluir salida de paso en historial**  
Seleccione esta opción para incluir la salida del paso de trabajo en el historial de trabajos.  
  
## <a name="options-for-powershell-job-steps"></a>Opciones de pasos de trabajo de PowerShell  
**Archivo de salida**  
Establece el archivo que se utiliza para la salida desde el paso de trabajo.  
  
**...**  
Permite buscar el archivo que se utiliza para la salida desde el paso de trabajo.  
  
**Vista**  
En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], este botón está deshabilitado para ver los archivos de salida. En su lugar, use el Bloc de notas para ver los archivos de salida de paso de trabajo.  
  
**Anexar la salida al archivo existente**  
Anexa la salida del paso de trabajo al contenido del archivo anterior cada vez que se ejecuta.  
  
**Registro en tabla**  
Registra la salida del paso de trabajo en la tabla **sysjobstepslogs** de la base de datos **msdb**.  
  
**Vista**  
Después de ejecutar el paso de trabajo al menos una vez, seleccione **Ver** para ver su salida en la tabla.  
  
**Anexar salida a la entrada existente de la tabla**  
Anexa la salida al contenido existente de la tabla. De lo contrario, el anterior contenido de la tabla se sobrescribe cada vez que se ejecuta el paso de trabajo.  
  
**Incluir salida de paso en historial**  
Seleccione esta opción para incluir la salida del paso de trabajo en el historial de trabajos.  
  
## <a name="options-for-replication-queue-reader-job-steps"></a>Opciones de pasos de trabajo del Lector de cola de replicación  
**Server**  
Establece el servidor que se utiliza en un paso de trabajo de un lector de cola de replicación.  
  
**Base de datos**  
Establece la base de datos que se utiliza en un paso de trabajo de un lector de cola de replicación.  
  
## <a name="options-for-sql-server-analysis-services-job-steps"></a>Opciones de pasos de trabajo de SQL Server Analysis Services  
**Archivo de salida**  
Establece el archivo que se utiliza para la salida desde el paso de trabajo. Esta opción solo está disponible para los miembros del rol fijo de servidor **sysadmin** .  
  
**...**  
Permite buscar el archivo que se utiliza para la salida desde el paso de trabajo.  
  
**Vista**  
En [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], este botón está deshabilitado para ver los archivos de salida. En su lugar, use el Bloc de notas para ver los archivos de salida de paso de trabajo.  
  
**Anexar la salida al archivo existente**  
Anexa la salida al contenido existente del archivo. De lo contrario, el anterior contenido del archivo se sobrescribe cada vez que se ejecuta el paso de trabajo.  
  
**Registro en tabla**  
Registra la salida del paso de trabajo en la tabla **sysjobstepslogs** de la base de datos **msdb**.  
  
**Vista**  
Después de ejecutar el paso de trabajo al menos una vez, seleccione **Ver** para ver su salida en la tabla.  
  
**Anexar salida a la entrada existente de la tabla**  
Anexa la salida al contenido existente de la tabla. De lo contrario, el anterior contenido de la tabla se sobrescribe cada vez que se ejecuta el paso de trabajo.  
  
**Incluir salida de paso en historial**  
Seleccione esta opción para incluir la salida del paso de trabajo en el historial de trabajos.  
  
## <a name="see-also"></a>Consulte también

- [Administrar pasos de trabajo](manage-job-steps.md)
