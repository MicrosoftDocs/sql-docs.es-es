---
title: Estadísticas automáticas
description: Describe la característica de estadísticas automáticas introducida en Analytics Platform System AU7.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: 4311195ce4b748ffeb73c4a001086b3f199b2c5b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052786"
---
# <a name="configure-auto-statistics"></a>Configurar estadísticas automáticas

Aprenda a configurar el almacenamiento de datos paralelo para usar estadísticas automáticas para crear y actualizar estadísticas automáticamente.  Utilice esta función para mejorar los planes de consulta y, por tanto, mejorar el rendimiento de las consultas.

**Se aplica a:** APS (a partir de 2016-AU7)

## <a name="what-are-statistics"></a>¿Qué son las estadísticas?
Las estadísticas para la optimización de consultas son objetos que contienen información estadística acerca de la distribución de valores en una o más columnas de una tabla. El optimizador de consultas utiliza estas estadísticas para estimar la cardinalidad, o número de filas, en el resultado de la consulta. Estas estimaciones de cardinalidad habilitan al optimizador de consultas para crear un plan de consulta de alta calidad. Como ejemplo, en APS, el optimizador de consultas MPP utiliza las estimaciones de cardinalidad para elegir el orden aleatorio o la replicación del menor de dos tablas usadas en una cláusula join y, de este modo, mejorar el rendimiento de las consultas.  Para obtener más información, vea [Statistics](../relational-databases/statistics/statistics.md) y [DBCC SHOW_STATISTICS](../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)

## <a name="what-are-auto-statistics"></a>¿Qué son las estadísticas automáticas?
Las estadísticas automáticas son estadísticas que el optimizador de consultas crea y actualiza automáticamente para mejorar el plan de consulta. Las estadísticas pueden quedar desactualizadas después de las operaciones de carga, inserciones, actualizaciones y eliminaciones. Sin estadísticas automáticas, debe realizar su propio análisis para comprender qué columnas necesitan estadísticas y Cuándo es necesario actualizar las estadísticas.

Estadísticas automáticas incluye las tres opciones siguientes: 

### <a name="auto_create_statistics"></a>AUTO_CREATE_STATISTICS
Cuando está activada la opción automática de creación de estadísticas, AUTO_CREATE_STATISTICS, el optimizador de consultas crea las estadísticas en columnas individuales en el predicado de consulta, según sea necesario, para mejorar las estimaciones de cardinalidad del plan de consulta. Estas estadísticas de columna única se crean en las columnas que aún no tienen un histograma en un objeto de estadísticas existente.

### <a name="auto_update_statistics"></a>AUTO_UPDATE_STATISTICS 
Cuando está activada la opción automática de actualización de estadísticas, AUTO_UPDATE_STATISTICS, el optimizador de consultas determina cuándo las estadísticas pueden estar desfasadas y las actualiza cuando son usadas por una consulta. Las estadísticas se vuelven obsoletas después de que las operaciones de inserción, actualización, eliminación o combinación cambien la distribución de los datos en la tabla o la vista indexada. El optimizador de consultas determina cuándo han podido quedar obsoletas las estadísticas contando el número de modificaciones de datos desde la actualización más reciente de las estadísticas, comparando el número de modificaciones con respecto a un umbral. El umbral se basa en el número de filas de la tabla o la vista indizada.

### <a name="auto_update_statistics_async"></a>AUTO_UPDATE_STATISTICS_ASYNC
La opción de actualización asincrónica de estadísticas AUTO_UPDATE_STATISTICS_ASYNC determina si el Optimizador de consultas utiliza actualizaciones sincrónicas o asincrónicas de las estadísticas. En el caso de los APS, la opción de actualización asincrónica de las estadísticas está activada de forma predeterminada y el optimizador de consultas actualiza las estadísticas de forma asincrónica. La opción AUTO_UPDATE_STATISTICS_ASYNC se aplica a los objetos de estadística creados para índices y columnas únicas de los predicados de consulta, así como a las estadísticas creadas con la instrucción CREATE STATISTICS.

## <a name="configuration-settings-for-system-administrators"></a>Opciones de configuración para administradores del sistema
Después de actualizar a APS AU7, las estadísticas automáticas están habilitadas de forma predeterminada. El administrador del sistema puede habilitar o deshabilitar las estadísticas automáticas con la opción de [cambio de características](appliance-feature-switch.md) en el dispositivo Configuration Manager.  Una vez habilitado, los usuarios pueden cambiar la configuración de las estadísticas por base de datos.
El cambio de los valores de modificador de características requiere un reinicio del servicio en APS.

## <a name="change-auto-statistics-settings-on-a-database"></a>Cambiar la configuración de estadísticas automática en una base de datos
Cuando el administrador del sistema habilita las estadísticas automáticas, puede usar [ALTER DATABASE (almacenamiento de datos paralelos)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) para cambiar la configuración de las estadísticas en una base de datos. Si el administrador del sistema habilita el modificador de características de estadísticas automáticas, las nuevas bases de datos creadas después de la actualización a AU7 tendrán habilitadas las estadísticas automáticas. Todas las bases de datos que existían antes de la actualización a AU7 tienen deshabilitadas las estadísticas automáticas. En el siguiente ejemplo se habilitan las estadísticas automáticas en la base de datos existente myPDW.

```sql
ALTER DATABASE myPDW SET AUTO_CREATE_STATISTICS ON
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS ON 
ALTER DATABASE myPDW SET AUTO_UPDATE_STATISTICS_ASYNC ON
```
 
AUTO_UPDATE STATISTICS_ASYNC opción solo funciona si AUTO_UPDATE_STATISTICS está activada.  Por lo tanto, las estadísticas no se actualizan cuando AUTO_UPDATE_STATISTICS está desactivada y AUTO_UPDATE_STATISTICS_ASYNC está activada. 

### <a name="error-messages"></a>Mensajes de error
Podría recibir el mensaje de error "esta opción no es compatible con PDW".  Este error se produce cuando el administrador del sistema no ha habilitado las estadísticas automáticas e intenta establecer cualquiera de las opciones de estadísticas automáticas en ALTER DATABASE. 

### <a name="limitations-and-restrictions"></a>Limitaciones y restricciones
Las estadísticas automáticas no funcionan en tablas externas. 

### <a name="check-the-current-values"></a>Comprobar los valores actuales
La consulta siguiente devuelve los valores actuales de la configuración de las estadísticas automáticas para todas las bases de datos.

```sql
SELECT NAME
    , IS_AUTO_CREATE_STATS_ON 
    , IS_AUTO_UPDATE_STATS_ON
    , IS_AUTO_UPDATE_STATS_ASYNC_ON
FROM
    sys.databases;
```

Un valor devuelto de 1 significa que la opción está activada y 0 significa que la opción está desactivada. 

## <a name="next-steps"></a>Pasos siguientes
Para ver el rendimiento de las consultas, consulte [supervisar consultas activas](monitoring-active-queries.md)
