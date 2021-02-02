---
title: Propiedades de base de datos (página del Almacén de consultas)
description: Obtenga información sobre cómo usar la ficha Almacén de consultas del cuadro de diálogo Propiedades de la base de datos para configurar modos de almacén de consultas, intervalos, umbrales y otras propiedades.
ms.custom: ''
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.querystore.f1
author: stevestein
ms.author: sstein
ms.date: 01/25/2021
ms.openlocfilehash: 786d73d8f90b12d094a5f53e1045448e05fa7517
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98812787"
---
# <a name="database-properties-query-store-page"></a>Propiedades de base de datos (página del Almacén de consultas)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]

  Obtenga acceso a esta página desde la base de datos principal y úsela para configurar y modificar las propiedades del almacén de consultas de la base de datos. Estas opciones también se pueden configurar mediante las [opciones ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md). Para información sobre el almacén de consultas, vea [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
## <a name="options"></a>Opciones  
 Modo de operación  
 Los valores válidos son OFF, READ_ONLY y READ_WRITE. OFF deshabilita el almacén de consultas. En el modo READ_WRITE, el almacén de consultas recopila y continúa el plan de consultas y la información de estadística del tiempo de ejecución. En el modo READ_ONLY, la información se puede leer del almacén de consultas, pero no se agrega información nueva. Si se ha agotado el espacio máximo del almacén de consultas, el almacén de consultas cambiará su modo de operación a READ_ONLY.  
  
 Modo de operación (real)  
 Obtiene el modo de operación real del almacén de consultas.  
  
 Modo de operación (solicitado)  
 Obtiene y establece el modo de operación que desea del almacén de consultas.  
  
 Intervalo del vaciado de datos (minutos)  
 Determina la frecuencia con la que los datos escritos en el almacén de consultas se conservan en el disco. Para optimizar el rendimiento, los datos recopilados por el almacén de consultas se escriben de manera asincrónica en el disco. Se configura la frecuencia con la que se produce esta transferencia asincrónica.  
  
 Intervalo de la recopilación de estadísticas  
 Obtiene y establece el valor del intervalo de la recopilación de estadísticas.  
  
 Tamaño máximo (MB)  
 Obtiene y establece el espacio asignado total al almacén de consultas.  
  
 Modo de captura del almacén de consultas  
 -   Ninguno no captura las nuevas consultas.  
  
-   Todo captura todas las consultas.  
  
-   Auto captura las consultas basadas en el consumo de recursos.  
  
 Umbral de consultas obsoletas (días)  
 Obtiene y establece el umbral de consultas obsoletas. Configure el argumento STALE_QUERY_THRESHOLD_DAYS para especificar el número de días en que se conservarán los datos en el almacén de consultas.  
  
 Depurar datos de consulta  
 Quita el contenido del almacén de consultas.  
  
 Gráficos circulares  
 En el gráfico de la izquierda se muestra el consumo total del archivo de la base de datos en el disco y la parte del archivo que se rellena con los datos del almacén de consultas.  
  
 En el gráfico de la derecha se muestra la parte de la cuota del almacén de consultas que se ha consumido actualmente. Observe que no se muestra la cuota en el gráfico de la izquierda. La cuota puede superar el tamaño actual de la base de datos.  
  
## <a name="remarks"></a>Observaciones  
 La característica del almacén de consultas ofrece a los DBA conocimientos sobre el rendimiento y la elección del plan de consultas. Esta característica simplifica la solución de problemas de rendimiento al permitirle encontrar rápidamente diferencias de rendimiento provocadas por cambios en los planes de consulta. Captura automáticamente un historial de consultas, planes y estadísticas en tiempo de ejecución, y los conserva para su revisión. Separa los datos por ventanas de tiempo, lo que permite ver patrones de uso la base de datos y comprender cuándo se produjeron cambios del plan de consultas en el servidor. El almacén de consultas se puede configurar con esta página de propiedades de base de datos de almacén de consultas o mediante la opción [ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) . El almacén de consultas presenta información mediante un cuadro de diálogo [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] . Para más información sobre el almacén de consultas, vea [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
## <a name="see-also"></a>Consulte también  
 [Query Store Stored Procedures &#40;Transact-SQL&#41; (Procedimientos almacenados del Almacén de consultas &#40;Transact-SQL&#41;)](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)   
 [Query Store Catalog Views (Vistas de catálogo del almacén de consultas) &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)