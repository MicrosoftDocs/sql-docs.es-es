---
description: DBCC DROPCLEANBUFFERS (Transact-SQL)
title: DBCC DROPCLEANBUFFERS (Transact-SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROPCLEANBUFFERS
- DBCC_DROPCLEANBUFFERS_TSQL
- DROPCLEANBUFFERS_TSQL
- DBCC DROPCLEANBUFFERS
dev_langs:
- TSQL
helpviewer_keywords:
- clean buffers
- cold buffer cache
- buffer pools [SQL Server]
- dropping buffers
- removing buffers
- DBCC DROPCLEANBUFFERS statement
ms.assetid: a4121927-f2ce-4926-aa2c-9b1519dac048
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c463accb2ddc329855abafcd6a4f7aea19ce75b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340966"
---
# <a name="dbcc-dropcleanbuffers-transact-sql"></a>DBCC DROPCLEANBUFFERS (Transact-SQL)

[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

Quita del grupo de búferes todos los búferes borrados y quita del grupo de objetos de almacén de columnas los objetos de almacén de columnas.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis

Sintaxis para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSOD](../../includes/sssodfull-md.md)]:

```syntaxsql
DBCC DROPCLEANBUFFERS [ WITH NO_INFOMSGS ]  
```  
Sintaxis de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]:

```syntaxsql  
DBCC DROPCLEANBUFFERS ( COMPUTE | ALL ) [ WITH NO_INFOMSGS ]  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 WITH NO_INFOMSGS  
 Suprime todos los mensajes de información. Los mensajes informativos siempre se suprimen en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 COMPUTE  
 Purga la caché de datos en memoria de cada nodo de ejecución.  
  
 ALL  
 Purga la caché de datos en memoria de cada nodo de ejecución y del nodo de control. Este es el valor predeterminado si no se especifica ningún valor.  
  
## <a name="remarks"></a>Observaciones  
Utilice DBCC DROPCLEANBUFFERS para probar consultas con una caché de búferes COLD sin apagar y reiniciar el servidor.
Para quitar del grupo de búferes los búferes borrados y para quitar del grupo de objetos de almacén de columnas los objetos de almacén de columnas, use primero CHECKPOINT para crear una caché de búferes COLD. Así se obliga a que todas las páginas desfasadas de la base de datos actual se escriban en el disco y se borren los búferes. Una vez hecho esto, puede emitir el comando DBCC DROPCLEANBUFFERS para quitar todos los búferes del grupo de búferes.
  
## <a name="result-sets"></a>Conjuntos de resultados  
DBCC DROPCLEANBUFFERS en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve:
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Permisos  
Debe pertenecer al rol fijo de servidor `sysadmin` para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
Debe pertenecer al rol fijo de servidor `DB_OWNER` para [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
## <a name="see-also"></a>Consulte también  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)  
  
  
