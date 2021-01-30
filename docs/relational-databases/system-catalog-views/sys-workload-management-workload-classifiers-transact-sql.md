---
description: sys.workload_management_workload_classifiers (Transact-SQL)
title: sys.workload_management_workload_classifiers (Transact-SQL) | Microsoft Docs
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
ms.openlocfilehash: 0b354af4a220adcaed93663fc899f80306e0a85a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190846"
---
# <a name="sysworkload_management_workload_classifiers-transact-sql"></a>sys.workload_management_workload_classifiers (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

 Devuelve los detalles de los clasificadores de carga de trabajo.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|
|classifier_id|**int**|IDENTIFICADOR único del clasificador. No admite valores NULL||
group_name|**sysname**|Nombre del grupo de cargas de trabajo al que está asignado el clasificador. No admite valores NULL. Se combina con sys.workload_management_workload_groups ||
name|**sysname**|Nombre del clasificador. Debe ser único para la instancia de. No admite valores NULL.||
|importance|**sysname**|Es la importancia relativa de una solicitud en este grupo de cargas de trabajo y entre grupos de cargas de trabajo para los recursos compartidos.  La importancia especificada en el clasificador invalida la configuración de importancia del grupo de cargas de trabajo. Acepta valores NULL.  Cuando es null, se usa la configuración de importancia del grupo de cargas de trabajo.|Low, below_normal, normal (valor predeterminado), above_normal, alto |
|create_time|**datetime**|Hora en que se creó el clasificador. No admite valores NULL.||
modify_time|**datetime**|Hora en que se modificó por última vez el clasificador. No admite valores NULL.||
is_enabled|**bit**|INTERNAL||
|&nbsp;||||
  
## <a name="permissions"></a>Permisos

Requiere el permiso VIEW SERVER STATE.

## <a name="next-steps"></a>Pasos siguientes

 Para obtener una lista de todas las vistas de catálogo de Azure Synapse Analytics y almacenamiento de datos paralelos, consulte [vistas de catálogo de Azure Synapse Analytics y almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md). Para crear un clasificador de cargas de trabajo, consulte [crear clasificador de cargas de trabajo](../../t-sql/statements/create-workload-classifier-transact-sql.md). Para obtener más información sobre la clasificación de cargas de trabajo, consulte clasificación de la [carga de trabajo](/azure/sql-data-warehouse/sql-data-warehouse-workload-classification)
