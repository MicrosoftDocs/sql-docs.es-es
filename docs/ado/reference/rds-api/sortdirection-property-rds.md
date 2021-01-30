---
description: Propiedad SortDirection (RDS)
title: Propiedad SortDirection (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SortDirection property [RDS]
ms.assetid: 1d9d8715-e4ad-4ff3-bf7f-f1dc0532d8c2
author: rothja
ms.author: jroth
ms.openlocfilehash: 8bfd6b6c504077bd20ced3d302206c129ab781ae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166122"
---
# <a name="sortdirection-property-rds"></a>Propiedad SortDirection (RDS)
Indica si un criterio de ordenación es ascendente o descendente.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DataControl.SortDirection = value  
```  
  
#### <a name="parameters"></a>Parámetros  
 *DataControl*  
 Variable de objeto que representa un objeto [RDS. Objeto DataControl](./datacontrol-object-rds.md) .  
  
 *Valor*  
 Valor **booleano** que, cuando se establece en **true**, indica que la dirección de ordenación es ascendente. **False** indica un orden descendente.  
  
## <a name="remarks"></a>Observaciones  
 Las propiedades [SortColumn](./sortcolumn-property-rds.md), **SortDirection**, [FilterValue](./filtervalue-property-rds.md), [FilterCriterion](./filtercriterion-property-rds.md)y [FilterColumn](./filtercolumn-property-rds.md) proporcionan funcionalidad de ordenación y filtrado en la memoria caché del lado cliente. La funcionalidad de ordenación ordena los registros utilizando los valores de una columna. La funcionalidad de filtrado muestra un subconjunto de registros basados en criterios de búsqueda, mientras que el [conjunto de registros](../ado-api/recordset-object-ado.md) completo se mantiene en la memoria caché. El método [RESET](./reset-method-rds.md) ejecutará los criterios y reemplazará el **conjunto de registros** actual por un **conjunto de registros** actualizable.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades FilterColumn, FilterCriterion, FilterValue, SortColumn y SortDirection y el método Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Propiedad Sort](../ado-api/sort-property.md)   
 [Propiedad SortColumn (RDS)](./sortcolumn-property-rds.md)