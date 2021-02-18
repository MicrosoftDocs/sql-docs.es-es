---
title: TuningOptions (DTA, elemento)
description: En la utilidad DTA, el elemento TuningOptions contiene las opciones de optimización de una sesión de optimización concreta.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- TuningOptions element
ms.assetid: 58a22ba1-8e03-411f-bd46-85e4540f217a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 5ee768da96546cfcc4619098f5f0c962205047d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351117"
---
# <a name="tuningoptions-element-dta"></a>TuningOptions (DTA, elemento)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Contiene las opciones de optimización de una sesión de optimización concreta.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<DTAInput>  
    <Server>  
...code removed...  
    <Workload>...</Workload>  
    <TuningOptions>...</TuningOptions>  
```  
  
## <a name="element-characteristics"></a>Características de los elementos  
  
|Característica|Descripción|  
|--------------------|-----------------|  
|**Tipo y longitud de los datos**|Ninguno.|  
|**Valor predeterminado**|Ninguno.|  
|**Repetición**|Opcional. Si se utiliza, solo puede hacerse una vez para cada elemento **DTAInput** .|  
  
## <a name="element-relationships"></a>Relaciones del elemento  
  
|Relación|Elementos|  
|------------------|--------------|  
|**Elemento primario**|[Elemento DTAInput &#40;DTA&#41;](../../tools/dta/dtainput-element-dta.md)|  
|**Elementos secundarios**|Elemento **ReportSet** . Para obtener más información, vea el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Elemento **TuningLogTable** . Para obtener más información, vea el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Elemento **NumberOfEvents** . Para obtener más información, vea el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> [TuningTimeInMin &#40;DTA, elemento&#41;](../../tools/dta/tuningtimeinmin-element-dta.md)<br /><br /> [StorageBoundInMB &#40;DTA, elemento&#41;](../../tools/dta/storageboundinmb-element-dta.md)<br /><br /> Elemento **MaxKeyColumnsInIndex** . Para obtener más información, vea el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Elemento **MaxColumnsInIndex** . Para obtener más información, vea el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Elemento **MinPercentageImprovement** . Para obtener más información, vea el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100)<br /><br /> [TestServer &#40;DTA, elemento&#41;](../../tools/dta/testserver-element-dta.md)<br /><br /> [FeatureSet &#40;DTA, elemento&#41;](../../tools/dta/featureset-element-dta.md)<br /><br /> [Partitioning &#40;DTA, elemento&#41;](../../tools/dta/partitioning-element-dta.md)<br /><br /> [DropOnlyMode &#40;DTA, elemento&#41;](../../tools/dta/droponlymode-element-dta.md)<br /><br /> [KeepExisting &#40;DTA, elemento&#41;](../../tools/dta/keepexisting-element-dta.md)<br /><br /> [OnlineIndexOperation &#40;DTA, elemento&#41;](../../tools/dta/onlineindexoperation-element-dta.md)<br /><br /> [DatabaseToConnect &#40;DTA, elemento&#41;](../../tools/dta/databasetoconnect-element-dta.md)<br /><br /> Elemento **IgnoreConstantsInWorkload** . Para obtener más información, vea el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100).<br /><br /> Elemento **RetainShellDB** . Para obtener más información, vea el [esquema XML del Asistente para la optimización de motor de base de datos](https://go.microsoft.com/fwlink/?linkid=43100).|  
  
## <a name="example"></a>Ejemplo  
 Para ver ejemplos del elemento **TuningOptions**, vea [Ejemplos de archivos de entrada XML &#40;DTA&#41;](../../tools/dta/xml-input-file-samples-dta.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia del archivo de entrada XML &#40;Asistente para la optimización de motor de base de datos&#41;](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  
