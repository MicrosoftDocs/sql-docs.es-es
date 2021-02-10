---
description: Errores (ADO)
title: Errores (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 8ae6611b-3069-4155-b014-c0c9da37be39
author: rothja
ms.author: jroth
ms.openlocfilehash: b26899566d428a1b04c918c2552972448d996597
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037515"
---
# <a name="errors-ado"></a>Errores (ADO)
Cualquier operación relacionada con objetos ADO puede generar uno o más errores de proveedor. Cuando se produce cada error, se colocan uno o varios objetos de **error** en la colección de **errores** del objeto de **conexión** . Para obtener más información sobre cómo controlar las advertencias y los errores de la aplicación ADO, vea [control de errores](./error-handling.md).  
  
 Los errores de aplicación se pueden generar mediante un mecanismo independiente. Por ejemplo, en Visual Basic, el objeto **Err** contendrá errores en el nivel de la aplicación.