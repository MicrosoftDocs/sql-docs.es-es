---
description: Comando NULL del conjunto
title: ESTABLECER comando NULL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set nullSET NULL
ms.assetid: 410c5a6e-e957-4ecc-9e2d-e591cbc0bc4f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a190c913b2f8cc9d7a111a0c95d0408de2c1c6c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208608"
---
# <a name="set-null-command"></a>Comando NULL del conjunto
Determina cómo se admiten los valores NULL en los comandos ALTER TABLE-SQL, CREATE TABLE-SQL e INSERT-SQL.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET NULL ON | OFF  
```  
  
## <a name="arguments"></a>Argumentos  
 ACTIVAR  
 (Valor predeterminado para el controlador; el valor predeterminado para Visual FoxPro es OFF). Especifica que todas las columnas de una tabla creada con ALTER TABLE y CREATE TABLE permitirán valores NULL. Puede invalidar la compatibilidad con valores NULL para las columnas de la tabla incluyendo la cláusula NOT NULL en las definiciones de las columnas.  
  
 También especifica que INSERT-SQL insertará valores NULL en las columnas que no se incluyan en la cláusula INSERT-SQL VALUE. INSERT-SQL insertará valores NULL solo en columnas que permitan valores NULL.  
  
 Apagado  
 Especifica que todas las columnas de una tabla creada con ALTER TABLE y CREATE TABLE no permitirán valores NULL. Puede designar la compatibilidad con valores NULL para las columnas de ALTER TABLE y CREATE TABLE incluyendo la cláusula NULL en las definiciones de las columnas.  
  
 También especifica que INSERT-SQL insertará valores en blanco en las columnas que no se incluyan en la cláusula INSERT-SQL VALUE.  
  
## <a name="remarks"></a>Observaciones  
 SET NULL solo afecta al modo en que se admiten los valores NULL en ALTER TABLE, CREATE TABLE e INSERT-SQL. Los demás comandos no se ven afectados por SET NULL.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER TABLE-comando SQL](../../odbc/microsoft/alter-table-sql-command.md)   
 [Comando CREATE TABLE-SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [Insertar: comando SQL](../../odbc/microsoft/insert-sql-command.md)
