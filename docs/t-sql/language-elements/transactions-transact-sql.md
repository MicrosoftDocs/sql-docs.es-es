---
description: Transacciones (Transact-SQL)
title: Transacciones (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/25/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Transactions
- Transactions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- transactions [SQL Server]
- transactions [SQL Server], about transactions
- UOW [SQL Server]
- unit of work [SQL Server]
ms.assetid: 1485c375-921a-42af-a871-bb333cc08d3e
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fc99b1213fc15d6ed271c99dc7e70080be4c602a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189848"
---
# <a name="transactions-transact-sql"></a>Transacciones (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Una transacción es una unidad única de trabajo. Si una transacción tiene éxito, todas las modificaciones de los datos realizadas durante la transacción se confirman y se convierten en una parte permanente de la base de datos. Si una transacción encuentra errores y debe cancelarse o revertirse, se borran todas las modificaciones de los datos.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] funciona en los modos de transacción siguientes:  
  
 Transacciones de confirmación automática  
 Cada instrucción individual es una transacción.  
  
 Transacciones explícitas  
 Cada transacción se inicia explícitamente con la instrucción BEGIN TRANSACTION y se termina explícitamente con una instrucción COMMIT o ROLLBACK.  
  
 Transacciones implícitas  
 Se inicia implícitamente una nueva transacción cuando se ha completado la anterior, pero cada transacción se completa explícitamente con una instrucción COMMIT o ROLLBACK.  
  
 Transacciones de ámbito de lote  
 Una transacción implícita o explícita de [!INCLUDE[tsql](../../includes/tsql-md.md)] que se inicia en una sesión de MARS (conjuntos de resultados activos múltiples), que solo es aplicable a MARS, se convierte en una transacción de ámbito de lote. Si no se confirma o revierte una transacción de ámbito de lote cuando se completa el lote, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la revierte automáticamente.  

> [!NOTE] 
> Para obtener consideraciones especiales relacionadas con los productos de almacenamiento de datos, consulte [Transacciones (Azure Synapse Analytics)](transactions-sql-data-warehouse.md).   

## <a name="in-this-section"></a>En esta sección  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona las siguientes instrucciones de transacción:  
  
:::row:::
    :::column:::
        [BEGIN DISTRIBUTED TRANSACTION](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)
    :::column-end:::
    :::column:::
        [ROLLBACK TRANSACTION](../../t-sql/language-elements/rollback-transaction-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [BEGIN TRANSACTION](../../t-sql/language-elements/begin-transaction-transact-sql.md)
    :::column-end:::
    :::column:::
        [ROLLBACK WORK](../../t-sql/language-elements/rollback-work-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [COMMIT TRANSACTION](../../t-sql/language-elements/commit-transaction-transact-sql.md)
    :::column-end:::
    :::column:::
        [SAVE TRANSACTION](../../t-sql/language-elements/save-transaction-transact-sql.md)
    :::column-end:::
:::row-end:::  
:::row:::
    :::column:::
        [COMMIT WORK](../../t-sql/language-elements/commit-work-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Vea también  
 [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
  
  
