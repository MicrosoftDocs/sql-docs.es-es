---
description: Secuencia de escape LIKE
title: LIKE (secuencia de escape) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC escape sequences [ODBC], LIKE
- LIKE escape sequence [ODBC]
- escape sequences [ODBC], LIKE
ms.assetid: 798d75ea-be9d-4bef-b297-318bc327f1ca
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d32b0470cb2adf0960e23dac17d8cb4942f296c0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184392"
---
# <a name="like-escape-sequence"></a>Secuencia de escape LIKE
ODBC usa secuencias de escape para la cláusula LIKE. La sintaxis de esta secuencia de escape es la siguiente:  
  
```  
{'escape-character'}  
```  
  
## <a name="remarks"></a>Observaciones  
 En la notación BNF, la sintaxis es la siguiente:  
  
 *ODBC-like-escape* :: =  
  
 *ODBC-ESC-Initiator* escape '*carácter de escape*' *ODBC-ESC-Terminator*  
  
 carácter *de escape* : *: =*  
  
 *ODBC-ESC-Initiator* :: = {  
  
 *ODBC-ESC-Terminator* :: =}  
  
 Para determinar si el controlador admite la secuencia de escape LIKE, una aplicación puede llamar a **SQLGetInfo** con el tipo de información SQL_LIKE_ESCAPE_CLAUSE.
