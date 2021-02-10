---
description: Carga de objetos de base de datos convertidos en SQL Server (MySQLToSQL)
title: Carga de objetos de base de datos convertidos en SQL Server (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ac993a6d-0283-4823-8793-6b217677dfa3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: c3945a348a40c9770334f00d119aa7e9564f8525
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042061"
---
# <a name="loading-converted-database-objects-into-sql-server-mysqltosql"></a>Carga de objetos de base de datos convertidos en SQL Server (MySQLToSQL)
Después de convertir las bases de datos de MySQL en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, puede cargar los objetos de base de datos resultantes en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Puede hacer que SSMA cree los objetos, o bien puede crear un script de los objetos y ejecutar los scripts usted mismo. Además, SSMA le permite actualizar los metadatos de destino con el contenido real de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database.  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Elección entre sincronización y scripts  
Si desea cargar los objetos de base de datos convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure sin modificaciones, puede hacer que SSMA cree directamente o vuelva a crear los objetos de base de datos. Ese método es rápido y sencillo, pero no permite la personalización del código de Transact-SQL que define los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos o SQL Azure.  
  
Si desea modificar el objeto de Transact-SQL que se usa para crear objetos, o si desea tener más control sobre la creación de objetos, utilice SSMA para crear scripts. Después, puede modificar estos scripts, crear cada objeto individualmente e incluso usar Agente SQL Server para programar la creación de estos objetos.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Usar SSMA para sincronizar objetos con SQL Server  
Para usar SSMA para crear SQL Server o Azure SQL Database objetos, seleccione los objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure el explorador de metadatos y, a continuación, sincronice los objetos con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, tal como se muestra en el procedimiento siguiente. De forma predeterminada, si los objetos ya existen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, y si los metadatos de SSMA tienen algún cambio local o actualizaciones de la definición de esos objetos muy, SSMA modificará las definiciones de objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Puede cambiar el comportamiento predeterminado editando la **configuración del proyecto**.  
  
> [!NOTE]  
> Puede seleccionar objetos existentes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database que no se hayan convertido de bases de datos MySQL. No obstante, SSMA no volverá a crear ni modificar estos objetos.  
  
##### <a name="to-synchronize-objects-with-sql-server-or-sql-azure"></a>Para sincronizar objetos con SQL Server o SQL Azure  
  
1.  En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure explorador de metadatos, expanda el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nodo superior o SQL Azure y, a continuación, expanda **bases** de datos.  
  
2.  Seleccionar los objetos que se van a procesar:  
  
    -   Para sincronizar una base de datos completa, active la casilla situada junto al nombre de la base de datos.  
  
    -   Para sincronizar u omitir objetos o categorías de objetos individuales, Active o desactive la casilla situada junto al objeto o la carpeta.  
  
3.  Una vez que haya seleccionado los objetos que desea procesar en SQL Server o SQL Azure el explorador de metadatos, haga clic con el botón secundario en **bases** de datos y, a continuación, haga clic en **sincronizar con base** de datos.  
  
    También puede sincronizar objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en su carpeta primaria y, a continuación, haga clic en **sincronizar con base de datos**.  
  
    Después, SSMA mostrará el cuadro de diálogo **sincronizar con base de datos** , donde podrá ver dos grupos de elementos. En el lado izquierdo, SSMA muestra los objetos de base de datos seleccionados representados en un árbol. En el lado derecho, puede ver un árbol que representa los mismos objetos en los metadatos de SSMA. Para expandir el árbol, haga clic en el botón de la derecha o la izquierda ' + '. La dirección de la sincronización se muestra en la columna Acción situada entre los dos árboles.  
  
    Un signo de acción puede estar en los tres Estados siguientes:  
  
    -   Una flecha izquierda significa que el contenido de los metadatos se guardará en la base de datos (valor predeterminado).  
  
    -   Una flecha A la derecha significa que el contenido de la base de datos sobrescribirá los metadatos de SSMA.  
  
    -   Un signo cruzado significa que no se realizará ninguna acción.  
  
    -   Haga clic en el signo de acción para cambiar el estado. La sincronización real se realizará al hacer clic en el botón **Aceptar** del cuadro de diálogo **sincronizar con base de datos** .  
  
## <a name="scripting-objects"></a>Objetos de scripting  
Para guardar [!INCLUDE[tsql](../../includes/tsql-md.md)] definiciones de los objetos de base de datos convertidos, o para modificar las definiciones de objeto y ejecutar scripts, puede guardar las definiciones de objetos de base de datos convertidas en [!INCLUDE[tsql](../../includes/tsql-md.md)] scripts.  
  
**Para guardar objetos como scripts**  
  
1.  Una vez que haya seleccionado los objetos que desea guardar en un script, haga clic con el botón secundario en **bases de datos** y, a continuación, haga clic en **Guardar como script**.  
  
    También puede incluir en el script objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en su carpeta principal y, a continuación, haga clic en **Guardar como script**.  
  
2.  En el cuadro de diálogo **Guardar como** , busque la carpeta en la que desea guardar el script, escriba un nombre de archivo en el cuadro **nombre de archivo** y, a continuación, [!INCLUDE[clickOK](../../includes/clickok-md.md)] SSMA anexará la extensión de nombre de archivo. SQL.  
  
### <a name="modifying-scripts"></a>Modificar scripts  
Una vez guardadas las definiciones de objetos de SQL Server o SQL Azure como un script, puede usar SQL Server Management Studio para modificar el script.  
  
**Para modificar un script**  
  
1.  En el menú Management Studio **archivo** , seleccione **abrir** y, a continuación, haga clic en **archivo**.  
  
2.  En el cuadro de diálogo abrir, busque y seleccione el archivo de script y, a continuación, haga clic en **Aceptar**.  
  
3.  Edite el archivo de script mediante el editor de consultas. Para obtener más información sobre el editor de consultas, vea "comandos y características de la comodidad del editor" en Libros en pantalla de SQL Server.  
  
4.  Para guardar el script, en el menú Archivo, seleccione **Guardar**.  
  
### <a name="running-scripts"></a>Ejecutar scripts  
Puede ejecutar un script, o instrucciones individuales, en SQL Server Management Studio.  
  
**Para ejecutar un script**  
  
1.  En el menú SQL Server Management Studio **archivo** , seleccione **abrir** y, a continuación, haga clic en **archivo**.  
  
2.  En el cuadro de diálogo abrir, busque y seleccione el archivo de script y, a continuación, haga clic en **Aceptar**.  
  
3.  Para ejecutar el script completo, presione la tecla **F5** .  
  
4.  Para ejecutar un conjunto de instrucciones, seleccione las instrucciones en la ventana del editor de consultas y, a continuación, presione la tecla **F5** .  
  
5.  Para obtener más información sobre cómo usar el editor de consultas de para ejecutar scripts, vea "SQL Server Management Studio consulta de Transact-SQL" en Libros en pantalla de SQL Server.  
  
6.  También puede ejecutar scripts desde la línea de comandos mediante la utilidad **sqlcmd** y desde Agente SQL Server. Para obtener más información sobre **sqlcmd**, vea "utilidad SQLCMD" en libros en pantalla de SQL Server. Para obtener más información acerca de Agente SQL Server, vea "automatizar las tareas administrativas (Agente SQL Server)" en Libros en pantalla de SQL Server.  
  
## <a name="securing-objects-in-sql-server"></a>Proteger objetos en SQL Server  
Una vez que haya cargado los objetos de base de datos convertidos en SQL Server, puede conceder y denegar permisos en estos objetos. Es una buena idea hacerlo antes de migrar los datos a SQL Server. Para obtener información sobre cómo proteger objetos en SQL Server, vea "consideraciones de seguridad para bases de datos y aplicaciones de base de datos" en Libros en pantalla de SQL Server.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [migrar los datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
## <a name="see-also"></a>Consulte también  
[Migración de bases de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
