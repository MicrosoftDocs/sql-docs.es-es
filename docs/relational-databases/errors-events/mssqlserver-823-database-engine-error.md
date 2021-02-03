---
title: MSSQLSERVER error 823  | mssqlserver_823
description: Descripción del error 823 de Microsoft SQL Server (mssqlserver_823), que es una condición de error grave de nivel de sistema que amenaza la integridad de la base de datos y que se debe solucionar inmediatamente, y algunas soluciones comunes.
ms.custom: ''
ms.date: 01/27/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 823 (Database Engine error)
ms.assetid: 0d9fce3c-3772-46ce-a7a3-4f4988dc6cae
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7ed748bdf748aa51813d11348859bc769525ed35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197895"
---
# <a name="mssqlserver-error-823"></a>Error 823 de MSSQLSERVER
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|823|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|B_HARDERR|  
|Texto del mensaje|El sistema operativo ha devuelto el error %ls en SQL Server durante %S_MSG en el desplazamiento %#016I64x del archivo '%ls'. Encontrará más detalles en los mensajes adicionales del registro de errores de SQL Server y el registro de eventos del sistema. Se trata de una condición de error grave de nivel de sistema que amenaza a la integridad de la base de datos y que debe corregirse inmediatamente. Ejecute una comprobación de coherencia completa de la base de datos (DBCC CHECKDB). Este error puede deberse a muchos factores. Para obtener más información vea los Libros en pantalla de SQL Server.|  
  
## <a name="explanation"></a>Explicación  
SQL Server usa las API de Windows (por ejemplo, [ReadFile](/windows/win32/api/fileapi/nf-fileapi-readfile), [WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile), [ReadFileScatter](/windows/win32/api/fileapi/nf-fileapi-readfilescatter) y [WriteFileGather](/windows/win32/api/fileapi/nf-fileapi-writefilegather)) para realizar operaciones de E/S de archivos. Después de realizar estas operaciones de E/S, SQL Server busca condiciones de error asociadas a estas llamadas API. Si se producen un error del sistema operativo en las llamadas API, SQL Server notifica el error 823.

 El mensaje del error 823 contiene la siguiente información:
 - El archivo de base de datos en el que se ha realizado la operación de E/S.
 - El desplazamiento en el archivo donde se ha intentado la operación de E/S. Es el desplazamiento de bytes físico desde el inicio del archivo. La división de este número por 8192 proporciona el número de página lógico que se ve afectado por el error.
 - Si la operación de E/S es una solicitud de lectura o escritura.
 - El código de error del sistema operativo y la descripción del error entre paréntesis.
 

**Error del sistema operativo:** Una llamada API de Windows de lectura o escritura no es correcta y SQL Server detecta un error del sistema operativo relacionado con la llamada API de Windows. El siguiente mensaje es un ejemplo de un error 823:

```
Error: 823, Severity: 24, State: 2.
2010-03-06 22:41:19.55 spid58 The operating system returned error 1117 (The request could not be performed because of an I/O device error.) to SQL Server during a read at offset 0x0000002d460000 in file 'e:\program files\Microsoft SQL Server\mssql\data\mydb.MDF'. Additional messages in the SQL Server error log and system event log may provide more detail. This is a severe, system-level error condition that threatens database integrity and must be corrected immediately. It is recommended to complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.
```

Es posible que vea o no errores de la instrucción DBCC CHECKDB en la base de datos asociada al archivo en el mensaje de error. Puede ejecutar la instrucción DBCC CHECKDB cuando vea un error 823. Si la instrucción DBCC CHECKDB no notifica ningún error, probablemente haya un problema intermitente del sistema o de un disco.

Se puede escribir información de diagnóstico adicional de los errores 823 en el archivo de registro de errores de SQL Server si se usa la marca de seguimiento 818.
Para obtener más información, vea [KB 826433: Diagnósticos adicionales de SQL Server agregados para detectar problemas de E/S no notificados](https://support.microsoft.com/help/826433/sql-server-diagnostics-added-to-detect-unreported-i-o-problems-due-to)


## <a name="cause"></a>Causa
El mensaje de error 823 suele indicar que hay un problema con el sistema de almacenamiento subyacente, el hardware o un controlador que se encuentra en la ruta de acceso de la solicitud de E/S. Este error puede producirse si hay incoherencias en el sistema de archivos o si el archivo de base de datos está dañado. En el caso de una lectura de archivo, SQL Server ya ha reintentado la solicitud de lectura cuatro veces antes de devolver 823. Si la operación de reintento se realiza correctamente, la consulta no produce ningún error, pero se escribe el mensaje [825](mssqlserver-825-database-engine-error.md) en ERRORLOG y en el registro de eventos.

## <a name="user-action"></a>Acción del usuario  
 - Revise la tabla [suspect_pages](../system-tables/suspect-pages-transact-sql.md) de MSDB para ver otras páginas que tengan este problema (en la misma base de datos o en otras).
 - Compruebe la coherencia de las bases de datos ubicadas en el mismo volumen (como la que se indica en el mensaje 823) mediante el comando DBCC CHECKDB. Si detecta incoherencias en el comando DBCC CHECKDB, use las instrucciones de [Cómo solucionar los errores de coherencia de la base de datos indicados por DBCC CHECKB](https://support.microsoft.com/help/2015748/how-to-troubleshoot-database-consistency-errors-reported-by-dbcc-check). 
 - Revise los registros de eventos de Windows para ver si hay errores o mensajes notificados desde el sistema operativo, un dispositivo de almacenamiento o un controlador de dispositivo. Si están relacionados con este error de alguna manera, solucione esos errores primero. Por ejemplo, aparte del mensaje 823, también puede observar un evento como "El controlador detectó un error de controladora en \Device\Harddisk4\DR4" notificado por el disco en el registro de eventos. En ese caso, tiene que evaluar si este archivo está presente en este dispositivo y luego corregir esos errores de disco en primer lugar.
 - Use la utilidad [SQLIOSim](https://support.microsoft.com/help/231619/how-to-use-the-sqliosim-utility-to-simulate-sql-server-activity-on-a-d) para averiguar si estos errores 823 se pueden reproducir fuera de las solicitudes normales de E/S de SQL Server. La utilidad SQLIOSim se incluye en SQL Server 2008 y versiones posteriores, por lo que no hay necesidad de una descarga independiente. Normalmente se puede encontrar en la carpeta `C:\Program Files\Microsoft SQL Server\MSSQLxx.MSSQLSERVER\MSSQL\Binn`.
 - Trabaje con el proveedor de hardware o con el fabricante del dispositivo para asegurarse de que
   - Los dispositivos de hardware y la configuración se ajustan a los requisitos de E/S de SQL Server
   - Los controladores de dispositivo y otros componentes de software complementarios de todos los dispositivos de la ruta de acceso de E/S están actualizados
 - Si el fabricante del dispositivo o el proveedor de hardware le ha proporcionado cualquier utilidad de diagnóstico, úsela para evaluar el estado del sistema de E/S
 - Evalúe si hay [controladores de filtro](https://support.microsoft.com/help/2454053/use-of-system-filter-drivers-can-lead-to-sql-server-database-engine-pe) en la ruta de acceso de estas solicitudes de E/S que experimentan problemas.
   - Compruebe si hay alguna actualización para estos controladores de filtro
   - Puede quitar o deshabilitar estos controladores de filtro para observar si el problema que genera el error 823 desaparece  
