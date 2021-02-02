---
description: '&#x40;&#x40;PACKET_ERRORS (Transact-SQL)'
title: '@@PACKET_ERRORS (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 09/18/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '@@PACKET_ERRORS'
- '@@PACKET_ERRORS_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@PACKET_ERRORS function'
- number of packet errors
- packets [SQL Server], errors
- networking [SQL Server], packet errors
- connections [SQL Server], packets
ms.assetid: f7da1b80-5cbe-42fa-be71-40c6af16383a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: a5511972e841d0dc4c2f0ef51b9d398cc26024bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99100463"
---
# <a name="x40x40packet_errors-transact-sql"></a>&#x40;&#x40;PACKET_ERRORS (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el número de errores de paquetes de red en las conexiones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde la última vez que se inició [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
@@PACKET_ERRORS  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **integer**  
  
## <a name="remarks"></a>Comentarios  
 Para ver un informe que contenga varias estadísticas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], incluidos los errores de paquetes, ejecute **sp_monitor**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra el uso de `@@PACKET_ERRORS`.  
  
```sql  
SELECT @@PACKET_ERRORS AS 'Packet Errors';  
```  
  
 A continuación se muestra un conjunto de resultados de ejemplo.  
  
```  
Packet Errors  
-------------  
0  
```  
  
## <a name="see-also"></a>Consulte también  
 [@@PACK_RECEIVED &#40;Transact-SQL&#41;](../../t-sql/functions/pack-received-transact-sql.md)   
 [@@PACK_SENT &#40;Transact-SQL&#41;](../../t-sql/functions/pack-sent-transact-sql.md)   
 [sp_monitor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [Funciones estadísticas del sistema &#40;Transact-SQL&#41;](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  
