---
description: MSSQLSERVER_854
title: MSSQLSERVER_854
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 854 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: e854963d9b14833bcadbdbe046cccfb16e08e03f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348676"
---
# <a name="mssqlserver_854"></a>MSSQLSERVER_854
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|854|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|HARDWARE_MEMORY_SCRUBBER|
|Texto del mensaje|La máquina admite la recuperación de errores de memoria. La protección de memoria SQL está habilitada para recuperarse si se daña la memoria.|
||

## <a name="explanation"></a>Explicación

Este mensaje indica que el hardware del sistema operativo admite la capacidad de recuperarse de errores de memoria. En los equipos con hardware más reciente que ejecutan Windows Server 2012 o una versión posterior, el hardware puede notificar al sistema operativo y a las aplicaciones de que las páginas de memoria (páginas del sistema operativo) están marcadas como dañadas o con errores. Las aplicaciones como [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden registrar estas notificaciones de páginas de memoria dañadas mediante el siguiente conjunto de API:

- `GetMemoryErrorHandlingCapabilities`
- `RegisterBadMemoryNotification`
- `BadMemoryCallbackRoutine`

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agrega compatibilidad para estas notificaciones en Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012 y en versiones posteriores. Durante el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprueba si el hardware es compatible con esta nueva característica. Aparte de esto, recibirá el siguiente mensaje en el registro de errores:

> \<Datetime> Servidor La máquina admite la recuperación de errores de memoria. La protección de memoria SQL está habilitada para recuperarse si se daña la memoria.

## <a name="user-action"></a>Acción del usuario

Compruebe si hay otros errores, como 855 y 856, y lleve a cabo las acciones correctivas adecuadas.

## <a name="more-information"></a>Más información

Puede usar la marca de seguimiento 849 de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para evitar que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se registre con el sistema operativo para recibir notificaciones de errores de memoria. Pero tenga en cuenta que la habilitación de la marca de seguimiento 849 impedirá que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reciba notificaciones de memoria dañada del sistema operativo. Por lo tanto, no se recomienda usar esta marca de seguimiento en circunstancias normales.
