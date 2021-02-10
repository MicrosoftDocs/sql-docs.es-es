---
title: Migrar bases de datos de Access a SQL Server-Azure SQL Database | Microsoft Docs
description: Utilice este proceso recomendado para migrar bases de datos de Access a SQL Server o Azure SQL Database mediante SQL Server Migration Assistant (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- instructions, migration
- migrating databases, overview
- overview, migration process
- procedure, migration
- recommended migration process
ms.assetid: 76a3abcf-2998-4712-9490-fe8d872c89ca
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 683c17a23b1eda96c040dabcf62d58b24fbb53ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066710"
---
# <a name="migrating-access-databases-to-sql-server---azure-sql-database-accesstosql"></a>Migración de bases de datos de Access a SQL Server-Azure SQL Database (AccessToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) es una herramienta que proporciona un entorno completo que le ayuda a migrar rápidamente las bases de datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure. Con SSMA, puede revisar el acceso y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los objetos de Azure SQL Database, evaluar la base de datos de Access para la migración, convertir los objetos de base de datos de Access, cargarlos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure y, a continuación, migrar los datos.  
  
## <a name="recommended-migration-process"></a>Proceso de migración recomendado  
Para migrar correctamente objetos y datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, utilice el siguiente proceso:  
  
1.  [Cree un nuevo proyecto de SSMA](creating-and-managing-projects-accesstosql.md). Después de crear el proyecto, puede [establecer las opciones del proyecto](setting-conversion-and-migration-options-accesstosql.md), incluidas las opciones de conversión, las opciones de migración y las asignaciones de tipos de datos.  
  
2.  [Agregue archivos de base de datos de Access](adding-and-removing-access-database-files-accesstosql.md) al proyecto.  
  
    Puede Agregar archivos individuales, incluidos los archivos que encuentre en el equipo o la red.  
  
3.  [Conéctese a la instancia de destino de SQL Server](connecting-to-sql-server-accesstosql.md) o [Conéctese a la instancia de destino de SQL Azure](connecting-to-azure-sql-db-accesstosql.md).  
  
    Puede conectarse a SQL Server o SQL Azure.  
  
4.  Para personalizar la asignación entre una o varias bases de datos de Access y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure esquemas,  [asigne las bases de datos de origen y de destino](mapping-source-and-target-databases-accesstosql.md).  
  
5.  Opcionalmente, puede [crear un informe de evaluación](assessing-access-database-objects-for-conversion-accesstosql.md) para determinar si los objetos de base de datos de Access se pueden convertir [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure correctamente.  
  
6.  [Convierta objetos de base de datos de Access](converting-access-database-objects-accesstosql.md) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure definiciones de objeto.  
  
7.  [Cargue los objetos de base de datos convertidos en SQL Server](loading-converted-database-objects-into-sql-server-accesstosql.md).  
  
    Puede cargar los objetos de base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure mediante SSMA, o puede guardar [!INCLUDE[tsql](../../includes/tsql-md.md)] scripts.  
  
8.  [Migre los datos de Access en SQL Server](migrating-access-data-into-sql-server-azure-sql-db-accesstosql.md).  
  
    > [!NOTE]  
    > Puede convertir, cargar y migrar esquemas y datos en un solo paso. Para realizar una migración con un solo clic, haga clic en el botón **convertir, cargar y migrar** .  
  
9. Si desea que las aplicaciones de Access utilicen los datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, use [vincular las tablas de Access a las tablas de SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md).  
  
También puede usar el Asistente para migración para guiarle en este proceso. Para obtener más información, vea [Asistente para migración](migration-wizard-accesstosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Introducción con SQL Server Migration Assistant para el acceso](getting-started-with-sql-server-migration-assistant-for-access-accesstosql.md)  
[Preparar las bases de datos de Access para la migración](preparing-access-databases-for-migration-accesstosql.md)
