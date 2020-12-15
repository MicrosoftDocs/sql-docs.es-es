---
description: sys.sysreferences (Transact-SQL)
title: Referencias de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysreferences
- sys.sysreferences_TSQL
- sysreferences
- sysreferences_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysreferences compatibility view
- sysreferences system table
ms.assetid: 81276f13-202e-4e74-962d-46eb98c98d2e
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2911993e51f7404b0b1b49fe8bf43af722623e28
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482856"
---
# <a name="syssysreferences-transact-sql"></a>sys.sysreferences (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene asignaciones de definiciones de restricciones FOREIGN KEY a las columnas a las que se hace referencia en la base de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**constid**|**int**|Id. de la restricción FOREIGN KEY.|  
|**fkeyid**|**int**|Id. de la tabla que hace referencia.|  
|**rkeyid**|**int**|Id. de la tabla a la que se hace referencia.|  
|**rkeyindid**|**smallint**|Id. del índice exclusivo de la tabla a la que se hace referencia que abarca las columnas de clave con referencia.|  
|**keycnt**|**smallint**|Número de columnas de la clave.|  
|**forkeys**|**varbinary(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**refkeys**|**varbinary(32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**fkeydbid**|**smallint**|Reservado.|  
|**rkeydbid**|**smallint**|Reservado.|  
|**fkey1**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey2**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey3**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey4**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey5**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey6**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey7**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey8**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey9**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey10**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey11**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey12**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey13**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey14**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey15**|**smallint**|Id. de la columna que hace referencia.|  
|**fkey16**|**smallint**|Id. de la columna que hace referencia.|  
|**rkey1**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey2**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey3**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey4**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey5**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey6**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey7**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey8**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey9**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey10**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey11**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey12**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey13**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey14**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey15**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
|**rkey16**|**smallint**|ID. de columna de la columna a la que se hace referencia.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
