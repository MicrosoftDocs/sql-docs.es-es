---
description: Dar formato JSON a los resultados de consulta con FOR JSON (SQL Server)
title: Aplicación de formato JSON a los resultados de consulta con FOR JSON
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON
- JSON, exporting
- exporting JSON
ms.assetid: 15b56365-58c2-496c-9d4b-aa2600eab09a
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 916ff3bf29640807314e139ceeaf82283133d73b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478136"
---
# <a name="format-query-results-as-json-with-for-json-sql-server"></a>Dar formato JSON a los resultados de consulta con FOR JSON (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

Dé formato JSON a los resultados de las consultas o exporte datos de SQL Server como JSON mediante la adición de la cláusula **FOR JSON** a una instrucción **SELECT**. Use la cláusula **FOR JSON** para simplificar las aplicaciones cliente mediante la delegación del formato de la salida JSON desde la aplicación a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) es el editor de consultas recomendado para las consultas JSON porque da formato automáticamente a los resultados JSON (como se muestra en este artículo), en lugar de mostrar una cadena plana.
  
 Cuando se usa la cláusula **FOR JSON**, puede especificar explícitamente la salida JSON o permitir que la estructura de la instrucción SELECT determine la salida.  
  
-   Para mantener el control total sobre el formato de la salida JSON, use **FOR JSON PATH**. Puede crear objetos contenedores y anidar propiedades complejas.  
  
-   Para aplicar formato a la salida JSON automáticamente según la estructura de la instrucción SELECT, use **FOR JSON AUTO**.  
  
Este es un ejemplo de una instrucción **SELECT** con la cláusula **FOR JSON** y su salida.
  
 ![FOR JSON](../../relational-databases/json/media/jsonslides2forjson.png)
  
## <a name="option-1---you-control-output-with-for-json-path"></a>Opción 1: Controla de la salida con FOR JSON PATH
En el modo **PATH**, puede usar la sintaxis de puntos (por ejemplo, `'Item.UnitPrice'`) para dar formato a la salida anidada.  

Esta es una consulta de ejemplo en la que se usa el modo **PATH** con la cláusula **FOR JSON** . En el ejemplo siguiente también se usa la opción **ROOT** para especificar un elemento raíz con nombre. 
  
 ![Diagrama de flujo de salida FOR JSON](../../relational-databases/json/media/forjson-example1.png) 

