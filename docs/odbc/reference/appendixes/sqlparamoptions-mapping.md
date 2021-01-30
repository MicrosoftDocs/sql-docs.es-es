---
description: Asignación de SQLParamOptions
title: Asignación de SQLParamOptions | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLParamOptions
- SQLParamOptions function [ODBC], mapping
ms.assetid: 57ed65f6-9620-4738-b331-19d2a2b5cae4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 16b5678840736bf43f2a6ee817e25fd485bb1c4d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202692"
---
# <a name="sqlparamoptions-mapping"></a>Asignación de SQLParamOptions
Cuando una aplicación llama a **SQLParamOptions** a través de un controlador ODBC *3. x* , la llamada a  
  
```  
SQLParamOptions(hstmt, crow, piRow);  
```  
  
 se asignará a dos llamadas de **SQLSetStmtAttr** como se indica a continuación:  
  
```  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAMSET_SIZE, crow, 0);  
SQLSetStmtAttr(hstmt, SQL_ATTR_PARAMS_PROCESSED_PTR, piRow, 0);  
```
