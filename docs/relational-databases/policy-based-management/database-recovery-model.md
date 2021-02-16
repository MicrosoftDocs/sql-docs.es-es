---
title: Modelo de recuperación de base de datos
description: Obtenga información sobre cómo habilitar una directiva para comprobar el modelo de recuperación de copias de seguridad de las bases de datos de usuario para reducir la pérdida de datos.
ms.custom: seo-lt-2019
ms.date: 08/31/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
author: dzsquared
ms.author: drskwier
ms.openlocfilehash: 8c68b60243ec033fff3735901cc4e35134222704
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345502"
---
# <a name="database-recovery-model"></a>Modelo de recuperación de base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En las ediciones Enterprise y Standard de SQL Server, esta regla comprueba si hay bases de datos de usuario que no sean de solo lectura y que tengan el modelo de recuperación establecido en simple. En las bases de datos de producción, recomendamos que utilice el modelo de recuperación completa en lugar del simple. El modo de recuperación completa habilita la recuperación a un momento dado. Esto ayuda a reducir la pérdida de datos si se produce una recuperación ante un desastre.
  
## <a name="best-practices-recommendations"></a>Procedimientos recomendados  
 Las bases de datos de producción deberían estar en modo de recuperación completa y se debería hacer copia de seguridad del registro de transacciones con frecuencia para ayudar a asegurarse de la capacidad de recuperación con una pérdida mínima de datos.
  
## <a name="see-also"></a>Consulte también 
  
 [Información general sobre restauración y recuperación](../backup-restore/restore-and-recovery-overview-sql-server.md)   
  
 [Restauraciones de base de datos completas (modelo de recuperación completa)](../backup-restore/complete-database-restores-full-recovery-model.md)  

 [Copias de seguridad de registros de transacciones](../backup-restore/transaction-log-backups-sql-server.md)   
  
