---
description: Propiedad de las filas (ADO)
title: Row (propiedad) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordConstruction::PutRow
- ADORecordConstruction::GetRow
- ADORecordConstruction::get_Row
- ADORecordConstruction::Row
- ADORecordConstruction::put_Row
helpviewer_keywords:
- Row property [ADO]
ms.assetid: 21019d89-2dd1-4a26-ac6f-384b81d66949
author: rothja
ms.author: jroth
ms.openlocfilehash: cf90fe1219103713797dfd04b1475d7fc77c80a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170352"
---
# <a name="row-property-ado"></a>Propiedad de las filas (ADO)
Obtiene o establece un objeto de **fila** de OLE DB desde o en un objeto de [interfaz ADORecordConstruction](./adorecordconstruction-interface.md) . Cuando se usa **put_Row** para establecer un objeto **Row** , una fila se convierte en un objeto **Record** de ADO.  
  
## <a name="readwritesyntax"></a>Lectura/escritura. Sintáctica  
  
```  
HRESULT get_Row([out, retval] IUnknown** ppRow);  
HRESULT put_Row([in] IUnknown* pRow);  
```  
  
## <a name="parameters"></a>Parámetros  
 *ppRow*  
 Puntero a un objeto de **fila** OLE DB.  
  
 *PRow*  
 Objeto de **fila** de OLE DB.  
  
## <a name="return-values"></a>Valores devueltos  
 Este método de propiedad devuelve los valores HRESULT estándar, incluidos S_OK y E_FAIL.  
  
## <a name="applies-to"></a>Se aplica a  
 [Interfaz ADORecordConstruction](./adorecordconstruction-interface.md)