---
title: ISSCommandWithParameters::GetParameterProperties (OLE DB) | Microsoft Docs
description: ISSCommandWithParameters::GetParameterProperties devuelve una matriz de estructuras de conjunto de propiedades en OLE DB Driver for SQL Server, una por cada parámetro UDT o XML.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSCommandWithParameters::GetParameterProperties (OLE DB)
apitype: COM
helpviewer_keywords:
- GetParameterProperties method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b820f2bb9fe248b8ca4645024214b19112c56ad8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183694"
---
# <a name="isscommandwithparametersgetparameterproperties-ole-db"></a>ISSCommandWithParameters::GetParameterProperties (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Devuelve una matriz de estructuras de conjunto de propiedades SSPARAMPROPS, un conjunto de propiedades SSPARAMPROPS para cada parámetro XML o UDT.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
HRESULT GetParameterProperties(  
      DB_UPARAMS *pcParams,  
      SSPARAMPROPS **prgParamProperties);  
```  
  
## <a name="arguments"></a>Argumentos  
 *pcParams*[out][in]  
 Un puntero a la memoria que contiene el número de estructuras SSPARAMPROPS devueltas en *prgParamProperties*.  
  
 *prgParamProperties*[out]  
 Un puntero a la memoria que devuelve una matriz de estructuras SSPARAMPROPS. El proveedor asigna memoria para las estructuras y devuelve la dirección a esta memoria; el consumidor libera esa memoria con `IMalloc::Free` cuando ya no necesita las estructuras. Antes de llamar a `IMalloc::Free` para *prgParamProperties*, el consumidor debe llamar también a `VariantClear` para la propiedad *vValue* de cada estructura DBPROP con el fin de evitar una fuga de memoria en los casos donde la variante contenga un tipo de referencia (como un BSTR). Si *pcParams* da cero como resultado o se produce un error distinto de DB_E_ERRORSOCCURRED, el proveedor no asigna memoria y se asegura de que *prgParamProperties* dé como resultado un puntero nulo.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 El método `GetParameterProperties` devuelve los mismos códigos de error que el método OLE DB básico `ICommandProperties::GetProperties`, salvo en que no puede dar DB_S_ERRORSOCCURRED ni DB_E_ERRORSOCCURED.  
  
## <a name="remarks"></a>Observaciones  
 El método `ISSCommandWithParameters::GetParameterProperties` se comporta de forma coherente con respecto a `GetParameterInfo`. Si no se ha llamado a [`ISSCommandWithParameters::SetParameterProperties`](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md) ni a `SetParameterInfo` o se les ha llamado con cParams igual a cero, `GetParameterInfo` deriva la información del parámetro y la devuelve. Si se ha llamado a `ISSCommandWithParameters::SetParameterProperties` o a `SetParameterInfo` para al menos un parámetro, el método `ISSCommandWithParameters::GetParameterProperties` devuelve propiedades solo para esos parámetros para los que se ha llamado a `ISSCommandWithParameters::SetParameterProperties`. Si se llama a `ISSCommandWithParameters::SetParameterProperties` después de `ISSCommandWithParameters::GetParameterProperties` o `GetParameterInfo`, las llamadas subsiguientes a `ISSCommandWithParameters::GetParameterProperties` devuelven los valores invalidados de esos parámetros para los que se ha llamado al método `ISSCommandWithParameters::SetParameterProperties`.  
  
 La estructura SSPARAMPROPS se define del siguiente modo:  
  
 `struct SSPARAMPROPS {`  
  
 `DBORDINAL iOrdinal;`  
  
 `ULONG cPropertySets;`  
  
 `DBPROPSET *rgPropertySets;`  
  
 `};`  
  
|Member|Descripción|  
|------------|-----------------|  
|*iOrdinal*|El ordinal del parámetro que se ha pasado.|  
|*cPropertySets*|El número de estructuras DBPROPSET de *rgPropertySets*.|  
|*rgPropertySets*|Un puntero a la memoria que devuelve una matriz de estructuras DBPROPSET.|  
  
## <a name="see-also"></a>Consulte también  
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  
