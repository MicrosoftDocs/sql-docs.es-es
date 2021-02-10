---
description: Cláusulas COMPUTE de forma que intervengan
title: Cláusulas Compute Shape intermedias | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- COMPUTE clause [ADO]
- data shaping [ADO], COMPUTE clause
ms.assetid: a576bf81-8f3c-4ba1-817b-87e89a8da684
author: rothja
ms.author: jroth
ms.openlocfilehash: 1b19762b1e06a093d194636bb699dd7dcce368be
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037305"
---
# <a name="intervening-shape-compute-clauses"></a>Cláusulas COMPUTE de forma que intervengan
Es válido incrustar una o más cláusulas Compute entre el elemento primario y el elemento secundario en un comando Shape con parámetros, como en el ejemplo siguiente:  
  
```  
SHAPE {select au_lname, state from authors} APPEND   
   ((SHAPE   
      (SHAPE   
         {select * from authors where state = ?} rs   
      COMPUTE rs, ANY(rs.state) state, ANY(rs.au_lname) au_lname   
      BY au_id) rs2   
   COMPUTE rs2, ANY(rs2.state) BY au_lname)   
RELATE state TO PARAMETER 0)  
```  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de forma de datos](./data-shaping-example.md)   
 [Gramática de forma formal](./formal-shape-grammar.md)   
 [Comandos Shape en General](./shape-commands-in-general.md)