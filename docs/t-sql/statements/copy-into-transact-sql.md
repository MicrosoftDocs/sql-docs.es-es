---
title: COPY INTO (Transact-SQL)
titleSuffix: (Azure Synapse Analytics) - SQL Server
description: Use la instrucción COPY en Azure Synapse Analytics para la carga desde cuentas de almacenamiento externo.
ms.date: 09/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- COPY_TSQL
- COPY INTO
- COPY
- LOAD
dev_langs:
- TSQL
author: kevinvngo
ms.author: kevin
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: fb52e9e869c0210e8e320c11dfec21aa8f7d585e
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076813"
---
# <a name="copy-transact-sql"></a>COPY (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

En este artículo se explica cómo usar la instrucción COPY en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] para la carga desde cuentas de almacenamiento externo. La instrucción COPY proporciona la máxima flexibilidad para la ingesta de datos de alto rendimiento en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]. Use COPY para las siguientes funcionalidades:

- Utilizar la carga de usuarios con menos privilegios sin necesidad de estrictos permisos de CONTROL en el almacenamiento de datos
- Ejecutar una instrucción T-SQL única sin tener que crear objetos de base de datos adicionales
- Analizar y cargar correctamente los archivos CSV donde los **delimitadores** (cadena, campo y fila) **se** **escapan dentro de las columnas delimitadas por cadenas**
- Especificar un modelo de permisos más preciso sin exponer las claves de la cuenta de almacenamiento mediante firmas de acceso compartido (SAS)
- Usar una cuenta de almacenamiento diferente para la ubicación de ERRORFILE (REJECTED_ROW_LOCATION)
- Personalizar los valores predeterminados de cada columna de destino y especificar los campos de datos de origen que se van a cargar en columnas de destino concretas
- Especificar un terminador de fila personalizado para los archivos .csv
- Aprovechar los formatos de fecha de SQL Server para los archivos .csv
- Especificar caracteres comodín y varios archivos en la ruta de acceso de la ubicación de almacenamiento

Consulte la documentación siguiente para obtener ejemplos completos y guías de inicio rápido con la instrucción COPY:

- [Inicio rápido: Carga masiva de datos mediante la instrucción COPY](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql)
- [Inicio rápido: Ejemplos de uso de la instrucción COPY y sus métodos de autenticación admitidos](/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql-examples)
- [Inicio rápido: Creación de la instrucción COPY mediante la interfaz de usuario de Synapse Studio enriquecida](/azure/synapse-analytics/quickstart-load-studio-sql-pool)

## <a name="syntax"></a>Sintaxis  

```syntaxsql
COPY INTO [schema.]table_name
[(Column_list)] 
FROM '<external_location>' [,...n]
WITH  
 ( 
 [FILE_TYPE = {'CSV' | 'PARQUET' | 'ORC'} ]
 [,FILE_FORMAT = EXTERNAL FILE FORMAT OBJECT ]  
 [,CREDENTIAL = (AZURE CREDENTIAL) ]
 [,ERRORFILE = '[http(s)://storageaccount/container]/errorfile_directory[/]]' 
 [,ERRORFILE_CREDENTIAL = (AZURE CREDENTIAL) ]
 [,MAXERRORS = max_errors ] 
 [,COMPRESSION = { 'Gzip' | 'DefaultCodec'| 'Snappy'}] 
 [,FIELDQUOTE = 'string_delimiter'] 
 [,FIELDTERMINATOR =  'field_terminator']  
 [,ROWTERMINATOR = 'row_terminator']
 [,FIRSTROW = first_row]
 [,DATEFORMAT = 'date_format'] 
 [,ENCODING = {'UTF8'|'UTF16'}] 
 [,IDENTITY_INSERT = {'ON' | 'OFF'}]
)
```

## <a name="arguments"></a>Argumentos  

*schema_name*  
Es opcional si el esquema predeterminado para el usuario que realiza la operación es el esquema de la tabla especificada. Si no se especifica *schema* y el esquema predeterminado del usuario que realiza la operación COPY es diferente de la tabla especificada, COPY se cancelará y se devolverá un mensaje de error.  

