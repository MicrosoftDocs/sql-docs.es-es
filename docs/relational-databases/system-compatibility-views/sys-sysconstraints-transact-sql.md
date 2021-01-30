---
description: sys.sysconstraints (Transact-SQL)
title: Restricciones de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysconstraints
- sys.sysconstraints
- sysconstraints_TSQL
- sys.sysconstraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysconstraints compatibility view
- sysconstraints system table
ms.assetid: f2b2e2ad-ba24-48a1-913c-8ee4e0895dc4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6b4b35b030cda96abc944ede2438f6cb0483e5c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99149684"
---
# <a name="syssysconstraints-transact-sql"></a>sys.sysconstraints (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene correspondencias entre restricciones y los objetos que las poseen en la base de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**constid**|**int**|Número de restricción.|  
|**id**|**int**|Id. de la tabla que posee la restricción.|  
|**colid**|**smallint**|Id. de la columna en la que se define la restricción.<br /><br /> 0 = Restricción de tabla|  
|**spare1**|**tinyint**|Reservado|  
|**status**|**int**|Pseudomáscara de bits que indica el estado. Entre los valores posibles figuran los siguientes:<br /><br /> 1 = Restricción PRIMARY KEY<br /><br /> 2 = Restricción UNIQUE KEY<br /><br /> 3 = Restricción FOREIGN KEY<br /><br /> 4 = Restricción CHECK<br /><br /> 5 = Restricción DEFAULT<br /><br /> 16 = Restricción de nivel de columna<br /><br /> 32 = Restricción de nivel de tabla|  
|**actions**|**int**|Reservado|  
|**error**|**int**|Reservado|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
