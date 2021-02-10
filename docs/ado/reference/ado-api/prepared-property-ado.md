---
description: Propiedad Prepared (ADO)
title: Propiedad Prepared (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::Prepared
helpviewer_keywords:
- Prepared property [ADO]
ms.assetid: 11ca8825-765e-4bb4-a6ce-3f6564ad8755
author: rothja
ms.author: jroth
ms.openlocfilehash: e8e595f471de1219f0500f51160422ed91922e3c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041005"
---
# <a name="prepared-property-ado"></a>Propiedad Prepared (ADO)
Indica si se debe guardar una versión compilada de un [comando](./command-object-ado.md) antes de la ejecución.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor **booleano** que, si se establece en **true**, indica que el comando debe estar preparado.  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **Prepared** para que el proveedor guarde una versión preparada (o compilada) de la consulta especificada en la propiedad [CommandText](./commandtext-property-ado.md) antes de la primera ejecución de un objeto [Command](./command-object-ado.md) . Esto puede ralentizar la primera ejecución de un comando, pero una vez que el proveedor compila un comando, el proveedor usará la versión compilada del comando para las ejecuciones posteriores, lo que dará como resultado un rendimiento mejorado.  
  
 Si la propiedad es **false**, el proveedor ejecutará el objeto de **comando** directamente sin crear una versión compilada.  
  
 Si el proveedor no admite la preparación de comandos, puede devolver un error si esta propiedad se establece en **true**. Si el proveedor no devuelve un error, simplemente omite la solicitud para preparar el comando y establece la propiedad **preparado** en **false**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Command (ADO)](./command-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedad Prepared (VB)](./prepared-property-example-vb.md)   
 [Ejemplo de la propiedad Prepared (VC ++)](./prepared-property-example-vc.md)