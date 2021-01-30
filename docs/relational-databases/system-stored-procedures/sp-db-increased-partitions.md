---
description: sp_db_increased_partitions
title: sp_db_increased_partitions | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_db_increased_partitions_TSQL
- sp_db_increased_partitions
dev_langs:
- TSQL
helpviewer_keywords:
- sp_db_increased_partitions
ms.assetid: a8c043ec-b504-4929-ac0e-8babaa99d989
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 2241ce80c8537fa16feaedbf5c0f1c316d13821b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201336"
---
# <a name="sp_db_increased_partitions"></a>sp_db_increased_partitions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Habilita o deshabilita la compatibilidad con hasta 15.000 particiones de la base de datos especificada.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dp_increased_partitions   
[ @dbname ] = 'database_name'   
[ , [ @increased_partitions = ] 'increased_partitions' ] [;]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @dbname =] '*database_name*'  
 Es el nombre de la base de datos. *dbname* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *dbname* , se usa la base de datos actual.  
  
 [ @increased_partitions =] '*increased_partitions*'  
 Habilita o deshabilita la compatibilidad con 15.000 particiones en la base de datos especificada. *increased_partitions* es de tipo **VARCHAR (6)** y su valor predeterminado es NULL. Los valores aceptados son 'ON' o 'TRUE' para habilitar la compatibilidad y 'OFF' o 'FALSE' para deshabilitarla. Si no se especifica *increased_partitions* , el procedimiento devuelve 1 para indicar que la compatibilidad está habilitada para la base de datos especificada o 0 para indicar que la compatibilidad está deshabilitada.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso ALTER DATABASE en la base de datos especificada.  
  
  
