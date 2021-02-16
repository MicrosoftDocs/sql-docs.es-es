---
description: MSSQLSERVER_3988
title: MSSQLSERVER_3988
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3988 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 42e07f7731696e23579650ccbfe8ca3930c69cb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348789"
---
# <a name="mssqlserver_3988"></a>MSSQLSERVER_3988
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Detalles

|Atributo|Value|
|---|---|
|Nombre de producto|SQL Server|
|Id. de evento|3988|
|Origen de eventos|MSSQLSERVER|
|Componente|SQLEngine|
|Nombre simbólico|XACT_UNSUPPORT_PARALLEL_TRAN2|
|Texto del mensaje|No se permite una nueva transacción porque hay otros subprocesos en ejecución en la sesión.|
||

## <a name="explanation"></a>Explicación

Este error se produce al ejecutar una consulta distribuida que combina varias tablas hospedadas por instancias remotas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mientras la configuración de sesión `XACT_ABORT` está **ACTIVADA**. El usuario recibe un mensaje de error similar al siguiente:

> Mensaje 3988, nivel 16, estado 1, n.º de línea  
No se permite una nueva transacción porque hay otros subprocesos en ejecución en la sesión.

## <a name="cause"></a>Causa

Hay algunas limitaciones de diseño en la forma en que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] controla las consultas distribuidas (DQ) cuando se cumplen las condiciones siguientes:

- SQL Server combina varias tablas de un origen de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] remoto.
- La sesión que emite la consulta no está dada de alta en una transacción distribuida.

En esta situación, un intento de ejecutar la consulta puede producir cualquiera de los dos errores que se mencionan en la sección **Explicación**.

## <a name="user-action"></a>Acción del usuario

Para solucionar el problema, incluya la consulta distribuida en una instrucción "begin distributed transaction":

```sql
BEGIN DISTRIBUTED TRANSACTION <Distributed Query> COMMIT TRANSACTION
```
