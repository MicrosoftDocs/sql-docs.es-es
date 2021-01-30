---
description: Limitaciones de nombre de columna
title: Limitaciones de nombre de columna | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- desktop database drivers [ODBC], column names
- ODBC desktop database drivers [ODBC], column names
ms.assetid: 5a339f61-c52f-40ad-8deb-d785f72753d4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 46719ea0a0ff34e103252678e25242f010d9bd37
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190648"
---
# <a name="column-name-limitations"></a>Limitaciones de nombre de columna
Los nombres de columna pueden contener cualquier carácter válido (por ejemplo, espacios). Si los nombres de columna contienen caracteres excepto letras, números y caracteres de subrayado, el nombre debe delimitarse mediante la inclusión de comillas dobles (').  
  
 Cuando se usa el controlador de Microsoft Access o Microsoft Excel, los nombres de columna se limitan a 64 caracteres y los nombres más largos generan un error. Cuando se usa el controlador de Paradox, el nombre de columna máximo es de 25 caracteres. Cuando se usa el controlador de texto, el nombre de columna máximo es de 64 caracteres y los nombres más largos se truncan.  
  
 Cuando se usa el controlador dBASE, los caracteres con un valor ASCII superior a 127 se convierten en caracteres de subrayado.  
  
 Cuando se utiliza el controlador de Microsoft Excel, si los nombres de columna están presentes, deben estar en la primera fila. Un nombre que en Microsoft Excel usaría el carácter "!" debe ir entre comillas ('). El carácter "!" se convierte en el carácter "$", porque el carácter "!" no es válido en un nombre ODBC, incluso cuando el nombre está entre comillas. Todos los demás caracteres válidos de Microsoft Excel (excepto el carácter de barra vertical (&#124;)) se pueden usar en un nombre de columna, incluidos los espacios. Se debe utilizar un identificador delimitado para que un nombre de columna de Microsoft Excel incluya un espacio. Los nombres de columna no especificados se reemplazarán por nombres generados por el controlador, por ejemplo, "col1" para la primera columna.  
  
 El carácter de barra vertical (&#124;) no se puede usar en un nombre de columna, tanto si el nombre está entre comillas como si no.  
  
 Cuando se usa el controlador de texto, el controlador proporciona un nombre predeterminado si no se especifica un nombre de columna. Por ejemplo, el controlador llama a la primera columna F1, la segunda columna F2, etc.
