---
description: Quitar combinaciones (Visual Database Tools)
title: Quitar combinaciones
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- removing joins
- joins [SQL Server], removing
- deleting joins
ms.assetid: ccc212a1-fd13-48d6-85e5-5ff310c34bbd
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: b13ffa676da9c96baa120ced607ca7607dea96d9
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88497135"
---
# <a name="remove-joins-visual-database-tools"></a>Quitar combinaciones (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Si no desea que las tablas se combinen mediante una combinación interna o externa, puede quitar la combinación existente entre ellas. Por ejemplo, puede quitar una combinación que el [Diseñador de consultas y vistas](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md) haya creado automáticamente entre dos tablas.  
  
> [!NOTE]  
> Cuando se quita una combinación de una consulta, no se modifica la relación subyacente en la base de datos.  
  
Si dos tablas combinadas forman parte de la consulta y quita todas las condiciones de combinación que existen entre ellas, la consulta resultante es el producto de ambas tablas, es decir, una instancia CROSS JOIN.  
  
### <a name="to-remove-a-join"></a>Para quitar una combinación  
  
-   En el [panel Diagrama](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md), seleccione la línea de combinación de la combinación que desea quitar y, a continuación, presione la tecla SUPR. Puede seleccionar y eliminar varias líneas de combinación al mismo tiempo.  
  
El Diseñador de consultas y vistas quita la línea de combinación y modifica la instrucción en el [panel SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md).  
  
## <a name="see-also"></a>Consulte también  
[Combinar tablas automáticamente (Visual Database Tools)](../../ssms/visual-db-tools/join-tables-automatically-visual-database-tools.md)  
[Combinar tablas manualmente (Visual Database Tools)](../../ssms/visual-db-tools/join-tables-manually-visual-database-tools.md)  
[Realizar consultas con combinaciones &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/query-with-joins-visual-database-tools.md)  
  
