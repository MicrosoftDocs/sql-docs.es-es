---
description: CHECKSUM (Transact-SQL)
title: CHECKSUM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHECKSUM_TSQL
- CHECKSUM
dev_langs:
- TSQL
helpviewer_keywords:
- hash indexes
- CHECKSUM function
- checksum values
ms.assetid: e26d3339-845c-49c2-9d89-243376874c13
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 913ed7ab201c522cd7029365e739c4914c7e8892
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193412"
---
# <a name="checksum-transact-sql"></a>CHECKSUM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

La función `CHECKSUM` devuelve el valor de la suma de comprobación calculado sobre una fila de una tabla o sobre una lista de expresiones. Use `CHECKSUM` para generar índices hash.
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CHECKSUM ( * | expression [ ,...n ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumentos
\*  
Este argumento especifica que el cálculo de la suma de comprobación cubre todas las columnas de la tabla. `CHECKSUM` devuelve un error si alguna columna tiene un tipo de datos no comparable. Entre los tipos de datos no comparables están los siguientes:

- **cursor**
- **image**
- **ntext**
- **text**
- **XML**

Otro tipo de datos no comparable es **sql_variant** con cualquiera de los tipos de datos anteriores como tipo base.
  
*expression*  
Una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de cualquier tipo, excepto un tipo de datos no comparable.
  
## <a name="return-types"></a>Tipos de valores devueltos
 **int**  
  
## <a name="remarks"></a>Observaciones  
`CHECKSUM` calcula un valor hash, denominado suma de comprobación, sobre su lista de argumentos. Use este valor hash para generar índices hash. Se generará un índice hash si la función `CHECKSUM` tiene argumentos de columna y se crea un índice sobre el valor `CHECKSUM` calculado. Éste se puede utilizar para búsquedas de igualdades sobre las columnas.
  
La función `CHECKSUM` cumple las propiedades de una función hash: si se aplica `CHECKSUM` sobre dos listas de expresiones, se devolverá el mismo valor si los elementos correspondientes de ambas listas tienen el mismo tipo de datos y estos elementos son iguales cuando se comparan con el operador igual (=). Se definen valores NULL de un tipo especificado para compararse como iguales con fines de la función `CHECKSUM`. Si al menos uno de los valores de la lista de expresiones cambia, la suma de comprobación de la lista probablemente cambiará, aunque dicho extremo no está garantizado. Por lo tanto, para detectar si los valores han cambiado, se recomienda usar `CHECKSUM` solo si su aplicación puede tolerar una posible ausencia de cambio. De lo contrario, es más aconsejable usar `HASHBYTES`. Con un algoritmo hash MD5 específico, la probabilidad de que `HASHBYTES` devuelva el mismo resultado (de dos entradas diferentes) es mucho menor en comparación con `CHECKSUM`.
  
El orden de las expresiones afecta al valor `CHECKSUM` calculado. El orden de las columnas usado para `CHECKSUM(*)` es el orden de las columnas especificado en la definición de la tabla o la vista. Esto incluye las columnas calculadas.
  
El valor de `CHECKSUM` depende de la intercalación. El mismo valor almacenado con una intercalación diferente devolverá un valor de `CHECKSUM` diferente.
  
`CHECKSUM ()` no garantiza resultados únicos.

## <a name="examples"></a>Ejemplos  
En estos ejemplos se muestra el uso de `CHECKSUM` para generar índices hash.
  
Para crear el índice hash, en el primer ejemplo se agrega una columna de suma de comprobación calculada a la tabla que se quiere indizar. Luego se crea un índice en la columna de la suma de comprobación. 
  
```sql
-- Create a checksum index.  

SET ARITHABORT ON;  
USE AdventureWorks2012;   
GO  
ALTER TABLE Production.Product  
ADD cs_Pname AS CHECKSUM(Name);  
GO  
CREATE INDEX Pname_index ON Production.Product (cs_Pname);  
GO  
```  
  
En este ejemplo se muestra el uso de un índice de suma de comprobación como índice hash. Esto puede ayudar a mejorar la velocidad de indización cuando la columna que se quiere indizar es una columna de cadenas largas de caracteres. El índice de suma de comprobación se puede utilizar para búsquedas de igualdades.
  
```sql
/*Use the index in a SELECT query. Add a second search   
condition to catch stray cases where checksums match,   
but the values are not the same.*/  

SELECT *   
FROM Production.Product  
WHERE CHECKSUM(N'Bearing Ball') = cs_Pname  
AND Name = N'Bearing Ball';  
GO  
```  
  
La creación del índice en la columna calculada se materializa en la columna de suma de comprobación y cualquier cambio en el valor de `ProductName` se propagará a la columna de suma de comprobación. Como alternativa se puede crear un índice directamente en la columna que se quiere indizar, aunque para los valores de clave largos es probable que un índice normal no se comporte tan bien como un índice de suma de comprobación.
  
## <a name="see-also"></a>Consulte también
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)  
[HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
  
  
