---
description: Propiedad Number (ADO)
title: Propiedad Number (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::Number
- Error::GetNumber
- Error::get_Number
helpviewer_keywords:
- number property [ADO]
ms.assetid: f92323c5-dd11-4a63-a505-d9014a0f067f
author: rothja
ms.author: jroth
ms.openlocfilehash: c39192c6158dfdd180c6f3235668157b9750e1e0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041565"
---
# <a name="number-property-ado"></a>Propiedad Number (ADO)
Indica el número que identifica de forma única un objeto de [error](./error-object.md) .  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor **Long** que puede corresponder a una de las constantes [ErrorValueEnum](./errorvalueenum.md) .  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **Number** para determinar el error que se ha producido. El valor de la propiedad es un número único que corresponde a la condición de error.  
  
 La colección de [errores](./errors-collection-ado.md) devuelve un valor HRESULT en formato hexadecimal (por ejemplo, 0x80004005) o valor largo (por ejemplo, 2147467259). Estos valores HRESULT pueden ser generados por componentes subyacentes, como OLE DB o incluso OLE. Para obtener más información sobre estos números, vea [errores (OLE DB)](/previous-versions/windows/desktop/ms724533(v=vs.85)) en la [Referencia del programador de OLE DB](/previous-versions/windows/desktop/ms713643(v=vs.85))*.*  
  
## <a name="applies-to"></a>Se aplica a  
 [Error (objeto)](./error-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Propiedad Description](./description-property.md)   
 [HelpContext, propiedades de HelpFile](./helpcontext-helpfile-properties.md)   
 [Propiedad Source (Error de ADO)](./source-property-ado-error.md)