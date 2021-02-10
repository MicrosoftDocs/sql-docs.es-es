---
description: Propiedad FilterCriterion (RDS)
title: FilterCriterion (propiedad, RDS) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- FilterCriterion property [RDS]
ms.assetid: 24eb03ba-ccfd-4353-b6af-03586b2da6fd
author: rothja
ms.author: jroth
ms.openlocfilehash: 09f5306f6df62792f8be42ed06081596682ca0bd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053156"
---
# <a name="filtercriterion-property-rds"></a>Propiedad FilterCriterion (RDS)
Indica el operador de evaluación que se va a usar en el valor de filtro.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DataControl.FilterCriterion = String  
```  
  
#### <a name="parameters"></a>Parámetros  
 *DataControl*  
 Variable de objeto que representa un objeto [RDS. Objeto DataControl](./datacontrol-object-rds.md) .  
  
 *String*  
 Valor de **cadena** que especifica el operador de evaluación de [FilterValue](./filtervalue-property-rds.md) en los registros. Puede ser cualquiera de los siguientes: <, \<=, > , >=, = o <>.  
  
## <a name="remarks"></a>Observaciones  
 Las propiedades [SortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), [FilterValue](./filtervalue-property-rds.md), **FilterCriterion** y [FilterColumn](./filtercolumn-property-rds.md) proporcionan funcionalidad de ordenación y filtrado en la memoria caché del lado cliente. La funcionalidad de ordenación ordena los registros por los valores de una columna. La funcionalidad de filtrado muestra un subconjunto de registros basados en criterios de búsqueda, mientras que el [conjunto de registros](../ado-api/recordset-object-ado.md) completo se mantiene en la memoria caché. El método [RESET](./reset-method-rds.md) ejecutará los criterios y reemplazará el **conjunto de registros** actual por un **conjunto de registros** actualizable.  
  
 El operador "! =" no es válido para **FilterCriterion**; en su lugar, use "<>".  
  
 Si se establecen las propiedades Filter y Sort y se llama al método **RESET** , primero se filtra el conjunto de filas y, a continuación, se ordena. En el caso de las ordenaciones ascendentes, los valores NULL se encuentran en la parte superior; en el caso de las ordenaciones descendentes, los valores NULL están en la parte inferior (el comportamiento predeterminado es ascendente).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades FilterColumn, FilterCriterion, FilterValue, SortColumn y SortDirection y el método Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [FilterColumn (propiedad, RDS)](./filtercolumn-property-rds.md)   
 [Propiedad FilterValue (RDS)](./filtervalue-property-rds.md)   
 [Propiedad SortColumn (RDS)](./sortcolumn-property-rds.md)   
 [Propiedad SortDirection (RDS)](./sortdirection-property-rds.md)