---
description: Comando de eliminaciones de Set
title: ESTABLECER comando eliminado | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET DELETED command [ODBC]
ms.assetid: 6b5e0086-156d-471d-8e7f-6c5fa9686cd5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6447e2d0c77e1a161df58ca070fae8c624c196a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208633"
---
# <a name="set-deleted-command"></a>Comando de eliminaciones de Set
Especifica si se procesan los registros marcados para su eliminación y si están disponibles para su uso en otros comandos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
SET DELETED ON | OFF  
```  
  
## <a name="arguments"></a>Argumentos  
 ACTIVAR  
 (Valor predeterminado para el controlador; el valor predeterminado para Visual FoxPro es OFF). Especifica que los comandos que operan en registros (incluidos los registros de las tablas relacionadas) mediante un ámbito omiten los registros marcados para su eliminación.  
  
 Apagado  
 Especifica que los comandos que operan en registros (incluidos los registros de las tablas relacionadas) pueden tener acceso a los registros marcados para su eliminación mediante un ámbito.  
  
## <a name="remarks"></a>Observaciones  
 Las consultas que usan DELETEd () para probar el estado de los registros se pueden optimizar mediante la tecnología Rushmore de Visual FoxPro si la tabla está indizada en DELETEd ().  
  
> [!IMPORTANT]  
>  SET DELETEd se omite si el ámbito predeterminado del comando es el registro actual o si se incluye un ámbito de un único registro. INDEX siempre omite SET DELETEd y indexa todos los registros de la tabla.  
  
## <a name="see-also"></a>Consulte también  
 [ELIMINAR, comando SQL](../../odbc/microsoft/delete-sql-command.md)
