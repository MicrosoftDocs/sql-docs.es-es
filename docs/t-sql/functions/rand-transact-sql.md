---
description: RAND (Transact-SQL)
title: RAND (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RAND
- RAND_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RAND function
- values [SQL Server], random float
- random float value
ms.assetid: 363c84d6-b9fa-49ba-9a75-e44f27535ff6
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 344408b9cdaafe14954764a3715a44c8d4cd9bd5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345296"
---
# <a name="rand-transact-sql"></a>RAND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Devuelve un valor **float** pseudoaleatorio de 0 a 1, ambos excluidos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  

```syntaxsql  
RAND ( [ seed ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumentos
 *seed*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de tipo entero (**tinyint**, **smallint** o **int**) que proporciona el valor de inicialización. Si no se especifica *seed*, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] asigna un valor de inicialización de forma aleatoria. Para un valor de inicialización especificado, el resultado devuelto es siempre el mismo.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **float**  
  
## <a name="remarks"></a>Comentarios  
 Las llamadas repetitivas de RAND() con el mismo valor de inicialización devuelven los mismos resultados.  
  
 Para una conexión, si se llama a RAND() con el valor de inicialización especificado, todas las llamadas posteriores de RAND() generan resultados basados en la llamada a RAND() inicializada. Por ejemplo, la siguiente consulta siempre devuelve la misma secuencia de números.  
  
```sql  
SELECT RAND(100), RAND(), RAND()   
```  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se producen cuatro números aleatorios diferentes, generados con la función RAND.  
  
```sql  
DECLARE @counter SMALLINT;  
SET @counter = 1;  
WHILE @counter < 5  
   BEGIN  
      SELECT RAND() Random_Number  
      SET @counter = @counter + 1  
   END;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones matemáticas &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)  
  
  
