---
title: Usar aserciones de Transact-SQL en pruebas unitarias de SQL Server
description: Obtenga información sobre las aserciones de Transact-SQL. Vea cuándo se deben usar aserciones en pruebas unitarias de SQL Server y cuándo condiciones de prueba, y vea ejemplos de uso de aserciones.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 55d8be9c-9282-47d3-be7f-e2c26f00c95e
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 23dff8f246d741896a042486f0dbe6c95067a8d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066766"
---
# <a name="using-transact-sql-assertions-in-sql-server-unit-tests"></a>Usar aserciones de Transact-SQL en pruebas unitarias de SQL Server

En una prueba unitaria de SQL Server, el script de prueba Transact\-SQL se ejecuta y devuelve un resultado. En ocasiones, los resultados se devuelven como un conjunto de resultados. Puede validar los resultados mediante condiciones de prueba. Por ejemplo, puede usar una condición de prueba para comprobar cuántas filas se devuelven en un conjunto de resultados determinado o comprobar cuánto tiempo tardó en ejecutarse una prueba determinada. Para más información sobre las condiciones de prueba, consulte [Usar condiciones de prueba en pruebas unitarias de SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md).  
  
En lugar de usar condiciones de prueba, puede usar también aserciones de Transact\-SQL, que son instrucciones THROW o RAISERROR en un script Transact\-SQL. En determinadas circunstancias, quizá prefiera usar una aserción de Transact\-SQL en lugar de una condición de prueba.  
  
## <a name="using-transact-sql-assertions"></a>Uso de las aserciones de Transact-SQL  
Debe tener en cuenta los puntos siguientes antes de decidir validar los datos mediante aserciones de Transact\-SQL o mediante condiciones de prueba.  
  
-   **Rendimiento**. Es más rápido ejecutar una aserción de Transact\-SQL en el servidor que mover primero los datos a un equipo cliente y manipularlos localmente.  
  
-   **Conocimiento del lenguaje**. Quizá prefiera un lenguaje determinado según sus conocimientos actuales y, por tanto, elegir las aserciones de Transact\-SQL o las condiciones de prueba de Visual C\# o Visual Basic.  
  
-   **Complejidad de la validación**. En algunos casos, puede compilar pruebas más complejas en Visual C\# o Visual Basic y validar las pruebas en el cliente.  
  
-   **Simplicidad**. Suele ser más sencillo usar una condición de prueba predefinida que escribir el script equivalente en Transact\-SQL.  
  
-   **Bibliotecas de validación heredadas**. Si ya tiene código que realiza la validación, puede usarlo en una prueba unitaria de SQL Server en lugar de usar condiciones de prueba.  
  
## <a name="mark-unit-test-methods-with-the-expected-exception"></a>Marcar los métodos de prueba unitaria con la excepción esperada  
Para marcar un método de prueba unitaria de SQL Server con excepciones esperadas, agregue el atributo siguiente:  
  
```vb  
<ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)> _  
```  
  
```csharp  
[ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)]  
```  
  
Donde:  
  
-   *nnnnn* es el número del mensaje esperado, por ejemplo 14025.  
  
-   *x* es la gravedad de la excepción esperada.  
  
-   *y* es el estado de la excepción esperada.  
  
Se omite cualquier parámetro no especificado. Estos parámetros se pasan a la instrucción RAISERROR en el código de la base de datos. Si especifica MatchFirstError = true, el atributo coincidirá con cualquier error de SQL en la excepción. El comportamiento predeterminado (MatchFirstError = true) consiste en hacer coincidir solamente el primer error que aparezca.  
  
Para obtener un ejemplo de cómo usar excepciones esperadas y una prueba unitaria negativa de SQL Server, consulte [Tutorial: Crear y ejecutar una prueba unitaria de SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
## <a name="the-raiserror-statement"></a>Instrucción RAISERROR  
  
> [!NOTE]  
> Use THROW en lugar de RAISERROR. RAISERROR está desusado.  
  
Puede usar directamente las aserciones de Transact\-SQL en el servidor mediante la instrucción RAISERROR en el script Transact\-SQL. La sintaxis es:  
  
**RAISERROR (\@ErrorMessage, \@ErrorSeverity, \@ErrorState)**  
  
donde:  
  
@ErrorMessage es cualquier mensaje de error definido por el usuario. Puede dar formato a esta cadena de mensaje de forma similar a como lo hace para la función printf_s.  
  
@ErrorSeverity es un nivel de gravedad definido por el usuario de 0 a 18.  
  
> [!NOTE]  
> Los valores "0" y "10" para el nivel de gravedad no hacen que la prueba unitaria de SQL Server genere un error. Puede usar cualquier otro valor en el rango de 0 a 18 para hacer que la prueba genere un error.  
  
@ErrorState es un entero arbitrario de 1 a 127. Puede usar este entero para distinguir entre las repeticiones de un solo error que se genera en ubicaciones diferentes en el código.  
  
Para más información, consulte [RAISERROR (Transact-SQL)](../t-sql/language-elements/raiserror-transact-sql.md). En el tema [Cómo: Escribir una prueba unitaria de SQL Server que se ejecuta en el ámbito de una única transacción](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md) se proporciona un ejemplo de cómo usar RAISERROR en una prueba unitaria de SQL Server.  
  
## <a name="see-also"></a>Consulte también  
[Crear y definir pruebas unitarias de SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Usar condiciones de prueba en pruebas unitarias de SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
[Comprobar el código de base de datos con pruebas unitarias de SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[Cómo: Abrir una prueba unitaria de SQL Server para editarla](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)  
