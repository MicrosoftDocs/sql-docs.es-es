---
description: MSSQLSERVER_41368
title: MSSQLSERVER_41368 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41368 (Database Engine error)
ms.assetid: abc71559-4c4d-4cce-a08f-3299dd167842
author: MashaMSFT
ms.author: mathoma
robots: noindex,nofollow
ms.openlocfilehash: 97fec0c4cb8f24fe7e11b0399fa60289d68b7b07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179493"
---
# <a name="mssqlserver_41368"></a>MSSQLSERVER_41368
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|41368|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SQL_IMPLICIT_AND_EXPLICIT_TX_NOT_SUPPORTED|  
|Texto del mensaje|El acceso a las tablas optimizadas en memoria con el nivel de aislamiento READ COMMITTED se admite solo para las transacciones de confirmación automática. No se admite para las transacciones explícitas o implícitas. Proporciona un nivel de aislamiento admitido para la tabla optimizada en memoria mediante una sugerencia de tabla, como WITH (SNAPSHOT).|  
  
## <a name="explanation"></a>Explicación  
El acceso a tablas optimizadas para memoria con el nivel de aislamiento READ COMMITTED solo se admite para las transacciones de confirmación automática. Para obtener más información, consulte [Transacciones con tablas en memoria y procedimientos](~/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md).  
  
Al obtener acceso a una tabla optimizada para memoria desde una transacción explícita iniciada con BEGIN TRANSACTION, o desde una transacción implícita, si IMPLICIT_TRANSACTIONS se establece en ON, no se admite el nivel de aislamiento READ COMMITTED.  
  
## <a name="user-action"></a>Acción del usuario  
Utilice SNAPSHOT para tener acceso a una tabla optimizada para memoria desde una transacción READ COMMITTED explícita o implícita. Se puede tener acceso mediante la sugerencia de tabla WITH (SNAPSHOT) (para obtener más información, vea [Transacciones con tablas en memoria y procedimientos](~/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables.md)) o estableciendo la opción de base de datos MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT en ON (para obtener más información, vea [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql.md)).  
  
## <a name="see-also"></a>Consulte también  
[OLTP en memoria &#40;optimización en memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
