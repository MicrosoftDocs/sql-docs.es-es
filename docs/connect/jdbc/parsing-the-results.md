---
description: Análisis de los resultados
title: Análisis de los resultados | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: 7b56223594d1126c373b4fd8b24dabc1e81dab67
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837086"
---
# <a name="parsing-the-results"></a>Análisis de los resultados

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

En este artículo se describe cómo espera SQL Server que los usuarios procesen completamente los resultados devueltos de cualquier consulta.

## <a name="update-counts-and-result-sets"></a>Recuentos de actualizaciones y conjuntos de resultados

En esta sección se tratarán los dos resultados más habituales que devuelve SQL Server: recuento de actualizaciones y ResultSet. En general, cualquier consulta que ejecute un usuario hará que se devuelva uno de estos resultados; se espera que los usuarios administren ambos al procesarlos.

En siguiente código es un ejemplo de cómo podría un usuario recorrer en iteración todos los resultados del servidor:
```java
try (Connection connection = DriverManager.getConnection(URL); Statement statement = connection.createStatement()) {
    boolean resultsAvailable = statement.execute(USER_SQL);
    int updateCount = -2;
    while (true) {
        updateCount = statement.getUpdateCount();
        if (!resultsAvailable && updateCount == -1)
            break;
        if (resultsAvailable) {
            // handle ResultSet
        } else {
            // handle Update Count
        }
        resultsAvailable = statement.getMoreResults();
    }
}
```

## <a name="exceptions"></a>Excepciones
Al ejecutar una instrucción que produce un error o un mensaje informativo, SQL server responderá de forma diferente dependiendo de si puede generar un plan de ejecución. Se puede generar un mensaje de error inmediatamente después de la ejecución de instrucciones o podría requerir un conjunto de resultados independiente. En el último caso, las aplicaciones deben analizar el conjunto de resultados para recuperar la excepción.

Cuando SQL Server no puede generar un plan de ejecución, se inicia la excepción inmediatamente.

```java
String SQL = "SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    statement.execute(SQL);
} catch (SQLException e) {
    e.printStackTrace();
}
```

Cuando SQL Server devuelve un mensaje de error en un conjunto de resultados, dicho conjunto debe procesarse para recuperar la excepción.

```java
String SQL = "SELECT 1/0;";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    if (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            // Exception is thrown on next().
            while (rs.next()) {}
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

Si la ejecución de instrucciones genera varios conjuntos de resultados, cada uno de ellos debe procesarse hasta alcanzarse el que tiene la excepción.

```java
String SQL = "SELECT 1; SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    // Does not throw an exception on execute().
    boolean hasResult = statement.execute(SQL);
    while (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            while (rs.next()) {
                System.out.println(rs.getString(1));
            }
        }
        // Moves the next result set that generates the exception.
        hasResult = statement.getMoreResults();
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

En el caso de `String SQL = "SELECT * FROM nonexistentTable; SELECT 1;";`, se inicia la excepción inmediatamente en `execute()` y `SELECT 1` no se ejecuta en absoluto.

Si el error de SQL Server tiene una gravedad de `0` a `9`, se considera un mensaje informativo y se devuelve como `SQLWarning`.

```java
String SQL = "RAISERROR ('WarningLevel5', 5, 2);";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    SQLWarning warning = statement.getWarnings();
    System.out.println(warning);
}
```

## <a name="see-also"></a>Consulte también

[Introducción al controlador JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)