*table_name*  
Es el nombre de la tabla en la que se van a copiar (COPY) los datos. La tabla de destino puede ser una tabla temporal o permanente, y ya debe existir en la base de datos. 

*(column_list)*  
Es una lista opcional de una o varias columnas que se usa para asignar campos de datos de origen a las columnas de la tabla de destino para cargar datos. *column_list* debe ir entre paréntesis y delimitada con comas. La lista de columnas tiene el formato siguiente:

[(Column_name [Default_value] [Field_number] [,...n])]

- *Column_name*: el nombre de la columna en la tabla de destino.
- *Default_value*: el valor predeterminado que sustituirá a cualquier valor NULL en el archivo de entrada. El valor predeterminado se aplica a todos los formatos de archivo. COPY intentará cargar NULL desde el archivo de entrada cuando se omita una columna de la lista de columnas o cuando haya un campo de archivo de entrada vacío.
- *Field_number*: el número de campo del archivo de entrada que se asignará al nombre de la columna de destino.
- La indización de campos comienza en 1.

Cuando no se especifica una lista de columnas, COPY asigna columnas en función de la posición ordinal de origen y de destino: El campo de entrada 1 irá a la columna de destino 1, el campo 2 irá a la columna 2, etc.

*Ubicaciones externas*</br>
Es donde se almacenan provisionalmente los archivos que contienen los datos. Actualmente se admiten Azure Data Lake Storage (ADLS) Gen2 y Azure Blob Storage:

- *Ubicación externa* para Blob Storage: https://<account>.blob.core.windows.net/<container>/<path>
- *Ubicación externa* para ADLS Gen2: https://<account>. dfs.core.windows.net/<container>/<path>

> [!NOTE]  
> El punto de conexión .blob también está disponible para ADLS Gen2 y actualmente ofrece el mejor rendimiento. Use el punto de conexión .blob cuando no se requiera .dfs para su método de autenticación.

- *Cuenta*: el nombre de la cuenta de almacenamiento

- *Contenedor*: el nombre del contenedor de blobs

- *Ruta*: la carpeta o la ruta de acceso de archivo para los datos. La ubicación comienza en el contenedor. Si se especifica una carpeta, COPY recuperará todos los archivos de la carpeta y todas sus subcarpetas. COPY omite las carpetas ocultas y no devuelve los archivos que comienzan por un subrayado (_) o un punto (.), a menos que se especifique explícitamente en la ruta de acceso. Este comportamiento es el mismo incluso cuando se especifica una ruta de acceso con un carácter comodín.

Se pueden incluir caracteres comodín en la ruta de acceso, donde

