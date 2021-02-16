---
description: NEWID (Transact-SQL)
title: NEWID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NEWID
- NEWID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- uniqueidentifier data type
- NEWID function
ms.assetid: f7014e60-96d5-457e-afc3-72b60ba20c0f
author: julieMSFT
ms.author: jrasnick
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 85ef57b23e4e23c34f63a219c760ddb9c865aac2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341650"
---
# <a name="newid-transact-sql"></a>NEWID (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Crea un valor único del tipo **uniqueidentifier**.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql 
NEWID ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **uniqueidentifier**  
  
## <a name="remarks"></a>Comentarios  
 `NEWID()` es compatible con RFC4122.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-the-newid-function-with-a-variable"></a>A. Utilizar la función NEWID con una variable  
 En el siguiente ejemplo se usa `NEWID()` para asignar un valor a una variable declarada con el tipo de datos **uniqueidentifier**. El valor de la variable de tipo **uniqueidentifier** se imprime antes de ser comprobado.  
  
```sql
-- Creating a local variable with DECLARE/SET syntax.  
DECLARE @myid uniqueidentifier  
SET @myid = NEWID()  
PRINT 'Value of @myid is: '+ CONVERT(varchar(255), @myid)  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Value of @myid is: 6F9619FF-8B86-D011-B42D-00C04FC964FF  
```  
  
> [!NOTE]  
>  El valor que devuelve NEWID es distinto en cada equipo. Este número solo se indica a título ilustrativo.  
  
### <a name="b-using-newid-in-a-create-table-statement"></a>B. Utilizar NEWID en una instrucción CREATE TABLE  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  
 En el siguiente ejemplo se crea una tabla `cust` con un tipo de datos **uniqueidentifier** y se usa NEWID para llenarla con un valor predeterminado. Al asignar como valor predeterminado `NEWID()`, cada fila nueva y existente tendrá un valor único en la columna `CustomerID`.  
  
```sql
-- Creating a table using NEWID for uniqueidentifier data type.  
CREATE TABLE cust  
(  
 CustomerID uniqueidentifier NOT NULL  
   DEFAULT newid(),  
 Company VARCHAR(30) NOT NULL,  
 ContactName VARCHAR(60) NOT NULL,   
 Address VARCHAR(30) NOT NULL,   
 City VARCHAR(30) NOT NULL,  
 StateProvince VARCHAR(10) NULL,  
 PostalCode VARCHAR(10) NOT NULL,   
 CountryRegion VARCHAR(20) NOT NULL,   
 Telephone VARCHAR(15) NOT NULL,  
 Fax VARCHAR(15) NULL  
);  
GO  
-- Inserting 5 rows into cust table.  
INSERT cust  
(Company, ContactName, Address, City, StateProvince,   
 PostalCode, CountryRegion, Telephone, Fax)  
VALUES  
 ('Wartian Herkku', 'Pirkko Koskitalo', 'Torikatu 38', 'Oulu', NULL,  
 '90110', 'Finland', '981-443655', '981-443655')  
,('Wellington Importadora', 'Paula Parente', 'Rua do Mercado, 12', 'Resende', 'SP',  
 '08737-363', 'Brasil', '(14) 555-8122', '')  
,('Cactus Comidas para Ilevar', 'Patricio Simpson', 'Cerrito 333', 'Buenos Aires', NULL,   
 '1010', 'Argentina', '(1) 135-5555', '(1) 135-4892')  
,('Ernst Handel', 'Roland Mendel', 'Kirchgasse 6', 'Graz', NULL,  
 '8010', 'Austria', '7675-3425', '7675-3426')  
,('Maison Dewey', 'Catherine Dewey', 'Rue Joseph-Bens 532', 'Bruxelles', NULL,  
 'B-1180', 'Belgium', '(02) 201 24 67', '(02) 201 24 68');  
GO
```  
  
### <a name="c-using-uniqueidentifier-and-variable-assignment"></a>C. Utilizar uniqueidentifier y la asignación de variable  
 En este ejemplo se declara una variable local denominada `@myid` como variable del tipo de datos **uniqueidentifier**. A continuación, se asigna un valor a la variable con la instrucción `SET`.  
  
```sql  
DECLARE @myid uniqueidentifier ;  
SET @myid = 'A972C577-DFB0-064E-1189-0154C99310DAAC12';  
SELECT @myid;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [NEWSEQUENTIALID &#40;Transact-SQL&#41;](../../t-sql/functions/newsequentialid-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [uniqueidentifier &#40;Transact-SQL&#41;](../../t-sql/data-types/uniqueidentifier-transact-sql.md)   
 [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
