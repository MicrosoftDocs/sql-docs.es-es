---
description: CREAR las limitaciones de la instrucción de índice
title: Limitaciones de la instrucción CREATE INDEX | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- CREATE INDEX statement limitations [ODBC]
- ODBC SQL grammar, CREATE INDEX statement limitations
ms.assetid: 832dcda1-e452-48e6-8adb-7fb33c4fb4ff
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60952e7b9236c392e10bc155a19c5b147e011dbf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206163"
---
# <a name="create-index-statement-limitations"></a>CREAR las limitaciones de la instrucción de índice
La instrucción CREATE INDEX no es compatible con los controladores de texto o de Microsoft Excel.  
  
 Un índice se puede definir en un máximo de 10 columnas. Si se incluyen más de 10 columnas en una instrucción CREATE INDEX, no se reconocerá el índice y la tabla se tratará como si no se hubiera creado ningún índice.  
  
 El controlador dBASE no puede crear un índice en una columna lógica.  
  
 Cuando se usa el controlador dBASE, se puede mejorar el tiempo de respuesta de archivos grandes creando un índice. MDX (o. NDX) en la columna (campo) especificada en las cláusulas WHERE de una instrucción SELECT. Los índices. MDX existentes se aplicarán automáticamente para =, >, \<, > =, =< y entre los operadores en una cláusula WHERE, y como los predicados, así como en los predicados de combinación.  
  
 Cuando se utiliza el controlador dBASE, el índice creado por una instrucción CREATE UNIQUE INDEX es realmente no único y los valores duplicados se pueden insertar en la columna indizada. Solo se puede Agregar al índice un registro de un conjunto con valores de clave idénticos.  
  
 Cuando se usa el controlador de Paradox, se debe definir un índice único en un subconjunto contiguo de las columnas de una tabla, incluida la primera columna. El controlador de Paradox no puede actualizar una tabla si no se ha definido un índice único en la tabla o si se usa el controlador de Paradox sin la implementación de Borland Motor de base de datos.
