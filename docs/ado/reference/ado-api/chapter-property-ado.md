---
description: Capítulo (propiedad, ADO)
title: Chapter (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADORecordsetConstruction::Chapter
- ADORecordsetConstruction::put_Chapter
- ADORecordsetConstruction::get_Chapter
helpviewer_keywords:
- Chapter property [ADO]
ms.assetid: 8aa90cb0-f588-4141-9dc9-3b22918394ee
author: rothja
ms.author: jroth
ms.openlocfilehash: e4d156c6e117d5597569918496c161729cf6fc41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164725"
---
# <a name="chapter-property-ado"></a>Capítulo (propiedad, ADO)
Obtiene o establece un objeto **Chapter** de OLE DB desde/en un objeto de [interfaz ADORecordsetConstruction](./adorecordsetconstruction-interface.md) . Cuando se usa **put_Chapter** para establecer el objeto **Chapter** , se convierte un subconjunto de filas en un objeto [Recordset](./recordset-object-ado.md) de ADO. Esto establece el capítulo actual del objeto de **conjunto de filas**. Esta propiedad es de lectura y escritura.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
HRESULT get_Chapter([out, retval] long* plChapter);  
HRESULT put_Chapter([in] long lChapter);  
```  
  
## <a name="parameters"></a>Parámetros  
 *plChapter*  
 Puntero al identificador de un capítulo.  
  
 *LChapter*  
 Identificador de un capítulo.  
  
## <a name="return-values"></a>Valores devueltos  
 Este método de propiedad devuelve los valores HRESULT estándar, incluidos S_OK y E_FAIL.  
  
## <a name="applies-to"></a>Se aplica a  
 [Interfaz ADORecordsetConstruction](./adorecordsetconstruction-interface.md)