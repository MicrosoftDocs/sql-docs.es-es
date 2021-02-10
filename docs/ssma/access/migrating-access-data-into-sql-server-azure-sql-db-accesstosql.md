---
description: Migración de datos de Access en SQL Server-Azure SQL Database (AccessToSQL)
title: Migración de datos de Access en SQL Server-Azure SQL Database (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- bulk loading data
- data, loading into SQL Azure
- data, loading into SQL Server
- migrating databases, loading data
- migrating databases, options
- options, migrating data
- SQL Azure, migrating data into
- SQL Server, migrating data into
ms.assetid: f3b18af7-1af0-499d-a00d-a0af94895625
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a86ae9cff414e052865cf0efd1ec5c7889877d0f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066780"
---
# <a name="migrating-access-data-into-sql-server---azure-sql-database-accesstosql"></a>Migración de datos de Access en SQL Server-Azure SQL Database (AccessToSQL)
Después de haber creado correctamente los objetos de base [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de datos en, puede migrar los datos de Access a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
## <a name="setting-migration-options"></a>Establecer opciones de migración  
Antes de migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure, revise las opciones de migración del proyecto en el cuadro de diálogo **configuración del proyecto** . En este cuadro de diálogo, puede establecer el tamaño del lote de migración, el bloqueo de tablas, la comprobación de restricciones, la activación del desencadenador de inserción, la identidad y el control de valores NULL, y cómo controlar las fechas que están fuera del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intervalo. Para obtener más información, vea [configuración del proyecto (migración)](./project-settings-migration-accesstosql.md).  
  
## <a name="migrating-data"></a>Migrar datos  
La migración de datos es una operación de carga masiva que mueve filas de datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure en transacciones. El número de filas que se van a cargar en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure en cada transacción se configura en la configuración del proyecto.  
  
Para ver los mensajes de migración, asegúrese de que el panel de resultados esté visible. Si no es así, en el menú **Ver** , seleccione **salida**.  
  
**Para migrar datos**  
  
1.  Asegúrese de que ha cargado los objetos de base de datos de Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o SQL Azure.  
  
2.  En el explorador de metadatos de Access, seleccione los objetos que contienen los datos que desea migrar:  
  
    -   Para migrar datos de una base de datos completa, active la casilla situada junto al nombre de la base de datos.  
  
    -   Para migrar datos de tablas individuales, expanda la base de datos, expanda **tablas** y, a continuación, active la casilla situada junto a la tabla. Para omitir los datos de tablas individuales, desactive la casilla.  
  
3.  Haga clic con el botón derecho en **bases** de **datos y seleccione migrar datos**.  
  
También puede migrar datos fuera de SSMA mediante la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilidad de línea de comandos **BCP** o [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Para obtener más información acerca de estas herramientas, vea los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libros en pantalla de.  
  
## <a name="next-step"></a>siguiente paso  
Si tiene acceso a las aplicaciones de base de datos que desea seguir usando después de la migración, vincule las tablas de base de datos de Access a las [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tablas o SQL Azure. Para obtener más información, consulte [vincular aplicaciones de Access a SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md).  
  
## <a name="see-also"></a>Consulte también  
[Migrar bases de datos de Access a SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Configuración de las opciones de conversión y migración](setting-conversion-and-migration-options-accesstosql.md)  
