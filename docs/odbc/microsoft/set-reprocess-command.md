---
description: CONJUNTO de comandos de volver a procesar
title: ESTABLECER comando volver a procesar | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET REPROCESS command [ODBC]
ms.assetid: b0708757-b1d7-42f3-8988-787f2a806b8b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b45188db00eda553bb3443d791d889a4d8c31a77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208602"
---
# <a name="set-reprocess-command"></a>CONJUNTO de comandos de volver a procesar
Especifica cuántas veces o cuánto tiempo se bloquea un archivo o registro después de un intento de bloqueo incorrecto.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET REPROCESS TO nAttempts [SECONDS] | TO AUTOMATIC  
```  
  
## <a name="arguments"></a>Argumentos  
 A *nAttempts*[segundos]  
 Especifica el número de veces o el número de segundos que se intentará bloquear un registro o archivo después de un intento incorrecto inicial. El valor predeterminado es 0; el valor máximo es 32.000.  
  
 SEGUNDOS especifica que Visual FoxPro intenta bloquear un archivo o registro durante *nAttempts* segundos. Solo está disponible cuando *nAttempts* es mayor que cero.  
  
 Por ejemplo, si *nAttempts* es 30, Visual FoxPro intentará bloquear un registro o archivo hasta 30 veces. Si también incluye segundos (establecer volver a procesar en 30 segundos), Visual FoxPro intenta continuamente bloquear un registro o archivo durante 30 segundos.  
  
 Si una rutina ON ERROR está en vigor y si los intentos de un comando para bloquear el registro o el archivo no son correctos, se ejecuta la rutina ON ERROR. Sin embargo, si una función intenta el bloqueo, no se ejecuta una rutina ON ERROR y la función devuelve false (. F.).  
  
 Si no está en vigor una rutina ON ERROR, un comando intenta bloquear el registro o el archivo y no se puede colocar el bloqueo, se genera un error. Si una función intenta colocar el bloqueo, la alerta no se muestra y la función devuelve false (. F).  
  
 Si *nAttempts* es 0 (el valor predeterminado) y se emite un comando o una función que intenta bloquear un registro o un archivo, Visual FoxPro intenta bloquear el registro o el archivo de forma indefinida. Si el registro o el archivo están disponibles para bloquear mientras espera, se coloca el bloqueo y se borra el mensaje del sistema. Si una función ha intentado colocar el bloqueo, la función devuelve true (. T.).  
  
 Si una rutina ON ERROR está en vigor y un comando está intentando bloquear el registro o el archivo, la rutina ON ERROR tiene prioridad sobre los intentos adicionales de bloquear el registro o el archivo. La rutina ON ERROR se ejecuta inmediatamente. Visual FoxPro no intenta realizar bloqueos de registro o archivos adicionales y no muestra el mensaje del sistema.  
  
 Si *nAttempts* es 1, Visual FoxPro intenta bloquear el registro o el archivo de forma indefinida y no se ejecuta una rutina on error.  
  
 Si otro usuario ha colocado un bloqueo en el registro o archivo que intenta bloquear, debe esperar hasta que el usuario libere el bloqueo.  
  
 A AUTOMÁTICO  
 Especifica que Visual FoxPro intenta bloquear el registro o el archivo de forma indefinida. (Establezca volver a procesar en-2 es un comando equivalente).  
  
## <a name="remarks"></a>Observaciones  
 El primer intento de bloquear un registro o archivo no siempre es correcto. Con frecuencia, un registro o un archivo está bloqueado por otro usuario de la red. SET Reprocess determina si Visual FoxPro realiza más intentos de bloquear el registro o el archivo cuando el intento inicial no se realiza correctamente. Puede especificar el número de veces que se realizan intentos adicionales o el tiempo durante el que se realizan los intentos. Una rutina ON ERROR afecta al modo en que se administran los intentos de bloqueo incorrectos.
