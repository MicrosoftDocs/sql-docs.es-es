---
title: Creación de reflejo de la base de datos y catálogos de texto completo
description: Obtenga información sobre cómo configurar un reflejo de base de datos en una base de datos que tenga un catálogo de texto completo y sobre los índices antes y después de la conmutación por error.
ms.custom: seo-lt-2019
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- full-text catalogs [SQL Server], database mirroring
- catalogs [SQL Server], database mirroring
ms.assetid: e34072ae-fe8a-462d-bb03-02fa0987f793
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 67676ff897203f4ae1fea1cbd713453f0188cc66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353222"
---
# <a name="database-mirroring-and-full-text-catalogs-sql-server"></a>Creación de reflejo de la base de datos y catálogos de texto completo (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Para crear un reflejo de base de datos que tenga un catálogo de texto completo, utilice una copia de seguridad de la forma habitual para crear una copia de seguridad completa de base de datos de la base de datos principal y, a continuación, restaure la copia de seguridad para copiar la base de datos al servidor reflejado. Para obtener más información, vea [Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
## <a name="full-text-catalog-and-indexes-before-failover"></a>Catálogo de texto completo e índices antes de la conmutación por error  
 En la base de datos reflejada recién creada, el catálogo de texto completo es el mismo que cuando se creó la copia de seguridad de la base de datos. Después de que empiece la creación de reflejo de la base de datos, los cambios en el catálogo realizados por instrucciones de DDL (CREATE FULLTEXT CATALOG, ALTER FULLTEXT CATALOG, DROP FULLTEXT CATALOG) se registran y se envían al servidor reflejado para que se vuelvan a reproducir en la base de datos reflejada. Sin embargo, los cambios en el índice no se reproducen en la base de datos reflejada porque no está registrada en el servidor principal. Por tanto, puesto que el contenido del catálogo de texto completo cambia en la base de datos principal, el contenido del catálogo de texto completo de la base de datos reflejada no está sincronizado.  
  
## <a name="full-text-indexes-after-failover"></a>Índices de texto completo después de la conmutación por error  
 Después de una conmutación por error, es posible que se necesite o que resulte útil un rastreo completo de un índice de texto completo en el nuevo servidor principal en las siguientes situaciones:  
  
-   Si el seguimiento de cambios está DESACTIVADO en un índice de texto completo, debe iniciar un rastreo completo en ese índice con la siguiente instrucción:  
  
     ALTER FULLTEXT INDEX ON *table_name* START FULL POPULATION  
  
-   Si un índice de texto completo está configurado para seguimiento de cambios automático, el índice de texto completo se sincronizará automáticamente. Sin embargo, la sincronización reduce el rendimiento de texto completo en cierta medida. Si el rendimiento es demasiado lento, puede desencadenar un rastreo completo desactivando el seguimiento de cambios y restableciéndolo al modo automático.  
  
    -   Para desactivar el seguimiento de cambios:  
  
         ALTER FULLTEXT INDEX ON *table_name* SET CHANGE_TRACKING OFF  
  
    -   Para establecer el seguimiento de cambios automático al modo automático:  
  
         ALTER FULLTEXT INDEX ON *table_name* SET CHANGE_TRACKING AUTO  
  
    > [!NOTE]  
    >  Para ver si el seguimiento de cambios automático está activado, puede usar la función [OBJECTPROPERTYEX](../../t-sql/functions/objectpropertyex-transact-sql.md) para consultar la propiedad **TableFullTextBackgroundUpdateIndexOn** de la tabla.  
  
 Para obtener más información, vea [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md).  
  
> [!NOTE]  
>  El inicio de un rastreo después de una conmutación por error funciona igual que el inicio de un rastreo después de una operación de restauración.  
  
## <a name="after-forcing-service"></a>Después de forzar el servicio  
 Después de forzar el servicio al servidor reflejado (con posible pérdida de datos), inicie un rastreo completo. El método que debe usar para iniciar un rastreo completo depende de si el índice de texto completo tiene seguimiento de cambios. Para obtener más información, vea "Índices de texto completo después de la conmutación por error" anteriormente en este tema.  
  
## <a name="see-also"></a>Consulte también  
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Realizar copias de seguridad de los catálogos e índices de texto completo y restaurarlos](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)  
  
  
