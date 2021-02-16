---
description: sp_db_selective_xml_index (Transact-SQL)
title: sp_db_selective_xml_index (Transact-SQL)
ms.custom: ''
ms.date: 02/11/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_selective_xml_index_TSQL
- sp_db_selective_xml_index
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_selective_xml_index procedure
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed30d2b8df6141b308d32dee8f2fdcec0c2eceb1
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525181"
---
# <a name="sp_db_selective_xml_index-transact-sql"></a>sp_db_selective_xml_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Habilita y deshabilita la funcionalidad de Índice XML selectivo en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si se llama sin ningún parámetro, el procedimiento almacenado devuelve 1 si el índice XML selectivo está habilitado en una base de datos determinada.  

> [!NOTE]  
> A partir de [!INCLUDE[sssql14-md](../../includes/sssql14-md.md)] , no se puede deshabilitar la funcionalidad de índice XML selectivo. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] , para deshabilitar la característica de índice XML selectivo mediante este procedimiento almacenado, la base de datos se debe colocar en el modelo de recuperación simple mediante el comando [ALTER database Set &#40;TRANSACT-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
      sys.sp_db_selective_xml_index[[ @dbname = ] 'dbname'],   
[[ @selective_xml_index = ] 'selective_xml_index']  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @ dbname = ] 'dbname'` Nombre de la base de datos en la que se va a habilitar o deshabilitar el índice XML selectivo. Si *dbname* es null, se supone que es la base de datos actual. *@dbname* es de **tipo sysname**.


`[ @selective_xml_index = ] 'selective_xml_index'` Determina si se debe habilitar o deshabilitar el índice. Valores permitidos: ' on ', ' OFF ', ' true ', ' false '. Si se pasa otro valor, excepto ' on ', ' true ', ' OFF ' o ' false ', se producirá un error. *@selective_xml_index* es de tipo **VARCHAR (6)**.

  
## <a name="return-code-values"></a>Valores de código de retorno  
 **1** si el índice XML selectivo está habilitado en una base de datos determinada, **0** si está deshabilitado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-enable-selective-xml-index-functionality"></a>A. Habilitar la funcionalidad de índice XML selectivo  
 En el ejemplo siguiente se habilita el índice XML selectivo en la base de datos actual.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'on';  
GO  
```  
  
 En el ejemplo siguiente se habilita el índice XML selectivo en la base de datos AdventureWorks2012.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'true';  
GO  
```  
  
### <a name="b-disable-selective-xml-index-functionality"></a>B. Deshabilitar la funcionalidad de índice XML selectivo  
 En el ejemplo siguiente se deshabilita el índice XML selectivo en la base de datos actual.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = NULL  
  , @selective_xml_index = N'off';  
GO  
```  
  
 En el ejemplo siguiente se deshabilita el índice XML selectivo en la base de datos AdventureWorks2012.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index  
    @dbname = N'AdventureWorks2012'  
  , @selective_xml_index = N'false';  
GO  
```  
  
### <a name="c-detect-if-selective-xml-index-is-enabled"></a>C. Detectar si el índice XML selectivo está habilitado  
 En el ejemplo siguiente se detecta si el índice XML selectivo está habilitado. Devuelve 1 si el índice XML selectivo está habilitado.  
  
```sql
EXECUTE sys.sp_db_selective_xml_index;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Índices XML selectivos &#40;SXI&#41;](../../relational-databases/xml/selective-xml-indexes-sxi.md)  
   