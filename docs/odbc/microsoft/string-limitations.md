---
description: Limitaciones de cadena
title: Limitaciones de las cadenas | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC]
- desktop database drivers [ODBC]
ms.assetid: ec1da65f-c69d-415d-bf75-8fda8aa2b39f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12d02a6e69b34c13219e4bb5f00aeacfa9945cfb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188925"
---
# <a name="string-limitations"></a>Limitaciones de cadena
La longitud máxima de una cadena de instrucción SQL es de 65.000 caracteres.  
  
 Cuando se usa el controlador de Microsoft Access, solo se admiten las constantes de cadena de SQL-92 (con comillas simples, no comillas dobles).  
  
 El carácter de barra vertical (&#124;) no se puede usar en una cadena, tanto si el carácter está entre comillas como si no.  
  
 Para obtener la máxima interoperabilidad, las aplicaciones deben pasar cadenas en parámetros, en lugar de pasar cadenas entre comillas.
