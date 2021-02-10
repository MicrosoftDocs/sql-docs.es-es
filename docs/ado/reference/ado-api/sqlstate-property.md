---
description: Propiedad SQLState
title: Propiedad SQLState | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Error::GetSQLState
- Error::SQLState
- Error::get_SQLState
helpviewer_keywords:
- SQLState property
ms.assetid: f9e25967-54b0-444d-af2a-0d2c75365d3e
author: rothja
ms.author: jroth
ms.openlocfilehash: 0f6d10db3da5ea8c1b6d5daba3062e53d2f35244
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051346"
---
# <a name="sqlstate-property"></a>Propiedad SQLState
Indica el estado de SQL para un objeto de [error](./error-object.md) determinado.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de **cadena** de cinco caracteres que sigue al estándar ANSI SQL e indica el código de error.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **SQLSTATE** para leer el código de error de cinco caracteres que devuelve el proveedor cuando se produce un error durante el procesamiento de una instrucción SQL. Por ejemplo, al usar el proveedor de OLE DB de Microsoft para ODBC con una base de datos de Microsoft SQL Server, los códigos de error de estado de SQL se originan a partir de ODBC, en función de los errores específicos de ODBC o de los errores que se originan en Microsoft SQL Server y, a continuación, se asignan a los errores de ODBC. Estos códigos de error están documentados en el estándar ANSI SQL, pero pueden implementarse de forma diferente en distintos orígenes de datos.  
  
## <a name="applies-to"></a>Se aplica a  
 [Error (objeto)](./error-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VB)](./description-helpcontext-helpfile-nativeerror-number-source-example-vb.md)   
 [Ejemplo de las propiedades Description, HelpContext, HelpFile, NativeError, Number, Source y SQLState (VC + +)](./description-helpcontext-helpfile-nativeerror-number-source-example-vc.md)