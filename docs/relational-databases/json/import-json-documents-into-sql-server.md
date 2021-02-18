---
description: Importar documentos JSON en SQL Server
title: Importación de documentos JSON
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
ms.assetid: 0e908ec0-7173-4cd2-8f48-2700757b53a5
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 34f8655a24a84d50c5f677d819330537fa030cc6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075076"
---
# <a name="import-json-documents-into-sql-server"></a>Importar documentos JSON en SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

En este artículo se describe cómo importar archivos JSON en SQL Server. Actualmente, hay muchos documentos JSON almacenados en archivos. Las aplicaciones registran información en archivos JSON, los sensores generan información que se almacena en archivos JSON, etc. Es importante ser capaz de leer los datos JSON almacenados en archivos, cargar los datos en SQL Server y analizarlos.

## <a name="import-a-json-document-into-a-single-column"></a>Importar un documento JSON en una sola columna

**OPENROWSET(BULK)** es una función con valores de tabla que puede leer datos de cualquier archivo que se encuentre en la unidad local o la red, si SQL Server tiene acceso de lectura a esa ubicación. Devuelve una tabla con una sola columna con el contenido del archivo. Hay varias opciones que puede utilizar con la función OPENROWSET(BULK), como pueden ser los separadores. Pero en el caso más simple, solamente puede cargar todo el contenido de un archivo como un valor de texto. (Este valor grande único se conoce como un objeto grande de carácter único o SINGLE_CLOB). 

Este es un ejemplo de la función **OPENROWSET(BULK)** que lee el contenido de un archivo JSON y lo devuelve al usuario como un valor único:

```sql
SELECT BulkColumn
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j;
```

OPENJSON(BULK) lee el contenido del archivo y lo devuelve en `BulkColumn`.

También puede cargar el contenido del archivo en una variable local o en una tabla, como se muestra en el ejemplo siguiente:

```sql
-- Load file contents into a variable
SELECT @json = BulkColumn
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j

-- Load file contents into a table 
SELECT BulkColumn
 INTO #temp 
 FROM OPENROWSET (BULK 'C:\JSON\Books\book.json', SINGLE_CLOB) as j
```

Después de cargar el contenido del archivo JSON, puede guardar el texto JSON en una tabla.


## <a name="import-json-documents-from-azure-file-storage"></a>Importar documentos JSON desde Azure File Storage

También puede usar la opción OPENROWSET(BULK) como se ha descrito anteriormente para leer archivos JSON de otras ubicaciones de archivo a las que SQL Server puede acceder. Por ejemplo, Azure File Storage admite el protocolo SMB. Como resultado, puede asignar una unidad virtual local al recurso compartido de Azure File Storage mediante el procedimiento siguiente:

1.  Cree una cuenta de almacenamiento de archivos (por ejemplo, `mystorage`), un recurso compartido de archivos (por ejemplo, `sharejson`) y una carpeta de archivos en Azure File Storage mediante Azure Portal o Azure PowerShell.
2.  Cargue algunos archivos JSON en el recurso compartido de almacenamiento de archivos.
3.  Cree una regla de firewall de salida en el Firewall de Windows en el equipo que permite el puerto 445. Tenga en cuenta que su proveedor de servicio de Internet puede bloquear este puerto. Si recibe un error DNS (error 53) en el paso siguiente, es que no ha abierto el puerto 445 o su ISP lo bloquea.
4. Monte el recurso compartido de Azure File Storage como una unidad local (por ejemplo, `T:`).

    La sintaxis del comando es la siguiente:

    ```dos
    net use [drive letter] \\[storage name].file.core.windows.net\[share name] /u:[storage account name] [storage account access key]
    ```

    Aquí tiene un ejemplo que asigna la letra de unidad local `T:` al recurso compartido de Azure File Storage:

    ```dos
    net use t: \\mystorage.file.core.windows.net\sharejson /u:myaccount hb5qy6eXLqIdBj0LvGMHdrTiygkjhHDvWjUZg3Gu7bubKLg==
    ```

    Puede encontrar la clave de la cuenta de almacenamiento y la clave de acceso de la cuenta de almacenamiento principal o secundaria en la sección Claves de Configuración en Azure Portal.

5.  Ahora puede acceder a los archivos JSON desde el recurso compartido de Azure File Storage mediante la unidad asignada, tal como se muestra en el ejemplo siguiente:

    ```sql
    SELECT book.* FROM
     OPENROWSET(BULK N't:\books\books.json', SINGLE_CLOB) AS json
     CROSS APPLY OPENJSON(BulkColumn)
     WITH( id nvarchar(100), name nvarchar(100), price float,
        pages_i int, author nvarchar(100)) AS book
    ```

