---
title: Migración de bases de datos de MySQL a SQL Server Azure SQL Database | Microsoft Docs
description: Utilice este proceso recomendado para migrar bases de datos de MySQL a SQL Server o Azure SQL Database mediante SQL Server Migration Assistant (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8006f9a0-394d-4238-8dc5-44255134628b
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 2f4599389fd313447a926cfbc898735fc60f4823
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014655"
---
# <a name="migrating-mysql-databases-to-sql-server---azure-sql-database-mysqltosql"></a>Migración de bases de datos de MySQL a SQL Server-Azure SQL Database (MySQLToSql)
SQL Server Migration Assistant (SSMA) para MySQL es un entorno completo que le ayuda a migrar rápidamente las bases de datos de MySQL a SQL Server o SQL Azure. Mediante SSMA para MySQL, puede revisar los datos y los objetos de base de datos, evaluar las bases de datos para la migración, migrar objetos de base de datos a SQL Server o SQL Azure y, a continuación, migrar datos a SQL Server o SQL Azure.  
  
## <a name="recommended-migration-process"></a>Proceso de migración recomendado  
Para migrar correctamente objetos y datos de bases de datos MySQL a SQL Server o SQL Azure, utilice el siguiente proceso:  
  
1.  [Trabajar con proyectos de SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/working-with-ssma-projects-mysqltosql.md).  
  
    Después de crear el proyecto, puede establecer las opciones de conversión de proyectos, migración y asignación de tipos. Para obtener más información sobre la configuración del proyecto, vea [establecer opciones de proyecto &#40;MySQLToSQL&#41;](../../ssma/mysql/setting-project-options-mysqltosql.md). Para obtener información sobre cómo personalizar las asignaciones de tipos de datos, consulte [asignación de tipos de datos de MySQL y SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-and-sql-server-data-types-mysqltosql.md)  
  
2.  [Conexión a MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
3.  [Conexión a SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
4.  [Asignación de bases de datos de MySQL a esquemas de SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md)  
  
5.  [Conexión a Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
  
6.  Opcionalmente, la [evaluación de las bases de datos de MySQL para la conversión &#40;MySQLToSQL&#41;](../../ssma/mysql/assessing-mysql-databases-for-conversion-mysqltosql.md) para evaluar los objetos de base de datos para su conversión y calcular el tiempo de conversión.  
  
7.  [Conversión de bases de datos MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
8.  [Sincronización](loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
9. Para ello, siga uno de estos métodos:  
  
    -   Guarde un script y ejecútelo en SQL Server o SQL Azure.  
  
    -   Sincronizar los objetos de base de datos.  
  
10. [Migración de datos de MySQL a SQL Server Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/migrating-mysql-data-into-sql-server-azure-sql-db-mysqltosql.md)  
  
11. Si es necesario, actualice las aplicaciones de base de datos.  
  
> [!NOTE]  
> No se pueden migrar esquemas de Information_schema y MySQL.  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SSMA para MySQL &#40;MySqlToSql&#41;](../../ssma/mysql/installing-ssma-for-mysql-mysqltosql.md)  
[Introducción con SSMA para MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/getting-started-with-ssma-for-mysql-mysqltosql.md)  
  
