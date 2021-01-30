---
description: Asignación de SQLError
title: Asignación de SQLError | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLError
- SQLError function [ODBC], mapping
ms.assetid: 802ac711-7e5d-4152-9698-db0cafcf6047
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 95cb5d4c22ff0baec48e20da2be582d0fbab97c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202878"
---
# <a name="sqlerror-mapping"></a>Asignación de SQLError
Cuando una aplicación llama a **SQLError** a través de un controlador ODBC *3. x* , la llamada a  
  
```  
SQLError(henv, hdbc, hstmt, szSqlState, pfNativeError, szErrorMsg, cbErrorMsgMax, pcbErrorMsg)   
```  
  
 está asignado a  
  
```  
SQLGetDiagRec(HandleType, Handle, RecNumber, szSqlstate, pfNativeErrorPtr, szErrorMsg, cbErrorMsgMax, pcbErrorMsg)  
```  
  
 con el argumento *HandleType* establecido en el valor SQL_HANDLE_ENV, SQL_HANDLE_DBC o SQL_HANDLE_STMT, según corresponda, y el argumento *Handle* establecido en el valor de *HENV*, *hdbc* o *hstmt*, según corresponda. El argumento *RecNumber* viene determinado por el administrador de controladores.
