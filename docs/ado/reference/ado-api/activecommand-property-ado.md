---
description: Propiedad ActiveCommand (ADO)
title: Propiedad ActiveCommand (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::ActiveCommand
helpviewer_keywords:
- ActiveCommand property [ADO]
ms.assetid: fb4088d5-5968-42d6-aeaa-3955046bb4da
author: rothja
ms.author: jroth
ms.openlocfilehash: 7cb446e14f0ac6887ef81234343c330f5caf4788
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031642"
---
# <a name="activecommand-property-ado"></a>Propiedad ActiveCommand (ADO)
Indica el objeto de [comando](./command-object-ado.md) que creó el objeto de [conjunto de registros](./recordset-object-ado.md) asociado.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un **valor de tipo Variant** que contiene un objeto **Command** . El valor predeterminado es una referencia de objeto null.  
  
## <a name="remarks"></a>Observaciones  
 La propiedad **ActiveCommand** es de solo lectura.  
  
 Si un objeto de **comando** no se utilizó para crear el **conjunto de registros** actual, se devuelve una referencia de objeto **null** .  
  
 Utilice esta propiedad para buscar el objeto de **comando** asociado cuando solo se le proporciona el objeto de **conjunto de registros** resultante.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad ActiveCommand (VB)](./activecommand-property-example-vb.md)   
 [Ejemplo de la propiedad ActiveCommand (JScript)](./activecommand-property-example-jscript.md)   
 [Ejemplo de la propiedad ActiveCommand (VC + +)](./activecommand-property-example-vc.md)   
 [Objeto Command (ADO)](./command-object-ado.md)