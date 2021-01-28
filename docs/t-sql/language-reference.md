---
title: Referencia de Transact-SQL (motor de base de datos)
description: Referencia de Transact-SQL (motor de base de datos)
ms.custom: ''
ms.date: 04/29/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- sql13.tsqlref.f1
- devlang-tsql
helpviewer_keywords:
- Transact-SQL
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dfe9a07eb1b37f0bddf128b1cbdf39de3d74a643
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98689084"
---
# <a name="transact-sql-reference-database-engine"></a>Referencia de Transact-SQL (motor de base de datos)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]

En este tema se proporcionan los conceptos básicos sobre cómo encontrar y utilizar los temas de referencia de Microsoft [!INCLUDE[tsql](../includes/tsql-md.md)] (T-SQL). T-SQL es fundamental para trabajar con servicios y productos de Microsoft SQL. Todas las herramientas y aplicaciones que se comunican con una base de datos SQL lo hacen enviando comandos T-SQL.  

## <a name="t-sql-compliance-with-sql-standard"></a>Cumplimiento de T-SQL con el estándar de SQL
Para obtener documentos técnicos detallados sobre cómo se implementan determinados estándares en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vea la [documentación de compatibilidad de los estándares de Microsoft SQL Server](/openspecs/sql_standards/ms-sqlstandlp/89fb00b1-4b9e-4296-92ce-a2b3f7ca01d2).

## <a name="tools-that-use-t-sql"></a>Herramientas que usan T-SQL
Estas son algunas de las herramientas de Microsoft que emiten comandos T-SQL:

- [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md)
- [sqlcmd](../tools/sqlcmd-utility.md)

## <a name="locate-the-transact-sql-reference-topics"></a>Encontrar los temas de referencia de Transact-SQL  
Para encontrar los temas de T-SQL, utilice la búsqueda situada en la parte superior derecha de esta página, o bien la tabla de contenido que hay en la parte izquierda de la página. También puede escribir una palabra clave de T-SQL en la ventana del editor de consultas de Management Studio y pulsar F1.

## <a name="find-system-views"></a>Encontrar las vistas del sistema
Para encontrar las tablas, las visualizaciones, las funciones y los procedimientos del sistema, vea estos vínculos, que se encuentran en la sección [Uso de bases de datos relacionales](../relational-databases/databases/databases.md) de la documentación de SQL.

- [Vistas de catálogo del sistema](../relational-databases/system-catalog-views/catalog-views-transact-sql.md)
- [Vistas de compatibilidad del sistema](../relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)
- [Vistas de administración dinámica del sistema](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)
- [System functions](../relational-databases/system-functions/system-functions-category-transact-sql.md) (Funciones del sistema)
- [Vistas de esquema de información del sistema](../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [Procedimientos almacenados del sistema](../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
- [Tablas del sistema](../relational-databases/system-tables/system-tables-transact-sql.md)

## <a name="applies-to-references"></a>Referencias de "Se aplica a"  

Los temas de referencia de T-SQL abarcan varias versiones de SQL Server, empezando por la de 2008, y otros servicios de Azure SQL. En la parte superior de cada tema hay una sección que indica qué productos y servicios son compatibles con el tema. 

Por ejemplo, este tema se aplica a todas las versiones y tiene la etiqueta siguiente.

[!INCLUDE[tsql-appliesto-ss2008-all_md](../includes/tsql-appliesto-ss2008-all-md.md)]

A modo de ejemplo adicional, la etiqueta siguiente indica un tema que solo se aplica a Azure Synapse Analytics y al Almacenamiento de datos paralelos.

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw_md](../includes/applies-to-version/asa-pdw.md)]

En algunos casos, hay un producto o servicio que utiliza el tema en cuestión, pero no se admiten todos los argumentos. En este caso, se insertan otras secciones **Se aplica a** en las descripciones del argumento correspondiente en el cuerpo del tema.

## <a name="get-help-from-microsoft-q--a"></a>Obtención de ayuda en las preguntas y respuestas de Microsoft

Para obtener ayuda en línea, consulte el [foro Transact-SQL de preguntas y respuestas de Microsoft](/answers/topics/sql-server-transact-sql.html).

## <a name="see-other-language-references"></a>Ver otras referencias de idioma

En los documentos de SQL se incluyen estas otras referencias de idioma:

- [Referencia del lenguaje de XQuery](../xquery/xquery-language-reference-sql-server.md)
- [Referencia del lenguaje de Integration Services](../integration-services/integration-services-language-reference.md)
- [Referencia del lenguaje de replicación](../relational-databases/replication/replication-language-reference.md)
- [Referencia del lenguaje de Analysis Services](../mdx/multidimensional-expressions-mdx-reference.md)

## <a name="next-steps"></a>Pasos siguientes
Ahora que ya sabe cómo encontrar los temas de referencia de T-SQL, puede hacer lo siguiente:

- Consultar un breve tutorial sobre cómo escribir instrucciones T-SQL, vea [Tutorial: Escribir instrucciones Transact-SQL](../t-sql/tutorial-writing-transact-sql-statements.md).
- Ver el artículo [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).