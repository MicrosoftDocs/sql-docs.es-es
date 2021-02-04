---
description: MSSQLSERVER_1205
title: MSSQLSERVER_1205 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1205 (Database Engine error)
ms.assetid: 9fe3f67c-df3c-4642-a3a4-ccc0e138b632
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 8ce9d2d99d3db0fc18586b9292876d2e4e6c985b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197172"
---
# <a name="mssqlserver_1205"></a>MSSQLSERVER_1205
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|1205|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|LK_VICTIM|  
|Texto del mensaje|La transacción (Id. de proceso %d) quedó en interbloqueo en %.*ls recursos con otro proceso y fue elegida como sujeto del interbloqueo. Vuelva a ejecutar la transacción.|  
  
## <a name="explanation"></a>Explicación

El orden en el que se accede a los recursos en transacciones independientes es conflictivo y provoca un [interbloqueo](../sql-server-transaction-locking-and-row-versioning-guide.md?#deadlocks). Por ejemplo:  
  
- Transaction1 actualiza **Table1.Row1**, mientras que Transaction2 actualiza **Table2.Row2**.
- Transaction1 intenta actualizar **Table2.Row2**, pero se bloquea porque Transaction2 todavía no se ha confirmado.  
- Transaction2 ahora intenta actualizar **Table1.Row1**, pero se bloquea porque Transaction1 no se ha confirmado.
- Se produce un interbloqueo porque Transacción1 está esperando a que Transacción2 finalice, pero Transacción2 está esperando a que finalice Transacción1.  
  
El sistema detectará este interbloqueo y elegirá una de las transacciones implicadas como "víctima". Seguidamente, emitirá este mensaje de error y revertirá la transacción de la víctima.  Para obtener más información, consulte [Interbloqueos](../sql-server-transaction-locking-and-row-versioning-guide.md?#deadlocks).

## <a name="user-action"></a>Acción del usuario  

Ejecute de nuevo la transacción. También puede revisar la aplicación para evitar los interbloqueos. La transacción elegida como víctima se puede volver a intentar y probablemente se realizará correctamente, en función de qué operaciones se estén ejecutando simultáneamente.  
  
Para evitar la aparición de interbloqueos, valore la posibilidad de hacer que todas las transacciones accedan a las filas en el mismo orden (primero, **Table1**; después, **Table2**). De esta forma, aunque se puedan producir bloqueos, no se generarán interbloqueos.  
  
Para obtener más información, consulte [Controlar interbloqueos](../sql-server-transaction-locking-and-row-versioning-guide.md?#handling-deadlocks) y [Minimizar los interbloqueos](../sql-server-transaction-locking-and-row-versioning-guide.md#deadlock_minimizing).
