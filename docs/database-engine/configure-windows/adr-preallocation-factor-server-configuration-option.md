---
title: Opción de configuración del factor de asignación previa de ADR | Microsoft Docs
description: Explica el valor de configuración de la instancia de SQL Server para el factor de asignación previa de ADR.
ms.custom: ''
ms.date: 06/01/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ADR Preallocation Factor
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 9469b5de278c01b7b256ee348380ebd1ae19cde4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347467"
---
# <a name="adr-preallocation-factor-configuration-option"></a>Opción de configuración del factor de asignación previa de ADR

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Introducido en SQL Server 2019.

Esta opción de configuración es necesaria para la [recuperación acelerada de la base de datos](../../relational-databases/accelerated-database-recovery-concepts.md).

La recuperación acelerada de la base de datos (ADR) mantiene versiones de los datos con fines de recuperación. Estas versiones se generan como parte de varias operaciones del lenguaje de manipulación de datos (DML). Las versiones se almacenan en una tabla interna denominada almacén de versiones persistente (PVS). 

## <a name="remarks"></a>Observaciones  

El rendimiento puede disminuir si se asignan páginas para la tabla PVS como parte de las operaciones del DML del usuario en primer plano. Para solucionarlo, hay un subproceso en segundo plano que asigna previamente las páginas y las mantiene disponibles para las transacciones del DML. El rendimiento es mejor cuando el subproceso en segundo plano asigna previamente suficientes páginas y el porcentaje de asignaciones de PVS en primer plano se aproxima a 0. El registro de errores contiene entradas con la etiqueta `PreallocatePVS` si el porcentaje es alto y está afectando al rendimiento.

El número de páginas que el subproceso en segundo plano asigna previamente se basa en varias heurísticas de carga de trabajo, pero en gran medida asigna páginas en fragmentos de 512 páginas. El factor de asignación previa de ADR es un múltiplo del fragmento. De forma predeterminada, el factor es 4. Esto significa que asigna previamente 2048 páginas al mismo tiempo cuando es necesario. 

Aunque el subproceso en segundo plano toma en consideración los patrones de carga de trabajo, este factor se puede aumentar si es necesario para mejorar el rendimiento.

> [!CAUTION]
> Si la asignación previa de PVS aumenta demasiado, competirá con otras asignaciones del sistema, lo que podría reducir el rendimiento general.
>
> Antes de modificar este valor, pruebe el rendimiento general del sistema.

## <a name="examples"></a>Ejemplos  

En el ejemplo siguiente se establece el factor de asignación previa en 4.

```tsql
sp_configure 'show advanced options', 1;
RECONFIGURE;
GO 
sp_configure 'ADR Preallocation Factor', 4;
RECONFIGURE;
GO
```

## <a name="see-also"></a>Consulte también  

- [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)
- [Recuperación acelerada de bases de datos](../../relational-databases/accelerated-database-recovery-concepts.md)
- [Administrar la recuperación de bases de datos acelerada](../../relational-databases/accelerated-database-recovery-management.md)