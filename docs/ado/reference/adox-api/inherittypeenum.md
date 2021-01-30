---
description: InheritTypeEnum
title: InheritTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- InheritTypeEnum
helpviewer_keywords:
- InheritTypeEnum enumeration [ADOX]
ms.assetid: c2f6ce79-c4b3-4d40-ac95-21025208f991
author: rothja
ms.author: jroth
ms.openlocfilehash: d758c78b8778d5a00c0105c8c9dcebfe40da22e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164190"
---
# <a name="inherittypeenum"></a>InheritTypeEnum
Especifica cómo los objetos heredarán los permisos establecidos con [SetPermissions](./setpermissions-method-adox.md).  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adInheritBoth**|3|Los dos objetos y otros contenedores contenidos en el objeto principal heredan la entrada.|  
|**adInheritContainers**|2|Otros contenedores contenidos en el objeto principal heredan la entrada.|  
|**adInheritNone**|0|Predeterminada. No se produce herencia.|  
|**adInheritNoPropagate**|4|Las marcas **adInheritObjects** y **adInheritContainers** no se propagan a una entrada heredada.|  
|**adInheritObjects**|1|Los objetos que no son contenedores del contenedor heredan los permisos.|  
  
## <a name="applies-to"></a>Se aplica a  
 [SetPermissions (método, ADOX)](./setpermissions-method-adox.md)