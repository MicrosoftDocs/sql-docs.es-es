---
description: Datos JSON en SQL Server
title: Uso de datos JSON
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: quickstart
helpviewer_keywords:
- JSON
- JSON, built-in support
ms.assetid: c9a4e145-33c3-42b2-a510-79813e67806a
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||= azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 551a0ad762f6cc8fdc0a347c5896a3bbbea27b82
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076011"
---
# <a name="json-data-in-sql-server"></a>Datos JSON en SQL Server

[!INCLUDE[appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

JSON es un formato de datos de texto muy popular que se usa para intercambiar datos en las aplicaciones web y móviles modernas. JSON también sirve para almacenar los datos no estructurados en archivos de registro o en bases de datos NoSQL, como Microsoft Azure Cosmos DB. Muchos servicios web REST devuelven resultados con formato de texto JSON o bien aceptan datos con este formato. Por ejemplo, la mayoría de los servicios de Azure, como Azure Search, Azure Storage y Azure Cosmos DB, cuentan con extremos REST que devuelven o usan JSON. JSON es también el formato principal para intercambiar datos entre páginas web y servidores web a través de llamadas AJAX. 

Las funciones JSON de SQL Server permiten combinar conceptos NoSQL y relacionales en la misma base de datos. Ahora puede combinar columnas relacionales clásicas con columnas que contienen documentos con formato de texto JSON en la misma tabla, analizar e importar documentos JSON en estructuras relacionales o dar formato de texto JSON a datos relacionales. Vea cómo las funciones JSON conectan conceptos NoSQL y relacionales en SQL Server y Azure SQL Database en el vídeo siguiente:

*JSON as a bridge between NoSQL and relational worlds* (JSON como puente entre los universos NoSQL y relacional)
> [!VIDEO https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds/player]

Este es un ejemplo de texto JSON:

```json
[
  {
    "name": "John",
    "skills": ["SQL", "C#", "Azure"]
  },
  {
    "name": "Jane",
    "surname": "Doe"
  }
]
```

SQL Server proporciona funciones y operadores integrados que permiten hacer lo siguiente con texto JSON:

- Analizar texto JSON y leer o modificar valores.  
- Transformar matrices de objetos JSON a formato de tabla.  
- Ejecutar cualquier consulta de Transact-SQL en los objetos JSON convertidos.  
- Dar formato JSON a los resultados de consultas de Transact-SQL.  
  
![Información general de la compatibilidad integrada de JSON](../../relational-databases/json/media/jsonslides1overview.png "Información general de la compatibilidad integrada de JSON")  
  
## <a name="key-json-capabilities-of-sql-server-and-sql-database"></a>Funcionalidades clave de JSON de SQL Server y SQL Database

En las siguientes secciones se analizan las funcionalidades clave que proporciona SQL Server con su compatibilidad de JSON integrada. Puede ver cómo se usan los operadores y las funciones JSON en el vídeo siguiente:

*SQL Server 2016 and JSON Support* (SQL Server 2016 y compatibilidad con JSON)
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support/player?WT.mc_id=dataexposed-c9-niner]

### <a name="extract-values-from-json-text-and-use-them-in-queries"></a>Extraer valores de texto JSON y usarlos en consultas

Si tiene texto JSON almacenado en tablas de base de datos, puede usar las siguientes funciones integradas para leer o modificar los valores de ese texto JSON:  

- [ISJSON (Transact-SQL)](../../t-sql/functions/isjson-transact-sql.md) prueba si una cadena contiene un valor JSON válido.
- [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md) extrae un valor escalar de una cadena JSON.
- [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md) extrae un objeto o una matriz de una cadena JSON.
- [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md) cambia un valor de una cadena JSON.

**Ejemplo**

En el siguiente ejemplo, la consulta usa datos tanto relacionales como JSON (almacenados en la columna denominada `jsonCol`) de una tabla:  
  
```sql  
SELECT Name, Surname,
  JSON_VALUE(jsonCol, '$.info.address.PostCode') AS PostCode,
  JSON_VALUE(jsonCol, '$.info.address."Address Line 1"') + ' '
  + JSON_VALUE(jsonCol, '$.info.address."Address Line 2"') AS Address,
  JSON_QUERY(jsonCol, '$.info.skills') AS Skills
FROM People
WHERE ISJSON(jsonCol) > 0
  AND JSON_VALUE(jsonCol, '$.info.address.Town') = 'Belgrade'
  AND Status = 'Active'
ORDER BY JSON_VALUE(jsonCol, '$.info.address.PostCode')
```  
  
Las herramientas y aplicaciones no distinguen entre los valores extraídos de las columnas de la tabla escalar y los valores de las columnas JSON. Puede usar los valores del texto JSON en cualquier parte de la consulta de Transact-SQL (incluidas las cláusulas WHERE, ORDER BY y GROUP BY, los agregados de ventanas, etc.). Las funciones JSON usan una sintaxis parecida a la de JavaScript para hacer referencia a los valores de texto JSON.

Para obtener más información, consulte [Validar, consultar y cambiar datos JSON con funciones integradas (SQL Server)](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md), [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md) y [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md).  
  
### <a name="change-json-values"></a>Cambiar valores de JSON

Si tiene que modificar partes del texto JSON, puede usar la función [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md) para actualizar el valor de una propiedad en una cadena JSON y devolver la cadena JSON actualizada. En el siguiente ejemplo se actualiza el valor de una propiedad de una variable que contiene JSON:  
  
```sql
DECLARE @json NVARCHAR(MAX);
SET @json = '{"info": {"address": [{"town": "Belgrade"}, {"town": "Paris"}, {"town":"Madrid"}]}}';
SET @json = JSON_MODIFY(@json, '$.info.address[1].town', 'London');
SELECT modifiedJson = @json;
```

**Resultados**

|modifiedJson|  
|--------|  
|{"info":{"address":[{"town":"Belgrade"},{"town":"London"},{"town":"Madrid"}]}|  
  
### <a name="convert-json-collections-to-a-rowset"></a>Convertir colecciones de JSON en un conjunto de filas

No es necesario un lenguaje de consulta personalizado para consultar JSON en SQL Server. Para consultar datos JSON, puede usar T-SQL estándar. Si tiene que crear una consulta o un informe sobre datos JSON, puede convertir los datos JSON fácilmente en filas y columnas llamando a la función de conjunto de filas **OPENJSON**. Para obtener más información, consulte [Análisis y transformación de datos JSON con OPENJSON (SQL Server)](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md).  
  
En el siguiente ejemplo, se llama a **OPENJSON** y se transforma la matriz de objetos almacenada en la variable `@json` en un conjunto de filas que se puede consultar con una instrucción estándar **SELECT** de SQL:  
  
```sql  
DECLARE @json NVARCHAR(MAX);
SET @json = N'[
  {"id": 2, "info": {"name": "John", "surname": "Smith"}, "age": 25},
  {"id": 5, "info": {"name": "Jane", "surname": "Smith"}, "dob": "2005-11-04T12:00:00"}
]';

SELECT *
FROM OPENJSON(@json)
  WITH (
    id INT 'strict $.id',
    firstName NVARCHAR(50) '$.info.name',
    lastName NVARCHAR(50) '$.info.surname',
    age INT,
    dateOfBirth DATETIME2 '$.dob'
  );
```

**Resultados**

|id|firstName|lastName|age|dateOfBirth|  
|--------|---------------|--------------|---------|-----------------|  
|2|John|Smith|25||  
|5|Julia|Smith||2005-11-04T12:00:00|  
  
**OPENJSON** transforma la matriz de objetos JSON en una tabla, donde cada objeto se representa como una fila y los pares clave-valor se devuelven en forma de celdas. El resultado detecta las siguientes reglas:

- **OPENJSON** convierte valores JSON a los tipos especificados en la cláusula **WITH**.
- **OPENJSON** se puede encargar tanto de los pares clave-valor sin formato como de los objetos anidados organizados jerárquicamente.
- No hay que devolver todos los campos incluidos en el texto JSON.
- Si no existen valores JSON, **OPENJSON** devuelve valores NULL.
- Opcionalmente, puede especificar una ruta de acceso después de la especificación de tipo para hacer referencia a una propiedad anidada o a una propiedad mediante un nombre diferente.
- El prefijo opcional **strict** en la ruta de acceso señala que los valores de las propiedades especificadas deben existir en el texto JSON.

Para obtener más información, consulte [Análisis y transformación de datos JSON con OPENJSON (SQL Server)](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md) y [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md).  

Los documentos JSON pueden tener subelementos y datos jerárquicos que no se pueden asignar directamente a las columnas relacionales estándares. En este caso, es posible simplificar la jerarquía JSON mediante la combinación de la entidad primaria con submatrices.

En el ejemplo siguiente, el segundo objeto de la matriz tiene una submatriz que representa las aptitudes de una persona. Todos los objetos secundarios se pueden analizar mediante una llamada adicional a la función `OPENJSON`:

```sql  
DECLARE @json NVARCHAR(MAX);
SET @json = N'[  
  {"id": 2, "info": {"name": "John", "surname": "Smith"}, "age": 25},
  {"id": 5, "info": {"name": "Jane", "surname": "Smith", "skills": ["SQL", "C#", "Azure"]}, "dob": "2005-11-04T12:00:00"}  
]';

SELECT *  
FROM OPENJSON(@json)  
  WITH (
    id INT 'strict $.id',
    firstName NVARCHAR(50) '$.info.name',
    lastName NVARCHAR(50) '$.info.surname',  
    age INT,
    dateOfBirth DATETIME2 '$.dob',
    skills NVARCHAR(MAX) '$.info.skills' AS JSON
  )
OUTER APPLY OPENJSON(skills)
  WITH (skill NVARCHAR(8) '$');
```

La matriz **aptitudes** se devuelve en la primera `OPENJSON` como el fragmento de texto JSON original y se pasa a otra función `OPENJSON` mediante el operador `APPLY`. La segunda función `OPENJSON` analizará la matriz JSON y los valores de cadena devueltos como un conjunto de filas de una columna única que se combinarán con el resultado de la primera `OPENJSON`.
El resultado de esta consulta se muestra en la tabla siguiente:

**Resultados**

|id|firstName|lastName|age|dateOfBirth|aptitudes|  
|--------|---------------|--------------|---------|-----------------|----------|  
|2|John|Smith|25|||  
|5|Julia|Smith||2005-11-04T12:00:00|SQL|
|5|Julia|Smith||2005-11-04T12:00:00|C#|
|5|Julia|Smith||2005-11-04T12:00:00|Azure|

`OUTER APPLY OPENJSON` se unirá a la entidad de primer nivel con la submatriz y devolverá un conjunto de resultados sin formato. Debido a JOIN, la segunda fila se repetirá para cada aptitud.

### <a name="convert-sql-server-data-to-json-or-export-json"></a>Convertir datos de SQL Server a JSON o exportar JSON

>[!NOTE]
>No se admite la conversión de datos de Azure Synapse Analytics a JSON ni la exportación de JSON.

Para dar formato JSON a los datos de SQL Server o a los resultados de las consultas, agregue la cláusula **FOR JSON** a una instrucción **SELECT** . Use **FOR JSON** para delegar en SQL Server la aplicación de formato de los resultados de JSON de las aplicaciones cliente. Para obtener más información, consulte [Dar formato JSON a los resultados de consulta con FOR JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md).  
  
En el siguiente ejemplo se usa el modo PATH con la cláusula **FOR JSON**:  
  
```sql  
SELECT id, firstName AS "info.name", lastName AS "info.surname", age, dateOfBirth AS dob
FROM People
FOR JSON PATH;
```

El **FOR JSON** cambia el formato de los resultados SQL a texto JSON que podrá usarse en cualquier aplicación que comprenda JSON. En la opción PATH se usan alias separados por puntos en la cláusula SELECT para anidar objetos en los resultados de la consulta.  
  
**Resultados**

```json  
[
  {
    "id": 2,
    "info": {
      "name": "John",
      "surname": "Smith"
    },
    "age": 25
  },
  {
    "id": 5,
    "info": {
      "name": "Jane",
      "surname": "Smith"
    },
    "dob": "2005-11-04T12:00:00"
  }
]
```  
  
Para obtener más información, consulte [Dar formato JSON a los resultados de consulta con FOR JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md) y [FOR Clause (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md) (Cláusula FOR [Transact-SQL]).  

## <a name="use-cases-for-json-data-in-sql-server"></a>Casos de uso para datos JSON en SQL Server

La compatibilidad de JSON con SQL Server y Azure SQL Database le permite combinar conceptos relacionales y de NoSQL. Puede transformar fácilmente los datos semiestructurados en relacionales y viceversa. Sin embargo, JSON no es un reemplazo para los modelos relacionales existentes. Estos son algunos casos de uso específicos que se benefician de la compatibilidad de JSON en SQL Server y SQL Database.

### <a name="simplify-complex-data-models"></a>Simplificación de modelos de datos complejos

Le recomendamos que desnormalice el modelo de datos con campos JSON en lugar de varias tablas secundarias.

### <a name="store-retail-and-e-commerce-data"></a>Almacenamiento de datos de comercio electrónico y de venta al por menor

Almacene información sobre productos con una amplia gama de atributos variables en un modelo desnormalizado para obtener una mayor flexibilidad.

### <a name="process-log-and-telemetry-data"></a>Procesamiento de datos de telemetría y registros

Cargue, consulte y analice datos de registro almacenados como archivos JSON con toda la potencia del lenguaje Transact-SQL.

### <a name="store-semi-structured-iot-data"></a>Almacenamiento de datos de IoT semiestructurados

Si necesita un análisis en tiempo real de datos de IoT, cargue los datos entrantes directamente en la base de datos en lugar de almacenarlos provisionalmente en una ubicación de almacenamiento.

### <a name="simplify-rest-api-development"></a>Simplificación del desarrollo de la API de REST

Transforme datos relacionales de la base de datos fácilmente en el formato JSON que usan las API de REST que admite su sitio web.

## <a name="combine-relational-and-json-data"></a>Combinar datos relacionales y datos JSON

SQL Server proporciona un modelo híbrido para almacenar y procesar datos JSON y relacionales usando el lenguaje Transact-SQL estándar. Puede organizar colecciones de documentos JSON en tablas, establecer relaciones entre ellas, combinar columnas escalares fuertemente tipadas y almacenadas en tablas con pares clave-valor flexibles almacenados en columnas JSON y, además, consultar valores JSON y escalares en una o varias tablas mediante Transact-SQL al completo.

El texto JSON se almacena en columnas `VARCHAR` o `NVARCHAR` y se indexa como texto sin formato. Cualquier componente o característica de SQL Server que admita texto admite JSON, por lo que no hay casi ninguna restricción en cuanto a la interacción entre JSON y otras características de SQL Server. Así, JSON puede almacenar en tablas temporales o tablas en memoria, aplicar predicados de seguridad de nivel de fila al texto JSON, etc.

Si tiene cargas de trabajo JSON puras en las que quiera usar algún tipo de lenguaje de consulta personalizado para procesar documentos JSON, sopese la posibilidad de usar Microsoft Azure [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).  
  
Estos son algunos casos de uso que muestran cómo se puede usar la compatibilidad integrada de JSON en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="store-and-index-json-data-in-sql-server"></a>Almacenamiento e índice de datos JSON en SQL Server

JSON es un formato de texto para que los documentos JSON se puedan almacenar en columnas `NVARCHAR` en una instancia de SQL Database. Como el tipo `NVARCHAR` es compatible con todos los subsistemas de SQL Server, puede colocar los documentos JSON en tablas con índices **CLUSTERED COLUMNSTORE**, tablas **optimizadas para memoria** o archivos externos que se pueden leer mediante OPENROWSET o PolyBase.

Para obtener más información sobre las opciones para almacenar, indexar y optimizar datos JSON en SQL Server, consulte los siguientes artículos:

- [Almacenamiento de documentos JSON en SQL Server o SQL Database](store-json-documents-in-sql-tables.md)
- [Indexación de datos JSON](index-json-data.md)
- [Optimización del procesamiento de OLTP en memoria JSON](optimize-json-processing-with-in-memory-oltp.md)

### <a name="load-json-files-into-sql-server"></a>Cargar archivos JSON en SQL Server  

Puede dar formato a información que se almacena en archivos JSON estándar o JSON delimitado por línea. SQL Server puede importar el contenido de archivos JSON, analizarlo mediante las funciones **OPENJSON** o **JSON_VALUE**, y cargarlo en tablas.  
  
-   Si los documentos JSON están almacenados en archivos locales, en unidades de red compartidas o en ubicaciones de Azure Files a los que SQL Server tenga acceso, puede realizar una operación de importación en bloque para cargar los datos JSON en SQL Server.
  
-   Si los archivos JSON delimitados por línea están almacenados en el sistema de archivos de Hadoop o en Azure Blob Storage, puede usar PolyBase para cargar el texto JSON, analizarlo en código Transact-SQL y cargarlo en tablas.  

### <a name="import-json-data-into-sql-server-tables"></a>Importar datos JSON a tablas de SQL Server

Si tiene que cargar los datos JSON a SQL Server desde un servicio externo, puede usar **OPENJSON** para importar los datos a SQL Server en lugar de analizarlos en el nivel de aplicación.  
  
```sql  
DECLARE @jsonVariable NVARCHAR(MAX);

SET @jsonVariable = N'[
  {
    "Order": {  
      "Number":"SO43659",  
      "Date":"2011-05-31T00:00:00"  
    },  
    "AccountNumber":"AW29825",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":1  
    }  
  },  
  {  
    "Order": {  
      "Number":"SO43661",  
      "Date":"2011-06-01T00:00:00"  
    },  
    "AccountNumber":"AW73565",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":3  
    }  
  }
]';

-- INSERT INTO <sampleTable>  
SELECT SalesOrderJsonData.*
FROM OPENJSON (@jsonVariable, N'$')
  WITH (
    Number VARCHAR(200) N'$.Order.Number',
    Date DATETIME N'$.Order.Date',
    Customer VARCHAR(200) N'$.AccountNumber',
    Quantity INT N'$.Item.Quantity'
  ) AS SalesOrderJsonData;
```

Puede proporcionar el contenido de la variable JSON desde un servicio REST externo, enviarlo como un parámetro desde un marco de trabajo de JavaScript de cliente o bien cargarlo desde archivos externos. Los resultados de texto JSON se pueden insertar, actualizar o combinar fácilmente en tablas de SQL Server.

## <a name="analyze-json-data-with-sql-queries"></a>Analizar datos JSON con consultas SQL

Si tiene que filtrar o agregar datos JSON para elaborar informes, puede usar **OPENJSON** para transformar JSON en formato relacional. Luego puede usar el lenguaje [!INCLUDE[tsql](../../includes/tsql-md.md)] estándar y las funciones integradas para preparar los informes.  
  
```sql
SELECT Tab.Id, SalesOrderJsonData.Customer, SalesOrderJsonData.Date
FROM SalesOrderRecord AS Tab
CROSS APPLY OPENJSON (Tab.json, N'$.Orders.OrdersArray')
  WITH (
    Number VARCHAR(200) N'$.Order.Number',
    Date DATETIME N'$.Order.Date',
    Customer VARCHAR(200) N'$.AccountNumber',
    Quantity INT N'$.Item.Quantity'
  ) AS SalesOrderJsonData
WHERE JSON_VALUE(Tab.json, '$.Status') = N'Closed'
ORDER BY JSON_VALUE(Tab.json, '$.Group'), Tab.DateModified;
```  
  
Puede usar tanto valores como columnas de tabla estándar de texto JSON en la misma consulta. Puede agregar índices a la expresión `JSON_VALUE(Tab.json, '$.Status')` para mejorar el rendimiento de la consulta. Para obtener más información, consulte [Indexación de datos JSON](../../relational-databases/json/index-json-data.md).

## <a name="return-data-from-a-sql-server-table-formatted-as-json"></a>Obtener datos de una tabla de SQL Server con formato JSON

Si tiene un servicio web que toma datos de la capa de base de datos y los devuelve en formato JSON, o si tiene bibliotecas o marcos de trabajo de JavaScript que aceptan datos con formato JSON, puede dar formato JSON a los resultados directamente en una consulta de SQL. En lugar de escribir código o incluir una biblioteca para convertir los resultados de consulta tabulares para, luego, serializar esos objetos en formato JSON, puede usar **FOR JSON** para delegar en SQL Server la aplicación de formato JSON.  
  
Por ejemplo, imaginemos que quiere generar un resultado JSON que sea compatible con la especificación OData. El servicio web espera una solicitud y una respuesta en el siguiente formato: 
  
- Solicitud: `/Northwind/Northwind.svc/Products(1)?$select=ProductID,ProductName`  

- Respuesta: `{"@odata.context": "https://services.odata.org/V4/Northwind/Northwind.svc/$metadata#Products(ProductID,ProductName)/$entity", "ProductID": 1, "ProductName": "Chai"}`  
  
Esta dirección URL de OData representa una solicitud de las columnas ProductID y ProductName del producto con el `ID` 1. Puede usar **FOR JSON** para aplicar al resultado el formato que SQL Server espera.  
  
```sql  
SELECT 'https://services.odata.org/V4/Northwind/Northwind.svc/$metadata#Products(ProductID,ProductName)/$entity' AS '@odata.context',
  ProductID,
  Name as ProductName
FROM Production.Product
WHERE ProductID = 1
FOR JSON AUTO;
```  
  
El resultado de esta consulta es un texto JSON totalmente compatible con la especificación OData. SQL Server se encarga de aplicar el formato y las secuencias de escape. SQL Server también puede dar formato a resultados de consulta en cualquier formato, como JSON de OData o GeoJSON.  
  
## <a name="test-drive-built-in-json-support-with-the-adventureworks-sample-database"></a>Probar la compatibilidad integrada de JSON con la base de datos de ejemplo de AdventureWorks

Para obtener la base de datos de ejemplo de AdventureWorks, descargue al menos el archivo de base de datos y el archivo de ejemplos y scripts en [GitHub](https://github.com/microsoft/sql-server-samples/releases/tag/adventureworks).

Después de restaurar la base de datos de ejemplo en una instancia de SQL Server 2016, extraiga el archivo de ejemplos y abra el archivo *JSON Sample Queries procedures views and indexes.sql* desde la carpeta JSON. Ejecute los scripts de este archivo para cambiar el formato de algunos datos existentes a datos JSON, probar consultas de ejemplo e informes basados en los datos JSON, indexar los datos JSON e importar y exportar JSON.  
  
Esto es lo que se puede hacer con los scripts incluidos en el archivo:  
  
- Desnormalizar el esquema existente para crear columnas de datos JSON.

  - Almacene la información SalesReasons, SalesOrderDetails, SalesPerson, Customer y otras tablas con información relativa a pedidos de ventas en columnas JSON de la tabla SalesOrder_json.  
  
  - Almacene información de las tablas EmailAddresses/PersonPhone en la tabla Person_json como matrices de objetos JSON.  
  
- Crear procedimientos y vistas que consultan datos JSON.  
  
- Indexar datos JSON. Cree índices en propiedades JSON e índices de texto completo.  
  
- Importar y exportar JSON. Cree y ejecute procedimientos que exporten el contenido de las tablas Person y SalesOrder como resultados JSON. Importe y actualice las tablas Person y SalesOrder con entradas JSON.  
  
- Ejecutar ejemplos de consultas. Ejecute algunas consultas que llamen a las vistas y los procedimientos almacenados creados en los pasos 2 y 4.  
  
- Limpiar scripts. No lleve esto a cabo si quiere conservar las vistas y los procedimientos almacenados creados en los pasos 2 y 4.  
  
## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Más información sobre JSON en SQL Server y Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vídeos de Microsoft

Para obtener una introducción visual a la compatibilidad integrada de JSON en SQL Server y Azure SQL Database, vea el siguiente vídeo:

*Using JSON in SQL Server 2016 and Azure SQL Database* (Uso de JSON en SQL Server 2016 y Azure SQL Database)
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database/player?WT.mc_id=dataexposed-c9-niner]

*Compilar una API de REST con SQL Server mediante funciones JSON*
> [!VIDEO https://www.youtube.com/embed/0m6GXF3-5WI]

### <a name="reference-articles"></a>Artículos de referencia

- [FOR Clause (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md) (Cláusula FOR [Transact-SQL]) (para JSON)  
- [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)  
- [JSON Functions (Transact-SQL)](../../t-sql/functions/json-functions-transact-sql.md) (Funciones JSON [Transact-SQL])  
  - [ISJSON (Transact-SQL)](../../t-sql/functions/isjson-transact-sql.md)  
  - [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md)  
  - [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)  
  - [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md)  
