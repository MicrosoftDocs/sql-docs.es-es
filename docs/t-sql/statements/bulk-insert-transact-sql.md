---
title: BULK INSERT (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la instrucción BULK INSERT.
ms.date: 02/21/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BULK_TSQL
- BULK_INSERT
- BULK_INSERT_TSQL
- BULK INSERT
dev_langs:
- TSQL
helpviewer_keywords:
- tables [SQL Server], importing data into
- inserting files
- views [SQL Server], importing data into
- BULK INSERT statement
- views [SQL Server], exporting data from
- importing data, bulk import
- bulk importing [SQL Server], BULK INSERT statement
- file importing [SQL Server]
ms.assetid: be3984e1-5ab3-4226-a539-a9f58e1e01e2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 1d1659f427386fd8fa807bc10e782eca572a2278
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100342550"
---
# <a name="bulk-insert-transact-sql"></a>BULK INSERT (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Importa un archivo de datos en una tabla o vista de base de datos con un formato especificado por el usuario en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
BULK INSERT
   { database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }
      FROM 'data_file'
     [ WITH
    (
   [ [ , ] BATCHSIZE = batch_size ]
   [ [ , ] CHECK_CONSTRAINTS ]
   [ [ , ] CODEPAGE = { 'ACP' | 'OEM' | 'RAW' | 'code_page' } ]
   [ [ , ] DATAFILETYPE =
      { 'char' | 'native'| 'widechar' | 'widenative' } ]
   [ [ , ] DATA_SOURCE = 'data_source_name' ]
   [ [ , ] ERRORFILE = 'file_name' ]
   [ [ , ] ERRORFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] FIRSTROW = first_row ]
   [ [ , ] FIRE_TRIGGERS ]
   [ [ , ] FORMATFILE_DATA_SOURCE = 'data_source_name' ]
   [ [ , ] KEEPIDENTITY ]
   [ [ , ] KEEPNULLS ]
   [ [ , ] KILOBYTES_PER_BATCH = kilobytes_per_batch ]
   [ [ , ] LASTROW = last_row ]
   [ [ , ] MAXERRORS = max_errors ]
   [ [ , ] ORDER ( { column [ ASC | DESC ] } [ ,...n ] ) ]
   [ [ , ] ROWS_PER_BATCH = rows_per_batch ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
   [ [ , ] TABLOCK ]

   -- input file format options
   [ [ , ] FORMAT = 'CSV' ]
   [ [ , ] FIELDQUOTE = 'quote_characters']
   [ [ , ] FORMATFILE = 'format_file_path' ]
   [ [ , ] FIELDTERMINATOR = 'field_terminator' ]
   [ [ , ] ROWTERMINATOR = 'row_terminator' ]
    )]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

*database_name* Es el nombre de la base de datos donde reside la tabla o la vista especificada. Si no se especifica, es la base de datos actual.

*schema_name* Es el nombre del esquema de la tabla o vista. *schema_name* es opcional si el esquema predeterminado para el usuario que realiza la operación de importación masiva es el esquema de la tabla o vista especificada. Si no se especifica *schema* y el esquema predeterminado del usuario que realiza la operación de importación masiva es diferente de la tabla o la vista especificada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un mensaje de error y se cancela la operación de importación masiva.

*table_name* Es el nombre de la tabla o vista en la que se va a realizar una importación en bloque de datos. Solo se pueden utilizar vistas en las que todas las columnas hagan referencia a la misma tabla base. Para más información sobre las restricciones para cargar datos en vistas, vea [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md).

**"** _data_file_ **"** Es la ruta de acceso completa al archivo de datos que contiene los datos que se van a importar en la tabla o vista especificada. BULK INSERT puede importar datos desde un disco o Azure Blob Storage (incluidos una ubicación de red, disquete, disco duro, etc.).

*data_file* debe especificar una ruta de acceso válida del servidor en el que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si *data_file* es un archivo remoto, especifique un nombre UNC (convención de nomenclatura universal). Un nombre UNC tiene el formato \\\\*Systemname*\\*ShareName*\\*Path*\\*FileName*. Por ejemplo:

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.dat';
```

**Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 y Azure SQL Database.
A partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1, data_file puede estar en Azure Blob Storage. En ese caso, deberá especificar la opción **data_source_name**. Para obtener un ejemplo, vea [Importación de datos desde un archivo en Azure Blob Storage](#f-importing-data-from-a-file-in-azure-blob-storage).

> [!IMPORTANT]
> Azure SQL Database solo admite la lectura desde Azure Blob Storage.

**"** _data_source_name_ **"** 
**Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 y Azure SQL Database.
Es un origen de datos externo con nombre que apunta a la ubicación de Azure Blob Storage del archivo que se importará. El origen de datos externo se debe crear con la opción `TYPE = BLOB_STORAGE` que se ha incluido en [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1. Para más información, vea [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md). Para obtener un ejemplo, vea [Importación de datos desde un archivo en Azure Blob Storage](#f-importing-data-from-a-file-in-azure-blob-storage).

BATCHSIZE **=** _tamaño_de_lote_ Especifica el número de filas de un lote. Cada lote se copia en el servidor como una transacción. Si no ocurre así, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] confirma o revierte la transacción de cada lote. De forma predeterminada, todos los datos del archivo de datos especificado componen un lote. Para obtener información acerca de consideraciones de rendimiento, vea la sección "Comentarios" más adelante en este tema.

CHECK_CONSTRAINTS Especifica que se deben comprobar todas las restricciones de la tabla o vista de destino durante la operación de importación en bloque. Sin la opción CHECK_CONSTRAINTS, se omiten las restricciones CHECK y FOREIGN KEY, y, después de la operación, la restricción sobre la tabla se marca como de no confianza.

> [!NOTE]
> Las restricciones UNIQUE y PRIMARY KEY se aplican siempre. Cuando se importa en una columna de caracteres definida con la restricción NOT NULL, BULK INSERT inserta una cadena vacía cuando no hay valor en el archivo de texto.

En algún momento, debe examinar las restricciones de toda la tabla. Si la tabla no estaba vacía antes de la operación de importación masiva, el costo de revalidar la restricción puede superar del costo de aplicar restricciones CHECK a los datos incrementales.

Una situación en la que quizá desee que las restricciones estén deshabilitadas (comportamiento predeterminado) es si los datos de entrada contienen filas que infringen las restricciones. Con las restricciones CHECK deshabilitadas, puede importar los datos y utilizar después instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] para quitar los datos no válidos.

> [!NOTE]
> La opción MAXERRORS no se aplica a la comprobación de restricciones.

CODEPAGE **=** { **'** ACP **'** \| **'** OEM **'** \| **'** RAW **'** \| **'** _code_page_ **'** } Especifica la página de códigos de los datos del archivo de datos. CODEPAGE solo es pertinente si los datos contienen columnas de tipo **char**, **varchar** o **text** con valores de caracteres mayores que **127** o menores que **32**. Para obtener un ejemplo, vea [Especificación de una página de códigos](#d-specifying-a-code-page).

> [!IMPORTANT]
> CODEPAGE no es una opción admitida en [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]. Para [!INCLUDE[ssSQLv15_md](../../includes/sssql19-md.md)], solo la opción **"RAW"** se permite para CODEPAGE.

> [!NOTE]
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] recomienda especificar un nombre de intercalación para cada columna de un [archivo de formato](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md).

|Valor de CODEPAGE|Descripción|
|--------------------|-----------------|
|ACP|Convierte columnas de los tipos de datos **char**, **varchar** o **text** de la página de códigos [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)]/[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (ISO 1252) a la página de códigos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|OEM (valor predeterminado)|Convierte columnas de los tipos de datos **char**, **varchar** o **text** de la página de códigos OEM a la página de códigos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|
|RAW|No se realiza ninguna conversión de una página de códigos a otra; se trata de la opción más rápida.|
|*code_page*|Número específico de una página de códigos; por ejemplo, 850.<br /><br /> **&#42;&#42;Importante&#42;&#42;** Las versiones anteriores a [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] no admiten la página de códigos 65001 (codificación UTF-8).|
| &nbsp; | &nbsp; |

DATAFILETYPE **=** { **'char'** \| **'native'** \| **'widechar'** \| **'widenative'** } Especifica que BULK INSERT realiza la operación de importación con el valor del tipo de archivo de datos especificado.

&nbsp;

|Valor de DATAFILETYPE|Todos los datos representados en:|
|------------------------|------------------------------|
|**char** (valor predeterminado)|Formato de caracteres.<br /><br /> Para obtener más información, vea [Usar el formato de caracteres para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).|
|**native**|Tipos de datos nativos (base de datos). Cree el archivo de datos nativos mediante la importación masiva de datos desde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la utilidad **bcp**.<br /><br /> El valor native ofrece una alternativa de mayor rendimiento al valor char. Se recomienda usar el formato nativo cuando se realiza una transferencia de datos en bloque entre varias instancias de SQL Server mediante un archivo de datos que no contiene juegos de caracteres extendidos o de doble byte (DBCS).<br /><br /> Para obtener más información, vea [Usar el formato nativo para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).|
|**widechar**|Caracteres Unicode.<br /><br /> Para obtener más información, vea [Usar el formato de caracteres Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).|
|**widenative**|Tipos de datos nativos (base de datos), salvo en las columnas **char**, **varchar** y **text**, en las que los datos se almacenan como datos Unicode. Cree el archivo de datos **widenative** mediante la importación masiva de datos desde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la utilidad **bcp**.<br /><br /> El valor **widenative** ofrece una alternativa de mayor rendimiento a **widechar**. Si el archivo de datos contiene caracteres extendidos [!INCLUDE[vcpransi](../../includes/vcpransi-md.md)], especifique **widenative**.<br /><br /> Para obtener más información, vea [Usar el formato nativo Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).|
| &nbsp; | &nbsp; |

ERRORFILE **="** _nombre_de_archivo_ **"** Especifica el archivo que se usa para recopilar filas que tienen errores de formato y no se pueden convertir en un conjunto de filas OLE DB. Estas filas se copian en este archivo de errores desde el archivo de datos "tal cual".

El archivo de errores se crea cuando se ejecuta el comando. Se produce un error si el archivo ya existe. Además, se crea un archivo de control con la extensión .ERROR.txt. Este archivo hace referencia a cada fila del archivo de errores y proporciona diagnósticos de errores. Tan pronto como se corrigen los errores, se pueden cargar los datos.
**Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
A partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)], `error_file_path` puede estar en Azure Blob Storage.

"errorfile_data_source_name" **Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Es un origen de datos externo con nombre que apunta a la ubicación de Azure Blob Storage del archivo de error que contendrá los errores encontrados durante la importación. El origen de datos externo se debe crear con la opción `TYPE = BLOB_STORAGE` que se ha incluido en [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1. Para más información, vea [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

FIRSTROW **=** _first_row_ Especifica el número de la primera fila que se va a cargar. El valor predeterminado es la primera fila del archivo de datos especificado. FIRSTROW está en base 1.

> [!NOTE]
> El atributo FIRSTROW no está pensado para saltar los encabezados de columna. La instrucción BULK INSERT no permite omitir los encabezados. Al omitir filas, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] solo analiza los terminadores de campo y no valida los datos en los campos de las filas omitidas.

FIRE_TRIGGERS Especifica que todos los desencadenadores de inserción definidos en la tabla de destino se ejecutarán durante la operación de importación en bloque. Si se definen desencadenadores para operaciones INSERT en la tabla de destino, se activan para cada lote completado.

Si no se especifica FIRE_TRIGGERS, no se ejecuta ningún desencadenador de inserción.

FORMATFILE_DATA_SOURCE **=** "nombre_del_origen_de_datos" **Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] 1.1.
Es un origen de datos externo con nombre que apunta a la ubicación de Azure Blob Storage del archivo de formato que definirá el esquema de los datos importados. El origen de datos externo se debe crear con la opción `TYPE = BLOB_STORAGE` que se ha incluido en [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1. Para más información, vea [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

KEEPIDENTITY Especifica que se usará el valor o valores de identidad del archivo de datos importado para la columna de identidad. Si no se especifica KEEPIDENTITY, los valores de identidad de esta columna se comprueban pero no se importan y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna automáticamente valores únicos basados en los valores de inicialización y de incremento especificados durante la creación de la tabla. Si el archivo de datos no contiene valores para la columna de identidad de la tabla o vista, utilice un archivo de formato para especificar que se debe omitir la columna de identidad de la tabla o vista cuando se importen los datos; [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigna automáticamente valores únicos para la columna. Para obtener más información, vea [DBCC CHECKIDENT &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).

Si quiere saber más sobre cómo mantener valores de identidad, vea [Mantener valores de identidad al importar datos de forma masiva &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md).

KEEPNULLS Especifica que las columnas vacías deben conservar un valor NULL durante la operación de importación en bloque, en lugar de tener valores predeterminados para las columnas insertadas. Para obtener más información, vea [Mantener valores NULL o usar valores predeterminados durante la importación en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).

KILOBYTES_PER_BATCH **=** _kilobytes_per_batch_ Especifica el número aproximado de kilobytes (KB) de datos por lote como *kilobytes_per_batch*. De forma predeterminada, el valor de KILOBYTES_PER_BATCH es desconocido. Para obtener información acerca de consideraciones de rendimiento, vea la sección "Comentarios" más adelante en este tema.

LASTROW **=** _last_row_ Especifica el número de la última fila que se va a cargar. El valor predeterminado es 0, que indica la última fila del archivo de datos especificado.

MAXERRORS **=** _max_errors_ Especifica el número máximo de errores de sintaxis permitidos en los datos antes de cancelar la operación de importación en bloque. Cada fila que no se puede importar con la operación de importación masiva se omite y se considera un error. Si no se especifica *max_errors*, el valor predeterminado es 10.

> [!NOTE]
> La opción MAX_ERRORS no se aplica a comprobaciones de restricciones ni para convertir tipos de datos **money** y **bigint**.

ORDER ( { *columna* [ ASC | DESC ] } [ **,** ... *n* ] ) Especifica cómo se ordenan los datos del archivo de datos. El rendimiento de la importación masiva mejora si los datos importados se ordenan según el índice clúster de la tabla, si lo hay. Si el archivo de datos se ordena siguiendo otro criterio que no sea el orden de una clave de índice agrupado, o si no hay ningún índice agrupado en la tabla, se omite la cláusula ORDER. Los nombres de columna facilitados deben ser nombres válidos en la tabla de destino. De forma predeterminada, la operación de inserción masiva presupone que los datos del archivo no están ordenados. En las importaciones masivas optimizadas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también valida que los datos importados estén ordenados.

*n* Es un marcador de posición que indica que se pueden especificar varias columnas.

ROWS_PER_BATCH **=** _rows_per_batch_ Indica el número aproximado de filas de datos del archivo de datos.

De forma predeterminada, todos los datos del archivo de datos se envían al servidor en una sola transacción y el optimizador de consultas desconoce el número de filas del lote. Si especifica ROWS_PER_BATCH (con el valor > 0) el servidor utiliza este valor para optimizar la operación de importación masiva. El valor especificado para ROWS_PER_BATCH debe ser aproximadamente el mismo que el número real de filas. Para obtener información acerca de consideraciones de rendimiento, vea la sección "Comentarios" más adelante en este tema.

TABLOCK Especifica que se obtiene un bloqueo de nivel de tabla durante la operación de importación en bloque. Varios clientes pueden cargar una tabla simultáneamente si ésta no tiene índices y se especifica TABLOCK. De forma predeterminada, el comportamiento del bloqueo viene determinado por la opción de tabla **table lock on bulk load**. Al mantener un bloqueo durante la operación de importación masiva, se reduce la contención por bloqueos de la tabla y en algunos casos puede mejorarse notablemente el rendimiento. Para obtener información acerca de consideraciones de rendimiento, vea la sección "Comentarios" más adelante en este tema.

Para el índice de almacén de columnas, El comportamiento de bloqueo es distinto porque internamente se divide en varios conjuntos de filas. Cada subproceso carga datos exclusivamente en cada conjunto de filas mediante la aplicación de un bloqueo X en el conjunto de filas, lo que permite cargar datos en paralelo con las sesiones de carga de datos simultáneas. El uso de la opción TABLOCK hará que el subproceso realice un bloqueo X en la tabla (a diferencia del bloqueo de actualización masiva para conjuntos de filas tradicionales) que impedirá que otros subprocesos simultáneos carguen datos al mismo tiempo.

### <a name="input-file-format-options"></a>Opciones de formato de archivos de entrada

FORMAT **=** "CSV" **Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Especifica un archivo de valores separados por comas conforme a la norma [RFC 4180](https://tools.ietf.org/html/rfc4180).

```sql
BULK INSERT Sales.Orders
FROM '\\SystemX\DiskZ\Sales\data\orders.csv'
WITH ( FORMAT='CSV');
```

FIELDQUOTE **=** "comillas_de_campo" **Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1.
Especifica un carácter que se usará como carácter de comillas en el archivo CSV. Si no se especifica, se usará el carácter de comillas (") como carácter de comillas, según define la norma [RFC 4180](https://tools.ietf.org/html/rfc4180).

FORMATFILE **=** "_ruta_de_acceso_de_formato_" Especifica la ruta de acceso completa de un archivo de formato. Un archivo de formato describe el archivo de datos que contiene respuestas almacenadas creado con la utilidad **bcp** en la misma tabla o vista. Se debe usar el archivo de formato si:

- El archivo de datos contiene un número de columnas mayor o menor que la tabla o vista.
- Las columnas están en un orden diferente.
- Los delimitadores de columna varían.
- Hay otros cambios en el formato de los datos. Los archivos de formato se suelen crear con la utilidad **bcp** y se modifican con un procesador de texto si es necesario. Para más información, vea [Utilidad bcp](../../tools/bcp-utility.md) y [Creación de un archivo de formato](../../relational-databases/import-export/create-a-format-file-sql-server.md).

**Se aplica a:** [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1 y Azure SQL Database.
A partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1, format_file_path puede estar en Azure Blob Storage.

FIELDTERMINATOR **="** _terminador_de_campo_ **"** Especifica el terminador de campo que se va a usar para archivos de datos de tipo **char** y **widechar**. El terminador de campo predeterminado es \t (tabulador). Para obtener más información, vea [Especificar terminadores de campo y de fila &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

ROWTERMINATOR **="** _terminador_de_fila_ **"** Especifica el terminador de fila que se va a usar para archivos de datos de tipo **char** y **widechar**. El terminador de fila predeterminado es **\r\n** (carácter de nueva línea). Para obtener más información, vea [Especificar terminadores de campo y de fila &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).

## <a name="compatibility"></a>Compatibilidad

BULK INSERT aplica una estricta validación y comprobación de los datos leídos de un archivo que pueden dar lugar a errores en los scripts existentes cuando se ejecutan en datos no válidos. Por ejemplo, BULK INSERT comprueba que:

- Las representaciones nativas de los tipos de datos **float** o **real** son válidas.
- Los datos Unicode tienen una longitud de bytes uniforme.

## <a name="data-types"></a>Tipo de datos

### <a name="string-to-decimal-data-type-conversions"></a>Conversiones de tipos de datos de cadena a decimal

Las conversiones de tipos de datos de cadena a decimal usadas en BULK INSERT siguen las mismas reglas que la función [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) de [!INCLUDE[tsql](../../includes/tsql-md.md)], que rechaza las cadenas que representan valores numéricos con notación científica. Por lo tanto, BULK INSERT trata esas cadenas como valores no válidos y genera errores de conversión.

Para solucionar este comportamiento, use un archivo de formato para la importación masiva de datos de tipo **float** con notación científica en una columna con valores decimales. En el archivo de formato, describa explícitamente la columna como de datos **real** o **float**. Para más información sobre estos tipos de datos, vea [float y real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md).

> [!NOTE]
> Los archivos de formato representan datos **real** como el tipo de datos **SQLFLT4** y datos **float** como el tipo de datos **SQLFLT8**. Para más información sobre archivos de formato distintos de XML, vea [Especificar el tipo de almacenamiento de archivos mediante bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).

#### <a name="example-of-importing-a-numeric-value-that-uses-scientific-notation"></a>Ejemplo de importación de un valor numérico que utiliza notación científica

En este ejemplo se utiliza la siguiente tabla:

```sql
CREATE TABLE t_float(c1 FLOAT, c2 DECIMAL (5,4));
```

 El usuario desea importar masivamente datos en la tabla `t_float`. El archivo de datos (C:\t_float-c.dat) contiene datos **float** con notación científica; por ejemplo:

```input
8.0000000000000002E-28.0000000000000002E-2
```

No obstante, BULK INSERT no puede importar estos datos directamente en `t_float`, ya que su segunda columna, `c2`, utiliza el tipo de datos `decimal`. Por lo tanto, es necesario un archivo de formato. El archivo de formato debe asignar los datos **float** con notación científica al formato decimal de la columna `c2`.

El siguiente archivo de formato utiliza el tipo de datos `SQLFLT8` para asignar el segundo campo de datos a la segunda columna:

```xml
<?xml version="1.0"?>
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<RECORD>
<FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="30"/>
<FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\r\n" MAX_LENGTH="30"/> </RECORD> <ROW>
<COLUMN SOURCE="1" NAME="c1" xsi:type="SQLFLT8"/>
<COLUMN SOURCE="2" NAME="c2" xsi:type="SQLFLT8"/> </ROW> </BCPFORMAT>
```

Para utilizar este archivo de formato (con el nombre de archivo `C:\t_floatformat-c-xml.xml`) para importar los datos de prueba en la tabla de prueba, emita la siguiente instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)]:

```sql
BULK INSERT bulktest..t_float
FROM 'C:\t_float-c.dat' WITH (FORMATFILE='C:\t_floatformat-c-xml.xml');
```

> [!IMPORTANT]
> Azure SQL Database solo admite la lectura desde Azure Blob Storage.

### <a name="data-types-for-bulk-exporting-or-importing-sqlxml-documents"></a>Tipos de datos para importar o exportar masivamente documentos SQLXML

Para importar o exportar de forma masiva datos SQLXML, utilice uno de los tipos de datos siguientes en el archivo de formato:

|Tipo de datos|Efecto|
|---------------|------------|
|SQLCHAR o SQLVARCHAR|Los datos se envían a la página de códigos del cliente o a la página de códigos implícita por la intercalación. El efecto es el mismo que si se especifica DATAFILETYPE **='char'** sin especificar un archivo de formato.|
|SQLNCHAR o SQLNVARCHAR|Los datos se envían como datos Unicode. El efecto es el mismo que si se especifica DATAFILETYPE **= 'widechar'** sin especificar un archivo de formato.|
|SQLBINARY o SQLVARBIN|Los datos se envían sin realizar ninguna conversión.|
| &nbsp; | &nbsp; |

## <a name="general-remarks"></a>Notas generales

Para obtener una comparación de la instrucción BULK INSERT, la instrucción INSERT ... Para saber más sobre la instrucción SELECT \* FROM OPENROWSET(BULK...) y el comando **bcp**, vea [Importar y exportar datos de forma masiva &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md).

Para más información sobre cómo preparar datos para importaciones masivas, vea [Preparar los datos para exportar o importar de forma masiva &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

La instrucción BULK INSERT se puede ejecutar en una transacción definida por el usuario para importar datos en una tabla o una vista. Opcionalmente, para utilizar varias coincidencias para la importación masiva de datos, una transacción puede especificar la cláusula BATCHSIZE en la instrucción BULK INSERT. Si una transacción de varios lotes se revierte, cada lote que la transacción ha enviado a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se revierte.

## <a name="interoperability"></a>Interoperabilidad

### <a name="importing-data-from-a-csv-file"></a>Importar datos desde un archivo CSV

A partir de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1, BULK INSERT admite el formato CSV, al igual que Azure SQL Database.
Antes de [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] CTP 1.1, las operaciones de importación masiva [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no admiten archivos de valores separados por comas (CSV). Sin embargo, en algunos casos se puede utilizar un archivo de valores separados por comas (CSV) como archivo de datos para una importación masiva de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para conocer los requisitos para importar datos desde un archivo de datos CSV, vea [Preparar los datos para exportar o importar de forma masiva &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

## <a name="logging-behavior"></a>Comportamiento del registro

 Para más información sobre cuándo se incluyen en el registro de transacciones las operaciones de inserción de filas realizadas durante una importación en bloque a SQL Server, consulte [Requisitos previos para el registro mínimo durante la importación en bloque](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md). En Azure SQL Database no se admite el registro mínimo.

## <a name="restrictions"></a><a name="Limitations"></a> Restricciones

Cuando se usa un archivo de formato con BULK INSERT, solo se puede especificar un máximo de 1024 campos. Es el mismo número máximo de columnas permitido en una tabla. Si usa un archivo de formato con BULK INSERT con un archivo de datos que contenga más de 1024 campos, BULK INSERT genera el error 4822. La [utilidad bcp](../../tools/bcp-utility.md) no tiene esta limitación, por lo que para los archivos de datos que contengan más de 1024 campos, use BULK INSERT sin un archivo de formato, o bien use el comando **bcp**.

## <a name="performance-considerations"></a>Consideraciones de rendimiento

Si el número de páginas que van a vaciarse en un único lote supera un umbral interno, podría producirse un examen completo del grupo de búferes para identificar qué páginas se han de vaciar cuando el lote se confirme. Este examen completo puede afectar de forma desfavorable al rendimiento de la importación masiva. Un caso en el que es probable que se supere el umbral interno se produce cuando un grupo de búferes grande se combina con un subsistema de E/S lento. Para evitar los desbordamientos del búfer en equipos grandes, no utilice la sugerencia TABLOCK (que quita la optimización masiva) o use un tamaño de lote menor (que la preserva).

Dado que los equipos varían, es recomendable que pruebe varios tamaños de lote con la carga de datos para averiguar lo que funciona mejor en su caso.

Con Azure SQL Database, considere la posibilidad de aumentar temporalmente el nivel de rendimiento de la base de datos o la instancia antes de la importación si va a importar un gran volumen de datos.

## <a name="security"></a>Seguridad

### <a name="security-account-delegation-impersonation"></a>Delegación de cuentas de seguridad (suplantación)

Si un usuario utiliza un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se utilizará el perfil de seguridad de la cuenta de proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un inicio de sesión que use autenticación de SQL Server no se puede autenticar fuera del Motor de base de datos. Por tanto, cuando un inicio de sesión que usa autenticación de SQL Server inicia un comando BULK INSERT, la conexión con los datos se realiza usando el contexto de seguridad de la cuenta de proceso de SQL Server (la cuenta usada por el servicio Motor de base de datos de SQL Server). Para leer correctamente los datos de origen, debe conceder acceso a los datos de origen a la cuenta usada por el Motor de base de datos de SQL Server. Por el contrario, si un usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inicia sesión con autenticación de Windows, el usuario solo puede leer aquellos archivos a los que tenga acceso la cuenta de usuario, cualquiera que sea el perfil de seguridad del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Es posible que reciba un error 4861 si ejecuta la instrucción BULK INSERT usando **sqlcmd** o **osql** desde un equipo, insertando datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en otro equipo y especificando *data_file* en un tercer equipo mediante una ruta de acceso UNC.

Para resolver este error, use la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y especifique un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que use el perfil de seguridad de la cuenta del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], o bien configure Windows para habilitar la delegación de la cuenta de seguridad. Para obtener información acerca de cómo habilitar una cuenta de usuario para que sea de confianza para la delegación, vea la Ayuda de Windows.

Para más información sobre esto y otras consideraciones de seguridad, vea [Importación en bloque de datos mediante las instrucciones BULK INSERT u OPENROWSET&#40;BULK...&#41; &#40;SQL Server&#41;](../../relational-databases/import-export/import-bulk-data-by-using-bulk-insert-or-openrowset-bulk-sql-server.md).

Cuando se importa desde Azure Blob Storage y los datos no son públicos (acceso anónimo), cree una instancia de [DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md) basada en una clave SAS que esté cifrada con [MASTER KEY](create-master-key-transact-sql.md) y, después, cree un [origen de base de datos externo](../../t-sql/statements/create-external-data-source-transact-sql.md) para usarlo en el comando BULK INSERT. Para obtener un ejemplo, vea [Importación de datos desde un archivo en Azure Blob Storage](#f-importing-data-from-a-file-in-azure-blob-storage).

### <a name="permissions"></a>Permisos

Se requieren los permisos INSERT y ADMINISTER BULK OPERATIONS. En Azure SQL Database, se necesitan los permisos INSERT y ADMINISTER DATABASE BULK OPERATIONS. No se admiten los permisos ADMINISTER BULK OPERATIONS o el rol bulkadmin para SQL Server en Linux. Solo `sysadmin` puede realizar inserciones masivas para SQL Server en Linux. 

Además, es necesario el permiso ALTER TABLE si se da una o varias de las siguientes circunstancias:

- Existen restricciones y no se ha especificado la opción CHECK_CONSTRAINTS.

   > [!NOTE]
   > La deshabilitación de restricciones es el comportamiento predeterminado. Para comprobar las restricciones CHECK explícitamente, utilice la opción CHECK_CONSTRAINTS.

- Existen desencadenadores y no se ha especificado la opción FIRE_TRIGGER.

   > [!NOTE]
   > De forma predeterminada, los desencadenadores no se activan. Para activar los desencadenadores explícitamente, use la opción FIRE_TRIGGER.

- Se utiliza la opción KEEPIDENTITY para importar el valor de identidad de un archivo de datos.

## <a name="examples"></a>Ejemplos

### <a name="a-using-pipes-to-import-data-from-a-file"></a>A. Usar canalizaciones para importar datos de un archivo

En el siguiente ejemplo se importa información detallada de pedidos en la tabla `AdventureWorks2012.Sales.SalesOrderDetail` desde un archivo de datos especificado utilizando una canalización (`|`) como el terminador de campo y `|\n` como el terminador de fila.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
      (  
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR =' |\n'
      );
```

> [!IMPORTANT]
> Azure SQL Database solo admite la lectura desde Azure Blob Storage.

### <a name="b-using-the-fire_triggers-argument"></a>B. Usar el argumento FIRE_TRIGGERS

En el ejemplo siguiente se especifica el argumento `FIRE_TRIGGERS`.

```sql
BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
   FROM 'f:\orders\lineitem.tbl'
   WITH
     (
         FIELDTERMINATOR =' |'
         , ROWTERMINATOR = ':\n'
         , FIRE_TRIGGERS
      );
```

> [!IMPORTANT]
> Azure SQL Database solo admite la lectura desde Azure Blob Storage.

### <a name="c-using-line-feed-as-a-row-terminator"></a>C. Usar el salto de línea como terminador de fila

 En el siguiente ejemplo se importa un archivo que utiliza el salto de línea como terminador de fila, igual que en una salida de UNIX:

```sql
DECLARE @bulk_cmd VARCHAR(1000);
SET @bulk_cmd = 'BULK INSERT AdventureWorks2012.Sales.SalesOrderDetail
FROM ''<drive>:\<path>\<filename>''
WITH (ROWTERMINATOR = '''+CHAR(10)+''')';
EXEC(@bulk_cmd);
```

> [!NOTE]
> Debido al modo en que Microsoft Windows trata los archivos de texto, **(\n** se reemplaza automáticamente por **\r\n)** .

> [!IMPORTANT]
> Azure SQL Database solo admite la lectura desde Azure Blob Storage.

### <a name="d-specifying-a-code-page"></a>D. Especificar una página de códigos

En este ejemplo se muestra cómo especificar una página de código.

```sql
BULK INSERT MyTable
FROM 'D:\data.csv'
WITH
( CODEPAGE = '65001'
   , DATAFILETYPE = 'char'
   , FIELDTERMINATOR = ','
);
```

> [!IMPORTANT]
> Azure SQL Database solo admite la lectura desde Azure Blob Storage.

### <a name="e-importing-data-from-a-csv-file"></a>E. Importar datos desde un archivo CSV

En el ejemplo siguiente se muestra cómo especificar un archivo CSV, omitir el encabezado (primera fila), utilizar `;` como terminador de campo y `0x0a` como terminador de línea:

```sql
BULK INSERT Sales.Invoices
FROM '\\share\invoices\inv-2016-07-25.csv'
WITH (FORMAT = 'CSV'
      , FIRSTROW=2
      , FIELDQUOTE = '\'
      , FIELDTERMINATOR = ';'
      , ROWTERMINATOR = '0x0a');
```

> [!IMPORTANT]
> Azure SQL Database solo admite la lectura desde Azure Blob Storage.

### <a name="f-importing-data-from-a-file-in-azure-blob-storage"></a>F. Importar datos desde un archivo en Azure Blog Storage

En el ejemplo siguiente se muestra cómo cargar datos desde un archivo csv en una ubicación de Azure Blob Storage en la que se ha creado una clave SAS. La ubicación de Azure Blob Storage está configurada como origen de datos externo. Esto requiere credenciales con ámbito de base de datos mediante una firma de acceso compartido que se cifra con una clave maestra en la base de datos de usuario.

```sql
--> Optional - a MASTER KEY is not required if a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'YourStrongPassword1';
GO
--> Optional - a DATABASE SCOPED CREDENTIAL is not required because the blob is configured for public (anonymous) access!
CREATE DATABASE SCOPED CREDENTIAL MyAzureBlobStorageCredential
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=2016-12-29T16:55:34Z***************';

 -- NOTE: Make sure that you don't have a leading ? in SAS token, and
 -- that you have at least read permission on the object that should be loaded srt=o&sp=r, and
 -- that expiration period is valid (all dates are in UTC time)

CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
WITH ( TYPE = BLOB_STORAGE,
          LOCATION = 'https://****************.blob.core.windows.net/invoices'
          , CREDENTIAL= MyAzureBlobStorageCredential --> CREDENTIAL is not required if a blob is configured for public (anonymous) access!
);

BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (DATA_SOURCE = 'MyAzureBlobStorage');
```

> [!IMPORTANT]
> Azure SQL solo admite la lectura mediante Azure Blob Storage.

### <a name="g-importing-data-from-a-file-in-azure-blob-storage-and-specifying-an-error-file"></a>G. Importar datos desde un archivo en Azure Blob Storage y especificar un archivo de error

En este ejemplo se muestra cómo cargar datos desde un archivo csv en una ubicación de Azure Blob Storage, que se ha configurado como un origen de datos externo, y también cómo especificar un archivo de error. Para hacerlo, se necesita una credencial de ámbito de base de datos que use una firma de acceso compartido. Observe que si se ejecuta en Azure SQL Database, la opción ERRORFILE debe ir acompañada de ERRORFILE_DATA_SOURCE; de lo contrario, la importación podría presentar un error de permisos. El archivo especificado en ERRORFILE no debe existir en el contenedor.

```sql
BULK INSERT Sales.Invoices
FROM 'inv-2017-12-08.csv'
WITH (
         DATA_SOURCE = 'MyAzureInvoices'
         , FORMAT = 'CSV'
         , ERRORFILE = 'MyErrorFile'
         , ERRORFILE_DATA_SOURCE = 'MyAzureInvoices');
```

Para ver ejemplos completos de `BULK INSERT`, incluido cómo configurar la credencial y el origen de datos externo, vea [Ejemplos de acceso masivo a datos en Azure Blob Storage](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md).

### <a name="additional-examples"></a>Otros ejemplos

 Se proporcionan otros ejemplos de `BULK INSERT` en estos temas:

- [Ejemplos de importación y exportación en bloque de documentos XML &#40;SQL Server&#41;](../../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)
- [Mantener valores de identidad al importar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)
- [Mantener valores NULL o usar valores predeterminados durante la importación en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)
- [Especificar terminadores de campo y de fila &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)
- [Usar un archivo de formato para importar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)
- [Usar el formato de caracteres para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)
- [Usar el formato nativo para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)
- [Usar el formato de caracteres Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)
- [Usar el formato nativo Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)
- [Usar un archivo de formato para omitir una columna de tabla &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)
- [Usar un archivo de formato para asignar columnas de tabla a campos de un archivo de datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)

## <a name="see-also"></a>Consulte también

- [Importar y exportar datos en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)
- [bcp (utilidad)](../../tools/bcp-utility.md)
- [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)
- [Preparar los datos para exportar o importar en bloque &#40;SQL Server&#41;](../../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [sp_tableoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)