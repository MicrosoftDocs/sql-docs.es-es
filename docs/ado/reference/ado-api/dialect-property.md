---
description: Propiedad Dialect
title: Propiedad de dialecto | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Command::Dialect
helpviewer_keywords:
- Dialect property
ms.assetid: 329c3a71-ba88-4009-b04f-2f52195a5957
author: rothja
ms.author: jroth
ms.openlocfilehash: ed95df8ca18a60cf34108258e7b3f624cb0cb1fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171252"
---
# <a name="dialect-property"></a>Propiedad Dialect
Indica el dialecto de las propiedades [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) o [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) . El dialecto define la sintaxis y las reglas generales que el proveedor utiliza para analizar la cadena o el flujo.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 La propiedad **Dialect** contiene un GUID válido que representa el dialecto del texto del comando o de la secuencia. El valor predeterminado de esta propiedad es {C8B521FB-5CF3-11CE-ADE5-00AA0044773D}, que indica que el proveedor debe elegir cómo interpretar el texto o la secuencia del comando.  
  
## <a name="remarks"></a>Observaciones  
 ADO no consulta el proveedor cuando el usuario lee el valor de esta propiedad; Devuelve una representación de cadena del valor almacenado actualmente en el objeto de [comando](../../../ado/reference/ado-api/command-object-ado.md) .  
  
 Cuando el usuario establece la propiedad de **dialecto** , ADO valida el GUID y genera un error si el valor proporcionado no es un GUID válido. Consulte la documentación del proveedor para determinar los valores de GUID admitidos por la propiedad de **dialecto** .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Command (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Método Execute (Command ADO)](../../../ado/reference/ado-api/execute-method-ado-command.md)
