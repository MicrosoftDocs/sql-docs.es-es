---
description: Comando de ANSI SET
title: ESTABLECER comando ANSI | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set ANSI command [ODBC]
ms.assetid: cf9a01b2-14bf-458c-a73c-2a58ddef32d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bb9dbd2b73b4ff0f7f75442c42de31dbe389211a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208667"
---
# <a name="set-ansi-command"></a>Comando de ANSI SET
Determina cómo se realizan las comparaciones entre cadenas de longitudes diferentes con el operador = en los comandos SQL de Visual FoxPro.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET ANSI ON | OFF  
```  
  
## <a name="arguments"></a>Argumentos  
 ACTIVAR  
 (Valor predeterminado para el controlador; el valor predeterminado para Visual FoxPro es OFF). Rellena la cadena más corta con los espacios en blanco necesarios para que sea igual a la longitud de la cadena más larga. En ese caso, las dos cadenas comparan el carácter para obtener sus longitudes completas. Considere esta comparación:  
  
```  
'Tommy' = 'Tom'  
```  
  
 El resultado es false (. F.) si SET ANSI está establecido en ON, dado que al rellenar, ' Tom ' se convierte en ' Tom ' y las cadenas ' Tom ' y ' Tommy ' no coinciden con el carácter del carácter.  
  
 El operador = = usa este método para realizar comparaciones en comandos SQL de Visual FoxPro.  
  
 Apagado  
 Especifica que la cadena más corta no se rellena con espacios en blanco. Las dos cadenas comparan el carácter del carácter hasta que se alcanza el final de la cadena más corta. Considere esta comparación:  
  
```  
'Tommy' = 'Tom'  
```  
  
 El resultado es true (. T.) cuando SET ANSI es OFF, ya que la comparación se detiene después de ' Tom '.  
  
## <a name="remarks"></a>Observaciones  
 SET ANSI determina si el menor de dos cadenas se rellenan con espacios en blanco cuando se realiza una comparación de cadenas SQL. SET ANSI no tiene ningún efecto en el operador = =; Cuando se usa el operador = =, la cadena más corta siempre se rellena con espacios en blanco para la comparación.  
  
## <a name="string-order"></a>Orden de cadena  
 En los comandos SQL, el orden de izquierda a derecha de las dos cadenas en una comparación es irrelevantswitching una cadena de un lado del operador = o = = a la otra no afecta al resultado de la comparación.  
  
## <a name="see-also"></a>Consulte también  
 [Comando SELECT-SQL](../../odbc/microsoft/select-sql-command.md)   
 [Comando exacto de conjunto](../../odbc/microsoft/set-exact-command.md)
