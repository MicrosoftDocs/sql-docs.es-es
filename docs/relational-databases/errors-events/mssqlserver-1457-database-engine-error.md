---
description: MSSQLSERVER_1457
title: MSSQLSERVER_1457 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1457 (Database Engine error)
ms.assetid: 28434ba1-b033-4866-ab41-111fccef45a2
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 183dc6f35122d695653ca66cf9b7f78f4187d124
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196958"
---
# <a name="mssqlserver_1457"></a>MSSQLSERVER_1457
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|1457|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|DBM_PAGE_UNDO_PENDING|  
|Texto del mensaje|Se interrumpió la sincronización de la base de datos reflejada, '%.*ls' y ésta quedó en un estado incoherente. Error del comando ALTER DATABASE. Asegúrese de que hay una copia de seguridad la base de datos reflejada y de que la base de datos está en línea, y vuelva a conectar la instancia del servidor reflejado y permita que la base de datos reflejada finalice la sincronización.|  
  
## <a name="explanation"></a>Explicación  
Este mensaje indica un error en la instrucción ALTER DATABASE *nombreDeBaseDeDatos* SET PARTNER OFF. El error al intentar quitar el reflejo de la base de datos ha interrumpido la sincronización de la base de datos reflejada. La base de datos se encuentra en un estado incoherente.  
  
## <a name="user-action"></a>Acción del usuario  
Para solucionar este error, puede realizar una de las siguientes acciones:  
  
-   Restaure el contacto entre el servidor reflejado y el principal para permitir la sincronización de la base de datos reflejada.  
  
-   Quite la base de datos reflejada.  
  
    Después de quitar la base de datos reflejada, puede crear una nueva base de datos reflejada a partir de las copias de seguridad.  
  
    > [!NOTE]  
    > Puede quitar la base de datos reflejada cuando está todavía habilitada la creación de reflejo solo después de un error de instrucción SET PARTNER OFF.  
  
## <a name="see-also"></a>Consulte también  
[Creación de reflejo de la base de datos &#40;SQL Server&#41;](~/database-engine/database-mirroring/database-mirroring-sql-server.md)  
[ALTER DATABASE &#40;Transact-SQL&#41;](~/t-sql/statements/alter-database-transact-sql-set-options.md)  
[Configurar la creación de reflejo de la base de datos &#40;SQL Server&#41;](~/database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md)  
[Quitar la creación de reflejo de la base de datos &#40;SQL Server&#41;](~/database-engine/database-mirroring/removing-database-mirroring-sql-server.md)  
[Preparar una base de datos reflejada para la creación de reflejo &#40;SQL Server&#41;](~/database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
  
