---
description: MSSQLSERVER_7105
title: MSSQLSERVER_7105
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 7105 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: beb0e6bb3f960972b0f79413cd9186b2d005b7f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194120"
---
# <a name="mssqlserver_7105"></a>MSSQLSERVER_7105
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|7105|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|TXT_PGNOTEXIST|
|Texto del mensaje|El id. de base de datos %d, página %S_PGID, ranura %d para el nodo de tipo de datos LOB no existe. Esto suele ser debido a transacciones que pueden leer datos no confirmados de una página de datos. Ejecute DBCC CHECKTABLE.|
||

## <a name="explanation"></a>Explicación

Una consulta puede encontrar el mensaje 7105 cuando no se puede acceder a los datos de objetos grandes (LOB) a los que hace referencia la fila de una página de base de datos.

Dado que este error tiene un nivel de gravedad 22, el servidor finaliza la conexión. Este mensaje de error también se escribe en el archivo ERRORLOG de SQL y en el registro de eventos de la aplicación de Windows como EventID=7105.

## <a name="possible-causes"></a>Causas posibles

Este error puede producirse por alguno de los siguientes motivos:

- Existe un problema de daños en una página de base de datos o en las estructuras de la página de LOB a las que hace referencia la página de base de datos.
- La consulta que ha detectado el error está usando el argumento `READ UNCOMMITTED ISOLATION LEVEL` o la sugerencia de consulta `NOLOCK`.
- Hay un problema en el motor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que provoca este error en la consulta.

