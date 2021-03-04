---
description: CREATE EXTERNAL FILE FORMAT (Transact-SQL)
title: CREATE EXTERNAL FILE FORMAT (Transact-SQL)
ms.custom: ''
ms.date: 02/23/2021
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE EXTERNAL FILE FORMAT
- CREATE_EXTERNAL_FILE_FORMAT
dev_langs:
- TSQL
helpviewer_keywords:
- External
- External, file format
- PolyBase, external file format
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 458641983730dbec51055c7f610b7775acd09ac8
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837727"
---
# <a name="create-external-file-format-transact-sql"></a>CREATE EXTERNAL FILE FORMAT (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

Crea un objeto Formato de archivo externo que define datos externos almacenados en Hadoop, Azure Blob Storage, Azure Data Lake Store o para los flujos de entrada y salida asociados con flujos externos. La creación de un formato de archivo externo es un requisito previo para crear una tabla externa. Al crear un formato de archivo externo, se especifica el diseño real de los datos a los que hace referencia una tabla externa.  
  
Se admiten los formatos de archivo siguientes:
  
- Texto delimitado  
  
- Hive RCFile: no se aplica a Azure Synapse Analytics.
  
- Hive ORC
  
- Parquet

- JSON: solo se aplica a Azure SQL Edge.

Para crear una tabla externa, vea [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md).
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis
  
### <a name="delimited-text"></a>[Texto delimitado](#tab/delimited)
```syntaxsql
-- Create an external file format for DELIMITED (CSV/TSV) files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
        FORMAT_TYPE = DELIMITEDTEXT  
    [ , FORMAT_OPTIONS ( <format_options> [ ,...n  ] ) ]  
    [ , DATA_COMPRESSION = {  
           'org.apache.hadoop.io.compress.GzipCodec'  
         | 'org.apache.hadoop.io.compress.DefaultCodec'  
        }  
     ]);

<format_options> ::=  
{  
    FIELD_TERMINATOR = field_terminator  
    | STRING_DELIMITER = string_delimiter 
    | First_Row = integer -- ONLY AVAILABLE FOR AZURE SYNAPSE ANALYTICS
    | DATE_FORMAT = datetime_format  
    | USE_TYPE_DEFAULT = { TRUE | FALSE } 
    | Encoding = {'UTF8' | 'UTF16'} 
}
```
### <a name="rc"></a>[RC](#tab/rc)
```syntaxsql
--Create an external file format for RC files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = {  
        'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'  
      | 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'  
    }  
    [ , DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec' ]);
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

### <a name="orc"></a>[ORC](#tab/orc)
```syntaxsql  
--Create an external file format for ORC file.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (
         FORMAT_TYPE = ORC  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.DefaultCodec'      }  
    ]);  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

