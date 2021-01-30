---
description: Asignación de SQLBindParam
title: Asignación de SQLBindParam | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLBindparam function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLBindParam
ms.assetid: 375f8f24-36de-4946-916e-c75abc6f070d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62951dc5ae69e86eb5cbc2fba21407d63952e74b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202938"
---
# <a name="sqlbindparam-mapping"></a>Asignación de SQLBindParam
**SQLBindParam** no se puede llamar realmente en desuso porque nunca estaba en ODBC. sin embargo, sigue representando la funcionalidad duplicada: el administrador de controladores debe exportarla porque ISO y las aplicaciones compatibles con el grupo abierto la usarán. Dado que **SQLBindParameter** contiene toda la funcionalidad **de SQLBindParam**, **SQLBindParam** se asignará en **SQLBindParameter** (cuando el controlador subyacente sea un controlador ODBC *3. x* ). No es necesario que un controlador ODBC *3. x* implemente **SQLBindParam**.  
  
## <a name="remarks"></a>Observaciones  
 Cuando se realiza la siguiente llamada a **SQLBindParam** :  
  
```  
SQLBindParam(   StatementHandle,    ParameterNumber,    ValueType,    ParameterType,    ColumnSize,    DecimalDigits,    ParameterValuePtr,    StrLen_or_IndPtr)  
```  
  
 el administrador de controladores llama a **SQLBindParameter** en el controlador de la siguiente manera:  
  
```  
SQLBindParameter(   StatementHandle,    ParameterNumber,    SQL_PARAM_INPUT,    ValueType,    ParameterType,    ColumnSize,    DecimalDigits,    ParameterValuePtr,    BufferLength,    StrLen_or_IndPtr)  
```  
  
 Consulte la [información de ODBC 64](../../../odbc/reference/odbc-64-bit-information.md)bits, si la aplicación se ejecutará en un sistema operativo de 64 bits.  
  
## <a name="see-also"></a>Consulte también  
 [Asignación de funciones en desuso](../../../odbc/reference/appendixes/mapping-deprecated-functions.md)
