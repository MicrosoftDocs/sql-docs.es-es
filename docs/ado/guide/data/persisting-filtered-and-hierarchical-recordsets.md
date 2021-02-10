---
description: Almacenar conjuntos de registros filtrados y jerárquicos
title: Conservar conjuntos de registros jerárquicos y filtrados | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- filtered Recordset persistence [ADO]
- persisting data [ADO]
- data updates [ADO], persisting data
- data persistence [ADO]
- updating data [ADO], persisting data
ms.assetid: d01aeb4d-4e43-450b-b3f2-0c27eaaf9f86
author: rothja
ms.author: jroth
ms.openlocfilehash: 774670bf109dfa28c3b9253daaae0e801167ce1d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032637"
---
# <a name="persisting-filtered-and-hierarchical-recordsets"></a>Almacenar conjuntos de registros filtrados y jerárquicos
Si la propiedad [Filter](../../../ado/reference/ado-api/filter-property.md) está en vigor para el **conjunto de registros**, solo se guardan las filas a las que se puede tener acceso en el filtro. Si el **conjunto de registros** es jerárquico, se guardan el **conjunto de registros** secundario actual y sus elementos secundarios, incluido el **conjunto de registros** primario. Si se llama al método **Save** de un **conjunto de registros** secundario, se guardan el elemento secundario y todos sus elementos secundarios, pero el elemento primario no lo es. Para obtener más información sobre los **conjuntos de registros** jerárquicos, vea [modelado de datos](../../../ado/guide/data/data-shaping.md).  
  
> [!NOTE]
>  Se aplican algunas limitaciones al guardar **conjuntos de registros** jerárquicos (formas de datos) en formato XML. Para obtener más información, vea [guardar registros en formato XML](../../../ado/guide/data/persisting-records-in-xml-format.md).
