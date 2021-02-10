---
description: ADCPROP_AUTORECALC_ENUM
title: ADCPROP_AUTORECALC_ENUM | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ADCPROP_AUTORECALC_ENUM
helpviewer_keywords:
- ADCPROP_AUTORECALC_ENUM [ADO]
ms.assetid: ded4f087-87b9-4efa-8026-bde53d3e9e8a
author: rothja
ms.author: jroth
ms.openlocfilehash: 9c29d26af25a09c659fe141584b2e0200a674142
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035855"
---
# <a name="adcprop_autorecalc_enum"></a>ADCPROP_AUTORECALC_ENUM
Especifica cuándo el proveedor [MSDataShape](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) vuelve a calcular las columnas agregadas y calculadas en un conjunto de registros jerárquico.  
  
 Estas constantes solo se usan con el proveedor **MSDataShape** y la propiedad dinámica de **conjunto de registros** "**auto Calc**", a la que se hace referencia en el [Índice de propiedades dinámicas de ADO](./ado-dynamic-property-index.md) y se documentan en el servicio [de cursor de Microsoft para OLE DB](../../guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md) o en el [servicio de forma de datos de Microsoft para OLE DB](../../guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) documentación.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adRecalcAlways**|1|Predeterminada. Vuelve a calcular cada vez que el proveedor **MSDataShape** determina los valores de los que dependen las columnas calculadas.|  
|**adRecalcUpFront**|0|Calcula solo cuando se compila inicialmente el conjunto de **registros** jerárquico.|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Estas constantes no tienen equivalentes de ADO/WFC.