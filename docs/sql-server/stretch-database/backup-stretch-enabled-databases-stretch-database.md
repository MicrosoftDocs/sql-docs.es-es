---
description: Copia de seguridad de bases de datos habilitadas para Stretch (Stretch Database)
title: Copia de seguridad y restauración de bases de datos habilitadas para Stretch
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Stretch Database, disabling
- disabling Stretch Database
ms.assetid: 18f0dff0-d8ce-4bee-a935-76ed6dfb3208
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: b407e37ec4a6c3337bd5c5bc98018c73792396dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081111"
---
# <a name="backup-stretch-enabled-databases-stretch-database"></a>Copia de seguridad de bases de datos habilitadas para Stretch (Stretch Database)
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


 Las copias de seguridad de bases de datos ayudan a recuperarse de muchos tipos de errores y desastres.  
  
 -   Debe hacer una copia de seguridad de las bases de datos de SQL Server habilitadas para Stretch.  
      
 -   Microsoft Azure realiza automáticamente una copia de seguridad de los datos remotos que Stretch Database ha migrado de SQL Server a Azure.  

> [!TIP]
> La copia de seguridad es solo una parte de una completa solución de continuidad del negocio y alta disponibilidad. Para obtener más información sobre la alta disponibilidad, vea [Soluciones de alta disponibilidad](../../database-engine/sql-server-business-continuity-dr.md).
   
## <a name="back-up-your-sql-server-data"></a>Realizar una copia de seguridad de los datos de SQL Server  
  
Para hacer una copia de seguridad de las bases de datos de SQL Server habilitadas para Stretch, puede seguir recurriendo a los mismos métodos de copia de seguridad de SQL Server que hasta ahora. Para obtener más información, consulte [Realizar copias de seguridad y restaurar bases de datos de SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).
  
 Las copias de seguridad de una base de datos de SQL Server habilitada para Stretch solo contienen datos locales y los datos válidos para la migración en el momento en que se hizo la copia de seguridad. (Por datos válidos se entiende aquellos datos que aún no se han migrado a Azure, pero lo harán, de acuerdo con la configuración de migración de las tablas). Esto se conoce como copia de seguridad **superficial** y no incluye los datos ya migrados a Azure.  
  
## <a name="back-up-your-remote-azure-data"></a>Realizar copias de seguridad de los datos de Azure remotos   
  
Microsoft Azure realiza automáticamente una copia de seguridad de los datos remotos que Stretch Database ha migrado de SQL Server a Azure.    
### <a name="azure-reduces-the-risk-of-data-loss-with-automatic-backup"></a>Azure reduce el riesgo de pérdida de datos con la copia de seguridad automática  
El servicio SQL Server Stretch Database en Azure protege las bases de datos remotos con instantáneas de almacenamiento automáticas al menos cada 8 horas. Conserva cada instantánea durante 7 días para proporcionarle una gama de posibles puntos de restauración.  
  
### <a name="azure-reduces-the-risk-of-data-loss-with-geo-redundancy"></a>Azure reduce el riesgo de pérdida de datos con la redundancia geográfica  
Las copias de seguridad de bases de datos de Azure se almacenan en Azure Storage con redundancia geográfica (RA-GRS) y, por tanto, tienen esta característica de forma predeterminada. El almacenamiento con redundancia geográfica replica los datos en una región secundaria que se encuentra a cientos de kilómetros de la región principal. En las regiones principal y secundarias, los datos se replican tres veces en cada una, en dominios de error y dominios de actualización independientes. Esto garantiza que los datos sean duraderos incluso si se produce un apagón regional completo o un desastre que haga que una de las regiones de Azure no esté disponible.

### <a name="stretch-database-reduces-the-risk-of-data-loss-for-your-azure-data-by-retaining-migrated-rows-temporarily"></a><a name="stretchRPO"></a>Stretch Database reduce el riesgo de pérdida de datos de Azure al conservar temporalmente las filas migradas
Después de migrar las filas válidas de SQL Server a Azure, Stretch Database conserva las filas de la tabla de almacenamiento provisional durante un mínimo de 8 horas. Si restaura una copia de seguridad de la base de datos de Azure, Stretch Database usará las filas guardadas en la tabla de almacenamiento provisional para conciliar las bases de datos de SQL Server y de Azure.

Después de restaurar una copia de seguridad de los datos de Azure, debe ejecutar el procedimiento almacenado [sys.sp_rda_reauthorize_db](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) para volver a conectar la base de datos de SQL Server con Stretch habilitado y la base de datos de Azure remota. Al ejecutar **sys.sp_rda_reauthorize_db**, Stretch Database concilia automáticamente las bases de datos de SQL Server y de Azure.

Para aumentar el número de horas de datos migrados que Stretch Database conserva temporalmente en la tabla de almacenamiento provisional, ejecute el procedimiento almacenado [sys.sp_rda_set_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-set-rpo-duration-transact-sql.md) y especifique un número de horas superior a 8. Para decidir cuántos datos se conservan, tenga en cuenta los siguientes factores:
-   La frecuencia de las copias de seguridad automáticas de Azure (al menos cada 8 horas).
-   El tiempo necesario tras un problema para detectarlo y decidir restaurar una copia de seguridad.
-   La duración de la operación de restauración de Azure.

> [!NOTE]
> Si aumenta la cantidad de datos que Stretch Database conserva temporalmente en la tabla de almacenamiento provisional, aumenta la cantidad de espacio necesario en SQL Server.

Para comprobar el número de horas de datos que Stretch Database conserva temporalmente en la tabla de almacenamiento provisional, ejecute el procedimiento almacenado [sys.sp_rda_set_rpo_duration](../../relational-databases/system-stored-procedures/sys-sp-rda-get-rpo-duration-transact-sql.md).

## <a name="see-also"></a>Consulte también  
[Restauración de bases de datos habilitadas para Stretch](../../sql-server/stretch-database/restore-stretch-enabled-databases-stretch-database.md)  
 [Administración y solución de problemas de Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)   
   
  
  
