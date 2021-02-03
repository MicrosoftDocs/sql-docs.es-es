---
description: Read (motor de base de datos) mediante CSharp
title: Read (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- Read_TSQL
- Read
dev_langs:
- TSQL
helpviewer_keywords:
- Read [Database Engine]
ms.assetid: f2b8207c-b69f-4327-a874-100b3a1f27d8
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4acfebf93f27a64fd0902a3bf9a2bad4e6d5ffe3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187846"
---
# <a name="read-database-engine-by-using-csharp"></a>Read (motor de base de datos) mediante CSharp
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Read lee una representación binaria de **SqlHierarchyId** desde la clase **BinaryReader** pasada y establece el objeto **SqlHierarchyId** en dicho valor. No se puede llamar a Read con [!INCLUDE[tsql](../../includes/tsql-md.md)]. En su lugar, use CAST o CONVERT.
  
## <a name="syntax"></a>Sintaxis  

<!--
This is not T-SQL, despite the ```sql colorizer specified.
Neither should this be ```syntaxsql.
Rather, this is C# (or C# syntax).  Same for the later code blocks.
I am making this fix now, from ```sql to ```cs, on 2020/04/16.  GeneMi.
-->

```csharp
void Read( BinaryReader r )   
```  

## <a name="arguments"></a>Argumentos
*r*  
 El objeto **BinaryReader** que produce un flujo binario correspondiente a una representación binaria de un nodo **hierarchyid**.  
  
## <a name="return-types"></a>Tipos de valores devueltos
 **Tipo de valor devuelto de CLR: void**  
  
## <a name="remarks"></a>Observaciones  
 Read no valida su entrada. Si se proporciona una entrada binaria no válida, Read puede provocar una excepción. O bien, la operación puede realizarse sin errores y producir un objeto **SqlHierarchyId** no válido cuyos métodos pueden producir resultados imprevisibles o provocar una excepción.  
  
 Solo se puede llamar a Read en un objeto **SqlHierarchyId** recién creado.  
  
 Read usa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] internamente cuando es necesario, por ejemplo, al escribir datos en la columna **hierarchyid**. También se llama a Read internamente cuando se realiza una conversión entre **varbinary** y **hierarchyid**.  
  
## <a name="examples"></a>Ejemplos  
  
```csharp
Byte[] encoding = new byte[] { 0x58 };  
MemoryStream stream = new MemoryStream(encoding, false /*not writable*/);  
BinaryReader br = new BinaryReader(stream);  
SqlHierarchyId hid = new SqlHierarchyId();  
hid.Read(br);   
```  
  
## <a name="see-also"></a>Consulte también  
[Write &#40;motor de base de datos&#41;](../../t-sql/data-types/write-database-engine.md)  
[ToString &#40;motor de base de datos&#41;](../../t-sql/data-types/tostring-database-engine.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Referencia de los métodos del tipo de datos hierarchyid](./hierarchyid-data-type-method-reference.md)
  
