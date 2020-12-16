---
title: Importación y exportación de datos en bloque con BCP
description: Use BCP para exportar datos desde cualquier lugar de una base de datos SQL Server en la que funcione la instrucción SELECT. Realice una exportación masiva de datos desde una tabla o una consulta y una importación en bloque desde un archivo.
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], bcp utility
- bulk importing [SQL Server], bcp utility
- bcp utility [SQL Server], about bcp utility
ms.assetid: 73e949de-67a3-4c84-9735-7da1ad4ba34a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.date: 09/28/2016
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 1bd365ee0af1bd1e8064b9bb6a5ba486abdf06cf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473986"
---
# <a name="import-and-export-bulk-data-using-bcp-sql-server"></a>Importación y exportación de datos en bloque con BCP (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Este tema constituye una introducción al uso de la [utilidad bcp](../../tools/bcp-utility.md) para exportar datos desde cualquier lugar de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la que funcione una instrucción SELECT, incluidas las vistas con particiones.  
  
 La utilidad bcp (Bcp.exe) es una herramienta de línea de comandos que utiliza la API de importación masiva de Bulk Copy Program o Programa de copia masiva (BCP). La utilidad bcp realiza las tareas siguientes:  
  
-   Exportaciones masivas de datos de una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un archivo de datos.  
  
-   Exportaciones masivas de una consulta.  
  
-   Importaciones masivas de datos de un archivo de datos a una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Genera archivos de formato.  
  
 A la utilidad bcp se accede con el comando **bcp** . Para usar el comando **bcp** para realizar importaciones masivas de datos, debe conocer el esquema de la tabla y los tipos de datos de sus columnas, a menos que se use un archivo con un formato ya existente.  
  
 La utilidad bcp permite exportar datos de una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un archivo de datos para usarlos en otros programas. También permite importar datos a una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] desde otro programa, normalmente otro sistema de administración de bases de datos (DBMS). Los datos se exportan primero desde el programa de origen a un archivo de datos y, después, se copian del archivo de datos a una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 El comando **bcp** proporciona modificadores para especificar el tipo de datos del archivo de datos y otra información. Si no se especifican estos modificadores, el comando solicitará información de formato, como el tipo de campos de datos de un archivo de datos A continuación, el comando preguntará si se desea crear un archivo de formato con las respuestas interactivas. Si desea flexibilidad para operaciones futuras de importación o exportación masivas, un archivo de formato suele resultar útil. Puede especificar el archivo de formato en comandos **bcp** posteriores para archivos de datos equivalentes. Para obtener más información, vea [Especificar formatos de datos por razones de compatibilidad mediante bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
>**Nota** La utilidad bcp se escribe usando la copia masiva de ODBC.
  
 Para obtener una descripción de la sintaxis del comando **bcp** , vea [bcp Utility](../../tools/bcp-utility.md).  
  
## <a name="examples"></a>Ejemplos  

|Los temas siguientes incluyen ejemplos de uso de bcp: |
|---|
|[bcp (utilidad)](../../tools/bcp-utility.md)<br /><br />Formatos de datos para importación o exportación masivas (SQL Server)<br />&emsp;&#9679;&emsp;[Usar el formato nativo para importar o exportar datos (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar el formato de caracteres para importar o exportar datos (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar el formato nativo Unicode para importar o exportar datos (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar el formato de caracteres Unicode para importar o exportar datos (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br /><br />[Especificar terminadores de campo y de fila (SQL Server)](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)<br /><br />[Mantener valores NULL o usar valores predeterminados durante la importación masiva (SQL Server)](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)<br /><br />[Mantener valores de identidad al importar datos de forma masiva (SQL Server)](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)<br /><br />Archivos de formato para importar o exportar datos (SQL Server)<br />&emsp;&#9679;&emsp;[Crear un archivo de formato (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar un archivo de formato para importar datos de forma masiva (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar un archivo de formato para omitir una columna de tabla (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar un archivo de formato para omitir un campo de datos (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[Usar un archivo de formato para asignar columnas de tabla a campos de un archivo de datos (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)<br /><br />[Ejemplos de importación y exportación de forma masiva documentos XML (SQL Server)](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)<br /><p>                                                                                                                                                                                                                  </p>|

## <a name="more-examples-and-information"></a>Más información y ejemplos

- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)

- [Cláusula SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-clause-transact-sql.md)

- [bcp (utilidad)](../../tools/bcp-utility.md)

- [Preparación para la importación de datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-to-bulk-import-data-sql-server.md)

- [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)

- [Importar y exportar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)

- [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)

- [Crear un archivo de formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)