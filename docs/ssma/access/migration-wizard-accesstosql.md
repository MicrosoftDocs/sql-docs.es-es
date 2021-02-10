---
description: Asistente para migración (AccessToSQL)
title: Asistente para migración (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Migration Wizard dialog box
- Migration Wizard, adding Access databases
- Migration Wizard, Connect to SQL Azure
- Migration Wizard, Connect to SQL Server
- Migration Wizard, Link Tables
- Migration Wizard, Migration status
- Migration Wizard, New Project
- Migration Wizard, Selecting objects to migrate
ms.assetid: 5bab5914-b2ae-4795-8cf5-83e42d64bef2
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f5f97b55b4d357c2caa2314c993806c61f51e07
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066680"
---
# <a name="migration-wizard-accesstosql"></a>Asistente para migración (AccessToSQL)
El Asistente para migración le guía a través de la migración de una o varias bases de datos de acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Mediante el asistente, se crea un proyecto, se agregan bases de datos al proyecto, se seleccionan los objetos que se van a migrar y se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. También convertirá, cargará y migrará los esquemas y los datos de Access. Opcionalmente, puede vincular tablas de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tablas de o SQL Azure.  
  
La mayoría de las páginas del Asistente para migración contienen las mismas opciones que los cuadros de diálogo de SSMA existentes. Por lo tanto, las páginas del asistente se describen aquí y, a continuación, se proporcionan vínculos para que pueda obtener más información sobre las opciones individuales. Si una página contiene opciones únicas, se documentan aquí.  
  
## <a name="starting-the-migration-wizard"></a>Inicio del Asistente para migración  
De forma predeterminada, el Asistente para migración aparece cuando se inicia SSMA. También puede iniciar el asistente en el menú **archivo** seleccionando **Asistente para migración**.  
  
## <a name="welcome-page"></a>Página de bienvenida  
La página de bienvenida presenta el Asistente para migración y proporciona la siguiente opción para iniciar el asistente.  
  
**Inicie este asistente en el inicio.**  
De forma predeterminada, SSMA iniciará el Asistente para migración al iniciar SSMA. Para evitar el inicio automático del asistente, desactive esta casilla.  
  
## <a name="create-new-project-page"></a>Página crear nuevo proyecto  
La página crear nuevo proyecto es donde se escribe el nombre de archivo del proyecto, la ubicación y el tipo de proyecto de migración (la versión de destino SQL Server usar para la migración). Para obtener más información, vea [nuevo proyecto (SSMA)](./new-project-ssma-accesstosql.md) .  
  
## <a name="add-access-databases-page"></a>Página agregar bases de datos de Access  
En la página Agregar bases de datos de Access se agregan una o varias bases de datos de Access al proyecto. Para agregar bases de datos individuales, haga clic en **Agregar bases de datos** y, a continuación, seleccione las bases de datos en la ventana **abrir** . O bien, puede buscar las bases de datos mediante el botón **Buscar bases de datos** . Para obtener más información, vea los temas siguientes:  
  
-   [Agregar y quitar archivos de base de datos de Access](adding-and-removing-access-database-files-accesstosql.md)  
  
-   [Asistente Buscar bases de datos (Seleccionar ubicaciones)](./find-databases-wizard-select-locations-accesstosql.md)  
  
-   [Asistente Buscar bases de datos (Seleccionar archivos)](./find-databases-wizard-select-files-accesstosql.md)  
  
-   [Asistente Buscar bases de datos (Comprobar selección)](./find-databases-wizard-verify-selection-accesstosql.md)  
  
## <a name="select-objects-to-migrate-page"></a>Página seleccionar objetos que se van a migrar  
En la página seleccionar objetos que se van a migrar, se seleccionan los objetos que se van a convertir. Puede seleccionar todos los objetos, grupos de objetos u objetos individuales.  
  
**Para seleccionar objetos**  
  
1.  Expanda **Access-metabase** y, a continuación, expanda **bases de datos**.  
  
2.  Realice uno o varios de los procedimientos siguientes:  
  
    -   Para convertir todas las bases de datos, active la casilla situada junto a **bases de datos**.  
  
    -   Para convertir u omitir las bases de datos individuales, Active o desactive la casilla situada junto al nombre de la base de datos.  
  
    -   Para convertir u omitir consultas, expanda la base de datos y, a continuación, Active o desactive la casilla **consultas** .  
  
    -   Para convertir u omitir tablas individuales, expanda la base de datos, expanda **tablas** y, a continuación, Active o desactive la casilla situada junto a la tabla.  
  
Si tiene muchos objetos, es posible que desee usar las opciones **avanzadas de selección de objetos** en el panel derecho para filtrar los objetos de base de datos de Access. Por ejemplo, si selecciona **tablas** en el panel izquierdo, puede filtrar la lista de tablas escribiendo cadenas en el cuadro de **filtro** . A continuación, puede activar o desactivar las tablas filtradas para la migración con los botones situados en la parte superior del panel.  
  
Para obtener más información sobre el filtrado, consulte la sección Opciones de [selección avanzada de objetos (SSMA Common)](../sybase/advanced-object-selection-sybasetosql.md).  
  
## <a name="connect-to-sql-server-page"></a>Página conectar con SQL Server  
En la página conectar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , especifique las propiedades de conexión y, a continuación, conéctese a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para obtener más información, consulte [conexión a SQL Server](connect-to-sql-server-accesstosql.md).
  
> [!IMPORTANT]  
> En cuanto la conexión se realiza correctamente, se encontrará en la página **vincular tablas** , donde tiene la opción de vincular las tablas. Haga clic en **siguiente** y se iniciará la migración.  
  
## <a name="connect-to-sql-azure-page"></a>Página conectar con SQL Azure  
En la página conectar a SQL Azure, especifique las propiedades de conexión y, a continuación, conéctese a SQL Azure. Para crear una nueva base de datos de Azure, puede hacerlo mediante la opción **crear base de datos de Azure** que aparece en el botón hacer clic en **examinar** . Para obtener más información, consulte [conexión a SQL Azure](connect-to-azure-sql-db-accesstosql.md)  
  
> [!IMPORTANT]  
> En cuanto la conexión se realiza correctamente, se encontrará en la página **vincular tablas** , donde tiene la opción de vincular las tablas. Haga clic en el botón **siguiente** en la página vínculos para iniciar la migración.  
  
## <a name="link-tables-page"></a>Página vincular tablas  
La página vincular tablas le permite vincular las tablas de Access originales a las tablas migradas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. La vinculación de tablas modifica la base de datos de Access de forma que las consultas, los formularios, los informes y las páginas de acceso a datos usan los datos de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database o en lugar de los datos de la base de datos de Access.  
  
**Vincular tablas**  
Active la casilla **vincular tablas** para vincular las tablas de Access a las tablas migradas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Para iniciar la migración, haga clic en el botón **siguiente** .  
  
## <a name="migration-status-page"></a>Página estado de la migración  
La página estado de la migración muestra el progreso de la conversión de los esquemas de acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure esquemas, la carga de los esquemas convertidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure y, a continuación, la migración de datos.  
  
Para obtener más información acerca de esta página, vea [convertir, cargar y migrar](./convert-load-and-migrate-accesstosql.md) .  
  
## <a name="see-also"></a>Consulte también  
[Introducción con SQL Server Migration Assistant para el acceso &#40;AccessToSQL&#41;](../../ssma/access/getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Referencia de la interfaz de usuario (acceso)](./user-interface-reference-accesstosql.md)  
