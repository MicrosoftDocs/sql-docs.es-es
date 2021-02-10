---
title: Migración de bases de datos DB2 a SQL Server (DB2ToSQL) | Microsoft Docs
description: Utilice este proceso recomendado para migrar bases de datos de DB2 a SQL Server o Azure SQL Database mediante SQL Server Migration Assistant (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 14d2e655-af7e-4aa5-ba28-0e3d0d025518
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 912f1ffd342517add68d1ce7f628251fc86dc6aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017705"
---
# <a name="migrating-db2-databases-to-sql-server-db2tosql"></a>Migración de bases de datos DB2 a SQL Server (DB2ToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) para DB2 es un entorno completo que le ayuda a migrar rápidamente las bases de datos de DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Con SSMA para DB2, puede revisar los datos y los objetos de base de datos, evaluar las bases de datos para la migración, migrar objetos de base de datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database y, a continuación, migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database. Tenga en cuenta que no puede migrar los esquemas SYS y SYSTEM DB2.  
  
## <a name="recommended-migration-process"></a>Proceso de migración recomendado  
Para migrar correctamente objetos y datos de bases de datos DB2 a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o Azure SQL Database, utilice el siguiente proceso:  
  
1.  [Nuevo proyecto de SSMA](./new-project-db2tosql.md).  
  
    Después de crear el proyecto, puede establecer las opciones de conversión de proyectos, migración y asignación de tipos. Para obtener información sobre la configuración del proyecto, vea [configuración del proyecto &#40;conversión&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md) y secciones relacionadas. Para obtener información acerca de cómo personalizar las asignaciones de tipos de datos, consulte [asignar tipos de datos de DB2 y SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/mapping-db2-and-sql-server-data-types-db2tosql.md).  
  
2.  [Conéctese a la base de datos DB2](./connecting-to-db2-database-db2tosql.md).  
  
3.  [Conexión a SQL Server](./connecting-to-sql-server-db2tosql.md).  
  
4.  [Asigne esquemas DB2 a esquemas de SQL Server](./mapping-db2-schemas-to-sql-server-schemas-db2tosql.md).  
  
5.  Opcionalmente, los [informes de evaluación](./assessment-report-db2tosql.md) para evaluar los objetos de base de datos para la conversión y estiman el tiempo de conversión.  
  
6.  [Convertir esquemas DB2](./converting-db2-schemas-db2tosql.md).  
  
7.  [Cargue los objetos de base de datos convertidos en SQL Server](./loading-converted-database-objects-into-sql-server-db2tosql.md).  
  
    Para ello, siga uno de estos métodos:  
  
    -   Guarde un script y ejecútelo en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Sincronizar los objetos de base de datos.  
  
8.  [Migración de datos de DB2 a SQL Server](./migrating-db2-data-into-sql-server-db2tosql.md).  
  
9. Si es necesario, actualice las aplicaciones de base de datos.  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SSMA para DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/installing-ssma-for-db2-db2tosql.md)  
[Introducción con SSMA para DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/getting-started-with-ssma-for-db2-db2tosql.md)  
