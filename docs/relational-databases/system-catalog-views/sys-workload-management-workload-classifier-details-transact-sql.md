---
description: sys.workload_management_workload_classifier_details (Transact-SQL)
title: sys.workload_management_workload_classifier_details (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/05/2019
ms.prod: sql
ms.technology: system-objects
ms.prod_service: sql-data-warehouse
ms.reviewer: jrasnick
ms.topic: reference
dev_langs:
- TSQL
author: ronortloff
ms.author: rortloff
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: b758ec142d3627c545b2e4f98c7787b1dd195685
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207143"
---
# <a name="sysworkload_management_workload_classifier_details-transact-sql"></a>sys.workload_management_workload_classifier_details (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

  Devuelve los detalles de cada clasificador.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|
|classifier_id|**int**|IDENTIFICADOR del clasificador.  No admite valores NULL.|
|classifier_type|**sysname**|Se combina con [Sys.workload_management_workload_classifiers](sys-workload-management-workload-classifiers-transact-sql.md).|`membername`</br>`wlm_label`</br>`wlm_context`</br>`start_time`</br>`end_time`|
|classifier_value|**sysname**|Valor del clasificador. No admite valores NULL.||

## <a name="permissions"></a>Permisos

Requiere el permiso VIEW SERVER STATE.

## <a name="next-steps"></a>Pasos siguientes
  
Para obtener una lista de todas las vistas de catálogo de Azure Synapse Analytics y almacenamiento de datos paralelos, consulte [vistas de catálogo de Azure Synapse Analytics y almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md). Para crear un clasificador de cargas de trabajo, consulte [crear clasificador de cargas de trabajo](../../t-sql/statements/create-workload-classifier-transact-sql.md). Para más información sobre la clasificación de cargas de trabajo, consulte [importancia](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification) de la carga de trabajo y la [clasificación de cargas](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification) de trabajo de Azure Synapse Analytics
