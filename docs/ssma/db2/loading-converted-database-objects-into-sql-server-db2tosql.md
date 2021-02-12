---
description: Carga de objetos de base de datos convertidos en SQL Server (DB2ToSQL)
title: Carga de objetos de base de datos convertidos en SQL Server (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f4ea1ced-9f9f-4a9d-88ab-81dbab64adc3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 209b49e3719400b92a5425bdd680d8bed548e285
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072479"
---
# <a name="loading-converted-database-objects-into-sql-server-db2tosql"></a>Carga de objetos de base de datos convertidos en SQL Server (DB2ToSQL)
Después de convertir los esquemas DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede cargar los objetos de base de datos resultantes en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede hacer que SSMA cree los objetos, o bien puede crear un script de los objetos y ejecutar los scripts usted mismo. Además, SSMA le permite actualizar los metadatos de destino con el contenido real de la base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="choosing-between-synchronization-and-scripts"></a>Elección entre sincronización y scripts  
Si desea cargar los objetos de base de datos convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sin modificaciones, puede hacer que SSMA cree directamente o vuelva a crear los objetos de base de datos. Ese método es rápido y sencillo, pero no permite la personalización del [!INCLUDE[tsql](../../includes/tsql-md.md)] código que define los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos, excepto los procedimientos almacenados.  
  
Si desea modificar el objeto [!INCLUDE[tsql](../../includes/tsql-md.md)] que se usa para crear objetos, o si desea tener más control sobre la creación de objetos, utilice SSMA para crear scripts. Después, puede modificar esos scripts, crear cada objeto individualmente e incluso usar el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente para programar la creación de esos objetos.  
  
## <a name="using-ssma-to-synchronize-objects-with-sql-server"></a>Usar SSMA para sincronizar objetos con SQL Server  
Para usar SSMA para crear [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos de base de datos, seleccione los objetos en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos y, a continuación, sincronice los objetos con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tal y como se muestra en el procedimiento siguiente. De forma predeterminada, si los objetos ya existen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y si los metadatos de SSMA son más recientes que el objeto en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA modificará las definiciones de objeto en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Puede cambiar el comportamiento predeterminado editando la **configuración del proyecto**.  
  
> [!NOTE]  
> Puede seleccionar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objetos de base de datos existentes que no se hayan convertido de bases de datos DB2. No obstante, SSMA no volverá a crear ni modificar esos objetos.  
  
**Para sincronizar objetos con SQL Server**  
  
1.  En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] el explorador de metadatos, expanda el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nodo superior y, a continuación, expanda **bases** de datos.  
  
2.  Seleccionar los objetos que se van a procesar:  
  
    -   Para sincronizar una base de datos completa, active la casilla situada junto al nombre de la base de datos.  
  
    -   Para sincronizar u omitir objetos o categorías de objetos individuales, Active o desactive la casilla situada junto al objeto o la carpeta.  
  
3.  Una vez que haya seleccionado los objetos que desea procesar en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorador de metadatos, haga clic con el botón secundario en **bases** de datos y, a continuación, haga clic en **sincronizar con base** de datos.  
  
    También puede sincronizar objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en su carpeta primaria y, a continuación, haga clic en  **sincronizar con base de datos**.  
  
    Después, SSMA mostrará el cuadro de diálogo **sincronizar con base de datos** , donde podrá ver dos grupos de elementos. En el lado izquierdo, SSMA muestra los objetos de base de datos seleccionados representados en un árbol. En el lado derecho, puede ver un árbol que representa los mismos objetos en los metadatos de SSMA. Para expandir el árbol, haga clic en el botón de la derecha o la izquierda ' + '. La dirección de la sincronización se muestra en la columna Acción situada entre los dos árboles.  
  
    Un signo de acción puede tener tres Estados:  
  
    -   Una flecha izquierda significa que el contenido de los metadatos se guardará en la base de datos (valor predeterminado).  
  
    -   Una flecha A la derecha significa que el contenido de la base de datos sobrescribirá los metadatos de SSMA.  
  
    -   Un signo cruzado significa que no se realizará ninguna acción.  
  
Haga clic en el signo de acción para cambiar el estado. La sincronización real se realizará al hacer clic en el botón **Aceptar** del cuadro de diálogo **sincronizar con base de datos** .  
  
## <a name="scripting-objects"></a>Objetos de scripting  
Para guardar [!INCLUDE[tsql](../../includes/tsql-md.md)] definiciones de los objetos de base de datos convertidos, o para modificar las definiciones de objeto y ejecutar scripts, puede guardar las definiciones de objetos de base de datos convertidas en [!INCLUDE[tsql](../../includes/tsql-md.md)] scripts.  
  
**Para guardar objetos como scripts**  
  
1.  Una vez que haya seleccionado los objetos que desea guardar en un script, haga clic con el botón secundario en **bases de datos** y, a continuación, haga clic en **Guardar como script**.  
  
    También puede incluir en el script objetos individuales o categorías de objetos; para ello, haga clic con el botón secundario en el objeto o en su carpeta principal y, a continuación, haga clic en **Guardar como script**.  
  
2.  En el cuadro de diálogo **Guardar como** , busque la carpeta en la que desea guardar el script, escriba un nombre de archivo en el cuadro **nombre de archivo** y, a continuación, [!INCLUDE[clickOK](../../includes/clickok-md.md)] . SSMA anexará la extensión de nombre de archivo. SQL.  
  
### <a name="modifying-scripts"></a>Modificar scripts  
Una vez guardadas las [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] definiciones de objeto como uno o varios scripts, puede usar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para ver y modificar los scripts.  
  
**Para modificar un script**  
  
1.  En el menú [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Archivo** , seleccione **Abrir** y haga clic en **Archivo**.  
  
2.  En el cuadro de diálogo **abrir** , seleccione el archivo de script y, a continuación, haga clic en Aceptar.
  
3.  Edite el archivo de script mediante el editor de consultas.  
  
    Para obtener más información acerca del editor de consultas, vea el tema sobre los comandos y las características de la comodidad del editor en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
4.  Para guardar el script, haga clic en **Guardar** en el menú archivo.  
  
### <a name="running-scripts"></a>Ejecutar scripts  
Puede ejecutar un script, o instrucciones individuales, en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
**Para ejecutar un script**  
  
1.  En el menú [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **Archivo** , seleccione **Abrir** y haga clic en **Archivo**.  
  
2.  En el cuadro de diálogo **abrir** , seleccione el archivo de script y, a continuación, [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Para ejecutar el script completo, presione la tecla **F5** .  
  
4.  Para ejecutar un conjunto de instrucciones, seleccione las instrucciones en la ventana del editor de consultas y, a continuación, presione la tecla **F5** .  
  
Para obtener más información sobre cómo usar el editor de consultas de para ejecutar scripts, vea " [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] [!INCLUDE[tsql](../../includes/tsql-md.md)] consulta" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
También puede ejecutar scripts desde la línea de comandos mediante la utilidad **sqlcmd** y desde el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente. Para obtener más información sobre **sqlcmd**, vea "utilidad SQLCMD" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de. Para obtener más información acerca del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente, vea "automatizar tareas administrativas ( [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente)" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
## <a name="securing-objects-in-sql-server"></a>Proteger objetos en SQL Server  
Una vez cargados los objetos de base de datos convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede conceder y denegar permisos en esos objetos. Es una buena idea hacerlo antes de migrar los datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener información sobre cómo proteger objetos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea "consideraciones de seguridad para bases de datos y aplicaciones de base de datos" en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
## <a name="next-step"></a>siguiente paso  
El siguiente paso del proceso de migración consiste en [migrar los datos de DB2 a SQL Server](./migrating-db2-data-into-sql-server-db2tosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migración de datos de DB2 a SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
