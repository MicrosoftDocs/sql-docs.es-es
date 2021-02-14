---
description: Carga de objetos de base de datos convertidos en SQL Server (AccessToSQL)
title: Carga de objetos de base de datos convertidos en SQL Server (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, loading converted objects into SQL Azure
- Access databases, loading converted objects into SQL Server
- data, securing
- loading objects into SQL Azure
- loading objects into SQL Server
- migrating objects into SQL Azure
- migrating objects into SQL Server
- moving objects into SQL Azure
- moving objects into SQL Server
- schemas, loading into SQL Azure
- schemas, loading into SQL Server
- scripting converted objects
- securing data
- SQL Azure, loading objects into
- SQL Server, loading objects into
- synchronizing metadata with SQL Azure
- synchronizing metadata with SQL Server
- uploading objects into SQL Azure
- uploading objects into SQL Server
ms.assetid: 4e854eee-b10c-4f0b-9d9e-d92416e6f2ba
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4fb81a05cebfaa1a2c59f7e2b90d8289412e140a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071142"
---
# <a name="loading-converted-database-objects-into-sql-server-accesstosql"></a>Carga de objetos de base de datos convertidos en SQL Server (AccessToSQL)
Después de haber convertido los objetos de base de datos de Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, puede cargar los objetos de base de datos resultantes en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Puede hacer que SSMA cree los objetos, o bien puede crear un script de los objetos y ejecutar los scripts usted mismo. Además, SSMA le permite actualizar los metadatos de destino con el contenido real de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Elección entre sincronización y scripts  
Si desea cargar los objetos de base de datos convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure sin modificaciones, puede hacer que SSMA cree directamente o vuelva a crear los objetos de base de datos. Ese método es rápido y sencillo, pero no permite la personalización del [!INCLUDE[tsql](../../includes/tsql-md.md)] código que define los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos o SQL Azure, excepto los procedimientos almacenados.  
  
Si desea modificar el objeto [!INCLUDE[tsql](../../includes/tsql-md.md)] que se usa para crear objetos, o si desea tener más control sobre la creación de objetos, utilice SSMA para crear scripts. Después, puede modificar esos scripts, crear cada objeto individualmente e incluso usar el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente para programar la creación de esos objetos.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Usar SSMA para sincronizar objetos con SQL Server  
Para usar SSMA para crear [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database objetos, seleccione los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos y, a continuación, sincronice los objetos con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, tal como se muestra en el procedimiento siguiente. De forma predeterminada, si los objetos ya existen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, y si los metadatos de SSMA tienen algún cambio local o actualizaciones de la definición de esos objetos muy, SSMA modificará las definiciones de objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Puede cambiar el comportamiento predeterminado editando la **configuración del proyecto**.  
  
> [!NOTE]  
> Puede seleccionar objetos existentes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database que no se hayan convertido a partir de bases de datos de Access. Sin embargo, SSMA no volverá a crear o modificar los objetos.  
  
**Para sincronizar objetos con SQL Server o SQL Azure**  
  
1.  En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos, expanda el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nodo superior o SQL Azure y, a continuación, expanda **bases** de datos.  
  
2.  Seleccionar los objetos que se van a procesar:  
  
    -   Para sincronizar una base de datos completa, active la casilla situada junto al nombre de la base de datos.  
  
    -   Para sincronizar u omitir objetos o categorías de objetos individuales, Active o desactive la casilla situada junto al objeto o la carpeta.  
  
3.  Una vez que haya seleccionado los objetos que desea procesar en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos o SQL Azure, haga clic con el botón secundario en **bases** de datos y, a continuación, haga clic en **sincronizar con base** de datos.  
  
    También puede sincronizar objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en su carpeta primaria y, a continuación, haga clic en **sincronizar con base de datos**.  
  
    Después, SSMA mostrará el cuadro de diálogo **sincronizar con base de datos** , donde podrá ver dos grupos de elementos. En el lado izquierdo, SSMA muestra los objetos de base de datos seleccionados representados en un árbol. En el lado derecho, puede ver un árbol que representa los mismos objetos en los metadatos de SSMA. Para expandir el árbol, haga clic en el botón de la derecha o la izquierda ' + '. La dirección de la sincronización se muestra en la columna Acción situada entre los dos árboles.  
  
    Un signo de acción puede tener tres Estados:  
  
    -   Una flecha izquierda significa que el contenido de los metadatos se guardará en la base de datos (valor predeterminado).  
  
    -   Una flecha A la derecha significa que el contenido de la base de datos sobrescribirá los metadatos de SSMA.  
  
    -   Un signo cruzado significa que no se realizará ninguna acción.  
  
    Haga clic en el signo de acción para cambiar el estado. La sincronización real se realizará al hacer clic en el botón **Aceptar** del cuadro de diálogo **sincronizar con base de datos** .  
  
## <a name="scripting-objects"></a>Objetos de scripting  
Si desea guardar [!INCLUDE[tsql](../../includes/tsql-md.md)] definiciones de los objetos de base de datos convertidos, o si desea modificar las definiciones de objeto y ejecutar scripts usted mismo, puede guardar las definiciones de objetos de base de datos convertidas en [!INCLUDE[tsql](../../includes/tsql-md.md)] scripts.  
  
**Para guardar uno o más objetos en un script**  
  
1.  En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el explorador de metadatos, expanda el nodo superior (el nombre del servidor) y, a continuación, expanda **bases** de datos.  
  
2.  Realice uno o varios de los procedimientos siguientes:  
  
    -   Para incluir en el script una base de datos completa, active la casilla situada junto al nombre de la base de datos.  
  
    -   Para generar un script u omitir vistas individuales, expanda la base de datos, expanda **vistas** y, a continuación, Active o desactive la casilla situada junto a la vista.  
  
    -   Para generar un script u omitir tablas individuales, expanda la base de datos, expanda **tablas** y, a continuación, Active o desactive la casilla situada junto a la tabla.  
  
    -   Para generar un script u omitir los índices individuales de una tabla, expanda la tabla, expanda **índices** y, a continuación, Active o desactive el índice.  
  
3.  Haga clic con el botón derecho en **bases de datos** y seleccione **Guardar como script**.  
  
    También puede incluir en script objetos individuales. Para generar un script de un objeto, independientemente de qué objetos estén seleccionados, haga clic con el botón derecho en el objeto y seleccione **Guardar como script**.  
  
4.  En el cuadro de diálogo **Guardar como** , busque la carpeta en la que desea guardar el script, escriba un nombre de archivo en el cuadro **nombre de archivo** y, a continuación, haga clic en **Aceptar**.  
  
    SSMA anexará la extensión de nombre de archivo. SQL.  
  
### <a name="modifying-scripts"></a>Modificar scripts  
Una vez guardadas las [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiciones de objeto o SQL Azure como un script, puede usar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para modificar el script.  
  
**Para modificar un script**  
  
1.  En el menú [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] **Archivo** , seleccione **Abrir** y haga clic en **Archivo**.  
  
2.  En el cuadro de diálogo **abrir** , busque y seleccione el archivo de script y, a continuación, haga clic en **Aceptar**.  
  
3.  Edite el archivo de script mediante el editor de consultas.  
  
    Para obtener más información acerca del editor de consultas, vea el tema sobre los comandos y las características de la comodidad del editor en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
4.  Para guardar el script, en el menú Archivo, seleccione **Guardar**.  
  
### <a name="running-scripts"></a>Ejecutar scripts  
Puede ejecutar un script, o instrucciones individuales, en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
**Para ejecutar un script**  
  
1.  En el [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] menú **archivo** , seleccione **abrir** y, a continuación, haga clic en **archivo**.  
  
2.  En el cuadro de diálogo **abrir** , busque y seleccione el archivo de script y, a continuación, haga clic en **Aceptar**.  
  
3.  Para ejecutar el script completo, presione la tecla **F5** .  
  
4.  Para ejecutar un conjunto de instrucciones, seleccione las instrucciones en la ventana del editor de consultas y, a continuación, presione la tecla **F5** .  
  
Para obtener más información sobre cómo usar el editor de consultas de para ejecutar scripts, vea " [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] consulta" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
También puede ejecutar scripts desde la línea de comandos mediante la utilidad **sqlcmd** y desde el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente. Para obtener más información sobre **sqlcmd**, vea "utilidad SQLCMD" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de. Para obtener más información acerca del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente, vea "automatizar tareas administrativas ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente)" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
## <a name="securing-objects-in-sql-server"></a>Proteger objetos en SQL Server  
Una vez cargados los objetos de base de datos convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede conceder y denegar permisos en esos objetos. Es una buena idea hacerlo antes de migrar los datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener información sobre cómo proteger objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea "consideraciones de seguridad para bases de datos y aplicaciones de base de datos" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [migrar datos a SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
