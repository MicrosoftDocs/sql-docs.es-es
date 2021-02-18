---
title: Supervisar T-SQL con eventos extendidos
description: Obtenga información sobre cómo usar eventos extendidos para supervisar y solucionar problemas de instrucciones de T-SQL PREDICT en SQL Server Machine Learning Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/24/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 099e01e8e626b7a812ebcf2135b7d5e126a49e4e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337139"
---
# <a name="monitor-predict-t-sql-statements-with-extended-events-in-sql-server-machine-learning-services"></a>Supervisar instrucciones de T-SQL PREDICT con eventos extendidos en SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Obtenga información sobre cómo usar eventos extendidos para supervisar y solucionar problemas de instrucciones de T-SQL [PREDICT](../../t-sql/queries/predict-transact-sql.md) en SQL Server Machine Learning Services.

## <a name="table-of-extended-events"></a>Tabla de eventos extendidos

Los siguientes eventos extendidos están disponibles en todas las versiones de SQL Server que admiten instrucciones de T-SQL [PREDICT](../../t-sql/queries/predict-transact-sql.md). 

| name                       | object_type | description |
|----------------------------|-------------|-------------|
| predict_function_completed | event       | Detalles del tiempo de ejecución integrado|
| predict_model_cache_hit    | event       | Se produce cuando se recupera un modelo de la caché del modelo de la función PREDICT. El uso de este evento junto con otros eventos predict_model_cache_* permite solucionar problemas causados por la caché del modelo de la función PREDICT.|
| predict_model_cache_insert | event       | Se produce cuando se inserta un modelo en la caché del modelo de la función PREDICT. El uso de este evento junto con otros eventos predict_model_cache_* permite solucionar problemas causados por la caché del modelo de la función PREDICT.   |
| predict_model_cache_miss   | event       | Se produce cuando no se encuentra un modelo en la caché del modelo de la función PREDICT. Las apariciones frecuentes de este evento podrían indicar que SQL Server necesita más memoria. El uso de este evento junto con otros eventos predict_model_cache_* permite solucionar problemas causados por la caché del modelo de la función PREDICT.|
| predict_model_cache_remove | event       | Se produce cuando se quita un modelo de la caché del modelo para la función PREDICT. El uso de este evento junto con otros eventos predict_model_cache_* permite solucionar problemas causados por la caché del modelo de la función PREDICT.|

## <a name="query-for-related-events"></a>Consultar eventos relacionados

Para ver una lista de todas las columnas devueltas para estos eventos, ejecute la siguiente consulta en SQL Server Management Studio:

```sql
SELECT *
FROM sys.dm_xe_object_columns
WHERE object_name LIKE 'predict%'
```

## <a name="examples"></a>Ejemplos

Para capturar información sobre el rendimiento de una sesión de puntuación mediante PREDICT:

1. Cree una nueva sesión de eventos extendidos con Management Studio u otra [herramienta](../../relational-databases/extended-events/extended-events-tools.md) compatible.
2. Agregue los eventos `predict_function_completed` y `predict_model_cache_hit` a la sesión.
3. Inicie la sesión de eventos extendidos.
4. Ejecute la consulta que usa PREDICT.

En los resultados, revise estas columnas:

+ El valor de `predict_function_completed` muestra la cantidad de tiempo que la consulta dedicó a cargar el modelo y la puntuación.
+ El valor booleano de `predict_model_cache_hit` indica si la consulta usó o no un modelo almacenado en caché. 

### <a name="native-scoring-model-cache"></a>Caché del modelo de puntuación nativo

Además de los eventos específicos de la predicción (PREDICT), puede utilizar las siguientes consultas para obtener más información sobre el uso de la caché y el modelo en caché:

Ver la caché del modelo de puntuación nativo:

```sql
SELECT *
FROM sys.dm_os_memory_clerks
WHERE type = 'CACHESTORE_NATIVESCORING';
```

Ver los objetos en la caché del modelo:

```sql
SELECT *
FROM sys.dm_os_memory_objects
WHERE TYPE = 'MEMOBJ_NATIVESCORING';
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los eventos extendidos (a veces denominados XEvents) y cómo realizar un seguimiento de los eventos de una sesión, vea estos artículos:

+ [Supervisar scripts de R y Python con eventos extendidos en SQL Server Machine Learning Services](extended-events.md)
+ [Conceptos y arquitectura de eventos extendidos](../../relational-databases/extended-events/extended-events.md)
+ [Configuración de la captura de eventos en SSMS](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)
+ [Administrar sesiones de eventos en el Explorador de objetos](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)