---
description: MSSQLSERVER_
title: MSSQLSERVER_4214
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 4214 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 1ccce1ca406527cfca2485bec41f2fb9d287862c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348770"
---
# <a name="mssqlserver_4214"></a>MSSQLSERVER_4214
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|4214|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|DMPX_NODBBACKUP|
|Texto del mensaje|No se puede ejecutar BACKUP LOG porque no hay una copia de seguridad de la base de datos actual|
||

## <a name="explanation"></a>Explicación

Este error se produce cuando se intenta realizar una copia de seguridad del registro de transacciones antes de realizar una copia de seguridad completa de una base de datos. Para poder realizar una copia de seguridad del registro de transacciones de una base de datos, primero debe realizar una copia de seguridad completa de la base de datos. Aparte de esto, recibirá el siguiente mensaje en el registro de errores:

> \<Datetime> Copia de seguridad Error: 3041, Gravedad: 16, estado: 1.  
\<Datetime> Copia de seguridad BACKUP no pudo completar el comando BACKUP LOG\<db_name>. Compruebe el registro de la aplicación de copia de seguridad para ver los mensajes detallados.

## <a name="user-action"></a>Acción del usuario

Realice una copia de seguridad completa de la base de datos para poder realizar una copia de seguridad del registro de transacciones de la base de datos.

## <a name="more-information"></a>Más información

Para obtener más información, consulte el artículo [Realizar copias de seguridad y restaurar bases de datos de SQL Server](../backup-restore/back-up-and-restore-of-sql-server-databases.md).