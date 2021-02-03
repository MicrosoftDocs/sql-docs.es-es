---
description: HASHBYTES (Transact-SQL)
title: HASHBYTES (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HASHBYTES_TSQL
- HASHBYTES
dev_langs:
- TSQL
helpviewer_keywords:
- hash input
- HASHBYTES
ms.assetid: 0ea6a4d1-313e-4f70-b939-dd2cd570f6d6
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dd4bc13a7d1e6d980ceb391c7dd548bf71a5acd3
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233223"
---
# <a name="hashbytes-transact-sql"></a>HASHBYTES (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve el hash MD2, MD4, MD5, SHA, SHA1 o SHA2 de su entrada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
HASHBYTES ( '<algorithm>', { @input | 'input' } )  
  
<algorithm>::= MD2 | MD4 | MD5 | SHA | SHA1 | SHA2_256 | SHA2_512   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

`<algorithm>`  
Identifica el algoritmo hash que se va a utilizar para realizar el hash de la entrada. Es un argumento requerido y no tiene valor predeterminado. Las comillas simples son necesarias. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], todos los algoritmos están en desuso, salvo SHA2_256 y SHA2_512.  
  
`@input`  
Especifica una variable que contiene los datos en los que se va a realizar el hash. `@input` es **varchar**, **nvarchar** o **varbinary**.  
  
"*input*"  
Especifica una expresión que se evalúa como un carácter o una cadena binaria que se va a aplicar el algoritmo hash.  
  
 La salida se ajusta al algoritmo estándar: 128 bits (16 bytes) para MD2, MD4 y MD5; 160 bits (20 bytes) para SHA y SHA1; 256 bits (32 bytes) para SHA2_256 y 512 bits (64 bytes) para SHA2_512.  
  
**Válido para** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.
  
 En [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones anteriores, los valores de entrada permitidos tienen un límite de 8000 bytes.  
  
## <a name="return-value"></a>Valor devuelto  
 **varbinary** (máximo de 8000 bytes)  

## <a name="remarks"></a>Observaciones  
Plantéese usar `CHECKSUM` o `BINARY_CHECKSUM` como alternativas para calcular un valor hash.

Los algoritmos MD2, MD4, MD5, SHA y SHA1 están en desuso desde [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Use SHA2_256 o SHA2_512 en su lugar. Los algoritmos antiguos seguirán funcionando, pero generarán un evento de desuso.

## <a name="examples"></a>Ejemplos  
### <a name="return-the-hash-of-a-variable"></a>Devuelve el valor hash de una variable  
 En el siguiente ejemplo se devuelve el hash `SHA2_256` de los datos **nvarchar** almacenados en la variable `@HashThis`.  
  
```sql  
DECLARE @HashThis NVARCHAR(32);  
SET @HashThis = CONVERT(NVARCHAR(32),'dslfdkjLK85kldhnv$n000#knf');  
SELECT HASHBYTES('SHA2_256', @HashThis);  
```  
  
### <a name="return-the-hash-of-a-table-column"></a>Devuelve el valor hash de una columna de tabla  
 En el ejemplo siguiente se devuelve el valor hash SHA2_256 de los valores de la columna `c1` de la tabla `Test1`.  
  
```sql  
CREATE TABLE dbo.Test1 (c1 NVARCHAR(32));  
INSERT dbo.Test1 VALUES ('This is a test.');  
INSERT dbo.Test1 VALUES ('This is test 2.');  
SELECT HASHBYTES('SHA2_256', c1) FROM dbo.Test1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------------------------------  
0x741238C01D9DB821CF171BF61D72260B998F7C7881D90091099945E0B9E0C2E3 
0x91DDCC41B761ACA928C62F7B0DA61DC763255E8247E0BD8DCE6B22205197154D  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también  
[Elegir un algoritmo de cifrado](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)
[CHECKSUM &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-transact-sql.md)
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
