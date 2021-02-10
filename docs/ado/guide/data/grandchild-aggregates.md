---
description: Agregados secundarios
title: Agregados terciarios | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- grandchild aggregates [ADO]
- data shaping [ADO], grandchild aggregates
ms.assetid: 4162d35f-2ce1-4218-80a5-b6933348837e
author: rothja
ms.author: jroth
ms.openlocfilehash: a5cae4f996112d660564cb8efd3ea9b7e53c4f9f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033155"
---
# <a name="grandchild-aggregates"></a>Agregados secundarios
A la columna de capítulo creada en una cláusula de un comando de forma se le puede asignar un *nombre de alias de capítulo* (normalmente con la palabra clave as). Puede identificar cualquier columna de cualquier capítulo del **conjunto de registros** con forma con un nombre completo que identifique el elemento secundario que contiene la columna. Por ejemplo, si el capítulo principal, chap1, contiene un capítulo secundario, chap2, que tiene una columna amount, AMT, el nombre completo sería chap1. chap2. AMT. El nombre completo se puede usar como argumento para una de las funciones de agregado (SUM, AVG, MAX, MIN, COUNT, STDEV o ANY).  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la forma de datos](./data-shaping-example.md)