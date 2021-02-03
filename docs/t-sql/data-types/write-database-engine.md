---
description: Write (motor de base de datos)
title: Write (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Write_TSQL
- Write
dev_langs:
- TSQL
helpviewer_keywords:
- Write [Database Engine]
ms.assetid: 7c554334-d2d9-4eae-a4ae-097aa4020e1a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: aba1271dd68224d237595a81d61b23d8db38b7c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211491"
---
# <a name="write-database-engine"></a>Write (motor de base de datos)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Write escribe una representación binaria de **SqlHierarchyId** en la clase **BinaryWriter** pasada. No se puede llamar a Write por medio de [!INCLUDE[tsql](../../includes/tsql-md.md)]. En su lugar, use CAST o CONVERT.
  
## <a name="syntax"></a>Sintaxis  
  
```csharp
void Write( BinaryWriter w )
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*w*  
Un objeto **BinaryWriter** en el que se escribirá la representación binaria de este nodo de **hierarchyid**.
  
## <a name="return-types"></a>Tipos de valor devueltos  
**Tipo de valor devuelto de CLR: void**
  
## <a name="remarks"></a>Observaciones  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa Write internamente cuando es necesario, por ejemplo, al cargar datos en la columna **hierarchyid**. También se llama a Write internamente cuando se realiza una conversión entre **hierarchyid** y **varbinary**.
  
## <a name="examples"></a>Ejemplos  
  
```csharp
MemoryStream stream = new MemoryStream();  
BinaryWriter bw = new BinaryWriter(stream);  
hid.Write(bw);  
byte[] encoding = stream.ToArray();  
```  
  
## <a name="see-also"></a>Vea también
[Read &#40;motor de base de datos&#41;](../../t-sql/data-types/read-database-engine.md)  
[ToString &#40;motor de base de datos&#41;](../../t-sql/data-types/tostring-database-engine.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Referencia de los métodos del tipo de datos hierarchyid](./hierarchyid-data-type-method-reference.md)
  
