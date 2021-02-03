---
description: MSSQLSERVER_9532
title: MSSQLSERVER_9532 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9532 (Database Engine error)
ms.assetid: ab95cce8-4f97-4aea-a746-a73eea7c9aab
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1bd1948e2475b792198465ed8befc838ab00db48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187573"
---
# <a name="mssqlserver_9532"></a>MSSQLSERVER_9532
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|9532|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|XMLERR_COLUMNSET_CANNOT_CONVERT_FROM_TO|  
|Texto del mensaje|En la operación consulta/DML que implica al conjunto de columnas "%.*ls", no se pudo convertir el tipo de datos "%ls" al tipo "%ls" para la columna "%.\*ls".|  
  
## <a name="explanation"></a>Explicación  
Un conjunto de columnas es una representación XML sin tipo que combina algunas columnas de una tabla en una salida estructurada. Cuando se insertan o actualizan valores de columnas dispersas mediante el conjunto de columnas XML, los valores que se insertan en las columnas dispersas subyacentes se convierten de manera implícita del tipo de datos **xml**. Se proporcionó un valor que no se puede convertir al tipo de datos de la columna.  
  
## <a name="user-action"></a>Acción del usuario  
Dado que el valor proporcionado no se pudo convertir de manera implícita, podría ser una entrada no válida. Corrija el error e inténtelo de nuevo. Si el valor es correcto, modifique la instrucción para utilizar las columnas individuales en lugar del conjunto de columnas. Esto permitirá convertir de manera explícita el valor al tipo de datos correcto.  
  
