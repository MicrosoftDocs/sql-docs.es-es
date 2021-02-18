---
title: TuningTimeInMin (DTA, elemento)
description: En la utilidad DTA, el elemento TuningTimeInMin especifica la longitud máxima de una sesión de optimización en minutos.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- TuningTimeInMin element
ms.assetid: 4973d9ac-20fd-4ac3-bc9f-5d60e39fdb7d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 18bd9b4578a6221a6754f7ef906039633e3ed082
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354432"
---
# <a name="tuningtimeinmin-element-dta"></a>TuningTimeInMin (DTA, elemento)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Especifica la duración máxima de una sesión de optimización en minutos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<DTAInput>  
...code removed...  
    <TuningOptions>  
      <TuningTimeInMin>...</TuningTimeInMin>  
```  
  
## <a name="element-characteristics"></a>Características de los elementos  
  
|Característica|Descripción|  
|--------------------|-----------------|  
|**Tipo y longitud de los datos**|**unsignedInt**, longitud ilimitada.|  
|**Valor predeterminado**|480 minutos (8 horas).|  
|**Repetición**|Obligatoria a menos que se haya especificado un valor para el elemento **NumberOfEvents** .|  
  
## <a name="element-relationships"></a>Relaciones del elemento  
  
|Relación|Elementos|  
|------------------|--------------|  
|**Elemento primario**|[TuningOptions &#40;DTA, elemento&#41;](../../tools/dta/tuningoptions-element-dta.md)|  
|**Elementos secundarios**|None|  
  
## <a name="example"></a>Ejemplo  
  
## <a name="description"></a>Descripción  
 En el siguiente ejemplo de código se muestra cómo establecer un tiempo de optimización máximo de 12 horas:  
  
## <a name="code"></a>Código  
  
```  
<DTAInput>  
  <Server>...</Server>  
  <Workload>...</Workload>  
  <TuningOptions>  
    <TuningTimeInMin>720</TuningTimeInMin>  
...code removed here...  
</DTAInput>  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
