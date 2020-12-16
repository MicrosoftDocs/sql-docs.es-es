---
title: Uso del formato nativo para importar o exportar datos
description: En la importación y la exportación de SQL Server, el formato nativo mantiene los tipos de datos nativos de una base de datos para la transferencia de datos de alta velocidad entre tablas de SQL Server.
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- native data format [SQL Server]
- data formats [SQL Server], native
ms.assetid: eb279b2f-0f1f-428f-9b8f-2a7fc495b79f
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 4a1802cc2bcd64141c35dcc1e15f4609848e7c79
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465556"
---
# <a name="use-native-format-to-import-or-export-data-sql-server"></a>Uso del formato nativo para importar o exportar datos (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Se recomienda usar el formato nativo cuando se realice una transferencia masiva de datos entre varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usando un archivo de datos que no contiene caracteres extendidos o de doble byte (DBCS).  

> [!NOTE]
>  Para realizar una transferencia masiva de datos entre varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando un archivo de datos que contenga caracteres extendidos o DBCS, debe utilizar el formato nativo Unicode. Para obtener más información, vea [Usar el formato nativo Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).

El formato nativo mantiene los tipos de datos nativos de una base de datos. Está pensado para transferencias de datos de alta velocidad entre tablas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si utiliza un archivo de formato, las tablas de origen y destino no tienen que ser idénticas. La transferencia de datos implica dos pasos:  
  
1.  Exportación masiva de datos de una tabla de origen a un archivo de datos  
  
2.  Importación masiva de datos de un archivo de datos a una tabla de destino  

El uso del formato nativo entre tablas idénticas evita la conversión innecesaria de tipos de datos a y desde el formato de caracteres, lo que ahorra tiempo y espacio. No obstante, para obtener la velocidad de transferencia óptima, se realizan algunas comprobaciones relativas al formato de los datos. Para evitar problemas con los datos cargados, vea la siguiente lista de restricciones.  


## <a name="restrictions"></a>Restricciones
Para importar datos con formato nativo correctamente, asegúrese de lo siguiente:  
  
-   El archivo de datos tiene formato nativo.  
  