### <a name="more-info-about-for-json-path"></a>Más información sobre FOR JSON PATH
Para obtener más información y ejemplos, vea [Formato de salida JSON anidada con el modo PATH &#40;SQL Server&#41;](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md).

Para ver la sintaxis y el uso, consulte [Cláusula FOR &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md).  

## <a name="option-2---select-statement-controls-output-with-for-json-auto"></a>Opción 2: Control de la salida con FOR JSON AUTO mediante la instrucción SELECT
En el modo **AUTO** , la estructura de la instrucción SELECT determina el formato de la salida JSON.

Los valores **null** no se incluyen en la salida de forma predeterminada. Puede usar la opción **INCLUDE_NULL_VALUES** para cambiar este comportamiento.  

Esta es una consulta de ejemplo en la que se usa el modo **AUTO** con la cláusula **FOR JSON** .

```sql  
SELECT name, surname  
FROM emp  
FOR JSON AUTO;
```  

Y este es el JSON devuelto.

```json  
[{
    "name": "John"
}, {
    "name": "Jane",
    "surname": "Doe"
}]
```

### <a name="2b---example-with-join-and-null"></a>2.b: Ejemplo de JOIN y NULL

En el siguiente ejemplo de `SELECT...FOR JSON AUTO` se incluye una visualización del aspecto que tendrían los resultados JSON cuando hay una relación uno a varios entre los datos de las tablas donde se ha aplicado `JOIN`.

También se ilustra la ausencia del valor NULL en el código JSON devuelto. Sin embargo, este comportamiento predeterminado se puede invalidar si se usa la palabra clave `INCLUDE_NULL_VALUES` en la cláusula `FOR`.

```sql
go

DROP TABLE IF EXISTS #tabStudent;
DROP TABLE IF EXISTS #tabClass;

go

CREATE TABLE #tabClass
(
   ClassGuid   uniqueIdentifier  not null  default newid(),
   ClassName   nvarchar(32)      not null
);

CREATE TABLE #tabStudent
(
   StudentGuid   uniqueIdentifier  not null  default newid(),
   StudentName   nvarchar(32)      not null,
   ClassGuid     uniqueIdentifier      null   -- Foreign key.
);

go

INSERT INTO #tabClass
      (ClassGuid, ClassName)
   VALUES
      ('DE807673-ECFC-4850-930D-A86F921DE438', 'Algebra Math'),
      ('C55C6819-E744-4797-AC56-FF8A729A7F5C', 'Calculus Math'),
      ('98509D36-A2C8-4A65-A310-E744F5621C83', 'Art Painting')
;

INSERT INTO #tabStudent
      (StudentName, ClassGuid)
   VALUES
      ('Alice Apple', 'DE807673-ECFC-4850-930D-A86F921DE438'),
      ('Alice Apple', 'C55C6819-E744-4797-AC56-FF8A729A7F5C'),
      ('Betty Boot' , 'C55C6819-E744-4797-AC56-FF8A729A7F5C'),
      ('Betty Boot' , '98509D36-A2C8-4A65-A310-E744F5621C83'),
      ('Carla Cap'  , null)
;

go

SELECT
      c.ClassName,
      s.StudentName
   from
                       #tabClass   as c
      RIGHT OUTER JOIN #tabStudent as s ON s.ClassGuid = c.ClassGuid
   --where
   --   c.ClassName LIKE '%Math%'
   order by
      c.ClassName,
      s.StudentName
   FOR
      JSON AUTO
      --, INCLUDE_NULL_VALUES
;

go

DROP TABLE IF EXISTS #tabStudent;
DROP TABLE IF EXISTS #tabClass;

go
```

Y este sería el JSON que se obtendría con la instrucción SELECT anterior.

```json
JSON_F52E2B61-18A1-11d1-B105-00805F49916B

[
   {"s":[{"StudentName":"Carla Cap"}]},
   {"ClassName":"Algebra Math","s":[{"StudentName":"Alice Apple"}]},
   {"ClassName":"Art Painting","s":[{"StudentName":"Betty Boot"}]},
   {"ClassName":"Calculus Math","s":[{"StudentName":"Alice Apple"},{"StudentName":"Betty Boot"}]}
]
```

### <a name="more-info-about-for-json-auto"></a>Más información sobre FOR JSON AUTO

Para obtener más información y ejemplos, vea [Aplicar formato a la salida JSON automáticamente con el modo AUTO &#40;SQL Server&#41;](../../relational-databases/json/format-json-output-automatically-with-auto-mode-sql-server.md).

Para ver la sintaxis y el uso, consulte [Cláusula FOR &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md).  
  
## <a name="control-other-json-output-options"></a>Controlar otras opciones de salida JSON  
Controle la salida de la cláusula **FOR JSON** mediante el uso de las siguientes opciones adicionales.  
  
-   **ROOT**. Para agregar un solo elemento de nivel superior a la salida JSON, especifique la opción **ROOT** . Si no especifica esta opción, la salida JSON no tiene ningún elemento raíz. Para obtener más información, vea [Agregar un nodo raíz a la salida JSON con la opción ROOT &#40;SQL Server&#41;](../../relational-databases/json/add-a-root-node-to-json-output-with-the-root-option-sql-server.md).  
  
-   **INCLUDE_NULL_VALUES**. Para incluir valores NULL en la salida JSON, especifique la opción **INCLUDE_NULL_VALUES**. Si no especifica esta opción, la salida no incluye las propiedades JSON para los valores NULL en los resultados de la consulta. Para obtener más información, consulte [Inclusión de valores Null en la salida de JSON con la opción INCLUDE_NULL_VALUES &#40;SQL Server&#41;](../../relational-databases/json/include-null-values-in-json-include-null-values-option.md).   

-   **WITHOUT_ARRAY_WRAPPER**. Para quitar los corchetes que rodean la salida JSON de la cláusula **FOR JSON** de manera predeterminada, especifique la opción **WITHOUT_ARRAY_WRAPPER** . Use esta opción para generar un objeto JSON único como salida de un resultado de fila única. Si no especifica esta opción, a la salida JSON se le aplica un formato como una matriz, es decir, está delimitada por corchetes. Para obtener más información, vea [Quitar corchetes de la salida JSON con la opción WITHOUT_ARRAY_WRAPPER &#40;SQL Server&#41;](../../relational-databases/json/remove-square-brackets-from-json-without-array-wrapper-option.md). 
   
## <a name="output-of-the-for-json-clause"></a>Salida de la cláusula FOR JSON  
La salida de la cláusula **FOR JSON** tiene las características siguientes:  
  
1.  El conjunto de resultados contiene una sola columna.
    -   Un conjunto de resultados pequeño puede contener una sola fila.
    -   Un conjunto de resultados grande divide la cadena JSON larga en varias filas.
        -   De forma predeterminada, SQL Server Management Studio (SSMS) concatena los resultados en una sola fila cuando el valor de salida es **Resultados a cuadrícula**. La barra de estado de SSMS muestra el recuento de filas real.
        -   Otras aplicaciones cliente pueden requerir código para recombinar los resultados extensos en una sola cadena JSON válida mediante la concatenación del contenido de varias filas. Para obtener un ejemplo de este código en una aplicación de C#, vea [Uso de salidas FOR JSON en una aplicación cliente de C#](../../relational-databases/json/use-for-json-output-in-sql-server-and-in-client-apps-sql-server.md#use-for-json-output-in-a-c-client-app).
  
     ![Ejemplo de salida FOR JSON](../../relational-databases/json/media/forjson-example2.png)  
  
2.  Los resultados reciben el formato de una matriz de objetos JSON.  
  
    -   El número de elementos de la matriz JSON es igual al número de filas en los resultados de la instrucción SELECT (antes de aplicar la cláusula FOR JSON). 
  
    -   Cada fila de los resultados de la instrucción SELECT (antes de aplicar la cláusula FOR JSON) se convierte en un objeto JSON independiente en la matriz.  
  
    -   Cada columna de los resultados de la instrucción SELECT (antes de aplicar la cláusula FOR JSON) se convierte en una propiedad del objeto JSON.  
  
3.  Tanto los nombres de las columnas como sus valores van acompañados de un carácter de escape, como reza la sintaxis de JSON. Para obtener más información, vea [Cómo FOR JSON inserta caracteres de escape en los caracteres especiales y caracteres de control &#40;SQL Server&#41;](../../relational-databases/json/how-for-json-escapes-special-characters-and-control-characters-sql-server.md).

### <a name="example"></a>Ejemplo
Este es un ejemplo que muestra cómo la cláusula **FOR JSON** da formato al resultado JSON.  
  
**Resultados de la consulta**  

|A|B|C|D|
|-|-|-|-|
|10|11|12|X|  
|20|21|22|Y|  
|30|31|32|Z|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |

 **Salida de JSON**  
  
```json  
[{
    "A": 10,
    "B": 11,
    "C": 12,
    "D": "X"
}, {
    "A": 20,
    "B": 21,
    "C": 22,
    "D": "Y"
}, {
    "A": 30,
    "B": 31,
    "C": 32,
    "D": "Z"
}] 
```  

 Para más información sobre lo que ve en la salida de la cláusula **FOR JSON**, vea los temas siguientes.  

-   [Conversión por parte de FOR JSON de tipos de datos de SQL Server en tipos de datos JSON &#40;SQL Server&#41;](../../relational-databases/json/how-for-json-converts-sql-server-data-types-to-json-data-types-sql-server.md)  
    La cláusula **FOR JSON** usa las reglas descritas en este tema para convertir tipos de datos SQL a tipos JSON en la salida JSON.  

-   [Cómo FOR JSON inserta caracteres de escape en los caracteres especiales y caracteres de control &#40;SQL Server&#41;](../../relational-databases/json/how-for-json-escapes-special-characters-and-control-characters-sql-server.md)  
    La cláusula **FOR JSON** inserta un carácter de escape en los caracteres especiales y representa caracteres de control en la salida JSON, como se describe en este tema.  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Más información sobre JSON en SQL Server y Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vídeos de Microsoft

Para obtener una introducción visual a la compatibilidad integrada de JSON en SQL Server y Azure SQL Database, vea los siguientes vídeos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 y compatibilidad con JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Uso de JSON en SQL Server 2016 y Azure SQL Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON como puente entre los universos NoSQL y relacional)
  
## <a name="see-also"></a>Consulte también  
 [Cláusula FOR &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md)   
 [Uso de salidas FOR JSON en SQL Server y en aplicaciones cliente &#40;SQL Server&#41;](../../relational-databases/json/use-for-json-output-in-sql-server-and-in-client-apps-sql-server.md)  
  
  
