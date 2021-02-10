---
description: Comandos de proceso de comandos con parámetros con intermedias
title: Comandos con parámetros con comandos de proceso intermedios | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
- APPEND clause [ADO]
- COMPUTE command [ADO]
ms.assetid: 732f624f-8900-4608-9815-194302d22e8b
author: rothja
ms.author: jroth
ms.openlocfilehash: ce78d5edf2ded2744c3633368d571d72912c1322
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037225"
---
# <a name="parameterized-commands-with-intervening-compute-commands"></a>Comandos de proceso de comandos con parámetros con intermedias
Un comando APPEND de forma parametrizada típico tiene una cláusula que crea un **conjunto de registros** primario con un comando de consulta y otra cláusula que crea un **conjunto de registros** secundario con un comando de consulta con parámetros, es decir, un comando que contiene un marcador de posición de parámetro (un signo de interrogación "?"). El conjunto de **registros** con forma resultante tiene dos niveles, en los que el elemento primario ocupa el nivel superior y el secundario ocupa el nivel inferior.  
  
 La cláusula que crea el **conjunto de registros** secundario puede ser ahora un número arbitrario de comandos de proceso de forma anidada, donde el comando más profundamente anidado contiene la consulta con parámetros. El conjunto de **registros** con forma resultante tiene varios niveles, en los que el elemento primario ocupa el nivel superior, el secundario ocupa el nivel inferior y un número arbitrario de **conjuntos de registros** generados por los comandos COMPUTE de forma ocupan los niveles intermedios.  
  
 El uso típico de esta característica es invocar la función de agregado y agrupar las capacidades de los comandos shapeCOMPUTE para crear objetos de **conjunto de registros** intermedios con información analítica sobre el **conjunto de registros** secundario. Además, dado que se trata de un comando Shape con parámetros, cada vez que se tiene acceso a una columna Chapter del elemento primario, se puede recuperar un nuevo **conjunto de registros** secundario. Dado que los niveles intermedios se derivan del elemento secundario, también se volverán a calcular.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la forma de datos](../../../ado/guide/data/data-shaping-example.md)
