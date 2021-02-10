---
description: Propiedad NativeError (ADO)
title: Propiedad NativeError (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::GetNativeError
- Error::get_NativeError
- Error::NativeError
helpviewer_keywords:
- NativeError property [ADO]
ms.assetid: b9b47e57-18a4-4186-aef5-5bd18d7b1d74
author: rothja
ms.author: jroth
ms.openlocfilehash: 10ed79bf5629ce6439362ce1e80226444928a16b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041595"
---
# <a name="nativeerror-property-ado"></a>Propiedad NativeError (ADO)
Indica el código de error específico del proveedor para un objeto de [error](./error-object.md) determinado.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de **tipo Long** que indica el código de error.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **NativeError** para recuperar la información de error específica de la base de datos para un objeto de **error** determinado. Por ejemplo, al usar el proveedor ODBC de Microsoft para OLE DB con una base de datos de Microsoft SQL Server, los códigos de error nativos que se originan a partir de SQL Server pasan a través de ODBC y el proveedor ODBC a la propiedad **NativeError** de ADO.  
  
## <a name="applies-to"></a>Se aplica a  
 [Error (objeto)](./error-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)