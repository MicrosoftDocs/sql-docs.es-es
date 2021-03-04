---
description: Recuperación de ParameterMetaData a través de useFmtOnly
title: Recuperación de ParameterMetaData a través de useFmtOnly | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: a88707cc4679343c2e29a86ecc3a9efb8a9719dd
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837456"
---
# <a name="retrieving-parametermetadata-via-usefmtonly"></a>Recuperación de ParameterMetaData a través de useFmtOnly
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Microsoft JDBC Driver para SQL Server incluye una forma alternativa de consultar metadatos de parámetros del servidor, **useFmtOnly**. Esta característica se presentó por primera vez en la versión 7.4 del controlador y es necesaria como solución alternativa de los problemas conocidos de `sp_describe_undeclared_parameters`.
  
  El controlador usa principalmente el procedimiento almacenado `sp_describe_undeclared_parameters` para consultar metadatos de parámetros, ya que este es el enfoque recomendado para la recuperación de metadatos de parámetros en la mayoría de los casos. Sin embargo, actualmente se produce un error en la ejecución del procedimiento almacenado en los siguientes casos de uso:
  
-   Contra las columnas de Always Encrypted
  
-   Contra las tablas temporales y las variables de tabla
  
-   Contra las vistas 
  
  La solución propuesta para estos casos de uso es analizar la consulta SQL del usuario para los parámetros y destinos de tabla y, a continuación, ejecute una `SELECT` consulta con el valor `FMTONLY` habilitado. El siguiente fragmento ayudará a visualizar la característica.
  
```sql
--create a normal table 'Foo' and a temporary table 'Bar'
CREATE TABLE Foo(c1 int);
CREATE TABLE #Bar(c1 int);

EXEC sp_describe_undeclared_parameters N'SELECT * FROM Foo WHERE c1 = @p0' --works fine
EXEC sp_describe_undeclared_parameters N'SELECT * FROM #Bar WHERE c1 = @p0' --fails with "Invalid object name '#Bar'"

SET FMTONLY ON;
SELECT c1 FROM Foo; --works
SET FMTONLY OFF;
SET FMTONLY ON;
SELECT c1 FROM #Bar; --works
SET FMTONLY OFF;
```
 
## <a name="turning-the-feature-onoff"></a>Activación o desactivación de la característica 
 La característica **useFmtOnly** está desactivada de forma predeterminada. Los usuarios pueden habilitar esta característica a través de la cadena de conexión especificando `useFmtOnly=true`. Por ejemplo: `jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;useFmtOnly=true;`.
 
 Como alternativa, la característica está disponible a través de `SQLServerDataSource`.
 ```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName(<server>);
ds.setPortNumber(<port>);
ds.setDatabaseName("<databaseName>");
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setUseFmtOnly(true);
try (Connection c = ds.getConnection()) {
    // do work with connection
}
 ```
 
 La característica también está disponible en el nivel de instrucción. Los usuarios pueden activar o desactivar la característica `PreparedStatement.setUseFmtOnly(boolean)`.
> [!NOTE]  
>  El controlador dará prioridad a la propiedad de nivel de instrucción frente a la propiedad de nivel de conexión.

## <a name="using-the-feature"></a>Uso de la característica
  Una vez habilitado, el controlador se iniciará de forma interna mediante la nueva característica en lugar de `sp_describe_undeclared_parameters` al consultar metadatos de parámetros. No es necesario que el usuario final realice más acciones.
```java
final String sql = "INSERT INTO #Bar VALUES (?)";
try (Connection c = DriverManager.getConnection(URL, USERNAME, PASSWORD)) {
    try (Statement s = c.createStatement()) {
        s.execute("CREATE TABLE #Bar(c1 int)");
    }
    try (PreparedStatement p1 = c.prepareStatement(sql); PreparedStatement p2 = c.prepareStatement(sql)) {
        ((SQLServerPreparedStatement) p1).setUseFmtOnly(true);
        ParameterMetaData pmd1 = p1.getParameterMetaData();
        System.out.println(pmd1.getParameterTypeName(1)); // prints int
        ParameterMetaData pmd2 = p2.getParameterMetaData(); // throws exception, Invalid object name '#Bar'
    }
}
```
> [!NOTE]  
>  La característica solo admite consultas `SELECT/INSERT/UPDATE/DELETE`. Las consultas deben empezar por una de las cuatro palabras clave admitidas o una [expresión de tabla común](../../t-sql/queries/with-common-table-expression-transact-sql.md) seguida de una de las consultas admitidas. No se admiten los parámetros dentro de expresiones de tabla comunes.

## <a name="known-issues"></a>Problemas conocidos
  Actualmente hay algunos problemas con esta característica; su causa son las deficiencias en la lógica de análisis de SQL. Estos problemas pueden solucionarse en una futura actualización de la característica y se documentan a continuación junto con sugerencias de soluciones alternativas.
  
A. Uso de un alias "declarado por adelantado"
```sql
CREATE TABLE Foo(c1 int)

DELETE fooAlias FROM Foo fooAlias WHERE c1 > ?; --Invalid object name 'fooAlias'

--Workaround #1: Specify AS keyword
DELETE fooAlias FROM Foo AS fooAlias WHERE c1 > ?;
--Workaround #2: Use the table name
DELETE Foo FROM Foo fooAlias WHERE c1 > ?;
```

B. Nombre de columna ambiguo cuando las tablas tienen nombres de columna compartidos
```sql
CREATE TABLE Foo(c1 int, c2 int, c3 int)
CREATE TABLE Bar(c1 int, c2 int, c3 int)

SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar WHERE c1 > ? and c2 < ? and c3 = ?); --Ambiguous Column Name

--Workaround: Use aliases
SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar b WHERE b.c1 = ? and b.c2 = ? and b.c3 = ?);
```

C. SELECCIÓN en una subconsulta con parámetros
```sql

CREATE TABLE Foo(c1 int)

SELECT * FROM (SELECT * FROM Foo WHERE c1 = ?) WHERE c1 = ?; --Incorrect syntax near '?'

--Workaround: N/A
```

D. Subconsultas en una cláusula SET
```sql
CREATE TABLE Foo(c1 int)

UPDATE Foo SET c1 = (SELECT c1 FROM Foo) WHERE c1 = ?; --Incorrect syntax near ')'

--Workaround: Add a 'delimiting' condition
UPDATE Foo SET c1 = (SELECT c1 FROM Foo HAVING (HASH JOIN)) WHERE c1 = ?;
```

## <a name="see-also"></a>Consulte también  
 [Establecimiento de las propiedades de conexión](../../connect/jdbc/setting-the-connection-properties.md)  
  
