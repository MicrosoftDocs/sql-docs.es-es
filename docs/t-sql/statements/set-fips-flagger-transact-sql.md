---
description: SET FIPS_FLAGGER (Transact-SQL)
title: SET FIPS_FLAGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FIPS_FLAGGER
- SET_FIPS_FLAGGER_TSQL
- FIPS_FLAGGER_TSQL
- SET FIPS_FLAGGER
dev_langs:
- TSQL
helpviewer_keywords:
- SET FIPS_FLAGGER statement
- FIPS 127-2 standard
- FIPS_FLAGGER option
ms.assetid: e82f6bee-6cf6-4061-be22-9ad2e8e9d3d6
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bb266f6aadef4690df7ebb7f16a17e1c5ae2ca05
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190392"
---
# <a name="set-fips_flagger-transact-sql"></a>SET FIPS_FLAGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Especifica la comprobación del cumplimiento del estándar FIPS 127-2. Esto se basa en el estándar ISO. Para más información sobre la compatibilidad con SQL Server FIPS, vea [How to use SQL Server 2016 in FIPS 140-2-compliant mode](https://support.microsoft.com/help/4014354/how-to-use-sql-server-2016-in-fips-140-2-compliant-mode) (Cómo usar SQL Server 2016 en el modo compatible con FIPS 140-2). 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SET FIPS_FLAGGER ( 'level' |  OFF )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **'** *level* **'**  
 Es el nivel de cumplimiento del estándar FIPS 127-2 que se comprueba en todas las operaciones de base de datos. Si una operación de la base de datos entra en conflicto con el nivel elegido de los estándares ISO, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] genera una advertencia.  
  
 *level* debe tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|ENTRY|Comprobación de los estándares ISO de compatibilidad con el nivel básico.|  
|FULL|Comprobación de los estándares ISO de compatibilidad plena.|  
|INTERMEDIATE|Comprobación de los estándares ISO de compatibilidad con el nivel intermedio.|  
|Apagado|Sin comprobación del estándar.|  
  
## <a name="remarks"></a>Comentarios  
 El valor de `SET FIPS_FLAGGER` se establece en tiempo de análisis, en lugar de en tiempo de ejecución. El hecho de que se establezca en tiempo de análisis supone que si la instrucción SET está presente en el lote o el procedimiento almacenado, se aplica aunque la ejecución del código no llegue al punto donde se encuentre. Además, la instrucción `SET` se aplica antes de que se ejecute ninguna otra instrucción. Por ejemplo, aunque la instrucción `SET` se encuentre en un bloque de instrucciones de `IF...ELSE` al que nunca se llega durante la ejecución, la instrucción `SET` se seguirá aplicando porque se ha analizado el bloque de instrucciones `IF...ELSE`.  
  
 Si `SET FIPS_FLAGGER` se establece en un procedimiento almacenado, el valor de `SET FIPS_FLAGGER` se restablecerá cuando el procedimiento almacenado devuelva el control. Por tanto, una instrucción `SET FIPS_FLAGGER` especificada en SQL dinámico no tiene ningún efecto en las instrucciones siguientes de SQL dinámico.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="see-also"></a>Vea también  
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
