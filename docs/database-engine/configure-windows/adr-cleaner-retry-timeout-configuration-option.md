---
title: " Opción de configuración \"ADR cleaner retry timeout (min)\" | Microsoft Docs"
description: Explica el valor de configuración de la instancia de SQL Server para el tiempo de expiración de reintento del limpiador de ADR.
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR cleaner retry timeout (min)
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 05502bbf760b0f8aa18d81739ce89e19e92397cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347476"
---
# <a name="adr-cleaner-retry-timeout-min-configuration-option"></a>Opción de configuración "ADR cleaner retry timeout (min)"

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introducido en SQL Server 2019.

Esta opción de configuración es necesaria para la [recuperación acelerada de la base de datos](../../relational-databases/accelerated-database-recovery-concepts.md). El limpiador es el proceso asincrónico que se reactiva periódicamente y limpia las versiones de página que no son necesarias.

En ocasiones, el limpiador experimenta incidencias a la vez que se adquieren bloqueos de nivel de objeto debido a conflictos con la carga de trabajo del usuario durante su limpieza. Realiza un seguimiento de estas páginas en una lista independiente. El tiempo de expiración de reintento del limpiador de ADR (con un valor predeterminado de 15) controla la cantidad de tiempo que el limpiador dedicaría exclusivamente a reintentar la adquisición y limpieza del bloqueo de objeto de la página antes de abandonar la limpieza. La finalización de una limpieza con el 100 % de éxito es fundamental para mantener el crecimiento de las transacciones anuladas en el mapa de transacciones anuladas. Si no se puede limpiar la lista independiente en el tiempo de expiración establecido, se abandonará la limpieza actual y se iniciará la siguiente.

## <a name="remarks"></a>Observaciones  

El limpiador tiene un único subproceso en SQL Server 2019 y, por tanto, una instancia de SQL Server solo puede funcionar en una base de datos a la vez. Si la instancia tiene más de una base de datos de usuario con el ADR habilitado, no aumente el tiempo de expiración hasta un valor grande, ya que podría retrasar la limpieza en una base de datos mientras se produce el reintento en otra.

## <a name="examples"></a>Ejemplos

En los siguientes ejemplos se establece el tiempo de expiración de reintento del limpiador.

```tsql
sp_configure 'show advanced options', 1;  
RECONFIGURE;
GO 
sp_configure 'ADR cleaner retry timeout', 15;  
RECONFIGURE;  
GO  
```  

## <a name="see-also"></a>Consulte también  

- [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Recuperación acelerada de bases de datos](../../relational-databases/accelerated-database-recovery-concepts.md)
- [Administración de la recuperación de bases de datos acelerada](../../relational-databases/accelerated-database-recovery-management.md)