---
description: MSSQLSERVER_856
title: MSSQLSERVER_856
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 856 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 3c84818a233fabbe5829339849663c4044f6490e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348702"
---
# <a name="mssqlserver_856"></a>MSSQLSERVER_856
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|856|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|BAD_MEMORY_CLEAN_DATABASE_PAGE|
|Texto del mensaje|SQL Server ha detectado daños en la memoria de hardware en la base de datos "%ls", id. de archivo: %u; id. de página: %u; dirección de memoria: 0x%I64x y ha recuperado la página correctamente|
||

## <a name="explanation"></a>Explicación

Este mensaje indica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] detectó una página de memoria dañada en un objeto almacenado en caché fuera del grupo de búferes. Este mensaje se genera en sistemas que admiten la capacidad de recuperarse de errores de memoria. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] corrige estos errores de memoria descartando las páginas de memoria dañadas que no se han modificado y registra este mensaje de error. Si la página de memoria dañada se ha modificado (es decir, es una página desfasada), se produce el error 824 ([MSSQLSERVER_824](mssqlserver-824-database-engine-error.md)).

## <a name="user-action"></a>Acción del usuario

Debe realizar comprobaciones de hardware o del sistema para determinar si existen problemas de CPU o de memoria. Asegúrese de que se han aplicado todos los controladores del sistema, las actualizaciones del sistema operativo y las actualizaciones de hardware. Considere la posibilidad de ejecutar los diagnósticos del fabricante del hardware, incluidas pruebas relacionadas con la memoria. Siempre que vea este error, considere también la posibilidad de ejecutar `DBCC CHECKDB` en todas las bases de datos de esta instancia.

## <a name="more-information"></a>Más información

En los equipos con hardware más reciente que ejecutan Windows Server 2012 o una versión posterior, el hardware puede notificar al sistema operativo y a las aplicaciones de que las páginas de memoria (páginas del sistema operativo) están marcadas como dañadas o con errores. Las aplicaciones como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden registrar estas notificaciones de páginas de memoria dañadas mediante el siguiente conjunto de API:

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agrega compatibilidad para estas notificaciones en Microsoft [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y en versiones posteriores. Durante el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprueba si el hardware es compatible con esta nueva característica. Aparte de esto, recibirá el siguiente mensaje en el registro de errores:

> \<Datetime> Servidor La máquina admite la recuperación de errores de memoria. La protección de memoria SQL está habilitada para recuperarse si se daña la memoria.

Actualmente, solo el grupo de búferes entra en acción cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibe estas notificaciones. Al recibirlas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene que recorrer en iteración todo el grupo de búferes y descubrir la dirección de cada búfer asignado. Después, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa la API `QueryWorkingSetEX` para comprobar si alguna de las páginas de memoria que respaldan la página de datos está marcada como dañada. La estructura de salida `PSAPI_WORKING_SET_EX_BLOCK` que corresponde a esta página de memoria tendrá el elemento "Bad" establecido en 1 si se ha producido algún daño.

Si esa página de datos o ese grupo de búferes no se han modificado o no están procesando la E/S, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede descartar y anular la confirmación de la página de datos. Después, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra el mensaje siguiente:

> SQL Server ha detectado daños en la memoria de hardware en la base de datos "%ls", id. de archivo: %u; id. de página: %u; dirección de memoria: 0x%I64x y ha recuperado correctamente la página.

Cuando las consultas requieran esa página de datos de nuevo, el grupo de búferes puede volver a leerla desde el disco y devolver su contenido al grupo de búferes. También es posible que la versión en disco de la página esté en un estado dañado. En ese caso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede registrar otros errores, como el error 824.

Si el grupo de búferes no usa la página dañada, pero sí la usa algún otro objeto o estructura almacenados en caché, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra el siguiente mensaje:

> Se detectaron daños en la memoria de hardware que no pueden corregirse. El sistema puede volverse inestable. Compruebe el registro de eventos de Windows para obtener más detalles.

Si el servidor informa de errores de memoria, debe ponerse en contacto con el fabricante del hardware del equipo y realizar las acciones adecuadas, como ejecutar diagnósticos de memoria, actualizar el BIOS y el firmware, y reemplazar los módulos de memoria dañados.

Puede usar la marca de seguimiento 849 de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para evitar que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se registre con el sistema operativo para recibir notificaciones de errores de memoria. Pero tenga en cuenta que la habilitación de la marca de seguimiento 849 impedirá que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reciba notificaciones de memoria dañada del sistema operativo. Por lo tanto, no se recomienda usar esta marca de seguimiento en circunstancias normales.

Además, tenga en cuenta que, de forma predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibirá estas notificaciones en el hardware compatible.

También debe tener presente que, cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] registra estas notificaciones de errores de memoria, el proceso del sistema de escritura diferida no realiza comprobaciones de página constantes. Para obtener más información sobre las comprobaciones de páginas constantes, vea [Procedimiento para solucionar el error Msg 832 (la página constante ha cambiado) en SQL Server](https://support.microsoft.com/help/2015759).