### <a name="parquet"></a>[Parquet](#tab/parquet)
```syntaxsql
--Create an external file format for PARQUET files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
         FORMAT_TYPE = PARQUET  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      }  
    ]);    
```
### <a name="json"></a>[JSON](#tab/json)
```syntaxsql
-- Create an external file format for JSON files.
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = JSON  
     [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      
      | 'org.apache.hadoop.io.compress.DefaultCodec'  }  
    ]);  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

---
  
## <a name="arguments"></a>Argumentos  
*file_format_name*  
Especifica un nombre para el formato de archivo externo.
 
### <a name="format_type"></a>FORMAT_TYPE 
`FORMAT_TYPE = [ PARQUET | ORC | RCFILE | DELIMITEDTEXT]`

Especifica el formato de los datos externos.
  
- PARQUET Especifica un formato Parquet.
  
- ORC  
  Especifica un formato ORC (Optimized Row Columnar). Esta opción requiere Hive versión 0.11 o superior en el clúster externo de Hadoop. En Hadoop, el formato de archivo ORC ofrece un rendimiento y una comprensión mejores que el formato de archivo RCFILE.

- RCFILE (en combinación con SERDE_METHOD = *SERDE_method*) Especifica un formato de archivo RcFile (Record Columnar). Esta opción exige especificar un método Serializer y Deserializer (SerDe) de Hive. Este requisito es el mismo si se usa Hive/HiveQL en Hadoop para consultar archivos RC. Tenga en cuenta que el método SerDe distingue mayúsculas de minúsculas.

  Ejemplos de especificación de RCFile con los dos métodos SerDe que admite PolyBase.

  - FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe'

  - FORMAT_TYPE = RCFILE, SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'

- DELIMITEDTEXT Especifica un formato de texto con delimitadores de columna, también denominados terminadores de campo.
   
- JSON Especifica un formato JSON. Solo se aplica a Azure SQL Edge. 

### <a name="data_compression"></a>DATA\_COMPRESSION
 `DATA_COMPRESSION = *data_compression_method*`  
 Especifica el método de compresión de datos de los datos externos. Si no se especifica DATA_COMPRESSION, el valor predeterminado es datos sin comprimir.
Para que funcionen correctamente, los archivos comprimidos de Gzip deben tener la extensión de archivo ".gz".
 
 #### <a name="delimited-text"></a>[Texto delimitado](#tab/delimited)
 El tipo de formato DELIMITEDTEXT admite estos métodos de compresión:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'

#### <a name="rc"></a>[RC](#tab/rc)
 El tipo de formato RCFILE admite estos métodos de compresión:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
  
#### <a name="orc"></a>[ORC](#tab/orc)
 El tipo de formato ORC admite estos métodos de compresión:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

#### <a name="parquet"></a>[Parquet](#tab/parquet)
 El tipo de formato PARQUET admite estos métodos de compresión:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

#### <a name="json"></a>[JSON](#tab/json)
 El tipo de formato de archivo JSON admite estos métodos de compresión:
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
-   DATA COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'
---  
### <a name="delimited-text-format-options"></a>Opciones de formato de texto delimitado

Las opciones de formato descritas en esta sección son opcionales y solo se aplican a archivos de texto delimitado.

#### <a name="field_terminator"></a>FIELD_TERMINATOR
`FIELD_TERMINATOR = *field_terminator*`  
Se aplica solo a los archivos de texto delimitado. El terminador de campo especifica uno o varios caracteres que marcan el final de cada campo (columna) en el archivo de texto delimitado. El valor predeterminado es el carácter de barra vertical ꞌ|ꞌ. Para garantizar la compatibilidad se recomienda usar uno o más caracteres ascii.
  
  
Ejemplos:  
  
-   FIELD_TERMINATOR = '|'  
  
-   FIELD_TERMINATOR = ' '  
  
-   FIELD_TERMINATOR = ꞌ\tꞌ  
  
-   FIELD_TERMINATOR = '~|~'  
  
#### <a name="string_delimiter"></a>STRING_DELIMITER
`STRING_DELIMITER = *string_delimiter*`

Especifica el terminador de campo de los datos de tipo cadena en el archivo de texto delimitado. El delimitador de cadena tiene una longitud de uno o más caracteres y se escribe entre comillas simples. El valor predeterminado es la cadena vacía "". Para garantizar la compatibilidad se recomienda usar uno o más caracteres ascii.
 
  
 Ejemplos:  

-   STRING_DELIMITER = '"'

-   STRING_DELIMITER = '0x22' -- hexadecimal de comilla doble
  
-   STRING_DELIMITER = '*'  
  
-   STRING_DELIMITER = ꞌ,ꞌ  
  
-   STRING_DELIMITER = '0x7E0x7E' -- dos tildes (por ejemplo, ~~)
 
#### <a name="first_row"></a>FIRST_ROW
 `FIRST_ROW = *First_row_int*`  
Especifica el número de filas que se leen en primer lugar en todos los archivos durante una carga de PolyBase. Este parámetro puede tener los valores comprendidos entre 1 y 15. Si el valor se establece en dos, se omite la primera fila de cada archivo (fila de encabezado) al cargar los datos. Las filas se omiten en función de la existencia de terminadores de fila (/r/n, /r, /n). Cuando esta opción se usa para la exportación, se agregan filas a los datos para asegurarse de que el archivo pueda leerse sin pérdida de datos. Si el valor se establece en >2, la primera fila exportada son los nombres de columna de la tabla externa.

#### <a name="date_format"></a>DATE\_FORMAT
 `DATE_FORMAT = *datetime_format*`  
Especifica un formato personalizado para todos los datos de fecha y hora que pueden aparecer en un archivo de texto delimitado. Si el archivo de origen usa formatos de fecha y hora predeterminados, esta opción no es necesaria. Solo se permite un formato de fecha y hora personalizado por archivo. No se puede especificar más de un formato de fecha y hora personalizado por archivo. Pero puede usar más de un formato de fecha y hora si cada uno de ellos es el predeterminado de su tipo de datos respectivo en la definición de la tabla externa.

> [!IMPORTANT]
> PolyBase solo usa el formato de fecha personalizado para importar los datos. No lo usa para escribir datos en un archivo externo.

 Si DATE_FORMAT no se especifica o es una cadena vacía, PolyBase usa los formatos predeterminados siguientes:
  
-   DateTime: 'aaaa-MM-dd HH:mm:ss'  
  
-   SmallDateTime: 'aaaa-MM-dd HH:mm'  
  
-   Date: 'aaaa-MM-dd'  
  
-   DateTime2: 'aaaa-MM-dd HH:mm:ss'  
  
-   DateTimeOffset: 'aaaa-MM-dd HH:mm:ss'  
  
-   Hora: "HH:mm:ss"  

> [!IMPORTANT]
> La especificación de `DATE_FORMAT` personalizado invalidará todos los formatos de tipo predeterminados. Esto significa que deberá tener los mismos formatos de fecha en todas las celdas de datetime, fecha y hora de sus archivos. Con el `DATE_FORMAT` invalidado, no podrá tener los valores de fecha y hora en un formato distinto.

Los **formatos de fecha de ejemplo** se encuentran en la tabla siguiente:
  
Notas sobre la tabla:  
  
-   El año, el mes y el día pueden adoptar distintos formatos y órdenes. La tabla muestra solo el formato **amd**. El mes puede tener uno o dos dígitos, o tres caracteres. El día puede tener uno o dos dígitos. El año puede tener dos o cuatro dígitos.
  
-   Los milisegundos (fffffff) no son necesarios.
  
-   a.m. y p.m. (tt) no son necesarios. El valor predeterminado es a.m.
  
|Tipo de fecha|Ejemplo|Descripción|  
|---------------|-------------|-----------------|  
|DateTime|DATE_FORMAT = 'aaaa-MM-dd HH:mm:ss.fff'|Además de año, mes y día, este formato de fecha incluye 00-24 horas, 00-59 minutos, 00-59 segundos y 3 dígitos para los milisegundos.|  
|DateTime|DATE_FORMAT = 'aaaa-MM-dd hh:mm:ss.ffftt'|Además de año, mes y día, este formato de fecha incluye 00-12 horas, 00-59 minutos, 00-59 segundos, 3 dígitos para los milisegundos y a.m. o p.m. |  
|SmallDateTime|DATE_FORMAT =  'aaaa-MM-dd HH:mm'|Además de año, mes y día, este formato de fecha incluye 00-23 horas, 00-59 minutos.|  
|SmallDateTime|DATE_FORMAT =  'aaaa-MM-dd hh:mmtt'|Además de año, mes y día, este formato de fecha incluye 00-11 horas, 00-59 minutos, sin segundos y a.m. o p.m.|  
|Date|DATE_FORMAT =  'aaaa-MM-dd'|Año, mes y día. No se incluye ningún elemento de hora.|  
|Date|DATE_FORMAT = 'aaaa-MMM-dd'|Año, mes y día. Cuando se especifica el mes con tres emes, el valor de entrada es una de las cadenas siguientes: ene, feb, mar, abr, may, jun, jul, ago, sep, oct, nov o dic.|  
|DateTime2|DATE_FORMAT = 'aaaa-MM-dd HH:mm:ss.fffffff'|Además de año, mes y día, este formato de fecha incluye 00-23 horas, 00-59 minutos, 00-59 segundos y 7 dígitos para los milisegundos.|  
|DateTime2|DATE_FORMAT = 'aaaa-MM-dd hh:mm:ss.ffffffftt'|Además de año, mes y día, este formato de fecha incluye 00-11 horas, 00-59 minutos, 00-59 segundos, 7 dígitos para los milisegundos y a.m. o p.m.|  
|DateTimeOffset|DATE_FORMAT = 'aaaa-MM-dd HH:mm:ss.fffffff zzz'|Además de año, mes y día, este formato de fecha incluye 00-23 horas, 00-59 minutos, 00-59 segundos y 7 dígitos para los milisegundos, además del desfase de zona horaria incluido en el archivo de entrada como `{+&#124;-}HH:ss`. Por ejemplo, puesto que la hora de Los Ángeles sin horario de verano aplicado es 8 horas por delante de la hora UTC, un valor de -08:00 en el archivo de entrada especifica la zona horaria de Los Ángeles.|  
|DateTimeOffset|DATE_FORMAT = 'aaaa-MM-dd hh:mm:ss.ffffffftt zzz'|Además de año, mes y día, este formato de fecha incluye 00-11 horas, 00-59 minutos, 00-59 segundos, 7 dígitos para los milisegundos, (a.m. o p.m.) y el desfase de zona horaria. Vea la descripción de la fila anterior.|  
|Time|DATE_FORMAT = 'HH:mm:ss'|No hay ningún valor de fecha, solo 00-23 horas, 00-59 minutos y 00-59 segundos.|  
  
 ##### <a name="supported-date-and-time-formats"></a>Formatos de fecha y hora admitidos
 
 El formato de archivo externo puede describir un gran número de formatos de fecha y hora:
  
|datetime|smalldatetime|date|datetime2|datetimeoffset|  
|--------------|-------------------|----------|---------------|--------------------|  
|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fff]|[M[M]]M-[d]d-[yy]yy HH:mm[:00]|[M[M]]M-[d]d-[yy]yy|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff]|[M[M]]M-[d]d-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm[:00][tt]||[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt]|[M[M]]M-[d]d-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fff]|[M[M]]M-[yy]yy-[d]d HH:mm[:00]|[M[M]]M-[yy]yy-[d]d|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff]|[M[M]]M-[yy]yy-[d]d HH:mm:ss[.fffffff] zzz|  
|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm[:00][tt]||[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt]|[M[M]]M-[yy]yy-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fff]|[yy]yy-[M[M]]M-[d]d HH:mm[:00]|[yy]yy-[M[M]]M-[d]d|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]|[yy]yy-[M[M]]M-[d]d HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm[:00][tt]||[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt]|[yy]yy-[M[M]]M-[d]d hh:mm:ss[.fffffff][tt] zzz|  
|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fff]|[yy]yy-[d]d-[M[M]]M HH:mm[:00]|[yy]yy-[d]d-[M[M]]M|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]|[yy]yy-[d]d-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm[:00][tt]||[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt]|[yy]yy-[d]d-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fff]|[d]d-[M[M]]M-[yy]yy HH:mm[:00]|[d]d-[M[M]]M-[yy]yy|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff]|[d]d-[M[M]]M-[yy]yy HH:mm:ss[.fffffff] zzz|  
|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm[:00][tt]||[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt]|[d]d-[M[M]]M-[yy]yy hh:mm:ss[.fffffff][tt] zzz|  
|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fff]|[d]d-[yy]yy-[M[M]]M HH:mm[:00]|[d]d-[yy]yy-[M[M]]M|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]|[d]d-[yy]yy-[M[M]]M HH:mm:ss[.fffffff]  zzz|  
|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm[:00][tt]||[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt]|[d]d-[yy]yy-[M[M]]M hh:mm:ss[.fffffff][tt] zzz|  
  
 Detalles:  
  
