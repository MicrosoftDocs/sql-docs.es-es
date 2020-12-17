---
description: Controlar varios pasos del trabajo
title: Controlar varios pasos del trabajo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- job steps [SQL Server Agent]
- ordering job steps [SQL Server]
- multiple job steps
- SQL Server Agent jobs, job steps
- control of flow for jobs [SQL Server]
ms.assetid: 7aba19ff-72b3-45f6-8e54-23f4988d63a8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 1bb3a13c67e00fd9da9b9ca48bab95b21e46769a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474456"
---
# <a name="handle-multiple-job-steps"></a>Controlar varios pasos del trabajo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Si el trabajo está formado por varios pasos, debe especificar el orden de ejecución de los pasos del trabajo. Esto se denomina *control de flujo.* En cualquier momento puede agregar nuevos pasos del trabajo y reorganizar el flujo de los pasos; los cambios se aplicarán la próxima vez que se ejecute el trabajo. Esta ilustración muestra el control de flujo de un trabajo de copia de seguridad de una base de datos.  
  
![Control de flujo de los pasos de trabajo del Agente SQL Server](../../ssms/agent/media/dbflow01.gif "Control de flujo de los pasos de trabajo del Agente SQL Server")  
  
El primer paso es Copia de seguridad de la base de datos. Si este paso genera un error, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] informa del error al operador que se ha definido que reciba la notificación. Si el paso Copia de seguridad de la base de datos es correcto, el trabajo pasa al siguiente paso: "Normalizar" los datos del cliente. Si este paso genera un error, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avanza a Restaurar base de datos. Si "Normalizar" los datos del cliente es correcto, el trabajo avanza al siguiente paso: Actualizar estadísticas y, así sucesivamente, hasta que el paso final da como resultado un informe correcto o con errores.  
  
Se define una acción de control de flujo para la ejecución satisfactoria o con errores de cada paso del trabajo. Debe especificar una acción que se deberá realizar cuando un paso del trabajo se ejecute correctamente y la acción que se llevará a cabo cuando se ejecute con errores. También puede definir el número de reintentos y el intervalo entre ellos para los pasos del trabajo que no se han ejecutado correctamente.  
  
> [!NOTE]  
> Cuando utiliza la interfaz gráfica de usuario (GUI) del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y se elimina uno o varios pasos de un trabajo de varios pasos, la GUI quita todos los pasos del trabajo y vuelve a agregar los pasos restantes con las referencias correctas en caso de éxito o en caso de error. Por ejemplo, imagine que tiene un trabajo de cinco pasos en el que el primero se configura para que salte al cuarto paso si se completa con éxito. Si elimina el tercer paso, la GUI elimina todos los pasos del trabajo y agrega los cuatro pasos restantes (1, 2, 4 y 5) con las referencias corregidas. En este caso, la referencia del primer paso se volvería a configurar para que salte al tercer paso si el primero se completa con éxito.  
  
Los pasos del trabajo deben ser independientes. Es decir, un trabajo no puede pasar datos, valores booleanos o numéricos entre pasos del trabajo. Sin embargo, puede pasar valores de un paso del trabajo de [!INCLUDE[tsql](../../includes/tsql-md.md)] a otro si utiliza tablas permanentes o tablas temporales globales. También puede pasar valores de pasos del trabajo que ejecuten programas ejecutables de un paso del trabajo a otro mediante archivos. Por ejemplo, el programa ejecutado mediante un paso del trabajo escribe un archivo y el programa ejecutado por un paso del trabajo posterior lee el archivo.  
  
> [!NOTE]  
> Si crea pasos del trabajo en bucle (el paso del trabajo 1 va seguido del paso del trabajo 2 y, a continuación, del paso del trabajo 2 se vuelve al paso del trabajo 1), aparecerá un mensaje de advertencia cuando cree el trabajo mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente registra la información del trabajo y de los pasos de trabajo en el historial de trabajos.  
  
## <a name="see-also"></a>Consulte también  
[sp_add_job](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
[sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
[sysjobs (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
[sysjobsteps](../../relational-databases/system-tables/dbo-sysjobsteps-transact-sql.md)  
[Implementar trabajos](../../ssms/agent/implement-jobs.md)  
[Administrar pasos de trabajo](../../ssms/agent/manage-job-steps.md)  
