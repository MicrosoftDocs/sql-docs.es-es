---
title: Archivos de formato para importar y exportar datos
description: Al realizar una importación en bloque en una tabla de SQL Server o una exportación masiva desde una tabla, un archivo de formato puede almacenar información de formato de campo para un archivo de datos relativo a una tabla.
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], format files
- bulk importing [SQL Server], format files
- format files [SQL Server]
ms.assetid: b7b97d68-4336-4091-aee4-1941fab568e3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 35650bcd31127dcf3eaad46be4c958bc8f94547e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351383"
---
# <a name="format-files-to-import-or-export-data-sql-server"></a>Archivos de formato para importar y exportar datos (SQL Server)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Al importar masivamente datos en una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o exportar masivamente datos de una tabla, puede usar un *archivo de formato* para almacenar toda la información de formato necesaria para exportar o importar datos masivamente. Esto incluye la información de formato para cada campo de un archivo de datos relativo a la tabla.

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] admite dos tipos de archivos de formato: formatos XML y archivos de formato no XML. Tanto los archivos con formato XML como los archivos no XML contienen descripciones de todos los campos de un archivo de datos, y los archivos de formato XML también contienen descripciones de las columnas de tabla correspondientes. Por lo general, los archivos de formato XML y no XML son intercambiables. Sin embargo, es recomendable utilizar la sintaxis XML para los nuevos archivos de formato porque proporciona varias ventajas con relación a los archivos de formato no XML. Para obtener más información, vea [XML, archivos de formato &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).

## <a name="benefits-of-format-files"></a><a name="Benefits"></a> Ventajas de los archivos de formato

- El archivo de formato proporciona una manera flexible de escribir archivos de datos que requiere poca o ninguna modificación para adaptarlos a otros formatos de datos y para leer archivos de datos de otros programas.
- Permite realizar la importación masiva de datos sin tener que agregar o eliminar datos innecesarios o reordenar los datos existentes en el archivo de datos. Los archivos de formato resultan especialmente útiles cuando existe una discrepancia entre los campos del archivo de datos y las columnas de la tabla.

## <a name="examples-of-format-files"></a><a name="ExamplesOfFFs"></a> Ejemplos de archivos de formato

Los siguientes ejemplos muestran el diseño de un archivo de formato no XML y de un archivo de formato XML. Estos archivos de formato corresponden a la tabla `HumanResources.myTeam` de la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . Esta tabla tiene cuatro columnas: `EmployeeID`, `Name`, `Title`y `ModifiedDate`.

> [!NOTE]
> Para obtener más información sobre la tabla y cómo crearla, vea [Tabla de ejemplo HumanResources.myTeam &#40;SQL Server&#41;](../../relational-databases/import-export/humanresources-myteam-sample-table-sql-server.md).

### <a name="a-using-a-non-xml-format-file"></a>A. Uso de un archivo de formato no XML

El siguiente archivo de formato no XML usa el formato de datos nativo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la tabla `HumanResources.myTeam` . Este formato se creó usando el siguiente comando `bcp` .

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Fmt -n -T
The contents of this format file are as follows: 9.0
4
1       SQLSMALLINT   0       2       ""   1     EmployeeID               ""  
2       SQLNCHAR      2       100     ""   2     Name                     SQL_Latin1_General_CP1_CI_AS  
3       SQLNCHAR      2       100     ""   3     Title                    SQL_Latin1_General_CP1_CI_AS  
4       SQLNCHAR      2       100     ""   4     Background               SQL_Latin1_General_CP1_CI_AS  
```  

Para obtener más información, vea [Archivos de formato no XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).

### <a name="b-using-an-xml-format-file"></a>B. Uso de un archivo de formato XML

El siguiente archivo de formato XML usa el formato de datos nativo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la tabla `HumanResources.myTeam` . Este formato se creó usando el siguiente comando `bcp` .

```cmd
bcp AdventureWorks.HumanResources.myTeam format nul -f myTeam.Xml -x -n -T
```

El archivo de formato contiene:

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
  <FIELD ID="1" xsi:type="NativePrefix" LENGTH="1"/>
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="3" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
  <FIELD ID="4" xsi:type="NCharPrefix" PREFIX_LENGTH="2" MAX_LENGTH="100" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>
 </RECORD>
 <ROW>
  <COLUMN SOURCE="1" NAME="EmployeeID" xsi:type="SQLSMALLINT"/>
  <COLUMN SOURCE="2" NAME="Name" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="3" NAME="Title" xsi:type="SQLNVARCHAR"/>
  <COLUMN SOURCE="4" NAME="Background" xsi:type="SQLNVARCHAR"/>
</ROW>
</BCPFORMAT>
```

Para obtener más información, vea [XML, archivos de formato &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md). 

## <a name="when-is-a-format-file-required"></a><a name="WhenFFrequired"></a> ¿Cuándo se necesita un archivo de formato?

- Una instrucción INSERT ... SELECT * FROM OPENROWSET(BULK...) necesita siempre un archivo de formato.
- Para **bcp** o BULK INSERT, en situaciones simples, el uso de un archivo de formato es opcional y pocas veces necesario. Sin embargo, en situaciones de importación masiva complejas, el archivo de formato suele ser necesario.

Los archivos de formato se necesitan cuando:

- Se utiliza el mismo archivo de datos como origen de varias tablas que tienen esquemas distintos.
- El archivo de datos tiene un número de campos distinto al de columnas de la tabla de destino; por ejemplo:

  - La tabla de destino contiene como mínimo una columna para la que se ha definido un valor predeterminado o se admite un valor NULL.
  - Los usuarios no tienen permisos SELECT/INSERT en una o más columnas de la tabla.
  - Se utiliza un único archivo de datos con dos o más tablas que tienen esquemas distintos.

- El orden de columnas del archivo de datos y de la tabla es distinto.
- Los caracteres de terminación o las longitudes de prefijo no coinciden entre las columnas del archivo de datos.

> [!NOTE]
> En caso de no disponer de ningún archivo de formato, si un comando **bcp** especifica un modificador de formato de datos ( **-n**, **-c**, **-w** o **-N**) o una operación BULK INSERT especifica la opción DATAFILETYPE, se usará el formato de datos especificado como método predeterminado para interpretar los campos del archivo de datos.

## <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas

- [Crear un archivo de formato &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md)
- [Usar un archivo de formato para importar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Usar un archivo de formato para omitir una columna de tabla &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Usar un archivo de formato para omitir un campo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)
- [Usar un archivo de formato para asignar columnas de tabla a campos de un archivo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Consulte también

- [Archivos de formato no XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md)
- [XML, archivos de formato &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md)
- [Formatos de datos para importación en bloque o exportación masiva &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)
