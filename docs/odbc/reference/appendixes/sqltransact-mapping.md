---
description: Asignación de SQLTransact
title: Asignación de SQLTransact | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLTransact
- SQLTransact function [ODBC], mapping
ms.assetid: 8a01041f-3572-46f9-8213-b817f3cf929c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 09dbfe0c97de5b7b2800869dbb02c1290fd3df3a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202568"
---
# <a name="sqltransact-mapping"></a>Asignación de SQLTransact
**SQLTransact** se ha reemplazado por **SQLEndTran**. La principal diferencia entre las dos funciones es que **SQLEndTran** contiene un argumento *HandleType*, que especifica el ámbito del trabajo que se va a realizar. El argumento *HandleType* puede especificar el entorno o el identificador de conexión. La siguiente llamada a **SQLTransact**:  
  
```  
SQLTransact(henv, hdbc, fType)  
```  
  
 está asignado a  
  
```  
SQLEndTran(SQL_HANDLE_DBC, ConnectionHandle, CompletionType);  
```  
  
 Si *ConnectionHandle* no es igual a SQL_NULL_HDBC. El argumento *ConnectionHandle* se establece en el valor de *hdbc*.  
  
 **SQL_Transact** está asignado a  
  
```  
SQLEndTran (SQL_HANDLE_ENV, EnvironmentHandle, CompletionType);  
```  
  
 Si *ConnectionHandle* es igual a SQL_NULL_HDBC. El argumento *EnvironmentHandle* se establece en el valor de *HENV*.  
  
 En los dos casos anteriores, el argumento *CompletionType* se establece en el mismo valor que *fType*.
