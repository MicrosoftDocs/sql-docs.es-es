---
title: Expresiones lógicas (XQuery) | Microsoft Docs
description: Obtenga información sobre las expresiones lógicas admitidas en XQuery.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- logical operators [SQL Server], XQuery
- effective Boolean value [XQuery]
- logical expressions [XQuery]
- EBV
- expressions [XQuery], logical
ms.assetid: de94cd2e-2d48-49fb-9ebd-a2d90c79bf62
author: rothja
ms.author: jroth
ms.openlocfilehash: 44f05374320dc2d5c329ad234e62dd0c420bf22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341854"
---
# <a name="logical-expressions-xquery"></a>Expresiones lógicas (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  XQuery admite los operadores **lógicos and y** **or** .  
  
```  
expression1 and expression2  
expression1 or expression2  
```  
  
 Las expresiones de prueba, `expression1,``expression2` , en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pueden dar como resultado una secuencia vacía, una secuencia de uno o más nodos o un solo valor booleano. En función del resultado, se determinará su valor booleano efectivo de la forma siguiente:    
  
-   Si la expresión de prueba da como resultado una secuencia vacía, el resultado será False.  
  
-   Si la expresión de prueba da como resultado un solo valor booleano, este valor será el resultado de la expresión.  
  
-   Si la expresión de prueba da como resultado una secuencia de uno o varios nodos, el resultado de la expresión será True.  
  
-   De lo contrario, se generará un error estático.  
  
 A continuación, se aplican los **operadores lógicos and y** **or** a los valores booleanos resultantes de las expresiones con la semántica lógica estándar.  
  
 La siguiente consulta recupera del catálogo de productos las imágenes pequeñas de ángulo delantero, el elemento <`Picture`> para un modelo de producto específico. Tenga en cuenta que para cada documento de descripción de productos, se pueden almacenar en el catálogo una o varias imágenes de producto con distintos atributos, como el tamaño y el ángulo.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     for $F in /PD:ProductDescription/PD:Picture[PD:Size="small"   
                                                 and PD:Angle="front"]  
     return   
         $F   
    ') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 El resultado es el siguiente:  
  
```  
<PD:Picture   
  xmlns:PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
  <PD:Angle>front</PD:Angle>  
  <PD:Size>small</PD:Size>  
  <PD:ProductPhotoID>31</PD:ProductPhotoID>  
</PD:Picture>  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Expresiones XQuery](../xquery/xquery-expressions.md)  
  
  
