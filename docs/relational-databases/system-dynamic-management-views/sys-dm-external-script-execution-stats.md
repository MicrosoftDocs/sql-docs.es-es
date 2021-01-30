---
description: sys.dm_external_script_execution_stats
title: sys.dm_external_script_execution_stats | Microsoft Docs
ms.custom: ''
ms.date: 06/25/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: reference
f1_keywords:
- sys.dm_external_script_execution_stats
- sys.dm_external_script_execution_stats_TSQL
- dm_external_script_execution_stats
- dm_external_script_execution_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_external_script_execution_stats dynamic management view
ms.assetid: 2e99f026-ceb2-42a2-a549-c71d31ed0cf4
author: dphansen
ms.author: davidph
manager: cgronlun
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 2885f83185218d389d2a6fcf7dce6c3fbd097286
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201343"
---
# <a name="sysdm_external_script_execution_stats"></a>sys.dm_external_script_execution_stats
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Devuelve una fila por cada tipo de solicitud de script externo. Las solicitudes de script externo se agrupan según el lenguaje compatible del script externo. Se genera una fila para cada función registrada de script externo. Las funciones de script externo arbitrarias no se registran a menos que las envíe un proceso principal, como `rxExec`.
  
> [!NOTE]  
> Esta vista de administración dinámica (DMV) solo está disponible si ha instalado y habilitado la característica que admite la ejecución de scripts externos. Para obtener más información, consulte [r Services en SQL Server 2016](../../machine-learning/r/sql-server-r-services.md), [Machine Learning Services (R, Python) en SQL Server 2017 y versiones posteriores](../../machine-learning/sql-server-machine-learning-services.md) y [Azure instancia administrada Machine Learning Services](/azure/azure-sql/managed-instance/machine-learning-services-overview).
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|language|**nvarchar**|Nombre del lenguaje de script externo registrado. Cada script externo debe especificar el lenguaje en la solicitud de script para iniciar el iniciador asociado. |  
|counter_name|**nvarchar**|Nombre de una función de script externo seleccionada. No admite valores NULL.|  
|counter_value|**integer**|Número total de instancias que en que se ha llamado la función registrada de script externo en el servidor. Este valor es acumulativo, comienza en el momento en que se instaló la característica en la instancia y no se puede restablecer.|  

## <a name="permissions"></a>Permisos

 Requiere el permiso VIEW SERVER STATE en el servidor.  
  
> [!NOTE]  
> Los usuarios que ejecuten scripts externos deben tener el permiso adicional EXECUTE ANY EXTERNAL SCRIPT, sin embargo, los administradores pueden usar esta DMV sin este permiso.
  
## <a name="remarks"></a>Observaciones

  Esta DMV se proporciona para la telemetría interna, a fin de supervisar el uso general de la nueva característica de ejecución de scripts externos que se proporciona en [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]. El servicio de telemetría comienza cuando LaunchPad comienza e incrementa un contador basado en disco cada vez que se llama una función registrada de script externo.

Por lo general, los contadores de rendimiento solo son válidos siempre que el proceso que los genera esté activo. Por lo tanto, una consulta en una DMV no puede mostrar datos detallados para servicios que ya no están en ejecución. Por ejemplo, si un iniciador ejecuta un script externo y aún lo completa muy rápidamente, es posible que una DMV convencional no muestre ningún dato.

Por lo tanto, los contadores a los que esta DMV hace seguimiento siguen en ejecución y se mantiene el estado de sys.dm_external_script_requests mediante escrituras en disco, incluso si la instancia está apagada.

### <a name="counter-values"></a>Valores de contador

En SQL Server 2016 el único lenguaje externo compatible es R y administra las solicitudes de script externo [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] . En SQL Server 2017 y versiones posteriores, y en Azure SQL Instancia administrada, R y Python son lenguajes externos compatibles y administra las solicitudes de script externo [!INCLUDE[rsql_productname_md](../../includes/rsql-productnamenew-md.md)] .

Para R, esta DMV realiza un seguimiento del número de llamadas de R que se realizan en una instancia. Por ejemplo, si se llama a `rxLinMod` y se ejecuta de manera simultánea, el contador aumenta en 1.

En el caso del lenguaje R, los valores del contador que aparecen en el campo *counter_name* representan los nombres de las funciones registradas de ScaleR. Los valores del campo *counter_value* representan los números acumulados de instancias de la función específica de ScaleR. 

En el caso de Python, esta DMV realiza un seguimiento del número de llamadas de Python realizadas en una instancia.

La cuenta comienza cuando se instala la característica y se habilita en la instancia, y se acumula hasta que un administrador sobrescribe o elimina el archivo que mantiene el estado. Por lo tanto, generalmente no es posible restablecer los valores en *counter_value*. Si desea supervisar el uso por sesión, tiempos calendario u otros intervalos, le recomendamos registrar los conteos en una tabla.

### <a name="registration-of-external-script-functions-in-r"></a>Registro de funciones de script externo en R

R admite scripts arbitrarios y la comunidad de R proporciona muchos miles de paquetes, cada uno con sus propias funciones y métodos. Sin embargo, esta DMV solo supervisa las funciones de ScaleR que se instalan con SQL Server 2016 R Services.

El registro de estas funciones se realiza cuando se instala la característica; además, no se pueden agregar ni borrar funciones registradas.

## <a name="examples"></a>Ejemplos  
  
### <a name="viewing-the-number-of-r-scripts-run-on-the-server"></a>Visualización del número de scripts de R en ejecución en el servidor

 En el ejemplo siguiente se muestra el número acumulado de ejecuciones de scripts externos para el lenguaje R.  
  
```sql
SELECT counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE language = 'R';
```  

### <a name="viewing-the-number-of-python-scripts-run-on-the-server"></a>Ver el número de scripts de Python que se ejecutan en el servidor

En el ejemplo siguiente se muestra el número acumulativo de ejecuciones de scripts externos para el lenguaje Python.  
  
```sql
SELECT counter_name, counter_value
FROM sys.dm_external_script_execution_stats
WHERE language = 'Python';
```  

## <a name="see-also"></a>Vea también

+ [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
+ [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)
+ [sys.dm_external_script_requests](../../relational-databases/system-dynamic-management-views/sys-dm-external-script-requests.md)  
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)  
