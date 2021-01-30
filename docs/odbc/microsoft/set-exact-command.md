---
description: Comando exacto de conjunto
title: ESTABLECER comando exacto | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET EXACT command [ODBC]
ms.assetid: 9533d3e0-e7c1-49de-a3a3-0cc4373a91cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 34a22d320dbd099bd26db1b5396cc55a15a4b288
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203953"
---
# <a name="set-exact-command"></a>Comando exacto de conjunto
Especifica las reglas para comparar dos cadenas de longitudes diferentes.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET EXACT ON | OFF  
```  
  
## <a name="arguments"></a>Argumentos  
 ACTIVAR  
 Especifica que las expresiones deben coincidir con un carácter que sea equivalente. Los espacios en blanco finales de las expresiones se omiten en la comparación. Para la comparación, el menor de las dos expresiones se rellena a la derecha con espacios en blanco para que coincida con la longitud de la expresión más larga.  
  
 Apagado  
 (Valor predeterminado). Especifica que, para ser equivalente, las expresiones deben coincidir con el carácter del carácter hasta que se alcance el final de la expresión del lado derecho.  
  
## <a name="remarks"></a>Observaciones  
 La configuración exacta de SET no tiene ningún efecto si ambas cadenas tienen la misma longitud.  
  
## <a name="string-comparisons"></a>Comparaciones de cadenas  
 Visual FoxPro tiene dos operadores relacionales que comprueban si son iguales.  
  
 El operador = realiza una comparación entre dos valores del mismo tipo. Este operador es adecuado para comparar los datos de caracteres, numéricos, de fecha y lógicos.  
  
 Sin embargo, cuando se comparan expresiones de caracteres con el operador =, es posible que los resultados no sean exactamente los esperados. Las expresiones de caracteres comparan el carácter de izquierda a derecha hasta que una de las expresiones no sea igual a la otra, hasta que se alcance el final de la expresión del lado derecho del operador = (SET EXACT OFF) o hasta que se alcancen los extremos de ambas expresiones (SET EXACT ON).  
  
 Se puede utilizar el operador = = cuando se necesita una comparación exacta de los datos de caracteres. Si dos expresiones de caracteres se comparan con el operador = =, las expresiones en ambos lados del operador = = deben contener exactamente los mismos caracteres, incluidos los espacios en blanco, para que se considere igual. La configuración exacta se omite cuando las cadenas de caracteres se comparan mediante = =.  
  
 En la tabla siguiente se muestra cómo afectan a las comparaciones la opción de operador y el valor de configuración exacto. (Un carácter de subrayado representa un espacio en blanco).  
  
|De comparación|= EXACTO DESACTIVADO|= EXACT EN|= = EXACTO activado o desactivado|  
|----------------|------------------|-----------------|--------------------------|  
|"ABC" = "ABC"|Coincidir con|Coincidir con|Coincidir con|  
|"AB" = "ABC"|Ninguna coincidencia|Ninguna coincidencia|Ninguna coincidencia|  
|"ABC" = "AB"|Coincidir con|Ninguna coincidencia|Ninguna coincidencia|  
|"ABC" = "ab_"|Ninguna coincidencia|Ninguna coincidencia|Ninguna coincidencia|  
|"AB" = "ab_"|Ninguna coincidencia|Coincidir con|Ninguna coincidencia|  
|"ab_" = "AB"|Coincidir con|Coincidir con|Ninguna coincidencia|  
|"" = "AB"|Ninguna coincidencia|Ninguna coincidencia|Ninguna coincidencia|  
|"AB" = ""|Coincidir con|Ninguna coincidencia|Ninguna coincidencia|  
|"__" = ""|Coincidir con|Coincidir con|Ninguna coincidencia|  
|"" = "___"|Ninguna coincidencia|Coincidir con|Ninguna coincidencia|  
|TRIM ("___") = ""|Coincidir con|Coincidir con|Coincidir con|  
|"" = TRIM ("___")|Coincidir con|Coincidir con|Coincidir con|  
  
## <a name="see-also"></a>Consulte también  
 [Comando de ANSI SET](../../odbc/microsoft/set-ansi-command.md)
