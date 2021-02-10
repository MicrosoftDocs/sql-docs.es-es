---
description: Reset (método) (RDS)
title: Método Reset (RDS) | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Reset method [ADO]
ms.assetid: 3957197a-f543-4d6b-9e11-67a77c2063b7
author: rothja
ms.author: jroth
ms.openlocfilehash: 2acfa0ad906070e36aeefb94decd8eec76b25713
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052946"
---
# <a name="reset-method-rds"></a>Reset (método) (RDS)
Ejecuta la ordenación o el filtro en un conjunto de **registros** del lado cliente basándose en las propiedades de ordenación y filtro especificadas.  
  
> [!IMPORTANT]
>  A partir de Windows 8 y Windows Server 2012, los componentes de servidor RDS ya no se incluyen en el sistema operativo Windows (consulte la guía de compatibilidad de Windows 8 y [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) para obtener más detalles). Los componentes de cliente RDS se quitarán en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Las aplicaciones que utilizan RDS deben migrar al [servicio de datos de WCF](/dotnet/framework/wcf/).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
DataControl.Reset(value)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *DataControl*  
 Variable de objeto que representa un objeto [RDS. Objeto DataControl](./datacontrol-object-rds.md) .  
  
 *value*  
 Opcional. Un valor **booleano** que es **true** (valor predeterminado) Si desea filtrar por el conjunto de filas "filtrado" actual. **False** indica que se filtra por el conjunto de filas original y se quitan las opciones de filtro anteriores.  
  
## <a name="remarks"></a>Observaciones  
 Las propiedades [SortColumn](./sortcolumn-property-rds.md), [SortDirection](./sortdirection-property-rds.md), [FilterValue](./filtervalue-property-rds.md), [FilterCriterion](./filtercriterion-property-rds.md)y [FilterColumn](./filtercolumn-property-rds.md) proporcionan funcionalidad de ordenación y filtrado en la memoria caché del lado cliente. La funcionalidad de ordenación ordena los registros por los valores de una columna. La funcionalidad de filtrado muestra un subconjunto de registros basándose en un criterio de búsqueda, mientras que el [conjunto de registros](../ado-api/recordset-object-ado.md) completo se mantiene en la memoria caché. El método **RESET** ejecutará los criterios y reemplazará el **conjunto de registros** actual por un **conjunto de registros** actualizable.  
  
 Si hay cambios en los datos originales que no se han enviado, se producirá un error en el método de **restablecimiento** . En primer lugar, use el método [SubmitChanges](./submitchanges-method-rds.md) para guardar los cambios en un **conjunto de registros** de lectura/escritura y, a continuación, utilice el método **RESET** para ordenar o filtrar los registros.  
  
 Si desea realizar más de un filtro en el conjunto de filas, puede usar el argumento *booleano* opcional con el método **RESET** . El ejemplo siguiente muestra cómo hacerlo:  
  
```  
ADC.SQL = "Select au_lname from authors"  
ADC.Refresh    ' Get the new rowset.  
  
ADC.FilterColumn = "au_lname"  
ADC.FilterCriterion = "<"  
ADC.FilterValue = "'M'"  
ADC.Reset         ' Rowset now has all Last Names < "M".  
  
ADC.FilterCriterion = ">"  
ADC.FilterValue = "'F'"  
' Passing True is not necessary, because it is the   
' default filter on the current "filtered" rowset.  
ADC.Reset(TRUE)     ' Rowset now has all Last   
                    ' Names < "M" and > "F".  
  
ADC.FilterCriterion = ">"  
ADC.FilterValue = "'T'"  
' Filter on the original rowset, throwing out the  
' previous filter options.  
ADC.Reset(FALSE)   ' Rowset now has all Last Names > "T".  
```  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto DataControl (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades FilterColumn, FilterCriterion, FilterValue, SortColumn y SortDirection y el método Reset (VBScript)](./filter-column-criterion-value-sortcolumn-sortdirection-example-vbscript.md)   
 [Método SubmitChanges (RDS)](./submitchanges-method-rds.md)