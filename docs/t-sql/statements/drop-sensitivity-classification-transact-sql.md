---
description: DROP SENSITIVITY CLASSIFICATION (Transact-SQL)
title: DROP SENSITIVITY CLASSIFICATION (Transact-SQL) | Microsoft Docs
ms.date: 03/25/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
ms.custom: ''
ms.author: giladm
author: giladmit
f1_keywords:
- DROP SENSITIVITY CLASSIFICATION
- DROP_SENSITIVITY_CLASSIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- DROP SENSITIVITY CLASSIFICATION statement
- dropping labels
- drop labels
- removing labels
- remove labels
- classification [SQL]
- labels [SQL]
- information types
- data classification
monikerRange: " >= sql-server-ver15 || = azuresqldb-current"
ms.openlocfilehash: 4c1eef43401cd55ef7983e620d3d2f73f221809c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340939"
---
# <a name="drop-sensitivity-classification-transact-sql"></a>DROP SENSITIVITY CLASSIFICATION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Quita los metadatos de la clasificación de confidencialidad de una o varias columnas de base de datos.

## <a name="syntax"></a>Sintaxis

```syntaxsql
DROP SENSITIVITY CLASSIFICATION FROM
    <object_name> [, ...n ]

<object_name> ::=
{
    [schema_name.]table_name.column_name
}
```  

## <a name="arguments"></a>Argumentos  

*object_name* ([schema_name.]table_name.column_name)

Es el nombre de la columna de base de datos de la que se va a quitar la clasificación. Actualmente solo se admite la clasificación de columnas.
    - *schema_name* (opcional): es el nombre del esquema al que pertenece la columna clasificada.
    - *table_name*: es el nombre de la tabla a la que pertenece la columna clasificada.
    - *column_name*: es el nombre de la columna de la que se va a quitar la clasificación.

## <a name="remarks"></a>Observaciones  

- Se pueden quitar varias clasificaciones de objetos con una sola instrucción "DROP SENSITIVITY CLASSIFICATION".

## <a name="permissions"></a>Permisos  

Requiere el permiso ALTER ANY SENSITIVITY CLASSIFICATION. ALTER ANY SENSITIVITY CLASSIFICATION está implícito en el permiso de base de datos ALTER, o en el permiso de servidor CONTROL SERVER.


## <a name="examples"></a>Ejemplos  


### <a name="a-dropping-classification-from-a-single-column"></a>A. Eliminación de la clasificación de una sola columna

En este ejemplo se quita la clasificación de la columna `dbo.sales.price`.  

```sql
DROP SENSITIVITY CLASSIFICATION FROM
    dbo.sales.price
```

### <a name="b-dropping-classification-from-multiple-columns"></a>B. Eliminación de la clasificación de varias columnas

En este ejemplo se quita la clasificación de las columnas `dbo.sales.price`, `dbo.sales.discount` y`SalesLT.Customer.Phone`.  

```sql
DROP SENSITIVITY CLASSIFICATION FROM
    dbo.sales.price, dbo.sales.discount, SalesLT.Customer.Phone  
```

## <a name="see-also"></a>Consulte también  

[ADD SENSITIVITY CLASSIFICATION (Transact-SQL)](../../t-sql/statements/add-sensitivity-classification-transact-sql.md)

[sys.sensitivity_classifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)

[Clasificación y detección de datos de Azure SQL Database](/azure/azure-sql/database/data-discovery-and-classification-overview)