---
title: 'Paso 3: Prueba de concepto de la conexión a SQL mediante Ruby'
description: El paso 3 es una prueba de concepto, que muestra cómo puede conectarse a SQL Server mediante Ruby. Los ejemplos básicos demuestran la selección e inserción de datos.
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cac20b18-0a6d-4243-bbda-a5d1b9476441
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93c40d3c64f2a32cd9f318dc236c4c350cfddc28
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058120"
---
# <a name="step-3-proof-of-concept-connecting-to-sql-using-ruby"></a>Paso 3: Prueba de concepto de la conexión a SQL mediante Ruby

Este ejemplo solo debe considerarse una prueba de concepto.  El código de ejemplo se simplifica para mayor claridad y no representa necesariamente los procedimientos recomendados por Microsoft.  
  
## <a name="step-1--connect"></a>Paso 1:  Conectar  
  
La función [TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) se usa para conectarse a SQL Database.  
  
```ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
```  
  
## <a name="step-2--execute-a-query"></a>Paso 2:  Ejecutar una consulta  
  
Copie y pegue el código siguiente en un archivo vacío. Llámelo test.rb. Después, ejecútelo especificando el siguiente comando en el símbolo del sistema:  
  
```ruby
    ruby test.rb  
```
  
En el ejemplo de código, la función [TinyTds::Result](https://github.com/rails-sqlserver/tiny_tds) sirve para recuperar un conjunto de resultados de una consulta en SQL Database. Esta función acepta una consulta y devuelve un conjunto de resultados. El conjunto de resultados se recorre en iteración usando [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds).  
  
```ruby 
    require 'tiny_tds'    
    print 'test'       
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC")  
    results.each do |row|  
    puts row  
    end  
```  
  
## <a name="step-3--insert-a-row"></a>Paso 3:  Inserción de una fila  
  
En este ejemplo se muestra cómo ejecutar la instrucción [INSERT](../../t-sql/statements/insert-transact-sql.md) de forma segura y pasar parámetros que protejan la aplicación ante el valor de [inyección de código SQL](../../relational-databases/tables/primary-and-foreign-key-constraints.md).    
  
Para utilizar TinyTDS con Azure, se recomienda ejecutar varias instrucciones `SET` para cambiar la forma en que la sesión actual trata determinada información. Las instrucciones `SET` recomendadas se proporcionan en el código de ejemplo. Por ejemplo, `SET ANSI_NULL_DFLT_ON` permitirá crear nuevas columnas para permitir valores null, aunque no se indique explícitamente el estado de aceptación de valores NULL de la columna.  
  
Para estar en consonancia con el formato [datetime](../../t-sql/data-types/datetime-transact-sql.md) de Microsoft SQL Server, use la función [strftime](https://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) para convertir al formato de fecha y hora correspondiente.  
  
```ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SET ANSI_NULLS ON")  
    results = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")  
    results = client.execute("SET ANSI_NULL_DFLT_ON ON")  
    results = client.execute("SET IMPLICIT_TRANSACTIONS OFF")  
    results = client.execute("SET ANSI_PADDING ON")  
    results = client.execute("SET QUOTED_IDENTIFIER ON")  
    results = client.execute("SET ANSI_WARNINGS ON")  
    results = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")  
    require 'date'  
    t = Time.now  
    curr_date = t.strftime("%Y-%m-%d %H:%M:%S.%L")  
    results = client.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)  
    OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, '#{curr_date}' )")  
    results.each do |row|  
    puts row  
    end  
```