-   Para separar los valores de mes, día y año, puede usar “-”, “/” o “.”. Para simplificar, en la tabla solo se usa el separador “-”.
  
-   Para especificar el mes como texto, use tres o más caracteres. Los meses con uno o dos caracteres se interpretan como un número.
  
-   Para separar valores de hora, use el símbolo ':'.
  
-   Las letras entre corchetes son opcionales.
  
-   Las letras "tt" designan [a.m. | p.m.]. a.m. es el valor predeterminado. Cuando se especifica 'tt', el valor de hora (hh) debe estar comprendido entre 0 y 12.
  
-   Las letras 'zzz' designan el desfase de zona horaria de la zona horaria actual del sistema en el formato {+|-}HH:ss].
 
#### <a name="use_type_default"></a>USE_TYPE_DEFAULT 
 `USE_TYPE_DEFAULT = { TRUE | FALSE }`
 
 Especifica cómo administrar valores que faltan en archivos de texto delimitado cuando PolyBase recupera datos del archivo de texto.
  
 TRUE  
 Al recuperar datos del archivo de texto, cada valor que falta se almacena mediante el valor predeterminado del tipo de datos de la columna correspondiente en la definición de la tabla externa. Por ejemplo, reemplace un valor que falta con:  
  
-   0 si la columna se define como una columna numérica. Las columnas decimales no se admiten y generarán un error.
  
