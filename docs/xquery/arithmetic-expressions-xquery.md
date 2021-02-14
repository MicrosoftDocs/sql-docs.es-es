---
title: Expresiones aritméticas (XQuery) | Microsoft Docs
description: Obtenga información sobre las expresiones aritméticas en XQuery y qué operadores aritméticos se admiten.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- expressions [XQuery], arithmetic
- arithmetic expressions
ms.assetid: 90d675bf-56da-459a-9771-8cd13920a9fc
author: rothja
ms.author: jroth
ms.openlocfilehash: f63fd1f6064d28a7ee977162e8993f53f9233519
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340482"
---
# <a name="arithmetic-expressions-xquery"></a>Expresiones aritméticas (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Se admiten todos los operadores aritméticos, excepto **IDIV**. Los siguientes ejemplos ilustran el uso básico de operadores aritméticos:  
  
```  
DECLARE @x xml  
SET @x=''  
SELECT @x.query('2 div 2')  
SELECT @x.query('2 * 2')  
```  
  
 Dado que **IDIV** no se admite, una solución es usar el constructor **xs: Integer ()** :  
  
```  
DECLARE @x xml  
SET @x=''  
-- Following will not work  
-- SELECT @x.query('2 idiv 2')  
-- Workaround   
SELECT @x.query('xs:integer(2 div 3)')  
```  
  
 El tipo resultante de un operador aritmético se basa en los tipos de los valores de entrada. Si los operandos son de tipos distintos, uno o los dos, cuando sea necesario, se convertirán a un tipo base primitivo común, según la jerarquía de tipos. Para obtener información acerca de la jerarquía de tipos, vea [reglas de conversión de tipos en XQuery](../xquery/type-casting-rules-in-xquery.md).  
  
 La promoción de tipos numéricos se produce si los dos operandos son de tipos base numéricos distintos. Por ejemplo, si agrega **xs: decimal** a un **xs: Double** , primero cambiaría el valor decimal a Double. A continuación, se efectuaría la suma, que daría como resultado un valor double.  
  
 Los valores atómicos sin tipo se convierten al tipo base numérico del otro operando, o a **xs: Double** si el otro operando también está sin tipo.  
  
## <a name="implementation-limitations"></a>Limitaciones de la implementación  
 Éstas son las limitaciones:  
  
-   Los argumentos de los operadores aritméticos deben ser de tipo numérico o **untypedAtomic**.  
  
-   Las operaciones con valores **xs: Integer** dan como resultado un valor de tipo **xs: decimal** en lugar de **xs: Integer**.  
  
  
