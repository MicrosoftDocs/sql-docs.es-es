---
description: Información de Error relacionado con el conjunto de registros
title: Recordset-Related información del error | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset-related errors [ADO]
- errors [ADO], Recordset-related
ms.assetid: 7e103574-59ad-4790-b5f9-fa8d715e711e
author: rothja
ms.author: jroth
ms.openlocfilehash: 2fcd596455cc1074d46ea6ee45d6c1f037553741
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037105"
---
# <a name="recordset-related-error-information"></a>Información de Error relacionado con el conjunto de registros
Durante el procesamiento por lotes, la propiedad **status** del objeto de **conjunto de registros** proporciona información sobre los registros individuales del **conjunto de registros**. Antes de que tenga lugar una actualización por lotes, la propiedad **status** del **conjunto de registros** refleja información acerca de los registros que se van a agregar, modificar y eliminar. Después de llamar a **UpdateBatch** , la propiedad **status** indica si la operación se ha realizado correctamente o no. A medida que se mueve de un registro a otro en el **conjunto de registros**, el valor de la propiedad **status** cambia para describir el estado del registro actual.