-   Cadena vacía "" si la columna es una columna de cadena.
  
-   1900-01-01 si la columna es una columna de fecha.
  
 **FALSE**  
 Almacena todos los valores que faltan como NULL. Los valores NULL que se almacenan mediante la palabra NULL en el archivo de texto delimitado se importan como la cadena 'NULL'.
 
#### <a name="encoding"></a>ENCODING
   `Encoding = {'UTF8' | 'UTF16'}`
   
 En [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] (APS CU7.4), PolyBase puede leer archivos de texto delimitados y codificados con UTF8 y UTF16-LE. En SQL Server, PolyBase no puede leer archivos codificados UTF16.

## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY EXTERNAL FILE FORMAT.
  
## <a name="general-remarks"></a>Notas generales
 El formato de archivo externo es de ámbito de base de datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Es de ámbito de servidor en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 Las opciones de formato son todas opcionales y solo se aplican a archivos de texto delimitado.  
  
 Cuando los datos se almacenan en uno de los formatos comprimidos, PolyBase primero descomprime los datos para devolver los registros de datos.
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones
  
 El delimitador de filas de los archivos de texto delimitado tiene que ser compatible con LineRecordReader de Hadoop. Es decir, debe ser '\r', '\n' o '\r\n'. El usuario no puede configurar estos delimitadores.
  
 Las combinaciones de métodos SerDe compatibles con RCFiles y los métodos de compresión de datos admitidos se han enumerado anteriormente en este artículo. No se admiten todas las combinaciones.
  
 El número máximo de consultas de PolyBase simultáneas es 32. Si se están ejecutando 32 consultas simultáneas, cada consulta puede leer un máximo de 33.000 archivos de la ubicación del archivo externo. La carpeta raíz y cada subcarpeta también cuentan como un archivo. Si el grado de simultaneidad es menor que 32, la ubicación del archivo externo puede contener más de 33.000 archivos.
  
 Debido a la limitación en el número de archivos en la tabla externa, se recomienda almacenar menos de 30.000 archivos en la raíz y las subcarpetas de la ubicación del archivo externo. Además, se recomienda mantener el número de subcarpetas bajo el directorio raíz en un número pequeño. Si hay referencias a demasiados archivos, podría producirse una excepción de memoria insuficiente de Máquina virtual Java.
  
  Al exportar datos a Hadoop o Azure Blob Storage mediante PolyBase, solo se exportan los datos y no los nombres de columna (metadatos), tal como se define en el comando CREATE EXTERNAL TABLE.

