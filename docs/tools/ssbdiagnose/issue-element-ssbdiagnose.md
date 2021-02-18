---
title: Elemento Issue
description: En SQL Server, el elemento Issue informa de un problema detectado por la utilidad ssbdiagnose. El archivo de salida XML tiene un elemento Issue por cada problema notificado.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- issue element
- XML output file format [ssbdiagnose], issue element
- ssbdiagnose
ms.assetid: 2246a886-686b-44ca-9771-b155cedad8be
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 5d7801f6234b8c94126f202db30155c8cd58be27
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338516"
---
# <a name="issue-element-ssbdiagnose"></a>Elemento Issue (ssbdiagnose)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Informa de un problema encontrado por la utilidad **ssbdiagnose** . El archivo de salida XML de **ssbdiagnose** tiene un elemento Issue por cada problema notificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
<Issue  
    type="..."   
    code="..."   
    server="..."   
    database="..."   
    object="...">   
    ...   
</Issue>  
```  
  
## <a name="element-attributes"></a>Atributos del elemento  
  
|Atributo|Descripción|  
|---------------|-----------------|  
|**type**|Identifica la categoría del problema sobre el que informa el elemento Issue.<br /><br /> **"Diagnóstico"** Informa de un problema de configuración encontrado al analizar una configuración de [!INCLUDE[ssSB](../../includes/sssb-md.md)] .<br /><br /> **"Problema"** Informa de un problema que ha impedido que **ssbdiagnose** complete el análisis. Corrija el problema y vuelva a ejecutar **ssbdiagnose**.<br /><br /> **"Evento"** Informa de un evento de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] encontrado al ejecutar una comprobación de **-RUNTIME** . Solo se informa de los eventos si se especifica **-SHOWEVENTS** .|  
|**code**|Identifica el número de error del mensaje.|  
|**servidor**|Identifica la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] en la que se encontró el problema. Si el problema estaba en una instancia predeterminada, el atributo "server" solo tiene el nombre del equipo. Si el problema estaba en una instancia con nombre, el atributo "server" tiene el formato nombreDeEquipo\nombreDeInstancia.|  
|**database**|Identifica el nombre de la base de datos en la que se encontró el problema.|  
|**object**|Identifica el nombre del objeto en el que se encontró el problema. Si el problema era un problema de nivel de instancia o de base de datos, el atributo "object" repite el nombre de la instancia o de la base de datos.|  
  
## <a name="element-characteristics"></a>Características de los elementos  
  
|Característica|Descripción|  
|--------------------|-----------------|  
|**Tipo y longitud de los datos**|**string**, longitud ilimitada.|  
|**Valor**|Devuelve el texto del mensaje de error.|  
|**Repetición**|Una vez por error notificado.|  
  
## <a name="element-relationships"></a>Relaciones del elemento  
  
|Relación|Elementos|  
|------------------|--------------|  
|**Elemento primario**|[Elemento DiagnosticInformation &#40;ssbdiagnose&#41;](../../tools/ssbdiagnose/diagnosticinformation-element-ssbdiagnose.md)|  
|**Elementos secundarios**|None|  
  
## <a name="example"></a>Ejemplo  
 Este elemento informa de un error 1102 para una base de datos que no tiene una clave maestra, en la que se encontró el error al analizar una configuración de [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
```  
<Issue type="Diagnosis" code="1102" server="TestComputer" database="TargetDB" object="TargetDB">The master key was not found</diagnostic>  
```  
  
## <a name="see-also"></a>Consulte también  
 [Utilidad ssbdiagnose &#40;Service Broker&#41;](../../tools/ssbdiagnose/ssbdiagnose-utility-service-broker.md)  
  
  
