---
description: MSSQLSERVER_824
title: MSSQLSERVER_824 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 824 (Database Engine error)
ms.assetid: 2aa22246-2712-4fdb-9744-36e7e6f3175e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 031a478a281538b096d0900ad80fa34741182269
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180309"
---
# <a name="mssqlserver_824"></a>MSSQLSERVER_824
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|824|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|B_HARDSSERR|  
|Texto del mensaje|SQL Server detectó un error de E/S de coherencia lógico: %ls. Ocurrió durante %S_MSG de la página %S_PGID en la base de datos con id. %d, desplazamiento %#016I64x, archivo '%ls'.  El registro de errores de SQL Server o el registro de eventos del sistema pueden contener mensajes adicionales con más detalles.|  
  
## <a name="symptom"></a>Síntoma  


Es posible que aparezca el mensaje de error siguiente en el registro de errores de SQL Server o en el de eventos de aplicación de Windows si se produce un error en una comprobación de coherencia lógica después de leer o escribir una página de base de datos:
 
``` 
2009-11-02 15:46:42.90 spid51      Error: 824, Severity: 24, State: 2.
2009-11-02 15:46:42.90 spid51      SQL Server detected a logical consistency-based I/O error: incorrect pageid (expected 1:43686; actual 0:0). It occurred during a read of page (1:43686) in database ID 23 at offset 0x0000001554c000 in file 'H:\MSSQL.SQL2008\MSSQL\DATA\my_db.mdf'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.
```
 
Si una aplicación encuentra este mensaje mientras consulta o modifica datos, se devuelve el mensaje de error a la aplicación y se termina la conexión a la base de datos. 
  
## <a name="cause"></a>Causa
Este error indica que Windows informa de que la página se lee correctamente desde el disco, pero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ha detectado algún problema en ella. Este error es similar al error 823, excepto que Windows no lo ha detectado y, normalmente, suele indicar un problema en el subsistema de E/S, como errores en la unidad de disco, problemas de firmware del disco, un controlador de dispositivo defectuoso, etc. Para obtener más información sobre los errores de E/S, vea el [capítulo 2 del documento sobre conceptos básicos de E/S de Microsoft SQL Server](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)).  

SQL Server usa las API de Windows (por ejemplo, ReadFile, WriteFile, ReadFileScatter o WriteFileGather) para realizar las operaciones de E/S. Después de realizar estas operaciones de E/S, SQL Server busca condiciones de error asociadas a estas llamadas API. Si se produce un error del sistema operativo en estas llamadas API, SQL Server notifica el error 823. Puede haber situaciones en las que la llamada API de Windows realmente sea correcta, pero que los datos transferidos por la operación de E/S hayan detectado un problema de coherencia lógica. Estos problemas de coherencia lógica se notifican a través del error 824.
 
El error 824 contiene la información siguiente:

- El archivo de base de datos en el que se realiza la operación de E/S.
- El desplazamiento con el archivo donde se ha intentado la operación de E/S.
- La base de datos a la que pertenece este archivo.
- El número de página implicado en la operación de E/S.
- Si era una operación de lectura o escritura.
- Detalles sobre la comprobación de coherencia lógica en la que se ha producido el error [el tipo de comprobación, el valor real y el valor esperado usados para esta comprobación].
 
Estas comprobaciones de coherencia lógica son comprobaciones de integridad adicionales que SQL Server realiza para asegurarse de que determinados aspectos clave de los datos que intervienen en la transferencia de datos se han mantenido durante la operación de E/S. Las comprobaciones incluyen las de suma de comprobación, página rasgada, transferencia corta, id. de página incorrecto, lectura obsoleta y error de auditoría de página. La naturaleza de las comprobaciones realizadas varía en función de las distintas opciones de configuración en el nivel de base de datos y de servidor. 
 
El mensaje de error 824 suele indicar que hay un problema con el sistema de almacenamiento subyacente, el hardware o un controlador que se encuentra en la ruta de acceso de la solicitud de E/S. Este error puede producirse si hay incoherencias en el sistema de archivos o si el archivo de base de datos está dañado.

## <a name="resolution"></a>Resolución  

Si se produce el error 824, puede probar las siguientes soluciones: 

- Revise la tabla [suspect_pages](../backup-restore/manage-the-suspect-pages-table-sql-server.md) de msdb para comprobar si otras páginas (en la misma base de datos o en otras) tienen este problema.
- Compruebe la coherencia de las bases de datos ubicadas en el mismo volumen (como el que se indica en el mensaje 824) mediante el comando DBCC CHECKDB. Si detecta incoherencias en el comando DBCC CHECKDB, use las instrucciones del artículo de Knowledge Base [Cómo solucionar los errores de coherencia de la base de datos indicados por DBCC CHECKDB](https://support.microsoft.com/help/2015748/how-to-troubleshoot-database-consistency-errors-reported-by-dbcc-check).
- Si la base de datos que encuentra estos errores 824 no tiene activada la opción de base de datos PAGE_VERIFY CHECKSUM, hágalo de inmediato. Los errores 824 se pueden producir por otros motivos que no sean un error de suma de comprobación, pero CHECKSUM proporciona la mejor opción para comprobar la coherencia de la página después de que se haya escrito en el disco.
- Revise los registros de eventos de Windows para ver si hay errores o mensajes notificados desde el sistema operativo, un dispositivo de almacenamiento o un controlador de dispositivo. Si de alguna manera están relacionados con este error, solucione primero esos errores. Por ejemplo, aparte del mensaje 824, también puede observar un evento como "El controlador ha detectado un error de controlador en \Device\Harddisk4\DR4" notificado por el disco en el registro de eventos. En ese caso, tiene que evaluar si este archivo está presente en este dispositivo y luego corregir esos errores de disco en primer lugar.
- Use la utilidad [SQLIOSim](https://support.microsoft.com/help/231619/how-to-use-the-sqliosim-utility-to-simulate-sql-server-activity-on-a-d) para averiguar si estos errores 824 se pueden reproducir fuera de las solicitudes normales de E/S de SQL Server. SQLIOSim se incluye en SQL Server 2008, por lo que no hay necesidad de una descarga independiente para esta versión o posteriores.
- Consulte al proveedor de hardware o al fabricante del dispositivo para asegurarse de que:
   - Los dispositivos de hardware y la configuración se ajustan a los [requisitos de E/S de SQL Server](https://support.microsoft.com/help/967576/microsoft-sql-server-database-engine-input-output-requirements).
   - Los controladores de dispositivo y otros componentes de software complementarios de todos los dispositivos de la ruta de acceso de E/S están actualizados.
- Si el fabricante del dispositivo o el proveedor de hardware le han proporcionado alguna utilidad de diagnóstico, úsela para evaluar el estado del sistema de E/S.
- Evalúe si hay controladores de filtro en la ruta de acceso de estas solicitudes de E/S que experimentan problemas.
   - Compruebe si hay alguna actualización para estos controladores de filtro
   - Puede quitar o deshabilitar estos controladores de filtro para observar si desaparece el problema que genera el error 824.
- Si el problema no está relacionado con el hardware y tiene una copia de seguridad limpia disponible, úsela para restaurar la base de datos.  

## <a name="see-also"></a>Consulte también  
[Administrar la tabla suspect_pages &#40;SQL Server&#41;](~/relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
