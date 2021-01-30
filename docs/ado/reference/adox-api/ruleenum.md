---
description: RuleEnum
title: RuleEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RuleEnum
helpviewer_keywords:
- RuleEnum enumeration [ADOX]
ms.assetid: 738fd3ff-3daf-483d-a0b9-88bef1be54c1
author: rothja
ms.author: jroth
ms.openlocfilehash: 978aa969105b3a773f353986ecd39a38995002df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164043"
---
# <a name="ruleenum"></a>RuleEnum
Especifica la regla que se debe seguir cuando se elimina una [clave](./key-object-adox.md) .  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adRICascade**|1|Cambios en cascada.|  
|**adRINone**|0|Predeterminada. No se realiza ninguna acción.|  
|**adRISetDefault**|3|El valor de clave externa se establece en el valor predeterminado.|  
|**adRISetNull**|2|El valor de clave externa se establece en NULL.|  
  
## <a name="applies-to"></a>Se aplica a  
 [DeleteRule (propiedad, ADOX)](./deleterule-property-adox.md)