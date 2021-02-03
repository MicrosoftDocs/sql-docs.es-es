---
description: MSSQLSERVER_10737
title: MSSQLSERVER_10737 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10737 (Database Engine error)
ms.assetid: 208af6ed-b271-4ab8-803e-7666025385c8
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 05bb027048e38f976a8b32de684e59ad5cf68157
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197322"
---
# <a name="mssqlserver_10737"></a>MSSQLSERVER_10737
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|MSSQLSERVER|  
|Id. de evento|10737|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|REBUILD_PARTITION_ALL_NOT_SPECIFIED|  
|Texto del mensaje|En una instrucción ALTER TABLE REBUILD o ALTER INDEX REBUILD, cuando se especifica una partición en una cláusula DATA_COMPRESSION, se debe especificar PARTITION=ALL. La cláusula PARTITION=ALL se utiliza para reforzar que se volverán a generar todas las particiones de la tabla o índice, aunque solo se especifique un subconjunto en la cláusula DATA_COMPRESSION.|  
  
## <a name="user-action"></a>Acción del usuario  
Agregue la cláusula PARTITION=ALL a la instrucción ALTER TABLE o ALTER INDEX. También puede recompilar una partición concreta mediante REBUILD PARTITION = \<partition-number-expr> WITH (DATA_COMPRESSION={ON | OFF}).  
  
