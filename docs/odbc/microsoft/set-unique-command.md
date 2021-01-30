---
description: CONJUNTO único comando
title: ESTABLECER comando único | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET UNIQUE command [ODBC]
ms.assetid: 1f69e31e-4599-47cc-ac89-b86fba8703c5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f30e589349ab1f0388e43af0aec132465b1fcdce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208592"
---
# <a name="set-unique-command"></a>CONJUNTO único comando
Especifica si los registros con valores de clave de índice duplicados se mantienen en un archivo de índice.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET UNIQUE ON | OFF  
```  
  
## <a name="arguments"></a>Argumentos  
 ACTIVAR  
 Especifica que cualquier registro con un valor de clave de índice duplicado no se incluya en el archivo de índice. En el archivo de índice solo se incluye el primer registro con el valor de clave de índice original.  
  
 Apagado  
 (Valor predeterminado). Especifica que los registros con valores de clave de índice duplicados se incluyen en el archivo de índice.  
  
## <a name="remarks"></a>Observaciones  
 Un archivo de índice conserva su configuración exclusiva al emitir REINDEX. Para obtener más información, vea [index](../../odbc/microsoft/index-command.md).
