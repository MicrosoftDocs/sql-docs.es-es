---
title: DropOnlyMode (DTA, elemento)
description: En la utilidad DTA, el elemento DropOnlyMode especifica que el Asistente para la optimización de motor de base de datos solo debe quitar índices, vistas indizadas o particiones ya existentes durante la sesión de optimización.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- DropOnlyMode element
ms.assetid: 80960676-7581-4074-889b-80ee665963dd
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 27d38719f0ee9572a86a7b196655489b33824071
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351107"
---
# <a name="droponlymode-element-dta"></a>DropOnlyMode (DTA, elemento)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Especifica que el Asistente para la optimización de motor de base de datos solo debe quitar índices, vistas indizadas o particiones ya existentes durante la sesión de optimización. Cuando se especifica esta opción de optimización, no se consideran nuevas estructuras de diseño físico.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <DropOnlyMode>...</DropOnlyMode>  
```  
  
## <a name="element-characteristics"></a>Características de los elementos  
 **Tipo y longitud de los datos**  
  
 **Valor predeterminado**  
  
 **Repetición**: Opcional. Se puede utilizar una sola vez por cada elemento **TuningOptions** . No se puede utilizar si se especifican los siguientes elementos en el elemento **TuningOptions** :  
  
-   [FeatureSet &#40;DTA, elemento&#41;](../../tools/dta/featureset-element-dta.md)  
  
-   [Partitioning &#40;DTA, elemento&#41;](../../tools/dta/partitioning-element-dta.md)  
  
-   [KeepExisting &#40;DTA, elemento&#41;](../../tools/dta/keepexisting-element-dta.md) se establece en **ALL**  
  
## <a name="element-relationships"></a>Relaciones del elemento  
 **Elemento principal**: [TuningOptions &#40;DTA, elemento&#41;](../../tools/dta/tuningoptions-element-dta.md)  
  
 **Elementos secundarios**  
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente muestra la sección **TuningOptions** de un archivo de entrada XML del Asistente para la optimización de motor de base de datos donde se especifica **DropOnlyMode** . En este ejemplo, la hora de optimización se limita a 24 horas (1440 minutos) y se considerará la eliminación de todos los clúster y no clúster existentes:  
  
```xml  
<TuningOptions  
  <TuningTimeInMin>1440</Name>  
  <KeepExisting>ALIGNED</KeepExisting>  
  <DropOnlyMode />  
</TuningOptions>  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
