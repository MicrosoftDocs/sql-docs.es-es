---
title: ISSDataClassification | Microsoft Docs
description: Interfaz ISSDataClassification
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSDataClassification
apitype: COM
helpviewer_keywords:
- ISSDataClassification interface
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: 6a0d7eb233df6712d84318a59c53c3dddf2d6af9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204008"
---
# <a name="issdataclassification"></a>ISSDataClassification
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../../../includes/applies-to-version/sql-asdb-asa.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  La interfaz **ISSDataClassification** proporciona la funcionalidad para recuperar los datos de clasificación de confidencialidad del conjunto de filas activo tal y como se reciben de SQL Server.
  

## <a name="methods"></a>Métodos

|Método|Descripción|  
|------------|-----------------|  
|[ISSDataClassification::GetSensitivityClassification](../../oledb/ole-db-interfaces/issdataclassification-getsensitivityclassification-ole-db.md)|Devuelve un puntero a una estructura SENSITIVITYCLASSIFICATION que contiene información de clasificación de confidencialidad.|  

## <a name="see-also"></a>Consulte también  
 [Interfaces &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/oledb-driver-for-sql-server-ole-db-interfaces.md)   
 [Conjuntos de filas](../ole-db-rowsets/rowsets.md)   
 [Uso de la clasificación de datos](../features/using-data-classification.md)
