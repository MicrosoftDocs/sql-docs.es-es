---
description: Limitaciones de declaración de tabla ALTER
title: Limitaciones de la instrucción ALTER TABLE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, ALTER TABLE statement limitations
- ALTER TABLE statement limitations [ODBC]
ms.assetid: f3e88f85-edf4-47cd-a822-292b106ddb34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d36e5dfb2457a9d3453dbe3bc877c5d267731ea1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205747"
---
# <a name="alter-table-statement-limitations"></a>Limitaciones de declaración de tabla ALTER
Cuando se usa el controlador dBASE o Paradox, una vez que se ha creado un índice y se ha agregado un nuevo registro, la instrucción ALTER TABLE no puede cambiar la estructura de la tabla a menos que se quite el índice y se elimine el contenido de la tabla.  
  
 No se admiten las instrucciones ALTER TABLE para los controladores de texto o de Microsoft Excel.  
  
> [!NOTE]  
>  Cuando se usa el controlador de Paradox sin implementar el Motor de base de datos de Borland, no se admiten las instrucciones ALTER TABLE. solo se permiten las instrucciones Read y Append.
