---
description: Propiedad FilterValue (RDS)
title: Propiedad FilterValue (RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterValue property [ADO]
ms.assetid: 28f17186-b842-4cf9-b320-a9bb941c481b
author: rothja
ms.author: jroth
ms.openlocfilehash: 44e1eca1ac4a949a3d46fe3a1ec58ff0c0b882dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049395"
---
# <a name="filtervalue-property-rds"></a>Propiedad FilterValue (RDS)
Indica el valor con el que se van a filtrar los registros.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DataControl.FilterValue = String  
```  
  
#### <a name="parameters"></a>Parámetros  
 *DataControl*  
 Variable de objeto que representa un objeto [RDS. Objeto DataControl](./datacontrol-object-rds.md) .  
  
 *String*  
 Valor de **cadena** que representa un valor de datos con el que se van a filtrar los registros (por ejemplo, `'Programmer'` o `125` ).  
  
## <a name="remarks"></a>Observaciones  
 Las propiedades [SortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), **FilterValue**, [FilterCriterion](./filtercriterion-property-rds.md)y [FilterColumn](./filtercolumn-property-rds.md) proporcionan funcionalidad de ordenación y filtrado en la memoria caché del lado cliente. La funcionalidad de ordenación ordena los registros por los valores de una columna. La funcionalidad de filtrado muestra un subconjunto de registros basados en criterios de búsqueda, mientras que el [conjunto de registros](../ado-api/recordset-object-ado.md) completo se mantiene en la memoria caché. El método [RESET](./reset-method-rds.md) ejecutará los criterios y reemplazará el **conjunto de registros** actual por un **conjunto de registros** actualizable.  
  
 Los valores NULL producen un error de falta de coincidencia de tipos.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades FilterColumn, FilterCriterion, FilterValue, SortColumn y SortDirection y el método Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterColumn (propiedad, RDS)](./filtercolumn-property-rds.md)   
 [FilterCriterion (propiedad, RDS)](./filtercriterion-property-rds.md)   
 [Propiedad SortColumn (RDS)](./sortcolumn-property-rds.md)   
 [Propiedad SortDirection (RDS)](./sortdirection-property-rds.md)