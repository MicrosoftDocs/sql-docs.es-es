---
title: Ver la información del filtro
titleSuffix: SQL Server Profiler
description: Descubra cómo ver los filtros que SQL Server Profiler está aplicando actualmente a las columnas de datos para limitar los eventos de los que se realiza un seguimiento.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 8d002dea-376a-452c-b3ca-3e93656ed75f
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: c4031cd865c84b9bc6171b25d40de21ab29379ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338601"
---
# <a name="view-filter-information-sql-server-profiler"></a>Ver información de un filtro (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describe cómo ver filtros de columnas de datos para clases de eventos mediante el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
### <a name="to-view-filter-information"></a>Para ver la información del filtro  
  
1.  Abra un archivo de seguimiento, tabla de seguimiento o script SQL y, en el menú **Archivo** , haga clic en **Propiedades**. Si edita una plantilla de seguimiento o crea un seguimiento nuevo, omita este paso.  
  
2.  En la pestaña **Selección de eventos** , haga clic con el botón derecho en el nombre de la columna de datos del filtro que quiere ver y haga clic en **Editar filtro de columna**.  
  
3.  En el cuadro de diálogo **Editar filtro** , expanda los operadores de comparación de filtros para ver el valor asignado al criterio especificado. Repita los pasos 2 y 3 para todas las columnas para las que desee ver información del filtro.  
  
> [!NOTE]  
>  Los operadores de comparación con valores asignados se muestran en negrita.  
  
## <a name="see-also"></a>Consulte también  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
