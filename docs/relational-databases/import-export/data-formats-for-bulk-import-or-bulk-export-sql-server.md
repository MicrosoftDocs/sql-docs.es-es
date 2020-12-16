---
title: Formatos de datos para importación o exportación masivas
description: SQL Server acepta datos en formato de caracteres o binario nativo. Use el formato de caracteres entre SQL Server y otras aplicaciones y el formato nativo entre instancias de SQL Server.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- data formats [SQL Server], choosing
- bulk importing [SQL Server], data formats
ms.assetid: 73fe6741-9437-4b26-b030-28b863e74399
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 3a043b45749674d09cd47bfc62eea7d7f70647ee
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474056"
---
# <a name="data-formats-for-bulk-import-or-bulk-export-sql-server"></a>Formatos de datos para importación o exportación masivas (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede aceptar datos en formato de datos de caracteres o binarios nativos. Use el formato de caracteres al mover datos entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y otra aplicación (como [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel) u otro servidor de bases de datos (como Oracle o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Solo puede usar el formato nativo al transferir datos entre instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **En este tema:**  
  
-   [Formatos de datos para importación o exportación masivas](#ComponentsAndConcepts)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="data-formats-for-bulk-import-or-export"></a><a name="ComponentsAndConcepts"></a> Formatos de datos para importación o exportación masivas  
 En la siguiente tabla se indica el formato de datos que resulta adecuado, de forma general, para usar dependiendo del modo en que los datos estén representados y el origen o el destino de la operación.  
  
|Operación|Nativa|Unicode nativo|Carácter|carácter Unicode|  
|---------------|------------|--------------------|---------------|-----------------------|  
|Transferencias masivas de datos entre varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante un archivo de datos que no contenga caracteres del juego de caracteres de doble byte (DBCS). A menos que se use un archivo de formato, estas tablas deben definirse de forma idéntica.|Sí*|-|-|-|  
|En el caso de las columnas **sql_variant** , se recomienda usar el formato de datos nativo, ya que conserva los metadatos de cada valor **sql_variant** , a diferencia de los formatos de caracteres o Unicode.|Sí|-|-|-|  
|Transferencias masivas de datos entre varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante un archivo de datos que contenga caracteres DBCS o extendidos.|-|Sí|-|-|  
|Importación masiva de datos desde un archivo de texto generado por otro programa.|-|-|Sí|-|  
|Exportación masiva de datos a un archivo de texto para usarlo en otro programa.|-|-|Sí|-|  
|Transferencias masivas de datos entre varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante un archivo de datos que contenga datos Unicode y no contenga caracteres DBCS o extendidos.|-|-|-|Sí|  
  
 \* El método más rápido para la exportación masiva de datos desde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando se usa **bcp**.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Usar el formato nativo para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato de caracteres para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato nativo Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato de caracteres Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Importar datos con formato nativo y de caracteres de versiones anteriores de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Especificar formatos de datos por razones de compatibilidad mediante bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md)  
  
  
