---
title: Migración de bases de datos de Oracle a SQL Server (OracleToSQL) | Microsoft Docs
description: Utilice este proceso recomendado para migrar bases de datos de Oracle a SQL Server o Azure SQL Database mediante SQL Server Migration Assistant (SSMA).
ms.prod: sql
ms.custom: ''
ms.date: 04/22/2018
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 1d196dd6-4322-4c98-bb72-602c57d96134
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: df5bd55fb9c5490b8063712dbebe5328917e08b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063360"
---
# <a name="migrating-oracle-databases-to-sql-server-oracletosql"></a>Migración de bases de datos de Oracle a SQL Server (OracleToSQL)
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Migration Assistant (SSMA) para Oracle es un entorno completo que le ayuda a migrar rápidamente las bases de datos de Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Azure SQL Database o Azure Synapse Analytics. Con SSMA para Oracle, puede revisar los datos y los objetos de base de datos, evaluar las bases de datos para la migración, migrar objetos de base de datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Azure SQL Database o Azure Synapse Analytics y, a continuación, migrar datos a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Azure SQL Database o Azure Synapse Analytics. Tenga en cuenta que no puede migrar los esquemas SYS y SYSTEM de Oracle.
  
## <a name="recommended-migration-process"></a>Proceso de migración recomendado  
Para migrar correctamente objetos y datos de bases de datos de Oracle a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Azure SQL Database o Azure Synapse Analytics, use el siguiente proceso:
  
1.  [Cree un nuevo proyecto de SSMA](working-with-ssma-projects-oracletosql.md).  
  
    Después de crear el proyecto, puede establecer las opciones de conversión de proyectos, migración y asignación de tipos. Para obtener información sobre la configuración del proyecto, vea [establecer opciones de proyecto &#40;OracleToSQL&#41;](../../ssma/oracle/setting-project-options-oracletosql.md). Para obtener información sobre cómo personalizar las asignaciones de tipos de datos, vea [asignar tipos de datos de Oracle y SQL Server &#40;&#41;OracleToSQL ](../../ssma/oracle/mapping-oracle-and-sql-server-data-types-oracletosql.md).  
  
2.  [Conéctese al servidor de base de datos de Oracle](connecting-to-oracle-database-oracletosql.md).  
  
3.  [Conectarse a una instancia de SQL Server](connecting-to-sql-server-oracletosql.md).  
  
4.  [Asignar esquemas de base de datos de Oracle a SQL Server esquemas de base de datos](mapping-oracle-schemas-to-sql-server-schemas-oracletosql.md).  
  
5.  Opcionalmente, [cree informes de evaluación](assessing-oracle-schemas-for-conversion-oracletosql.md) para evaluar los objetos de base de datos para la conversión y calcular el tiempo de conversión.  
  
6.  [Convertir esquemas de base de datos de Oracle en esquemas de SQL Server](converting-oracle-schemas-oracletosql.md).  
  
7.  [Cargue los objetos de base de datos convertidos en SQL Server](loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
    Para ello, siga uno de estos métodos:  
  
    -   Guarde un script y ejecútelo en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Sincronizar los objetos de base de datos.  
  
8.  [Migre los datos a SQL Server](migrating-oracle-data-into-sql-server-oracletosql.md).  
  
9. Si es necesario, actualice las aplicaciones de base de datos.  
  
## <a name="see-also"></a>Consulte también  
[Instalación de SSMA para Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/installing-ssma-for-oracle-oracletosql.md)  
[Introducción con SSMA para Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/getting-started-with-ssma-for-oracle-oracletosql.md)  
  
