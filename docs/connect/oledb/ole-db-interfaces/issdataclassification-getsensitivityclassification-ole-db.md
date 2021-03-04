---
title: ISSDataClassification::GetSensitivityClassification | Microsoft Docs
description: ISSDataClassification::GetSensitivityClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification::GetSensitivityClassification
apitype: COM
helpviewer_keywords:
- GetSensitivityClassification method
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 232bcf6187967e9dab7b09414626785d7808e18f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837332"
---
# <a name="issdataclassificationgetsensitivityclassification"></a>ISSDataClassification::GetSensitivityClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Recupera los datos de clasificación de confidencialidad del conjunto de filas activo. Para obtener más información y un ejemplo de código, vea [Uso de la clasificación de datos](../features/using-data-classification.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
HRESULT GetSensitivityClassification(
    SENSITIVITYCLASSIFICATION** ppSensitivityClassification);
```  
  
## <a name="arguments"></a>Argumentos  
  *ppSensitivityClassification*[out]  
 Puntero a un puntero de la estructura SENSITIVITYCLASSIFICATION. Si el método produce un error o no hay información de clasificación de datos disponible, el proveedor no asigna memoria y se asegura de que el argumento ppSensitivityClassification dé como resultado un puntero nulo.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 S_OK  
 El método se ha llevado a cabo de forma correcta.    
  
 E_INVALIDARG  
 El argumento *ppSensitivityClassification* era NULL.  
  
 E_OUTOFMEMORY  
 OLE DB Driver for SQL Server no pudo asignar memoria suficiente para completar la solicitud.  

  
## <a name="remarks"></a>Comentarios  
El controlador OLE DB Driver for SQL Server asigna un bloque de memoria que contiene la estructura SENSITIVITYCLASSIFICATION y los datos a los que se hace referencia en esta estructura. Cuando el consumidor ya no necesita tener acceso a los datos de clasificación, debe desasignar esta memoria llamando al método [IMalloc::Free](/windows/win32/api/objidl/nf-objidl-imalloc-free).  
  
 La estructura SENSITIVITYCLASSIFICATION se define de la siguiente manera:
  
```cpp
typedef struct tagSensitivityClassification
{
    USHORT                     cSensitivityLabels;
    SENSITIVITYLABEL          *rgSensitivityLabels;
    USHORT                     cInformationTypes;
    INFORMATIONTYPE           *rgInformationTypes;
    USHORT                     cColumnSensitivityMetadata;
    COLUMNSENSITIVITYMETADATA *rgColumnSensitivityMetadata;
    SENSITIVITYRANKENUM        eQuerySensitivityRank;
} SENSITIVITYCLASSIFICATION;
```  

|Miembro|Descripción|  
|------------|-----------------|  
|*cSensitivityLabels*|El número de estructuras SENSITIVITYLABEL en *rgSensitivityLabels*.|  
|*rgSensitivityLabels*|Una matriz de estructuras SENSITIVITYLABEL.|  
|*cInformationTypes*|El número de estructuras INFORMATIONTYPE en *rgInformationTypes*.|  
|*rgInformationTypes*|Una matriz de estructuras INFORMATIONTYPE.|  
|*cColumnSensitivityMetadata*|El número de estructuras COLUMNSENSITIVITYMETADATA en *rgColumnSensitivityMetadata*.|  
|*rgColumnSensitivityMetadata*|Una matriz de estructuras COLUMNSENSITIVITYMETADATA.|  
|*eQuerySensitivityRank*|Una clasificación relativa de la confidencialidad de una consulta que se ejecutó para obtener el conjunto de filas.|  

La estructura SENSITIVITYLABEL se define de la siguiente manera:
```cpp
typedef struct tagSENSITIVITYLABEL
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} SENSITIVITYLABEL;
```

|Miembro|Descripción|  
|------------|-----------------|  
|*pwszName*|El nombre de una etiqueta de confidencialidad.|  
|*pwszId*|El identificador de una etiqueta de confidencialidad.|  

La estructura INFORMATIONTYPE se define de la siguiente manera:
```cpp
typedef struct tagINFORMATIONTYPE
{
    LPOLESTR pwszName;
    LPOLESTR pwszId;
} INFORMATIONTYPE;
```

|Miembro|Descripción|  
|------------|-----------------|  
|*pwszName*|El nombre de un tipo de información.|  
|*pwszId*|El identificador de un tipo de información.|  

La estructura COLUMNSENSITIVITYMETADATA se define de la siguiente manera:
```cpp
typedef struct tagCOLUMNSENSITIVITYMETADATA
{
    SENSITIVITYPROPERTY* rgSensitivityProperties;
    USHORT cSensitivityProperties;
} COLUMNSENSITIVITYMETADATA;
```

|Miembro|Descripción|  
|------------|-----------------|  
|*cSensitivityProperties*|El número de estructuras SENSITIVITYPROPERTY en *rgSensitivityProperties*.|  
|*rgSensitivityProperties*|Una matriz de estructuras SENSITIVITYPROPERTY.|  

La enumeración SENSITIVITYRANKENUM se define de la siguiente manera:
```cpp
typedef enum tagSENSITIVITYRANKENUM
{
    SENSITIVITYRANK_NOT_DEFINED = -1,
    SENSITIVITYRANK_NONE = 0,
    SENSITIVITYRANK_LOW = 10,
    SENSITIVITYRANK_MEDIUM = 20,
    SENSITIVITYRANK_HIGH = 30,
    SENSITIVITYRANK_CRITICAL = 40
} SENSITIVITYRANKENUM;
```

La estructura SENSITIVITYPROPERTY se define de la siguiente manera:
```cpp
typedef struct tagSENSITIVITYPROPERTY
{
    SENSITIVITYLABEL* pSensitivityLabel;
    INFORMATIONTYPE* pInformationType;
    SENSITIVITYRANKENUM eSensitivityRank;
} SENSITIVITYPROPERTY;
```

|Miembro|Descripción|  
|------------|-----------------|  
|*pSensitivityLabel*|Un puntero a una estructura SENSITIVITYLABEL.|  
|*pInformationType*|Un puntero a una estructura INFORMATIONTYPE.|  
|*eSensitivityRank*|Una clasificación relativa de la confidencialidad de una columna que forma parte de los datos por columna.|  

## <a name="see-also"></a>Consulte también  
 [ISSDataClassification](../../oledb/ole-db-interfaces/issdataclassification-ole-db.md)  
 [Conjuntos de filas](../ole-db-rowsets/rowsets.md)  
