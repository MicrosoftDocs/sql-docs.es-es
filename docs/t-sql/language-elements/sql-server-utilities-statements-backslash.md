---
description: Barra diagonal inversa (continuación de línea) (Transact-SQL)
title: Barra diagonal inversa (continuación de línea) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '\_TSQL'
- '\'
dev_langs:
- TSQL
helpviewer_keywords:
- backwhack
- backslash
- escape character
- hack character
- '\ (backslash)'
- backslant
- bash
- reverse slant
- slosh
- reversed virgule
- line continuation character
- reverse solidus
ms.assetid: c97fbb20-3d12-4d0b-9b52-62a229bc83c0
author: cawrites
ms.author: chadam
ms.openlocfilehash: f205f5fe06767e4f27cc7e695b02c2e3ed824dc6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200501"
---
# <a name="backslash-line-continuation-transact-sql"></a>Barra diagonal inversa (continuación de línea) (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

`\` divide una constante de cadena larga, de carácter o binaria, en dos o más líneas para facilitar la lectura.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
<first section of string> \  
<continued section of string>  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 \<first section of string>  
 Es el principio de una cadena.  
  
 \<continued section of string>  
 Es la continuación de una cadena.  
  
## <a name="remarks"></a>Observaciones  
Este comando devuelve las secciones primera y de continuación de la cadena como una cadena, sin la barra diagonal inversa. La nueva línea después de la barra diagonal inversa debe ser un carácter de avance de línea (U + 000A) o una combinación de retorno de carro (U + 000D) y avance de línea (U + 000A) en ese orden. 

## <a name="examples"></a>Ejemplos  

### <a name="a-splitting-a-character-string"></a>A. División de una cadena de caracteres  

En el ejemplo siguiente se usa una barra diagonal inversa y un retorno de carro para dividir una cadena de caracteres en dos líneas.  
  
```sql  
SELECT 'abc\  
def' AS [ColumnResult];  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 ColumnResult  
 ------------  
 abcdef
 ```    

### <a name="b-splitting-a-binary-string"></a>B. División de una cadena binaria  

En el ejemplo siguiente se usa una barra diagonal inversa y un retorno de carro para dividir una cadena binaria en dos líneas.  

```sql  
SELECT 0xabc\
def AS [ColumnResult];  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 ColumnResult  
 ------------  
 0xABCDEF
 ```    

## <a name="see-also"></a>Consulte también  
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [&#40;Divisió&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/divide-transact-sql.md)   
 [&#40;Asignación y División&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/divide-equals-transact-sql.md)   
 [Operadores compuestos &#40;Transact-SQL&#41;](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  
