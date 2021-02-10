---
description: Almacenar conjuntos de registros jerárquicos
title: Conservar conjuntos de registros jerárquicos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hierarchical Recordsets [ADO], persisting
- persisting hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: 43798bb5-98a6-4ad6-9bf8-78154b3a1827
author: rothja
ms.author: jroth
ms.openlocfilehash: ddcda766d729e085ed654757d0125f18ec7a696a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037195"
---
# <a name="persisting-hierarchical-recordsets"></a>Almacenar conjuntos de registros jerárquicos
Puede guardar un **conjunto de registros** jerárquico en un archivo en formato ADTG o XML llamando al método [Save](../../../ado/reference/ado-api/save-method.md) . Sin embargo, se aplican dos limitaciones al guardar los **conjuntos de registros** jerárquicos en formato XML: no se puede guardar en XML si el **conjunto de registros** jerárquico contiene actualizaciones pendientes y no se puede guardar un conjunto de **registros** jerárquico con parámetros.  
  
 Para obtener más información sobre el proveedor de forma de datos, vea servicio de forma [de datos de Microsoft para OLE DB](../../../ado/guide/appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (ADO) e [información general sobre el servicio de forma de datos para OLE DB](/previous-versions/windows/desktop/ms719615(v=vs.85)).  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de forma de datos](../../../ado/guide/data/data-shaping-example.md)   
 [Gramática de forma formal](../../../ado/guide/data/formal-shape-grammar.md)   
 [Comandos Shape en General](../../../ado/guide/data/shape-commands-in-general.md)