---
description: Aplicar formato a la salida JSON automáticamente con el modo AUTO (SQL Server)
title: Aplicar formato a la salida JSON automáticamente con el modo AUTO
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON AUTO
ms.assetid: 178a2a4e-e0f6-49b9-9895-396956d3c7d9
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7161ce97faa4d1baab514df45429592629e2518b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125145"
---
# <a name="format-json-output-automatically-with-auto-mode-sql-server"></a>Aplicar formato a la salida JSON automáticamente con el modo AUTO (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Para aplicar formato a la salida de la cláusula **FOR JSON** automáticamente en función de la estructura de la instrucción **SELECT**, especifique la opción **AUTO**.  
  
Cuando especifica la opción **AUTO**, el formato de la salida JSON se determina automáticamente según el orden de las columnas en la lista SELECT y sus tablas de origen. No se puede cambiar este formato.
 
La alternativa consiste en usar la opción **PATH** para mantener el control sobre la salida.
-   Para obtener más información sobre la opción **PATH**, consulte [Format Nested JSON Output with PATH Mode](../../relational-databases/json/format-nested-json-output-with-path-mode-sql-server.md) (Aplicar formato a la salida JSON anidada con el modo PATH).
-   Para obtener información general sobre ambas opciones, consulte [Format Query Results as JSON with FOR JSON](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md) (Aplicar formato a los resultados de la consulta como JSON con FOR JSON).

Una consulta que usa la opción **FOR JSON AUTO** debe tener una cláusula **FROM** .  
  
Estos son algunos ejemplos de la cláusula **FOR JSON** con la opción **AUTO** . [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) es el editor de consultas recomendado para las consultas JSON porque da formato automáticamente a los resultados JSON (como se muestra en este artículo), en lugar de mostrar una cadena plana.
  
## <a name="examples"></a>Ejemplos

### <a name="example-1"></a>Ejemplo 1
 **Consultar**  
  
Cuando una consulta hace referencia solo a una tabla, los resultados de la cláusula FOR JSON AUTO son similares a los resultados de FOR JSON PATH. En este caso, FOR JSON AUTO no crea objetos anidados. La única diferencia es que FOR JSON AUTO genera alias separados por puntos (por ejemplo, `Info.MiddleName` en el siguiente ejemplo) como claves con puntos, no como objetos anidados.  
  
```sql  
SELECT TOP 5   
       BusinessEntityID As Id,  
       FirstName, LastName,  
       Title As 'Info.Title',  
       MiddleName As 'Info.MiddleName'  
   FROM Person.Person  
   FOR JSON AUTO  
```  
  
 **Resultado**  
  
```json  
[{
    "Id": 1,
    "FirstName": "Ken",
    "LastName": "Sánchez",
    "Info.MiddleName": "J"
}, {
    "Id": 2,
    "FirstName": "Terri",
    "LastName": "Duffy",
    "Info.MiddleName": "Lee"
}, {
    "Id": 3,
    "FirstName": "Roberto",
    "LastName": "Tamburello"
}, {
    "Id": 4,
    "FirstName": "Rob",
    "LastName": "Walters"
}, {
    "Id": 5,
    "FirstName": "Gail",
    "LastName": "Erickson",
    "Info.Title": "Ms.",
    "Info.MiddleName": "A"
}]
```  

### <a name="example-2"></a>Ejemplo 2

**Consultar**  
  
Al unir tablas, las columnas de la primera tabla se generan como propiedades del objeto raíz. Las columnas de la segunda tabla se generan como propiedades de un objeto anidado. El nombre de tabla o alias de la segunda tabla (por ejemplo, `D` en el ejemplo siguiente) se usa como el nombre de la matriz anidada.  
  
```sql  
SELECT TOP 2 SalesOrderNumber,  
        OrderDate,  
        UnitPrice,  
        OrderQty  
FROM Sales.SalesOrderHeader H  
   INNER JOIN Sales.SalesOrderDetail D  
     ON H.SalesOrderID = D.SalesOrderID  
FOR JSON AUTO   
```  
  
**Resultado**  
  
```json  
[{
    "SalesOrderNumber": "SO43659",
    "OrderDate": "2011-05-31T00:00:00",
    "D": [{
        "UnitPrice": 24.99,
        "OrderQty": 1
    }]
}, {
    "SalesOrderNumber": "SO43659",
    "D": [{
        "UnitPrice": 34.40
    }, {
        "UnitPrice": 134.24,
        "OrderQty": 5
    }]
}]
```  

### <a name="example-3"></a>Ejemplo 3
 
**Consultar**  
En lugar de usar FOR JSON AUTO, puede anidar una subconsulta FOR JSON PATH en la instrucción SELECT, como se muestra en el ejemplo siguiente. Este ejemplo produce el mismo resultado que el ejemplo anterior.  
  
```sql  
SELECT TOP 2  
    SalesOrderNumber,  
    OrderDate,  
    (SELECT UnitPrice, OrderQty  
      FROM Sales.SalesOrderDetail AS D  
      WHERE H.SalesOrderID = D.SalesOrderID  
     FOR JSON PATH) AS D  
FROM Sales.SalesOrderHeader AS H  
FOR JSON PATH  
```  
  
**Resultado**  
  
```json  
[{
    "SalesOrderNumber": "SO43659",
    "OrderDate": "2011-05-31T00:00:00",
    "D": [{
        "UnitPrice": 24.99,
        "OrderQty": 1
    }]
}, {
    "SalesOrderNumber": "SO4390",
    "D": [{
        "UnitPrice": 24.99
    }]
}]
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Más información sobre JSON en SQL Server y Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vídeos de Microsoft

Para obtener una introducción visual a la compatibilidad integrada de JSON en SQL Server y Azure SQL Database, vea los siguientes vídeos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 y compatibilidad con JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Uso de JSON en SQL Server 2016 y Azure SQL Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON como puente entre los universos NoSQL y relacional)

## <a name="see-also"></a>Consulte también  
 [Cláusula FOR &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md)  
