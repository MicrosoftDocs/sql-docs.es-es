---
description: Emitir comandos al proveedor de datos subyacente
title: Emitir comandos para el proveedor de datos subyacente | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- underlying providers [ADO]
- data shaping [ADO], commands
ms.assetid: d6001863-7733-4c32-817f-081e48587fa1
author: rothja
ms.author: jroth
ms.openlocfilehash: c01178c70a0e6a9e049f9fcfbc0a584fa5fa7ef0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032747"
---
# <a name="issuing-commands-to-the-underlying-data-provider"></a>Emitir comandos al proveedor de datos subyacente
Cualquier comando que no empiece por SHAPE se pasa a través del proveedor de datos. Esto es equivalente a emitir un comando de forma con el formato "SHAPE {Provider Command}". Estos comandos *no* tienen que generar un **conjunto de registros**. Por ejemplo, "SHAPE {DROP TABLE MyTable} es un comando Shape perfectamente válido, suponiendo que el proveedor de datos admite DROP TABLE.  
  
 Esta funcionalidad permite que los comandos de proveedor normales y los comandos de forma compartan la misma conexión y transacción.  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de forma de datos](./data-shaping-example.md)   
 [Gramática de forma formal](./formal-shape-grammar.md)   
 [Comandos Shape en General](./shape-commands-in-general.md)