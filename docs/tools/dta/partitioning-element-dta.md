---
title: Partitioning (DTA, elemento)
description: En la utilidad DTA, el elemento Partitioning contiene el esquema de particiones que se desea que utilice el Asistente para la optimización de motor de base de datos durante el análisis.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- Partitioning element
ms.assetid: 9bc5d1d5-27a7-4434-966f-c3935794af27
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 7dc261ca057aeebe07cf8dbd8cb4a735f05ffbce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335965"
---
# <a name="partitioning-element-dta"></a>Partitioning (DTA, elemento)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contiene el esquema de particiones que se desea que utilice el Asistente para la optimización de motor de base de datos durante el análisis.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <Partitioning>...</Partitioning>  
```  
  
## <a name="element-characteristics"></a>Características de los elementos  
  
|Característica|Descripción|  
|--------------------|-----------------|  
|**Tipo y longitud de los datos**|**string**, sin longitud máxima.|  
|**Valores permitidos**|**NONE**<br /> Ninguna partición.<br /><br /> **FULL**<br /> Partición total. (Mejora el rendimiento).<br /><br /> **ALIGNED**<br /> Únicamente partición alineada. (Mejora la administración).<br /><br /> Utilice solo uno de estos valores con este elemento.<br /><br /> **ALIGNED** significa que, en la recomendación generada por el Asistente para la optimización de motor de base de datos, cada índice propuesto se divide exactamente igual que la tabla subyacente para la que se ha definido el índice. Los índices no clúster de una vista indizada se alinean con la vista indizada.|  
|**Valor predeterminado**|**NONE**|  
|**Repetición**|Una obligatoria para el elemento **TuningOptions** , a menos que se utilice el elemento **DropOnlyMode** . Si se utiliza **DropOnlyMode** , no es posible utilizar **Partitioning**. Estos elementos son mutuamente exclusivos.|  
  
## <a name="element-relationships"></a>Relaciones del elemento  
  
|Relación|Elementos|  
|------------------|--------------|  
|**Elemento primario**|[TuningOptions &#40;DTA, elemento&#41;](../../tools/dta/tuningoptions-element-dta.md)|  
|**Elementos secundarios**|Ninguno.|  
  
## <a name="example"></a>Ejemplo  
 Para obtener un ejemplo de uso de este elemento, vea [Ejemplo de archivo de entrada XML simple &#40;DTA&#41;](../../tools/dta/simple-xml-input-file-sample-dta.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