-   O bien la tabla de destino debe ser compatible con el archivo de datos (tiene el mismo número de columnas, el mismo tipo de datos, la misma longitud, el estado NULL, etc.) o bien debe utilizar un archivo de formato para asignar cada campo a las columnas correspondientes.  
  
    > [!NOTE]
    >  Si importa datos de un archivo que no coincide con la tabla de destino, la operación de importación puede ser correcta pero, probablemente, los valores de los datos insertados en la tabla de destino son incorrectos. Esto se debe a que los datos del archivo se interpretan utilizando el formato de la tabla de destino. Por tanto, si hay alguna incoherencia, se insertan valores incorrectos. No obstante, en ningún caso puede tal incoherencia dar lugar a incoherencias lógicas o físicas en la base de datos.  
  
     Para obtener más información sobre cómo usar archivos de formato, vea [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 Una importación correcta no daña la tabla de destino.  
  
## <a name="how-bcp-handles-data-in-native-format"></a>Cómo trata bcp los datos con formato nativo
 Esta sección aborda una serie de consideraciones especiales sobre el modo en que la utilidad **bcp** exporta e importa datos con formato nativo.  
  
-   Datos que no son caracteres  
  
     La [utilidad bcp](../../tools/bcp-utility.md) usa el formato de datos binario interno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para escribir datos que no son caracteres de una tabla a un archivo de datos.  
  
-   Datos[char](../../t-sql/data-types/char-and-varchar-transact-sql.md) o [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)  
  
     Al principio de cada campo [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) o [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) , [bcp](../../tools/bcp-utility.md) agrega la longitud de prefijo.  
  
    > [!IMPORTANT]
    >  Cuando se usa el modo nativo, de forma predeterminada, la [utilidad bcp](../../tools/bcp-utility.md) convierte los caracteres de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en caracteres OEM antes de copiarlos en un archivo de datos. La [utilidad bcp](../../tools/bcp-utility.md) convierte los caracteres de un archivo de datos en caracteres ANSI antes de importarlos masivamente a una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Durante estas conversiones, se pueden perder datos que tengan caracteres extendidos. Para caracteres extendidos, utilice el formato nativo Unicode o especifique una página de códigos.
  
-   Datos[sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)  
  
     Si se almacenan datos [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) como SQLVARIANT en un archivo de datos con formato nativo, los datos mantienen todas sus características. Los metadatos que registran el tipo de datos de cada valor de datos se almacenan junto con el valor de los datos. Estos metadatos se usan para volver a crear el valor de los datos con el mismo tipo de datos en una columna [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) de destino.  
  
     Si el tipo de datos de la columna de destino no es [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md), cada valor de datos se convierte al tipo de datos de la columna de destino, con las reglas normales de conversión implícita de datos. Si se produce un error durante la conversión de los datos, se revierte el lote actual. Los valores [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) y [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) que se transfieren entre las columnas [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) pueden tener problemas de conversión de página de códigos.  
  
     Para obtener más información sobre la conversión de datos, vea [Conversiones de tipos de datos &#40;motor de base de datos&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md).  
  
## <a name="command-options-for-native-format"></a>Opciones de comando para el formato nativo
Puede importar datos con formato nativo a una tabla mediante el uso de [bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o [INSERT... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Para un comando [bcp](../../tools/bcp-utility.md) o una instrucción [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), puede especificar el formato de datos en la instrucción.  Para una instrucción [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md), debe especificar el formato de datos en un archivo de formato.  

El formato nativo admite las siguientes opciones de comando:  

|Get-Help|Opción|Descripción|  
|-------------|------------|-----------------|  
|BCP|**-n**|Hace que la utilidad bcp use los tipos de datos nativos de los datos.*|  
|BULK INSERT|DATAFILETYPE **='native'**|Utiliza los tipos de datos nativos o nativos anchos de los datos. Tenga en cuenta que DATAFILETYPE no es necesario si el archivo de formato especifica los tipos de datos.|  
|OPENROWSET|N/D|Debe usar un archivo de formato|

  
 \*Para cargar datos nativos ( **-n**) en un formato compatible con versiones anteriores de clientes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , use el modificador **-V** . Para obtener más información, vea [Importar datos con formato nativo y de caracteres de versiones anteriores de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
> [!NOTE]
>  Otra posibilidad es especificar el formato por campo en un archivo de formato. Para obtener más información, vea [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  

## <a name="example-test-conditions"></a>Condiciones de prueba de ejemplo
Los ejemplos de este tema se basan en la tabla y en el archivo de formato definidos a continuación.

### <a name="sample-table"></a>Tabla de ejemplo
El siguiente script crea una base de datos de prueba, una tabla denominada `myNative` , y la rellena con algunos valores iniciales.  Ejecutar el siguiente Transact-SQL en Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myNative ( 
   PersonID smallint NOT NULL,
   FirstName varchar(25) NOT NULL,
   LastName varchar(30) NOT NULL,
   BirthDate date,
   AnnualSalary money
   );

-- Populate table
INSERT TestDatabase.dbo.myNative
VALUES 
(1, 'Anthony', 'Grosse', '1980-02-23', 65000.00),
(2, 'Alica', 'Fatnowna', '1963-11-14', 45000.00),
(3, 'Stella', 'Rossenhain', '1992-03-02', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="sample-non-xml-format-file"></a>Archivo de formato no XML de ejemplo
SQL Server admite dos tipos de archivos de formato: XML y no XML.  El formato no XML es el formato original compatible con versiones anteriores de SQL Server.  Revise [Archivos de formato no XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) para obtener información detallada.  El siguiente comando hará uso de la [utilidad BCP](../../tools/bcp-utility.md) para generar un archivo de formato no XML, `myNative.fmt`, basado en el esquema de `myNative`.  Si quiere usar un comando [BCP](../../tools/bcp-utility.md) para crear un archivo de formato, especifique el argumento **format** y use **null** en lugar de una ruta de acceso de archivo de datos.  La opción format también requiere la opción **-f** .  Además, en este ejemplo, el calificador **c** se usa para especificar los datos de caracteres, mientras que **T** se usa para especificar una conexión de confianza que usa la seguridad integrada.  En un símbolo del sistema, escriba los comandos siguientes:

```cmd
bcp TestDatabase.dbo.myNative format nul -f D:\BCP\myNative.fmt -T 

REM Review file
Notepad D:\BCP\myNative.fmt
```

> [!IMPORTANT]
> Asegúrese de que el archivo de formato no XML termina con un retorno de carro o avance de línea.  De lo contrario, es probable que reciba el mensaje de error siguiente:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Ejemplos
En los siguientes ejemplos se usan la base de datos y los archivos de formato creados anteriormente.

### <a name="using-bcp-and-native-format-to-export-data"></a>Usar bcp y el formato nativo para exportar datos
Modificador **-n** y comando **OUT** .  Nota: el archivo de datos creado en este ejemplo se usará en todos los ejemplos siguientes.  En un símbolo del sistema, escriba los comandos siguientes:

```cmd
bcp TestDatabase.dbo.myNative OUT D:\BCP\myNative.bcp -T -n

REM Review results
NOTEPAD D:\BCP\myNative.bcp
```

### <a name="using-bcp-and-native-format-to-import-data-without-a-format-file"></a>Usar bcp y el formato nativo para importar datos sin un archivo de formato
Modificador **-n** y comando **IN** .  En un símbolo del sistema, escriba los comandos siguientes:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -T -n

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bcp-and-native-format-to-import-data-with-a-non-xml-format-file"></a>Usar bcp y el formato nativo para importar datos con un archivo de formato no XML
Modificadores **-n** y **-f** switches y **IN** commy.  En un símbolo del sistema, escriba los comandos siguientes:

```cmd
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myNative;"

REM Import data
bcp TestDatabase.dbo.myNative IN D:\BCP\myNative.bcp -f D:\BCP\myNative.fmt -T

REM Review results
SQLCMD -Q "SELECT * FROM TestDatabase.dbo.myNative;"
```

### <a name="using-bulk-insert-and-native-format-without-a-format-file"></a>Usar BULK INSERT y el formato nativo sin un archivo de formato
Argumento **DATAFILETYPE** .  Ejecutar el siguiente Transact-SQL en Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
    FROM 'D:\BCP\myNative.bcp'
    WITH (
        DATAFILETYPE = 'native'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-bulk-insert-and-native-format-with-a-non-xml-format-file"></a>Usar BULK INSERT y el formato nativo con un archivo de formato no XML
Argumento **FORMATFILE** .  Ejecutar el siguiente Transact-SQL en Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative; -- for testing
BULK INSERT TestDatabase.dbo.myNative
   FROM 'D:\BCP\myNative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myNative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```

### <a name="using-openrowset-and-native-format-with-a-non-xml-format-file"></a>Usar OPENROWSET y el formato nativo con un archivo de formato no XML
Argumento **FORMATFILE** .  Ejecutar el siguiente Transact-SQL en Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):

```sql
TRUNCATE TABLE TestDatabase.dbo.myNative;  -- for testing
INSERT INTO TestDatabase.dbo.myNative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myNative.bcp', 
        FORMATFILE = 'D:\BCP\myNative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myNative;
```
  
## <a name="related-tasks"></a>Tareas relacionadas
Para usar formatos de datos para la importación o exportación masivas 
  
-   [Importar datos con formato nativo y de caracteres de versiones anteriores de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Usar el formato de caracteres para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato de caracteres Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato nativo Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [bcp (utilidad)](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)   
 [Importar datos con formato nativo y de caracteres de versiones anteriores de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Usar el formato nativo Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
  
