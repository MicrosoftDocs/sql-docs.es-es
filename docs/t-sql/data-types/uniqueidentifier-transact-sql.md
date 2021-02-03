---
description: uniqueidentifier (Transact-SQL)
title: uniqueidentifier (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- uniqueidentifier
- uniqueidentifier_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- uniqueidentifier data type
- globally unique identifiers [SQL Server]
- GUIDs [SQL Server]
ms.assetid: b026035b-f3d2-4d70-989d-3884b4ca0233
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 41f90385398fc60fc198e22a74a36f810e84c985
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179662"
---
# <a name="uniqueidentifier-transact-sql"></a>uniqueidentifier (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Es un GUID de 16 bytes.
  
## <a name="remarks"></a>Observaciones  
Una columna o una variable local de tipo de datos **uniqueidentifier** se puede inicializar en un valor de las siguientes formas:
-   Mediante el uso de las funciones [NEWID](../../t-sql/functions/newid-transact-sql.md) o [NEWSEQUENTIALID](../../t-sql/functions/newsequentialid-transact-sql.md).    
-   Mediante la conversión a partir de una constante de cadena con el formato *xxxxxxxx*-*xxxx*-*xxxx*-*xxxx*-*xxxxxxxxxxxx*, donde cada *x* es un dígito hexadecimal en el intervalo 0-9 o a-f. Por ejemplo, 6F9619FF-8B86-D011-B42D-00C04FC964FF es un valor **uniqueidentifier** válido.  
  
Con los valores **uniqueidentifier** se pueden usar operadores de comparación. No obstante, no se implementa la ordenación mediante la comparación de los patrones de bits de los dos valores. Las únicas operaciones que se pueden realizar con un valor **uniqueidentifier** son comparaciones (=, <>, \<, >, \<=, >=) y comprobaciones para NULL (IS NULL e IS NOT NULL). No es posible utilizar otros operadores aritméticos. Con el tipo de datos **uniqueidentifier** se pueden usar todas las propiedades y restricciones de columna, excepto IDENTITY.
  
La replicación de mezcla y la replicación transaccional con suscripciones de actualización usan columnas **uniqueidentifier** para garantizar que las filas se identifican de forma única en varias copias de la tabla.
  
## <a name="converting-uniqueidentifier-data"></a>Convertir datos uniqueidentifier  
El tipo **uniqueidentifier** se considera un tipo de carácter para la conversión desde una expresión de caracteres y, por tanto, está sujeto a las reglas de truncamiento para la conversión a un tipo de carácter. Es decir, cuando se convierten expresiones de carácter a un tipo de datos de carácter de un tamaño distinto, se truncan los valores que son demasiado grandes para el nuevo tipo de datos. Vea la sección Ejemplos.
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

Estas herramientas y características no son compatibles con el tipo de datos `uniqueidentifier`:
- PolyBase
- [herramienta de carga dwloader](../../analytics-platform-system/dwloader.md) para almacenamiento de datos paralelos

## <a name="examples"></a>Ejemplos  
En el ejemplo siguiente se convierte un valor `uniqueidentifier` a un tipo de datos `char`.
  
```sql
DECLARE @myid uniqueidentifier = NEWID();  
SELECT CONVERT(CHAR(255), @myid) AS 'char';  
```  
  
En el ejemplo siguiente se muestra el truncamiento de los datos cuando el valor es demasiado largo para el tipo de datos al que se va a convertir. Como el tipo **uniqueidentifier** tiene un límite de 36 caracteres, se truncan los caracteres que superan esa longitud.
  
```sql
DECLARE @ID NVARCHAR(max) = N'0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong';  
SELECT @ID, CONVERT(uniqueidentifier, @ID) AS TruncatedValue;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
String                                       TruncatedValue  
-------------------------------------------- ------------------------------------  
0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong    0E984725-C51C-4BF4-9960-E1C80E27ABA0  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Consulte también
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md)  
[NEWSEQUENTIALID &#40;Transact-SQL&#41;](../../t-sql/functions/newsequentialid-transact-sql.md)    
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)
  
