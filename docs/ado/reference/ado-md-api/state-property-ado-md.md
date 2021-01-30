---
description: State (propiedad) (ADO MD)
title: Propiedad State (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- State
- Cellset::State
helpviewer_keywords:
- State property [ADO MD]
ms.assetid: 06d480ca-9eb6-4570-a45d-a73539bddd32
author: rothja
ms.author: jroth
ms.openlocfilehash: 61d34ef2afa96babbb194d4f52a560239f033154
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164412"
---
# <a name="state-property-ado-md"></a>State (propiedad) (ADO MD)
Indica el estado actual del Cellset.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un entero **largo** que indica la condición actual del objeto [Cellset](./cellset-object-ado-md.md) y es de solo lectura. Los valores siguientes son válidos: **adStateClosed** (0) y **adStateOpen** (1).  
  
## <a name="remarks"></a>Observaciones  
 Para usar los nombres de constantes [ObjectStateEnum](../ado-api/objectstateenum.md) , debe tener la biblioteca de tipos ADO a la que se hace referencia en el proyecto. Vea [usar ado con ADO MD](../../guide/multidimensional/using-ado-with-ado-md.md) para obtener más información.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de celdas (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Método Close (ADO MD)](./close-method-ado-md.md)   
 [Open (método) (ADO MD)](./open-method-ado-md.md)