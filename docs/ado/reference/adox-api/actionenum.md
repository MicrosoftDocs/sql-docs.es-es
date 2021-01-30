---
description: ActionEnum
title: ActionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ActionEnum
helpviewer_keywords:
- ActionEnum enumeration [ADOX]
ms.assetid: f948febd-c885-4621-823b-421e116fec4e
author: rothja
ms.author: jroth
ms.openlocfilehash: 35f4ddd60d4056cebbf0383aa0afb240c3ecb722
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169671"
---
# <a name="actionenum"></a>ActionEnum
Especifica el tipo de acción que se debe realizar cuando se llama a [SetPermissions](./setpermissions-method-adox.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adAccessDeny**|3|Al grupo o al usuario se le denegarán los permisos especificados.|  
|**adAccessGrant**|1|El grupo o usuario tendrá al menos los permisos solicitados.|  
|**adAccessRevoke**|4|Se revocarán todos los derechos de acceso explícitos del grupo o usuario.|  
|**adAccessSet**|2|El grupo o usuario tendrá exactamente los permisos solicitados.|