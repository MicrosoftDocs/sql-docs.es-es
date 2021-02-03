---
description: MSSQLSERVER_5180
title: MSSQLSERVER_5180
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 5180 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: f80f4dafad6b0570837728e15a79eebdd7d6f41c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185536"
---
# <a name="mssqlserver_5180"></a>MSSQLSERVER_5180
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|5180|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|DSK_BAD_FCB_FILEID|
|Texto del mensaje|No se pudo abrir File Control Bank (FCB) debido a un id. de archivo no válido (%d) en la base de datos '%.*ls'. Compruebe la ubicación del archivo. Ejecute DBCC CHECKDB.|
||

## <a name="explanation"></a>Explicación

Una consulta u operación puede producir un error 5180 cuando el [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion_md.md)] hace referencia a un id. de archivo no válido. Por ejemplo:

> Mensaje 5180, nivel 22, estado 1, línea 1  
No se pudo abrir File Control Bank (FCB) debido a un id. de archivo no válido (%d) en la base de datos '%.*ls'. Compruebe la ubicación del archivo. Ejecute DBCC CHECKDB.

Dado que el error se produce con gravedad 22, se desconectará la sesión del usuario. Este mensaje de error se escribe en el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y en el registro de eventos de la aplicación de Windows como EventID=5180.

## <a name="possible-causes"></a>Causas posibles

El [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)] hace referencia a un identificador de archivo en muchas situaciones diferentes, principalmente cuando hace referencia a un identificador de página (ya que el identificador de archivo es la primera parte del identificador de página). Si por alguna razón, el identificador de archivo al que se hace referencia es inferior a 0 o no es un identificador de archivo válido en una base de datos (según los identificadores de archivo válidos enumerados en las vistas de catálogo del sistema, como sys.database_files), se puede dar un error 5180.

Una posible causa es que un identificador de archivo almacenado no sea válido. El registro reenviado en una fila hace referencia a otra página, por lo que, cuando se accede a esa página y el identificador de archivo no es válido, se puede producir un error 5180. Esta condición generaría un error de base de datos dañada en la página con el registro reenviado. (En este ejemplo, `DBCC CHECKDB` notificaría el mensaje 8993).

Otro ejemplo es un identificador de archivo no válido que forma parte de un identificador de página almacenado en el encabezado de página para identificar la página siguiente o anterior. Este campo se usa para vincular una serie de páginas, por ejemplo, como en un índice agrupado. Si el identificador de archivo no es válido para la página anterior o siguiente, cuando el motor necesita hacer referencia a este identificador para ir a la página siguiente o a la anterior, se puede generar un error 5180. (En este ejemplo, `DBCC CHECKDB` notifica el mensaje 8981).

Si el problema no es un identificador de archivo no válido debido a un identificador de página almacenado dañado, el problema puede estar en el [!INCLUDE[ssDENoVersion](../../includes/ssdenoversion-md.md)].

## <a name="user-action"></a>Acción del usuario

Si se produce este error, debe ejecutar `DBCC CHECKDB` en la base de datos tal y como se indica en el mensaje de error. Si encuentra errores, debe restaurar la base de datos a partir de una copia de seguridad en buen estado. Si no puede restaurar a partir de una copia de seguridad, la alternativa es usar la opción de reparación de `DBCC CHECKDB` como se recomienda en la salida. En la mayoría de los casos, una reparación de este tipo de error conllevará una pérdida de datos. Para obtener más información sobre cómo usar el comando y sobre las posibles causas de los problemas de daños en una base de datos, vea el artículo: [Procedimiento para solucionar errores de coherencia de base de datos notificados por DBCC CHECKDB](https://support.microsoft.com/kb/2015748).

Si `DBCC CHECKDB` no informa de ningún error y el problema persiste, póngase en contacto con el equipo de soporte técnico de Microsoft para obtener ayuda. Tenga a mano la información de la consulta que encontró el error 5180. Consulte la sección [Más información](#more-information) para saber cómo determinar qué consulta encontró el error.

## <a name="more-information"></a>Más información

El bloque de control de archivos (FCB) es una estructura de memoria interna que realiza un seguimiento de la información importante del archivo asociado a la base de datos. Un identificador de archivo se usa para identificar de forma única un FCB para una base de datos. Si el motor del servidor intenta hacer referencia a un identificador de archivo que no es válido, no se puede encontrar la estructura del bloque de control de archivos, lo que desencadena la condición de error 5180.

Para saber qué consulta encontró este error, puede usar estas técnicas:

- Para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 y versiones posteriores, consulte si la sesión system_health tiene un registro del error, el cual debe incluir el texto de la consulta. Para obtener más información acerca de la sesión system_health, consulte el siguiente recurso: [Compatibilidad con SQL Server 2008: sesión system_health](https://techcommunity.microsoft.com/t5/sql-server-support/supporting-sql-server-2008-the-system-health-session/ba-p/315509).
- Use [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler y capture los eventos `SQL:BatchStarting`, `RPC:Starting` y de excepción. Busque la consulta que precede al evento de excepción del error 5180 para la sesión asociada al evento de excepción.
