---
title: Modificador de características
description: Muestra información sobre los dos modificadores de características que se introdujeron en Analytics Platform System AU7.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
monikerRange: '>= aps-pdw-2016-au7'
ms.openlocfilehash: 656b1592dd50567f59744e39c24be7d93b5b220f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052836"
---
# <a name="appliance-feature-switches"></a>Conmutadores de características del dispositivo

La página **conmutador de características** muestra información sobre los modificadores de características que se introducen en Analytics Platform System AU7 y versiones posteriores. Use esta página de configuración para actualizar o habilitar o deshabilitar características y configuraciones en Analytics Platform System.

> [!NOTE]
> Los cambios en los valores del modificador de características requieren un reinicio del servicio.

![Conmutador de características del dispositivo DWConfig](media/feature-switch/SQL_Server_PDW_DWConfig_feature_switch.png "Conmutador de características del dispositivo DWConfig")

## <a name="autostatsenabled"></a>AutoStatsEnabled

Controla la característica de estadísticas automáticas. Este modificador de característica se establece en true de forma predeterminada después de actualizar a AU7. Cualquier base de datos creada después de la actualización heredará la creación automática y la actualización asincrónica de las estadísticas. En el caso de las bases de datos existentes, los administradores de bases de datos pueden habilitar las estadísticas automáticas con [ALTER DATABASE (almacenamiento de datos paralelos)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw). Para obtener más información sobre las estadísticas, vea [estadísticas](../relational-databases/statistics/statistics.md).

## <a name="maxdopforinsertqueries"></a>MaxDOPForInsertQueries

Permite seleccionar la configuración maxdop mayor que 1 para las operaciones de inserción/selección. Las opciones de esta configuración son 0, 1, 2 y 4, y el valor predeterminado es 1.

## <a name="optimizecommonsubexpressions"></a>OptimizeCommonSubExpressions

Mejora el rendimiento de las consultas al eliminar el movimiento de datos para la subexpresión común en el optimizador de consultas de SQL. [Aquí](common-sub-expression-elimination.md)puede encontrar una explicación detallada de esta característica.

## <a name="usecatalogqueries"></a>UseCatalogQueries

El uso de objetos de catálogo para algunas llamadas de metadatos en lugar de usar SMO ha mostrado la mejora del rendimiento. Establecido en true de forma predeterminada en CU 7.1, este modificador controla ese comportamiento.

## <a name="dmsprocessstopmessagetimeoutinseconds"></a>DmsProcessStopMessageTimeoutInSeconds

Controla el tiempo que el servicio de movimiento de datos (DMS) espera a sincronizarse en un sistema ocupado cuando se cancela una consulta que implica el movimiento de datos. De forma predeterminada, al actualizar a AU7, este valor se establece en 900 segundos (15 minutos). El intervalo válido es de 0-3600 segundos.
