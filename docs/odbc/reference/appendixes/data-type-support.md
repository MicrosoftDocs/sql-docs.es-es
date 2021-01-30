---
description: Compatibilidad con tipos de datos
title: Compatibilidad con tipos de datos | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- minimum SQL syntax supported [ODBC]
- ODBC drivers [ODBC], minimum SQL syntax supported
- data types [ODBC], ODBC drivers
- ODBC drivers [ODBC], data types
ms.assetid: 782b4490-372b-4366-aad7-a486fb8a07c8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5b9249539c87e727b293c73ee12ed0fa734e08fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179542"
---
# <a name="data-type-support"></a>Compatibilidad con tipos de datos
Los controladores ODBC deben admitir al menos una de SQL_CHAR y SQL_VARCHAR. La compatibilidad con otros tipos de datos viene determinada por el nivel de cumplimiento de SQL-92 del controlador o del origen de datos. Una aplicación debe llamar a **SQLGetTypeInfo** para determinar los tipos de datos admitidos por el controlador.  
  
 Para obtener más información sobre los tipos de datos, vea [Apéndice D: tipos de datos](../../../odbc/reference/appendixes/appendix-d-data-types.md).