- La coincidencia de nombres de la ruta de acceso de caracteres comodín distingue mayúsculas de minúsculas
- El carácter comodín se puede escapar mediante el carácter de barra diagonal inversa (\\)
- La expansión de caracteres comodín se aplica de forma recursiva. Por ejemplo, todos los archivos CSV en Customer1 (incluidos los subdirectorios de Customer1 se cargarán en el ejemplo siguiente: 'Account/Container/Customer1/*.csv'

> [!NOTE]  
> Para obtener el mejor rendimiento, evite especificar caracteres comodín que se expandan en un número elevado de archivos. Si es posible, enumere varias ubicaciones de archivo en lugar de especificar caracteres comodín.

Solo se pueden especificar varias ubicaciones de archivos desde el mismo contenedor y cuenta de almacenamiento a través de una lista separada por comas, por ejemplo:

- 'https://<account>.blob.core.windows.net/<container>/<path>', 'https://<account>.blob.core.windows.net/<container>/<path>'…

*FILE_TYPE = { 'CSV' | 'PARQUET' | 'ORC' }*</br>
*FILE_TYPE* especifica el formato de los datos externos.

- CSV: Especifica un archivo de valores separados por comas conforme a la norma [RFC 4180](https://tools.ietf.org/html/rfc4180).
- PARQUET: Especifica un formato Parquet.
- ORC: Especifica un formato ORC (Optimized Row Columnar).

>[!NOTE]  
>El tipo de archivo "Delimited Text" (texto delimitado) de Polybase se sustituye por el formato de archivo "CSV", donde el delimitador de coma predeterminado se puede configurar mediante el parámetro FIELDTERMINATOR. 

*FILE_FORMAT = external_file_format_name*</br>
*FILE_FORMAT* se aplica solo a archivos Parquet y ORC y especifica nombre del objeto de formato de archivo externo que almacena el tipo de archivo y el método de compresión de los datos externos. Para crear un formato de archivo externo, use [CREATE EXTERNAL FILE FORMAT](create-external-file-format-transact-sql.md).

*CREDENTIAL (IDENTITY = '', SECRET = '')*</br>
*CREDENTIAL* especifica el mecanismo de autenticación para tener acceso a la cuenta de almacenamiento externa. Los métodos de autenticación son:

|                          |                CSV                |                      Parquet                       |                        ORC                         |
| :----------------------: | :-------------------------------: | :------------------------------------------------: | :------------------------------------------------: |
|  **Azure Blob Storage**  | SAS/MSI/SERVICE PRINCIPAL/KEY/AAD |                      SAS/KEY                       |                      SAS/KEY                       |
| **Azure Data Lake Gen2** | SAS/MSI/SERVICE PRINCIPAL/KEY/AAD | SAS (blob<sup>1</sup>)/MSI (dfs<sup>2</sup>)/SERVICE PRINCIPAL/KEY/AAD | SAS (blob<sup>1</sup>)/MSI (dfs<sup>2</sup>)/SERVICE PRINCIPAL/KEY/AAD |

1: Para este método de autenticación, se requiere el punto de conexión .blob ( **.blob**.core.windows.net) en la ruta de acceso a la ubicación externa.

2: Para este método de autenticación, se requiere el punto de conexión .dfs ( **.dfs**.core.windows.net) en la ruta de acceso a la ubicación externa.


> [!NOTE]  
>
> - Al autenticarse mediante AAD o en una cuenta de almacenamiento pública, no es necesario especificar CREDENTIAL. 
> - Si la cuenta de almacenamiento está asociada a una red virtual, debe autenticarse con MSI (identidad administrada).

- Autenticación con firmas de acceso compartido (SAS)
  
  - *IDENTITY: Una constante con un valor de "firma de acceso compartido"*
  - *SECRET: la* [*firma de acceso compartido*](/azure/storage/common/storage-sas-overview) *ofrece acceso delegado a recursos en la cuenta de almacenamiento.*
  -  Permisos mínimos necesarios: READ y LIST
  
- Autenticación con [*entidades de servicio*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)

  - *IDENTITY: <ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET: Clave principal de servicio de aplicación de AAD*
  -  Roles de RBAC mínimos necesarios: Colaborador de datos de Storage Blob, colaborador de datos de Storage Blob, propietario de datos de Storage Blob o lector de datos de Storage Blob

- Autenticación con la clave de la cuenta de almacenamiento
  
  - *IDENTITY: Una constante con un valor de "clave de cuenta de almacenamiento"*
  - *SECRET: Clave de cuenta de almacenamiento*
  
- Autenticación con [identidad administrada](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional) (puntos de conexión de servicio de red virtual)
  
  - *IDENTITY: Constante con el valor de "identidad administrada"*
  - Roles de RBAC mínimos necesarios: Colaborador de datos de Storage Blob o propietario de datos de Storage Blob para el servidor SQL Database registrado de AAD
  
- Autenticación con un usuario de AAD
  
  - *No se necesita CREDENTIAL*
  - Roles de RBAC mínimos necesarios: Colaborador de datos de Storage Blob o propietario de datos de Storage Blob para el usuario de AAD

*ERRORFILE = Ubicación del directorio*</br>
*ERRORFILE* solo se aplica a CSV. Especifica el directorio de la instrucción COPY donde se deben escribir las filas rechazadas y el archivo de error correspondiente. Se puede especificar la ruta de acceso completa de la cuenta de almacenamiento o se puede especificar la ruta de acceso relativa al contenedor. Si la ruta de acceso especificada no existe, se creará una en su nombre. Se crea un directorio secundario con el nombre “_rejectedrows”. El carácter “_ ” garantiza que se escape el directorio para otro procesamiento de datos, a menos que se mencione explícitamente en el parámetro de ubicación. 

En este directorio hay una carpeta que se crea según la hora de envío de la carga con el formato AñoMesDía-HoraMinutoSegundo (por ejemplo, 20180330-173205). En esta carpeta, se escriben dos tipos de archivos, el archivo de motivos (error) y el archivo de datos (fila) cada uno anexado previamente con queryID, distributionID y GUID de archivo. Como los datos y los motivos están en archivos independientes, los archivos correspondientes tienen un prefijo coincidente.

Si ERRORFILE tiene la ruta de acceso completa de la cuenta de almacenamiento definida, ERRORFILE_CREDENTIAL se usará para conectarse al almacenamiento. De lo contrario, se usará el valor mencionado para CREDENTIAL.

*ERRORFILE_CREDENTIAL = (IDENTITY= '', SECRET = '')*</br>
*ERRORFILE_CREDENTIAL* solo se aplica a los archivos CSV. Los métodos de autenticación y el origen de datos admitidos son:

- Azure Blob Storage - SAS/SERVICE PRINCIPAL/KEY/AAD
- Azure Data Lake Gen2 - SAS/MSI/SERVICE PRINCIPAL/KEY/AAD
  
- Autenticación con firmas de acceso compartido (SAS)
  - *IDENTITY: Una constante con un valor de "firma de acceso compartido"*
  - *SECRET: la* [*firma de acceso compartido*](/azure/storage/common/storage-sas-overview) *ofrece acceso delegado a recursos en la cuenta de almacenamiento.*
  - Permisos mínimos necesarios: READ, LIST, WRITE, CREATE, DELETE
  
- Autenticación con [*entidades de servicio*](/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store#create-a-credential)
  - *IDENTITY: <ClientID>@<OAuth_2.0_Token_EndPoint>*
  - *SECRET: Clave principal de servicio de aplicación de AAD*
  - Roles de RBAC mínimos necesarios: Colaborador de datos de Storage Blob o propietario de datos de Storage Blob
  
> [!NOTE]  
> Usar el punto de conexión de token de OAuth 2.0 **V1**

- Autenticación con la clave de la cuenta de almacenamiento
  - *IDENTITY: Una constante con un valor de "clave de cuenta de almacenamiento"*
  - *SECRET: Clave de cuenta de almacenamiento*
  
- Autenticación con [identidad administrada](/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase#authenticate-using-managed-identities-to-load-optional) (puntos de conexión de servicio de red virtual)
  - *IDENTITY: Constante con el valor de "identidad administrada"*
  - Roles de RBAC mínimos necesarios: Colaborador de datos de Storage Blob o propietario de datos de Storage Blob para el servidor SQL Database registrado de AAD
  
- Autenticación con un usuario de AAD
  - *No se necesita CREDENTIAL*
  - Roles de RBAC mínimos necesarios: Colaborador de datos de Storage Blob o propietario de datos de Storage Blob para el usuario de AAD

> [!NOTE]  
> Si usa la misma cuenta de almacenamiento para ERRORFILE y especifica la ruta de acceso de ERRORFILE relativa a la raíz del contenedor, no es necesario especificar ERROR_CREDENTIAL.

*MAXERRORS = max_errors*</br>
*MAXERRORS* especifica el número máximo de filas rechazadas permitidas en la carga antes de que se cancele la operación COPY. Cada fila que no se puede importar con la operación COPY se omite y se considera un error. Si no se especifica max_errors, el valor predeterminado es 0.

*COMPRESSION = { 'DefaultCodec ' \| 'Snappy' \| 'GZIP' \| 'NONE'}*</br>
*COMPRESSION* es opcional y especifica el método de compresión de datos para los datos externos.

- CSV admite GZIP
- Parquet admite GZIP y Snappy
- ORC admite DefaultCodec y Snappy.
- Zlib es la compresión predeterminada para ORC

El comando COPY detecta automáticamente el tipo de compresión en base a la extensión de archivo cuando no se especifica este parámetro:

- .gz - **GZIP**
- .snappy – **Snappy**
- .deflate - **DefaultCodec** (solo Parquet y ORC)

 *FIELDQUOTE = 'field_quote'*</br>
*FIELDQUOTE* se aplica a CSV y especifica un solo carácter que se usará como carácter de comilla (delimitador de cadena) en el archivo CSV. Si no se especifica, se usará el carácter de comillas (") como carácter de comillas, según define la norma RFC 4180. Los caracteres ASCII y multibyte extendidos no se admiten con UTF-8 para FIELDQUOTE.

> [!NOTE]  
> Los caracteres FIELDQUOTE se escapan en columnas de cadena en las que existe la presencia de un doble FIELDQUOTE (delimitador). 

*FIELDTERMINATOR = 'field_terminator'*</br>
*FIELDTERMINATOR* solo se aplica a CSV. Especifica el terminador de campo que se usará en el archivo CSV. El terminador de campo se puede especificar mediante notación hexadecimal. El terminador de campo puede ser de varios caracteres. El terminador de campo predeterminado es una coma (,). Los caracteres ASCII y multibyte extendidos no se admiten con UTF-8 para FIELDTERMINATOR.

ROW TERMINATOR = 'row_terminator'</br>
*ROW TERMINATOR* solo se aplica a CSV. Especifica el terminador de fila que se usará en el archivo CSV. El terminador de fila se puede especificar mediante notación hexadecimal. El terminador de fila puede ser de varios caracteres. De forma predeterminada, el terminador de fila es \r\n. 

El comando COPY antepone el carácter \r al especificar \n (nueva línea), lo cual genera \r\n. Para especificar solo el carácter \n, use la notación hexadecimal (0x0A). Al especificar los terminadores de fila de varios caracteres en formato hexadecimal, no especifique 0x entre cada carácter.

Los caracteres ASCII y multibyte extendidos no se admiten con UTF-8 para ROW TERMINATOR.

*FIRSTROW = First_row_int*</br>
*FIRSTROW* se aplica a CSV y especifica el número de fila que se lee primero en todos los archivos para el comando COPY. Los valores se inician a partir de 1, que es el valor predeterminado. Si el valor se establece en dos, se omite la primera fila de cada archivo (fila de encabezado) al cargar los datos. Las filas se omiten en función de la existencia de terminadores de fila.

*DATEFORMAT = { 'mdy' \| 'dmy' \| 'ymd' \| 'ydm' \| 'myd' \| 'dym' }*</br>
DATEFORMAT solo se aplica a CSV y especifica el formato de fecha de la asignación de fecha para los formatos de fecha de SQL Server. Para una introducción acerca de todos los tipos de datos y funciones de fecha y hora de Transact-SQL, vea [Funciones de fecha y hora (Transact-SQL)](../functions/date-and-time-data-types-and-functions-transact-sql.md). DATEFORMAT dentro del comando COPY tiene prioridad sobre [DATEFORMAT configurado en el nivel de sesión](set-dateformat-transact-sql.md).

*ENCODING = 'UTF8' | 'UTF16'*</br>
*ENCODING* solo se aplica a CSV. El valor predeterminado es UTF8. Especifica el estándar de codificación de datos para los archivos cargados por el comando COPY. 

*IDENTITY_INSERT = 'ON' | 'OFF'*</br>
IDENTITY_INSERT especifica si el valor o los valores de identidad del archivo de datos importado se van a utilizar para la columna de identidad. Si IDENTITY_INSERT está desactivado (OFF, valor predeterminado), se comprueban los valores de identidad de esta columna, pero no se importan. Azure Synapse Analytics asignará automáticamente valores únicos en función de los valores de inicialización e incremento especificados durante la creación de la tabla. Tenga en cuenta el siguiente comportamiento con el comando COPY:

- Si IDENTITY_INSERT está desactivado (OFF) y la tabla tiene una columna de identidad
  - Se debe especificar una lista de columnas que no asigne un campo de entrada a la columna de identidad.
- Si IDENTITY_INSERT está activado (ON) y la tabla tiene una columna de identidad
  - Si se pasa una lista de columnas, debe asignar un campo de entrada a la columna de identidad.
- No se admite el valor predeterminado para IDENTITY COLUMN en la lista de columnas.
- IDENTITY_INSERT solo se puede establecer para una tabla cada vez.

### <a name="permissions"></a>Permisos  

El usuario que ejecuta el comando COPY debe tener los permisos siguientes: 

- [ADMINISTER DATABASE BULK OPERATIONS](grant-database-permissions-transact-sql.md#remarks)
- [INSERT ](grant-database-permissions-transact-sql.md#remarks)

Se requieren los permisos INSERT y ADMINISTER BULK OPERATIONS. En [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], se necesitan los permisos INSERT y ADMINISTER DATABASE BULK OPERATIONS.

## <a name="examples"></a>Ejemplos  

### <a name="a-load-from-a-public-storage-account"></a>A. Realizar la carga desde una cuenta de almacenamiento público

El ejemplo siguiente es la forma más sencilla del comando COPY, que carga datos desde una cuenta de almacenamiento pública. En este ejemplo, los valores predeterminados de la instrucción COPY coinciden con el formato del archivo CSV del elemento de línea.

```sql
COPY INTO dbo.[lineitem] FROM 'https://unsecureaccount.blob.core.windows.net/customerdatasets/folder1/lineitem.csv'
```

Los valores predeterminados del comando COPY son:

- DATEFORMAT = DATEFORMAT de la sesión 

- MAXERRORS = 0

- COMPRESSION = descomprimido

- FIELDQUOTE = “” 

- FIELDTERMINATOR = “,” 

- ROWTERMINATOR = '\n'

> [!IMPORTANT]
> COPY trata '\n' como '\r\n' internamente. Para más información, vea la sección ROWTERMINATOR.

- FIRSTROW = 1

- ENCODING = 'UTF8'

- FILE_TYPE = 'CSV'

- IDENTITY_INSERT = 'OFF'

### <a name="b-load-authenticating-via-share-access-signature-sas"></a>B. Realizar la carga mediante la autenticación a través de la firma de acceso compartido (SAS)

En el ejemplo siguiente se cargan archivos que usan el avance de línea como terminador de fila, como una salida de UNIX. En este ejemplo también se usa una clave SAS para autenticarse en Azure Blob Storage.

```sql
COPY INTO test_1
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='?sv=2018-03-28&ss=bfqt&srt=sco&sp=rl&st=2016-10-17T20%3A14%3A55Z&se=2021-10-18T20%3A19%3A00Z&sig=IEoOdmeYnE9%2FKiJDSHFSYsz4AkNa%2F%2BTx61FuQ%2FfKHefqoBE%3D'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=';',
    ROWTERMINATOR='0X0A',
    ENCODING = 'UTF8',
    DATEFORMAT = 'ymd',
    MAXERRORS = 10,
    ERRORFILE = '/errorsfolder',--path starting from the storage container
    IDENTITY_INSERT = 'ON'
)
```

### <a name="c-load-with-a-column-list-with-default-values-authenticating-via-storage-account-key"></a>C. Realizar la carga con una lista de columnas con valores predeterminados mediante la autenticación a través de la clave de cuenta de almacenamiento

En este ejemplo se cargan archivos que especifican una lista de columnas con valores predeterminados. 

```sql
--Note when specifying the column list, input field numbers start from 1
COPY INTO test_1 (Col_one default 'myStringDefault' 1, Col_two default 1 3)
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='<Your_Account_Key>'),
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='x6RWv4It5F2msnjelv3H4DA80n0PQW0daPdw43jM0nyetx4c6CpDkdj3986DX5AHFMIf/YN4y6kkCnU8lb+Wx0Pj+6MDw=='),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=',',
    ROWTERMINATOR='0x0A',
    ENCODING = 'UTF8',
    FIRSTROW = 2
)
```

### <a name="d-load-parquet-or-orc-using-existing-file-format-object"></a>D. Realizar la carga de ORC o Parquet con el objeto de formato de archivo existente

 En este ejemplo se usa un carácter comodín para cargar todos los archivos de Parquet en una carpeta. 

```sql
COPY INTO test_parquet
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.parquet'
WITH (
    FILE_FORMAT = myFileFormat,
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>')
)
```

### <a name="e-load-specifying-wild-cards-and-multiple-files"></a>E. Realizar la carga mediante la especificación de caracteres comodín y varios archivos

```sql
COPY INTO t1
FROM 
'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt', 
    'https://myaccount.blob.core.windows.net/myblobcontainer/folder1'
WITH ( 
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= '<client_id>@<OAuth_2.0_Token_EndPoint>',SECRET='<key>'),
    FIELDTERMINATOR = '|'
)
```

### <a name="f-load-using-msi-credentials"></a>F. Realizar la carga con credenciales de MSI

```sql
COPY INTO dbo.myCOPYDemoTable
FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL = (IDENTITY = 'Managed Identity'),
    FIELDQUOTE = '"',
    FIELDTERMINATOR=','
)
```

## <a name="faq"></a>Preguntas más frecuentes

### <a name="what-is-the-performance-of-the-copy-command-compared-to-polybase"></a>¿Cuál es el rendimiento del comando COPY en comparación con PolyBase?
El comando COPY tendrá un mejor rendimiento en función de la carga de trabajo. Para obtener el mejor rendimiento, considere la posibilidad de dividir la entrada en varios archivos al cargar archivos .csv. Esta guía también es aplicable a los archivos comprimidos GZIP.

### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-csv-files"></a>¿Cuál es el procedimiento para dividir archivos a la hora de cargar archivos CSV con el comando COPY?
Las instrucciones sobre el número de archivos se describen en la tabla siguiente. Una vez alcanzado el número recomendado de archivos, obtendrá un mejor rendimiento cuanto mayor tamaño tengan estos. Para obtener una experiencia sencilla de división de archivos, consulte la siguiente [documentación](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/how-to-maximize-copy-load-throughput-with-file-splits/ba-p/1314474). 

| **DWU** | **Número de archivos** |
| :-----: | :--------: |
|   100   |     60     |
|   200   |     60     |
|   300   |     60     |
|   400   |     60     |
|   500   |     60     |
|  1,000  |    120     |
|  1500  |    180     |
|  2\.000  |    240     |
|  2,500  |    300     |
|  3,000  |    360     |
|  5\.000  |    600     |
|  6,000  |    720     |
|  7 500  |    900     |
| 10 000  |    1200    |
| 15,000  |    1800    |
| 30,000  |    3600    |


### <a name="what-is-the-file-splitting-guidance-for-the-copy-command-loading-parquet-or-orc-files"></a>¿Cuál es el procedimiento para dividir archivos a la hora de cargar archivos ORC o Parquet con el comando COPY?
No es necesario dividir los archivos ORC o Parquet porque el comando COPY lo hará de forma automática. Para obtener el mejor rendimiento, los archivos Parquet y ORC de la cuenta de almacenamiento de Azure deben tener un tamaño de 256 MB o más. 

### <a name="are-there-any-limitations-on-the-number-or-size-of-files"></a>¿Hay alguna limitación en el número o el tamaño de los archivos?
No hay limitaciones en cuanto al número o tamaño de los archivos; sin embargo, para obtener el mejor rendimiento, recomendamos usar archivos de al menos 4 MB.

### <a name="are-there-any-known-issues-with-the-copy-statement"></a>¿Hay algún problema conocido con la instrucción COPY?
Si tiene un área de trabajo de Synapse que se haya creado antes del 07/12/2020, puede aparecer un mensaje de error similar al autenticarse mediante Identidad administrada:

*com.microsoft.sqlserver.jdbc.SQLServerException: La identidad de servicio administrada no se ha habilitado en este servidor. Habilite la identidad de servicio administrada e inténtelo de nuevo.*

Siga estos pasos para solucionar este problema volviendo a registrar la identidad administrada del área de trabajo:

1. Vaya al área de trabajo de Synapse en Azure Portal.
2. Vaya a la hoja Identidades administradas. 
3. Si la opción "Permitir canalizaciones" ya está activada, debe desactivarla y guardar.
4. Active la opción "Permitir canalizaciones" y guarde.


## <a name="see-also"></a>Consulte también  

 [Información general sobre la carga con [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](/azure/sql-data-warehouse/design-elt-data-loading)