Consulte la resolución y la sección [Más información](#more-information) para determinar la causa de su problema en particular y la solución adecuada.

## <a name="user-action"></a>Acción del usuario

1. Como indica el mensaje, lo primero que debe hacer es ejecutar `DBCC CHECKDB` en la base de datos en la que se encontró el problema (o bien `DBCC CHECKTABLE` si se trata de una tabla).

    - El identificador de la base de datos se proporciona en el mensaje.
    - Para averiguar cuál es la tabla afectada sin ejecutar `DBCC CHECKDB`, deberá conocer a qué tablas ha accedido la consulta que ha encontrado el error. Un método consiste en usar SQL Server Profiler para hacer un seguimiento de la consulta. Por otro lado, en [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] y [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] R2, es posible que pueda encontrar la consulta mediante la sesión de eventos extendidos system_health. Consulte este vínculo para obtener más información sobre el uso de la sesión system_health: [Usar la sesión system_health](../extended-events/use-the-system-health-session.md).

    - Como con cualquier problema de coherencia de la base de datos, puede resolver estos errores restaurando la base de datos a partir de una copia de seguridad en buen estado que no contenga este problema.

    - Si no puede restaurarla a partir de una copia de seguridad, siga las recomendaciones de uso de `DBCC CHECKDB` o `DBCC CHECKTABLE` para reparar estos errores. Es posible que esto produzca una pérdida de datos. Para obtener más información sobre cómo usar CHECKDB y sobre las posibles causas de los problemas de daños en una base de datos, vea el artículo: [Procedimiento para solucionar errores de coherencia de base de datos notificados por DBCC CHECKDB](https://support.microsoft.com/kb/2015748).
  
1. Es posible que este error se haya producido porque la consulta que ha accedido a la tabla estaba usando un nivel de aislamiento `READ UNCOMMITTED` o la sugerencia de consulta `NOLOCK` (también denominada lectura de datos sucios).

   - Si `DBCC CHECKDB` o `DBCC CHECKTABLE` no muestran ningún error asociado con esta tabla ni con los datos de LOB, la causa más probable es el uso de una lectura de datos sucios. Si es así en el caso de su aplicación, evite usar una lectura de datos sucios o vuelva a intentar la consulta.
  
   - Si esta es la causa del error, no existe ningún problema real de coherencia de la base de datos.

## <a name="more-information"></a>Más información

Si la base de datos dañada es la causa del problema, los comandos `DBCC CHECKDB` o `DBCC CHECKTABLE` deben indicar los errores, pero no notificarán el mensaje 7105. Los errores encontrados con CHECKDB dependerán de lo que esté dañado en la referencia a las estructuras de LOB o en las propias estructuras.

- Si la fila de la página de base de datos no hace referencia correctamente a una página de LOB válida, es posible que vea errores como los siguientes:

    > Mensaje 8929, nivel 16, estado 1, línea 1  
    Id. de objeto 2137058649, id. de índice 0, id. de partición 72057594038910976, id. de unidad de asignación 72057594039828480 (tipo Datos consecutivos): se encontraron errores en datos no consecutivos con id. 131203072 cuyo propietario es el registro de datos identificado por RID = (1:179:1)  
    Mensaje 8964, nivel 16, estado 1, línea 1  
    Error de tabla: id. de objeto 2137058649, id. de índice 0, id. de partición 72057594038910976, id. de unidad de asignación 72057594039894016 (tipo Datos de LOB). No se hace referencia al nodo de datos no consecutivos de la página (1:177), zona 1, id. de texto 131203072.  
    Mensaje 8965, nivel 16, estado 1, línea 1  
    Error de tabla: id. de objeto 2137058649, id. de índice 0, id. de partición 72057594038910976, id. de unidad de asignación 72057594039894016 (tipo Datos de LOB). La página (1:179), zona 1 hace referencia al nodo de datos no consecutivos de la página (255:177), zona 1, id. de texto 131203072, pero no se encontró en el examen.  

- Los distintos escenarios para el problema pueden producir diferentes combinaciones de errores. En este ejemplo:  

    > La página de base de datos 1:179, zona 1 hace referencia a una página de LOB que no es una página válida en la base de datos (página 255:177). La página (1:177) es una página de LOB válida, pero ninguna página de base de datos hace referencia a ella. Por lo tanto, en esta situación, el problema es que la fila de la zona 1 en la página 1:179 hace referencia a la página 255:177 en lugar de a la página 1:177.

- La clave para determinar si los errores de `DBCC CHECKDB` están relacionados con los problemas de la página de LOB es buscar las frases "datos no consecutivos" y "tipo Datos de LOB".

    > El mensaje 8929 es un error relacionado con la página de base de datos que hace referencia a las páginas de LOB.  
El mensaje 8964 es un error que indica que ninguna página de base de datos hace referencia a una página de LOB.  
El mensaje 8965 es un error que indica que una página de base de datos hace referencia a una página de LOB, pero no es una página válida.

    En muchas situaciones relacionadas con estos tipos de errores, la reparación dará lugar a la eliminación de las filas que apuntan a los datos de LOB y los propios datos de LOB. El algoritmo de reparación intentará quitar solo los fragmentos de LOB que afecten a las filas de la base de datos en cuestión, pero esto no se puede garantizar en todas las situaciones puesto que depende de lo que esté dañado en la "estructura de árbol" de LOB.

- En el ejemplo que se muestra aquí, los mensajes devueltos por CHECKTABLE mediante el uso de `REPAIR_ALLOW_DATA_LOSS` tienen el siguiente aspecto:

    > Reparación: se ha eliminado el registro para el id. de objeto 2137058649, id. de índice 0, id. de partición 72057594038910976, id. de unidad de asignación 72057594039828480 (tipo Datos consecutivos), en la página (1:179), ranura 1. Se volverán a generar los índices.  
    Reparación: se eliminó una columna de datos no consecutivos con id. 131203072 para el id. de objeto 2137058649, id. de índice 0, id. de partición 72057594038910976, id. de unidad de asignación 72057594039894016 (tipo Datos de LOB) en la página (1:177), ranura 1.  
    Mensaje 8929, nivel 16, estado 1, línea 1  
    Id. de objeto 2137058649, id. de índice 0, id. de partición 72057594038910976, id. de unidad de asignación 72057594039828480 (tipo Datos consecutivos): se encontraron errores en datos no consecutivos con id. 131203072 cuyo propietario es el registro de datos identificado por RID = (1:179:1)  
            Se ha corregido el error.  
    Mensaje 8964, nivel 16, estado 1, línea 1  
    Error de tabla: id. de objeto 2137058649, id. de índice 0, id. de partición 72057594038910976, id. de unidad de asignación 72057594039894016 (tipo Datos de LOB). No se hace referencia al nodo de datos no consecutivos de la página (1:177), zona 1, id. de texto 131203072.  
            Se ha corregido el error.  
    Mensaje 8965, nivel 16, estado 1, línea 1  
    Error de tabla: id. de objeto 2137058649, id. de índice 0, id. de partición 72057594038910976, id. de unidad de asignación 72057594039894016 (tipo Datos de LOB). La página (1:179), zona 1 hace referencia al nodo de datos no consecutivos de la página (255:177), zona 1, id. de texto 131203072, pero no se encontró en el examen.  
            No se puede reparar este error.

    El último mensaje que indica `Could not repair this error` es engañoso. El error se ha reparado porque se ha eliminado la fila de la página de base de datos que apuntaba a la página incorrecta (255:177).