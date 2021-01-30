---
description: Crear tabla instrucción limitaciones
title: Limitaciones de la instrucción CREATE TABLE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE TABLE statement limitations [ODBC]
- ODBC SQL grammar, CREATE TABLE statement limitations
ms.assetid: c5067855-20c9-456f-8d63-f375b4297f2e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 94b72930fc33c3abbf3920d1290811d0379413c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183192"
---
# <a name="create-table-statement-limitations"></a>Crear tabla instrucción limitaciones
Cuando se utiliza Microsoft Access, Microsoft Excel o Paradoxdriver, y no se especifica la longitud de una columna de texto o binaria (o se especifica como 0), la longitud de la columna se establecerá en 255.  
  
 Cuando se utiliza el controlador dBASE y no se especifica la longitud de una columna de texto o binaria (o se especifica como 0), la longitud de la columna se establecerá en 254.  
  
 Se admite un máximo de 255 columnas.  
  
 Cuando se usa el controlador de Microsoft Excel en un origen de datos de MicrosoftExcel 5,0, 7,0 o 97, no se puede crear una hoja de cálculo con el mismo nombre que una hoja de cálculo que se quitó anteriormente. Cuando el controlador de Microsoft Excel se usa para tener acceso a una hoja de cálculo de la versión 5,0, 7,0 o 97, una instrucción DROP TABLE borra la hoja de cálculo, pero no elimina el nombre de la hoja de cálculo.  
  
 Cuando se usa el controlador de Paradox, no se pueden agregar columnas una vez que se ha definido un índice en una tabla. Si la primera columna de la lista de argumentos de una instrucción CREATE TABLE crea un índice, no se puede incluir una segunda columna en la lista de argumentos.
