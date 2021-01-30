---
description: Devuelve SQL_NO_DATA
title: Devolviendo SQL_NO_DATA | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQL_NO_DATA [ODBC]
- backward compatibility [ODBC], SQL_NO_DATA
- compatibility [ODBC], SQL_NO_DATA
ms.assetid: deed0163-9d1a-4e9b-9342-3f82e64477d2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f61d676897bf7b5633a0c9d37c12e44ae9de1cbe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187159"
---
# <a name="returning-sql_no_data"></a>Devuelve SQL_NO_DATA
Cuando una aplicación ODBC *2. x* se trabaja con un controlador ODBC *3. x* llama **a SQLExecDirect**, **SQLExecute** o **SQLParamData**, y se ejecutó una instrucción UPDATE o DELETE buscada pero no afectó a ninguna fila del origen de datos, el controlador ODBC *3. x* debe devolver SQL_SUCCESS. Cuando una aplicación ODBC *3. x* que trabaja con un controlador ODBC *3. x* llama a **SQLExecDirect**, **SQLExecute** o **SQLParamData** con el mismo resultado, el controlador ODBC *3. x* debe devolver SQL_NO_DATA.  
  
 Si una instrucción UPDATE o DELETE buscada en un lote de instrucciones no afecta a ninguna fila del origen de datos, **SQLMoreResults** devuelve SQL_SUCCESS. No puede devolver SQL_NO_DATA, porque eso significa que no hay más resultados, no que hay un resultado de una actualización o eliminación buscada que no afectó a ninguna fila.
