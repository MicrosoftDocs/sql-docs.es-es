---
description: FormattedValue (propiedad, ADO MD)
title: Propiedad FormattedValue (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cell::FormattedValue
- FormattedValue
helpviewer_keywords:
- FormattedValue property [ADO MD]
ms.assetid: 5c06451e-06ec-4da6-9a87-2d043469248a
author: rothja
ms.author: jroth
ms.openlocfilehash: c2b1f8ebc7341a85cbeb887133336653979b5d9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169850"
---
# <a name="formattedvalue-property-ado-md"></a>FormattedValue (propiedad, ADO MD)
Indica la presentación con formato de un valor de [celda](./cell-object-ado-md.md) .  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve una **cadena** y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **FormattedValue** para obtener el valor de presentación con formato de la propiedad [Value](./value-property-ado-md.md) de un objeto [Cell](./cell-object-ado-md.md) . Por ejemplo, si el valor de una celda era 1056,87 y este valor representa una cantidad de dólar, **FormattedValue** sería $1.056,87.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Cell (ADO MD)](./cell-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de Cellset (VB)](./cellset-example-vb.md)   
 [Value (propiedad) (ADO MD)](./value-property-ado-md.md)