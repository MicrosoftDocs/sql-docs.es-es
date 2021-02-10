---
description: HelpContext, HelpFile propiedades
title: Propiedades HelpContext y HelpFile | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::GetHelpContext
- Error::GetHelpFile
- Error::get_HelpFile
- Error::get_HelpContext
- Error::HelpContext
- Error::HelpFile
helpviewer_keywords:
- HelpContext property [ADO]
- HelpFile property [ADO]
ms.assetid: 2b9ef441-993c-44d4-8f87-fac0979dac1d
author: rothja
ms.author: jroth
ms.openlocfilehash: ee683a07b0a581788657af2233944dccf5ab05b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100020792"
---
# <a name="helpcontext-helpfile-properties"></a>HelpContext, HelpFile propiedades
Indica el archivo de ayuda y el tema asociados a un objeto de [error](./error-object.md) .  
  
## <a name="return-values"></a>Valores devueltos  
  
-   **HelpContextID** Devuelve un identificador de contexto, como un valor **largo** , para un tema en un archivo de ayuda.  
  
-   **ArchivoDeAyuda** Devuelve un valor de **cadena** que se evalúa como una ruta de acceso totalmente resuelta a un archivo de ayuda.  
  
## <a name="remarks"></a>Observaciones  
 Si se especifica un archivo de ayuda en la propiedad **HelpFile** , la propiedad **HelpContext** se usa para mostrar automáticamente el tema de ayuda que identifica. Si no hay ningún tema de ayuda relevante disponible, la propiedad **HelpContext** devuelve cero y la propiedad **HelpFile** devuelve una cadena de longitud cero ("").  
  
## <a name="applies-to"></a>Se aplica a  
 [Error (objeto)](./error-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)   
 [Propiedad Description](./description-property.md)   
 [Propiedad Number (ADO)](./number-property-ado.md)   
 [Propiedad Source (Error de ADO)](./source-property-ado-error.md)