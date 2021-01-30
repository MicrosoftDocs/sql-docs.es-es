---
description: QUITAR las limitaciones de declaración de tabla
title: Limitaciones de la instrucción DROP TABLE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, DROP TABLE statement limitations
- DROP TABLE statement limitations [ODBC]
ms.assetid: 0a1c80f5-c9f2-4655-9bfd-0131b2f015a9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ac99015ec033aaad32a3e775646fed4fd3cb12ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192610"
---
# <a name="drop-table-statement-limitations"></a>QUITAR las limitaciones de declaración de tabla
Cuando se utiliza el controlador Microsoft Excel 5,0, 7,0 o 97, la instrucción DROP TABLE borra la hoja de cálculo, pero no elimina el nombre de la hoja de cálculo. Dado que el nombre de la hoja de cálculo todavía existe en el libro, no se puede crear otra hoja de cálculo con el mismo nombre.
