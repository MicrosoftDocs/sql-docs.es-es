---
description: sp_addtabletocontents (Transact-SQL)
title: sp_addtabletocontents (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addtabletocontents_TSQL
- sp_addtabletocontents
helpviewer_keywords:
- sp_addtabletocontents
ms.assetid: 2ea27001-74f4-463e-bf1b-b6b5a86b9219
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 469d9255813e1a072539e6aa8ca7035378ce2b92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198403"
---
# <a name="sp_addtabletocontents-transact-sql"></a>sp_addtabletocontents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Inserta en las tablas de seguimiento de mezcla las referencias para las filas de una tabla de origen que no están incluidas actualmente en las tablas de seguimiento. Utilice esta opción si ha cargado de forma masiva una gran cantidad de datos mediante **BCP**, que no activará los desencadenadores de seguimiento de mezcla. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addtabletocontents [ @table_name = ] 'table_name'  
    [ , [ @owner_name = ] 'owner_name' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @table_name = ] 'table_name'` Es el nombre de la tabla. *TABLE_NAME* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @owner_name = ] 'owner_name'` Es el nombre del propietario de la tabla. *owner_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @filter_clause = ] 'filter_clause'` Especifica una cláusula de filtro que controla qué filas de los datos recién cargados deben agregarse a las tablas de seguimiento de mezcla. *filter_clause* es de tipo **nvarchar (4000)** y su valor predeterminado es NULL. Si *filter_clause* es **null**, se agregan todas las filas de carga masiva.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_addtabletocontents** solo se utiliza en la replicación de mezcla.  
  
 El **ROWGUIDCOL** hace referencia a las filas de la *TABLE_NAME* y las referencias se agregan a las tablas de seguimiento de mezcla. **sp_addtabletocontents** debe usarse después de la copia masiva de datos en una tabla que se publica mediante la replicación de mezcla. El procedimiento almacenado inicia el seguimiento de las filas copiadas y garantiza que las nuevas filas se incluirán en la siguiente sincronización.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addtabletocontents**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
