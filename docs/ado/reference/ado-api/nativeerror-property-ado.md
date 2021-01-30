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
ms.openlocfilehash: b25d6c3ffd9563032df8743c60a5db1cd3f78b4e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167054"
---
# <a name="nativeerror-property-ado"></a>Propiedad NativeError (ADO)
Indica el código de error específico del proveedor para un objeto de [error](./error-object.md) determinado.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de **tipo Long** que indica el código de error.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **NativeError** para recuperar la información de error específica de la base de datos para un objeto de **error** determinado. Por ejemplo, al usar el proveedor ODBC de Microsoft para OLE DB con una base de datos de Microsoft SQL Server, los códigos de error nativos que se originan a partir de SQL Server pasan a través de ODBC y el proveedor ODBC a la propiedad **NativeError** de ADO.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de error](./error-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)