Para más información sobre Azure File Storage, vea [File Storage](https://azure.microsoft.com/services/storage/files/).

## <a name="import-json-documents-from-azure-blob-storage"></a>Importar documentos JSON desde Azure Blob Storage

Puede cargar archivos directamente en Azure SQL Database desde Azure Blob Storage con el comando BULK INSERT de T-SQL o la función OPENROWSET.

> [!NOTE]
> Esta funcionalidad se agrega en [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y Azure SQL.

En primer lugar, cree un origen de datos externo, como se muestra en el ejemplo siguiente.

```sql
CREATE EXTERNAL DATA SOURCE MyAzureBlobStorage
 WITH ( TYPE = BLOB_STORAGE,
        LOCATION = 'https://myazureblobstorage.blob.core.windows.net',
        CREDENTIAL= MyAzureBlobStorageCredential);
```

A continuación, ejecute un comando BULK INSERT con la opción DATA_SOURCE.

```sql
BULK INSERT Product
FROM 'data/product.dat'
WITH ( DATA_SOURCE = 'MyAzureBlobStorage');
```

## <a name="parse-json-documents-into-rows-and-columns"></a>Analizar documentos JSON en filas y columnas

En lugar de leer un archivo completo de JSON como un valor único, puede interesarle analizarlo y devolver los libros del archivo y sus propiedades en filas y columnas. En este ejemplo se usa un archivo JSON de [este sitio](https://github.com/tamingtext/book/blob/master/apache-solr/example/exampledocs/books.json) que contiene una lista de libros.

### <a name="example-1"></a>Ejemplo 1

En el ejemplo más simple, solo puede cargar toda la lista desde el archivo. 

```sql
SELECT value
 FROM OPENROWSET (BULK 'C:\JSON\Books\books.json', SINGLE_CLOB) as j
 CROSS APPLY OPENJSON(BulkColumn)
```

La función OPENROWSET anterior lee un solo valor de texto del archivo. OPENROWSET devuelve el valor como BulkColumn y pasa BulkColumn a la función OPENJSON. OPENJSON itera a través de la matriz de objetos JSON de la matriz BulkColumn y devuelve un libro en cada fila. Cada fila tiene formato JSON, como se muestra a continuación.

```json
{"id":"978-0641723445", "cat":["book","hardcover"], "name":"The Lightning Thief", ... }
{"id":"978-1423103349", "cat":["book","paperback"], "name":"The Sea of Monsters", ... }
{"id":"978-1857995879", "cat":["book","paperback"], "name":"Sophie's World : The Greek", ... } 
{"id":"978-1933988177", "cat":["book","paperback"], "name":"Lucene in Action, Second", ... }
```

### <a name="example-2"></a>Ejemplo 2

La función OPENJSON puede analizar el contenido JSON y transformarlo en una tabla o un conjunto de resultados. En el ejemplo siguiente se carga el contenido, se analiza el JSON cargado y se devuelven los cinco campos como columnas:

```sql
SELECT book.*
 FROM OPENROWSET (BULK 'C:\JSON\Books\books.json', SINGLE_CLOB) as j
 CROSS APPLY OPENJSON(BulkColumn)
 WITH( id nvarchar(100), name nvarchar(100), price float,
 pages_i int, author nvarchar(100)) AS book
```

En este ejemplo, OPENROWSET(BULK) lee el contenido del archivo y pasa ese contenido a la función OPENJSON con un esquema definido para la salida. OPENJSON hace coincidir las propiedades de los objetos JSON utilizando nombres de columna. Por ejemplo, la propiedad `price` se devuelve como una columna `price` y se convierte al tipo de datos float. He aquí los resultados:

|Identificador|Nombre|price|pages_i|Autor|
|---|---|---|---|---|
|978-0641723445|The Lightning Thief|12.5|384|Rick Riordan| 
|978-1423103349|The Sea of Monsters|6,49|304|Rick Riordan| 
|978-1857995879|Sophie's World : The Greek Philosophers|3,07|64|Jostein Gaarder| 
|978-1933988177|Lucene in Action, Second Edition|30,5|475|Michael McCandless|
||||||

Ahora puede devolver esta tabla al usuario o cargar los datos en otra tabla.

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Más información sobre JSON en SQL Server y Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vídeos de Microsoft

Para obtener una introducción visual a la compatibilidad integrada de JSON en SQL Server y Azure SQL Database, vea los siguientes vídeos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 y compatibilidad con JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Uso de JSON en SQL Server 2016 y Azure SQL Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON como puente entre los universos NoSQL y relacional)
  
## <a name="see-also"></a>Consulte también

[Convertir datos JSON en filas y columnas con OPENJSON](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)