## <a name="locking"></a>Bloqueo  
 Toma un bloqueo compartido en el objeto EXTERNAL FILE FORMAT.
  
## <a name="performance"></a>Rendimiento
 El uso de archivos comprimidos siempre tiene el inconveniente de tener que elegir entre transferir menos datos entre el origen de datos externo y SQL Server y aumentar el uso de CPU para comprimir y descomprimir los datos.
  
 Los archivos de texto comprimidos de Gzip no son divisibles. Para mejorar el rendimiento de los archivos de texto comprimidos de Gzip, se recomienda generar varios archivos que se almacenen en el mismo directorio del origen de datos externo. Esta estructura de archivos permite a PolyBase leer y descomprimir los datos con mayor rapidez al usar varios procesos de lector y descompresión. El número ideal de archivos comprimidos es el número máximo de procesos de lector de datos por nodo de proceso. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], el número máximo de procesos del lector de datos es de ocho por nodo, excepto en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Gen2, que es de 20 lectores por nodo. En [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], el número máximo de procesos de lector de datos por nodo varía según el SLO. Para obtener más información, consulte [Modelos y estrategias de carga de [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://blogs.msdn.microsoft.com/sqlcat/2017/05/17/azure-sql-data-warehouse-loading-patterns-and-strategies/).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-create-a-delimitedtext-external-file-format"></a>A. Crear un formato de archivo externo DELIMITEDTEXT  
 En este ejemplo se crea un formato de archivo externo denominado *textdelimited1* para un archivo de texto delimitado. Las opciones enumeradas para FORMAT\_OPTIONS especifican que los campos del archivo deben separarse con un carácter de barra vertical '|'. El archivo de texto además se comprime con el códec de Gzip. Si no se especifica DATA\_COMPRESSION, se descomprime el archivo de texto.
  
 En un archivo de texto delimitado, el método de compresión de datos puede ser el códec predeterminado, 'org.apache.hadoop.io.compress.DefaultCodec', o el códec de Gzip, 'org.apache.hadoop.io.compress.GzipCodec'.
  
```sql  
CREATE EXTERNAL FILE FORMAT textdelimited1  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT,  
    FORMAT_OPTIONS (  
        FIELD_TERMINATOR = '|',  
        DATE_FORMAT = 'MM/dd/yyyy' ),  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
);  
```  
  
### <a name="b-create-an-rcfile-external-file-format"></a>B. Crear un formato de archivo externo RCFile  
 En este ejemplo se crea un formato de archivo externo para un RCFile que usa el método de serialización/deserialización org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe. Además se especifica el uso del códec predeterminado para el método de compresión de datos. Si no se especifica DATA_COMPRESSION, el valor predeterminado es ninguna compresión.
  
```sql  
CREATE EXTERNAL FILE FORMAT rcfile1  
WITH (  
    FORMAT_TYPE = RCFILE,  
    SERDE_METHOD = 'org.apache.hadoop.hive.serde2.columnar.LazyBinaryColumnarSerDe',  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.DefaultCodec'  
);  
```  
  
### <a name="c-create-an-orc-external-file-format"></a>C. Crear un formato de archivo externo ORC  
 En este ejemplo se crea un formato de archivo externo para un archivo ORC que comprime los datos con el método de compresión de datos org.apache.io.compress.SnappyCodec. Si no se especifica DATA_COMPRESSION, el valor predeterminado es ninguna compresión.
  
```sql 
CREATE EXTERNAL FILE FORMAT orcfile1  
WITH (  
    FORMAT_TYPE = ORC,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
  
### <a name="d-create-a-parquet-external-file-format"></a>D. Crear un formato de archivo externo PARQUET  
 En este ejemplo se crea un formato de archivo externo para un archivo Parquet que comprime los datos con el método de compresión de datos org.apache.io.compress.SnappyCodec. Si no se especifica DATA_COMPRESSION, el valor predeterminado es ninguna compresión.  
  
```sql  
CREATE EXTERNAL FILE FORMAT parquetfile1  
WITH (  
    FORMAT_TYPE = PARQUET,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  
### <a name="e-create-a-delimited-text-file-skipping-header-row-azure-synapse-analytics-only"></a>E. Crear un archivo de texto delimitado y omitir la fila de encabezado (solo Azure Synapse Analytics)
 En este ejemplo se crea un formato de archivo externo para el archivo CSV con una sola fila de encabezado. 
  
```sql  
CREATE EXTERNAL FILE FORMAT skipHeader_CSV
WITH (FORMAT_TYPE = DELIMITEDTEXT,
      FORMAT_OPTIONS(
          FIELD_TERMINATOR = ',',
          STRING_DELIMITER = '"',
          FIRST_ROW = 2, 
          USE_TYPE_DEFAULT = True)
)
```   
### <a name="f-create-a-json-external-file-format"></a>F. Crear un formato de archivo externo JSON  
 En este ejemplo se crea un formato de archivo externo para un archivo JSON que comprime los datos con el método de compresión de datos org.apache.io.compress.SnappyCodec. Si no se especifica DATA_COMPRESSION, el valor predeterminado es ninguna compresión. Este ejemplo se aplica a Azure SQL Edge y actualmente no es compatible con otros productos de SQL. 
  
```sql  
CREATE EXTERNAL FILE FORMAT jsonFileFormat  
WITH (  
    FORMAT_TYPE = JSON,  
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'  
);  
```  

## <a name="see-also"></a>Consulte también
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
 [sys.external_file_formats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-external-file-formats-transact-sql.md)  
