---
title: Creación de reflejo e instantáneas de base de datos
description: Obtenga información sobre la interoperabilidad del uso de instantáneas de base de datos con la creación de reflejo de la base de datos para descargar informes en SQL Server.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- snapshots [SQL Server database snapshots], database mirroring
- database snapshots [SQL Server], database mirroring
ms.assetid: 0bf1be90-7ce4-484c-aaa7-f8a782f57c5f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 28bc035d068f3dd857caca28f7f7187440b08daa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353202"
---
# <a name="database-mirroring-and-database-snapshots-sql-server"></a>Reflejo e instantáneas de base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Puede aprovechar una base de datos reflejada de la cual se está realizando el mantenimiento con fines de disponibilidad para descargar informes. A fin de utilizar una base de datos reflejada para informes, puede crear una instantánea de base de datos en la base de datos reflejada y dirigir las solicitudes de conexión de cliente a la instantánea más reciente. Una instantánea de base de datos es una instantánea estática coherente con las transacciones y de solo lectura de la base de datos de origen tal como existía en el momento de la creación de la instantánea. Para crear la instantánea de una base de datos en una base de datos reflejada, la base de datos debe hallarse en estado de reflejo sincronizado.  
  
 A diferencia de la base de datos reflejada, los clientes pueden obtener acceso a las instantáneas de base de datos. Mientras el servidor reflejado se comunique con el servidor principal, podrá dirigir clientes de informe para que se conecten a una instantánea. Observe que, puesto que las instantáneas de base de datos son estáticas, no hay nuevos datos disponibles. Para hacer que los datos relativamente recientes estén disponibles para los usuarios, deberá crear una nueva instantánea de base de datos de forma periódica y hacer que las aplicaciones dirijan las conexiones entrantes de cliente a la instantánea más reciente.  
  
 Una nueva instantánea de base de datos está prácticamente vacía, pero crece con el tiempo a medida que se actualizan por primera vez más y más páginas de la base de datos. Debido a que las instantáneas de base de datos crecen incrementalmente de esta forma, cada instantánea consume tantos recursos como una base de datos normal. En función de las configuraciones del servidor principal y el servidor reflejado, un número excesivo de instantáneas de base de datos en una base de datos reflejada puede reducir el rendimiento de la base de datos principal. Por lo tanto, se recomienda mantener solo algunas instantáneas relativamente recientes en las bases de datos reflejadas. Por lo general, tras la creación de una nueva instantánea, deberá redirigir las consultas entrantes a esta nueva instantánea y quitar la instantánea anterior una vez finalizadas las consultas en curso.  
  
> [!NOTE]  
>  Para obtener más información sobre las instantáneas de base de datos, vea [Instantáneas de bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md).  
  
 Si se produce la conmutación de roles, se reinicia la base de datos junto con sus instantáneas y se desconectan los usuarios de forma temporal. Posteriormente, las instantáneas de base de datos permanecen en la instancia de servidor en la que se crearon (que es ahora la nueva base de datos principal). Los usuarios podrán seguir utilizando estas instantáneas tras la conmutación por error. No obstante, esto aumenta la carga del nuevo servidor principal. Si el rendimiento es importante en el entorno, se recomienda crear una instantánea en la nueva base de datos reflejada cuando esté disponible, redirigir los clientes a la nueva instantánea y quitar todas las instantáneas de base de datos reflejada anterior.  
  
> [!NOTE]  
>  Para una solución de informes dedicada que admita la ampliación horizontal, considere la posibilidad de utilizar la duplicación. Para más información, consulte [SQL Server Replication](../../relational-databases/replication/sql-server-replication.md).  
  
## <a name="example"></a>Ejemplo  
 En este ejemplo se crean instantáneas en una base de datos reflejada.  
  
 Suponga que la base de datos de una sesión de creación de reflejo de la base de datos es [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. En este ejemplo se crean tres instantáneas de base de datos en la copia reflejada de la base de datos `AdventureWorks` , que reside en la unidad `F` . Las instantáneas se denominan `AdventureWorks_0600`, `AdventureWorks_1200`y `AdventureWorks_1800` para indicar su hora de creación aproximada.  
  
1.  Cree la primera instantánea de base de datos en el reflejo de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
    ```  
    CREATE DATABASE AdventureWorks_0600  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_0600.SNP')  
       AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
2.  Cree la segunda instantánea de base de datos en el reflejo de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Los usuarios que aún utilicen `AdventureWorks_0600` podrán seguir utilizándola.  
  
    ```  
    CREATE DATABASE AdventureWorks_1200  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_1200.SNP')  
       AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
     En este momento, se podrán dirigir mediante programación las nuevas conexiones de cliente a la instantánea más reciente.  
  
3.  Cree la tercera instantánea en el reflejo de [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Los usuarios que aún utilicen `AdventureWorks_0600` o `AdventureWorks_1200` podrán seguir utilizándolas.  
  
    ```  
    CREATE DATABASE AdventureWorks_1800  
    ON (NAME = 'datafile', FILENAME = 'F:\AdventureWorks_1800.SNP')  
        AS SNAPSHOT OF AdventureWorks2012  
    ```  
  
     En este momento, se podrán dirigir mediante programación las nuevas conexiones de cliente a la instantánea más reciente.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Crear una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/create-a-database-snapshot-transact-sql.md)  
  
-   [Ver una instantánea de base de datos &#40;SQL Server&#41;](../../relational-databases/databases/view-a-database-snapshot-sql-server.md)  
  
-   [Eliminar una instantánea de base de datos &#40;Transact-SQL&#41;](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md)  
  
  
## <a name="see-also"></a>Consulte también  
 [Instantáneas de bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-snapshots-sql-server.md)   
 [Conectar clientes a una sesión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)  
  
  
